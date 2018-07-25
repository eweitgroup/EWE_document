## Jerry-本地派送 推送订单API文档 

[TOC]

- **文档修改记录**

> 
| 版本      | 日期         | 修改人      | 备注   |
|:-------   |:-------       |:-------       |:-------|
|V 1.0	    |   06/11/2017 |	Bain Bai    |	第一期接口               |             
|V 1.5      |	06/12/2017 |    Bain Bai	|   第二期接口增加扣库存功能     |           
|V 2.0	    |   19/01/2018 |	Bain Bai    |	第三期接口增加ECI，默认发件人地址功能|
|V 2.1      |	20/01/2018 |	Bain Bai	|   增加中文品名|
|V 2.5      |	01/02/2018 |	Bain Bai	|   增加大宗货物Freight Service (原TNT)|
|V 2.6      |	05/03/2018 |	Liang Huang     |	增加Large Letter|
|V 2.7      |	13/03/2018 |	Liang Huang     |	增加Domestic EWE|
|V 2.8      |	21/03/2018 |	Liang Huang     |	增加单品长宽高|
|V 2.9      |	23/04/2018 |	Liang Huang	    |   修改TNT文案为Freight Service|
|V 3.0      |	18/07/2018 |	Liang Huang	    |   增加错误码|
|V 3.1      |	23/07/2018 |	Liang Huang	    |   文档改为md格式|



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
> [https://newomstest.ewe.com.au/eweApi/ewe/api/pushShipment](#)

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
| shipments  | array[shipments]    |  是     |见后|
| msgType  |  String    |  是     |为 pushUnCreatedShipments|
| version  |  String    |  否     |可填 3.1 |

- **Shipments**
>
| 字段      |     类型  |   是否必须   | 说明 |
| :-------- | :--------| :------ | :------ |
|sender_reference|	String|	是	|客户使用的自己的单号 |
|email_tracking_enabled	| Boolean	| 否	| 是否允许邮件追踪，当填写邮件追踪时，收件人邮件要必填，否则会失效可不填，不填以客户默认为准 |
|from	|from	|否	| 发件地址 注：如果用户设置了默认发件人地址，那么可以不推送发件人地址，系统会采用默认发件人地址。这样效率更高。推荐使用。设置默认发件人地址：联系jerry相关人员，或者登陆jerry系统进行设置|
|to	|to	                |是	    |接收地址
|items	| array[items]	| 是	|注意个数为1|

- **From**
>**对于每个客户来说，一般发件地址为固定提货点，如果是委托EWE进行dispatch的模式，那么应该是EWE的仓库地址，会由EWE提供。而如果由自己发货，则是自己发货的地址**


| 字段      |     类型  |   是否必须   | 说明 |
| :-------- | :--------| :------ | :------ |
|name	 |String	    |是	|不得超过40个字符|
|lines	 |Array<String>	|是	|最多三行，每行不得超过40个字符|
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
|lines	    | Array<String>	    | 是	| 最多三行，每行不得超过40个字符  |
|suburb	    | String	        | 是	| 地区,会对邮编和州和地区进行校验 |
|state	    | String	        | 是	| 州,会对邮编和州和地区进行校验   |
|postcode	| String	        | 是	| 邮编,会对邮编和州和地区进行校验 |
|phone	    | String	        | 否	| 澳洲电话标准 10-20长度，允许字符0123456789 -+() |
|email	    | String	        | 否	| 符合邮箱标准     |
|country	| String	        | 是	| EPARCEL填写AU对于ECI用户，填写相应的country code，country code 符合[ISO 3166-1 alpha-2 standard](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2).|


- **Items**
>
**allow_partial_delivery, authority_to_leave, safe_drop_enabled 这三个字段有着丰富的业务意义，鉴于此文档为API文档，所以不再这里赘述了使用Freight Service须知**
##### 使用Freight Service须知
*	<font face="宋体" size=3>一定使用jerry库存，</font>
*	<font face="宋体" size=3>订单全部信息要为英文，除了库存信息之外；</font>
*	<font face="宋体" size=3>重量最大限制为100KG（这个100KG是计费重量，以计费重量为标准，计费重量是以实际重量和体积重量  相比，取其大者；体积重Cubic weight = length(m)&times;width(m)&times;height(m)&times;250；</font>
*	<font face="宋体" size=3>包裹的单边长尺寸最大的不能超5m</font>
>
| 字段      |     类型  |   是否必须   | 说明 |
| :-------- | :--------| :------ | :------ |
|item_description|	String|	否|	物品描述，会显示在label上面|
|product_id|	String|	是|	填写charge code详细见附表1|
|length	|Double|	否|	长宽高单位为厘米，均不得超过105厘米，不得超过两个小于5cm，体积不得超过0.25m3|
|width|	Double|	否	|包裹宽|
|height|	Double|	否|包裹高|	
|weight|	Double|	是|	申报重量,重量上限详细见附表1|
|allow_partial_delivery	|Boolean|否|	可不填，不填以客户默认为准|
|authority_to_leave|	Boolean|否|可不填，不填以客户默认为准|
|safe_drop_enabled|	Boolean|否|	可不填，不填以客户默认为准|
|useStock|	Integer	|否|	使用库存填1不使用库存不填，或者填0|
|merchandise|	Array[merchandise]|	使用库存必填，使用ECI必填不然非必填|见后|

- **Merchandise**
**使用库存须知：**
*	barcode:商品货物的条码，每一种商品一个barcode，注意，即使同一种产品，新旧包装，其中barcode也是不同的。
*	sku:管理商品的维度，Jerry用sku管理货物，这样，换了barcode的物品就是同一种物品。
*	goodsName:商品名字
>
| 字段      |     类型  |   是否必须   | 说明 |
| :-------- | :--------| :------ | :------ |
|sku	|String	|否，|使用库存必填	|
|goodsName|	String|	推荐填写	|英文品名|
|goodsNameCn	|String	|推荐填写	|中文品名|
|barcode	|String	|否	|条形码,即UPC|
|description	|String|	否|选ECI为必填	物品描述，英文描述，通常为宽泛的产品描述，示例：milk， red wine 等|
|country_of_origin	|String	|否|	原产地
|quantity	|Integer	|使用库存必填,选ECI为必填	|个数
value	|Decimal	|选ECI为必填	            |单品价格
|length	|Decimal	|选 Freight Service为必填	|单品长度|
|width	|Decimal	|选 Freight Service为必填	|单品宽度|
|height	|Decimal	|选 Freight Service为必填	|单品高度|
|weight	|Decimal	|选 Freight Service为必填	|单品重量|



###  返回报文说明
> 
| 字段      |     类型  |   是否必须   | 说明 |
| :-------- | :--------| :------ | :------ |
| shipments	|array[shipments]	|否	|订单推送成功时才返回|
|errors|	array[errors]	|是|	当订单有错误时此字段不为空|
|code	|Integer|是	|见附表二|

- **Shipments**
>
| 字段      |     类型  |   是否必须   | 说明 |
| :-------- | :--------| :------ | :------ |
|sender_references	|array[shipments]	|是	|订单推送成功时才返回|
|items	|array[items]	|是	|订单推时才返回|
|msg	|String	|是|	提示信息|

- **Items**
>
| 字段      |     类型  |   是否必须   | 说明 |
| :-------- | :--------| :------ | :------ |
| article_id	| String	| 否	| 箱号| 
| consignment_id| 	String	| 否	| 订单号| 
| item_description| 	String	| 是	| 包裹描述信息| 
| pdf_url	| String	| 否	| 面单Label路径| 
- **Errors**
>
| 字段      |     类型  |   是否必须   | 说明 |
| :-------- | :--------| :------ | :------ |
|message	|String	|是|	错误信息|
|sender_reference	|String|	是	|客户单号|


### 签名说明
签名为常见的MD5签名+base64算法，对于字符串123456 ，
digest  = 用户名 + 订单数量 + msgType + 由EWE提供的API密码;
如:
digest  = test + 1 + pushUnCreatedShipments + 123456
签名结果为Hf+Gw8Yl5KPtn047o9yzwQ==
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
Password由EWE提供，示例json也有示范


### 示例JSON
>    
* **请求示例**
```json 
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
* **返回示例**

```json
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
