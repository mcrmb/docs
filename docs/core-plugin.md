# 核心插件使用说明

## 功能

* 提供玩家游戏内充值方式；
* 对接Mcrmb云平台接口，供玩家查询点券余额等；
* 与其他插件如PlaceholderApi等挂钩，提供展示变量；
* 提供接口及事件，供有开发能力的服主开发自有插件；

## 配置文件 config.yml

核心插件中的config.yml文件，用于配置插件各项设置。

?>直接编辑config.yml文件时，请注意yml格式，每一行的缩进不能随便修改，冒号后的1个空格不能随意删除~
<table>
  <thead>
    <tr>
      <th style="text-align:left">&#x914D;&#x7F6E;&#x9879;</th>
      <th style="text-align:left">&#x7C7B;&#x578B;</th>
      <th style="text-align:left">&#x91CA;&#x4E49;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">sid</td>
      <td style="text-align:left">string</td>
      <td style="text-align:left">&#x670D;&#x52A1;&#x5668;&#x7684;SID</td>
    </tr>
    <tr>
      <td style="text-align:left">key</td>
      <td style="text-align:left">string</td>
      <td style="text-align:left">&#x670D;&#x52A1;&#x5668;&#x7684;KEY</td>
    </tr>
    <tr>
      <td style="text-align:left">logapi</td>
      <td style="text-align:left">boolean</td>
      <td style="text-align:left">&#x662F;&#x5426;&#x5C55;&#x51FA;API&#x8BF7;&#x6C42;&#x5185;&#x5BB9;</td>
    </tr>
    <tr>
      <td style="text-align:left">renew_on_join</td>
      <td style="text-align:left">boolean</td>
      <td style="text-align:left">&#x662F;&#x5426;&#x5728;&#x73A9;&#x5BB6;&#x52A0;&#x5165;&#x670D;&#x52A1;&#x5668;&#x65F6;&#x5237;&#x65B0;&#x4E00;&#x6B21;&#x70B9;&#x5238;&#x4F59;&#x989D;&#x7F13;&#x5B58;</td>
    </tr>
    <tr>
      <td style="text-align:left">cache_timeout</td>
      <td style="text-align:left">int</td>
      <td style="text-align:left">&#x7F13;&#x5B58;&#x8D85;&#x65F6;&#x65F6;&#x95F4;</td>
    </tr>
    <tr>
      <td style="text-align:left">whitelist</td>
      <td style="text-align:left">int</td>
      <td style="text-align:left">
        <p>&#x767D;&#x540D;&#x5355;&#x95E8;&#x69DB;&#x529F;&#x80FD;&#xFF0C;&#x82E5;&#x4E3A;0&#x5219;&#x7981;&#x7528;</p>
        <p>&#x82E5;&#x5927;&#x4E8E;0&#xFF0C;&#x73A9;&#x5BB6;&#x9700;&#x8981;&#x6709;&#x8BE5;&#x6570;&#x503C;&#x7684;&#x70B9;&#x5238;&#x4F59;&#x989D;&#x65B9;&#x53EF;&#x8FDB;&#x670D;</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">op_modify</td>
      <td style="text-align:left">boolean</td>
      <td style="text-align:left">OP&#x662F;&#x5426;&#x53EF;&#x4EE5;&#x64CD;&#x4F5C;&#x73A9;&#x5BB6;&#x70B9;&#x5238;</td>
    </tr>
    <tr>
      <td style="text-align:left">qrpay_ingame</td>
      <td style="text-align:left">boolean</td>
      <td style="text-align:left">&#x662F;&#x5426;&#x5F00;&#x542F;&#x6E38;&#x620F;&#x5185;&#x626B;&#x7801;&#x652F;&#x4ED8;</td>
    </tr>
    <tr>
      <td style="text-align:left">qrpay_compatible</td>
      <td style="text-align:left">boolean</td>
      <td style="text-align:left">&#x6E38;&#x620F;&#x5185;&#x626B;&#x7801;&#x652F;&#x4ED8;&#x517C;&#x5BB9;&#x6A21;&#x5F0F;</td>
    </tr>
    <tr>
      <td style="text-align:left">playerpoints</td>
      <td style="text-align:left">boolean</td>
      <td style="text-align:left">
        <p>&#x8F6C;&#x5165;PlayerPoints&#x6A21;&#x5F0F;</p>
        <p>&#x672C;&#x529F;&#x80FD;&#x5F00;&#x542F;&#x60C5;&#x51B5;&#x4E0B;&#xFF0C;MCRMB&#x7CFB;&#x5217;&#x5B50;&#x63D2;&#x4EF6;&#x4E0D;&#x53EF;&#x7528;&#x3002;&#x56E0;&#x4E3A;&#x70B9;&#x5238;&#x53EA;&#x5728;MCRMB&#x4E2D;&#x4E34;&#x65F6;&#x505C;&#x7559;&#xFF0C;&#x73A9;&#x5BB6;&#x8FDB;&#x670D;&#x6216;&#x5237;&#x65B0;&#x4F59;&#x989D;&#x65F6;&#xFF0C;&#x70B9;&#x5238;&#x5C06;&#x8F6C;&#x5165;<code>PlayerPoints</code>&#x63D2;&#x4EF6;</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">command</td>
      <td style="text-align:left">string</td>
      <td style="text-align:left">&#x6307;&#x4EE4;&#x5185;&#x5BB9;&#xFF0C;&#x8BF7;&#x52FF;&#x968F;&#x610F;&#x4FEE;&#x6539;!
        &#x82E5;&#x4FEE;&#x6539;&#x5FC5;&#x987B;&#x540C;&#x65F6;&#x4FEE;&#x6539;
        plugin.yml &#x6587;&#x4EF6;&#x3002;</td>
    </tr>
    <tr>
      <td style="text-align:left">point</td>
      <td style="text-align:left">string</td>
      <td style="text-align:left">&#x5355;&#x4F4D;&#x540D;&#x79F0;&#xFF08;&#x70B9;&#x5238;&#x3001;&#x94BB;&#x77F3;&#x3001;&#x5143;&#x5B9D;&#x7B49;&#xFF09;</td>
    </tr>
    <tr>
      <td style="text-align:left">prefix</td>
      <td style="text-align:left">string</td>
      <td style="text-align:left">&#x63D0;&#x793A;&#x524D;&#x7F00;</td>
    </tr>
    <tr>
      <td style="text-align:left">help</td>
      <td style="text-align:left">string</td>
      <td style="text-align:left">&#x73A9;&#x5BB6;&#x5E2E;&#x52A9;&#x4FE1;&#x606F;</td>
    </tr>
  </tbody>
