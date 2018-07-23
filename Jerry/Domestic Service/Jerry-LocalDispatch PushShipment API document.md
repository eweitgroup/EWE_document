##Jerry-LocalDispatch PushShipment API document

[TOC]

- **Document modification record**

> 
| Version      | Date         | Author      | Comment   |
|:-------   |:-------       |:-------       |:-------|
|V 1.0	    |   06/11/2017 |	Bain Bai    |	Initial               |             
|V 1.5      |	06/12/2017 |    Bain Bai	|   Second version of API Stock control added|       
|V 2.0	    |   19/01/2018 |	Bain Bai    |	Third version of API ECI added, default sender address added.|
|V 2.1      |	20/01/2018 |	Bain Bai	|  Add Chinese products name|
|V 2.5      |	01/02/2018 |	Bain Bai	|   Add product TNT|
|V 2.6      |	05/03/2018 |	Liang Huang     |Add product Large Letter|
|V 2.7      |	13/03/2018 |	Liang Huang     |	Add product Domestic EWE|
|V 2.8      |	21/03/2018 |	Liang Huang     |Add Single item length , width ,height|
|V 2.9      |	23/04/2018 |	Liang Huang	    |  Modify the TNT name to be Freight Service|
|V 3.0      |	18/07/2018 |	Liang Huang	    |  Increase return code|



---


###  Requirements

*	**The customer should register in Jerry system before using this API**
>
*	**The customer who use stock control should lodge their product into Jerry system.**
>
*	**EParcel product, which is AU POST domestic service.**
>
*	**ECI product, which is AU POST international service. All info will be transfer to AUPOST**
>
*	**EParcel label will be about 100mm*150mm**
>
*	**ECI label will be of A4 size.**
>
*	**LARGE LETTERl will be about 100mm*90mm, DOMESTIC EWl will be about 100mm*150mm**
>
*	**EParcel label is generate in JERRY system, so there will be no time gap between lodging shipment and print label.**
>
*   **ECI label is generate in AU POST system, which may take some time, so we suggested you to wait for time, after you lodge your shipments. If you wait too long, you could contact JERRY.**

---
###  JSON data

- **Test Address**
> [https://newomstest.ewe.com.au/eweApi/ewe/api/pushShipment](#)

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
| digest  |  String    |  Mandatory     |The digest algorithm will be shown below|
| shipments  | array[shipments]    |  Mandatory     |See below|
| msgType  |  String    |  Mandatory     |Should be  pushUnCreatedShipments|
| version  |  String    |  Optional     |Could be  3.0 |

- **Shipments**
>
| Element name      |     Type  |   Optional/Mandatory   | Comments |
| :-------- | :--------| :------ | :------ |
|sender_reference|	String|	Mandatory	|The customer reference number |
|email_tracking_enabled	| Boolean	| Optional	| 	Whether enable AuPost to send email to notice the receiver<br/>Note: when this area is set to true, the receiver email must be provided or else it will not work<br/>It is optional, if it is not set, it will use the user default setting.|
|from	|from	|Optional	| If the customer has already set the default sender address, when the customer does not send the sender address info, the system will automatically add the default one. Which will improve the efficiency. <br/>To set default address, the customer can either log in Jerry system to set, or contact with the Jerry person.|
|to	|to	                |Mandatory	    |Receiver info|
|items	| array[items]	| Mandatory	|The item size must be 1|

- **From**
>**For customer, usually the sender info is a static address:<br/>If EWE do the dispatch work, the sender address should be EWE warehouse address, provided by EWE.<br/>	If the customer do the dispatch work, the sender address will be the customer address.**

>
| Element name      |     Type  |   Optional/Mandatory   | Comments |
| :-------- | :--------| :------ | :------ |
|name	 |String	    |Mandatory	|No Longer than 40 words|
|lines	 |Array<String>	|Mandatory	|The address lines of the address. Minimum of one address line, up to a maximum of three. Each address line is a maximum of 40 characters.|
|suburb	 |String	    |Mandatory	|There will be a validation of these area|
|state	 |String	    |Mandatory	|There will be a validation of these area|
|postcode|String	    |Mandatory	|There will be a validation of these area|
|phone	 |String	    |Mandatory	|The phone number must be between 10 and 20 characters in length and the allowable characters are "()- 0-9" (digits, space, hyphen and parentheses.) Examples are 0491-570-156, 02) 5550 1234, +61 (3) 7010 4321.|
|email	 |String	    |Optional	|Sender email address|

- **To**
>
| Element name      |     Type  |   Optional/Mandatory   | Comments |
| :-------- | :--------| :------ | :------ |
|name	    | String	        | Mandatory	| No Longer than 40 words |
|lines	    | Array<String>	    | Mandatory	| The address lines of the address. Minimum of one address line, up to a maximum of three. Each address line is a maximum of 40 characters.  |
|suburb	    | String	        | Mandatory	| There will be a validation of these area |
|state	    | String	        | Mandatory	| There will be a validation of these area   |
|postcode	| String	        | Mandatory	| There will be a validation of these area |
|phone	    | String	        | Optional	|The phone number must be between 10 and 20 characters in length and the allowable characters are "()- 0-9" (digits, space, hyphen and parentheses.) Examples are 0491-570-156, 02) 5550 1234, +61 (3) 7010 4321. |
|email	    | String	        | Optional	| Receiver email address     |
|country	| String	        | Mandatory	| For EPARCEL. Please set AU<br/>For ECI, The country code for the address. The country code must conform to ISO 3166-1 alpha-2 standard.|


- **Items**
>
**For Freight Service product the usestock field should be 1**
>
| Element name      |     Type  |   Optional/Mandatory   | Comments |
| :-------- | :--------| :------ | :------ |
|item_description|	String|	Optional|	Item description, it will appear on the label|
|product_id|	String|	Mandatory|See appendix 1.|
|length	|Double|	Optional|in centimeters. Each should not be over 105 cm, every two should not be less than 5cm, the volume should not be over 0.25m3|
|width|	Double|	Optional	|width|
|height|	Double|	Optional|height|	
|weight|	Double|	Mandatory|	weight,See appendix 1.|
|authority_to_leave|	Boolean	|Optional|	Whether the item can be left in a safe place on delivery without receiving a signature. This is optional for domestic shipments (and defaults to true), but must not be specified for international shipments. This field works with the safe_drop_enabled field. Optional，if the API does not provide the field, it will be set as user default setting|
|safe_drop_enabled|	Boolean|	Optional|	Whether your customer can request that the item be left in a safe place at the delivery address if they are not home to sign for their delivery. Your customer can request this while the item is on its way. This field is optional for domestic shipments (and defaults to true), but must not be specified for international shipments. This field works with the authority_to_leave field. If the authority_to_leave field is set to true, the safe_drop_enabled field is ignored. Optional，if the API does not provide the field, it will be set as user default setting|
|useStock|	Integer	|Optional|	If the customer use stock , set this area to 1If the customer don’t use stock, you can ignore this area or set to 0|
|merchandise|	Array[merchandise]|	Optional for not using stock,Mandatory for using stock,Mandatory for using ECI|See below|

- **Merchandise**
**Basic information about stock control:**
*	Barcode: The product barcode，one item for one barcode，Note: for one product the old version the new version will have two different barcode.
*	Sku: That’s why Jerry use sku to organize all the products，so that the different version of products will count as one product.
*	goodsName: the product name Basic info when using ECI:
*   Product Type，except EPARCEL and EXPRESS, the rest are international, product the 

>
| Element name      |     Type  |   Optional/Mandatory   | Comments |
| :-------- | :--------| :------ | :------ |
|sku	|String	|Optional for not using stock, Mandatory for using stock	|
|goodsName|	String|	Suggested	|Products name|
|goodsNameCn	|String	|Suggested	|Products name in Chinese|
|barcode	|String	|Optional	|UPC|
|description	|String|	Optional|Mandatory for ECI|General item description, example milk, red wine|
|country_of_origin	|String	|Optional|	Product origin country|
|quantity	|Integer	|Optional for not using stock, Mandatory for stock Mandatory for ECI	|quantity|
|value	|Decimal	|Optional Mandatory for ECI	            |Single item price|
|length	|Decimal	|Optional Mandatory for Freight Service	|Single item length|
|width	|Decimal	|Optional Mandatory for Freight Service	|Single item width|
|height	|Decimal	|Optional Mandatory for Freight Service	|Single item height|
|weight	|Decimal	|Optional Mandatory for Freight Service	|Single item weight|



### Response Data details
> 
| Element name      |     Type  |   Optional/Mandatory   | Comments |
| :-------- | :--------| :------ | :------ |
| shipments	|array[shipments]	|Optional	||
|errors|	array[errors]	|是Mandatory|	errors|
|code	|Integer|Mandatory	|	See appendix 2.|

- **Shipments**
>
| Element name      |     Type  |   Optional/Mandatory   | Comments |
| :-------- | :--------| :------ | :------ |
|sender_references	|array[shipments]	|Mandatory	|	The customer reference number|
|items	|array[items]	|Mandatory	||
|msg	|String	|Mandatory|	message|

- **Items**
>
| Element name      |     Type  |   Optional/Mandatory   | Comments |
| :-------- | :--------| :------ | :------ |
| article_id	| String	| Optional	| article id| 
| consignment_id| 	String	| Optional	| consignment id| 
| item_description| 	String	| Mandatory	| Item description| 
| pdf_url	| String	| Optional	| Label URL| 
- **Errors**
>
| Element name      |     Type  |   Optional/Mandatory   | Comments |
| :-------- | :--------| :------ | :------ |
|message	|String	|Mandatory|	Errors|
|sender_reference	|String|	Mandatory	|The customer reference number|


### Digest algorithm
he algorithm is MD5 + base64 algorithm, for string 123456, 
the digest will be Hf+Gw8Yl5KPtn047o9yzwQ==
example:
digest  = test + 1 + pushUnCreatedShipments + 123456
```java
String content = username  + auPostShipmentsToJerryDto.getShipments().size() + auPostShipmentsToJerryDto.getMsgType();

String digest = MD5Util.MD5Encode(content, auCustomer.getPassword());
//content = test1pushUnCreatedShipments
//keys = 123456
public static String MD5Encode(String content, String keys) {
    String sign = "";
    String charset = "UTF-8";
     content = content + keys;
     try {	
        MessageDigest md = MessageDigest.getInstance("MD5");
         md.update(content.getBytes(charset));
         sign = new String(Base64.encodeBase64(md.digest()), charset);
    } catch (Exception e) {
         throw new RuntimeException(e);
    }
    return sign;
}

```
EWE will provide the key and the test is shown in the example below.


### JSON
>    
* **The request example**
```java 
//API密码为123456
//示例JSON
{
	"username":"test",
	"digest":"Hf+Gw8Yl5KPtn047o9yzwQ==",
	"msgType":"pushUnCreatedShipments",
	"version":"1.0",
	"shipments":[{
		"sender_reference":"124",
		"from":{
			"name":"Sydney ",
			"phone":"0296442648",
			"suburb":"Chullora",
			"state":"NSW",
			"lines":["haha"],
			"postcode":"2190"
		},
		"to":{
			"name":"MengyangBai",
			"phone":"0404280016",
			"suburb":"Croydon",
			"state":"NSW",
			"lines":["haha"],
			"postcode":"2132"		
		},
		"items":[{
			"item_description":"123",
			"product_id":"3D35",
			"weight":1.25,
			"length":5,
            "merchandise": [
            {
              "sku": "N004-1",
              "goodsName": "milk",
              "goodsNameCn": "中文",
              "description": "milk",
              "quantity": "1",
              "value": 0.8
            }
          ]
		}]
	}]
}

```
* **The response example**

```java
{
    "shipments": [
        {
            "isSuccess": true,
            "items": [
                {
                    "article_id": "ABC200000001000931503",
                    "consignment_id": "ABC2000000",
                    "item_description": "123",
                    "pdf_url": "https://newomstest.ewe.com.au/eweApi/ewe/aupost/printSingleLabel?articleId=ABC200000001000931503"
                }
            ],
            "msg": "推送成功"
        }
    ],
"errors": [],
"code":0
}

```

### Appendix 1
####Note:
* **The green is EParcel product, which is AU POST domestic service.**
* **The yellow is ECI product, which is AU POST international service. All info will be transfer to AU POST**
* **The blue is Freight Service product**
* **The gray is LARGE LETTER product**
* **The red is DOMESTIC EWE product**


>
| Charge Code      |     Product Type  |   Return to the waybill number   | Max Weight(KG) |
| :-------- | :--------| :------ | :------ |
|<font  color=green>3D35</font>	|<font  color=green>PARCEL POST + SIGNATURE</font>	|Yes	|22|
|<font  color=green>3J35</font>	|<font  color=green>EXPRESS POST + SIGNATURE	</font>|Yes	|22|
|<font  color=#ffcc66>ECM8</font>	|<font  color=#ffcc66>INTL EXPRESS MERCH/ECI MERCH</font>	|No|	20|
|<font  color=#ffcc66>PTI8</font>	|<font  color=#ffcc66>INTL STANDARD/PACK & TRACK	|No</font>	|22|
|<font  color=#ffcc66>ECD8</font>	|<font  color=#ffcc66>INTL EXPRESS DOCS/ECI DOCS	|No</font>	|2|
|<font  color=#ffcc66>RPI8</font>	|<font  color=#ffcc66>INTL ECONOMY W SOD/ REGD POST</font>|	No|	2|
|<font  color=blue>999998</font>	|<font  color=blue>Freight Service	</font>|No	|100|
|<font  color=gray>999997</font>	|<font  color=gray>LARGE LETTER	</font>|No	|22|
|<font  color=#FF4500>999996</font>	|<font  color=#FF4500>DOMESTIC EWE	</font>|No	|∞|

### Appendix 2
####API return code
|  Code      |     Error Description  | 
| :-------- | :--------|
|0	|Success|
|1	|Unknown error|
|2	|Service temporarily unavailable|
|3	|Unsupported openapi method|
|10	|Invalid parameter|
|101	|Invalid API key|
|104	|Incorrect signature|
