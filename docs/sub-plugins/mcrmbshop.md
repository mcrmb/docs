# McrmbShop

## 简介

该插件用于定义商品，在玩家购买的时候自动扣除Mcrmb云平台点券并发放商品。

## 指令


|指令|说明|
|:---|:---|
|/b shop|查询所有商品代码，GUI模式直接出GUI面板|
|/b shop buy \<shopcode\> \<count\>|使用指令购买商品（展示性）<br>shopcode = 商品代码<br>count = 购买数量|
|/b shop buy \<shopcode\> \<count\> sure|确认购买商品|
|/b shop help|展示帮助信息|
|/b shop reload|重载插件（管理员可用）|
|/b shop del \<shopcode\>|下架商品|


## 商品的本质
商品在本插件中均需要为指令可发送的内容：  
例如你需要给玩家发钻石，则准备好这样的指令 `give player diamond 1`  
需要给玩家加金币，则准备好类似这样的指令 `eco give player 100`  

## 定义商品指令\(cmd\)

config.yml文件中，`cmd`为发放商品的指令模板，其中有两个变量可以使用  
`{player}` 将在最终执行时被替换为玩家名， `{sum}` 将在最终执行是被替换为总数量。  
指令后的 `|||` 之后的数值，代表基数。用于计算出最终的总数量，  `基数\*购买份数` = `总数量`。

?>例如定义cmd内容为：  eco give {player} {sum}\|\|\|100  
玩家 testing 购买该商品5份，则最终将生成的指令为：  
eco give testing 500  
  
这个500的计算来源是 基数\(100\)\*购买份数\(5\) = 500  
所以这个商品的名字，可以叫做“100金币”
## 商品定义中的其他参数 

<table>
  <thead>
    <tr>
      <th style="text-align:left">参数</th>
      <th style="text-align:left">含义</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">name</td>
      <td style="text-align:left">商品名字</td>
    </tr>
    <tr>
      <td style="text-align:left">text</td>
      <td style="text-align:left">商品说明</td>
    </tr>
    <tr>
      <td style="text-align:left">price</td>
      <td style="text-align:left">商品价格</td>
    </tr>
    <tr>
      <td style="text-align:left">min</td>
      <td style="text-align:left">单次购买下限，0则不限</td>
    </tr>
    <tr>
      <td style="text-align:left">max</td>
      <td style="text-align:left">单次购买上限，0则不限</td>
    </tr>
    <tr>
      <td style="text-align:left">count</td>
      <td style="text-align:left">物品总数量，0则不限</td>
    </tr>
    <tr>
      <td style="text-align:left">quantity</td>
      <td style="text-align:left">每玩家限购数量，0则不限</td>
    </tr>
    <tr>
      <td style="text-align:left">brocast</td>
      <td style="text-align:left">购买后是否全服公告购买</td>
    </tr>
    <tr>
      <td style="text-align:left">itemid</td>
      <td style="text-align:left">GUI 物品ID值</td>
    </tr>
    <tr>
      <td style="text-align:left">slot</td>
      <td style="text-align:left">
      在面板上的Slot位置，如：0,1,2，<br>
      面板每行有九个位置，第一行是0-8，第二行是9-17，以此类推
      </td>
    </tr>
    <tr>
      <td style="text-align:left">permission</td>
      <td style="text-align:left">
        购买此商品需要的特殊权限节点，如&quot;vip.yxb.100&quot;<br>设置此项后，系统将检测玩家是否拥有此权限节点，有才可以购买此商品
      </td>
    </tr>
    <tr>
      <td style="text-align:left">cmd</td>
      <td style="text-align:left">
        商品具体内容指令集，为数组形式。<br>实例：<br>- 'say 玩家【{player}】在此祝全服朋友新年快乐~!
          我也要送:/bshop'<br>- 'give {player} 322:1 1'<br>- 'give {player} 264 20'<br><br>请务必注意缩进格式。建议是根据默认config.yml中修改，不要随便删除空格。
      </td>
    </tr>
  </tbody>
</table>

## Config.yml文件示例


