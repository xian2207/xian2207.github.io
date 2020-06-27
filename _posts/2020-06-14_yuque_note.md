# SOP代码梳理和记录

## XSPACE 简单流程整理 

> 2020-06-23 09:50:58

## SOP 唤起流程图

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

### SOP检验的时序简图

```plantuml
@startuml
title SOP总体执行流程简图

actor "User" as User
collections WebClient
collections "xspace" as Server
collections "sopengine" as Server2
database "DataBase" as DataBase


autonumber
User -> WebClient : 点击工单详情
activate WebClient
WebClient -> Server : Get:发送caseId UserId
activate Server
Server -> Server2 : 查询case和User对应的SOP信息
activate Server2
Server2 -> DataBase : 查询SOP匹配规则
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
collections  WebClient
collections  Server
database DataBase
autonumber 1
user -> WebClient : 查看工单详情
activate WebClient  
WebClient -> Server : Get: caseId and UserInfo 
activate Server

Server -> DataBase : caseId 和  userId
activate DataBase
return : case 详细信息和User详细信息

activate Server #DarkSalmon
Server -> Server : 校验用户权限和case处理是否匹配


Server --> WebClient : 最终校验成果 true/false
deactivate Server
WebClient  --> user : 显示/不显示SOP判责按钮

footer 图 1-0 总体流程图
@enduml
```

### SOP权限校验时序图

```plantuml
@startuml
title SOP权限校验逻辑时序图
actor User
collections WebClient
box "gaea-xspace-desk" #LightBlue 
collections "SopPlayController\n.match()" as SopPlayController
collections "SopPlayManager\n.match()" as SopPlayManager
end box
box "gaea-application" #LightGreen
collections "SopMatchService\n.metchSopApplyWithApplyRule()" as SopMatchService
collections "SopMatchManager\n.matchSopApply" as SopMatchManager
end box
' 中间不知名调用链路
box "middle-link" #LightYellow
collections "middle link service " as MiddleService
end box


box "sop-engine" #LightRed
collections "sop-engine" as SopEngine
end box

database  DB
autonumber
User -> WebClient : 点击工单详情页面
activate WebClient

 
WebClient -> SopPlayController : 查询订单和用户权限
activate SopPlayController
SopPlayController ->  SopPlayController : AccountUtil\n.getAccountFromRequest \n获取用户数据
activate SopPlayController #DarkSalmon
SopPlayController -> SopPlayManager : caseId和Uer 查询是否匹配
activate SopPlayManager 
SopPlayManager -> SopMatchService : MetchSopApplyRequest\n检查是否存在匹配的规则
activate SopMatchService
SopMatchService -> SopMatchManager : MetchSopApplyRequest\n 请求Sop匹配
activate SopMatchManager

SopMatchManager -> MiddleService: MetchSopApplyRequest \n 请求服务







@enduml
```

### SOP判责助手使用流程

```plantuml
@startuml
title SOP判责助手使用流程简图

actor user
collections WebClient
box Xspace #LightBlue
collections Server
end box

box middle-Server #LightGreen
collections MiddleServer
end box

box sop-engine
collections SopEngine
end box

database DB
'用户点击SOP判责助手并开始流程
autonumber 
user -> WebClient : 点击<b>SOP判责按钮</b>操作开始流程
activate WebClient 
WebClient -> Server : caseId和userInfo 进行校验
activate Server
Server -> MiddleServer : 传输数据
activate MiddleServer
MiddleServer -> SopEngine : caseId和userInfo
activate SopEngine
SopEngine -> DB : 查询数据表
activate DB
return SOP信息
return 中间信息
return 


@enduml
```


## SopEngine 代码查看记录

### 关键对外的HSF服务
1. `com.cainiao.gaea.platform.spi.component.sop.SopActionConfigService`:
- 主要功能: Sop动作适配的服务实现接口；其它没有实现，主要时通过ActionId获取对应的SopActionDTO；
- SopActionDTO 主要数据结构如下:
  ```java
  public class SopActionDTO extends BaseObject {
        // 基本数据域
        private Long id;
        private String name;
        private String code;
        private Date gmtCreate;
        private Date gmtModified;

        private String desc;
        private Integer actionCategory;
        private Integer responseCategory;
        // 
        private String preUrl;
        private String onlineUrl;
        
        private String feature;
        private Long creatorId;
        
        private Integer creatorDomain;
        private Long modifierId;
        private Integer modifierDomain;
        
        private String tenantCode;
   }
  ```
   - 对应Manager: `SopActionConfigManager`：主要功能是从SopActionDiamondConfig同步数据中根据id和tenantCode进行筛选。
   - 对应数据访问`SopActionDiamondConfig`
   - 主要是通过: DiamondConfig进行数据的同步和更新。
   - [Diamond类](http://mw.alibaba-inc.com/products/diamondserver/_book/api.html?spm=a1zco.8292286.0.0.57d42588rwJhEM)
1. `com.cainiao.gaea.platform.spi.component.sop.SopConfigServiceImpl`:
   - 主要功能: Sop规则的查询包括更新等操作
   - 对应的Manager:`SopConfigManager`:主要是对`gaea_sop_define`表的查询工作；
     - 注意：**这里的所有操作接口，都是由`Map<String, Object>`进行参数传入**
   - 主要的数据结构:
   ```java
   public class SopDTO extends BaseObject {
        private static final long serialVersionUID = -2530179139368713205L;

        private Long id;
        private Date gmtCreate;
        private Date gmtModified;
        // 名称以及相关域
        private String name;
        private Integer validStatus;
        private String desc;

        private Integer category;
        private String outerKey;
        private Long outerKeyId;
        private JSONObject tag;
        private Integer defaultSpeechTemplate;
        private String feature;

        private Long creatorId;
        private Integer creatorDomain;
        private Long modifierId;
        private Integer modifierDomain;
        
        private String tenantCode;
        private Integer enabledStatus;
   }
   ```
2. `com.cainiao.gaea.platform.spi.component.sop.SopRuntimeService`:
3. `com.cainiao.gaea.platform.spi.component.sop.SopTraceService`:

