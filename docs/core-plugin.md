# MCRMB核心插件使用说明

## 功能

* 提供玩家游戏内充值方式（扫二维码、提交卡密）；
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
| qrpay_compatible | boolean | `2.0b12` 游戏内扫码支付兼容模式<br>如果开启后依然没法展示二维码，请先移除MOD、插件，**仅保留MCRMB插件**进行测试。 |
| qrpay_empty_hand | boolean | `2.0b13` 是否必须空手情况下才可发起扫码支付 |
| qrpay_limit_sec | int | `2.0b13` 扫码支付状态最长可以持续几秒？0为不限制。 |
| qrpay_limit_one_payer | boolean | `2.0b16` 是否限制只能有一个玩家在扫码状态中。 |
| bmoney_after_exit_qrpay | boolean | `2.0b16` 玩家按Q退出充值后是否要自动执行一次/b money查询余额 |
| playerpoints | boolean | 转入PlayerPoints模式<br>本功能开启情况下，MCRMB系列子插件不可用。因为点券只在MCRMB中临时停留，玩家进服或刷新余额时，点券将转入`PlayerPoints`插件 |
| playerpoints_check_on_join_server | boolean | `2.0b16` 转入PlayerPoints模式下，玩家进服时是否查询点券余额转入PlayerPoints |
| command | string | 指令内容，请勿随意修改! 若修改必须同时修改 plugin.yml 文件。 |
| point | string | 提示信息单位名称（可以是点券、钻石、元宝等） |
| prefix | string | 提示信息前缀，默认为`[&c点券中心&r] &e` |
| help | string | 玩家帮助信息 |
| transfer_mode | boolean | `2.0b17` 是否开启自定义指令转入功能，该模式可自定义指令实现玩家查询、登录及充值到账时自动转入点券到服务器（自定义指令方式），优先于playerpoints模式。 |
| transfer_mode_cmds | list | `2.0b17` 定义转入功能指令集，其中`{player}`将自动替换为目标玩家，`{points}`将自动替换为可转换点券余额，`{timestamp}`将替换为时间戳可适配一些累计充值累计插件.<br>默认 config.yml 示例:：<br> `- "p give {player} {points}"` 后台模式执行<br> `- "op:say 我充值了 {points} 点券! (测试,正式使用请删除)" ` 指令前加"op:"： 玩家以临时OP权限执行<br>`- "p:msg {player} 我充值了{points} 点券! (测试,正式使用请删除)"` 指令前加"p:"： 玩家执行|
| transfer_mode_on_join_server | boolean | `2.0b17` 自定义转入模式下，玩家进服时是否查询点券并执行逻辑 |
| separator | string | `2.0b17` 自定义分隔符，可使用颜色代码 `&` 或 `§` |
| awards | Object | `2.0b18` 累计充值奖励内容配置部分，具体用法请往下拉查看默认config.yml中的注释 |


## 管理指令

| 指令 | 作用 |
| :--- | :--- |
| /b setup &lt;sid&gt; &lt;key&gt; | 配置sid与key |
| /b status | 查看插件综合状态，/b admin 也可以 |
| /b reload | 重载核心插件 |
| /b money &lt;player&gt; | 查询玩家点券余额<br> 如果有转入PlayerPoints模式 `playerpoints` 或自定义指令转入模式 `transfer_mode` 启用时，也将会在查询时执行 |
| /b give &lt;player&gt; &lt;points&gt; | 增加玩家点券余额\(云平台\) |
| /b take &lt;player&gt; &lt;points&gt; | 减少玩家点券余额\(云平台\) |
| /b set &lt;player&gt; &lt;points&gt; | 重设玩家点券余额\(云平台\) |
| /b test \[url\] | 测试网络连通性，URL可选 |
| /b clearcache | `2.0b6` 清理玩家本地点券缓存\(谨慎操作\) |
| /b clearmap \[player\] | `2.0b13` 清理玩家背包中残留的地图（不指定则清理所有在线玩家） |
| /b cancel \[player\] | `2.0b13` 强行退出玩家的支付状态 |
| /b cancelall | `2.0b13` 强行退出所有在线玩家的支付状态 |
| /b unset <玩家名> <奖励代码> | `2.0b18` 清除玩家的领奖状态(若符合条件玩家可再次领取) |
| /b debug | 开启调试模式 |

