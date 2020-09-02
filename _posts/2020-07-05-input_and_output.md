## 4. 前端接口输入和输出参数确定

### 4.1 `/action/loadSpeechTemplateContent`
request
```json
{"success":true,"data":"外呼小件员，小件员表示已经上门揽收","errorCode":null,"errorMsg":null}
```

```json
{
    "success":true,
    "data":"外呼小件员，小件员表示已经上门揽收",
    "errorCode":null,
    "errorMsg":null
}
```

### 4.2 `action/getOutBound`
request
```json
:authority: gaea-xspace-desk.taobao.com
:method: GET
:path: /action/getOutBound?caseId=1021694429990
:scheme: https
```

```json
{
    "success":true,
    "data":{
        "hotLineNumbers":[
            {
                "contactRoleName":"菜鸟消费者热线",
                "contactRole":"cnBuyerHotLine",
                "callNumber":"057156264814",
                "tuoMinCallNumber":"057****4814"
            },
            {
                "contactRoleName":"天猫海外外呼热线(香港)",
                "contactRole":"tmailHKHotLine",
                "callNumber":"0085230183612",
                "tuoMinCallNumber":"008****3612"
            },
            {
                "contactRoleName":"菜鸟消费者统一热线",
                "contactRole":"cnConsumerHotline",
                "callNumber":"9519666",
                "tuoMinCallNumber":"9****66"
            }],
        "contacts":[
            {
                "contactRoleName":"用户手机",
                "contactRole":"customerMobile",
                "callNumber":"18300891185",
                "tuoMinCallNumber":"183****1185"
            },
            {
                "contactRoleName":"收件人手机",
                "contactRole":"receiverMobile",
                "callNumber":"15019815841",
                "tuoMinCallNumber":"150****5841"
            },
            {
                "contactRoleName":"裹裹寄件人手机",
                "contactRole":"guoguoEtcherMobile",
                "callNumber":"18586907153",
                "tuoMinCallNumber":"185****7153"
            },
            {
                "contactRoleName":"顺丰",
                "contactRole":"SFExpress",
                "callNumber":"95538",
                "tuoMinCallNumber":"9****"
            },
            {
                "contactRoleName":"小件员电话",
                "contactRole":"deliveryGuyMobile",
                "callNumber":"18286052381",
                "tuoMinCallNumber":"182****2381"
            }],
        "canUserDefined":null,
        "wangwangUrl":"aliim:sendmsg?touid=cntaobaoawyzhf&siteid=cntaobao&fenliu=1&status=0"
    },
    "errorCode":null,
    "errorMsg":null
}

```

### 4.3 `newMessage`

```json
:authority: gaea-xspace-desk.taobao.com
:method: POST
:path: /caseDeal/newMessage

memo: 外呼小件员，小件员表示已经上门揽收2
maskCustomer: true
maskCP: true
contactRole: receiverMobile
messageNotifyEmail: 673018396@qq.com
caseId: 1021694429990
taskId: 1002180721251
operateType: 50
{
  "memo":"外呼小件员，小件员表示已经上门揽收2"
"maskCustomer": true
"maskCP": true
"contactRole": "receiverMobile"
"messageNotifyEmail": "673018396@qq.com"
"caseId": "1021694429990"
"taskId": "1002180721251"
"operateType": 50
}
```
```json
memo: 外呼小件员，小件员表示可上门取件你好
maskCustomer: true
maskCP: true
contactRole: receiverMobile
messageNotifyEmail: 673018396@qq.com
caseId: 1021694429990
taskId: 1002180721251
operateType: 50
attach: https://56newroute.oss-cn-shanghai.aliyuncs.com/crm_cainiao/1596080627522-KU7a-tempsnip.png
```


{
  "memo":"外呼小件员，小件员表示已经上门揽收2"
"maskCustomer": true
"maskCP": true
"contactRole": "receiverMobile"
"messageNotifyEmail": "673018396@qq.com"
"caseId": "1021837717650"
"taskId": "1002379750204"
"operateType": 50
}
```

```json
{
    "success":true,
    "data":true,
    "errorCode":null,
    "errorMsg":null
}
```

### 4.4 `action/previewText`

```json

:authority: gaea-xspace-desk.taobao.com
:method: GET
:path: /action/previewText?caseId=1021694429990&noticeTaskId=25&receiverRole=34003&templateId=150
```

```json
{
    "success":true,
    "data":{
        "mobile":"185****7153",
        "content":"【菜鸟】亲，您好。因关于您LP00183127933252订单的问题给您回电未接通，请您保持手机畅通，我们预计会在3个工作时后再次与您联系，感谢您对我们工作的支持与理解，祝您生活愉快。"
    },
    "errorCode":null,
    "errorMsg":null
}
```

### 4.5 `action/listTextReceiver`

```json
:authority: gaea-xspace-desk.taobao.com
:method: GET
:path: /action/listTextReceiver?caseId=1021694429990
:scheme: https
```

```json
{
    "success":true,
    "data":[
        {
            "contactRoleName":"订单寄件人",
            "contactRole":"34003",
            "callNumber":"18586907153",
            "tuoMinCallNumber":"185****7153"
        },
        {
            "contactRoleName":"订单收件人",
            "contactRole":"34002",
            "callNumber":"15019815841",
            "tuoMinCallNumber":"150****5841"
        },
        {
            "contactRoleName":"订单下单人",
            "contactRole":"5001",
            "callNumber":"18300891185",
            "tuoMinCallNumber":"183****1185"
        }],
    "errorCode":null,
    "errorMsg":null
}
```

### 4.6 `action/text`

```json
:authority: gaea-xspace-desk.taobao.com
:method: GET
:path: /action/text?caseId=1021694429990&noticeTaskId=25&receiverRole=34002&templateId=150
:scheme: https
```

```json
{
    "success":true,
    "data":null,
    "errorCode":null,
    "errorMsg":null
}
```

### 4.7 `action/cancelGuoguo`

```json
:authority: gaea-xspace-desk.taobao.com
:method: GET
:path: /action/cancelGuoguo?caseId=1021694429990
:scheme: https
```

```json

{"success":false,"data":null,"errorCode":null,"errorMsg":"取消订单失败"}
```

### 4.8 `action/callOutTransport`

```json
:authority: gaea-xspace-desk.taobao.com
:method: GET
:path: /action/callOutTransport?caseId=1021694429990
```

```json
{
    "success":true,
    "data":"https://member.cainiao.com/tbcnsso.htm?redirectHttps=true&type=OTHERS&returnUrl=https%3A%2F%2Fpage.cainiao.com%2Fmcn%2Fpost-platform%2Findex.html%3FcId%3DMTAwMDM%26aType%3DSFNG%26aCode%3DYTU2ODg1NjktMjI5Ni00NzBlLWEwYzItMTM5MTdkNTVhNjFh%26itemCodes%3DMzAwMDAwMDA0MA%26rName%3D6ZmI5b-X5a6J%26rMobile%3DMTUwMTk4MTU4NDE%26rAreaId%3DNDQxMzIzMTAx%26rDetailAddress%3D5Lit5Zu95bm_5Lic55yB5oOg5bee5biC5oOg5Lic5Y6_ICAg5aSn5bKt6ZWHIOa0quWPkei3rzjlj7flkI7moIvnpZ3npo_pnovljoIy5qW8KOeUqOmFjemAgeeahOWUruWQjuWNoeaIluiAheeZvee6uOWhq-WGmeWlvSDmt5jlrp3otKblj7cu5pS25Lu25Lq65aeT5ZCN77yM5omL5py66IGU57O75pa55byPLuiuouWNleWPtyDmlL7ov5vpnovnm5Ip%26sName%3D5byg57qi6Iqz%26sMobile%3DMTg1ODY5MDcxNTM%26sAreaId%3DNTIwMTEyNDAz%26sDetailAddress%3D5Lit5Zu96LS15bee55yB6LS16Ziz5biC5LmM5b2T5Yy65Yib5paw56S-5Yy65paw5bqE6Lev5paw5bqE5p2R5bCP6YOR576O5Y-R5bqX%26extIds%3DTFAwMDE4MzEyNzkzMzI1Mg%26t%3DMTU5MzkzOTY2MTkwNA%26sign%3DNjQ0ZWFiMjBlY2YzY2NjZWVmMDQ0NGYwZGQ1ZjhjMDQ%26",
    "errorCode":null,
    "errorMsg":null
}
```

### 4.9 `taskDeal/checkGetSkillGroup`

```json

:authority: gaea-xspace-desk.taobao.com
:method: GET
:path: /taskDeal/checkGetSkillGroup?taskId=1002180721251
:scheme: https
```

```json
{"success":true,"data":true,"errorCode":null,"errorMsg":null}
```

### 4.10 `action/listXSpaceTransferSkillGroup`

```json
:authority: gaea-xspace-desk.taobao.com
:method: GET
:path: /action/listXSpaceTransferSkillGroup
:scheme: https
```

```json
{
    "success":true,
    "data":[
        {
            "id":181,
            "name":"万声裹裹咨询工单组"
        },
        {
            "id":168,
            "name":"猫超破损U+升级技能组"
        },
        {
            "id":165,
            "name":"万声裹裹U+升级投诉组"
        },
        {
            "id":167,
            "name":"菜鸟驿站投诉优+升级处理组"
        },
        {
            "id":334,
            "name":"零售通U+升级技能组"
        },
        {
            "id":70,
            "name":"驿站投诉-普通组"
        },
        {
            "id":75,
            "name":"驿站咨询-普通"
        },
        {
            "id":458,
            "name":"优加驿站投诉工单二次升级组"
        }],
    "errorCode":null,
    "errorMsg":null
}

```

### 4.10  finishTask

```json
memo=%E6%82%A8%E5%A5%BD%EF%BC%8C%E6%82%A8%E5%92%A8%E8%AF%A2%E7%9A%84%E8%AE%A2%E5%8D%95%E5%8F%B7%EF%BC%9A%2A%2A%2A%2A%EF%BC%8C%E9%97%AE%E9%A2%98%E5%A4%84%E7%90%86%E7%BB%93%E6%9E%9C%E4%B8%BA%2A%2A%2A%2A%EF%BC%8C%E6%84%9F%E8%B0%A2%E6%82%A8%E5%AF%B9%E8%8F%9C%E9%B8%9F%E8%A3%B9%E8%A3%B9%E7%9A%84%E6%94%AF%E6%8C%81%EF%BC%81&sessionId=724634075553468416-1002180721251-10002&caseId=1021694429990&taskId=1002180721251
```


### 4.11 `common/getOssTempPolicy`

```json

:authority: gaea-xspace-desk.taobao.com
:method: GET
:path: /common/getOssTempPolicy
:scheme: https
```

```json
{
    "success":true,
    "data":{
        "signature":"DH0VG9EGqKdDcANi+aq8Ay3TWJs=",
        "accessid":"nXOaB8BT48mO33gK",
        "policy":"eyJleHBpcmF0aW9uIjoiMjAyMC0wNy0wNVQxNTowOTozOS4zNTRaIiwiY29uZGl0aW9ucyI6W1siY29udGVudC1sZW5ndGgtcmFuZ2UiLDAsMjA5NzE1Ml0sWyJzdGFydHMtd2l0aCIsIiRrZXkiLCJjcm1fY2Fpbmlhby8iXV19",
        "fileName":"1593958179354-YRCy-",
        "dir":"crm_cainiao/",
        "host":"https://56newroute.oss-cn-shanghai.aliyuncs.com/",
        "expire":"1593961779",
        "Access-Control-Allow-Origin":"*",
        "Access-Control-Allow-Methods":"GET, POST"
    },
    "errorCode":null,
    "errorMsg":null
}
```

### 4.12 https://56newroute.oss-cn-shanghai.aliyuncs.com/

```json
Accept: */*
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9,zh-CN;q=0.8,zh;q=0.7
Connection: keep-alive
Content-Length: 5245
Content-Type: multipart/form-data; boundary=----WebKitFormBoundary45z8jbEDEYSLABeh
Host: 56newroute.oss-cn-shanghai.aliyuncs.com


key: crm_cainiao/1593958179354-YRCy-2019-04-10-markdown_learning_note.md
policy: eyJleHBpcmF0aW9uIjoiMjAyMC0wNy0wNVQxNTowOTozOS4zNTRaIiwiY29uZGl0aW9ucyI6W1siY29udGVudC1sZW5ndGgtcmFuZ2UiLDAsMjA5NzE1Ml0sWyJzdGFydHMtd2l0aCIsIiRrZXkiLCJjcm1fY2Fpbmlhby8iXV19
OSSAccessKeyId: nXOaB8BT48mO33gK
sucess_action_status: 200
signature: DH0VG9EGqKdDcANi aq8Ay3TWJs=
file: (binary)
```



```json


```

### 4.13 taskDeal/listRemindType

```json

:authority: gaea-xspace-desk.taobao.com
:method: GET
:path: /taskDeal/listRemindType?_api=listRemindType&_mock=false&_stamp=1593958531947


```

```json
// 返回分类提醒
{
    "success":true,
    "data":[
        {
            "label":"211回呼",
            "value":"1"
        },
        {
            "label":"待回访",
            "value":"3"
        },
        {
            "label":"待举证",
            "value":"4"
        },
        {
            "label":"待履约",
            "value":"5"
        },
        {
            "label":"待解决方案确认",
            "value":"6"
        },
        {
            "label":"待CP举证回复",
            "value":"7"
        },
        {
            "label":"待商家举证回复",
            "value":"8"
        },
        {
            "label":"待goc回复",
            "value":"9"
        },
        {
            "label":"订单托管",
            "value":"10"
        },
        {
            "label":"其他",
            "value":"11"
        }],
    "errorCode":null,
    "errorMsg":null
}

```

### 4.15 http://gm.mmstat.com/gaea-desk.gaeaDialog.endTimeLength?endTime=1593958711996&dialogType=ticketHangUpVisible&id=1021694429990&t=1593958711996&platform=xspace&userId=8992088
这个是干什么的

```json

http://gm.mmstat.com/gaea-desk.gaeaDialog.endTimeLength?endTime=1593958711996&dialogType=ticketHangUpVisible&id=1021694429990&t=1593958711996&platform=xspace&userId=8992088


```
### 4.14 taskDeal/setReminder

```json
:authority: gaea-xspace-desk.taobao.com
:method: POST
:path: /taskDeal/setReminder?_api=setReminder&_mock=false&_stamp=1594000879340

{
    "caseId":1021694429990,
    "taskId":1002180721251,
    "remindType":"4",
    "remindTime":"2020-07-06T02:10:36.000Z",
    "remindNote":"测试挂起",
    "ignoreTimeConstraint":false /* 是否强制挂起 */
}

```

```json
{
    "success":true,
    "data":{
        "timeValid":false,
        "tip":"已超出全链路处理效:2020-06-22 17:09:24请重新选择"
    },
    "errorCode":null,
    "errorMsg":null
}
```
ignoreTimeConstraint 会检查时效；可以选择强制挂起

```json
{
    "caseId":1021694429990,
    "taskId":1002180721251,
    "remindType":"10",
    "remindTime":"2020-07-06T02:19:37.000Z",
    "remindNote":"挂起测试",
    "ignoreTimeConstraint":true
}


```



### 工单挂起

```json
https://gaea-xspace-desk.taobao.com/taskDeal/setReminder?_api=setReminder&_mock=false&_stamp=1594785596022
{"caseId":1021694429990,"taskId":1002180721251,"remindType":"5","remindTime":"2020-07-15T04:09:44.000Z","remindNote":"测试","ignoreTimeConstraint":false}
```
```json
caseId: 1021694429990
ignoreTimeConstraint: true
remindNote: "工单挂起测试"
remindTime: "2020-07-16T06:57:13.000Z"
remindType: "4"
taskId: 1002180721251
```
注意会产生超出全链路处理时效

```json
{"success":true,"data":{"timeValid":false,"tip":"已超出全链路处理效:2020-06-22 17:09:24请重新选择"},"errorCode":null,"errorMsg":null}
```


### xspace 咨询结单
```json
memo: 您好，您咨询的订单号：****，问题处理结果为****，感谢您对菜鸟裹裹的支持！
sessionId: 724634075553468416-1002180721251-10002
caseId: 1021694429990
taskId: 1002180721251
```

### sop drive

```json
{
    "sessionId":"724634075553468416-1002180721251-10002",
    "nodeTag":"factor_6ysnck1qv4w0000",
    "bpmKey":"1568023898166_b541",
    "interactiveNodeDTO":{
        "attributeMap":{
            "attributeCode":"lianxibushankuaidiy"
        }
    },
    "parameters":{
        "lpCode":"LP00183127933252",
        "gaeaBizOrderCode":"LP00183127933252",
        "gaeaCaseId":1021694429990
    }
}
```

```json
{
    "success":true,
    "data":{
        "sessionId":"724634075553468416-1002180721251-10002",
        "coreBizOrderNo":"1002180721251",
        "bizOrderDomain":10002,
        "end":false,
        "sopId":null,
        "sopName":null,
        "nodeList":[
            {
                "nodeTag":"factor_6ysnck1qv4w0000",
                "nodeName":"外呼小件员结果",
                "nodeType":"simple_interactive_factor",
                "solutionType":null,
                "processKey":"1568023898166_b541",
                "inputParamMap":{
                    "e":{
                        "nodeResult":"lianxibushankuaidiy"
                    },
                    "nodeResult":"lianxibushankuaidiy",
                    "attributeMap":{
                        "attributeCode":"lianxibushankuaidiy"
                    }
                },
                "outputParamMap":{
                },
                "display":true,
                "nodeTip":null,
                "status":"COMPLETED",
                "nodeData":null,
                "extFields":{
                }
            },

            {
                "nodeTag":"action_5b367sdk76s0000",
                "nodeName":"动作",
                "nodeType":"action",
                "solutionType":null,
                "processKey":"1568023898166_b541",
                "inputParamMap":{
                },
                "outputParamMap":{
                    "result":{
                        "branchResult":"Default"
                    },
                    "e":{
                        "result":{
                            "branchResult":"Default"
                        }
                    },
                    "nodeResult":[
                        "9"]
                },
                "display":true,
                "nodeTip":null,
                "status":"COMPLETED",
                "nodeData":[
                    {
                        "id":9,
                        "name":"留言",
                        "code":"addNewMessasge",
                        "gmtCreate":1565170392000,
                        "gmtModified":1565170396000,
                        "desc":"留言",
                        "actionCategory":1,
                        "responseCategory":1,
                        "preUrl":"https://mtop.taobao.com",
                        "onlineUrl":"https://mtop.taobao.com",
                        "feature":"cloud_xiaoer",
                        "creatorId":107345,
                        "creatorDomain":81,
                        "modifierId":107345,
                        "modifierDomain":81,
                        "tenantCode":"1"
                    }],
                "extFields":{
                    "leaveMessageTemplateId":"1433"
                }
            },

            {
                "nodeTag":"factor_a52s22ow0f40000_k6vmj48l_k6vq82tf_k6vqdlyq_k71p7inw_k72sa2wr",
                "nodeName":"是否联系上消费者（裹裹）",
                "nodeType":"simple_interactive_factor",
                "solutionType":null,
                "processKey":"1568023898166_b541",
                "inputParamMap":{
                },
                "outputParamMap":{
                    "result":{
                        "branchResult":"Default"
                    },
                    "s":{
                        "result":{
                            "branchResult":"Default"
                        }
                    },
                    "nodeResult":[
                        {
                            "id":6941,
                            "factorId":874,
                            "attributeValue":"no",
                            "attributeName":"否",
                            "attributeType":1,
                            "priority":2,
                            "status":1,
                            "tenantId":1,
                            "gmtModified":1568796785000,
                            "gmtCreate":1568796785000,
                            "creatorId":771261,
                            "creatorDomain":82,
                            "modifierId":771261,
                            "modifierDomain":82
                        },
                        {
                            "id":6938,
                            "factorId":874,
                            "attributeValue":"yes",
                            "attributeName":"是",
                            "attributeType":1,
                            "priority":1,
                            "status":1,
                            "tenantId":1,
                            "gmtModified":1568796683000,
                            "gmtCreate":1568796683000,
                            "creatorId":771261,
                            "creatorDomain":82,
                            "modifierId":771261,
                            "modifierDomain":82
                        }]
                },
                "display":true,
                "nodeTip":null,
                "status":"WAIT",
                "nodeData":[
                    {
                        "id":6941,
                        "factorId":874,
                        "attributeValue":"no",
                        "attributeName":"否",
                        "attributeType":1,
                        "priority":2,
                        "status":1,
                        "tenantId":1,
                        "gmtModified":1568796785000,
                        "gmtCreate":1568796785000,
                        "creatorId":771261,
                        "creatorDomain":82,
                        "modifierId":771261,
                        "modifierDomain":82
                    },
                    {
                        "id":6938,
                        "factorId":874,
                        "attributeValue":"yes",
                        "attributeName":"是",
                        "attributeType":1,
                        "priority":1,
                        "status":1,
                        "tenantId":1,
                        "gmtModified":1568796683000,
                        "gmtCreate":1568796683000,
                        "creatorId":771261,
                        "creatorDomain":82,
                        "modifierId":771261,
                        "modifierDomain":82
                    }],
                "extFields":{
                }
            }
            
        ],
        "parameters":{
            "lpCode":"LP00183127933252",
            "gaeaBizOrderCode":"LP00183127933252",
            "gaeaCaseId":1021694429990
        },
        "resumedPlay":null
    },
    "errorCode":null,
    "errorMsg":null
}

