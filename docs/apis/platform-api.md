# 平台层接口文档
使用这些接口，您可以自行开发完整与MCRMB平台对接的插件或网站~
## 简介

MCRMB平台层接口使用Get方式请求，使用Json格式进行回复。

## 返回Json格式

> {"code":"提示代码","msg":"提示信息","data":"返回数据,有数据则必然为数组"}

若请求错误或没有数据，则返回的JSON的data部分将为空白

## UserAgent要求

为了防止无效请求，接口请求默认检测UA，需要包含`java`字样才可以正常获得返回信息，否则将返回403代码。

## 请求基本格式

必须使用get方式，请求参数必须具备`sid`和`wname`和`sign`三项。  
`sid`为服务器编号，通常在插件设置里设定好，`wname`为操作对象的玩家名；  
`sign`为请求签名，使用md5加密，`sign`参数必须放在第一位；

## 签名加密算法

32位MD5加密\(参数一 + 参数二 + 参数三 + 参数N + 密钥key\)，密钥key通常在插件设置里设定好，“+”号的意思为字符串相连。

参数顺序需要和GET请求里的参数顺序一致，GET请求里`sign`参数必须排在第一位。

假设：sid=1，wname=test，密钥=abcabc

则加密字符串为`1testabcabc`，加密后md5为`2d459e0f9ab822d7ecf61f4e7b119763`

则请求参数为 `?sign=2d459e0f9ab822d7ecf61f4e7b119763&sid=1&wname=test`

## 全局错误提示代码

| Code | Msg |
| :--- | :--- |
| 444 | 签名错误 |
| 404 | 参数不齐全 |

## 网关地址

`http://api.mcrmb.com/Api/接口名称?参数`   接口名称大小写敏感

例：

> http://api.mcrmb.com/Api/CheckMoney?sign=2d459e0f9ab822d7ecf61f4e7b119763&sid=1&wname=test

该请求查询sid为1的服务器中玩家test的余额

##  接口列表

### 查询余额 CheckMoney

本接口用于查询玩家的余额。

#### 返回示例

```json
{
"code":"101",
"msg":"玩家余额查询正常!",
"data":[{"money":"2334","allcharge":"2413","allpay":"79"}]
}
```

```json
{
"code":"102",
"msg":"玩家余额为0",
"data":""
}
```

#### Data部分解释

| 键值 | 含义 |
| :--- | :--- |
| money | 玩家当前点券余额 |
| allcharge | 历史充值点券 |
| allpay | 历史消费点券 |



### 查询流水 CheckRecord

本接口用于查询玩家的最后5条交易流水。

#### 返回示例

> {"code":"101","msg":"玩家流水查询正常!","data":\[{"date":"1473450812","text":"API加款\(PlayerPoints加款\)","money":"2"},{"date":"1473450728","text":"API扣款\(PlayerPoints扣款\)","money":"-1"},{"date":"1473450385","text":"API加款\(PlayerPoints重设点券\)","money":"2333"},{"date":"1473449675","text":"API扣款\(PlayerPoints清零点券\)","money":"-3"},{"date":"1473449652","text":"API加款\(PlayerPoints加款\)","money":"1"}\]}

> {"code":"102","msg":"玩家未有流水或未开户","data":""}

#### Data部分解释

| 键值 | 含义 |
| :--- | :--- |
| date | 流水的时间戳 |
| text | 流水说明 |
| money | 流水金额（有负数） |

### 查询卡记录 CheckCard

本接口用于查询玩家的最后5条卡密提交记录。

#### 返回示例

> {"code":"101","msg":"充值卡记录查询正常!","data":\[{"date":"1468434839","num":"12312312312312312","status":"303","money":"0"},{"date":"1468434827","num":"12312312312312312","status":"303","money":"0"},{"date":"1466963726","num":"8013361020215011","status":"303","money":"0"},{"date":"1466962633","num":"123456789123456","status":"203","money":"0"},{"date":"1466959159","num":"123123","status":"303","money":"0"}\]}

