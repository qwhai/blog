
## 360
### 获取本机
接口地址：
```
http://ip.360.cn/IPShare/info
```
参数：无<br>
返回类型：json<br>
返回值：

| 字段        | 描述                                       |
|-------------|-------------------------------------------|
| greetheader | 提示语(如上午好、中午好等)                    |
| nickname    | 本机已登录的360账号                          |
| ip          | 本机IP地址                                  |
| location    | IP所对应的地理位置(中间会有“\t”分隔地区与运营商) |
| loc_client  | 作用不明                                    |
示例：<br>
```json
{
    "greetheader":"下午好,",
    "nickname":"",
    "ip":"121.237.143.229",
    "location":"江苏南京	电信",
    "loc_client":""
}
```

### 指定IP查询
接口地址：
```
http://ip.360.cn/IPQuery/ipquery
```
参数：ip 待查询IP<br>
返回类型：json<br>
返回值：

| 字段    | 描述                                           |
|--------|------------------------------------------------|
| errno  | 错误编号(为零则代表成功)                           |
| errmsg | 错误信息                                         |
| data   | 查询的IP所对应的地理位置(中间会有“\t”分隔地区与运营商) |

示例：
```json
{
    "errno":0,
    "errmsg":"",
    "data":"上海	电信/联通/移动"
}
```

## 淘宝

### 指定IP查询

接口地址：
```
http://ip.taobao.com/service/getIpInfo.php
```
参数：ip 待查询IP<br>
返回类型：json<br>
返回值：

| 字段       | 描述                                           |
|------------|------------------------------------------------|
| code       | 错误码(为零代表请求成功) |
| country    | 国名 |
| country_id | 国名(英文缩写) |
| area       | 地域(如：华东) |
| area_id    | 地域ID |
| region     | 行政区 |
| region_id  | 行政区ID |
| city       | 城市名 |
| city_id    | 城市ID |
| isp        | 网络提供商 |
| isp_id     | 网络提供商ID |
| ip         | 请求的IP地址 |

示例：
```json
{
    "code":0,
    "data":{
        "ip":"115.159.152.210",
        "country":"中国",
        "area":"",
        "region":"上海",
        "city":"上海",
        "county":"XX",
        "isp":"电信",
        "country_id":"CN",
        "area_id":"",
        "region_id":"310000",
        "city_id":"310100",
        "county_id":"xx",
        "isp_id":"100017"
    }
}
```

## 其他
```
http://ip-api.com/json/115.191.200.34?lang=zh-CN
```

## Ref
- https://cloud.tencent.com/developer/article/1152362
- https://www.cnblogs.com/huchong/p/9299875.html