```

```json
{
    "sessionId":"724634075553468416-1002180721251-10002",
    "nodeTag":"factor_a52s22ow0f40000_k6vmj48l_k6vq82tf_k6vqdlyq_k71p7inw_k72sa2wr",
    "bpmKey":"1568023898166_b541",
    "interactiveNodeDTO":{
        "attributeMap":{
            "attributeCode":"yes"
        }
    },
    "parameters":{
        "lpCode":"LP00183127933252",
        "gaeaBizOrderCode":"LP00183127933252",
        "gaeaCaseId":1021694429990
    }
}

```

```json

{
    "success":true,
    "data":{
        "sessionId":"724634075553468416-1002180721251-10002",
        "coreBizOrderNo":"1002180721251",
        "bizOrderDomain":10002,
        "end":false,
        "sopId":null,
        "sopName":null,
        "nodeList":[
            {
                "nodeTag":"factor_a52s22ow0f40000_k6vmj48l_k6vq82tf_k6vqdlyq_k71p7inw_k72sa2wr",
                "nodeName":"是否联系上消费者（裹裹）",
                "nodeType":"simple_interactive_factor",
                "solutionType":null,
                "processKey":"1568023898166_b541",
                "inputParamMap":{
                    "e":{
                        "nodeResult":"yes"
                    },
                    "nodeResult":"yes",
                    "attributeMap":{
                        "attributeCode":"yes"
                    }
                },
                "outputParamMap":{
                },
                "display":true,
                "nodeTip":null,
                "status":"COMPLETED",
                "nodeData":null,
                "extFields":{
                }
            },


            {
                "nodeTag":"action_u70vtavgnnk000_k72sa2wr",
                "nodeName":"动作",
                "nodeType":"action",
                "solutionType":null,
                "processKey":"1568023898166_b541",
                "inputParamMap":{
                },
                "outputParamMap":{
                    "result":{
                        "branchResult":"Default"
                    },
                    "e":{
                        "result":{
                            "branchResult":"Default"
                        }
                    },
                    "nodeResult":[
                        "1"]
                },
                "display":true,
                "nodeTip":null,
                "status":"COMPLETED",
                "nodeData":[
                    {
                        "id":1,
                        "name":"外呼",
                        "code":"call",
                        "gmtCreate":1560914802000,
                        "gmtModified":1560914802000,
                        "desc":"外呼",
                        "actionCategory":1,
                        "responseCategory":1,
                        "preUrl":"https://www.google.com",
                        "onlineUrl":"https://www.google.com",
                        "feature":"testtest",
                        "creatorId":107345,
                        "creatorDomain":82,
                        "modifierId":107345,
                        "modifierDomain":82,
                        "tenantCode":"1"
                    }],
                "extFields":{
                }
            },


            {
                "nodeTag":"factor_6kdxq4ssf000000_k71s9cbx_k71se6qf_k72rtyko_k7kb4db5",
                "nodeName":"是否揽收",
                "nodeType":"simple_interactive_factor",
                "solutionType":null,
                "processKey":"1568023898166_b541",
                "inputParamMap":{
                },
                "outputParamMap":{
                    "result":{
                        "branchResult":"Default"
                    },
                    "s":{
                        "result":{
                            "branchResult":"Default"
                        }
                    },
                    "nodeResult":[
                        {
                            "id":7190,
                            "factorId":988,
                            "attributeValue":"no",
                            "attributeName":"否",
                            "attributeType":1,
                            "priority":2,
                            "status":1,
                            "tenantId":1,
                            "gmtModified":1569321904000,
                            "gmtCreate":1569321904000,
                            "creatorId":771261,
                            "creatorDomain":82,
                            "modifierId":771261,
                            "modifierDomain":82
                        },
                        {
                            "id":7189,
                            "factorId":988,
                            "attributeValue":"yes",
                            "attributeName":"是",
                            "attributeType":1,
                            "priority":1,
                            "status":1,
                            "tenantId":1,
                            "gmtModified":1569321898000,
                            "gmtCreate":1569321898000,
                            "creatorId":771261,
                            "creatorDomain":82,
                            "modifierId":771261,
                            "modifierDomain":82
                        }]
                },
                "display":true,
                "nodeTip":null,
                "status":"WAIT",
                "nodeData":[
                    {
                        "id":7190,
                        "factorId":988,
                        "attributeValue":"no",
                        "attributeName":"否",
                        "attributeType":1,
                        "priority":2,
                        "status":1,
                        "tenantId":1,
                        "gmtModified":1569321904000,
                        "gmtCreate":1569321904000,
                        "creatorId":771261,
                        "creatorDomain":82,
                        "modifierId":771261,
                        "modifierDomain":82
                    },
                    {
                        "id":7189,
                        "factorId":988,
                        "attributeValue":"yes",
                        "attributeName":"是",
                        "attributeType":1,
                        "priority":1,
                        "status":1,
                        "tenantId":1,
                        "gmtModified":1569321898000,
                        "gmtCreate":1569321898000,
                        "creatorId":771261,
                        "creatorDomain":82,
                        "modifierId":771261,
                        "modifierDomain":82
                    }],
                "extFields":{
                }
            }
            
        ],
        "parameters":{
            "lpCode":"LP00183127933252",
            "gaeaBizOrderCode":"LP00183127933252",
            "gaeaCaseId":1021694429990
        },
        "resumedPlay":null
    },
    "errorCode":null,
    "errorMsg":null
}
```
### 4.16 cdesk 

```json
{
    "functionCode":"addNewMessage",
    "functionConfigInput":"{\"memo\":\"测试留言\",\"attachList\":[],\"caseId\":\"1011733648012\",\"taskId\":\"1002239796652\"}"
}
```

```json
{
  "success": true,
  "data": true,
  "errorCode": null,
  "errorMsg": null
}
```


### 4.17 cdesk 判责
1. 点击获取基本信息:`function/structJudge/queryJudgeCase`
```json
https://pre-desk.cainiao.com/function/structJudge/queryJudgeCase?caseId=1011741048007
```

```json
{
    "success":true,
    "data":{
        "bizLineId":24,
        "categoryId":1000000114,
        "setupId":null,
        // 业务线
        "bizLineName":"快消",
        // 投诉类型
        "categoryName":"投诉测试用，勿删",
        "setupName":null
    },
    "errorCode":null,
    "errorMsg":null
}

```
2. 获取责任方列表:`function/generalJudge`

```json
https://pre-desk.cainiao.com/function/generalJudge/cpJudgeListResponsibility?caseId=1011741048007
```
```json
{
    "success":true,
    "data":[
        {
            "label":"STB测试仓",
            "value":"3888275216",
            "responsibilityPartyRole":3,
            "accountDomain":81,
            "responsibilityPartyResId":5000000100570,
            "responsibilityPartyResTypeName":"仓库"
        },
        {
            "label":"urc创建配送anjun",
            "value":"695435227",
            "responsibilityPartyRole":3,
            "accountDomain":81,
            "responsibilityPartyResId":1216000000178348,
            "responsibilityPartyResTypeName":"配送"
        },
        {
            "label":"浙江菜鸟供应链管理有限公司",
            "value":"2183694116",
            "responsibilityPartyRole":7,
            "accountDomain":81,
            "responsibilityPartyResId":5000000075004,
            "responsibilityPartyResTypeName":null
        }],
    "errorCode":null,
    "errorMsg":null
}
```


3. 获取成立类型:`structJudge/queryCaseCategory`
```json
 https://pre-desk.cainiao.com/function/structJudge/queryCaseCategory?caseId=1011741048007&functionCode=general_judge

```
```json
{
    "success":true,
    "data":[
        {
            "value":1000000080,
            "label":"主动服务--货品丢失"
        },
        {
            "value":1000000018,
            "label":"服务态度"
        },
        {
            "value":1000000032,
            "label":"逆向丢失"
        },
        {
            "value":1000000033,
            "label":"逆向破损"
        },
        {
            "value":1000000012,
            "label":"外包完好，内件破损"
        },
        {
            "value":1000000013,
            "label":"外包破损，内件破损"
        },
        {
            "value":1000000014,
            "label":"外包破损，内件完好"
        },
        {
            "value":1000000019,
            "label":"非正常签收"
        },
        {
            "value":1000000028,
            "label":"货品丢失"
        },
        {
            "value":1000000022,
            "label":"配送延迟"
        },
        {
            "value":1000003671,
            "label":"天猫直送-未按预约配送"
        },
        {
            "value":1000005530,
            "label":"主动服务--确认货品丢失（小二）"
        },
        {
            "value":1000000309,
            "label":"头程单测试"
        },
        {
            "value":1000000114,
            "label":"投诉测试用，勿删"
        },
        {
            "value":1000006879,
            "label":"投名状测试"
        },
        {
            "value":1000010313,
            "label":"服务规范（配）"
        },
        {
            "value":1000010159,
            "label":"服务规范（仓）"
        },
        {
            "value":1000003219,
            "label":"服务规范"
        },
        {
            "value":1000000188,
            "label":"主动服务-逆向拒签拦截部分丢失/破损"
        },
        {
            "value":1000000186,
            "label":"主动服务-逆向拒签拦截破损"
        },
        {
            "value":1000000183,
            "label":"主动服务-逆向拒签拦截全部丢失"
        },
        {
            "value":1000000184,
            "label":"主动服务-逆向拒签拦截部分丢失"
        },
        {
            "value":1000004107,
            "label":"投名状测试误删"
        },
        {
            "value":1000000016,
            "label":"发货错误"
        },
        {
            "value":1000000037,
            "label":"未送货上门"
        },
        {
            "value":1000006875,
            "label":"南汴测试类型"
        },
        {
            "value":1000000306,
            "label":"服务体验"
        },
        {
            "value":1000000307,
            "label":"业务体验"
        },
        {
            "value":1000003670,
            "label":"未送货上门（买家）"
        },
        {
            "value":1000000645,
            "label":"保质期问题"
        },
        {
            "value":1000003232,
            "label":"控温仓服务"
        },
        {
            "value":1000000024,
            "label":"包材使用错误"
        },
        {
            "value":1000000021,
            "label":"发货延迟"
        },
        {
            "value":1000000115,
            "label":"发出货品过期"
        },
        {
            "value":1000000082,
            "label":"主动服务--确认货品丢失"
        },
        {
            "value":1000000081,
            "label":"主动服务--确认货品破损"
        },
        {
            "value":1000000120,
            "label":"主动服务--货品破损"
        },
        {
            "value":1000006000,
            "label":"主动服务--CP提报已出未揽"
        },
        {
            "value":1000000031,
            "label":"包材库存差异"
        },
        {
            "value":1000002677,
            "label":"主动服务-逆向拒签拦截部分丢失（CP提报认责）"
        },
        {
            "value":1000002678,
            "label":"主动服务-逆向拒签拦截全部丢失（CP提报认责）"
        },
        {
            "value":1000002679,
            "label":"主动服务-逆向拒签拦截部分丢失/破损（CP提报认责）"
        },
        {
            "value":1000002680,
            "label":"主动服务-逆向拒签拦截部分丢失（CP提报责任待定）"
        },
        {
            "value":1000002681,
            "label":"主动服务-逆向拒签拦截全部丢失（CP提报责任待定）"
        },
        {
            "value":1000002682,
            "label":"主动服务-逆向拒签拦截部分丢失/破损（CP提报责任待定）"
        },
        {
            "value":1000002683,
            "label":"主动服务-逆向拒签拦截破损（CP提报认责）"
        },
        {
            "value":1000002684,
            "label":"主动服务-逆向拒签拦截破损（CP提报认责待定）"
        },
        {
            "value":1000003105,
            "label":"双12逆向尾单清理（逆向丢失投诉）"
        },
        {
            "value":1000003104,
            "label":"双11逆向尾单清理（逆向丢失投诉）"
        }],
    "errorCode":null,
    "errorMsg":null
}


```

4. 点击确定进行判责:`https://pre-desk.cainiao.com/function/executeFunctionhttps://pre-desk.cainiao.com/function/executeFunction`

```json
{
    "functionCode":"generalJudge",
    "functionConfigInput":"{\"caseId\":\"1011741048007\",\"taskId\":\"1002272057003\",\"result\":true,\"memo\":\"判责测试\",\"category\":1000000114,\"attachList\":[],\"responsibilityPartyList\":[{\"name\":\"urc创建配送anjun\",\"id\":\"695435227\",\"role\":3,\"responsibilityResourceId\":1216000000178348,\"accountDomain\":81,\"percent\":100}],\"dealerRole\":3}"
}
// 注意这里的result表示是否接受
```

### 申请菜鸟小二介入

```
https://pre-desk.cainiao.com/function/executeFunction

post

{
    "functionCode":"applyCnDealerStepIn",
    "functionConfigInput":"{\"memo\":\"转交测试\",\"actionCode\":\"cnDealerStepIn\",\"caseId\":\"1011741048007\",\"taskId\":\"1002272057003\"}"
}

```


### sop play

```json
Request URL: https://gaea-xspace-desk.taobao.com/sopPlay/play?caseId=1021694429990
Request Method: GET
caseId=1021694429990
```

```json
{
    "success":true,
    "data":{
        "sessionId":"724634075553468416-1002180721251-10002",
        "coreBizOrderNo":"1002180721251",
        "bizOrderDomain":10002,
        "end":false,
        "sopId":85,
        "sopName":"优加-裹裹 -快递员什么时候来取件",
        "nodeList":[
            {
                "nodeTag":"factor_czy7donob7k0000",
                "nodeName":"【裹裹】订单是否取消",
                "nodeType":"system_factor",
                "solutionType":null,
                "processKey":"1568023898166_b541",
                "inputParamMap":{
                },
                "outputParamMap":{
                    "nodeResultDesc":"未取消",
                    "result":{
                        "branchResult":"no"
                    },
                    "e":{
                        "result":{
                            "branchResult":"no"
                        }
                    },
                    "nodeResult":"no"
                },
                "display":true,
                "nodeTip":null,
                "status":"COMPLETED",
                "nodeData":null,
                "extFields":{
                }
            },
            {
                "nodeTag":"action_80u1ia682cw0000",
                "nodeName":"动作",
                "nodeType":"action",
                "solutionType":null,
                "processKey":"1568023898166_b541",
                "inputParamMap":{
                },
                "outputParamMap":{
                    "result":{
                        "branchResult":"Default"
                    },
                    "e":{
                        "result":{
                            "branchResult":"Default"
                        }
                    },
                    "nodeResult":[
                        "1"]
                },
                "display":true,
                "nodeTip":null,
                "status":"COMPLETED",
                "nodeData":[
                    {
                        "id":1,
                        "name":"外呼",
                        "code":"call",
                        "gmtCreate":1560914802000,
                        "gmtModified":1560914802000,
                        "desc":"外呼",
                        "actionCategory":1,
                        "responseCategory":1,
                        "preUrl":"https://www.google.com",
                        "onlineUrl":"https://www.google.com",
                        "feature":"testtest",
                        "creatorId":107345,
                        "creatorDomain":82,
                        "modifierId":107345,
                        "modifierDomain":82,
                        "tenantCode":"1"
                    }],
                "extFields":{
                }
            },
            {
                "nodeTag":"factor_6ysnck1qv4w0000",
                "nodeName":"外呼小件员结果",
                "nodeType":"simple_interactive_factor",/* 简单人工因子 */
                "solutionType":null,
                "processKey":"1568023898166_b541",/* 关键处理因子 */
                "inputParamMap":{
                },
                "outputParamMap":{
                    "result":{
                        "branchResult":"Default"
                    },
                    "s":{
                        "result":{
                            "branchResult":"Default"
                        }
                    },
                    "nodeResult":[
                        {
                            "id":8255,
                            "factorId":1306,
                            "attributeValue":"xiaojianyuanyls",
                            "attributeName":"小件员表示已揽收",
                            "attributeType":1,
                            "priority":7,
                            "status":1,
                            "tenantId":1,
                            "gmtModified":1571987685000,
                            "gmtCreate":1571987685000,
                            "creatorId":851944,
                            "creatorDomain":82,
                            "modifierId":851944,
                            "modifierDomain":82
                        },
                        {
                            "id":8254,
                            "factorId":1306,
                            "attributeValue":"keshangmensj",
                            "attributeName":"预计XXXX时间可上门",
                            "attributeType":1,
                            "priority":6,
                            "status":1,
                            "tenantId":1,
                            "gmtModified":1571987685000,
                            "gmtCreate":1571987685000,
                            "creatorId":851944,
                            "creatorDomain":82,
                            "modifierId":851944,
                            "modifierDomain":82
                        },
                        {
                            "id":8253,
                            "factorId":1306,
                            "attributeValue":"yijingxianxiajichu",
                            "attributeName":"小件员表示消费者已经线下寄出，无需上门取件",
                            "attributeType":1,
                            "priority":5,
                            "status":1,
                            "tenantId":1,
                            "gmtModified":1571987685000,
                            "gmtCreate":1571987685000,
                            "creatorId":851944,
                            "creatorDomain":82,
                            "modifierId":851944,
                            "modifierDomain":82
                        },
                        {
                            "id":8252,
                            "factorId":1306,
                            "attributeValue":"chaiquwufaqujian",
                            "attributeName":"此单超区，无法取件",
                            "attributeType":1,
                            "priority":4,
                            "status":1,
                            "tenantId":1,
                            "gmtModified":1571987685000,
                            "gmtCreate":1571987685000,
                            "creatorId":851944,
                            "creatorDomain":82,
                            "modifierId":851944,
                            "modifierDomain":82
                        },
                        {
                            "id":8251,
                            "factorId":1306,
                            "attributeValue":"xjylianxibus",
                            "attributeName":"小件员表示联系不上消费者",
                            "attributeType":1,
                            "priority":3,
                            "status":1,
                            "tenantId":1,
                            "gmtModified":1571987685000,
                            "gmtCreate":1571987685000,
                            "creatorId":851944,
                            "creatorDomain":82,
                            "modifierId":851944,
                            "modifierDomain":82
                        },
                        {
                            "id":8250,
                            "factorId":1306,
                            "attributeValue":"kehuyoujweijinp",
                            "attributeName":"小件员表示客户邮寄的是违禁品",
                            "attributeType":1,
                            "priority":2,
                            "status":1,
                            "tenantId":1,
                            "gmtModified":1571987685000,
                            "gmtCreate":1571987685000,
                            "creatorId":851944,
                            "creatorDomain":82,
                            "modifierId":851944,
                            "modifierDomain":82
                        },
                        {
                            "id":8249,
                            "factorId":1306,
                            "attributeValue":"lianxibushankuaidiy",
                            "attributeName":"联系不上小件员/小件员拒绝取件",
                            "attributeType":1,
                            "priority":1,
                            "status":1,
                            "tenantId":1,
                            "gmtModified":1571987707000,
                            "gmtCreate":1571987685000,
                            "creatorId":851944,
                            "creatorDomain":82,
                            "modifierId":851944,
                            "modifierDomain":82
                        }]
                },
                "display":true,
                "nodeTip":null,
                "status":"WAIT",
                "nodeData":[
                    {
                        "id":8255,
                        "factorId":1306,
                        "attributeValue":"xiaojianyuanyls",
                        "attributeName":"小件员表示已揽收",
                        "attributeType":1,
                        "priority":7,
                        "status":1,
                        "tenantId":1,
                        "gmtModified":1571987685000,
                        "gmtCreate":1571987685000,
                        "creatorId":851944,
                        "creatorDomain":82,
                        "modifierId":851944,
                        "modifierDomain":82
                    },
                    {
                        "id":8254,
                        "factorId":1306,
                        "attributeValue":"keshangmensj",
                        "attributeName":"预计XXXX时间可上门",
                        "attributeType":1,
                        "priority":6,
                        "status":1,
                        "tenantId":1,
                        "gmtModified":1571987685000,
                        "gmtCreate":1571987685000,
                        "creatorId":851944,
                        "creatorDomain":82,
                        "modifierId":851944,
                        "modifierDomain":82
                    },
                    {
                        "id":8253,
                        "factorId":1306,
                        "attributeValue":"yijingxianxiajichu",
                        "attributeName":"小件员表示消费者已经线下寄出，无需上门取件",
                        "attributeType":1,
                        "priority":5,
                        "status":1,
                        "tenantId":1,
                        "gmtModified":1571987685000,
                        "gmtCreate":1571987685000,
                        "creatorId":851944,
                        "creatorDomain":82,
                        "modifierId":851944,
                        "modifierDomain":82
                    },
                    {
                        "id":8252,
                        "factorId":1306,
                        "attributeValue":"chaiquwufaqujian",
                        "attributeName":"此单超区，无法取件",
                        "attributeType":1,
                        "priority":4,
                        "status":1,
                        "tenantId":1,
                        "gmtModified":1571987685000,
                        "gmtCreate":1571987685000,
                        "creatorId":851944,
                        "creatorDomain":82,
                        "modifierId":851944,
                        "modifierDomain":82
                    },
                    {
                        "id":8251,
                        "factorId":1306,
                        "attributeValue":"xjylianxibus",
                        "attributeName":"小件员表示联系不上消费者",
                        "attributeType":1,
                        "priority":3,
                        "status":1,
                        "tenantId":1,
                        "gmtModified":1571987685000,
                        "gmtCreate":1571987685000,
                        "creatorId":851944,
                        "creatorDomain":82,
                        "modifierId":851944,
                        "modifierDomain":82
                    },
                    {
                        "id":8250,
                        "factorId":1306,
                        "attributeValue":"kehuyoujweijinp",
                        "attributeName":"小件员表示客户邮寄的是违禁品",
                        "attributeType":1,
                        "priority":2,
                        "status":1,
                        "tenantId":1,
                        "gmtModified":1571987685000,
                        "gmtCreate":1571987685000,
                        "creatorId":851944,
                        "creatorDomain":82,
                        "modifierId":851944,
                        "modifierDomain":82
                    },
                    {
                        "id":8249,
                        "factorId":1306,
                        "attributeValue":"lianxibushankuaidiy",
                        "attributeName":"联系不上小件员/小件员拒绝取件",
                        "attributeType":1,
                        "priority":1,
                        "status":1,
                        "tenantId":1,
                        "gmtModified":1571987707000,
                        "gmtCreate":1571987685000,
                        "creatorId":851944,
                        "creatorDomain":82,
                        "modifierId":851944,
                        "modifierDomain":82
                    }],
                "extFields":{
                }
            }],
        "parameters":{
            "lpCode":"LP00183127933252",
            "gaeaBizOrderCode":"LP00183127933252",
            "gaeaCaseId":1021694429990
        },
        "resumedPlay":false
    },
    "errorCode":null,
    "errorMsg":null
}

```

drive
```json
{
    "sessionId":"724634075553468416-1002180721251-10002",
    "nodeTag":"factor_6ysnck1qv4w0000",
    "bpmKey":"1568023898166_b541",
    "interactiveNodeDTO":{
        "attributeMap":{
            "attributeCode":"xiaojianyuanyls"
        }
    },
    "parameters":{
        "lpCode":"LP00183127933252",
        "gaeaBizOrderCode":"LP00183127933252",
        "gaeaCaseId":1021694429990
    }
}
```




```json
{
    "functionCode":"addNewMessage",
    "functionConfigInput":"{\"memo\":\"猫拨号界面\",\"attachList\":[{\"attach\":\"https://56newroute.oss-cn-shanghai.aliyuncs.com/crm_cainiao/1594781695098-BdlT-plantuml.jpeg\"}],\"caseId\":\"1011741048007\",\"taskId\":\"1002272057003\",\"hideType\":2}"
}
```

```json
{
    "functionCode":"outBoundCall"

}
```
https://56newroute.oss-cn-shanghai.aliyuncs.com/crm_cainiao/1595337546376-dT4h-2019-07-03 21-04-58.png,https://56newroute.oss-cn-shanghai.aliyuncs.com/crm_cainiao/1595337551070-Hqyc-2019-07-11-10-49-24.png

### cdesk 工单挂起
```json
{
    "functionCode":"caseHangUp",
    "functionConfigInput":"{\"caseId\":\"1011741048007\",\"taskId\":\"1002272057003\",\"remindType\":1,
    \"remindTime\":\"2020-07-15 11:51:19\",\"note\":\"测试挂起\",
    \"bizDomain\":2,
    \"bizId\":\"1002272057003\",\"notifyChannel\":1,
    \"receiverDomain\":83,\"receiverId\":2208144684830,\"remindCreateType\":\"2\",
    \"remindSpan\":9,\"remindStart\":1594784485817,\"reminderDomain\":83,\"reminderId\":2208144684830,
    \"source\":1,\"spanType\":1}"
}

```
```
"caseId":1021694429990,"taskId":1002180721251

1011801737002&taskId=1002350116105
```

```json
{
  "functionCode": "caseHangUp",
  "functionConfigInput": "{\"caseId\":\"1021694429990\",\"taskId\":\"1002180721251\",\"remindType\":1,\"remindTime\":\"2020-07-16 12:00:45\",\"note\":\"测试挂起\",\"bizDomain\":2,\"bizId\":\"1002272057003\",\"notifyChannel\":1,\"receiverDomain\":83,\"receiverId\":2208144684830,\"remindCreateType\":\"2\",\"remindSpan\":9,\"remindStart\":1594871458116,\"reminderDomain\":83,\"reminderId\":2208144684830,\"source\":1,\"spanType\":1}"},
  "tenantCode": "CAINIAO"
}
```

966


14:33:09 [http-nio-7001-exec-4] INFO  c.c.g.c.w.l.CDeskContextInterceptor-|2020-07-27 14:33:09|服务人员信息是{"attributeMap":{"dayLoad":1000,"dutyType":5,"servicerRole":9,"isUsePersonalHotlineConfig":true,"mobile":"15372073382","emergency":0,"xspaceDepartmentId":498,"isAdmin":2,"employeeNo":"testU123","employeeStatus":1,"entryTime":1590537600000,"ocsPriority":0,"firstLevelDepartmentId":3561,"ocsDisplayName":"测试疏爽","xspaceServicerId":30936042,"levelId":703001,"name":"疏爽测试","userType":1,"ocsMaxServiceNum":3,"tbId":2208170554321,"ocsIsInitiativeSendMsg":0},"departmentDTO":{"aeapDepartmentId":2323109,"attributes":{"servicerRole":9,"ocsNomalMinLevelName":"初学弟子","ocsNoAnswerTime":60,"xspaceDepartmentId":498,"type":27,"ocsExpertMinLevelName":"少年英雄","parentId":2323065,"bindingCpCode":"cnupp","fullCode":"CN_CBOSS~youjia~498~","ocsExpertMinLevelId":703006,"firstLevelDepartmentId":3561,"orgCode":"498","name":"菜鸟优加外呼业务","ocsNomalMinLevelId":703001,"ocsRestRate":20,"enterpriseId":16,"bindingEnterpriseId":274},"cooperateType":0,"creatorDomain":83,"creatorId":2207369826315,"departmentId":3571,"departmentIdPath":"1>3561>3571","departmentName":"菜鸟优加外呼业务","firstLevelDepartmentId":3561,"level":2,"modifierDomain":83,"modifierId":2207369826315,"parentDepartmentId":3561,"status":1,"tenantCode":"CAINIAO"},"departmentId":3571,"employeeId":2208144684830,"gmtCreate":1590559202000,"gmtModified":1595821551000,"name":"疏爽测试","servicerId":21974,"status":1,"tenantCode":"CAINIAO"}
14:33:09 [http-nio-7001-exec-4] ERROR c.c.g.c.c.b.m.s.i.SopPlayManagerImpl-|2020-07-27 14:33:09|failed to invoke sopMatchService.metchSopApplyWithApplyRule;caseId=1011801737002,taskId=1002350116105,errMsg=metchSopApplyWithApplyRule is Exception...reason:can not inference sop


caseId=1021836556028&taskId=1002377186016
caseId=1021836556028&taskId=1002377186016


1011801737002&taskId=1002350116105

https://pre-desk.cainiao.com/unified/ticketManage/ticketDetail?caseId=1021836556039&taskId=1002377093026



```json
{
  "success": true,
  "data": {
    "sessionId": "737440562810064896-1002379750204-10002",
    "coreBizOrderNo": "1002379750204",
    "bizOrderDomain": 10002,
    "end": false,
    "sopId": 870,
    "sopName": "敬方测试",
    "nodeList": [
      {
        "nodeTag": "factor_2nqh7a49yuc0000",
        "nodeName": "逸姿美不美",
        "nodeType": "simple_interactive_factor",
        "solutionType": null,
        "processKey": "1595818119178_a175",
        "inputParamMap": {},
        "outputParamMap": {
          "result": {
            "branchResult": "Default"
          },
          "s": {
            "result": {
              "branchResult": "Default"
            }
          },
          "nodeResult": [
            {
              "id": 6920,
              "factorId": 864,
              "attributeValue": "不美",
              "attributeName": "不美",
              "attributeType": 1,
              "priority": 2,
              "status": 1,
              "tenantId": 1,
              "gmtModified": "2019-09-18T08:31:18.000+0000",
              "gmtCreate": "2019-09-18T08:31:18.000+0000",
              "creatorId": 8131,
              "creatorDomain": 82,
              "modifierId": 8131,
              "modifierDomain": 82
            },
            {
              "id": 6919,
              "factorId": 864,
              "attributeValue": "m",
              "attributeName": "美",
              "attributeType": 1,
              "priority": 1,
              "status": 1,
              "tenantId": 1,
              "gmtModified": "2019-09-18T08:31:05.000+0000",
              "gmtCreate": "2019-09-18T08:31:05.000+0000",
              "creatorId": 8131,
              "creatorDomain": 82,
              "modifierId": 8131,
              "modifierDomain": 82
            }
          ]
        },
        "display": true,
        "nodeTip": null,
        "status": "WAIT",
        "nodeData": [
          {
            "id": 6920,
            "factorId": 864,
            "attributeValue": "不美",
            "attributeName": "不美",
            "attributeType": 1,
            "priority": 2,
            "status": 1,
            "tenantId": 1,
            "gmtModified": "2019-09-18T08:31:18.000+0000",
            "gmtCreate": "2019-09-18T08:31:18.000+0000",
            "creatorId": 8131,
            "creatorDomain": 82,
            "modifierId": 8131,
            "modifierDomain": 82
          },
          {
            "id": 6919,
            "factorId": 864,
            "attributeValue": "m",
            "attributeName": "美",
            "attributeType": 1,
            "priority": 1,
            "status": 1,
            "tenantId": 1,
            "gmtModified": "2019-09-18T08:31:05.000+0000",
            "gmtCreate": "2019-09-18T08:31:05.000+0000",
            "creatorId": 8131,
            "creatorDomain": 82,
            "modifierId": 8131,
            "modifierDomain": 82
          }
        ],
        "extFields": {}
      }
    ],
    "parameters": {
      "lpCode": "LP00166142746111",
      "gaeaBizOrderCode": "LBX0329964253276237",
      "lbxCode": "LBX0329964253276237",
      "gaeaCaseId": 1021837717650
    },
    "resumedPlay": false
  },
  "errorCode": null,
  "errorMsg": null
}
```

测试订单号：

https://desk.cainiao.com/unified/ticketManage/ticketDetail?caseId=1021837717650&taskId=1002379750204

```json
{
  "success": true,
  "data": {
    "sessionId": "737440562810064896-1002379750204-10002",
    "coreBizOrderNo": "1002379750204",
    "bizOrderDomain": 10002,
    "end": false,
    "sopId": 870,
    "sopName": "敬方测试",
    "nodeList": [
      {
        "nodeTag": "factor_63te85hwvuw0000",
        "nodeName": "订单是否取消",
        "nodeType": "simple_interactive_factor",
        "solutionType": null,
        "processKey": "1595818119178_a175",
        "inputParamMap": {},
        "outputParamMap": {
          "result": {
            "branchResult": "Default"
          },
          "s": {
            "result": {
              "branchResult": "Default"
            }
          },
          "nodeResult": [
            {
              "id": 14868,
              "factorId": 2725,
              "attributeValue": "no",
              "attributeName": "否",
              "attributeType": 1,
              "priority": 2,
              "status": 1,
              "tenantId": 1,
              "gmtModified": "2020-07-21T03:08:13.000+0000",
              "gmtCreate": "2020-07-21T03:08:13.000+0000",
              "creatorId": 771261,
              "creatorDomain": 82,
              "modifierId": 771261,
              "modifierDomain": 82
            },
            {
              "id": 14867,
              "factorId": 2725,
              "attributeValue": "yes",
              "attributeName": "是",
              "attributeType": 1,
              "priority": 1,
              "status": 1,
              "tenantId": 1,
              "gmtModified": "2020-07-21T03:07:58.000+0000",
              "gmtCreate": "2020-07-21T03:07:58.000+0000",
              "creatorId": 771261,
              "creatorDomain": 82,
              "modifierId": 771261,
              "modifierDomain": 82
            }
          ]
        },
        "display": true,
        "nodeTip": null,
        "status": "WAIT",
        "nodeData": [
          {
            "id": 14868,
            "factorId": 2725,
            "attributeValue": "no",
            "attributeName": "否",
            "attributeType": 1,
            "priority": 2,
            "status": 1,
            "tenantId": 1,
            "gmtModified": "2020-07-21T03:08:13.000+0000",
            "gmtCreate": "2020-07-21T03:08:13.000+0000",
            "creatorId": 771261,
            "creatorDomain": 82,
            "modifierId": 771261,
            "modifierDomain": 82
          },
          {
            "id": 14867,
            "factorId": 2725,
            "attributeValue": "yes",
            "attributeName": "是",
            "attributeType": 1,
            "priority": 1,
            "status": 1,
            "tenantId": 1,
            "gmtModified": "2020-07-21T03:07:58.000+0000",
            "gmtCreate": "2020-07-21T03:07:58.000+0000",
            "creatorId": 771261,
            "creatorDomain": 82,
            "modifierId": 771261,
            "modifierDomain": 82
          }
        ],
        "extFields": {}
      }
    ],
    "parameters": {
      "lpCode": "LP00166142746111",
      "gaeaBizOrderCode": "LBX0329964253276237",
      "lbxCode": "LBX0329964253276237",
      "gaeaCaseId": 1021837717650
    },
    "resumedPlay": false
  },
  "errorCode": null,
  "errorMsg": null
}
```

drive 驱动节点;主要请求

```json
{
  "bpmKey": "1595818119178_a175",
  "interactiveNodeDTO": {
    "attributeMap": {
      "attributeCode": "yes"
    }
  },
  "nodeTag": "factor_63te85hwvuw0000",
  "parameters": {
       "lpCode": "LP00166142746111",
      "gaeaBizOrderCode": "LBX0329964253276237",
      "lbxCode": "LBX0329964253276237",
      "gaeaCaseId": 1021837717650
   },
  "sessionId": "737440562810064896-1002379750204-10002"
}
```

返回值:
```json
{
  "success": true,
  "data": {
    "sessionId": "737440562810064896-1002379750204-10002",
    "coreBizOrderNo": "1002379750204",
    "bizOrderDomain": 10002,
    "end": false,
    "sopId": null,
    "sopName": null,
    "nodeList": [
      {
        "nodeTag": "factor_63te85hwvuw0000",
        "nodeName": "订单是否取消",
        "nodeType": "simple_interactive_factor",
        "solutionType": null,
        "processKey": "1595818119178_a175",
        "inputParamMap": {
          "e": {
            "nodeResult": "yes"
          },
          "nodeResult": "yes",
          "attributeMap": {
            "attributeCode": "yes"
          }
        },
        "outputParamMap": {},
        "display": true,
        "nodeTip": null,
        "status": "COMPLETED",
        "nodeData": null,
        "extFields": {}
      },
      {
        "nodeTag": "action_dyuwkabc9k00000_kd5nw7pl",
        "nodeName": "动作",
        "nodeType": "action",
        "solutionType": null,
        "processKey": "1595818119178_a175",
        "inputParamMap": {},
        "outputParamMap": {
          "result": {
            "branchResult": "Default"
          },
          "e": {
            "result": {
              "branchResult": "Default"
            }
          },
          "nodeResult": [
            "1"
          ]
        },
        "display": true,
        "nodeTip": null,
        "status": "COMPLETED",
        "nodeData": [
          {
            "id": 1,
            "name": "外呼",
            "code": "call",
            "gmtCreate": "2019-06-19T03:26:42.000+0000",
            "gmtModified": "2019-06-19T03:26:42.000+0000",
            "desc": "外呼",
            "actionCategory": 1,
            "responseCategory": 1,
            "preUrl": "https://mtop.taobao.com",
            "onlineUrl": "https://mtop.taobao.com",
            "feature": "testtest",
            "creatorId": 107345,
            "creatorDomain": 82,
            "modifierId": 107345,
            "modifierDomain": 82,
            "tenantCode": "1"
          }
        ],
        "extFields": {}
      },
      {
        "nodeTag": "factor_3jt4jac8xd80000",
        "nodeName": "判断订单取消的原因和角色",
        "nodeType": "simple_interactive_factor",
        "solutionType": null,
        "processKey": "1595818119178_a175",
        "inputParamMap": {},
        "outputParamMap": {
          "result": {
            "branchResult": "Default"
          },
          "s": {
            "result": {
              "branchResult": "Default"
            }
          },
          "nodeResult": [
            {
              "id": 10234,
              "factorId": 1747,
              "attributeValue": "xjyqx",
              "attributeName": "小件员取消",
              "attributeType": 1,
              "priority": 4,
              "status": 1,
              "tenantId": 1,
              "gmtModified": "2020-02-21T06:06:25.000+0000",
              "gmtCreate": "2020-02-21T06:06:25.000+0000",
              "creatorId": 771261,
              "creatorDomain": 82,
              "modifierId": 771261,
              "modifierDomain": 82
            },
            {
              "id": 10233,
              "factorId": 1747,
              "attributeValue": "yhqx",
              "attributeName": "用户取消",
              "attributeType": 1,
              "priority": 3,
              "status": 1,
              "tenantId": 1,
              "gmtModified": "2020-02-21T06:06:14.000+0000",
              "gmtCreate": "2020-02-21T06:06:14.000+0000",
              "creatorId": 771261,
              "creatorDomain": 82,
              "modifierId": 771261,
              "modifierDomain": 82
            },
            {
              "id": 10232,
              "factorId": 1747,
              "attributeValue": "xeqxfcq",
              "attributeName": "小二取消且取消原因为非超区",
              "attributeType": 1,
              "priority": 2,
              "status": 1,
              "tenantId": 1,
              "gmtModified": "2020-02-21T06:05:59.000+0000",
              "gmtCreate": "2020-02-21T06:05:59.000+0000",
              "creatorId": 771261,
              "creatorDomain": 82,
              "modifierId": 771261,
              "modifierDomain": 82
            },
            {
              "id": 10231,
              "factorId": 1747,
              "attributeValue": "qvyywcq",
              "attributeName": "取消原因为超区",
              "attributeType": 1,
              "priority": 1,
              "status": 1,
              "tenantId": 1,
              "gmtModified": "2020-02-21T06:05:26.000+0000",
              "gmtCreate": "2020-02-21T06:05:26.000+0000",
              "creatorId": 771261,
              "creatorDomain": 82,
              "modifierId": 771261,
              "modifierDomain": 82
            }
          ]
        },
        "display": true,
        "nodeTip": null,
        "status": "WAIT",
        "nodeData": [
          {
            "id": 10234,
            "factorId": 1747,
            "attributeValue": "xjyqx",
            "attributeName": "小件员取消",
            "attributeType": 1,
            "priority": 4,
            "status": 1,
            "tenantId": 1,
            "gmtModified": "2020-02-21T06:06:25.000+0000",
            "gmtCreate": "2020-02-21T06:06:25.000+0000",
            "creatorId": 771261,
            "creatorDomain": 82,
            "modifierId": 771261,
            "modifierDomain": 82
          },
          {
            "id": 10233,
            "factorId": 1747,
            "attributeValue": "yhqx",
            "attributeName": "用户取消",
            "attributeType": 1,
            "priority": 3,
            "status": 1,
            "tenantId": 1,
            "gmtModified": "2020-02-21T06:06:14.000+0000",
            "gmtCreate": "2020-02-21T06:06:14.000+0000",
            "creatorId": 771261,
            "creatorDomain": 82,
            "modifierId": 771261,
            "modifierDomain": 82
          },
          {
            "id": 10232,
            "factorId": 1747,
            "attributeValue": "xeqxfcq",
            "attributeName": "小二取消且取消原因为非超区",
            "attributeType": 1,
            "priority": 2,
            "status": 1,
            "tenantId": 1,
            "gmtModified": "2020-02-21T06:05:59.000+0000",
            "gmtCreate": "2020-02-21T06:05:59.000+0000",
            "creatorId": 771261,
            "creatorDomain": 82,
            "modifierId": 771261,
            "modifierDomain": 82
          },
          {
            "id": 10231,
            "factorId": 1747,
            "attributeValue": "qvyywcq",
            "attributeName": "取消原因为超区",
            "attributeType": 1,
            "priority": 1,
            "status": 1,
            "tenantId": 1,
            "gmtModified": "2020-02-21T06:05:26.000+0000",
            "gmtCreate": "2020-02-21T06:05:26.000+0000",
            "creatorId": 771261,
            "creatorDomain": 82,
            "modifierId": 771261,
            "modifierDomain": 82
          }
        ],
        "extFields": {}
      }
    ],
    "parameters": {
      "lpCode": "LP00166142746111",
      "gaeaBizOrderCode": "LBX0329964253276237",
      "lbxCode": "LBX0329964253276237",
      "gaeaCaseId": 1021837717650
    },
    "resumedPlay": null
  },
  "errorCode": null,
  "errorMsg": null
}
```




