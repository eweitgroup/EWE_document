## Jerry-Domestic Cancel Shipment API

[TOC]

- **Document modification record**

> 
| Version      | Date         | Author      | Comment   |
|:-------   |:-------       |:-------       |:-------|
|V 1.0	    |   06/11/2017 |	Liang Huang	     |	Initial               |             
|V 1.5      |	18/07/2018 |	Liang Huang	    | Error message code added|
|V 1.6      |	23/07/2018 |	Liang Huang	    | xlsx document to markdown|


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
> [https://newomstest.ewe.com.au/eweApi/ewe/api/cancelShipment](#)

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
| msgType  |  String    |  Mandatory     |Should be  cancelShipment|
| version  |  String    |  Optional     |Could be  1.6 |
| referenceNo  |  String    |  Mandatory     |The customer reference number |
| reason  |  String    |  Mandatory     |reason |




### Response Data details
> 
| Element name      |     Type  |   Optional/Mandatory   | Comments |
| :-------- | :--------| :------ | :------ |
| shipments	|array[shipments]	|Optional	||
|errors|	array[errors]	|Mandatory|	errors|
|code	|Integer|Mandatory	|	See appendix 2.|




### Digest algorithm
he algorithm is MD5 + base64 algorithm, for string 123456, 
the digest will be 1yL8YBCsG585SXnKLyLuSg==
example:
digest  = test + cancelShipment + 123456
```java
String content = username  + 
auPostShipmentsToJerryDto.getMsgType();

String digest = MD5Util.MD5Encode(content, auCustomer.getPassword());
//content = testcancelShipment
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
//API Password:123456
//Example JSON data
{
 "username":"test",
 "digest":"1yL8YBCsG585SXnKLyLuSg==",
 "msgType":"cancelShipment",
 "version":"1.0",
 "articleId":"ABC200001701000935004",
 "referenceNo":"TEST88888",
 "reason":"测试取消"
}


```
* **The response example**

```json
{
"success": true,
"message": "Shipment canceled!",
"code":0
}


```

### Appendix 1
#### Note:
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
