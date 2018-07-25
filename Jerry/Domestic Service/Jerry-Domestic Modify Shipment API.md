## Jerry-Domestic Modify Shipment API

[TOC]

- **Document modification record**

> 
| Version      | Date         | Author      | Comment   |
|:-------   |:-------       |:-------       |:-------|
|V 1.0	    |   04/18/2018 |	Liang Huang    |	Initial               |             
|V 1.5      |	06/27/2018 |    Liang Huang	|   Second version of API Merchandise added|       
|V 1.6	    |   23/07/2018 |	Liang Huang    |	Third version of API ECI added, default sender address added.|




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
*	**EParcel label will be about 100mm&times;150mm**
>
*	**ECI label will be of A4 size.**
>
*	**LARGE LETTERl will be about 100mm&times;90mm, DOMESTIC EWl will be about 100mm&times;150mm**
>
*	**EParcel label is generate in JERRY system, so there will be no time gap between lodging shipment and print label.**
>
*   **ECI label is generate in AU POST system, which may take some time, so we suggested you to wait for time, after you lodge your shipments. If you wait too long, you could contact JERRY.**

---
###  JSON data

- **Test Address**
> [https://newomstest.ewe.com.au/eweApi/ewe/api/modifyShipment](#)

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
| msgType  |  String    |  Mandatory     |Should be  modifyShipment|
| version  |  String    |  Optional     |Could be  1.6 |
| referenceNo  |  String    |  Optional     |The customer reference number(articleId or referenceNo not null) |
| articleId  |  String    |  Optional     |articleId (articleId or referenceNo not null) |
| reason  |  String    |  Mandatory     |resason |
| from      |  json    |  Mandatory     |Sender info |
| to        |  json    |  Mandatory     |Receiver info |
| merchandise  |  array[merchandise]    |  Optional     ||



- **From**


>
| Element name      |     Type  |   Optional/Mandatory   | Comments |
| :-------- | :--------| :------ | :------ |
|name	 |String	    |Mandatory	|No Longer than 40 words|
|lines	 |Array[String]	|Mandatory	|The address lines of the address. Minimum of one address line, up to a maximum of three. Each address line is a maximum of 40 characters.|
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
|lines	    | Array[String]	    | Mandatory	| The address lines of the address. Minimum of one address line, up to a maximum of three. Each address line is a maximum of 40 characters.  |
|suburb	    | String	        | Mandatory	| There will be a validation of these area |
|state	    | String	        | Mandatory	| There will be a validation of these area   |
|postcode	| String	        | Mandatory	| There will be a validation of these area |
|phone	    | String	        | Optional	|The phone number must be between 10 and 20 characters in length and the allowable characters are "()- 0-9" (digits, space, hyphen and parentheses.) Examples are 0491-570-156, 02) 5550 1234, +61 (3) 7010 4321. |
|email	    | String	        | Optional	| Receiver email address     |
|



- **Merchandise**
>**Use of stock can not be used**


>
| Element name      |     Type  |   Optional/Mandatory   | Comments |
| :-------- | :--------| :------ | :------ |
|sku	|String	|Optional 	|
|goodsName|	String|	Suggested	|Products name|
|goodsNameCn	|String	|Suggested	|Products name in Chinese|
|barcode	|String	|Optional	|UPC|
|description	|String|	Optional||
|country_of_origin	|String	|Optional|	Product origin country|
|quantity	|Integer	|Optional|quantity|
|value	|Decimal	|Optional   |Single item price|
|length	|Decimal	|Optional	|Single item length|
|width	|Decimal	|Optional	|Single item width|
|height	|Decimal	|Optional	|Single item height|
|weight	|Decimal	|Optional   |Single item weight|



### Response Data details
> 
| Element name      |     Type  |   Optional/Mandatory   | Comments |
| :-------- | :--------| :------ | :------ |
| success	|boolean	|Mandatory	|result|
|message|	array[errors]	|Mandatory|	message|
|code	|Integer|Mandatory	|	See appendix 2.|


### Digest algorithm
he algorithm is MD5 + base64 algorithm, for string 123456, 
the digest will be oNCmEhRuYMf62nPzLxGrjA==
example:
digest  = test + modifyShipment + 123456
```java
String content = username  +  auPostShipmentsToJerryDto.getMsgType();

String digest = MD5Util.MD5Encode(content, auCustomer.getPassword());
//content = testmodifyShipment
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
```json 
{
  "username": "test",
  "digest": "oNCmEhRuYMf62nPzLxGrjA==",
  "msgType": "modifyShipment",
  "version": "1.0",
  "articleId": "ABC200001901000935008",
  "referenceNo": "TEST88888",
  "from": {
    "name": "Sydney ",
    "phone": "0296442648",
    "suburb": "Chullora",
    "state": "NSW",
    "lines": [
      "haha"
    ],
    "postcode": "2190"
  },
  "to": {
    "name": "Tom Love Jerry",
    "phone": "0404280016",
    "suburb": "Croydon",
    "state": "NSW",
    "email": "123456@qq.com",
    "lines": [
      "xixi"
    ],
    "postcode": "2132"
  },
  "merchandise": [
    {
      "value": 5,
      "weight": 6,
      "quantity": 2,
      "barcode": "456132",
      "goodsName": "Unichi",
      "goodsNameCn": "Unichi玫瑰果精华美白胶囊",
      "sku": "uni",
      "description": "description"
    },
    {
      "value": "50",
      "weight": 2,
      "quantity": 1,
      "barcode": "9418783002851",
      "goodsName": "Aptamil",
      "goodsNameCn": "澳洲Aptamil爱他美4段",
      "sku": "Aptamil",
      "description": "description"
    }
  ]
}

```
* **The response example**

```json
{
    "success": true,
    "message": "Modify success!",
    "code": 0
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
#### API return code
|  Code      |     Error Description  | 
| :-------- | :--------|
|0	|Success|
|1	|Unknown error|
|2	|Service temporarily unavailable|
|3	|Unsupported openapi method|
|10	|Invalid parameter|
|101	|Invalid API key|
|104	|Incorrect signature|
