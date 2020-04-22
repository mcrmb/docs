# 核心插件使用说明

## 功能

* 提供玩家游戏内充值方式；
* 对接Mcrmb云平台接口，供玩家查询点券余额等；
* 与其他插件如PlaceholderApi等挂钩，提供展示变量；
* 提供接口及事件，供有开发能力的服主开发自有插件；

## 配置文件 config.yml

核心插件中的config.yml文件，用于配置插件各项设置。

?>直接编辑config.yml文件时，请注意yml格式，每一行的缩进不能随便修改，冒号后的1个空格不能随意删除~

| 配置项 | 类型 | 释义 |
|:---|:---|:---|
| sid | string | 服务器的SID |
| key | string | 服务器的KEY |
| logapi | boolean | 是否展出API请求内容 |
| renew_on_join | boolean | 是否在玩家加入服务器时刷新一次点券余额缓存 |
| cache_timeout | int | 缓存超时时间 |
| whitelist | int | 白名单门槛功能，若为0则禁用<br>若大于0，玩家需要有该数值的点券余额方可进服 |
| op_modify | boolean | OP是否可以操作玩家点券 |
| qrpay_ingame | boolean | `2.0b11` 是否开启游戏内扫码支付  |
| qrpay_compatible | boolean | `2.0b12` 游戏内扫码支付兼容模式 |
| qrpay_empty_hand | boolean | `2.0b13` 是否必须空手情况下才可发起扫码支付 |
| qrpay_limit_sec | int | `2.0b13` 扫码支付状态最长可以持续几秒？0为不限制。 |
| playerpoints | boolean | 转入PlayerPoints模式<br>本功能开启情况下，MCRMB系列子插件不可用。因为点券只在MCRMB中临时停留，玩家进服或刷新余额时，点券将转入`PlayerPoints`插件 |
| command | string | 指令内容，请勿随意修改! 若修改必须同时修改 plugin.yml 文件。 |
| point | string | 单位名称（点券、钻石、元宝等） |
| prefix | string | 提示前缀 |
| help | string | 玩家帮助信息 |

## 管理指令

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
| /b clearcache | `2.0b6` 清理玩家本地点券缓存\(谨慎操作\) |
| /b clearmap \[player\] | `2.0b13` 清理玩家背包中残留的地图（不指定则清理所有在线玩家） |
| /b cancel \[player\] | `2.0b13` 强行退出玩家的支付状态 |
| /b cancelall | `2.0b13` 强行退出所有在线玩家的支付状态 |
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
##  PlaceholderApi变量

| 变量 | 含义 |
| :--- | :--- |
| %mcrmb\_money% | 玩家的点券余额 |
| %mcrmb\_recharge% | 玩家的累计充值 |
| %mcrmb\_consume% | 玩家的累计消费 |

?>点券的**累计充值**目前已经可以选择**不统计手工加点**部分，服主可以到Mcmrb管理平台-&gt;服务器管理-&gt;服务器设置-&gt;手动加点计入累计选项，选择不计入。

##  ScoreboardStats变量

| 变量 | 含义 |
| :--- | :--- |
| %mcrmb% | 玩家的点券余额 |

##  InfoBoardReborn变量

| 变量 | 含义 |
| :--- | :--- |
| &lt;mcrmb&gt; | 玩家的点券余额 |

## 插件接口及事件

[跳转查看](/apis/core-plugin-api)

## 默认Config.yml 

```yaml
## MCRMB插件配置文件，该文件生成于版本：2.0b13

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

##  是否开启内置二维码支付   2.0b11
qrpay_ingame: true

##  是否开启二维码兼容MOD服模式，如果发现二维码无法展示，可以尝试开启。  2.0b12
qrpay_compatible: false

##  是否强制空手状态下才可以发起支付，如果发现充值结束时物品会丢失，安全起见可以开启这个。  2.0b13
qrpay_empty_hand: false

##  玩家可以打开二维码的时间限制，单位为秒，0则不限制   2.0b13
qrpay_limit_sec: 0

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