?>`/b give /b take /b set` 3个指令，均为修改云平台的点券余额。  
因此，如果您的服务器是转入PlayerPoints模式，这3个指令没有意义，需要增加玩家本地点券时，直接使用PlayerPoints的增加点券指令即可。  
`/b debug `指令为调试指令，开启之后，所有MCRMB网络请求会展示调用堆栈以及是否在主线程。可以根据其中的信息，判断是什么插件调用了Mcrmb以及是否会造成网络卡服。

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
| /b list | `2.0b18` 查看所有累计充值奖励 |
| /b get <奖励代码> | `2.0b18` 领取累计充值奖励 |

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
## MCRMB插件配置文件，该文件生成于版本：2.0b18

## 服务器信息，请先注册平台，然后从充值平台接口获取。可使用/b setup <sid> <key> 指令快捷设置。自动配置后本文件的中文提示会消失，请留意！
sid: 'Your_SID'
## SID如何获取？  进入www.mcrmb.com =》我的服务器 =》“编号”就是sid，必须填对
key: 'Your_KEY'
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

##  是否开启内置二维码支付   Ver: 2.0b11
qrpay_ingame: true

##  是否开启二维码兼容MOD服模式，如果发现二维码无法展示，可以尝试开启，KCauldron等服务端高发。  Ver: 2.0b12
qrpay_compatible: false

##  是否限制同时只能1个人进行支付，Catserver部分版本及KCauldron等服务端可能出现地图源所有玩家都为同一个的问题，这种情况下，如果有两个玩家在请求二维码，后操作的玩家的支付二维码会覆盖前面的玩家的。此时应打开此功能。  Ver: 2.0b16
qrpay_limit_one_payer: false

##  是否强制空手状态下才可以发起支付，如果发现充值结束时物品会丢失，安全起见可以开启这个。  Ver: 2.0b13
qrpay_empty_hand: false

##  玩家可以维持二维码扫码状态的时间限制，单位为秒，0则不限制。超过这个时间之后，会被强行退出支付状态。   Ver: 2.0b13
qrpay_limit_sec: 0

##  玩家退出二维码充值之后，自动执行一次/b money   Ver:2.0b16
bmoney_after_exit_qrpay: false

# 是否自动转换为PlayerPoints点券?
## 启用该模式后：玩家在进服时触发mcrmb点券余额查询,并自动扣除,加到PlayerPoints账户中. 玩家输入/b money也可以转换余额到PlayerPoints.
## 该模式下, b give指令增加的是玩家mcrmb点券余额, 如果想要增加PlayerPoints余额请直接操作PlayerPoints指令.
## 该模式下, McrmbShop / McrmbVip / McrmbDraw / McrmbBuyCommand 插件均不再可用. 请使用与PlayerPoints配套的插件.   Ver： 2.0b2
playerpoints: false

# 玩家加入服务器时检查是否有点券余额可以转换，如果你的服务器没有使用BungeeCord，可能会在没登录的时候就已经开始转换，这种情况，请将这里设置为false。  Ver： 2.0b16
playerpoints_check_on_join_server: true



## 2.0b17新增： 点券转入模式自定义指令，假设：玩家充值1元，得到100点券（MCRMB平台）。登录服务器后，MCRMB平台层玩家点券清空为0，并执行： pp give 玩家名 100.  此指令作用类似与转换为PlayerPoints点券功能。可兼容更多本地点券系统。
transfer_mode: false

## 2.0b17新增。
transfer_mode_cmds:
  - "p give {player} {points}"  ## 后台模式执行
  - "op:say 我充值了 {points} 点券! (测试,正式使用请删除)"  ## 指令前加"op:"： 玩家以临时OP权限执行
  - "p:msg {player} 我充值了{points} 点券! (测试,正式使用请删除)"  ## 指令前加"p:"： 玩家执行

## 2.0b17新增： 玩家进服时是否执行转入模式指令
transfer_mode_on_join_server: true

##  指令单词，请勿随意修改! 若修改必须同时修改 plugin.yml 文件。 本指令将影响所有Mcrmb子插件如/b shop 和 /b vip
command: 'b'

