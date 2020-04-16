# McrmbVip

## 简介

该插件用于定义服务器VIP型商品及自动管理服务器内的VIP  
工作原理与McrmbShop插件一致，不同的地方在于增加了一些逻辑：

* 定期检查VIP列表是否有到期的玩家
* VIP续费时可以自动折算时长
* 到期之后可以根据预设的指令自动取消权限组 
* 根据预设的指令可以让玩家自行设置称号
* 称号中可以添加敏感词

## 指令

### 玩家

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#x6307;&#x4EE4;</th>
      <th style="text-align:left">&#x8BF4;&#x660E;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">/b vip</td>
      <td style="text-align:left">&#x67E5;&#x8BE2;&#x6240;&#x6709;VIP&#x4EE3;&#x7801;&#xFF0C;GUI&#x6A21;&#x5F0F;&#x76F4;&#x63A5;&#x51FA;GUI&#x9762;&#x677F;</td>
    </tr>
    <tr>
      <td style="text-align:left">/b vip buy &lt;vipcode&gt; &lt;count&gt;</td>
      <td style="text-align:left">
        &#x4F7F;&#x7528;&#x6307;&#x4EE4;&#x8D2D;&#x4E70;VIP&#xFF08;&#x5C55;&#x793A;&#x6027;&#xFF09;<br>vipcode = VIP&#x4EE3;&#x7801;<br>count = &#x8D2D;&#x4E70;&#x6570;&#x91CF;
      </td>
    </tr>
    <tr>
      <td style="text-align:left">/b vip buy &lt;vipcode&gt; &lt;count&gt; sure</td>
      <td style="text-align:left">&#x786E;&#x8BA4;&#x8D2D;&#x4E70;VIP</td>
    </tr>
    <tr>
      <td style="text-align:left">/b vip help</td>
      <td style="text-align:left">&#x5C55;&#x793A;&#x5E2E;&#x52A9;&#x4FE1;&#x606F;</td>
    </tr>
    <tr>
      <td style="text-align:left">/b vip me</td>
      <td style="text-align:left">&#x67E5;&#x770B;&#x6211;&#x7684;VIP&#x4FE1;&#x606F;</td>
    </tr>
    <tr>
      <td style="text-align:left">/b vip prefix &lt;prefix&gt;</td>
      <td style="text-align:left">&#x8BBE;&#x7F6E;&#x6211;&#x7684;&#x79F0;&#x53F7;</td>
    </tr>
  </tbody>
</table>

### 管理

| 指令 | 说明 |
| :--- | :--- |
| /b vip list \[vipcode\] | 查询所有VIP玩家，若有vipcode则查询指定VIP组玩家 |
| /b vip show &lt;player&gt; | 展示玩家的VIP状态 |
| /b vip add &lt;player&gt; &lt;vipcode&gt; &lt;count&gt; | 给玩家VIP状态（不扣点券，需玩家在线） |
| /b vip del &lt;player&gt; | 删除玩家VIP状态 |
| /b vip clean | 马上执行一次VIP清理逻辑 |
| /b vip reload | 重新载入配置 |

## Config.yml文件示范

含有配置说明，建议仔细阅读。cmd部分的定义，请参考 [McrmbShop插件中cmd定义说明](/sub-plugins/mcrmbshop?id=定义商品指令cmd)。