```json
{
  "notSuccess": false,
  "data": {
    "gmtModified": 1594136707000,
    "bizStatusDesc": "处理中",
    "typeDesc": "咨询工单",
    "customerDomain": 81,
    "entryDesc": "在线智能卡片",
    "creatorId": 2692057536,
    "channel": 2,
    "creatorDomain": 81,
    "memo": "裹裹-寄件-快递员什么时候来取件：小件员超时未揽收，客户催促上门取件",
    "type": 2,
    "flowInstanceId": "24218118688168",
    "entryId": 46,
    "bizLineId": 60,
    "bizOrderDomainDesc": "订单中心",
    "feature": {
      "serviceItemId": "5000000011417",
      "lastUrgeDay": "2020-06-18",
      "urgeTime": "1",
      "showSLATime": "false",
      "orderFeeCurrency": "CNY",
      "serviceDefineId": "4314",
      "systemMessageMailNoList": "LP00183127933252",
      "caseSLADate": "2020-06-22 17:09:24",
      "deliverGoodsInfo": "520112",
      "bpmFlowTemplateCode": "consult-collocation-callTransportDoubleCheck-cn-deal",
      "bizlineRuleId": "1542",
      "orderFee": "0",
      "buyerId": "2692057536",
      "repeatCreateStrategy": "shareCreateTimesStrategy",
      "currentConsultFinishMailnos": "LP00183127933252",
      "mailNo": "4306372898240",
      "divisionInfo": "441323",
      "realDeliveryResourceId": "102",
      "guoguoNetworkId": "1262780",
      "tag": "96,102,12,61",
      "distributeRuleSetId": "3240",
      "buyerDomain": "81",
      "tradeId": "183127933252"
    },
    "customerId": 2692057536,
    "bizOrderDomain": 10000,/*  */
    "id": 1021694429990,
    "customerRoleDesc": "买家",
    "class": "com.cainiao.gaea.application.client.domain.CaseBaseInfo",
    "bizStatus": 2,
    "customerRole": 1,
    "creatorDomainDesc": "阿里用户帐号中心",
    "creatorRole": 1,
    "statusDesc": "工单处理中",
    "bizOrderCode": "LP00183127933252",/*   */
    "gmtCreate": 1592471364000,
    "creatorRoleDesc": "买家",
    "customerDomainDesc": "阿里用户帐号中心",
    "tenantId": 1,
    "channelDesc": "在线",
    "categoryId": 1000000513,
    "status": 2
  },
  "responseTime": 1596006230950,
  "success": true,
  "interruptChain": true,
  "class": "com.cainiao.gaea.application.client.domain.base.Result"
}
```


```json
com.cainiao.gaea.platform.spi.component.bizorder.BizOrderReadService:1.0.0
getBizOrder

返回信息


{
  "notSuccess": false,
  "data": {
    "gmtModified": 1595851995000,
    "bizStatusDesc": "处理中",
    "typeDesc": "咨询工单",
    "customerDomain": 81,
    "entryDesc": "其他入口",
    "creatorId": 2208144684830,
    "channel": 1,
    "creatorDomain": 83,
    "memo": "sop迁移测试",
    "type": 2,
    "flowInstanceId": "28070598588110",
    "entryId": 22,
    "bizLineId": 24,
    "bizOrderDomainDesc": "仓储中心",
    "feature": {
      "endResource": "1216000000178348",
      "orderFeeCurrency": "CNY",
      "shopOwnerId": "2672115996",
      "caseSLADate": "2020-08-05 12:13:15",
      "shopOwnerDomain": "81",
      "deliverGoodsInfo": "320102",
      "orderFee": "0",
      "realStoregeResourceId": "5000000100570",
      "buyerId": "36319222",
      "mailNo": "TEST7070017525",
      "sellerId": "2672115996",
      "tag": "61,92,112,201,221,228,247,251",
      "distributeRuleSetId": "4137",
      "serviceItemId": "5000000000059",
      "showSLATime": "false",
      "serviceDefineId": "128750",
      "bpmFlowTemplateCode": "complain_uplus_judge_muting",
      "bizlineRuleId": "2818",
      "repeatCreateStrategy": "",
      "firstResource": "5000000100570",
      "divisionInfo": "542524",
      "realDeliveryResourceId": "1216000000178348",
      "sellerDomain": "81",
      "planStoregeResourceId": "5000000100570",
      "buyerDomain": "81",
      "planDeliveryResourceId": "1216000000178348",
      "tradeId": "IWANT39034067286190"
    },
    "customerId": 2672115996,
    "bizOrderDomain": 10001,/*  */
    "id": 1021837717650,
    "customerRoleDesc": "商家",
    "class": "com.cainiao.gaea.application.client.domain.CaseBaseInfo",
    "bizStatus": 2,
    "customerRole": 2,
    "creatorDomainDesc": "菜鸟账号中心",
    "creatorRole": 9,
    "statusDesc": "工单处理中",
    "bizOrderCode": "LBX0329964253276237",/*  */
    "gmtCreate": 1595851995000,
    "creatorRoleDesc": "云小二",
    "customerDomainDesc": "阿里用户帐号中心",
    "tenantId": 1,
    "channelDesc": "自助",
    "categoryId": 1000011632,
    "status": 2
  },
  "responseTime": 1596006306887,
  "success": true,
  "interruptChain": true,
  "class": "com.cainiao.gaea.application.client.domain.base.Result"
}
```

```json

{
  "notSuccess": false,
  "data": {
    "cargosTotalPrice": 0,
    "perSellTag": 0,
    "orderType": 100201,
    "allowSellDesc": "",
    "tmallSeaDistributeMode": 2,
    "orderStatusDesc": "签收成功 ",
    "isInsurance": false,
    "ouCode": "504",
    "orderStatus": 200,
    "cargos": [
      {
        "owner": {
          "name": "菜鸟测试帐号1002",
          "class": "com.cainiao.gaea.platform.spi.component.bizorder.domain.bean.User",
          "userId": 2672115996
        },
        "item": {
          "serviceItemId": 5000000000059,
          "unitPrice": 0,
          "amount": 67239,
          "quantity": 1,
          "precious": 0,
          "goodsId": 591113648160,
          "planQuantity": 1,
          "picUrl": "https://img.alicdn.com/bao/uploaded/i2/1659297059/TB1vzAcxhSYBuNjSspjXXX73VXa_!!2-item_pic.png",
          "name": "测试商品勿拍0",
          "currency": "CNY",
          "id": 591113648160,
          "class": "com.cainiao.gaea.platform.spi.component.bizorder.domain.bean.Item",
          "disclaimer": 0
        },
        "class": "com.cainiao.gaea.platform.spi.component.bizorder.domain.bean.Cargo",
        "subTradeId": "83819171477528"
      }
    ],
    "isFpTime": 0,
    "sellOffTagDesc": "",
    "bizOrderPlatformTypeId": 10001,
    "jidanTag": 0,
    "sinkStat": "",
    "consignorAddress": {
      "province": {
        "name": "江苏省",
        "id": 320000,
        "class": "com.cainiao.gaea.platform.spi.component.bizorder.domain.bean.Province"
      },
      "city": {
        "name": "南京市",
        "id": 320100,
        "class": "com.cainiao.gaea.platform.spi.component.bizorder.domain.bean.City"
      },
      "street": {
        "name": "东南大学四牌楼校区",
        "class": "com.cainiao.gaea.platform.spi.component.bizorder.domain.bean.Street"
      },
      "district": {
        "name": "玄武区",
        "id": 320102,
        "class": "com.cainiao.gaea.platform.spi.component.bizorder.domain.bean.District"
      },
      "fullAddress": "江苏省南京市玄武区东南大学四牌楼校区",
      "class": "com.cainiao.gaea.platform.spi.component.bizorder.domain.bean.Address"
    },
    "lpOrderCreated": 1580978457000,
    "userIdPriceId": 3721,
    "deliveryStoreCodeWhiteList": [
      162
    ],
    "userIdFromLocWhiteList": [
      102
    ],
    "hasWmsArrive": 0,
    "class": "com.cainiao.gaea.platform.spi.component.bizorder.domain.WarehouseCenterOrder",
    "consignor": {
      "contact": {
        "mobilePhone": "13966666666",
        "telephone": "057188888888",
        "class": "com.cainiao.gaea.platform.spi.component.bizorder.domain.bean.Contact"
      },
      "name": "马七-仓经理",
      "class": "com.cainiao.gaea.platform.spi.component.bizorder.domain.bean.User"
    },
    "consigneeAddress": {
      "province": {
        "name": "西藏自治区",
        "id": 540000,
        "class": "com.cainiao.gaea.platform.spi.component.bizorder.domain.bean.Province"
      },
      "city": {
        "name": "阿里地区",
        "id": 542500,
        "class": "com.cainiao.gaea.platform.spi.component.bizorder.domain.bean.City"
      },
      "street": {
        "name": "西溪首座B1-2小邮局",
        "class": "com.cainiao.gaea.platform.spi.component.bizorder.domain.bean.Street"
      },
      "district": {
        "name": "日土县",
        "id": 542524,
        "class": "com.cainiao.gaea.platform.spi.component.bizorder.domain.bean.District"
      },
      "fullAddress": "西藏自治区阿里地区日土县西溪首座B1-2小邮局",
      "class": "com.cainiao.gaea.platform.spi.component.bizorder.domain.bean.Address"
    },
    "mailNoList": [
      "TEST7070017525"
    ],
    "forwardLbxOrderCreateTime": 1580978460000,
    "ext": {
      "class": "com.cainiao.gaea.platform.bizorder.ext.WareHouseCenterOrderExt",
      "locOrder": {
        "orderDO": {
          "statusDate": 1580978461000,
          "orderType": 1,
          "featureMap": {
            "orderType": "0",
            "shipper": "cn",
            "inv_order_type": "201",
            "bizType": "200",
            "dispatch": "1650685505",
            "city": "542500",
            "notifyERP": "30",
            "cc_platform_source": "bms",
            "ct_sub": "302",
            "ssignTime": "2020/02/11",
            "outUId": "IWANT39034067286190Mock4178503922",
            "province": "540000",
            "www": "0",
            "providerId": "2672115996",
            "cc_spcode": "5000000000059",
            "appKey": "SCM",
            "order_fulfil_split": "31",
            "goodsNum": "1",
            "dId": "542524100",
            "logistics_accept_finish": "1",
            "bmsOrder": "1",
            "serviceTags": "5000000000059-502$cutoffMinutes@1080&cutTime@18&mcTime@1439&capPeriod@1080&lgType@10&storeCode@STB&%",
            "tms_accept_time": "2020-02-06 16:45:41",
            "town": "542524100",
            "obtime": "2020-02-06 16:45:29",
            "tpc": "2020-02-06 16:40:57",
            "preference": "6600",
            "ip": "127.0.0.1",
            "postFee": "500",
            "ct": "3",
            "buyer_nick": "iwant-fulfil",
            "invMode": "1",
            "bizSource": "213",
            "district": "542524",
            "fulfilCode": "101",
            "seller_nick": "菜鸟测试帐号1002",
            "TC_CT": "1",
            "LG_CN": "1"
          },
          "timeoutStatus": 0,
          "logisType": 5,
          "orderId": 166142746111,
          "receiverMobilePhone": "13588888888",
          "orderSendToFlag": "",
          "receiverProvince": "西藏自治区",
          "receiverCity": "阿里地区",
          "receiverCountryId": 542524,
          "taobaoTradeId": 69584475470001,
          "features": ";orderType=0;shipper=cn;inv_order_type=201;bizType=200;dispatch=1650685505;city=542500;notifyERP=30;cc_platform_source=bms;ct_sub=302;ssignTime=2020/02/11;outUId=IWANT39034067286190Mock4178503922;province=540000;www=0;providerId=2672115996;cc_spcode=5000000000059;appKey=SCM;order_fulfil_split=31;goodsNum=1;dId=542524100;logistics_accept_finish=1;bmsOrder=1;serviceTags=5000000000059-502$cutoffMinutes@1080&cutTime@18&mcTime@1439&capPeriod@1080&lgType@10&storeCode@STB&%;tms_accept_time=2020-02-06 16:45:41;town=542524100;obtime=2020-02-06 16:45:29;tpc=2020-02-06 16:40:57;preference=6600;ip=127.0.0.1;postFee=500;ct=3;buyer_nick=iwant-fulfil;invMode=1;bizSource=213;district=542524;fulfilCode=101;seller_nick=菜鸟测试帐号1002;TC_CT=1;LG_CN=1;",
          "fetchBegin": "",
          "statusReason": "UOP监听履行单状态变化",
          "split": false,
          "standardStatus": 2000,
          "merge": false,
          "options": 2199025352704,
          "subServiceTypes": [],
          "companyCode": "DISTRIBUTOR_30096693",
          "syncVersion": 19,
          "fetcherId": 216388800122,
          "buyServiceFeeLong": 0,
          "receiverDDDAdr": "西藏自治区阿里地区日土县 日土镇   西溪首座B1-2小邮局",
          "sellerSendConfirm": 0,
          "bizOrderStatus": 200,
          "spiltMerge": false,
          "codSplitFeeDouble": 0,
          "receiverAddress": "西藏自治区^^^阿里地区^^^日土县^^^ 日土镇   西溪首座B1-2小邮局",
          "codSplitFeeLong": 0,
          "totalServiceFee": 0,
          "receiverDivisionId": 542524100,
          "companyId": 1216000000178348,
          "receiverWangWangId": "眼前不苟且",
          "feature2Map": {
            "ATT_EXT_WMS_CODE": "STB",
            "sdst": "1581390180000",
            "operateType": "1",
            "cdc_post_service": "cc",
            "buyerId": "36319222",
            "invGray": "1",
            "srt": "1581397200000",
            "sst": "1581422400000",
            "orderSourceType": "tb",
            "extOrderSource": "1",
            "order_amount": "8800",
            "cdc_gray_flag": "true",
            "sot": "1581393600000",
            "dis_amount": "4400",
            "outTasks": "{\"msg\":true,\"package\":true,\"inventory\":false}",
            "wlb_order_code": "LBX0329964253276237"
          },
          "itemsValue": 42556,
          "itemsValueDouble": 425.56,
          "provAndCity": "阿里地区",
          "outBizId": "IWANT39034067286190",
          "syncUpdate": false,
          "gmtModified": 1581007740000,
          "orderStatus": 100,
          "mailNo": "TEST7070017525",
          "preOrderId": 0,
          "buyServiceFeeDouble": 0,
          "receiverDistrict": "日土县",
          "orderTypeStr": "1",
          "parcelOrder": false,
          "class": "com.taobao.logistics.domain.dataobject.OrderDO",
          "consigneeId": 36319222,
          "outBizSource": "213",
          "sourceOrder": false,
          "orderSource": 3,
          "fetchEnd": "",
          "receiverContact": {
            "zipCode": "210012",
            "country": "中国",
            "address": "西藏自治区^^^阿里地区^^^日土县^^^ 日土镇   西溪首座B1-2小邮局",
            "districtName": "日土县",
            "ddd": "0571",
            "contactName": "iWant",
            "dDDAdr": "西藏自治区阿里地区日土县 日土镇   西溪首座B1-2小邮局",
            "telephone": "0571-88888888",
            "cityAndDistrict": "阿里地区,日土县",
            "countryId": 542524,
            "wangwangId": "眼前不苟且",
            "adr": "日土镇   西溪首座B1-2小邮局",
            "features": ";dId=542524100;",
            "sourceDivisionId": 542524100,
            "cityName": "阿里地区",
            "mobilePhone": "13588888888",
            "phone": "88888888",
            "detailAddress": "西溪首座B1-2小邮局",
            "divisionId": 542524100,
            "provinceName": "西藏自治区",
            "class": "com.taobao.logistics.domain.dataobject.ContactDO",
            "sourceDetailAddress": "西溪首座B1-2小邮局"
          },
          "receiverName": "iWant",
          "goodsValueLong": 42556,
          "receiverDetailAddress": "西溪首座B1-2小邮局",
          "gmtCreate": 1580978457000,
          "logisticsService": "5000000000059-502$cutoffMinutes@1080&cutTime@18&mcTime@1439&capPeriod@1080&lgType@10&storeCode@STB&%",
          "userId": 2672115996,
          "receiverAdr": " 日土镇   西溪首座B1-2小邮局",
          "receiverTelephone": "0571-88888888",
          "mailType": 200,
          "receiverCompanyName": "诗和远方",
          "receiverZipCode": "210012",
          "fetchDateStr": "",
          "goodsTypeId": 0,
          "goodsValueDouble": 425.56,
          "closed": false,
          "features2": ";ATT_EXT_WMS_CODE=STB;sdst=1581390180000;operateType=1;cdc_post_service=cc;buyerId=36319222;invGray=1;srt=1581397200000;sst=1581422400000;orderSourceType=tb;extOrderSource=1;order_amount=8800;cdc_gray_flag=true;sot=1581393600000;dis_amount=4400;outTasks={\"msg\":true,\"package\":true,\"inventory\":false};wlb_order_code=LBX0329964253276237;",
          "orderCode": "LP00166142746111",
          "itemValueDouble": 425.56,
          "cnService": "cdc$cutoffMinutes@1080&cutTime@18&capPeriod@1080&mcTime@1080&capDate@2020-02-06&ssid@5000000000059&ebs@502-2010001#;storeCode=STB;&&logisPattern@std&isMock@true&",
          "refundId": 216388800122,
          "receiverEmail": "iwant@cainiao.com",
          "tradeId": "40989503155562"
        },
        "orderGoods": [
          {
            "orderGoodsId": 228059428120,
            "subsGood": false,
            "gmtModified": 1580982971000,
            "featureMap": {
              "goodsIndex": "0",
              "is_fragile": "0",
              "cg": "1",
              "itemTag": "8833,513,2049,3073,18049,19457,19585,22145,28417,30977,34305,35713,37633,51329,51585,258,2178,2562,6146,28802,59010,101762,113666,3974,4614,7046,1675,2059,651,1163,1547,2443,2699,4491,7947,13707,36161,20545,28353,34369,37569,37697,39233,40897",
              "scItemCode": "591113648160",
              "tcSubOrderId": "77454151803399",
              "actualValue": "623",
              "handlePcNO": "1",
              "is_dangerous": "0",
              "scItemId": "591113648160",
              "type": "0",
              "inventory": "1",
              "realRootCat": "11566541",
              "itemId": "591113648160",
              "wareH": "STB",
              "goodsprice": "67239",
              "scItemName": "测试商品勿拍0",
              "www": "0",
              "providerId": "2672115996",
              "providerNick": "测试卖家0",
              "goods_value": "529",
              "categoryId": "84073446",
              "skuId": "591113648160"
            },
            "orderId": 166142746111,
            "itemValue": 0,
            "feature2": "",
            "feature": ";goodsIndex=0;is_fragile=0;cg=1;itemTag=8833,513,2049,3073,18049,19457,19585,22145,28417,30977,34305,35713,37633,51329,51585,258,2178,2562,6146,28802,59010,101762,113666,3974,4614,7046,1675,2059,651,1163,1547,2443,2699,4491,7947,13707,36161,20545,28353,34369,37569,37697,39233,40897;scItemCode=591113648160;tcSubOrderId=77454151803399;actualValue=623;handlePcNO=1;is_dangerous=0;scItemId=591113648160;type=0;inventory=1;realRootCat=11566541;itemId=591113648160;wareH=STB;goodsprice=67239;scItemName=测试商品勿拍0;www=0;providerId=2672115996;providerNick=测试卖家0;goods_value=529;categoryId=84073446;skuId=591113648160;",
            "hasSendTOTO": false,
            "sellerPropertyMap": {},
            "goodsQuantity": 1,
            "class": "com.taobao.logistics.domain.dataobject.OrderGoodsDO",
            "goodsName": "测试商品, 勿拍0",
            "itemValueString": "0.00",
            "syncVersion": 4,
            "bundGood": false,
            "bundMain": false,
            "canSplit": false,
            "splitNum": false,
            "gmtCreate": 1580978457000,
            "logisticsService": "",
            "userId": 2672115996,
            "goodsPicId": "i2/1659297059/TB1vzAcxhSYBuNjSspjXXX73VXa_!!2-item_pic.png",
            "virtualGood": false,
            "bundAttach": false,
            "feature2Map": {},
            "cantSplit": false,
            "itemValueDouble": 0,
            "leafCategoryId": 0,
            "categoryId": 84073446,
            "tradeId": 83819171477528
          }
        ],
        "class": "com.taobao.loc.common.domain.LocOrderDO"
      }
    },
    "masterTradeId": "IWANT39034067286190",
    "orderCreated": 1580978460000,
    "consignee": {
      "contact": {
        "mobilePhone": "13588888888",
        "telephone": "0571-88888888",
        "class": "com.cainiao.gaea.platform.spi.component.bizorder.domain.bean.Contact"
      },
      "name": "iWant",
      "class": "com.cainiao.gaea.platform.spi.component.bizorder.domain.bean.User"
    },
    "business": {
      "nickName": "菜鸟测试帐号1002",
      "contact": {
        "mobilePhone": "13071820189",
        "class": "com.cainiao.gaea.platform.spi.component.bizorder.domain.bean.Contact",
        "email": "cainiao17051052976@aliyun.com"
      },
      "name": "菜鸟测试账号1002",
      "shopName": "菜鸟测试帐号1002",
      "shopId": 2672115996,
      "class": "com.cainiao.gaea.platform.spi.component.bizorder.domain.bean.Shop",
      "userId": 2672115996
    },
    "deliveryStoreCode": "STB",
    "vipTag": 0,
    "userIdWhiteList": [
      102
    ],
    "userId": 2672115996,
    "hadHomeGot": 0,
    "userIdFromLoc": 2672115996,
    "isEsTimeExist": false,
    "lpCompanyCode": "DISTRIBUTOR_30096693",
    "orderFlag": 8589967360,
    "whcOrderBaseDTO": {
      "orderType": 1,
      "senderAddress": "东南大学四牌楼校区",
      "receiverWangwang": "眼前不苟且",
      "buyerNick": "眼前不苟且",
      "receiverProvince": "西藏自治区",
      "buyerId": "36319222",
      "receiverCity": "阿里地区",
      "senderArea": "玄武区",
      "senderName": "马七-仓经理",
      "receiverPhone": "0571-88888888",
      "orderSourceCode": "LP00166142746111",
      "erpOrderCode": "IWANT39034067286190",
      "id": 29964253276237,
      "confirmType": 0,
      "scheduleSpeed": 101,
      "operateType": 1,
      "senderZipCode": "2221111",
      "version": -1,
      "senderCity": "南京市",
      "senderPhone": "057188888888",
      "receiverAddress": "西溪首座B1-2小邮局",
      "totalAmount": 42556,
      "receiverDivisionId": 542524100,
      "tmsTpCode": "DISTRIBUTOR_30096693",
      "senderDivisionId": 320102003,
      "storeCode": "STB",
      "orderSubType": 201,
      "gmtModified": 1588819349000,
      "wmsOrderType": 201,
      "orderStatus": 200,
      "invoinceInfoList": [],
      "userNick": "菜鸟测试帐号1002",
      "class": "com.taobao.warehouse.common.dto.WhcOrderBaseDTO",
      "isCompleted": true,
      "orderSource": 14,
      "receiverName": "iWant",
      "receiverMobile": "13588888888",
      "senderMobile": "13966666666",
      "senderProvince": "江苏省",
      "receiverArea": "日土县",
      "privateProperties": {
        "issRtId": "cr-29964253276237",
        "bizCode": "5000000000059",
        "callback_confirm_time": "20200206164529",
        "buyerId": "36319222",
        "bizLine": "1",
        "biz_order_code": "IWANT39034067286190",
        "oicMode": "1",
        "order_amount": "8800",
        "serviceId": "502",
        "WEIGHT": "3000",
        "issInsId": "cr-310141694011",
        "serviceItemId": "5000000000059",
        "receiver_town": "日土镇",
        "tms_sv_name": "urc创建配送anjun",
        "orderConfirmTime": "20200206164529",
        "orderPackage": "true",
        "send_divisionId": "320102003",
        "ffService": "bs$2010001",
        "is_top": "false",
        "orderSourceType": "tb",
        "dis_amount": "4400",
        "fcsCreate": "1",
        "receive_divisionId": "542524100",
        "sender_town": "新街口街道",
        "consignMode": "1"
      },
      "gmtCreate": 1580978460000,
      "userId": 2672115996,
      "senderStreet": "新街口街道",
      "receiverZipCode": "210012",
      "receiverStreet": "日土镇",
      "orderFlag": 8589967360,
      "orderCode": "LBX0329964253276237",
      "properties": {
        "serviceItemId": "5000000000059",
        "supportJPKX": "1",
        "scheduleStockOutTime": "20200211120000",
        "whcGreyFlag": "2",
        "operateType": "1",
        "scheduleArriveTime": "2020/02/11/20/00/00",
        "lgType": "5",
        "deliveryServiceType": "std",
        "isStrongMode": "true",
        "confirmType": "0",
        "taobaoTradeId": "69584475470001"
      }
    },
    "orderCode": "LBX0329964253276237",
    "globalFulfillTag": 0,
    "isStocktaking": 0,
    "storeCode": "STB",
    "isReverseOrder": false,
    "customer": {
      "nickName": "眼前不苟且",
      "class": "com.cainiao.gaea.platform.spi.component.bizorder.domain.bean.User",
      "userId": 36319222
    },
    "tomWaybillServiceLayered": "2010001"
  },
  "success": true,
  "class": "com.cainiao.gaea.platform.spi.common.Result",
  "msgInfo": "success",
  "msgCode": "PLATFORM-PLATFORM-000-000",
  "responseCode": {
    "notSuccess": false,
    "code": "PLATFORM-PLATFORM-000-000",
    "success": true,
    "error": {
      "code": "000",
      "errorType": {
        "code": "000",
        "class": "com.cainiao.gaea.platform.spi.common.constant.ErrorType"
      },
      "message": "success",
      "class": "com.cainiao.gaea.platform.spi.common.constant.Error"
    },
    "message": "success",
    "class": "com.cainiao.gaea.platform.spi.common.ResponseCode"
  }
}
```


