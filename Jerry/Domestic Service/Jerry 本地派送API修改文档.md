## Jerry-本地派送 修改订单API文档 

[TOC]

- **文档修改记录**

> 
| 版本      | 日期         | 修改人      | 备注   |
|:-------   |:-------       |:-------       |:-------|
|V 1.0	    |   06/11/2017 |	Liang Huang    |	第一期接口               |             
|V 1.5      |	06/12/2017 |    Liang Huang	  |   第二期接口增加修改物品功能     |           
|V 1.6      |	23/07/2018 |	Liang Huang	    |   文档改为md格式|



---


###  需知

*	**使用文档的客户在使用前，需已经在jerry系统注册**
>
*	**使用库存功能的客户，需有物品在jerry入库**
>
*	**EParcel产品，即澳洲邮政的本地件。**
>
*	**ECI产品，即澳洲邮政的国际件，产品信息全程由澳洲邮政提供。**
>
*	**EParcel产品面单为100mm&times;150mm大小的面单。**
>
*	**ECI的产品面单为A4纸张大小**
>
*	**LARGE LETTER产品面单为100mm&times;90mm, DOMESTIC EWE面单大小为100mm&times;150mm**
>
*	**EParcel的面单打印在Jerry系统中生成，所以没有时间延迟**
>
*   **ECI的面单有澳邮系统提供，所以有时间延迟，建议客户录入订单后，等待一段时间在获得。如果等待过长时间还没有面单，联系Jerry相关人员**

---
###  JSON格式的API接口

- **测试请求URL**
> [https://newomstest.ewe.com.au/eweApi/ewe/api/modifyShipment ](#)

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
| digest  |  String    |  是     |签名算法见后|
| msgType  |  String    |  是     |为 modifyShipment|
| version  |  String    |  否     |可填 1.6 |
| referenceNo  |String |  是 |客户单号(客户单号,箱号必填一个)|
| articleId  | String   |  是       |箱号(客户单号,箱号必填一个)|
| reason    | String    |  是        |修改原因|
| from      | json    |  是         |发货地址|
| to        | json   |  是          |接收地址|
| merchandise  | array[merchandise]    |  否    |物品信息|


- **From**

| 字段      |     类型  |   是否必须   | 说明 |
| :-------- | :--------| :------ | :------ |
|name	 |String	    |是	|不得超过40个字符|
|lines	 |Array[String]	|是	|最多三行，每行不得超过40个字符|
|suburb	 |String	    |是	|地区,会对邮编和州和地区进行校验|
|state	 |String	    |是	|州,会对邮编和州和地区进行校验|
|postcode|String	    |是	|邮编,会对邮编和州和地区进行校验|
|phone	 |String	    |否	|澳洲电话标准 10-20长度，允许字符0123456789 -+()|
|email	 |String	    |否	|符合邮箱标准|

- **To**
>
| 字段      |     类型  |   是否必须   | 说明 |
| :-------- | :--------| :------ | :------ |
|name	    | String	        | 是	| 不得超过40个字符 |
|lines	    | Array[String]	    | 是	| 最多三行，每行不得超过40个字符  |
|suburb	    | String	        | 是	| 地区,会对邮编和州和地区进行校验 |
|state	    | String	        | 是	| 州,会对邮编和州和地区进行校验   |
|postcode	| String	        | 是	| 邮编,会对邮编和州和地区进行校验 |
|phone	    | String	        | 否	| 澳洲电话标准 10-20长度，允许字符0123456789 -+() |
|email	    | String	        | 否	| 符合邮箱标准     |




- **Merchandise**
>**使用EWE库存的订单不能修改物品**
>
| 字段      |     类型  |   是否必须   | 说明 |
| :-------- | :--------| :------ | :------ |
|sku	|String	|否|	|
|goodsName|	String|	推荐填写	|英文品名|
|goodsNameCn	|String	|推荐填写	|中文品名|
|barcode	|String	|否	|条形码,即UPC|
|description	|String|	否|选ECI为必填	物品描述，英文描述，通常为宽泛的产品描述，示例：milk， red wine 等|
|country_of_origin	|String	|否|	原产地
|quantity	|Integer	|否|个数
value	|Decimal	|否 |单品价格
|length	|Decimal	|否|单品长度|
|width	|Decimal	|否	|单品宽度|
|height	|Decimal	|否	|单品高度|
|weight	|Decimal	|否	|单品重量|



###  返回报文说明
> 
| 字段      |     类型  |   是否必须   | 说明 |
| :-------- | :--------| :------ | :------ |
|success|	boolean	|是| 修改成功/失败|
|message|	boolean	|是| 提示信息|
|code	|Integer|是	|见附表二|



### 签名说明
签名为常见的MD5签名+base64算法，对于字符串123456 ，
digest  = 用户名  + msgType + 由EWE提供的API密码;
如:
digest  = test  + modifyShipment + 123456
签名结果为oNCmEhRuYMf62nPzLxGrjA==
```java
String content = username  + auPostShipmentsToJerryDto.getMsgType();

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
Password由EWE提供，示例json也有示范


### 示例JSON
>    
* **请求示例**
```json 
//API密码为123456
//示例JSON
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
* **返回示例**

```json
{
    "success": true,
    "message": "Modify success!",
    "code": 0
}

```

### 附表1
#### 注:
* **绿色为EParcel产品，即澳洲邮政的本地件。**
* **黄色产品为ECI产品，即澳洲邮政的国际件，产品信息全程由澳洲邮政提供。**
* **蓝色为Freight Service产品，即大宗货物，详细信息联系JERRY销售**
* **灰色为平信(Large Letter)**
* **红色为EWE本地派送DOMESTIC EWE**


>
| Charge Code      |     产品类型  |   返回运单号   | 最大重量(KG) |
| :-------- | :--------| :------ | :------ |
|<font  color=green>3D35</font>	|<font  color=green>PARCEL POST + SIGNATURE</font>	|是	|22|
|<font  color=green>3J35</font>	|<font  color=green>EXPRESS POST + SIGNATURE	</font>|是	|22|
|<font  color=#ffcc66>ECM8</font>	|<font  color=#ffcc66>INTL EXPRESS MERCH/ECI MERCH</font>	|否|	20|
|<font  color=#ffcc66>PTI8</font>	|<font  color=#ffcc66>INTL STANDARD/PACK & TRACK	|否</font>	|22|
|<font  color=#ffcc66>ECD8</font>	|<font  color=#ffcc66>INTL EXPRESS DOCS/ECI DOCS	|否</font>	|2|
|<font  color=#ffcc66>RPI8</font>	|<font  color=#ffcc66>INTL ECONOMY W SOD/ REGD POST</font>|	否|	2|
|<font  color=blue>999998</font>	|<font  color=blue>Freight Service	</font>|否	|100|
|<font  color=gray>999997</font>	|<font  color=gray>LARGE LETTER	</font>|否	|22|
|<font  color=#FF4500>999996</font>	|<font  color=#FF4500>DOMESTIC EWE	</font>|否	|∞|

### 附表2
#### API返回状态码
| Charge Code      |     产品类型  | 
| :-------- | :--------|
|0	|成功|
|1	|未知错误|
|2	|服务暂不可用|
|3	|未知的方法|
|10	|请求参数无效|
|101	|api key无效|
|104	|无效签名|
