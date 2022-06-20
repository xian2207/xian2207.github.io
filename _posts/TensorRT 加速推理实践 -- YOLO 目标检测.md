#  TensorRT 加速推理实践 -- YOLO 目标检测
_作者：_ 
## TensorRT 简介
高性能深度学习推断库
## 用法：
输入训练好的模型;主要流程如下

![flow](../img/TensorRT_workflow.png)

下面输入YOLOv3网络架构：

![YOLOv3](../img/Yolov3.png)

注意进行视频流处理时，需要及时free内存；

深度学习自动量化:
[深度学习神经网络量化](https://blog.csdn.net/qq_28413479/article/details/77479722)

```plantuml
@startuml
title ** 多环境功能测试数据示意图 ** \n\n
skinparam backgroundColor #azure 
skinparam handwritten true

skinparam sequence {
    ArrowColor #DeepSkyBlue 
    ActorBorderColor #DeepSkyBlue 
    LifeLineBorderColor blue 
    LifeLineBackgroundColor #DeepSkyBlue
    ParticipantBorderColor #DeepSkyBlue 
    ParticipantBackgroundColor #DodgerBlue 
    ParticipantFontName Impact 
    ParticipantFontSize 17 
    ParticipantFontColor #Cyan
    ActorBackgroundColor #aqua 
    ActorFontColor #DeepSkyBlue 
    ActorFontSize 17 
    ActorFontName Aapex
}

actor "客户端" as Client

control "代理服务" as Proxy
note over Proxy #LightYellow
"配置DNS和染色规则"
end note

participant "容器SLB" as DockerSLB
note over DockerSLB #LightYellow
"按染色规则转发"
end note

box "容器染色发布" #LightYellow
collections "集成环境" as UAT4Test #DarkRed
collections "功能环境-1" as FAT14Test #Green
collections "功能环境-2" as FAT24Test #SteelBlue
collections "功能环境..." as FAT...4Test #MediumBlue
collections "功能环境-N" as FATN4Test #MidnightBlue
end box

Client [#gray]--> Client : 本地Hosts
Client [#red]--> Proxy : DNS查询
Client <--[#blue] Proxy : 解析域名对应的IP
||||
Client [#red]-> Proxy : 正常接口-访问
Proxy [#red]-> DockerSLB : 正常接口-透传
DockerSLB [#red]->o UAT4Test : 稳定集成版本
Client [#blue]-> Proxy : 染色接口-访问
Proxy [#blue]-> DockerSLB : 染色接口-透传
DockerSLB [#blue]->o FAT...4Test : 功能测试版本
@enduml
```



资源过滤器执行顺序

```plantuml!
skinparam dpi 150
scale 13500 width
scale 2200 height
skinparam conditionStyle InsideDiamond
start
:等待过滤母机;
:1.资源组标签过滤器;
note right
    保证相同资源组标签
    才能进行分配
    无标签只从无标签资源组中分配
end note 
:2.机型过滤;
:3.内存过滤;
:4.母机磁盘过滤;
:5.ip过滤;
if (存在指定安全组) then (yes)
    :6.安全组过滤;
else (no)
endif

if (独占盘数量>0) then (yes)
    :7.独占盘过滤;
else (no)
endif
:8.cpu过滤;
:过滤后母机;
stop
```

母机权重计算排序方法

```plantuml!
skinparam conditionStyle InsideDiamond
start
while (遍历候选母机)
fork 
    :统计母机剩余CPU最大值RemainCpuMax;
    :统计母机剩余CPU最大值RemainCpuMin;
fork again
    :统计母机剩余memory最大值RemainMemoryMax;
    :统计母机剩余memory最小值RemainMemoryMin;
fork again
    :统计母机剩余磁盘最大值RemainDiskMax;
    :统计母机剩余磁盘最小值RemainDiskMin;
fork again
    :统计母机剩余IP数最大值RemainIPMax;
    :统计母机剩余IP数最小值RemainIPMin;

endfork
endwhile

while ( 遍历候选母鸡HOST )
    while (遍历权重指标集合(CPU、Mmeory、Disk、IP))
        :计算HOST[i]单项权重指标,如CPUWeigh;
        note right
            单项权重为均一映射至(0~1)的值
        endnote
    endwhile
    -> 遍历结束;
    :获取leo单项权重系数配置;
    :计算HOST[i]总权重;
    note right
        HOST[i]总权重=SUM(单项权重系数*单项权重均一值)
    endnote
endwhile
-> 遍历结束;
:根据HOST[i]总权重进行排序(降序);
:选出HOST[0]作为最终目标母机;
end
```

技术方案主要逻辑
```plantuml!
!theme bluegray
skinparam conditionStyle InsideDiamond
|<size:24>gaia-net</size>|
start
:录入母机CPU拓扑结构;
|#AntiqueWhite|<size:24>gaia-scheduler</size>|
:...;
partition 母机CPU过滤 {
    if (母机剩余CPU>需要CPU) then (是)
        if (**<color:red><size:14>查询母机机型CPU拓扑结构</size></color>**) then (成功)
            :1.**查询母机剩余可用CPU逻辑核编号**;
            :2.**生成CPU拓扑可用表**;
            if (**可用物理核对应逻辑核总数 > 需要CPU**) then (是)
                :母机CPU过滤成功;
                :...;
                detach
            else (否)
            endif
        else (失败)
        endif
    else (否)
    endif
    :<color:red> <b>过滤失败;
    
}
:...;
partition 目标母机CPUSet生成 {
    if (查询母机机型CPU拓扑结构) then (成功)
        :1.**查询母机剩余可用CPU逻辑核编号**;
        :2.**生成CPU拓扑可用表**;
        :3.**取出前N/2个空闲物理核对应N个逻辑核编号**;
    else (失败)
        :1.可用CPU逻辑编号中取出前N个;
        :...;
    endif
    :返回目标CPU逻辑编号列表;
    :...;
}
end
```



```plantuml!
!theme bluegray
skinparam conditionStyle InsideDiamond
|<size:24>gaia</size>|
start
:录入母机CPU拓扑结构;
|#AntiqueWhite|<size:24>gaia-scheduler</size>|
:...;
partition 母机CPU过滤 {
    if (母机剩余CPU>需要CPU) then (是)
        if (**<color:red><size:14>查询母机机型CPU拓扑结构</size></color>**) then (成功)
            :1.**查询母机剩余可用CPU逻辑核编号**;
            :2.**生成CPU拓扑可用表**;
            if (**可用物理核对应逻辑核总数 > 需要CPU**) then (是)
                :母机CPU过滤成功;
                :...;
                detach
            else (否)
            endif
        else (失败)
        endif
    else (否)
    endif
    :<color:red> <b>过滤失败;
    
}
:...;
partition 目标母机CPUSet生成 {
    if (查询母机机型CPU拓扑结构) then (成功)
        :1.**查询母机剩余可用CPU逻辑核编号**;
        :2.**生成CPU拓扑可用表**;
        :3.**取出前N/2个空闲物理核对应N个逻辑核编号**;
    else (失败)
        :1.可用CPU逻辑编号中取出前N个;
        :...;
    endif
    :返回目标CPU逻辑编号列表;
    :...;
}
end
```


```plantuml!

@startuml 流程图
digraph flowchart {
    # 自建
    host_broken [label="发现母机故障(IDC)", shape="box",style="rounded"];
    xcloud [label="消息事件转发(xcloud)",shape="box",style="rounded"];
     
    shouquwancheng [label="授权完成",shape="box",style="rounded"];
    host_borken_proc[label="查询母机子机",shape="box",style="rounded"]
        subgraph "cluster_request" {
            label="分发授权请求(gaia)"
            style=""
            shape="ellipse"
            pvm1;
            pvm2[ label ="..."];
            pvm3[label="pvmN"];
        };
    host_borken_proc->{ pvm1, pvm2, pvm3 }
    subgraph "cluster_yewu" {
        label="业务方"
        yewu1[label="业务方1"]
        yewu2[label="..."]
        yewu3[label="业务方N"]
    }
   
    shouquan[label="授权请求转发(事件中心)",shape="box",style="rounded"];
    host_broken-> xcloud[label="母机故障消息"];
    pvm1-> shouquan [label="请求授权"];
    pvm2-> shouquan [label="请求授权"];
    pvm3-> shouquan [label="请求授权"];
    xcloud -> host_borken_proc [label="故障通知"];
    shouquan -> { yewu1,yewu2,yewu3 } [label="请求授权"]
    { yewu1,yewu2,yewu3 } -> shouquwancheng [label="授权"]
     
    start_repair [label="维修",shape="box",style="rounded"];
    shouquwancheng -> start_repair [label="通知维修",shape="box",style="rounded"]
    finish_repair[label="维修完成",shape="box",style="rounded"]
    start_repair -> finish_repair [label="维修"]
    inform_uers[label="通知业务方",shape="box",style="rounded"]
    finish_repair-> inform_uers[label="维修完成"];
    
}
@enduml
```

数据图表

```mermaid!
graph LR
    IDC[自建IDC]
    gaia[gaia/PVM]
    yewu[业务方]
    IDC-->| 0.PM故障 | gaia 
    gaia--> | 1. 请求PVM维修授权 | yewu
    yewu --> | 2. 授权维修PVM | gaia 
    gaia --> | 3. 授权维修PM | IDC 
    IDC --> | 4. PM维修 |IDC
    IDC --> | 5. PM维修完成  | gaia 
    gaia --> | 6. PVM维修完成 | yewu

```


```plantuml
@startuml
digraph finite_state_machine {
    rankdir = LR;
    
    node [shape=doublecircle style=filled]
    RUNNING [label="已运行" color="#00ff00"] 
    STOPPED [label="已关机" color="#eecc80"]
    LAUNCH_FAILED [label="创建失败" color="#ff0000"]
    SHUTDOWN [ label="待销毁" color="#33ff99" ]
   
    node [shape =circle style=filled color="#ffff99"]
    PENDING [label="创建中"] 
    STARTING [label="开机中" ]
    STOPPING [label="关机中" ]
    REBOOTING [label="重启中" ]
    TERMINATING [label="销毁中" ]
    TERMINATED [label="已销毁" color="#666666"]

    //node [shape=Mdiamond style=dashed color="#ffcc33"]
    //START_POINT [ label="start" ]
    //END_POINT [ label="end" ]

    //START_POINT -> PENDING [ label="创建实例" ];
    PENDING -> RUNNING [ label="创建成功" ];
    PENDING -> LAUNCH_FAILED [ label="创建失败"];
    RUNNING -> STOPPING [label="关机"];
    STOPPING -> STOPPED [label="已关机"];
    STOPPED ->  STARTING [label="开机"];
    STARTING -> RUNNING [label="开机成功"];
    RUNNING -> REBOOTING [label = "重启"];
    REBOOTING -> RUNNING [label = "重启成功"];
    STOPPED -> SHUTDOWN [label = "销毁"];
    SHUTDOWN -> STOPPED [label = "实例恢复"];
    SHUTDOWN -> TERMINATING [label = "销毁"];
    TERMINATING -> TERMINATED [label="销毁成功"];
    //TERMINATED -> END_POINT [ label="清除实例"];
    //LAUNCH_FAILED -> END_POINT [ label="清除创建失败实例"];
}
@enduml
```