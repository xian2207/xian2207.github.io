#  TensorRT 加速推理实践 -- YOLO 目标检测
_作者：_ 
## TensorRT 简介
高性能深度学习推断库
## 用法：
输入训练好的模型;主要流程如下

![flow](./img/TensorRT_workflow.png)

下面输入YOLOv3网络架构：

![YOLOv3](./img/Yolov3.png)

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

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