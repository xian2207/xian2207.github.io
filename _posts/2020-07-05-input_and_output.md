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