# 目录

--------------------------------------------------------------------------------

# 需求说明
# 调研流程
## 有道智云
xxx
xxx
```json
{
    "tSpeakUrl":"http://openapi.youdao.com/ttsapi?q=%E4%BD%A0%E5%A5%BD%EF%BC%8C%E4%B8%96%E7%95%8C&langType=zh-CHS&sign=B226F5B75C9AA9AA435B9F43053BE988&salt=1531827523160&voice=4&format=mp3&appKey=06a49965a7fa9491",
    "query":"Hello World.",
    "translation":[
        "你好，世界"
    ],
    "errorCode":"0",
    "dict":{
        "url":"yddict://m.youdao.com/dict?le=eng&q=Hello+World."
    },
    "webdict":{
        "url":"http://m.youdao.com/dict?le=eng&q=Hello+World."
    },
    "l":"EN2zh-CHS",
    "speakUrl":"http://openapi.youdao.com/ttsapi?q=Hello+World.&langType=en&sign=FDC3134C6BA032C582338B4CF6BDDC0F&salt=1531827523160&voice=4&format=mp3&appKey=06a49965a7fa9491"
}
```

## 金山词霸
xxx
xxx
```json
{
    "word_name":"hello",
    "is_CRI":1,
    "exchange":{
        "word_pl":[
            "hellos"
        ],
        "word_past":"",
        "word_done":"",
        "word_ing":"",
        "word_third":"",
        "word_er":"",
        "word_est":""
    },
    "symbols":[
        {
            "ph_en":"hə'ləʊ",
            "ph_am":"həˈloʊ",
            "ph_other":"",
            "ph_en_mp3":"",
            "ph_am_mp3":"http://res.iciba.com/resource/amp3/1/0/5d/41/5d41402abc4b2a76b9719d911017c592.mp3",
            "ph_tts_mp3":"http://res-tts.iciba.com/5/d/4/5d41402abc4b2a76b9719d911017c592.mp3",
            "parts":[
                {
                    "part":"int.",
                    "means":[
                        "哈喽，喂",
                        "你好，您好",
                        "表示问候",
                        "打招呼"
                    ]
                },
                {
                    "part":"n.",
                    "means":[
                        "“喂”的招呼声或问候声"
                    ]
                },
                {
                    "part":"vi.",
                    "means":[
                        "喊“喂”"
                    ]
                }
            ]
        }
    ],
    "items":[
        ""
    ]
}
```

## 百度翻译
xxx

xxx
```json
{
    "from":"en",
    "to":"zh",
    "trans_result":[
        {
            "src":"Hello World",
            "dst":"你好世界"
        }
    ]
}
```

## 必应
关于必应由于缺少需要的认证信息，这里只提供相关链接入口，如下：