> {"code":"102","msg":"玩家未有充值卡记录!","data":""}

#### Data部分解释

| 键值 | 含义 |
| :--- | :--- |
| date | 充值的时间戳 |
| num | 充值卡卡号 |
| status | 卡密处理状态编码 |
| money | 充值面额 |



### 卡密提交 Charge

本接口用于提交卡密进行充值。

#### 请求需要的额外参数

| 参数 | 含义 |
| :--- | :--- |
| ctype | 卡密渠道ID |
| cnum | 卡号 |
| cpwd | 卡密 |

#### 返回示例

> {"code":"201","msg":null,"data":\[{"wcode":"200","wmsg":"正在验证卡密"}\]}

> {"code":"101","msg":null,"data":\[{"wcode":"200","wmsg":"正在验证卡密"}\]}

> {"code":"202","msg":null,"data":\[{"wcode":"208","wmsg":"该卡密已被使用"}\]}

> {"code":"102","msg":null,"data":\[{"wcode":"208","wmsg":"该卡密已被使用"}\]}

#### Data部分解释

| 键值 | 含义 |
| :--- | :--- |
| Wcode | 卡密提交处理状态代码 |
| Wmsg | 卡密提交处理状态解释 |

#### code含义

| code | 含义 |
| :--- | :--- |
| 101 | 新用户，新建玩家账户成功，并成功提交卡密 |
| 102 | 新用户，开设玩家账户成功，但提交的卡密有问题，具体问题data部分里展示 |
| 201 | 老用户，成功提交卡密 |
| 202 | 老用户，提交的卡密有问题，具体问题data部分里展示 |

#### Data部分里的Wcode代码含义

| Wcode | 含义 |
| :--- | :--- |
| 200 | 充值卡提交成功，处理中 |
| 208 | 卡密已经提交过 |
| 303 | 充值卡无效或卡密长度错误 |
| 203 | 卡渠道维护中 |
| 其他 | 卡已经在处理中，不要重复提交，或多次提交错误 |



### 支付接口 Pay

本接口用于提交道具或VIP等消费。

#### 请求需要的额外参数

| 参数 | 含义 |
| :--- | :--- |
| use | 用途说明 |
| money | 消费的金额 |

#### 返回示例

> {"code":"101","msg":"成功扣款!","data":\[{"money":"200"}\]}

> {"code":"102","msg":"您的余额不足,请充值!","data":\[{"money":"1","need":"4"}\]}

#### Data部分解释

| 键值 | 含义 |
| :--- | :--- |
| money | 余额、消费后余额 |
| need | 还需要的点券数量 |

?>其他错误代码，可以直接使用msg作为用户提示。  
103 为玩家未充值过所以没有玩家数据  
105 为提交的金额不对
### 查询服务器卡种类 CardTypes

本接口用于查询服务器支持的卡密渠道及费率，插件载入时必须先查询一次以便获得该服务器允许的渠道方式以及渠道id。

?>wname参数使用任意玩家名或字符串例如`console`即可，不可以不指定。

#### 返回示例

> {"code":"101","msg":"查询支持卡密成功!","data":\[{"yd":{"id":1,"name":"移动神州行","num":17,"pwd":18,"rate":100},"lt":{"id":2,"name":"联通一卡充","num":15,"pwd":19,"rate":100},"dx":{"id":3,"name":"电信全国卡","num":19,"pwd":18,"rate":100},"jk":{"id":4,"name":"骏网一卡通","num":16,"pwd":16,"rate":80},"sd":{"id":5,"name":"盛大一卡通","num":0,"pwd":0,"rate":80},"wm":{"id":6,"name":"完美一卡通","num":10,"pwd":15,"rate":80},"sh":{"id":7,"name":"搜狐一卡通","num":0,"pwd":0,"rate":80},"zt":{"id":8,"name":"征途一卡通","num":16,"pwd":8,"rate":80},"wy":{"id":9,"name":"网易一卡通","num":13,"pwd":9,"rate":80},"qq":{"id":10,"name":"腾讯Q币卡","num":9,"pwd":12,"rate":75},"jy":{"id":11,"name":"久游一卡通","num":0,"pwd":0,"rate":80},"gy":{"id":12,"name":"光宇一卡通","num":0,"pwd":0,"rate":80},"tx":{"id":14,"name":"天下一卡通","num":15,"pwd":8,"rate":80},"th":{"id":15,"name":"天宏一卡通","num":12,"pwd":15,"rate":80}}\]}