</table>## 管理指令

| 指令 | 作用 |
| :--- | :--- |
| /b setup &lt;sid&gt; &lt;key&gt; | 配置sid与key |
| /b status | 查看插件综合状态，/b admin 也可以 |
| /b reload | 重载核心插件 |
| /b money &lt;player&gt; | 查询玩家点券余额 |
| /b give &lt;player&gt; &lt;points&gt; | 增加玩家点券余额\(云平台\) |
| /b take &lt;player&gt; &lt;points&gt; | 减少玩家点券余额\(云平台\) |
| /b set &lt;player&gt; &lt;points&gt; | 重设玩家点券余额\(云平台\) |
| /b test \[url\] | 测试网络连通性，URL可选 |
| /b clearcache | 清理玩家本地点券缓存\(谨慎操作\) |
| /b debug | 开启调试模式 |

?>/b give /b take /b set 3个指令，均为修改云平台的点券余额。因此，如果您的服务器是转入PlayerPoints模式，这3个指令没有意义，需要增加玩家本地点券时，直接使用PlayerPoints的增加点券指令即可。

/b debug 指令为调试指令，开启之后，所有MCRMB网络请求会展示调用堆栈以及是否在主线程。可以根据其中的信息，判断是什么插件调用了Mcrmb以及是否会造成网络卡服。
## 玩家指令

| 指令 | 作用 |
| :--- | :--- |
| /b help | 查看帮助 |
| /b money | 查询我的点券余额等信息 |
| /b wx &lt;money&gt; | 使用微信扫码充值，金额部分为人民币 |
| /b qq &lt;money&gt; | 使用QQ钱包扫码充值，金额部分为人民币 |
| /b zfb &lt;money&gt; | 使用支付宝扫码充值，金额部分为人民币 |
| /b cancel | 退出扫码充值状态 |
| /b cz | 查询卡密渠道列表及折算比例 |
| /b cz &lt;type&gt; &lt;card.num&gt; &lt;card.pass&gt; | 使用卡密充值，type见上衣指令 |
| /b ck \[card.num\] | 查询卡充值状态 |
| /b cx | 查询我的最后5个流水（充值+消费） |

?>玩家指令在 /b help 可以看到帮助，帮助内容可在config.yml文件中定义。
##  **PlaceholderApi变量**

| 变量 | 含义 |
| :--- | :--- |
| %mcrmb\_money% | 玩家的点券余额 |
| %mcrmb\_recharge% | 玩家的累计充值 |
| %mcrmb\_consume% | 玩家的累计消费 |