[https://www.microsoft.com/en-us/translator/translatorapi.aspx](https://www.microsoft.com/en-us/translator/translatorapi.aspx)

## 各平台N维度比较
### 1. 接口模式
以下在线翻译API均可通过HTTP接口进行接口接入。

### 2. key申请流程

### 3. 语言支持范围

**有道智云**一共支持 8 种语言的互译：

语言|代码
---|-----
中文|zh-CHS
日文|ja
英文|EN
韩文|ko
法文|fr
俄文|ru
葡萄牙文|pt
西班牙文|es

**金山词霸**目前只支持从英文翻译成中文的简单（单词）并单向翻译功能。

**百度翻译**一共支持 28 种语言的互译：

语言|代码|语言|代码
---|----|----|----
中文|zh|繁体中文|cht
意大利语|it|荷兰语|nl
英语|en|保加利亚语|bul
粤语|yue|爱沙尼亚语|est
文言文|wyw|丹麦语|dan
日语|jp|芬兰语|fin
韩语|kor|捷克语|cs
法语|fra|罗马尼亚语|rom
西班牙语|spa|斯洛文尼亚语|slo
泰语|th|瑞典语|swe
阿拉伯语|ara|匈牙利语|hu
俄语|ru|波兰语|pl
葡萄牙语|pt|越南语|vie
德语|de|希腊语|el

若以`auto`为代码标识，则表明是自动检测。

### 4. 准确性

准确性这里以 5 组互译来说明。

第一组：
```
原文：今天天气很不错。
译文：
    有道智云    It's a nice day today.
    金山词霸    /
    百度翻译    It's a nice day today.
```

第二组：
```
原文：这波操作666。
译文：
    有道智云    This wave operation is 666.
    金山词霸    /
    百度翻译    This wave operates 666.
```

第三组：
```
原文：下河道有眼。
译文：
    有道智云    There are eyes in the lower channel.
    金山词霸    /
    百度翻译    There is an eye in the river.
```

第四组：
```
原文：Positive people are lucky people, they can see the roses while others see only the thorns.
译文：
    有道智云    积极的人是幸运的人，他们能看到玫瑰，而其他人只看到刺。
    金山词霸    /
    百度翻译    积极的人是幸运的人，他们能看到玫瑰，而其他人只看到荆棘。
```

第四组：
```
原文：Meituan, the food delivery and online services platform, is the world's fourth most valuableprivate technology company.
译文：
    有道智云    美团是世界上第四大最具价值的民营科技公司。
    金山词霸    /
    百度翻译    食品交付和在线服务平台Meituan是全球第四家最有价值的私人科技公司。
```

### 5. 翻译模式

平台 | 文本 | 语音 | 图片
----|------|------|----
有道智云| True | True | True
金山词霸| True | False | False
百度翻译| True | True | False

百度翻译的图片翻译API，尚开发中，有开放倾向。

### 6. 传输协议类型

这里指的是对`HTTP`和`HTTPS`协议类型的支持情况。

平台 | HTTP | HTTPS
----|------|------
有道智云| True | True
金山词霸| True | False
百度翻译| True | True

### 7. 文本翻译服务配置

**有道智云**
> 单次查询最大字符数默认为5000、每小时最大查询次数默认为100万。可调整。

**金山词霸**
> 无

**百度翻译**
> 单次请求长度控制在 6000 bytes以内。（汉字约为2000个）

### 8. 参数多样性

**有道智云**

字段名 | 类型 | 含义 | 必填 | 备注
------|-----|------|-----|----
q | text | 要翻译的文本 | True | 必须是UTF-8编码
from | text | 源语言 | True | 语言列表 (可设置为auto)
to | text | 目标语言 | True | 语言列表 (可设置为auto)
appKey | text | 应用 ID | True | 可在 应用管理 查看
salt | text | 随机数 | True |
sign | text | 签名，通过md5(appKey+q+salt+应用密钥)生成 | True | appKey+q+salt+应用密钥的MD5值
ext | 翻译结果音频格式，支持mp3 | false | mp3 |
voice | 翻译结果发音选择，0为女声，1为男声，默认为女声 | false | 0 |

详情参见：[http://ai.youdao.com/docs/doc-trans-api.s#p02](http://ai.youdao.com/docs/doc-trans-api.s#p02)

**金山词霸**

字段名 | 类型 | 必填参数 | 描述 | 备注
------|-----|---------|-----|----
w | TEXT | Y | 请求翻译关键词 | ASCII编码
type | TEXT | N | 结果类型 | 默认为 XML，可填入 JSON
key | TEXT | Y | 应用 ID | 通过邮件接收

**百度翻译**

字段名 | 类型 | 必填参数 | 描述 | 备注
------|-----|---------|-----|----
q | TEXT | Y | 请求翻译query | UTF-8编码
from | TEXT | Y | 翻译源语言 | 语言列表(可设置为auto)
to | TEXT | Y | 译文语言 | 语言列表(不可设置为auto)
appid | INT | Y | APP ID | 可在管理控制台查看
salt | INT | Y | 随机数 |
sign | TEXT | Y | 签名 | appid+q+salt+密钥 的MD5值

详情参见：[http://api.fanyi.baidu.com/api/trans/product/apidoc#joinFile](http://api.fanyi.baidu.com/api/trans/product/apidoc#joinFile)

### 9. 定制服务

### 10. 收费标准

**有道智云**
> 主要有两种计费方式，按翻译字符数和按每月活跃客户端数计费。具体计费规则参见官方链接：
>
> http://ai.youdao.com/docs/doc-trans-price.s#p07

**金山词霸**
> 金山词霸只是提供简单的单词翻译，收费标准为完全免费。

**百度翻译**
> 每月翻译字符数低于200万，享免费服务；现价¥49.00/百万字符。具体计费规则参见官方链接：
>
> http://api.fanyi.baidu.com/api/trans/product/prodinfo#0

--------------------------------------------------------------------------------

# 文档属性
<table>
    <tr>
        <th width="120px" style="background:#EBEBE0">时间</th>
        <th width="60px" style="background:#EBEBE0">版本</th>
        <th width="75px" style="background:#EBEBE0">作者</th>
        <th width="600px" style="background:#EBEBE0">描述</th>
    </tr>
    <tr>
        <td>2018-07-17</td>
        <td>v1.0</td>
        <td>大鱼</td>
        <td>创建</td>
    </tr>
</table>

--------------------------------------------------------------------------------