#### Data部分解释

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#x952E;&#x503C;</th>
      <th style="text-align:left">&#x542B;&#x4E49;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">id</td>
      <td style="text-align:left">&#x5361;&#x6E20;&#x9053;ID</td>
    </tr>
    <tr>
      <td style="text-align:left">name</td>
      <td style="text-align:left">&#x5361;&#x6E20;&#x9053;&#x540D;&#x79F0;</td>
    </tr>
    <tr>
      <td style="text-align:left">num</td>
      <td style="text-align:left">&#x8BE5;&#x6E20;&#x9053;&#x5361;&#x53F7;&#x7684;&#x957F;&#x5EA6;&#x8981;&#x6C42;</td>
    </tr>
    <tr>
      <td style="text-align:left">pwd</td>
      <td style="text-align:left">&#x8BE5;&#x6E20;&#x9053;&#x5BC6;&#x7801;&#x7684;&#x957F;&#x5EA6;&#x8981;&#x6C42;</td>
    </tr>
    <tr>
      <td style="text-align:left">rate</td>
      <td style="text-align:left">
        &#x672C;&#x670D;&#x52A1;&#x5668;&#x8BE5;&#x6E20;&#x9053;&#x53EF;&#x5151;&#x6362;&#x70B9;&#x5238;&#x7684;&#x6BD4;&#x4F8B;<br>100%&#x5219;&#x4E3A;1&#x5143;=1&#x70B9;&#x5238;&#x3002;
      </td>
    </tr>
  </tbody>
</table>

### 手工加款扣款接口 Manual

本接口为后期加上，可以直接操作玩家点券，请谨慎使用。

#### 请求需要的额外参数

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#x53C2;&#x6570;</th>
      <th style="text-align:left">&#x542B;&#x4E49;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">type</td>
      <td style="text-align:left">
        &#x64CD;&#x4F5C;&#x7C7B;&#x578B;<br>1=&#x52A0;&#x6B3E;<br>2=&#x6263;&#x6B3E;<br>3=&#x76F4;&#x63A5;&#x8BBE;&#x7F6E;&#x70B9;&#x5238;&#x4F59;&#x989D;
      </td>
    </tr>
    <tr>
      <td style="text-align:left">text</td>
      <td style="text-align:left">&#x64CD;&#x4F5C;&#x8BF4;&#x660E;</td>
    </tr>
    <tr>
      <td style="text-align:left">money</td>
      <td style="text-align:left">&#x64CD;&#x4F5C;&#x91D1;&#x989D;&#xFF0C;&#x4E0D;&#x53EF;&#x8D1F;&#x6570;&#x3001;&#x5C0F;&#x6570;</td>
    </tr>
  </tbody>
</table>#### 返回示例

> {"code":"101","msg":"API接口加款操作成功","data":""}

> {"code":"102","msg":"API接口新用户加款操作成功","data":""}

> {"code":"201","msg":"API接口扣款操作成功","data":""}

> {"code":"202","msg":"API接口扣款操作失败，余额不足。","data":\[{"money":"1","need":"4"}\]}

> {"code":"203","msg":"API接口扣款操作失败，玩家不存在。"}

#### Data部分解释

| 键值 | 含义 |
| :--- | :--- |
| money | 当前剩余点券余额 |
| need | 还需多少方可扣款成功 |

#### 设置余额正常返回

> {"code":"101","msg":"API接口加款操作成功","data":""}

> {"code":"201","msg":"API接口扣款操作成功","data":""}

> {"code":"303","msg":"API设置余额操作失败\(余额无变化!\)","data":""}

