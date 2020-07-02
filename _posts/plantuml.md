```plantuml
@startuml
title 获取收信人列表时序图

' xspace
box xspace
collections "SopActionManager" as SopActionManager
end box
' gaea-apllication
box "gaea-application" #LightGreen
collections "SopActionPerformServiceImpl" as SopActionPerformService
'collections "SopActionPerformManager" as SopActionPerformManager
collections "CaseQueryManager" as CaseQueryManager
collections "CaseMapper" as CaseMapper
collections "CaseExtFieldsMapper" as CaseExtFieldsMapper
collections "OrderSenderStrategy" as OrderSenderStrategy
collections "GuoguoManager" as GuoguoManager
end box

box "gaea-platform" #LightRed
collections "ResourceServiceImpl" as ResourceService
collections "AccountServiceImpl" as AccountService
collections "BizOrderReadService" as BizOrderReadService
collections "StationReadService" as StationReadService
end box

box "taobao.wlb.res.client" #LightBlue 
collections "ResourceReadService" as ResourceReadService
end box

box "cainiao.tdtradeplatform" #899
collections "TdTradeQueryService" as TdTradeQueryService
end box 

box "taobao.loc.service" #556
collections "LocReadService" as LocReadService
end box
box "alibaba.cainiao.stationplatform" #895
collections "StationOrderService" as StationOrderService
end box

' 开始请求
autonumber 
[o-> SopActionManager : listTextReceiver
activate SopActionManager

SopActionManager -> SopActionPerformService:listTextReceiver
activate SopActionPerformService

== 查询工单信息 ==
SopActionPerformService -> CaseQueryManager : getCaseByCaseId
activate CaseQueryManager
CaseQueryManager -> CaseMapper : queryById
activate CaseMapper
return CaseDO
CaseQueryManager -> CaseExtFieldsMapper : queryByCaseId
activate CaseExtFieldsMapper

note right of CaseExtFieldsMapper
获取额
外属性信息
主要是判责
end note

return List<CaseExtFieldDO> feature
' 填充判责信息
alt ParamsUtil.isNotEmpty(feature)
    loop ResponsibilityParty : List<ResponsibilityParty>
        alt responsibilityParty.getRole() == CP
            CaseQueryManager -> ResourceService : getResourceById
            activate ResourceService
            ResourceService -> ResourceReadService : cacheGetResourceById
            activate ResourceReadService
            return ResourceDTO
            return resourceName
        else 其它角色
            CaseQueryManager -> AccountService : getAccountById
            activate AccountService
            return accountName
        end alt
    end loop
end alt

note right of CaseQueryManager
主要提取
判责方
end note

return CaseBaseInfo

note right of SopActionPerformService
主要是获取
bizOrderCode
用来查询电话
end note
' 开始查询工单中不同角色的电话
== 查询各种联系人方式 ==

' 获取订单寄件人
SopActionPerformService -> OrderSenderStrategy :getMobile
activate OrderSenderStrategy
OrderSenderStrategy -> BizOrderReadService : getBizOrder
activate BizOrderReadService
return Result<BizOrder>
return String mobile

' 获取订单收件人
SopActionPerformService -> OrderSenderStrategy :getMobile
activate OrderSenderStrategy
OrderSenderStrategy -> BizOrderReadService : getBizOrder
activate BizOrderReadService
return Result<BizOrder>
return String mobile

' 获取订单下单人

SopActionPerformService -> OrderSenderStrategy :getMobile
activate OrderSenderStrategy
OrderSenderStrategy -> GuoguoManager : getOrderPlacerMobile
activate GuoguoManager
GuoguoManager -> BizOrderReadService :getBizOrder
activate BizOrderReadService
return BizOrder
GuoguoManager -> TdTradeQueryService : queryTdOrderByBuyerIdAndOrderId
activate TdTradeQueryService


return TdOrder
note right of GuoguoManager
主要是获取
ggUserId
accountDomain
end note

GuoguoManager -> AccountService: getAccountById
activate AccountService
return Result<Account>

return String moblie
return String mobile

' 获取驿站手机
SopActionPerformService -> StationReadService : queryStationInfoByOrderCode
activate  StationReadService

StationReadService -> LocReadService : queryLogisticsOrderByCode
activate LocReadService
return SingleResultDO<LocOrderDO> 
StationReadService -> StationOrderService :queryStationOrderByOrderId
activate StationOrderService

return StationResultDTO<List<StationOrderDTO>>

return stationInfoDTOResult

' note right of SopActionManager
' 和呼叫相同，故省略
' end note
==  ==
return Result<List<ContactDTO>>
return RpcResult<List<ContactVO>>
@enduml
```