```json
{
  "notSuccess": false,
  "data": {
    "orderType": 100,
    "perSellTag": 0,
    "featureMap": {
      "ggCpCode": "YUNDA",
      "serviceTags": "5000000011417-2$%5000000000049-2$tgSource@cc&%",
      "ggFulfilOrderId": "2909157825",
      "city": "441300",
      "ggConsign": "1",
      "ggItemId": "3000000040",
      "outUId": "guoguo2692057536-2617841940053675",
      "province": "440000",
      "bizSource": "14",
      "ldMsgHeader": "1|JiJian#1",
      "consignment": "cc_web",
      "district": "441323",
      "goodsNum": "1",
      "dId": "441323"
    },
    "allowSellDesc": "",
    "tmallSeaDistributeMode": 2,
    "orderStatusDesc": "收件方已签收",
    "guoguoDipanId": "220352",
    "lpCpCodeWhiteList": [
      72,
      141,
      156
    ],
    "signDate": "2020-06-21",
    "sellOffTagDesc": "",
    "bizOrderPlatformTypeId": 10000,
    "jidanTag": 0,
    "guoguoCollectNetworkCodeWhiteList": [
      78
    ],
    "consigneeAddress": {
      "province": {
        "name": "广东省",
        "id": 440000,
        "class": "com.cainiao.gaea.platform.spi.component.bizorder.domain.bean.Province"
      },
      "city": {
        "name": "惠州市",
        "id": 441300,
        "class": "com.cainiao.gaea.platform.spi.component.bizorder.domain.bean.City"
      },
      "street": {
        "name": "广东省惠州市惠东县 大岭镇 洪发路8号后栋祝福鞋厂2楼(用配送的售后卡或者白纸填写好 淘宝账号.收件人姓名，手机联系方式.订单号 放进鞋盒)",
        "class": "com.cainiao.gaea.platform.spi.component.bizorder.domain.bean.Street"
      },
      "district": {
        "name": "惠东县",
        "id": 441323,
        "class": "com.cainiao.gaea.platform.spi.component.bizorder.domain.bean.District"
      },
      "fullAddress": "广东省惠州市惠东县广东省惠州市惠东县 大岭镇 洪发路8号后栋祝福鞋厂2楼(用配送的售后卡或者白纸填写好 淘宝账号.收件人姓名，手机联系方式.订单号 放进鞋盒)",
      "class": "com.cainiao.gaea.platform.spi.component.bizorder.domain.bean.Address"
    },
    "guoguoNetWorkId": 1262780,
    "ext": {
      "class": "com.cainiao.gaea.platform.bizorder.ext.LogisticsOrderCenterOrderExt"
    },
    "masterTradeId": "183127933252",
    "isCtdToHome": false,
    "consignee": {
      "contact": {
        "mobilePhone": "15019815841",
        "class": "com.cainiao.gaea.platform.spi.component.bizorder.domain.bean.Contact"
      },
      "name": "陈志安",
      "class": "com.cainiao.gaea.platform.spi.component.bizorder.domain.bean.User"
    },
    "globalFulfillTag": 0,
    "reportPriceCurrency": "",
    "cargosTotalPrice": 0,
    "gRejectRefundService": "null",
    "guoguoBizCode": "TRADE_REFUND_GOODS",
    "guoguoChannel": "10005",
    "lpCpCode": "YUNDA",
    "orderStatus": 100,
    "cargos": [
      {
        "owner": {
          "name": "",
          "class": "com.cainiao.gaea.platform.spi.component.bizorder.domain.bean.User"
        },
        "item": {
          "orderGoodsId": 244874913271,
          "unitPrice": 0,
          "serviceItemId": 5000000011417,
          "amount": 0,
          "precious": 0,
          "quantity": 1,
          "goodsId": 0,
          "name": "GuoGuoItem",
          "currency": "CNY",
          "id": 0,
          "class": "com.cainiao.gaea.platform.spi.component.bizorder.domain.bean.Item"
        },
        "class": "com.cainiao.gaea.platform.spi.component.bizorder.domain.bean.Cargo",
        "subTradeId": "244874913271"
      }
    ],
    "guoguoCollectNetworkCode": "YUNDA#550018",
    "separateOrderFlag": 0,
    "guoguoItemId": "3000000040",
    "isDelivering": 1,
    "isBizOrderSrcDefined": 0,
    "netWorkName": "贵州贵阳乌当公司",
    "lpOrderCreated": 1592468335000,
    "consignorAddress": {
      "province": {
        "name": "贵州省",
        "id": 520000,
        "class": "com.cainiao.gaea.platform.spi.component.bizorder.domain.bean.Province"
      },
      "city": {
        "name": "贵阳市",
        "id": 520100,
        "class": "com.cainiao.gaea.platform.spi.component.bizorder.domain.bean.City"
      },
      "street": {
        "name": "创新社区新庄路新庄村小郑美发店",
        "class": "com.cainiao.gaea.platform.spi.component.bizorder.domain.bean.Street"
      },
      "district": {
        "name": "乌当区",
        "id": 520112,
        "class": "com.cainiao.gaea.platform.spi.component.bizorder.domain.bean.District"
      },
      "fullAddress": "贵州省贵阳市乌当区创新社区新庄路新庄村小郑美发店",
      "class": "com.cainiao.gaea.platform.spi.component.bizorder.domain.bean.Address"
    },
    "providerId": 2692057536,
    "serviceId": [
      2,
      2
    ],
    "hasWmsArrive": 0,
    "class": "com.cainiao.gaea.platform.spi.component.bizorder.domain.LogisticsOrderCenterOrder",
    "worryFreeReturn": true,
    "consignor": {
      "contact": {
        "mobilePhone": "18586907153",
        "class": "com.cainiao.gaea.platform.spi.component.bizorder.domain.bean.Contact"
      },
      "name": "张红芳",
      "class": "com.cainiao.gaea.platform.spi.component.bizorder.domain.bean.User"
    },
    "orderCreated": 1592468335000,
    "guoguoIsSignedAmount": false,
    "business": {
      "nickName": "awyzhf",
      "contact": {
        "mobilePhone": "18300891185",
        "class": "com.cainiao.gaea.platform.spi.component.bizorder.domain.bean.Contact",
        "email": ""
      },
      "name": "张红芳",
      "shopName": "张红芳",
      "shopId": 2692057536,
      "class": "com.cainiao.gaea.platform.spi.component.bizorder.domain.bean.Shop",
      "userId": 2692057536
    },
    "gRefundStoreValue": false,
    "guoguoSendOrderStatus": 40,
    "isDistributeModeDefined": 0,
    "tpc": "",
    "transportCode": "4306372898240",
    "vipTag": 0,
    "userId": 2692057536,
    "ggOrderId": 2617841940053675,
    "multiLPSinglePackage": true,
    "productCode": "NORMAL_PACKAGE",
    "isEsTimeExist": false,
    "lpCompanyCode": "YUNDA",
    "locOrderDO": {
      "fetcher": {
        "zipCode": "000000",
        "country": "中国",
        "address": "贵州省^^^贵阳市^^^乌当区^^^   创新社区新庄路新庄村小郑美发店",
        "districtName": "乌当区",
        "orderId": 183127933252,
        "contactId": 239082196313,
        "contactName": "张红芳",
        "dDDAdr": "贵州省贵阳市乌当区   创新社区新庄路新庄村小郑美发店",
        "cityAndDistrict": "贵阳市,乌当区",
        "userId": 2692057536,
        "countryId": 520112,
        "adr": "创新社区新庄路新庄村小郑美发店",
        "features": ";dId=520112;",
        "cityName": "贵阳市",
        "mobilePhone": "18586907153",
        "detailAddress": "创新社区新庄路新庄村小郑美发店",
        "divisionId": 520112,
        "provinceName": "贵州省",
        "class": "com.taobao.logistics.domain.dataobject.ContactDO"
      },
      "orderDO": {
        "statusDate": 1592471174000,
        "orderType": 1,
        "featureMap": {
          "ggCpCode": "YUNDA",
          "serviceTags": "5000000011417-2$%5000000000049-2$tgSource@cc&%",
          "ggFulfilOrderId": "2909157825",
          "city": "441300",
          "ggConsign": "1",
          "ggItemId": "3000000040",
          "outUId": "guoguo2692057536-2617841940053675",
          "province": "440000",
          "bizSource": "14",
          "ldMsgHeader": "1|JiJian#1",
          "consignment": "cc_web",
          "district": "441323",
          "goodsNum": "1",
          "dId": "441323"
        },
        "timeoutStatus": 0,
        "logisType": 2,
        "orderId": 183127933252,
        "receiverMobilePhone": "15019815841",
        "orderSendToFlag": "",
        "receiverProvince": "广东省",
        "receiverCity": "惠州市",
        "receiverCountryId": 441323,
        "taobaoTradeId": 183127933252,
        "features": ";ggCpCode=YUNDA;serviceTags=5000000011417-2$%5000000000049-2$tgSource@cc&%;ggFulfilOrderId=2909157825;city=441300;ggConsign=1;ggItemId=3000000040;outUId=guoguo2692057536-2617841940053675;province=440000;bizSource=14;ldMsgHeader=1|JiJian#1;consignment=cc_web;district=441323;goodsNum=1;dId=441323;",
        "fetchBegin": "",
        "split": false,
        "standardStatus": 3000,
        "merge": false,
        "options": 128,
        "subServiceTypes": [],
        "companyCode": "YUNDA",
        "syncVersion": 2,
        "fetcherId": 239082196313,
        "buyServiceFeeLong": 0,
        "receiverDDDAdr": "广东省惠州市惠东县   广东省惠州市惠东县 大岭镇 洪发路8号后栋祝福鞋厂2楼(用配送的售后卡或者白纸填写好 淘宝账号.收件人姓名，手机联系方式.订单号 放进鞋盒)",
        "sellerSendConfirm": 0,
        "bizOrderStatus": 0,
        "spiltMerge": false,
        "codSplitFeeDouble": 0,
        "codSplitFeeLong": 0,
        "receiverAddress": "广东省^^^惠州市^^^惠东县^^^   广东省惠州市惠东县 大岭镇 洪发路8号后栋祝福鞋厂2楼(用配送的售后卡或者白纸填写好 淘宝账号.收件人姓名，手机联系方式.订单号 放进鞋盒)",
        "totalServiceFee": 0,
        "companyId": 102,
        "receiverDivisionId": 441323,
        "itemsValue": 0,
        "feature2Map": {
          "sellerId": "841116190"
        },
        "itemsValueDouble": 0,
        "provAndCity": "惠州",
        "outBizId": "guoguo2692057536-2617841940053675", /*  */
        "syncUpdate": false,
        "gmtModified": 1592471173000,
        "orderStatus": 100,
        "mailNo": "4306372898240",
        "preOrderId": 0,
        "buyServiceFeeDouble": 0,
        "receiverDistrict": "惠东县",
        "orderTypeStr": "1",
        "parcelOrder": false,
        "class": "com.taobao.logistics.domain.dataobject.OrderDO",
        "sourceOrder": false,
        "outBizSource": "14",
        "orderSource": 3,
        "fetchEnd": "",
        "receiverContact": {
          "country": "中国",
          "address": "广东省^^^惠州市^^^惠东县^^^   广东省惠州市惠东县 大岭镇 洪发路8号后栋祝福鞋厂2楼(用配送的售后卡或者白纸填写好 淘宝账号.收件人姓名，手机联系方式.订单号 放进鞋盒)",
          "districtName": "惠东县",
          "contactName": "陈志安",
          "dDDAdr": "广东省惠州市惠东县   广东省惠州市惠东县 大岭镇 洪发路8号后栋祝福鞋厂2楼(用配送的售后卡或者白纸填写好 淘宝账号.收件人姓名，手机联系方式.订单号 放进鞋盒)",
          "cityAndDistrict": "惠州市,惠东县",
          "countryId": 441323,
          "adr": "广东省惠州市惠东县 大岭镇 洪发路8号后栋祝福鞋厂2楼(用配送的售后卡或者白纸填写好 淘宝账号.收件人姓名，手机联系方式.订单号 放进鞋盒)",
          "features": ";dId=441323;",
          "sourceDivisionId": 441323,
          "cityName": "惠州市",
          "mobilePhone": "15019815841",
          "detailAddress": "广东省惠州市惠东县 大岭镇 洪发路8号后栋祝福鞋厂2楼(用配送的售后卡或者白纸填写好 淘宝账号.收件人姓名，手机联系方式.订单号 放进鞋盒)",
          "divisionId": 441323,
          "provinceName": "广东省",
          "class": "com.taobao.logistics.domain.dataobject.ContactDO",
          "sourceDetailAddress": "广东省惠州市惠东县 大岭镇 洪发路8号后栋祝福鞋厂2楼(用配送的售后卡或者白纸填写好 淘宝账号.收件人姓名，手机联系方式.订单号 放进鞋盒)"
        },
        "goodsValueLong": 0,
        "receiverName": "陈志安",
        "receiverDetailAddress": "广东省惠州市惠东县 大岭镇 洪发路8号后栋祝福鞋厂2楼(用配送的售后卡或者白纸填写好 淘宝账号.收件人姓名，手机联系方式.订单号 放进鞋盒)",
        "gmtCreate": 1592468335000,
        "logisticsService": "5000000011417-2$%5000000000049-2$tgSource@cc&%",
        "userId": 2692057536,
        "receiverAdr": "   广东省惠州市惠东县 大岭镇 洪发路8号后栋祝福鞋厂2楼(用配送的售后卡或者白纸填写好 淘宝账号.收件人姓名，手机联系方式.订单号 放进鞋盒)",
        "mailType": 200,
        "fetchDateStr": "",
        "goodsTypeId": 0,
        "goodsValueDouble": 0,
        "closed": false,
        "features2": ";sellerId=841116190;",
        "orderCode": "LP00183127933252",
        "itemValueDouble": 0,
        "refundId": 239082196314
      },
      "orderGoods": [
        {
          "orderGoodsId": 244874913271,
          "subsGood": false,
          "gmtModified": 1592471173000,
          "featureMap": {
            "goodsIndex": "0",
            "type": "00"
          },
          "orderId": 183127933252,
          "itemValue": 0,
          "feature": ";goodsIndex=0;type=00;",
          "hasSendTOTO": false,
          "sellerPropertyMap": {},
          "goodsQuantity": 1,
          "class": "com.taobao.logistics.domain.dataobject.OrderGoodsDO",
          "itemValueString": "0.00",
          "goodsName": "GuoGuoItem",
          "syncVersion": 1,
          "bundGood": false,
          "bundMain": false,
          "canSplit": false,
          "splitNum": false,
          "gmtCreate": 1592468335000,
          "logisticsService": "",
          "userId": 2692057536,
          "bundAttach": false,
          "virtualGood": false,
          "feature2Map": {},
          "cantSplit": false,
          "itemValueDouble": 0,
          "leafCategoryId": 0,
          "categoryId": 0,
          "tradeId": 244874913271
        }
      ],
      "class": "com.taobao.loc.common.domain.LocOrderDO",
      "extendsFieldDOs": []
    },
    "orderCode": "LP00183127933252",
    "isCuntaoDelivery": false,
    "isChengnuoda": false,
    "gInterceptService": "",
    "isStocktaking": 0
  },
  "success": true,
  "class": "com.cainiao.gaea.platform.spi.common.Result",
  "msgInfo": "success",
  "msgCode": "PLATFORM-PLATFORM-000-000",
  "responseCode": {
    "notSuccess": false,
    "code": "PLATFORM-PLATFORM-000-000",
    "success": true,
    "error": {
      "code": "000",
      "errorType": {
        "code": "000",
        "class": "com.cainiao.gaea.platform.spi.common.constant.ErrorType"
      },
      "message": "success",
      "class": "com.cainiao.gaea.platform.spi.common.constant.Error"
    },
    "message": "success",
    "class": "com.cainiao.gaea.platform.spi.common.ResponseCode"
  }
}

```



