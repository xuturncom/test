# BYUAN.Vip API Document

- - - - - -

## API 文档简介

欢迎使用BYUANVIP API，API分为[公共接口](#公共接口)和[私有接口](#私有接口)，你可以使用公共接口查询行情数据，并使用私有接口进行交易。


### 公共接口

**接口列表总览：**
- 1.1 [获取交易对](https://github.com/byuanvip/api/wiki/1.1-%E8%8E%B7%E5%8F%96%E4%BA%A4%E6%98%93%E5%AF%B9)
- 1.2 [获取行情](https://github.com/byuanvip/api/wiki/1.2-%E8%8E%B7%E5%8F%96%E8%A1%8C%E6%83%85)
- 1.3 [获取深度](https://github.com/byuanvip/api/wiki/1.3-%E8%8E%B7%E5%8F%96%E6%B7%B1%E5%BA%A6)
- 1.4 [获取历史成交数据](https://github.com/byuanvip/api/wiki/1.4-%E8%8E%B7%E5%8F%96%E5%8E%86%E5%8F%B2%E6%88%90%E4%BA%A4%E6%95%B0%E6%8D%AE)

### 私有接口

对于私有接口，为了防止请求在网络传输过程中被篡改，您需要使用您的secretkey做签名认证，以保证我们收到的请求为您本人发出。
对于secretkey的获取需登录[网站](https://www.byuan.vip/account/api)获得。

**签名认证算法**

签名生成规则如下：参与签名的字段包括accesskey(通过网站获取与secretkey一样),time（毫秒级时间戳）, 请求的所有参数。对所有待签名参数（务必先全部小写）按照字段名的ASCII 码从小到大排序（字典序）后，使用键值对的格式（即key1=value1&key2=value2…）拼接成字符串string。这里需要注意的是所有参数名均为小写字符。对string作HMAC-SHA1签名得到string1，然后使用base64编码，最后把它当成Authorization头的值放在header进行传输。（参考阿里云的[oss签名方式](https://help.aliyun.com/document_detail/31951.html)）.

示例：   
请求` 下单 `接口，自有的API对有：   
accesskey = "d5546d8c-c3a8-49a4-9130-8531b1049847"   
secretkey = "28b8e42848cbd317520bb899083c8938f0ee3351"   
那么：   
请求的下单参数如下：
```
accesskey=d5546d8c-c3a8-49a4-9130-8531b1049847
number=1.2
makret=btc_usdt
time=1562600365426
price=10567.89
type=1
```

步骤1. 对所有待签名参数先转成全部小写再按照字段名的ASCII 码从小到大排序（字典序）后，使用URL键值对的格式（即key1=value1&key2=value2…）拼接成字符串string：
```
accesskey=d5546d8c-c3a8-49a4-9130-8531b1049847&market=btc_usdt&number=1.2&price=10567.89&time=1562600365426&type=1
```

步骤2. 对string用secretkey进行HMAC-SHA1签名（使用secretkey做密钥），得到字节数组：

```
字节数组无法打印，如果需要打印的话，可以转成16进制的的小写字符串如下：
e29b71668981574ab8be315b253de5ecb39e0c9a
```

步骤3. 对字节数组（不是上面的16进制小写字符串，如果传入是16进制的小写字符串，务必先转成字节数组）进行base64编码等到signature：
```
4ptxZomBV0q4vjFbJT3l7LOeDJo=
```

步骤4. 把Authorization的键值对放在header传输：
Authorization = signature

最终效果是长这样：
```
POST /api/open/v1/orders HTTP/1.1
Host: tradeapi.byuan.vip
Authorization: 4ptxZomBV0q4vjFbJT3l7LOeDJo=
Content-Type: application/json

{
 "accesskey": "d5546d8c-c3a8-49a4-9130-8531b1049847",
 "number": "1.2",
 "market": "btc_usdt",
 "time": "1562600365426",
 "price": "10567.89",
 "type": "1"
}
```

**注意事项**

- 1.time为毫秒级时间戳，而且不能与服务器时间相差超过1分钟。
- 2.参与签名的字段，务必先转成小写，再进行字典排序。
- 3.签名的字符串必须为` UTF-8 `编码格式。含有中文字符的签名字符串必须先进行` UTF-8 `编码，再与 accesskey计算最终签名。
- 4.POST提交数据的编码方式为：application/json(即是Content—Type: application/json)。
- 5.客户端可以随机加上一些键值对，有助于防止泄露SecretKey的概率，但是参与的话，都需要验签，同时，不能超过20个键值对。

**接口列表总览：**
- 2.1 [获取资产信息](https://github.com/byuanvip/api/wiki/2.1-%E8%8E%B7%E5%8F%96%E8%B5%84%E4%BA%A7%E4%BF%A1%E6%81%AF)
- 2.2 [下单](https://github.com/byuanvip/api/wiki/2.2-%E4%B8%8B%E5%8D%95)
- 2.3 [获取委托单](https://github.com/byuanvip/api/wiki/2.3-%E8%8E%B7%E5%8F%96%E5%B8%82%E5%9C%BA%E5%8D%95%E4%B8%AA%E5%A7%94%E6%89%98%E5%8D%95)
- 2.4 [获取批量委托单](https://github.com/byuanvip/api/wiki/2.4-%E8%8E%B7%E5%8F%96%E5%B8%82%E5%9C%BA%E6%89%B9%E9%87%8F%E5%A7%94%E6%89%98%E5%8D%95)
- 2.5 [取消委托单](https://github.com/byuanvip/api/wiki/2.5-%E5%8F%96%E6%B6%88%E5%A7%94%E6%89%98%E5%8D%95)
- 2.6 [获取充值地址](https://github.com/byuanvip/api/wiki/2.6-%E8%8E%B7%E5%8F%96%E5%85%85%E5%80%BC%E5%9C%B0%E5%9D%80)
- 2.7 [获取充值记录](https://github.com/byuanvip/api/wiki/2.7-%E8%8E%B7%E5%8F%96%E5%85%85%E5%80%BC%E8%AE%B0%E5%BD%95)
- 2.8 [获取提现地址](https://github.com/byuanvip/api/wiki/2.8-%E8%8E%B7%E5%8F%96%E6%8F%90%E7%8E%B0%E5%9C%B0%E5%9D%80)
- 2.9 [获取提现记录](https://github.com/byuanvip/api/wiki/2.9-%E8%8E%B7%E5%8F%96%E6%8F%90%E7%8E%B0%E8%AE%B0%E5%BD%95)
- 2.10 [提现](https://github.com/byuanvip/api/wiki/2.10-%E6%8F%90%E7%8E%B0)