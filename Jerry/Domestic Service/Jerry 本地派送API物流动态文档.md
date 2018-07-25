## Jerry-本地派送 订单追踪API文档 

[TOC]

- **文档修改记录**

> 
| 版本      | 日期         | 修改人      | 备注   |
|:-------   |:-------       |:-------       |:-------|
|V 1.0	    |   07/23/2017 |	Liang Huang    |	第一期接口               |    



---


###  需知

*	**使用文档的客户在使用前，需已经在jerry系统注册**
>
*	**已推送相应的订单**

---
###  JSON格式的API接口

- **测试请求URL**
> [https://newomstest.ewe.com.au/eweApi/ewe/aupost/trackArticle ](#)

- **生产请求URL**
> **客户通过测试后，由EWE提供账号名和密码**

- **请求方式** 
>**POST**

- **Content-Type** 
>**application/javascript**

---
###  推送报文说明




><font color=red>**所有报文必须为英文**</font> 

| 字段      |     类型  |   是否必须   | 说明 |
| :-------- | :--------| :------ | :------ |
| username|  String    |  是     | EWE提供 |
| referenceNo  |String |  是 |客户单号|
| timezone  |String |  否 |时区,默认为:Australia/Sydney|





### 示例JSON
>    
* **请求示例**
```json 
{
  "username":"test",
  "sender_reference":[
    "33D7E200120601000935001",
    "sss"
    ]
}


```
* **返回示例**

```json
{
    "tracking_results": [
        {
            "tracking_id": "33D7E200120601000935001",
            "status": "In transit",
            "trackable_items": [
                {
                    "article_id": "33D7E200120601000935001",
                    "product_type": "Parcel Post",
                    "events": [
                        {
                            "description": "Shipping information approved by Australia Post",
                            "date": "2018-06-18 18:30:12"
                        },
                        {
                            "description": "Shipping information received by Australia Post",
                            "date": "2018-06-18 15:41:14"
                        }
                    ],
                    "status": "In transit"
                }
            ]
        },
        {
            "tracking_id": "sss",
            "errors": [
                {
                    "code": "ESB-10001"
                }
            ]
        }
    ]
}


```