{% hint style="success" %}
点券的**累计充值**目前已经可以选择**不统计手工加点**部分，服主可以到Mcmrb管理平台-&gt;服务器管理-&gt;服务器设置-&gt;手动加点计入累计选项，选择不计入。
##  **ScoreboardStats变量**

| 变量 | 含义 |
| :--- | :--- |
| %mcrmb% | 玩家的点券余额 |

##  **InfoBoardReborn变量**

| 变量 | 含义 |
| :--- | :--- |
| &lt;mcrmb&gt; | 玩家的点券余额 |

## 插件接口及事件

{% page-ref page="apis/core-plugin-api.md" %}

## 默认Config.yml 

```yaml
## MCRMB插件配置文件，该文件生成于版本：2.0b12

## 服务器信息，请先注册平台，然后从充值平台接口获取。可使用/b setup <sid> <key> 指令快捷设置。自动配置后本文件的中文提示会消失，请留意！
sid: 'Your SID'
## SID如何获取？  进入www.mcrmb.com =》我的服务器 =》“编号”就是sid，必须填对
key: 'Your KEY'
## KEY如何获取？  进入www.mcrmb.com =》我的服务器 =》 “密钥”就是key，必须填对

## 是否记录api接口? 如果希望查看插件与平台的请求情况，请打开！
logapi: false

## 是否在玩家入服时更新缓存的点券余额，不更新的话，点券为0，玩家必须 查点券余额 或 消费，才会更新点券缓存。
## 关闭该选项可以稍微减轻服务器负担，但必须全服重启才会生效。
## 缓存点券余额用的作用？  所有非Mcrmb自有插件均需要使用此缓存   如：InfoBoardReborn、ScoreboardStats、PlaceholderApi、PlayerPoints 等。  如有使用，请打开。
renew_on_join: true

## 第三方插件主动调用的缓存过期时间，单位为秒，填0为禁用缓存过期
## 禁用缓存过期可稍微减轻服务器负担，  玩家只有手动查询 点券余额 或 消费 ，才会进行更新
## 第三方插件调用缓存时， 发现过期后首次会返回旧值， 并进入不卡服的异步逻辑， 查询新的点券余额
cache_timeout: 120

##  白名单门槛功能，如果此项不为0，renew_on_join项将被自动打开。并且玩家必须有这个点券余额，方能进服。
whitelist: 0

##  本服是否开启管理员修改玩家点券
op_modify: false

##  是否开启内置二维码支付
qrpay_ingame: true

##  是否开启二维码兼容MOD服模式，如果发现二维码无法展示，可以尝试开启。
qrpay_compatible: false

# 是否自动转换为PlayerPoints点券?
## 启用该模式后：玩家在进服时触发mcrmb点券余额查询,并自动扣除,加到PlayerPoints账户中. 玩家输入/b money也可以转换余额到PlayerPoints.
## 该模式下, b give指令增加的是玩家mcrmb点券余额, 如果想要增加PlayerPoints余额请直接操作PlayerPoints指令.
## 该模式下, McrmbShop / McrmbVip / McrmbDraw / McrmbBuyCommand 插件均不再可用. 请使用与PlayerPoints配套的插件.
playerpoints: false


##  指令单词，请勿随意修改! 若修改必须同时修改 plugin.yml 文件。 本指令将影响所有Mcrmb子插件如/b shop 和 /b vip
command: 'b'

## 插件内部提示信息，可自行设置。
point: '点券'
prefix: '[&c点券中心&r] &e'
help:  '
&e===================MCRMB自动充值系统帮助说明===================<br>
&e※ 余额查询： /{command} money  &b查询你的余额/累计充值/累计消费<br>
&e※ 微信充值： /{command} wx <整数金额>  &b发起微信支付扫码充值<br>
&e※ QQ充值： /{command} qq <整数金额>  &b发起QQ支付扫码充值<br>
&e※ 支付宝充值： /{command} zfb <整数金额>  &b发起支付宝支付扫码充值<br>
&e※ 强退扫码状态： /{command} cancel  &b指令方式退出扫码充值状态<br>
&e※ 卡密充值： /{command} cz <类型> <卡号> <密码>  &b查看卡密类型: /{command} cz<br>
&b※  示范： /{command} cz yd 12345678912345678 123456789123456789  &d可充值一张移动卡<br>
&e※ 卡密查询： /{command} ck [卡号]  &b当没有输入卡号的时候，自动查询最后5条卡密记录<br>
&b※  示范： /{command} ck 12345678912345678   &d可查询对应的充值卡充值状态<br>
&e※ 流水查询： /{command} cx &b查询你自己的最后5条交易记录,包括充值和消费<br>
&e=================================================================='
```





