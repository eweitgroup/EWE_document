##Jerry-Domestic Tracking API

[TOC]

- **Document modification record**

> 
| Version      | Date         | Author      | Comment   |
|:-------   |:-------       |:-------       |:-------|
|V 1.0	    |   07/23/2018 |	Liang Huang    |	Initial               |             





---


###  Requirements

*	**The customer who use stock control should lodge their product into Jerry system.**
>
*	**Jerry system has related orders.**


---
###  JSON data

- **Test Address**
> [https://newomstest.ewe.com.au/eweApi/ewe/aupost/trackArticle](#)

- **Live Address**
> **Jerry will provide the username and key after the customer go through all the test API**

- **Request method** 
>**POST**

- **Header content-type** 
>**application/javascript**

---
### Request Data details

><font color=red>**All the data must be English.**</font> 

| Element name      |     Type  |   Optional/Mandatory   | Comments |
| :-------- | :--------| :------ | :------ |
| username|  String    |  Mandatory     |This will provided by EWE |
|sender_reference|array[String]|Mandatory|The customer reference number|
|timezone|String|Optional|time zone default:Australia/Sydney|




### JSON
>    
* **The request example**
```json 
{
  "username":"test",
  "sender_reference":[
    "33D7E200120601000935001",
    "sss"
    ]
}



```
* **The response example**

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