## 插件内部提示信息，可自行设置。
point: '点券' ##点券单位
prefix: '[&c点券中心&r] &e' ##系统前缀
help:  '
&e===================MCRMB自动充值系统帮助说明===================<br>
&e※ 余额查询： /{command} money  &b查询你的余额/累计充值/累计消费<br>
&e※ 微信充值： /{command} wx <整数金额>  &b发起微信支付扫码充值<br>
&e※ QQ充值： /{command} qq <整数金额>  &b发起QQ支付扫码充值<br>
&e※ 支付宝充值： /{command} zfb <整数金额>  &b发起支付宝支付扫码充值<br>
&e※ 强退扫码状态： /{command} cancel  &b指令方式退出扫码充值状态<br>
&e※ 累计充值奖励列表： /{command} list  &b查看所有累计充值奖励活动<br>
&e※ 获取累计充值奖励： /{command} get <奖励代码>  &b达标后领取累计充值的奖励<br>
&e※ 卡密充值： /{command} cz <类型> <卡号> <密码>  &b查看卡密类型: /{command} cz<br>
&b※  示范： /{command} cz yd 12345678912345678 123456789123456789  &d可充值一张移动卡<br>
&e※ 卡密查询： /{command} ck [卡号]  &b当没有输入卡号的时候，自动查询最后5条卡密记录<br>
&b※  示范： /{command} ck 12345678912345678   &d可查询对应的充值卡充值状态<br>
&e※ 流水查询： /{command} cx &b查询你自己的最后5条交易记录,包括充值和消费<br>
&e=================================================================='

## 2.0b17新增： 分割线, 本内容出现于各种MCRMB点券中心提示的头与尾。
separator: '&b-----------------------------------------'


## 2.0b18新增: 累计充值奖励功能, 请注意累计计算需要区分点券和充值人民币, 如果你的 人民币:点券 是 1:1 那可以忽略
## 以下是一些乱写的示例, 各位服主可以自行发挥想象力, {player}将替换为玩家名.
awards:
  #当type不存在时, 为普通累计器, 使用mcrmb平台玩家的累计充值点券, 该数值可在MCRMB后台清零. type不存在时, begin,end,qd,rmb均无效. 仅可以设置point值.
  '首充礼包':
    # 定义达标点券数  type不存在时
    point: 50
    # 定义达标奖励
    cmds:
      - "points give {player} 1"
      - "give {player} diamond 100"  ## 后台模式执行
      - "op:say 我领取了一个首充礼包，开始氪金之旅"  ## 临时OP模式执行
      - "p:msg {player} 我已累计充值超过50点券 (测试,正式使用请删除)"  ## 玩家模式执行


  #当type为sum或max, 为高级累计器,用于限时活动,该累计器将与充值平台的订单数据直接比对, 因此达标单位是[人民币], 如果你的 人民币:点券 是 1:1 可以忽略此提示
  #当type值存在时, 仅可以设置rmb值, 设置point值没有效果.
  '微信充值累计100元礼包':
    # 定义达标人民币金额
    rmb: 100
    # 当type为sum时,计算筛选条件内的总额(也就是累计充值多少元)
    type: "sum"
    # 开始时间, 格式如下
    begin: "2020-08-01 00:00:00"
    # 结束时间, 格式如下
    end: "2020-08-31 23:59:59"
    # 充值渠道, wx=微信, zfb=支付宝, qq=QQ钱包, card=充值卡, 若不限渠道, 请不要加这一项.
    qd: "wx"
    cmds:
      - "say {player}在2020.08.01-2020.08.31微信充值累计达到100元即可领取此奖励,只能领取一次"

  '支付宝单笔100元礼包':
    rmb: 100
    type: "max"
    begin: "2020-08-01 00:00:00"
    end: "2020-08-31 23:59:59"
    # 充值渠道, wx=微信, zfb=支付宝, qq=QQ钱包, card=充值卡, 若不限渠道, 请不要加这一项.
    qd: "zfb"
    cmds:
      - "say {player}在2020.08.01-2020.08.31微信充值累计达到100元即可领取此奖励,只能领取一次"

  #以下示范用于奖励限定时间内单笔充值高于指定值的玩家.
  '冲击200元礼包':
    rmb: 200
    # 当type为max时,判断筛选条件内最大一笔充值金额是否达到上述rmb数值.
    type: "max"
    begin: "2020-08-01 00:00:00"
    end: "2020-08-31 23:59:59"
    cmds:
      - "say {player}在2020.08.01-2020.08.31充值达到200单笔即可领取此奖励,只能领取一次"

  #以下示范用于奖励限定时间内单笔充值高于指定值的玩家.
  '每100元礼包':
    rmb: 4
    type: "gt"
    # 当type为gt时, 时间段内每次超过该金额均可领取一次奖励, award_log文件将统计已领取次数, 统计值小于云平台返回次数时玩家即可继续领取
    begin: "2020-08-01 00:00:00"
    end: "2020-08-31 23:59:59"
    cmds:
      - "say {player}在2020.08.01-2020.08.31每笔大于等于4块的订单都可以领一次这奖励..."

  #以下示范用于奖励限定时间内单笔充值高于指定值的玩家.
  'test1':
    rmb: 3
    type: "gt"
    end: "2020-08-31 23:59:59"
    cmds:
      - "say {player}在2020年8月31日之前每笔大于等于3块的订单都可以领一次这奖励..."

  'test2':
    rmb: 2
    type: "gt"
    begin: "2020-08-01 00:00:00"
    cmds:
      - "say {player}在2020年8月1日之后每笔大于等于2块的订单都可以领一次这奖励..."

  'test3':
    rmb: 1
    type: "gt"
    cmds:
      - "say {player}充值了1块钱..."


  '累计1000元礼包':
    rmb: 1000
    type: "sum"
    cmds:
      - "say {player}累计充值了1000块钱..."