```yaml
##VIP中心的展示前缀
prefix: '§e【VIP中心】'

##插件指令，请勿随意修改
command: 'vip'

##全局VIP变化记录到server.log
log: true

##是否打开面板显示
gui_show: true

##VIP到期检测间隔时间（单位：秒），默认为1200秒（即20分钟一次）
checktime: 18000

##VIP到期执行语句,可用代码{player}代替玩家名
stopvip:
  - 'manudel {player}'
  - 'nick {player} off'
  - 'mail send {player} 您的VIP已经到期啦，请尽快续费！'

##修改称号的语句
prefix_command: 'manuaddv {player} prefix &b[{prefix}]'

##称号不可以出现的禁词
prefix_ban:
  - 管理
  - op
  - 服主
  - 腐竹
  - 警察
  - 城管
  - 你妈
  - 国王
  - 皇帝
  - '&'
  - '§'

##是否记录称号到vip.yml文件，如有玩家喜好使用特殊字符做称号，建议关闭此功能以免影响会员载入.
##如果发现载入错误切vip.yml文件变成了backup.vip.yml，请修复vip.yml文件，并把该功能关闭，关闭后不影响称号使用。
prefix_log: true

##VIP中心帮助说明文字
help:  '
§e===================McrmbVIP会员系统帮助说明===================<br>
§e※列出VIP： /{command}  输入此指令可以展示所有VIP组的信息<br>
§e※预购VIP： /{command} buy <VIP代码> <数量>  用此指令预购VIP(不会扣钱)<br>
§b※指令示范： /{command} buy v3 1   使用此指令可查看购买1个月vip3的信息!<br>
§e※确认VIP： /{command} buy <VIP代码> <数量> sure  用此指令正式购买VIP<br>
§b※指令示范： /{command} buy v3 1 sure   使用此指令确认购买1个月VIP3!(会扣钱)<br>
§e※我的VIP： /{command} me  显示我的VIP详情及历史<br>
§e※修改称号： /{command} prefix <称号>  自助修改VIP称号,违规用词将封禁<br>
§e※显示帮助： /{command} help  显示此帮助说明<br>
§e※点券中心： /b  查看点券插件相关指令(充卡,查余额,消费记录等)<br>
§6=======================管理员专用指令=======================<br>
§6※查看所有VIP： /{command} list [VIP代码] 查看所有玩家的VIP（列表）<br>
§6※查看玩家VIP： /{command} show <玩家名>  查看玩家的VIP详情及历史<br>
§6※免费给予VIP： /{command} add <玩家名> <VIP代码> <份数> 给玩家免费开VIP，玩家必须在线，效果与玩家自行购买一致，但不扣点券。<br>
§6※强删玩家VIP： /{command} del <玩家名>  强制删除某个玩家的VIP<br>
§6※手动清理过期VIP： /{command} clean 立马执行一次过期VIP清理工作<br>
§6※重载配置文件： /{command} reload 重新载入一次配置文件<br>
§e=================================================================='

##VIP分组
##请注意，VIP分组代码（即下方的“v1”字样），一经设定不得修改！ 除非手动把vip.yml中的group也全部相应的修改好！ 或是还未有玩家开通相应的组。
vipgroups:
  v1:
    name: '月付VIP1（示范，带权限需求）'
    longtext: '特权：随身工作台，商店创建免申请，6个领地，领地上限300格，圈地价格0.04每格子。奖励：赠送钻石15个，游戏币 3000 个，每月续费时发放。'
    price: 15  ##VIP价格
    days: 30  ##VIP天数
    prefix: 0  ##VIP每月可以修改称号次数
    prefix_max: 0   ##VIP称号最大位数
    brocast: false   ##是否全服公告购买
    zs: true    ##是否允许折算，永久和年费最好禁止折算（设为false），月付的可以折算
    itemid: '264:0'       ##物品ID值'id:durability'     必须带单引号,冒号为英文冒号,  如何查看物品id？ 手拿物品输入/itemid
    slot: 0           ##在面板上的位置，从0开始表示面板上的第一个位置，比如：0,1,2，面板每行有九个位置， 所以第一行是0-8，第二行是9-17~~~ 以此类推
    permission: 'mcrmb.vip.v1'    ## 设置此项后，系统将检测玩家是否拥有此权限节点，有才可以购买此VIP！
    cmd: #以下为VIP开通时执行的指令，请修改为合适的指令。{player}=玩家名， {sum}=玩家购买份数乘以【|||后的数字】
    - 'manuadd {player} vip1'
    - 'give {player} 264 {sum}|||15'
    - 'eco give {player} {sum}|||3000'
    stopvip: #VIP到期时执行语句（可选），若停止VIP的指令一致，请在配置文件最上方配置stopvip节点即可，若不统一则在此节点分别设置
    - 'manudel {player}'
    - 'nick {player} off'
    - 'mail send {player} 您的VIP已经到期啦，请尽快续费！'
  v2:
    name: '月付VIP2（示范）'
    longtext: '特权：包含[VIP1]特权，飞行，昵称，随身末影箱，7个领地，领地上限350格，圈地价格0.03每格子。奖励：赠送钻石30个，游戏币 8000 个，每月续费时发放。'
    price: 30
    days: 30
    prefix: 0
    prefix_max: 0
    brocast: false
    zs: true
    itemid: '264:0'
    slot: 2
    cmd:
    - 'manuadd {player} vip2'
    - 'give {player} 264 {sum}|||30'
    - 'eco give {player} {sum}|||8000'
  v3:
    name: '月付VIP3（示范）'
    longtext: '特权：包含[VIP2]特权，查询破坏，时间自定义，饥饿恢复，工具修复，特别称号(3个中文,每月可改一次)，8个领地，领地上限400格，圈地价格0.02每格子。奖励：赠送钻石50个，游戏币 12000 个，每月续费时发放。'
    price: 50
    days: 30
    prefix: 1
    prefix_max: 3
    brocast: true
    zs: true
    itemid: '264:0'
    slot: 4
    cmd:
    - 'manuadd {player} vip3'
    - 'give {player} 264 {sum}|||50'
    - 'eco give {player} {sum}|||12000'
  v4:
    name: '月付VIP4（示范）'
    longtext: '特权：包含 [VIP3] 所有特权，10个领地，领地上限500格，圈地价格0.01每格子，特别称号(支持到4个中文,每月可改一次)。奖励：赠送钻石100个，游戏币 30000个，每月续费时发放。'
    price: 100
    days: 30
    prefix: 1
    prefix_max: 4
    brocast: true
    zs: true
    itemid: '264:0'
    slot: 6
    cmd:
    - 'manuadd {player} vip4'
    - 'give {player} 264 {sum}|||100'
    - 'eco give {player} {sum}|||30000'
  nv3:
    name: '年费VIP3（示范）'
    longtext: '得到 [VIP3] 权限一年，每年续费奖励钻石300，游戏币10万，称号修改权6次。称号长度3位。'
    price: 300
    days: 365
    prefix: 6
    prefix_max: 3
    brocast: true
    zs: false
    itemid: '264:0'
    slot: 8
    cmd:
    - 'manuadd {player} vip3'
    - 'give {player} 264 {sum}|||300'
    - 'eco give {player} {sum}|||100000'
  nv4:
    name: '年费VIP4（示范）'
    longtext: '得到 [VIP4] 权限一年，每年续费奖励钻石600，游戏币15万，称号修改权12次。称号长度4位。'
    price: 600
    days: 365
    prefix: 12
    prefix_max: 4
    brocast: true
    zs: false
    itemid: '264:0'
    slot: 9
    cmd:
    - 'manuadd {player} vip3'
    - 'give {player} 264 {sum}|||600'
    - 'eco give {player} {sum}|||150000'
  yv3:
    name: '永久VIP3（示范）'
    longtext: '得到 [VIP3] 权限永久，一次性奖励钻石300个，游戏币10万，刷怪笼1个，称号修改权12次。'
    price: 600
    days: 36500
    prefix: 12
    prefix_max: 3
    brocast: true
    zs: false
    itemid: '264:0'
    slot: 11
    cmd:
    - 'manuadd {player} vip3'
    - 'give {player} 264 {sum}|||300'
    - 'eco give {player} {sum}|||100000'
    - 'give {player} 52 1'
  yv4:
    name: '永久VIP4（示范）'
    longtext: '得到 [VIP4] 权限永久，一次性奖励钻石600个，游戏币15万，刷怪笼1个，龙蛋1个(申请warp用)，称号修改权24次。'
    price: 1200
    days: 36500
    prefix: 24
    prefix_max: 4
    brocast: true
    zs: false
    itemid: '264:0'
    slot: 13
    cmd:
    - 'manuadd {player} vip4'
    - 'give {player} 264 {sum}|||600'
    - 'eco give {player} {sum}|||150000'
    - 'give {player} 52 1'
    - 'give {player} 122 1'
```

