# SOP代码梳理和记录


> 2020-06-23 09:50:58

## SOP 唤起流程简述

SOP单主要的唤起流程如下:
1. 请求`/sopPlay/match`发送caseId和User信息；返回该用户是否拥有该case的修改权限
2. 根据修改权限返回值，决定是否出现SOP选择按钮
3. 点击选择按钮，请求`/sopPlay/play`开始播放当前任务，主要返回的数据是`SopRenderVO`
4. 对人工节点进行选择，请求`/sopPlay/drive`开始进行驱动并展示下一步的节点;
5. 当执行动作节点是，请求`/action/xxx`系列执行对应的请求；并返回对应执行结果。动作列表如下:
   1. 外呼
   2. 工单挂起
   3. 申请小二介入
   4. 留言
   5. 发送短信
6. 展现人工节点，填写结果信息；查找SOP标准流程表
7. 生成下一步的动作
8. 重复3-7步骤直到达到标准流程的终点，生成最终的解决方案

SOP判责的整体流程就是：
1. 根据当前情况，给出动作节点
2. 选择并执行动作，并填写人工节点结果
3. 根据人工节点结果，生成对应的动作节点和人工节点
4. 重复2-3步，直到走完SOP模板，最终生成对应的解决方案


项目主要结构:
1. web控制台：gaea-xspace-desk
2. SOP规则引擎：gaea-sop-engine
3. SOP匹配查询: gaea-application

### 2.1 SOP时序简图

```plantuml
@startuml
title SOP总体执行流程简图

actor "User" as User
boundary WebClient

collections "xspace" as Server
collections "gaea-application" as Server1
collections "sopengine" as Server2

database "DataBase" as DataBase


autonumber
User -> WebClient : 点击工单详情
activate WebClient
WebClient -> Server : Get:发送caseId UserId
activate Server
Server -> Server1 : 查询case和User\n对应的SOP信息
activate Server1
Server1 -> DataBase : 查询SOP匹配规则
activate DataBase
return 匹配规则列表
return SopApplyRuleDTO
return SopApplyRuleDTO == null
return 显示/不显示SOP判责按钮


'play 动作
User -> WebClient : 点击SOP判责按钮
activate WebClient
WebClient -> Server : Get: caseId和UserInfo 获取 
activate Server
Server -> Server2 : 请求SOP匹配规则流程
activate Server2
Server2 -> DataBase : 查询命中的SOP规则
activate DataBase
return SOP命中规则列表
return SOP匹配规则列表
return SopRenderVO 渲染节点信息
return 显示SOP判责页面

' 开始选择和执行
loop 未到达SOP规则末端

'开始动作流程
User -> WebClient : 执行动作节点动作
activate WebClient
WebClient -> Server : 发送动作信息
activate Server
Server -> Server2 : 动作执行请求
activate Server2

Server2 -> Server2 : 执行动作
activate Server2 #DarkSalmon
deactivate Server2
return 执行结果(成功失败)
return 执行结果
return 人工节点列表

'开始人工节点--主要是对动作节点的处理

User -> WebClient : 执行人工节点
activate WebClient
WebClient -> Server : 发送SOP执行节点实体 SopDriveEntity
activate Server
Server -> Server2 : 请求处理节点
activate Server2

Server2 -> Server2 : 处理节点请求;规则推理
activate Server2 #DarkSalmon
deactivate Server2
return 推理结果
return 新的SopRenderVO
return 产生动作/人工节点

end loop

' 最终解决方案
Server2 --> Server : 生成最终的解决方案
activate Server 
Server --> WebClient : 返回新的SopRenderVO节点列表
activate WebClient
WebClient --> User : 生成最终的解决方案 



@enduml
```

```plantuml

@startuml
header SOP判责校验简图

title SOP判责权限校验简图
actor user
boundary  WebClient
collections "Server" as Server
database  "DataBase" as DataBase
autonumber 1
user -> WebClient : 查看工单详情
activate WebClient  
WebClient -> Server : Get: caseId and UserInfo 
activate Server

Server -> DataBase : caseId 和  userId
activate DataBase
return : case 详细信息和User详细信息


Server -> Server : 查询用户权限匹配
activate Server #DarkSalmon
deactivate Server
alt [权限不匹配]
Server --> WebClient : false 
else [权限匹配]
Server -> DataBase : 查询case是否存在SOP规则
DataBase --> Server : 查询结果
Server --> WebClient : 最终校验成果 true/false
end alt 
WebClient  --> user : 显示/不显示SOP判责按钮

footer 图 1-0 校验流程图
@enduml
```