```yaml
## McrmbShop插件前缀
prefix: §e【点券商城】

## 指令单词,请勿随意修改! 若修改必须同时修改plugin.yml
command: 'shop'

##是否打开面板显示
gui_show: true

##帮助文档, <br>代表换行 
help:  '
§e===================MCRMB点券商城系统帮助说明===================<br>
§e※商品列表： /{command}  输入此指令可以展示所有商品的代码<br>
§e※商品下单： /{command} buy <商品代码> <数量>  用此指令预购商品(不会扣钱)<br>
§b※示范： /{command} buy zs 1   使用此指令可查看购买1个钻石(x10)的花费!<br>
§e※商品下单： /{command} buy <商品代码> <数量> sure  用此指令正式购买商品<br>
§b※示范： /{command} buy zs 1 sure   使用此指令确认购买1个钻石(x10)!(会扣钱)<br>
§e※显示帮助： /{command} help 显示此帮助说明<br>
§e※重新加载： /{command} reload 管理员专用重载指令<br>
§e※下架商品： /{command} del <商品代码> 管理员专用下架指令<br>
§e※点券中心： /b  查看点券插件相关指令(充卡,查余额,消费记录等)<br>
§e=================================================================='

#请全部使用小写,以免出错.
#第一层为商品代码, 例如下面的"xnlb"
#name 为商品名称, 将会传输到MCRMB系统, 若开放公屏展示, 也将在公屏里出现.
#text 为商品简介
#price 为商品价格(花费点券)
#brocast 必须是true或false, 意味着是否公屏展示玩家的购买操作.

#cmd是关键部分, 每行代表一个指令, 可以协助你实现几乎任何功能. cmd内的{player}代表替换的玩家名. {sum}代表数量, 若商品有数量基数, 请在指令最后打上基数, 用"|||"符号隔开!
#例如XXX商品包含50个钻石且可以购买多件(如果单次只允许购买一件,可以不打基数),  那么cmd内则是这样写: - 'give {player} 264 {sum}|||50' , sum将由50乘以玩家购买商品件数所得. 例如玩家买2件, 那么最终生成指令是 give 玩家 264 100, 因为2*50=100

#min和max是商品允许单次购买的最大额和最小额, min和max是可选的, 可以不设置, 若不设置请勿留空, 请直接去掉, 就如下面的"vip1"商品
#2016.1.13
#count 为商品的库存(0表示没有限制)
#quantity 为玩家可购买商品的总数(0表示无限制)

shops:
  vipyxb:
    name: '游戏币200个'
    text: '200游戏币(VIP专享)'
    price: 1
    min: 1    #单次购买的最小数额，0则不限
    max: 0    #单次购买的最大数额，0则不限
    count: 30    #这个商品一共卖几件？
    quantity: 2   #这个商品每个玩家最多可以买几件？
    brocast: true     #购买后是否全服公告？
    itemid: '371:0'      ##物品ID值： 必须带单引号,冒号为英文冒号,'id:durability',如何查看物品id？ 手拿物品输入/itemid
    slot: 2    ##在面板上的位置，从0开始表示面板上的第一个位置，比如：0,1,2，面板每行有九个位置， 所以第一行是0-8，第二行是9-17~~~ 以此类推. 不支持翻页
    permission: 'vip.yxb.200'    #设置此项后，系统将检测玩家是否拥有此权限节点，有才可以购买此商品！
    cmd:
    - 'eco give {player} {sum}|||200'
  xnlb:
    name: '新年礼包'
    text: '[Server]权限代你送新春祝福+钻石20个+金苹果'
    price: 5
    min: 1
    max: 1
    count: 2
    quantity: 3
    brocast: true
    itemid: '260'
    slot: 0
    cmd:
    - 'say 玩家【{player}】在此祝全服朋友新年快乐~! 我也要送:/bshop'
    - 'give {player} 322:1 1'
    - 'give {player} 264 20'
  yxb:
    name: '游戏币100个'
    text: '100游戏币!!'
    price: 1
    min: 1
    max: 99
    count: 3
    quantity: 2
    brocast: true
    itemid: '371:0'
    slot: 1
    cmd:
    - 'eco give {player} {sum}|||100'
```