```

## 插件下载
[各插件最新版本下载](/sub-plugins/downloads)


## 插件更新说明

版本号后的#为http://ci.mcrmb.com 的构建序号~

### V2.0b19 #94
Fix 修正较低版本客户端出现的地图可丢弃等充值状态玩家不受限制问题；  
Fix 兼容Windows下`b setup`指令自动保存SID和KEY到文件保存无效问题；

### V2.0b18 #92
Add 新增充值累计奖励功能，具体用法请往上看config.yml文件中注释；
Fix 修正扫码功能下1.9以后客户端版本玩家使用F键保留地图的问题；
Add transfer_mode_cmds的命令设置新增一个{timestamp}变量可兼容一些本地累计充值统计插件；

### V2.0b17 #90
Add 扫码功能更新，玩家充值过程中将自动持续检测是否完成支付并执行相应逻辑；  
Add 自定义指令转入功能`transfer_mode`，类似转入PlayerPoints模式，但可以自定义多个指令，兼容性提升；  
Add 新增`separator`配置用于自定义系统频繁出现的分隔线；    
Add 扫码请求API `PayApi.RequestQr` ，返回二维码内容及订单ID，方便开发者深度定制；  
Add 新的事件：扫码请求事件`QrDoneEvent`、扫码结果轮询事件`QrRequestEvent`，方便开发者深度定制；  
Fix 修正PayApi.Pay在新版本中可能产生的 `Asynchronous entity track!` 报错；


### V2.0b16 #86 
Add `qrpay_limit_one_payer`选项，限制同一时间只能有一个玩家可进入二维码状态。因KC等MOD服务端地图所有玩家使用同一对象，导致玩家A充值时二维码会被进入充值状态的玩家B的支付二维码覆盖，建议KC端开启，Catserver端最新版不存在此问题，旧版存在此问题，需要将此项设为 `true`。<br>
Add `bmoney_after_exit_qrpay`选项，是否在玩家二维码支付状态下按Q退出后自动执行一次/b money展示最新余额，`true` or `false`。<br>
Add `playerpoints_check_on_join_server`选项，点券转换为PlayerPoints模式下，默认玩家入服即开始转换，由于部分服务器非群组服，可能出现玩家未登陆就在转换的提示，这种情况下，请将此项设置为 `false`。<br>
Fix 将兼容模式的KC、Catserver判断去除，请更新此版本的服主务必自行检查二维码是否能正常生成展示，如不能则需要打开 `qrpay_compatible`，上一版本中此开关在Kc、Catserver下会自动打开。因同类服务端不同版本兼容问题存在情况不一样，现不再做服务端判断。<br> 
Fix 完善二维码支付Render逻辑。

### V2.0b15 #85
Fix 修正判断服主未设置SID和KEY的逻辑。

### V2.0b14 #83
Fix 修正`/b reload`指令对部分逻辑无效的BUG，此问题重启服务器也可以解决，只是不太方便。可选升级。

### V2.0b13 #82
Add `/b cancel <玩家名>` 取消指定玩家的二维码支付状态  
Add `/b cancelall` 取消所有在线玩家的二维码支付状态  
Add `/b clearmap [玩家名]`  清理指定玩家或所有在线玩家背包中残留的二维码地图  
Add `qrpay_limit_sec` 选项，可以限制玩家二维码支付状态持续的时间，单位为秒，0为不限制，超时后自动退出支付状态  
Add `qrpay_empty_hand` 选择，可以限制玩家进入二维码支付状态时是否必须空手，true/false
Fix 修正物品保护插件导致的退出支付状态后地图又回到玩家手上问题  


### V2.0b12 #81  
Add 扫码功能增加对CatServer及KCauldron的支持，其他MOD服如果有无法展示二维码的情况，尝试打开config.yml中的开关【qrpay_compatible: true】  
Fix 优化二维码Render的逻辑；  

### V2.0b11 #80  
Fix 完善游戏内扫码功能开关，可选择关闭。默认是开启状态【qrpay_ingame: true】  

### V2.0b10 #79  
Fix 修正Manual中的缓存调用报错问题，PlayerPoints转入模式高发  

### V2.0b9 #78  
Fix 修正事件触发到异步逻辑中 (使用配套PlayerPoints插件，能正常扣款但无法发放物品的请升级此版本)  

### V2.0b8 #77  
Add 增加一个玩家指令：/b cancel 用于强制退出扫码状态；  
Fix 改进扫码充值逻辑到异步，扫码充值逻辑统一到单类；  
Add 测试类新增，启动插件时若异常则启动测试；  
Add /b debug 模式下网络请求输出加上主线程判断；  
Add 加入公告、版本读取；  
Fix 插件启动信息样式优化；  
Add config.yml文件加入插件版本号；  

### V2.0b6 #75  
Add 新增PAPI变量【累计充值】：%mcrmb_recharge%  
Add 新增PAPI变量【累计消费】：%mcrmb_consume%  
Fix 重做插件的余额、累计充值、累计消费缓存，所以升级该版本需要同时升级魔改版的 PlayerPoints/InfoBroad；  
Add 增加指令/b clearcache 用于清理上述缓存（测试）  

### V2.0b5 #72  
Add 新增支付宝支付扫码，游戏内可以利用地图扫码支付。指令：/b zfb <金额>  

### V2.0b4 #71  
Add 新增QQ支付扫码，游戏内可以利用地图扫码支付。指令：/b qq <金额>  

### V2.0b3 #70  
Fix 地图扫码功能支持更多版本，当前已测试的版本支持 1.7.* 至 1.15.*  
Add 地图扫码加入图标

### V2.0b2 #68
Fix 修正com.mcrmb.Mcrmb.balances无法外部访问的问题  
Fix 修正com.mcrmb.Mcrmb.debugMode无法外部访问的问题  

### V2  .0b1 #63
Add 新增微信支付扫码功能，游戏内可以利用地图扫码支付。指令：/b wx <金额>  
Add 新增PlayerPoints挂钩及转入模式，该模式下，玩家查mcrmb余额或入服时，将自动把点券余额转为PlayerPoints余额。   
Fix 指令整理，去除/b admin * 等指令，具体管理指令可在/b admin或/b status中查看。  
Fix /b setup 将不会影响config文件注释内容，也不会将中文变为乱码，方便服主配置，旧版插件建议可以删掉config.yml文件重新配置一次。  
Fix /b test 部分加入一个https测试项，去除两个没有意义的附加测试。  
Fix 优化logapi功能，将展示更多的api接口信息便于排查问题。  
Fix 插件不再区分UTF-8或GBK，内部强制为UTF-8，兼容老版本GBK格式。  

### 更往前的历史记录
[请移步CI](https://ci.mcrmb.com/job/MCRMB/)

注意：若有任何问题，请截图发邮件至702048@qq.com