### 2.2 SOP权限校验类时序图

```plantuml
@startuml
title SOP权限校验逻辑时序图
' actor User
' collections WebClient

box "gaea-xspace-desk" #LightBlue 
collections "SopPlayController" as SopPlayController
collections "SopPlayManager" as SopPlayManager
end box
box "gaea-application" #LightGreen
collections "SopMatchService" as SopMatchService
collections "SopMatchManager" as SopMatchManager
end box
' 中间不知名调用链路
' box "middle-link" #LightYellow
' collections "middle link service " as MiddleService
' end box


' box "sop-engine" #LightRed
' collections "sop-engine" as SopEngine
' end box

database "DataBase" as  DB
' autonumber
' User -> WebClient : 点击工单详情页面
' activate WebClient


' 调用函数
[o-> SopPlayController : SopPlayController.match(caseId,request)
activate SopPlayController
' 获取用户信息
SopPlayController ->  SopPlayController :User loginUser uer = AccountUtil\n.getAccountFromRequest(request)
activate SopPlayController #DarkSalmon
deactivate SopPlayController
SopPlayController -> SopPlayManager : SopPlayManager.match(caseId , loginUser)
activate SopPlayManager

'检查用户是否有编辑权限
SopPlayManager -> SopPlayManager : res = checkAndGetCurrentUserTaskInfo()
activate SopPlayManager #DarkSalmon
deactivate 
alt [ res == null ]
SopPlayManager --> SopPlayController : RpcResult<Void>(false)
end alt
' 检查是否存在匹配的SOP规则
SopPlayManager -> SopMatchService : SopMatchService\n.metchSopApplyWithApplyRule()
activate SopMatchService
SopMatchService -> SopMatchManager : MetchSopApplyRequest\n 
note right of SopMatchService 
  请求Sop匹配
end note 
activate SopMatchManager

' 中间调用链路
' SopMatchManager -> MiddleService: MetchSopApplyRequest 
SopMatchManager -> DB : MetchSopApplyRequest
activate DB
return Result<SopApplyRuleDO>

return Result<SopApplyRuleDTO>
return Result<SopApplyRuleDTO>
return rpcResult<Void>

 <-- SopPlayController : RpcResult<Void> 

@enduml
```


### 2.3 SOP判责助手播放流程简图

```plantuml
@startuml
title SOP判责助手播放流程简图

actor user
boundary WebClient
'box  Xspace #LightBlue
collections  "xspace" as Server
'end box

' box middle-Server #LightGreen
' collections MiddleServer
' end box
collections "gaea-application" as GaeaApplication

'box sop-engine
collections "SopEngine" as SopEngine
'end box


database  "DataBase" as DB
'用户点击SOP判责助手并开始播放
autonumber 
user -> WebClient : 点击<b>SOP判责按钮</b>操作开始播放

== 编辑权限校验 ==

activate WebClient 
WebClient -> Server : caseId和userInfo 用户编辑权限校验 
activate Server
Server -> GaeaApplication : 编辑校验权限
activate GaeaApplication 
GaeaApplication -> DB : 查询用户和工单信息
activate DB
return 用户信息和工单信息
note right of Server
这里的检验流程和
match相同
不做详细介绍 
end note 
alt 校验失败
return Result<SopApplyRuleDTO>.success = false 
return RpcResult<SopRenderVO>.success = false
return 校验错误
else 校验成功
'开始播放流程
== 挂起续播校验 ==
GaeaApplication --> Server : SopApplyRuleDTO
activate Server
Server -> Server : 解析SopApplyRuleDTO
activate Server #DarkSalmon
deactivate Server
Server ->SopEngine  : 查询task是否有session记录
activate SopEngine
SopEngine -> DB : 查询task对应信息
activate DB
return task 信息
SopEngine -> DB : 查询session记录(动作记录)
alt 列表不为空
activate DB
return 历史动作列表
SopEngine --> Server :  Sop执行状态信息
Server -> Server : 状态信息转换为Sop渲染节点
activate Server #DarkSalmon
deactivate Server
else 列表为空
== 创建新的开始播放请求 ==
Server -> SopEngine : 请求新的播放
SopEngine -> SopEngine : 创建SOP实例和sessionId
activate SopEngine #DarkSalmon
deactivate
SopEngine -> DB : 插入session和实例
activate DB
return 操作结果
return SopExecutionResponse \n 新建任务当前状态
end 
Server --> WebClient : SOP渲染节点
WebClient --> user : SOP页面

end alt
@enduml
```
### 2.4 SOP判责助手播放类时序图