```json

com.cainiao.tdtradeplatform.client.service.OrderWriteService:1.0.0.pre
cancelOrder
tdtradeplatform
请求数据
[
  {
    "reason": "后台取消订单",
    "orderSystemType": "",
    "operatorInfo": {
      "name": "",
      "type": 6,
      "class": "com.cainiao.tdtradeplatform.client.domain.OperatorInfo",
      "userId": 0
    },
    "tdOrderId": 2617841940053675,
    "extendMap": {
      "": ""
    },
    "accessCode": "",
    "reasonCode": "-1",
    "buyer": {
      "nick": "",
      "userType": 0,
      "class": "com.cainiao.tdtradeplatform.client.dto.TdUser",
      "userId":2692057536
    }
  }
]
```

1011844939164 播放
```json
{
  "success": true,
  "data": {
    "sessionId": "738436960988827648-1002389092840-10002",
    "coreBizOrderNo": "1002389092840",
    "bizOrderDomain": 10002,
    "end": false,
    "sopId": 882,
    "sopName": "敬方投诉工单SOP测试",
    "nodeList": [
      {
        "nodeTag": "factor_33qdvqchrtu0000",
        "nodeName": "确认包裹是否收到测试",
        "nodeType": "simple_interactive_factor",
        "solutionType": null,
        "processKey": "1596089773535_2867",
        "inputParamMap": {},
        "outputParamMap": {
          "result": {
            "branchResult": "Default"
          },
          "s": {
            "result": {
              "branchResult": "Default"
            }
          },
          "nodeResult": [
            {
              "id": 15791,
              "factorId": 2857,
              "attributeValue": "wsd",
              "attributeName": "未收到",
              "attributeType": 1,
              "priority": 2,
              "status": 1,
              "tenantId": 1,
              "gmtModified": "2020-07-28T05:41:12.000+0000",
              "gmtCreate": "2020-07-28T05:41:12.000+0000",
              "creatorId": 790440,
              "creatorDomain": 82,
              "modifierId": 790440,
              "modifierDomain": 82
            },
            {
              "id": 15790,
              "factorId": 2857,
              "attributeValue": "sd",
              "attributeName": "收到",
              "attributeType": 1,
              "priority": 1,
              "status": 1,
              "tenantId": 1,
              "gmtModified": "2020-07-28T05:41:06.000+0000",
              "gmtCreate": "2020-07-28T05:41:06.000+0000",
              "creatorId": 790440,
              "creatorDomain": 82,
              "modifierId": 790440,
              "modifierDomain": 82
            }
          ]
        },
        "display": true,
        "nodeTip": null,
        "status": "WAIT",
        "nodeData": [
          {
            "id": 15791,
            "factorId": 2857,
            "attributeValue": "wsd",
            "attributeName": "未收到",
            "attributeType": 1,
            "priority": 2,
            "status": 1,
            "tenantId": 1,
            "gmtModified": "2020-07-28T05:41:12.000+0000",
            "gmtCreate": "2020-07-28T05:41:12.000+0000",
            "creatorId": 790440,
            "creatorDomain": 82,
            "modifierId": 790440,
            "modifierDomain": 82
          },
          {
            "id": 15790,
            "factorId": 2857,
            "attributeValue": "sd",
            "attributeName": "收到",
            "attributeType": 1,
            "priority": 1,
            "status": 1,
            "tenantId": 1,
            "gmtModified": "2020-07-28T05:41:06.000+0000",
            "gmtCreate": "2020-07-28T05:41:06.000+0000",
            "creatorId": 790440,
            "creatorDomain": 82,
            "modifierId": 790440,
            "modifierDomain": 82
          }
        ],
        "extFields": {}
      }
    ],
    "parameters": {
      "lpCode": "LP00401264563568",
      "gaeaBizOrderCode": "LBX0329964993269060",
      "lbxCode": "LBX0329964993269060",
      "gaeaCaseId": 1011844939164
    },
    "resumedPlay": false
  },
  "errorCode": null,
  "errorMsg": null
}
```
1011844939164 drive

{
  "bpmKey": "1595818119178_a175",
  "interactiveNodeDTO": {
    "attributeMap": {
      "attributeCode": "yes"
    }
  },
  "nodeTag": "factor_63te85hwvuw0000",
  "parameters": {
       "lpCode": "LP00166142746111",
      "gaeaBizOrderCode": "LBX0329964253276237",
      "lbxCode": "LBX0329964253276237",
      "gaeaCaseId": 1021837717650
   },
  "sessionId": "737440562810064896-1002379750204-10002"
}

返回消息
```json
{
  "success": true,
  "data": {
    "sessionId": "738456043306225664-1002392132501-10002",
    "coreBizOrderNo": "1002392132501",
    "bizOrderDomain": 10002,
    "end": false,
    "sopId": 882,
    "sopName": "敬方投诉工单SOP测试",
    "nodeList": [
      {
        "nodeTag": "factor_33qdvqchrtu0000",
        "nodeName": "确认包裹是否收到测试",
        "nodeType": "simple_interactive_factor",
        "solutionType": null,
        "processKey": "1596089773535_2867",
        "inputParamMap": {},
        "outputParamMap": {
          "result": {
            "branchResult": "Default"
          },
          "s": {
            "result": {
              "branchResult": "Default"
            }
          },
          "nodeResult": [
            {
              "id": 15791,
              "factorId": 2857,
              "attributeValue": "wsd",
              "attributeName": "未收到",
              "attributeType": 1,
              "priority": 2,
              "status": 1,
              "tenantId": 1,
              "gmtModified": "2020-07-28T05:41:12.000+0000",
              "gmtCreate": "2020-07-28T05:41:12.000+0000",
              "creatorId": 790440,
              "creatorDomain": 82,
              "modifierId": 790440,
              "modifierDomain": 82
            },
            {
              "id": 15790,
              "factorId": 2857,
              "attributeValue": "sd",
              "attributeName": "收到",
              "attributeType": 1,
              "priority": 1,
              "status": 1,
              "tenantId": 1,
              "gmtModified": "2020-07-28T05:41:06.000+0000",
              "gmtCreate": "2020-07-28T05:41:06.000+0000",
              "creatorId": 790440,
              "creatorDomain": 82,
              "modifierId": 790440,
              "modifierDomain": 82
            }
          ]
        },
        "display": true,
        "nodeTip": null,
        "status": "WAIT",
        "nodeData": [
          {
            "id": 15791,
            "factorId": 2857,
            "attributeValue": "wsd",
            "attributeName": "未收到",
            "attributeType": 1,
            "priority": 2,
            "status": 1,
            "tenantId": 1,
            "gmtModified": "2020-07-28T05:41:12.000+0000",
            "gmtCreate": "2020-07-28T05:41:12.000+0000",
            "creatorId": 790440,
            "creatorDomain": 82,
            "modifierId": 790440,
            "modifierDomain": 82
          },
          {
            "id": 15790,
            "factorId": 2857,
            "attributeValue": "sd",
            "attributeName": "收到",
            "attributeType": 1,
            "priority": 1,
            "status": 1,
            "tenantId": 1,
            "gmtModified": "2020-07-28T05:41:06.000+0000",
            "gmtCreate": "2020-07-28T05:41:06.000+0000",
            "creatorId": 790440,
            "creatorDomain": 82,
            "modifierId": 790440,
            "modifierDomain": 82
          }
        ],
        "extFields": {}
      }
    ],
    "parameters": {
      "lpCode": "LP00400869822683",
      "gaeaBizOrderCode": "LBX0329964980702416",
      "lbxCode": "LBX0329964980702416",
      "gaeaCaseId": 1011846872606
    },
    "resumedPlay": false
  },
  "errorCode": null,
  "errorMsg": null
}
```


{
  "bpmKey": "1596089773535_2867",
  "interactiveNodeDTO": {
    "attributeMap": {
      "attributeCode":"wsd"
    }
  },
  "nodeTag": "factor_33qdvqchrtu0000",
  "parameters": {
        "lpCode": "LP00166142746111",
      "gaeaBizOrderCode": "LBX0329964253276237",
      "lbxCode": "LBX0329964253276237",
      "gaeaCaseId": 1011853888283
   },
  "sessionId": "738870023245926400-1002400811016-10002"
}

memo=%E6%82%A8%E5%A5%BD%EF%BC%8C%E6%82%A8%E5%92%A8%E8%AF%A2%E7%9A%84%E8%AE%A2%E5%8D%95%E5%8F%B7%EF%BC%9A%2A%2A%2A%2A%EF%BC%8C%E9%97%AE%E9%A2%98%E5%A4%84%E7%90%86%E7%BB%93%E6%9E%9C%E4%B8%BA%2A%2A%2A%2A%EF%BC%8C%E6%84%9F%E8%B0%A2%E6%82%A8%E5%AF%B9%E8%8F%9C%E9%B8%9F%E8%A3%B9%E8%A3%B9%E7%9A%84%E6%94%AF%E6%8C%81%EF%BC%81&sessionId=724634075553468416-1002180721251-10002&caseId=1021694429990&taskId=1002180721251

gaea-platform 
com.cainiao.gaea.platform.spi.component.facade.FunctionExecuteFacadeService:1.0.0
singleExecute
[
  {
    "functionCode": "caseFinish",
    "appId": "",
    "dealerDTO": {
      "resourceId": 0,
      "companyId":0,
      "role": 9,
      "skillGoupId": 0,
      "domain": 83,
      "id": 2208717250982,
      "tenantCode": "CAINIAO",
      "class": "com.cainiao.gaea.platform.spi.component.facade.domain.DealerDTO"
    },
    "tenantId": 0,
    "functionConfigInput": {
      "caseId":"1011853888283",
      "memo":"请求测试",
      "taskId":"1002400811016"
    },
    "tenantCode": "CAINIAO",
    "lang": ""
  }
]


FunctionExecuteInputRequestWrapper[functionCode=caseFinish,functionConfigInput={"caseId":"1011853888283","memo":"测试结单","attachList":[],"taskId":"1002400811016"},tenantCode=CAINIAO]



{"functionCode":"addNewMessage","functionConfigInput":"{\"memo\":\"添加留言测试\",\"attachList\":[],\"caseId\":\"1011845736895\",\"taskId\":\"1002390876116\"}"}

https://desk.cainiao.com/function/executeFunction

functionCode: "caseHangUp"
functionConfigInput: "{"caseId":"1011853888283","taskId":"1002400811016","remindType":1,"remindTime":"2020-08-01 15:25:15","note":"敬方2020-08-01：15：03工单挂起测试","bizDomain":2,"bizId":"1002400811016","notifyChannel":1,"receiverDomain":83,"receiverId":2208717250982,"remindCreateType":"2","remindSpan":19,"remindStart":1596265541298,"reminderDomain":83,"reminderId":2208717250982,"source":1,"spanType":1}"


{
  "bizType": 1000011046,
  "customerRole": 1,
  "memo": "敬方投诉测试",
  "orderCode": "LBX0329964253276237"
}



```json
{
	"caseId": 1021694429990,
	"taskId": 1002180721251,
	"remindType": "11",
	"remindTime": "2020-08-01T12:33:22.000Z",
	"remindNote": "敬方2020-08-01：20：12 工单挂起测试",
	"ignoreTimeConstraint": false,
	"parameters": {
		"lpCode": "LP00183127933252",
		"gaeaBizOrderCode": "LP00183127933252",
		"gaeaCaseId": 1021694429990
	},
	"sessionId": "724634075553468416-1002180721251-10002"
}
```

{
	"bizOrderCode": "LBX0329964253276237",
	"customerRole": "2",
	"bizlineId": 24,
	"bizTypeId": "1000000114",
	"mobile": "18117842737",
	"memo": "敬方投诉测试:2020-08-01:21:22:58",
	"attachWrapperList": [{
		"path": "https://56newroute.oss-cn-shanghai.aliyuncs.com/crm_cainiao/1596288227194-OAEw-tempsnip.png"
	}],
	"caseType": 1,
	"bizOrderDomain": 10001
}

xspace

{
	"customerRole": 2,
	"bizOrderCode": "LBX0329964253276237",
	"bizLineId": "24",
	"bizTypeId": "1000000114",
	"telephone": "18117842737",
	"memo": "敬方测试2020-08-01： 21:27:58",
	"aliPayRealName": "",
	"bizLineRuleId": "2818",
	"serviceDefine": "966",
	"bizOrderDomain": 10001,
	"attach": "https://56newroute.oss-cn-shanghai.aliyuncs.com/crm_cainiao/1596288518469-iQ7R-tempsnip.png"
}

{
  "success": true,
  "data": {
    "bizOrderType": {
      "label": "交易出库(仓储中心)",
      "value": 100201
    },
    "bizOrderStatus": {
      "label": "签收成功 ",
      "value": 200
    },
    "bizOrderPlatformTypeId": {
      "label": "仓储中心",
      "value": 10001
    }
  },
  "errorCode": null,
  "errorMsg": null
}

{
"functionCode":"caseFinish",
 "functionConfigInput":"{\"memo\":\"转交测试\",\"actionCode\":\"caseFinish,finish\",\"caseId\":\"1011853888283\",\"taskId\":\"1002400811016,\"attachList\":[{\"attach\":\"https://56newroute.oss-cn-shanghai.aliyuncs.com/crm_cainiao/1594781695098-BdlT-plantuml.jpeg\"},{\"attach\":\"https://56newroute.oss-cn-shanghai.aliyuncs.com/crm_cainiao/1594781695098-BdlT-plantuml.jpeg\"}]"}"
}

{
	"bizOrderCode": "LBX0329964253276237",
	"customerRole": "2",
	"bizTypeId": "1000011622",
	"memo": "敬方投诉测试:2020-08-02:21:22:58",
	"attachWrapperList": [],
	"bizOrderDomain": 10001
}
user信息

{
  "success": true,
  "data": {
    "accountId": 2207317470772,
    "enterpriseId": 16,
    "employeeId": 2208717250982,
    "account": "15372073382",
    "cnUserLogin": true,
    "accountDomain": 83
  },
  "errorCode": null,
  "errorMsg": null
}

{
	"customerRole": "2",
	"bizOrderCode": "LBX0329964253276237",
	"bizlineId": 24,
	"bizTypeId": "1000011622",
	"mobile": "18117842737",
	"memo": "敬方前端联调测试；2020-08-03:11:44:58",
	"attachWrapperList": [{
		"path": "https://56newroute.oss-cn-shanghai.aliyuncs.com/crm_cainiao/1596426405366-EBxr-tempsnip.png"
	}, {
		"path": "https://56newroute.oss-cn-shanghai.aliyuncs.com/crm_cainiao/1596426412184-Q0Jq-捕获.JPG"
	}],
	"caseType": 1,
	"bizOrderDomain": 10001
}

{"customerRole":"2","bizOrderCode":"LBX0329964253276237","bizlineId":24,"bizTypeId":"1000011622","mobile":"18117842737","memo":"敬方投诉测试：2020-08-03：12：00：58","attachWrapperList":[],"caseType":1,"bizOrderDomain":10001}

[admin@gaea-application.pre011179100077.na62 /home/admin]
$grep "0bb4822715964277167095430e14a7" gaea-application/logs/application.log
2020-08-03 12:08:39.855 0bb4822715964277167095430e14a7 0.1.1.7.343.8 [HSFBizProcessor-DEFAULT-8-thread-50] ERROR BizContext.putObject:165 - instance context exceptioncaseIdcannot be null
2020-08-03 12:08:40.293 0bb4822715964277167095430e14a7 0.1.1.7 [HSFBizProcessor-DEFAULT-8-thread-49] ERROR DefaultCaseCreateManager.putFactorForGuoGuoVip:3038 - nbVipService queryVip error, userId:{}2672115996nbVipResult:{}com.cainiao.nbreward.dto.ResultDTO@3c137d9a[data=<null>,retCode=<null>,retMessage=<null>,isSuccess=true]
2020-08-03 12:08:41.157 0bb4822715964277167095430e14a7 0.1.1.7 [HSFBizProcessor-DEFAULT-8-thread-49] ERROR PreCreateCaseEvent.matchStrategy:155 - Failed to match strategy, serviceDefineId=128733,result=Result[data=StrategyApplyDTO[id=<null>,serviceDefineId=<null>,distributeRuleSetId=<null>,nodeTag=<null>,secretId=<null>,caseType=<null>,bizLineId=<null>,customerRole=<null>,bizOrderType=<null>,bizType=<null>,status=<null>,creator=<null>,modifier=<null>,gmtModified=<null>,gmtCreate=<null>,sceneId=<null>,executionPhase=<null>,retryType=<null>,retryTypeDesc=<null>,retryPeriod=<null>,retryPeriodDesc=<null>],success=true,exceptionStack=<null>,responseCode=ResponseCode[code=PLATFORM-PLATFORM-000-000,message=success,systemCode=PLATFORM,componentCode=PLATFORM,error=Error[errorType=com.cainiao.gaea.platform.spi.common.constant.ErrorType@7965294d,code=000,message=success]]]
2020-08-03 12:08:41.159 0bb4822715964277167095430e14a7 0.1.1.7 [HSFBizProcessor-DEFAULT-8-thread-49] ERROR OperationChain.doOperation:41- Failed to do operation for operation-chain[StrategyMatcher$$EnhancerBySpringCGLIB$$bc93a0f5->StrategyExecutor$$EnhancerBySpringCGLIB$$80c74eaa], caseId=LBX0329964253276237, event=PRE_CREATE_CASE, failed operation is StrategyMatcher, return Result[success=false,subSystem=<null>,module=<null>,errorCode=<null>,errorMsg=<null>,responseTime=1596427721159,data=<null>,interruptChain=true]
2020-08-03 12:08:41.168 0bb4822715964277167095430e14a7 0.1.1.7 [HSFBizProcessor-DEFAULT-8-thread-49] ERROR OperationChain.doOperation:43- Interrupt operation-chain[StrategyMatcher$$EnhancerBySpringCGLIB$$bc93a0f5->StrategyExecutor$$EnhancerBySpringCGLIB$$80c74eaa], caseId=LBX0329964253276237, event=PRE_CREATE_CASE, current operation is StrategyMatcher, return Result[success=false,subSystem=<null>,module=<null>,errorCode=<null>,errorMsg=<null>,responseTime=1596427721159,data=<null>,interruptChain=true]

