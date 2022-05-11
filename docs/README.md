># 共享单车接口文档

## 🍋全局加密方式

为保证重要数据在传输过程中的保密性，系统采用 AES 对称加密机制保证数据传输安全。本文档每个接口都需要传 parentId 参数，用于辨别第三方身份。
1. 加/解密说明： 加/解密方式：AES-128-ECB； 填充方式：PKCS5Padding；
2. 请求参数/响应数据 data(json 字符串)作为待加/解密内容，合作密钥(secret)
3. AES 加/解密密钥，偏移量 iv 为空；
4. 采用 AES 加密方式传输时，请求/响应数据中诸如用户手机号、车牌号等隐私数 据不再需要单独处理。 合作密钥(secret)是双方数据交互的基础，第三方机构必须严格保护本平台的合作 密钥，不可被他人知晓，且严禁作为请求参数发送。若第三方机构相关开发人员变动， 则应及时联系我司更换。
   直接使用以下代码需要引入 hutool
5. Maven 在项目的pom.xml的dependencies中加入以下内容:

```java
<groupId>cn.hutool</groupId>
<artifactId>hutool-all</artifactId>
<version>5.5.2</version>
```

```java
SymmetricCrypto aes = new SymmetricCrypto(SymmetricAlgorithm.AES, 合作秘钥.getBytes());
String enBody = aes.decryptStr(securityRequest.getData());
```


>## 共享单车位置信息

接口地址: https://ifc1.yunpoint.cn/bikePush/getBikePositionList

### 🧅接入参数

| 字段名    | 数据类型   | 必填  | 描述   |
|--------|--------|-----|------|
| appkey | String | 是   | 认证标识 |
| data   | String | 是   | 加密参数 |

**加密参数**

| 字段名        | 数据类型    | 必填  | 描述                   |
|------------|---------|-----|----------------------|
| pageIndex  | Integer | 是   | 页码                   |
| pageSize   | Integer | 是   | 每页条数                 |
| company    | Integer | 是   | 公司类型(1.美团 2.青桔 3.哈啰) |
| createDate | String  | 是   | 日期                   |



### 🍰参数示例

```json
{
    "appKey": "eOmt3vRlSTnvuddZ",
    "data": "0d1e1a149bc0b77579e526434555bf5424fc7d1099065ba5a35e65c0f37f3cb0843f3a40b309569b06f9011699bd20b4e5c4c8c14ed7c44bc434d8b4d8e1f77c34ed811a9e5bbf73760cbddd7ecc5106"
}
```

**解密后data参数**

```json
{
"pageIndex": 2,
"pageSize": 12,
"company": 2,
"createDate": "2021-03-05"
}
```

### 🧋返回值

| 字段名     | 数据类型    | 描述           |
|---------|---------|--------------|
| msg     | String  | 接口回调详情描述     |
| result  | Json    | 共享单车位置信息分页列表 |
| code    | Integer | 状态码          |
| success | Boolean | 接口调用状态       |

### ☕返回示例

```json
{
    "msg": "操作成功",
    "result": "{\"msg\":\"操作成功\",\"code\":200,\"success\":true,\"page\":{\"count\":1843,\"list\":[{\"id\":\"58391be8156a47fb828c01258255c4b4\",\"createDate\":\"2022-05-11 20:34:14\",\"updateDate\":\"2022-05-11 20:34:14\",\"company\":\"1\",\"code\":\"8651615223\",\"address\":\"上海市浦东新区陆家嘴街道中国商飞大厦\",\"areaCode\":\"lujiazui\",\"longitude\":121.50923607362948,\"latitude\":31.224286928468747},{\"id\":\"8412c502dca543b6aff34380dd6b6ecd\",\"createDate\":\"2022-05-11 20:34:14\",\"updateDate\":\"2022-05-11 20:34:14\",\"company\":\"1\",\"code\":\"8652139607\",\"address\":\"上海市浦东新区陆家嘴街道浦东大道501弄\",\"areaCode\":\"lujiazui\",\"longitude\":121.51889299624001,\"latitude\":31.241481733746387},{\"id\":\"7cd8b17ed43a488a8c627997110196cd\",\"createDate\":\"2022-05-11 20:34:13\",\"updateDate\":\"2022-05-11 20:34:13\",\"company\":\"1\",\"code\":\"8640696735\",\"address\":\"上海市浦东新区陆家嘴街道世界金融大厦\",\"areaCode\":\"lujiazui\",\"longitude\":121.50867549377274,\"latitude\":31.238525156043096}]}}",
    "code": 200,
    "success": true
}
```