下面是SOP判责助手按钮点击时的类流程图

### 2.5 SOP动作节点执行时序图

主要是SOP的动作节点的执行和人工节点填写的详细时序图

```plantuml
@startuml
' 用户
actor "user" as User
' 网页客户端
boundary "WebClient" as Client
' xsapce客户端
collections "xsapce" as Xspace
' sopengine
collections "SopEngine" as SopEngine
' 数据库
database "DataBase" as DB

autonumber 
== 请求SOP ==

' 请求动作执行
' User -> WebClient : 点击播放
' activate WebClient
' WebClient -> Xspace : caseId和UserInfo
' activate Xspace
' Xspace -> SopEngine : play
' activate SopEngine 
' SopEngine -> DB : caseId和sessionId
' activate DB
' return SOP信息
' return SOP节点列表
' return SOP渲染节点列表
' return 播放显示

note right Xspace
请求SOP播放信息
和SOP查询相同
end note


== 执行动作 ==

loop 存在新的分支节点
User -> Client : 点击动作按钮
activate Client
Client -> Xspace : 动作id
activate Xspace 
' 请求动作的详细信息 
Xspace -> SopEngine : 动作id
activate SopEngine 
SopEngine -> DB : 动作id
activate DB
return 动作信息
return 动作节点信息
return 动作渲染信息
return 动作详情页面
User -> Client : 执行动作
activate Client
Client -> Xspace : 动作输入参数
activate Xspace
Xspace -> SopEngine : inputMap
activate SopEngine
SopEngine -> SopEngine : 动作执行和\nSOP引擎计算
activate SopEngine #DarkSalmon
deactivate SopEngine
return outMap
return 执行结果
return 展示人工节点

User -> Client : 人工节点信息
activate Client
Client -> Xspace : 人工节点输入
activate Xspace
Xspace -> SopEngine : inputMap
activate SopEngine  
SopEngine -> SopEngine : 输入节点解析
activate SopEngine #DarkSalmon
deactivate 
SopEngine -> DB : 节点活动信息/任务状态信息
activate DB
return 更新结果
 
SopEngine -> SopEngine : 计算匹配分支
note right of SopEngine
这里的分支匹配
是跳出loop的关键
end note 
activate SopEngine #DarkSalmon
deactivate SopEngine 

SopEngine -> DB : 新节点Id
activate DB
return 节点信息
SopEngine --> Xspace : 节点列表
Xspace --> Client: 节点渲染列表
Client --> User:  SOP展示

end loop
note right of SopEngine
当推理达到末端
没有下一个节点时
生成对应的解决方案节点  
end note 
' 到达末端
activate SopEngine
SopEngine -> SopEngine : 生成解决方案
activate SopEngine #DarkSalmon
deactivate
SopEngine --> Xspace : 解决方案、最终动作
Xspace --> Client : sop渲染节点列表
Client --> User : 最终方案
' 执行最终的解决方案；这里主要有一个主流程撤销与实例状态的修改
== 执行解决方案 ==
note left of SopEngine 
解决方案的执行
以动作为核心
之后再无其它动作/人工节点
end note 
User -> Client : 执行动作
Client -> Xspace : 动作输入
Xspace -> SopEngine : 动作输入
SopEngine -> SopEngine : 动作执行
activate SopEngine #DarkSalmon
deactivate SopEngine
SopEngine -> DB : 更新task状态\n撤销主流程实例
activate DB
return 执行结果
return 执行结果
return 执行结果
return 执行结果
@enduml
```


**注意:人工因子是单选框，表单因子是多选**