{
  "success": true,
  "data": {
    "employeeId": 2208717250982,
    "accountId": 2207317470772,
    "enterpriseId": 16,
    "accountDomain": 83,
    "account": "15372073382",
    "cnUserLogin": true
  },
  "errorCode": null,
  "errorMsg": null
}


咨询工单列表:
1021881603798
1021881962573
1021881891579
1021882083544
1021881677857
1021882248202


投诉工单列表
1011881677947
1011881688897
1011882079628
1011881962704
1011882149468
1011882205663


裹裹投诉工单

1011883077264
1011882951890
1011883128108
1011883014702


https://pre-member.cainiao.com/tbcnsso.htm?redirectHttps=true&type=OTHERS&returnUrl=https%3A%2F%2Fpage-pre.cainiao.com%2Fmcn%2Fpost-platform%2Findex.html%3FcId%3DMTAwMDM%26aType%3DSFNG%26aCode%3DNDYzNGQ4YTgtOTIxMS00Mjk4LTk2ZTctNzU1N2E2MGQ1YmI4%26itemCodes%3DMzAwMDAwMDAyMg%26rName%3D6ZmI5b-X5a6J%26rMobile%3DMTUwMTk4MTU4NDE%26rAreaId%3DNDQxMzIzMTAx%26rDetailAddress%3D5Lit5Zu95bm_5Lic55yB5oOg5bee5biC5oOg5Lic5Y6_ICAg5aSn5bKt6ZWHIOa0quWPkei3rzjlj7flkI7moIvnpZ3npo_pnovljoIy5qW8KOeUqOmFjemAgeeahOWUruWQjuWNoeaIluiAheeZvee6uOWhq-WGmeWlvSDmt5jlrp3otKblj7cu5pS25Lu25Lq65aeT5ZCN77yM5omL5py66IGU57O75pa55byPLuiuouWNleWPtyDmlL7ov5vpnovnm5Ip%26sName%3D5byg57qi6Iqz%26sMobile%3DMTg1ODY5MDcxNTM%26sAreaId%3DNTIwMTEyNDA1%26sDetailAddress%3D5Lit5Zu96LS15bee55yB6LS16Ziz5biC5LmM5b2T5Yy65Yib5paw56S-5Yy65paw5bqE6Lev5paw5bqE5p2R5bCP6YOR576O5Y-R5bqX%26extIds%3DTFAwMDE4MzEyNzkzMzI1Mg%26t%3DMTU5NjgwNTA3NzIyNw%26sign%3DNzc0YWQxMjc0OWMyOWZjMGJiMDBmNWQ4YjJhMmUxYjY%26

https://member.cainiao.com/tbcnsso.htm?redirectHttps=true&type=OTHERS&returnUrl=https%3A%2F%2Fpage.cainiao.com%2Fmcn%2Fpost-platform%2Findex.html%3FcId%3DMTAwMDM%26aType%3DSFNG%26aCode%3DYTU2ODg1NjktMjI5Ni00NzBlLWEwYzItMTM5MTdkNTVhNjFh%26itemCodes%3DMzAwMDAwMDA0MA%26rName%3D6ZmI5b-X5a6J%26rMobile%3DMTUwMTk4MTU4NDE%26rAreaId%3DNDQxMzIzMTAx%26rDetailAddress%3D5Lit5Zu95bm_5Lic55yB5oOg5bee5biC5oOg5Lic5Y6_ICAg5aSn5bKt6ZWHIOa0quWPkei3rzjlj7flkI7moIvnpZ3npo_pnovljoIy5qW8KOeUqOmFjemAgeeahOWUruWQjuWNoeaIluiAheeZvee6uOWhq-WGmeWlvSDmt5jlrp3otKblj7cu5pS25Lu25Lq65aeT5ZCN77yM5omL5py66IGU57O75pa55byPLuiuouWNleWPtyDmlL7ov5vpnovnm5Ip%26sName%3D5byg57qi6Iqz%26sMobile%3DMTg1ODY5MDcxNTM%26sAreaId%3DNTIwMTEyNDAz%26sDetailAddress%3D5Lit5Zu96LS15bee55yB6LS16Ziz5biC5LmM5b2T5Yy65Yib5paw56S-5Yy65paw5bqE6Lev5paw5bqE5p2R5bCP6YOR576O5Y-R5bqX%26extIds%3DTFAwMDE4MzEyNzkzMzI1Mg%26t%3DMTU5NjgwNTcxMjY2OQ%26sign%3DMzM2NTQ4MGQ1OTAxMGZjOTU0MmQxMWMxNjg4NDgxMGE%26

{"success":true,"data":"https://pre-member.cainiao.com/tbcnsso.htm?redirectHttps=true&type=OTHERS&returnUrl=https%3A%2F%2Fpage-pre.cainiao.com%2Fmcn%2Fpost-platform%2Findex.html%3FcId%3DMTAwMDM%26aType%3DSFNG%26aCode%3DNDYzNGQ4YTgtOTIxMS00Mjk4LTk2ZTctNzU1N2E2MGQ1YmI4%26itemCodes%3DMzAwMDAwMDAyMg%26rName%3D6ZmI5b-X5a6J%26rMobile%3DMTUwMTk4MTU4NDE%26rAreaId%3DNDQxMzIzMTAx%26rDetailAddress%3D5Lit5Zu95bm_5Lic55yB5oOg5bee5biC5oOg5Lic5Y6_ICAg5aSn5bKt6ZWHIOa0quWPkei3rzjlj7flkI7moIvnpZ3npo_pnovljoIy5qW8KOeUqOmFjemAgeeahOWUruWQjuWNoeaIluiAheeZvee6uOWhq-WGmeWlvSDmt5jlrp3otKblj7cu5pS25Lu25Lq65aeT5ZCN77yM5omL5py66IGU57O75pa55byPLuiuouWNleWPtyDmlL7ov5vpnovnm5Ip%26sName%3D5byg57qi6Iqz%26sMobile%3DMTg1ODY5MDcxNTM%26sAreaId%3DNTIwMTEyNDA1%26sDetailAddress%3D5Lit5Zu96LS15bee55yB6LS16Ziz5biC5LmM5b2T5Yy65Yib5paw56S-5Yy65paw5bqE6Lev5paw5bqE5p2R5bCP6YOR576O5Y-R5bqX%26extIds%3DTFAwMDE4MzEyNzkzMzI1Mg%26t%3DMTU5Njg1NTIyMjAwMg%26sign%3DNThiNzZiMDdhOTZiMzY5Y2VlZjUyZDVjZTc4YmJlMzM%26","errorCode":null,"errorMsg":null}

[
  {
    "features": {
      "": ""
    },
    "accoutType": "XSPACE",
    "bizCode": "LP00183127933252",
    "source": "GAEA",
    "userId": 2208717250982
  }
]
https://pre-desk.cainiao.com/function/executeFunction

{"functionCode":"caseHangUp","functionConfigInput":"{\"caseId\":\"1011882149468\",\"taskId\":\"1002436163699\",\"remindType\":1,\"remindTime\":\"2020-08-10 17:13:16\",\"note\":\"敬方工单挂起测试 2020-08-10 17:03:58\",\"bizDomain\":2,\"bizId\":\"1002436163699\",\"notifyChannel\":1,\"receiverDomain\":83,\"receiverId\":2208717250982,\"remindCreateType\":\"2\",\"remindSpan\":9,\"remindStart\":1597050230589,\"reminderDomain\":83,\"reminderId\":2208717250982,\"source\":1,\"spanType\":1}"}

{"functionCode":"caseHangUp","functionConfigInput":"{\"caseId\":1021895315010,\"taskId\":1002454187018,\"remindType\":1,\"remindTime\":\"2020-08-10 18:11:01\",\"note\":\"挂起投诉测试2020-08-10 17：10：34\",\"bizDomain\":2,\"bizId\":1002454187018,\"notifyChannel\":1,\"receiverDomain\":83,\"receiverId\":2208717250982,\"remindCreateType\":\"2\",\"remindSpan\":59,\"remindStart\":1597050664493,\"reminderDomain\":83,\"reminderId\":2208717250982,\"source\":1,\"spanType\":1}"}


{"caseId":"1011884214561","taskId":"1002439482377","remindType":"11","remindTime":"2020-08-10T09:35:33.541Z","remindNote":"工单挂起测试","ignoreTimeConstraint":true,"parameters":{"lpCode":"LP00402157784862","gaeaBizOrderCode":"LP00402157784862","gaeaCaseId":1011884214561},"sessionId":"741317013032538112-1002439482377-10002"}

{"success":true,"data":{"timeValid":true,"tip":"setReminder success"},"errorCode":null,"errorMsg":null}




{"functionCode":"caseHangUp","functionConfigInput":"{\"caseId\":\"1011817393014\",\"taskId\":\"1002454187012\",\"remindType\":1,\"remindTime\":\"2020-08-10 20:19:35\",\"note\":\"敬方工单挂起测hi2020-08-10 17:19:58\",\"bizDomain\":2,\"bizId\":\"1002454187012\",\"notifyChannel\":1,\"receiverDomain\":83,\"receiverId\":2208717250982,\"remindCreateType\":\"2\",\"remindSpan\":179,\"remindStart\":1597051208389,\"reminderDomain\":83,\"reminderId\":2208717250982,\"source\":1,\"spanType\":1}"}

{"functionCode":"caseHangUp","functionConfigInput":"{\"caseId\":\"1011817393014\",\"taskId\":\"1002454187012\",\"remindType\":1,\"remindTime\":\"2020-08-10 20:21:18\",\"note\":\"敬方工单挂起测试2020-08-10 17:21:58\",\"bizDomain\":2,\"bizId\":\"1002454187012\",\"notifyChannel\":1,\"receiverDomain\":83,\"receiverId\":2208717250982,\"remindCreateType\":\"2\",\"remindSpan\":179,\"remindStart\":1597051334880,\"reminderDomain\":83,\"reminderId\":2208717250982,\"source\":1,\"spanType\":1}"}



{"functionCode":"caseHangUp","functionConfigInput":"{\"caseId\":\"1011881688897\",\"taskId\":\"1002436370437\",\"remindType\":1,\"remindTime\":\"2020-08-10 20:25:59\",\"note\":\"敬方工单挂起测试2020-08-10 17:26:58\",\"bizDomain\":2,\"bizId\":\"1002436370437\",\"notifyChannel\":1,\"receiverDomain\":83,\"receiverId\":2208717250982,\"remindCreateType\":\"2\",\"remindSpan\":179,\"remindStart\":1597051585289,\"reminderDomain\":83,\"reminderId\":2208717250982,\"source\":1,\"spanType\":1}"}

{"functionCode":"caseHangUp","functionConfigInput":"{\"caseId\":\"1011881688897\",\"taskId\":\"1002436370437\",\"remindType\":1,\"remindTime\":\"2020-08-10 20:57:56\",\"note\":\"测试方法\",\"bizDomain\":2,\"bizId\":\"1002436370437\",\"notifyChannel\":1,\"receiverDomain\":83,\"receiverId\":2208717250982,\"remindCreateType\":\"2\",\"remindSpan\":179,\"remindStart\":1597053498474,\"reminderDomain\":83,\"reminderId\":2208717250982,\"source\":1,\"spanType\":1}"}


{"functionCode":"caseHangUp","functionConfigInput":"{\"caseId\":1011817393014,\"taskId\":1002454187012,\"remindType\":1,\"remindTime\":\"2020-08-10 22:43:34\",\"note\":\"敬方挂起测试2020-08-10 19:43:\\\"58\",\"bizDomain\":2,\"bizId\":1002454187012,\"notifyChannel\":1,\"receiverDomain\":83,\"receiverId\":2208717250982,\"remindCreateType\":\"2\",\"remindSpan\":179,\"remindStart\":1597059866972,\"reminderDomain\":83,\"reminderId\":2208717250982,\"source\":1,\"spanType\":1}"}

{"functionCode":"caseHangUp","functionConfigInput":"{\"caseId\":1021895315011,\"taskId\":1002454187019,\"remindType\":1,\"remindTime\":\"2020-08-10 21:34:44\",\"note\":\"敬方投诉测试\",\"bizDomain\":2,\"bizId\":1002454187019,\"notifyChannel\":1,\"receiverDomain\":83,\"receiverId\":2208717250982,\"remindCreateType\":\"2\",\"remindSpan\":9,\"remindStart\":1597065893214,\"reminderDomain\":83,\"reminderId\":2208717250982,\"source\":1,\"spanType\":1}"}

{"functionCode":"caseHangUp","functionConfigInput":"{\"caseId\":1021895315011,\"taskId\":1002454187019,\"remindType\":1,\"remindTime\":\"2020-08-22T14:03:03.000Z\",\"note\":\"提交测试\",\"bizDomain\":2,\"bizId\":1002454187019,\"notifyChannel\":1,\"receiverDomain\":83,\"receiverId\":2208717250982,\"remindCreateType\":\"2\",\"remindSpan\":17254,\"remindStart\":1597069686523,\"reminderDomain\":83,\"reminderId\":2208717250982,\"source\":1,\"spanType\":1}"}

{"functionCode":"caseHangUp","functionConfigInput":"{\"caseId\":1011884378406,\"taskId\":1002439210928,\"remindType\":1,\"remindTime\":\"2020-08-29T16:00:00.000Z\",\"note\":\"敬方挂起测试1011884378406\",\"bizDomain\":2,\"bizId\":1002439210928,\"notifyChannel\":1,\"receiverDomain\":83,\"receiverId\":2208717250982,\"remindCreateType\":\"2\",\"remindSpan\":27445,\"remindStart\":1597070077198,\"reminderDomain\":83,\"reminderId\":2208717250982,\"source\":1,\"spanType\":1}"}

{
  "functionCode": "caseHangUp",
  "functionConfigInput": "{\"caseId\":1011884378406,\"taskId\":1002439210928,\"remindType\":1,\"note\":\"敬方挂起测试1011884378406\",\"bizDomain\":2,\"bizId\":1002439210928,\"notifyChannel\":1,\"receiverDomain\":83,\"receiverId\":2208717250982,\"remindCreateType\":\"2\",\"remindSpan\":27445,\"remindStart\":1597070077198,\"reminderDomain\":83,\"reminderId\":2208717250982,\"source\":1,\"spanType\":1}",
  "tenantCode": "CAINIAO"
}

{
    "functionCode": "caseHangUp",
    "functionConfigInput": "{\"caseId\":\"1011817393014\",\"taskId\":\"1002454187012\",\"remindType\":1,\"remindTime\":\"2020-08-10 20:19:35\",\"note\":\"敬方工单挂起测hi2020-08-10 17:19:58\",\"bizDomain\":2,\"bizId\":\"1002454187012\",\"notifyChannel\":1,\"receiverDomain\":83,\"receiverId\":2208717250982,\"remindCreateType\":\"2\",\"remindSpan\":179,\"remindStart\":1597051208389,\"reminderDomain\":83,\"reminderId\":2208717250982,\"source\":1,\"spanType\":1}"
}

/action/loadSpeechTemplateContent?caseId=1021859531006&speechTemplateId=1428

memo: 留言测试
contactRole: receiverMobile
messageNotifyEmail: 673018396@qq.com
caseId: 1021859531006
taskId: 1002409103005
operateType: 50

## 4. 前端接口输入和输出参数确定

### 4.1 `/action/loadSpeechTemplateContent`
request
```json
{"success":true,"data":"外呼小件员，小件员表示已经上门揽收","errorCode":null,"errorMsg":null}
```

```json
{
    "success":true,
    "data":"外呼小件员，小件员表示已经上门揽收",
    "errorCode":null,
    "errorMsg":null
}
```

###4.2 `action/getOutBound`
request
```json
:authority: gaea-xspace-desk.taobao.com
:method: GET
:path: /action/getOutBound?caseId=1021694429990
:scheme: https
```

```json
{
    "success":true,
    "data":{
        "hotLineNumbers":[
            {
                "contactRoleName":"菜鸟消费者热线",
                "contactRole":"cnBuyerHotLine",
                "callNumber":"057156264814",
                "tuoMinCallNumber":"057****4814"
            },
            {
                "contactRoleName":"天猫海外外呼热线(香港)",
                "contactRole":"tmailHKHotLine",
                "callNumber":"0085230183612",
                "tuoMinCallNumber":"008****3612"
            },
            {
                "contactRoleName":"菜鸟消费者统一热线",
                "contactRole":"cnConsumerHotline",
                "callNumber":"9519666",
                "tuoMinCallNumber":"9****66"
            }],
        "contacts":[
            {
                "contactRoleName":"用户手机",
                "contactRole":"customerMobile",
                "callNumber":"18300891185",
                "tuoMinCallNumber":"183****1185"
            },
            {
                "contactRoleName":"收件人手机",
                "contactRole":"receiverMobile",
                "callNumber":"15019815841",
                "tuoMinCallNumber":"150****5841"
            },
            {
                "contactRoleName":"裹裹寄件人手机",
                "contactRole":"guoguoEtcherMobile",
                "callNumber":"18586907153",
                "tuoMinCallNumber":"185****7153"
            },
            {
                "contactRoleName":"顺丰",
                "contactRole":"SFExpress",
                "callNumber":"95538",
                "tuoMinCallNumber":"9****"
            },
            {
                "contactRoleName":"小件员电话",
                "contactRole":"deliveryGuyMobile",
                "callNumber":"18286052381",
                "tuoMinCallNumber":"182****2381"
            }],
        "canUserDefined":null,
        "wangwangUrl":"aliim:sendmsg?touid=cntaobaoawyzhf&siteid=cntaobao&fenliu=1&status=0"
    },
    "errorCode":null,
    "errorMsg":null
}

```

### 4.3 `newMessage`

```json
:authority: gaea-xspace-desk.taobao.com
:method: POST
:path: /caseDeal/newMessage

memo: 外呼小件员，小件员表示已经上门揽收
maskCustomer: true
maskCP: true
contactRole: receiverMobile
messageNotifyEmail: 673018396@qq.com
caseId: 1021694429990
taskId: 1002180721251
operateType: 50
```


```json
{
    "success":true,
    "data":true,
    "errorCode":null,
    "errorMsg":null
}
```

### 4.4 `action/previewText`

```json

:authority: gaea-xspace-desk.taobao.com
:method: GET
:path: /action/previewText?caseId=1021694429990&noticeTaskId=25&receiverRole=34003&templateId=150
```

```json
{
    "success":true,
    "data":{
        "mobile":"185****7153",
        "content":"【菜鸟】亲，您好。因关于您LP00183127933252订单的问题给您回电未接通，请您保持手机畅通，我们预计会在3个工作时后再次与您联系，感谢您对我们工作的支持与理解，祝您生活愉快。"
    },
    "errorCode":null,
    "errorMsg":null
}
```

### 4.5 `action/listTextReceiver`

```json
:authority: gaea-xspace-desk.taobao.com
:method: GET
:path: /action/listTextReceiver?caseId=1021694429990
:scheme: https
```

```json
{
    "success":true,
    "data":[
        {
            "contactRoleName":"订单寄件人",
            "contactRole":"34003",
            "callNumber":"18586907153",
            "tuoMinCallNumber":"185****7153"
        },
        {
            "contactRoleName":"订单收件人",
            "contactRole":"34002",
            "callNumber":"15019815841",
            "tuoMinCallNumber":"150****5841"
        },
        {
            "contactRoleName":"订单下单人",
            "contactRole":"5001",
            "callNumber":"18300891185",
            "tuoMinCallNumber":"183****1185"
        }],
    "errorCode":null,
    "errorMsg":null
}
```

### 4.6 `action/text`

```json
:authority: gaea-xspace-desk.taobao.com
:method: GET
:path: /action/text?caseId=1021694429990&noticeTaskId=25&receiverRole=34002&templateId=150
:scheme: https
```

```json
{
    "success":true,
    "data":null,
    "errorCode":null,
    "errorMsg":null
}
```

### 4.7 `action/cancelGuoguo`

```json
:authority: gaea-xspace-desk.taobao.com
:method: GET
:path: /action/cancelGuoguo?caseId=1021694429990
:scheme: https
```

```json

{"success":false,"data":null,"errorCode":null,"errorMsg":"取消订单失败"}
```

### 4.8 `action/callOutTransport`

```json
:authority: gaea-xspace-desk.taobao.com
:method: GET
:path: /action/callOutTransport?caseId=1021694429990
```

```json
{
    "success":true,
    "data":"https://member.cainiao.com/tbcnsso.htm?redirectHttps=true&type=OTHERS&returnUrl=https%3A%2F%2Fpage.cainiao.com%2Fmcn%2Fpost-platform%2Findex.html%3FcId%3DMTAwMDM%26aType%3DSFNG%26aCode%3DYTU2ODg1NjktMjI5Ni00NzBlLWEwYzItMTM5MTdkNTVhNjFh%26itemCodes%3DMzAwMDAwMDA0MA%26rName%3D6ZmI5b-X5a6J%26rMobile%3DMTUwMTk4MTU4NDE%26rAreaId%3DNDQxMzIzMTAx%26rDetailAddress%3D5Lit5Zu95bm_5Lic55yB5oOg5bee5biC5oOg5Lic5Y6_ICAg5aSn5bKt6ZWHIOa0quWPkei3rzjlj7flkI7moIvnpZ3npo_pnovljoIy5qW8KOeUqOmFjemAgeeahOWUruWQjuWNoeaIluiAheeZvee6uOWhq-WGmeWlvSDmt5jlrp3otKblj7cu5pS25Lu25Lq65aeT5ZCN77yM5omL5py66IGU57O75pa55byPLuiuouWNleWPtyDmlL7ov5vpnovnm5Ip%26sName%3D5byg57qi6Iqz%26sMobile%3DMTg1ODY5MDcxNTM%26sAreaId%3DNTIwMTEyNDAz%26sDetailAddress%3D5Lit5Zu96LS15bee55yB6LS16Ziz5biC5LmM5b2T5Yy65Yib5paw56S-5Yy65paw5bqE6Lev5paw5bqE5p2R5bCP6YOR576O5Y-R5bqX%26extIds%3DTFAwMDE4MzEyNzkzMzI1Mg%26t%3DMTU5MzkzOTY2MTkwNA%26sign%3DNjQ0ZWFiMjBlY2YzY2NjZWVmMDQ0NGYwZGQ1ZjhjMDQ%26",
    "errorCode":null,
    "errorMsg":null
}
```

### 4.9 `taskDeal/checkGetSkillGroup`

```json

:authority: gaea-xspace-desk.taobao.com
:method: GET
:path: /taskDeal/checkGetSkillGroup?taskId=1002180721251
:scheme: https
```

```json
{"success":true,"data":true,"errorCode":null,"errorMsg":null}
```

### 4.10 `action/listXSpaceTransferSkillGroup`

```json
:authority: gaea-xspace-desk.taobao.com
:method: GET
:path: /action/listXSpaceTransferSkillGroup
:scheme: https
```

```json
{
    "success":true,
    "data":[
        {
            "id":181,
            "name":"万声裹裹咨询工单组"
        },
        {
            "id":168,
            "name":"猫超破损U+升级技能组"
        },
        {
            "id":165,
            "name":"万声裹裹U+升级投诉组"
        },
        {
            "id":167,
            "name":"菜鸟驿站投诉优+升级处理组"
        },
        {
            "id":334,
            "name":"零售通U+升级技能组"
        },
        {
            "id":70,
            "name":"驿站投诉-普通组"
        },
        {
            "id":75,
            "name":"驿站咨询-普通"
        },
        {
            "id":458,
            "name":"优加驿站投诉工单二次升级组"
        }],
    "errorCode":null,
    "errorMsg":null
}

```

### 4.11 `common/getOssTempPolicy`

```json

:authority: gaea-xspace-desk.taobao.com
:method: GET
:path: /common/getOssTempPolicy
:scheme: https
```

```json
{
    "success":true,
    "data":{
        "signature":"DH0VG9EGqKdDcANi+aq8Ay3TWJs=",
        "accessid":"nXOaB8BT48mO33gK",
        "policy":"eyJleHBpcmF0aW9uIjoiMjAyMC0wNy0wNVQxNTowOTozOS4zNTRaIiwiY29uZGl0aW9ucyI6W1siY29udGVudC1sZW5ndGgtcmFuZ2UiLDAsMjA5NzE1Ml0sWyJzdGFydHMtd2l0aCIsIiRrZXkiLCJjcm1fY2Fpbmlhby8iXV19",
        "fileName":"1593958179354-YRCy-",
        "dir":"crm_cainiao/",
        "host":"https://56newroute.oss-cn-shanghai.aliyuncs.com/",
        "expire":"1593961779",
        "Access-Control-Allow-Origin":"*",
        "Access-Control-Allow-Methods":"GET, POST"
    },
    "errorCode":null,
    "errorMsg":null
}
```

### 4.12 https://56newroute.oss-cn-shanghai.aliyuncs.com/

```json
Accept: */*
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9,zh-CN;q=0.8,zh;q=0.7
Connection: keep-alive
Content-Length: 5245
Content-Type: multipart/form-data; boundary=----WebKitFormBoundary45z8jbEDEYSLABeh
Host: 56newroute.oss-cn-shanghai.aliyuncs.com


key: crm_cainiao/1593958179354-YRCy-2019-04-10-markdown_learning_note.md
policy: eyJleHBpcmF0aW9uIjoiMjAyMC0wNy0wNVQxNTowOTozOS4zNTRaIiwiY29uZGl0aW9ucyI6W1siY29udGVudC1sZW5ndGgtcmFuZ2UiLDAsMjA5NzE1Ml0sWyJzdGFydHMtd2l0aCIsIiRrZXkiLCJjcm1fY2Fpbmlhby8iXV19
OSSAccessKeyId: nXOaB8BT48mO33gK
sucess_action_status: 200
signature: DH0VG9EGqKdDcANi aq8Ay3TWJs=
file: (binary)
```



```json


```

### 4.13 taskDeal/listRemindType

```json

:authority: gaea-xspace-desk.taobao.com
:method: GET
:path: /taskDeal/listRemindType?_api=listRemindType&_mock=false&_stamp=1593958531947


```

```json
// 返回分类提醒
{
    "success":true,
    "data":[
        {
            "label":"211回呼",
            "value":"1"
        },
        {
            "label":"待回访",
            "value":"3"
        },
        {
            "label":"待举证",
            "value":"4"
        },
        {
            "label":"待履约",
            "value":"5"
        },
        {
            "label":"待解决方案确认",
            "value":"6"
        },
        {
            "label":"待CP举证回复",
            "value":"7"
        },
        {
            "label":"待商家举证回复",
            "value":"8"
        },
        {
            "label":"待goc回复",
            "value":"9"
        },
        {
            "label":"订单托管",
            "value":"10"
        },
        {
            "label":"其他",
            "value":"11"
        }],
    "errorCode":null,
    "errorMsg":null
}

```

### 4.15 http://gm.mmstat.com/gaea-desk.gaeaDialog.endTimeLength?endTime=1593958711996&dialogType=ticketHangUpVisible&id=1021694429990&t=1593958711996&platform=xspace&userId=8992088
这个是干什么的

```json

http://gm.mmstat.com/gaea-desk.gaeaDialog.endTimeLength?endTime=1593958711996&dialogType=ticketHangUpVisible&id=1021694429990&t=1593958711996&platform=xspace&userId=8992088


```
### 4.14 taskDeal/setReminder

```json
:authority: gaea-xspace-desk.taobao.com
:method: POST
:path: /taskDeal/setReminder?_api=setReminder&_mock=false&_stamp=1594000879340

{
    "caseId":1021694429990,
    "taskId":1002180721251,
    "remindType":"4",
    "remindTime":"2020-07-06T02:10:36.000Z",
    "remindNote":"测试挂起",
    "ignoreTimeConstraint":false /* 是否强制挂起 */
}

```

```json
{
    "success":true,
    "data":{
        "timeValid":false,
        "tip":"已超出全链路处理效:2020-06-22 17:09:24请重新选择"
    },
    "errorCode":null,
    "errorMsg":null
}
```
ignoreTimeConstraint 会检查时效；可以选择强制挂起

```json
{
    "caseId":1021694429990,
    "taskId":1002180721251,
    "remindType":"10",
    "remindTime":"2020-07-06T02:19:37.000Z",
    "remindNote":"挂起测试",
    "ignoreTimeConstraint":true
}


```


### sop drive

```json
{
    "sessionId":"724634075553468416-1002180721251-10002",
    "nodeTag":"factor_6ysnck1qv4w0000",
    "bpmKey":"1568023898166_b541",
    "interactiveNodeDTO":{
        "attributeMap":{
            "attributeCode":"lianxibushankuaidiy"
        }
    },
    "parameters":{
        "lpCode":"LP00183127933252",
        "gaeaBizOrderCode":"LP00183127933252",
        "gaeaCaseId":1021694429990
    }
}
```

```json
{
    "success":true,
    "data":{
        "sessionId":"724634075553468416-1002180721251-10002",
        "coreBizOrderNo":"1002180721251",
        "bizOrderDomain":10002,
        "end":false,
        "sopId":null,
        "sopName":null,
        "nodeList":[
            {
                "nodeTag":"factor_6ysnck1qv4w0000",
                "nodeName":"外呼小件员结果",
                "nodeType":"simple_interactive_factor",
                "solutionType":null,
                "processKey":"1568023898166_b541",
                "inputParamMap":{
                    "e":{
                        "nodeResult":"lianxibushankuaidiy"
                    },
                    "nodeResult":"lianxibushankuaidiy",
                    "attributeMap":{
                        "attributeCode":"lianxibushankuaidiy"
                    }
                },
                "outputParamMap":{
                },
                "display":true,
                "nodeTip":null,
                "status":"COMPLETED",
                "nodeData":null,
                "extFields":{
                }
            },

            {
                "nodeTag":"action_5b367sdk76s0000",
                "nodeName":"动作",
                "nodeType":"action",
                "solutionType":null,
                "processKey":"1568023898166_b541",
                "inputParamMap":{
                },
                "outputParamMap":{
                    "result":{
                        "branchResult":"Default"
                    },
                    "e":{
                        "result":{
                            "branchResult":"Default"
                        }
                    },
                    "nodeResult":[
                        "9"]
                },
                "display":true,
                "nodeTip":null,
                "status":"COMPLETED",
                "nodeData":[
                    {
                        "id":9,
                        "name":"留言",
                        "code":"addNewMessasge",
                        "gmtCreate":1565170392000,
                        "gmtModified":1565170396000,
                        "desc":"留言",
                        "actionCategory":1,
                        "responseCategory":1,
                        "preUrl":"https://mtop.taobao.com",
                        "onlineUrl":"https://mtop.taobao.com",
                        "feature":"cloud_xiaoer",
                        "creatorId":107345,
                        "creatorDomain":81,
                        "modifierId":107345,
                        "modifierDomain":81,
                        "tenantCode":"1"
                    }],
                "extFields":{
                    "leaveMessageTemplateId":"1433"
                }
            },

            {
                "nodeTag":"factor_a52s22ow0f40000_k6vmj48l_k6vq82tf_k6vqdlyq_k71p7inw_k72sa2wr",
                "nodeName":"是否联系上消费者（裹裹）",
                "nodeType":"simple_interactive_factor",
                "solutionType":null,
                "processKey":"1568023898166_b541",
                "inputParamMap":{
                },
                "outputParamMap":{
                    "result":{
                        "branchResult":"Default"
                    },
                    "s":{
                        "result":{
                            "branchResult":"Default"
                        }
                    },
                    "nodeResult":[
                        {
                            "id":6941,
                            "factorId":874,
                            "attributeValue":"no",
                            "attributeName":"否",
                            "attributeType":1,
                            "priority":2,
                            "status":1,
                            "tenantId":1,
                            "gmtModified":1568796785000,
                            "gmtCreate":1568796785000,
                            "creatorId":771261,
                            "creatorDomain":82,
                            "modifierId":771261,
                            "modifierDomain":82
                        },
                        {
                            "id":6938,
                            "factorId":874,
                            "attributeValue":"yes",
                            "attributeName":"是",
                            "attributeType":1,
                            "priority":1,
                            "status":1,
                            "tenantId":1,
                            "gmtModified":1568796683000,
                            "gmtCreate":1568796683000,
                            "creatorId":771261,
                            "creatorDomain":82,
                            "modifierId":771261,
                            "modifierDomain":82
                        }]
                },
                "display":true,
                "nodeTip":null,
                "status":"WAIT",
                "nodeData":[
                    {
                        "id":6941,
                        "factorId":874,
                        "attributeValue":"no",
                        "attributeName":"否",
                        "attributeType":1,
                        "priority":2,
                        "status":1,
                        "tenantId":1,
                        "gmtModified":1568796785000,
                        "gmtCreate":1568796785000,
                        "creatorId":771261,
                        "creatorDomain":82,
                        "modifierId":771261,
                        "modifierDomain":82
                    },
                    {
                        "id":6938,
                        "factorId":874,
                        "attributeValue":"yes",
                        "attributeName":"是",
                        "attributeType":1,
                        "priority":1,
                        "status":1,
                        "tenantId":1,
                        "gmtModified":1568796683000,
                        "gmtCreate":1568796683000,
                        "creatorId":771261,
                        "creatorDomain":82,
                        "modifierId":771261,
                        "modifierDomain":82
                    }],
                "extFields":{
                }
            }
            
        ],
        "parameters":{
            "lpCode":"LP00183127933252",
            "gaeaBizOrderCode":"LP00183127933252",
            "gaeaCaseId":1021694429990
        },
        "resumedPlay":null
    },
    "errorCode":null,
    "errorMsg":null
}

```

```json
{
    "sessionId":"724634075553468416-1002180721251-10002",
    "nodeTag":"factor_a52s22ow0f40000_k6vmj48l_k6vq82tf_k6vqdlyq_k71p7inw_k72sa2wr",
    "bpmKey":"1568023898166_b541",
    "interactiveNodeDTO":{
        "attributeMap":{
            "attributeCode":"yes"
        }
    },
    "parameters":{
        "lpCode":"LP00183127933252",
        "gaeaBizOrderCode":"LP00183127933252",
        "gaeaCaseId":1021694429990
    }
}

```

```json

{
    "success":true,
    "data":{
        "sessionId":"724634075553468416-1002180721251-10002",
        "coreBizOrderNo":"1002180721251",
        "bizOrderDomain":10002,
        "end":false,
        "sopId":null,
        "sopName":null,
        "nodeList":[
            {
                "nodeTag":"factor_a52s22ow0f40000_k6vmj48l_k6vq82tf_k6vqdlyq_k71p7inw_k72sa2wr",
                "nodeName":"是否联系上消费者（裹裹）",
                "nodeType":"simple_interactive_factor",
                "solutionType":null,
                "processKey":"1568023898166_b541",
                "inputParamMap":{
                    "e":{
                        "nodeResult":"yes"
                    },
                    "nodeResult":"yes",
                    "attributeMap":{
                        "attributeCode":"yes"
                    }
                },
                "outputParamMap":{
                },
                "display":true,
                "nodeTip":null,
                "status":"COMPLETED",
                "nodeData":null,
                "extFields":{
                }
            },


            {
                "nodeTag":"action_u70vtavgnnk000_k72sa2wr",
                "nodeName":"动作",
                "nodeType":"action",
                "solutionType":null,
                "processKey":"1568023898166_b541",
                "inputParamMap":{
                },
                "outputParamMap":{
                    "result":{
                        "branchResult":"Default"
                    },
                    "e":{
                        "result":{
                            "branchResult":"Default"
                        }
                    },
                    "nodeResult":[
                        "1"]
                },
                "display":true,
                "nodeTip":null,
                "status":"COMPLETED",
                "nodeData":[
                    {
                        "id":1,
                        "name":"外呼",
                        "code":"call",
                        "gmtCreate":1560914802000,
                        "gmtModified":1560914802000,
                        "desc":"外呼",
                        "actionCategory":1,
                        "responseCategory":1,
                        "preUrl":"https://www.google.com",
                        "onlineUrl":"https://www.google.com",
                        "feature":"testtest",
                        "creatorId":107345,
                        "creatorDomain":82,
                        "modifierId":107345,
                        "modifierDomain":82,
                        "tenantCode":"1"
                    }],
                "extFields":{
                }
            },


            {
                "nodeTag":"factor_6kdxq4ssf000000_k71s9cbx_k71se6qf_k72rtyko_k7kb4db5",
                "nodeName":"是否揽收",
                "nodeType":"simple_interactive_factor",
                "solutionType":null,
                "processKey":"1568023898166_b541",
                "inputParamMap":{
                },
                "outputParamMap":{
                    "result":{
                        "branchResult":"Default"
                    },
                    "s":{
                        "result":{
                            "branchResult":"Default"
                        }
                    },
                    "nodeResult":[
                        {
                            "id":7190,
                            "factorId":988,
                            "attributeValue":"no",
                            "attributeName":"否",
                            "attributeType":1,
                            "priority":2,
                            "status":1,
                            "tenantId":1,
                            "gmtModified":1569321904000,
                            "gmtCreate":1569321904000,
                            "creatorId":771261,
                            "creatorDomain":82,
                            "modifierId":771261,
                            "modifierDomain":82
                        },
                        {
                            "id":7189,
                            "factorId":988,
                            "attributeValue":"yes",
                            "attributeName":"是",
                            "attributeType":1,
                            "priority":1,
                            "status":1,
                            "tenantId":1,
                            "gmtModified":1569321898000,
                            "gmtCreate":1569321898000,
                            "creatorId":771261,
                            "creatorDomain":82,
                            "modifierId":771261,
                            "modifierDomain":82
                        }]
                },
                "display":true,
                "nodeTip":null,
                "status":"WAIT",
                "nodeData":[
                    {
                        "id":7190,
                        "factorId":988,
                        "attributeValue":"no",
                        "attributeName":"否",
                        "attributeType":1,
                        "priority":2,
                        "status":1,
                        "tenantId":1,
                        "gmtModified":1569321904000,
                        "gmtCreate":1569321904000,
                        "creatorId":771261,
                        "creatorDomain":82,
                        "modifierId":771261,
                        "modifierDomain":82
                    },
                    {
                        "id":7189,
                        "factorId":988,
                        "attributeValue":"yes",
                        "attributeName":"是",
                        "attributeType":1,
                        "priority":1,
                        "status":1,
                        "tenantId":1,
                        "gmtModified":1569321898000,
                        "gmtCreate":1569321898000,
                        "creatorId":771261,
                        "creatorDomain":82,
                        "modifierId":771261,
                        "modifierDomain":82
                    }],
                "extFields":{
                }
            }
            
        ],
        "parameters":{
            "lpCode":"LP00183127933252",
            "gaeaBizOrderCode":"LP00183127933252",
            "gaeaCaseId":1021694429990
        },
        "resumedPlay":null
    },
    "errorCode":null,
    "errorMsg":null
}
```
### 4.16 cdesk 

```json
{
    "functionCode":"addNewMessage",
    "functionConfigInput":"{\"memo\":\"测试留言\",\"attachList\":[],\"caseId\":\"1011733648012\",\"taskId\":\"1002239796652\"}"
}
```

```json
{
  "success": true,
  "data": true,
  "errorCode": null,
  "errorMsg": null
}
```
