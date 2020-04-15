# McrmbShop

## 简介

该插件用于定义商品，在玩家购买的时候自动扣除Mcrmb云平台点券并发放商品。

## 指令

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#x6307;&#x4EE4;</th>
      <th style="text-align:left">&#x8BF4;&#x660E;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">/b shop</td>
      <td style="text-align:left">&#x67E5;&#x8BE2;&#x6240;&#x6709;&#x5546;&#x54C1;&#x4EE3;&#x7801;&#xFF0C;GUI&#x6A21;&#x5F0F;&#x76F4;&#x63A5;&#x51FA;GUI&#x9762;&#x677F;</td>
    </tr>
    <tr>
      <td style="text-align:left">/b shop buy &lt;shopcode&gt; &lt;count&gt;</td>
      <td style="text-align:left">
        <p>&#x4F7F;&#x7528;&#x6307;&#x4EE4;&#x8D2D;&#x4E70;&#x5546;&#x54C1;&#xFF08;&#x5C55;&#x793A;&#x6027;&#xFF09;</p>
        <p>shopcode = &#x5546;&#x54C1;&#x4EE3;&#x7801;</p>
        <p>count = &#x8D2D;&#x4E70;&#x6570;&#x91CF;</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">/b shop buy &lt;shopcode&gt; &lt;count&gt; sure</td>
      <td style="text-align:left">&#x786E;&#x8BA4;&#x8D2D;&#x4E70;&#x5546;&#x54C1;</td>
    </tr>
    <tr>
      <td style="text-align:left">/b shop help</td>
      <td style="text-align:left">&#x5C55;&#x793A;&#x5E2E;&#x52A9;&#x4FE1;&#x606F;</td>
    </tr>
    <tr>
      <td style="text-align:left">/b shop reload</td>
      <td style="text-align:left">&#x91CD;&#x8F7D;&#x63D2;&#x4EF6;&#xFF08;&#x7BA1;&#x7406;&#x5458;&#x53EF;&#x7528;&#xFF09;</td>
    </tr>
    <tr>
      <td style="text-align:left">/b shop del &lt;shopcode&gt;</td>
      <td style="text-align:left">&#x4E0B;&#x67B6;&#x5546;&#x54C1;</td>
    </tr>
  </tbody>
</table>## 商品的本质

商品在本插件中均为指令，例如你需要给玩家发钻石，则准备好这样的指令 `give player diamond 1`，需要给玩家加金币，则准备好类似这样的指令 `eco give player 100`

## 定义商品指令\(cmd\)

config.yml文件中，`cmd`为发放商品的指令模板，其中有两个变量可以使用  
`{player}` 将在最终执行时被替换为玩家名。  
`{sum}` 将在最终执行是被替换为总数量。  
指令后的 `|||` 之后的数值，代表基数。用于计算出最终的总数量。  基数\*购买份数 = 总数量

?>例如定义cmd内容为：  eco give {player} {sum}\|\|\|100  
玩家 testing 购买该商品5份，则最终将生成的指令为：  
eco give testing 500  
  
这个500的计算来源是 基数\(100\)\*购买份数\(5\) = 500  
所以这个商品的名字，可以叫做“100金币”
## 商品定义中的其他参数

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#x53C2;&#x6570;</th>
      <th style="text-align:left">&#x542B;&#x4E49;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">name</td>
      <td style="text-align:left">&#x5546;&#x54C1;&#x540D;&#x5B57;</td>
    </tr>
    <tr>
      <td style="text-align:left">text</td>
      <td style="text-align:left">&#x5546;&#x54C1;&#x8BF4;&#x660E;</td>
    </tr>
    <tr>
      <td style="text-align:left">price</td>
      <td style="text-align:left">&#x5546;&#x54C1;&#x4EF7;&#x683C;</td>
    </tr>
    <tr>
      <td style="text-align:left">min</td>
      <td style="text-align:left">&#x5355;&#x6B21;&#x8D2D;&#x4E70;&#x4E0B;&#x9650;&#xFF0C;0&#x5219;&#x4E0D;&#x9650;</td>
    </tr>
    <tr>
      <td style="text-align:left">max</td>
      <td style="text-align:left">&#x5355;&#x6B21;&#x8D2D;&#x4E70;&#x4E0A;&#x9650;&#xFF0C;0&#x5219;&#x4E0D;&#x9650;</td>
    </tr>
    <tr>
      <td style="text-align:left">count</td>
      <td style="text-align:left">&#x7269;&#x54C1;&#x603B;&#x6570;&#x91CF;&#xFF0C;0&#x5219;&#x4E0D;&#x9650;</td>
    </tr>
    <tr>
      <td style="text-align:left">quantity</td>
      <td style="text-align:left">&#x6BCF;&#x73A9;&#x5BB6;&#x9650;&#x8D2D;&#x6570;&#x91CF;&#xFF0C;0&#x5219;&#x4E0D;&#x9650;</td>
    </tr>
    <tr>
      <td style="text-align:left">brocast</td>
      <td style="text-align:left">&#x8D2D;&#x4E70;&#x540E;&#x662F;&#x5426;&#x5168;&#x670D;&#x516C;&#x544A;&#x8D2D;&#x4E70;</td>
    </tr>
    <tr>
      <td style="text-align:left">itemid</td>
      <td style="text-align:left">GUI &#x7269;&#x54C1;ID&#x503C;</td>
    </tr>
    <tr>
      <td style="text-align:left">slot</td>
      <td style="text-align:left">&#x5728;&#x9762;&#x677F;&#x4E0A;&#x7684;Slot&#x4F4D;&#x7F6E;&#xFF0C;&#x5982;&#xFF1A;0,1,2&#xFF0C;
        <br
        />&#x9762;&#x677F;&#x6BCF;&#x884C;&#x6709;&#x4E5D;&#x4E2A;&#x4F4D;&#x7F6E;&#xFF0C;
        &#x7B2C;&#x4E00;&#x884C;&#x662F;0-8&#xFF0C;&#x7B2C;&#x4E8C;&#x884C;&#x662F;9-17&#xFF0C;&#x4EE5;&#x6B64;&#x7C7B;&#x63A8;</td>
    </tr>
    <tr>
      <td style="text-align:left">permission</td>
      <td style="text-align:left">
        <p>&#x8D2D;&#x4E70;&#x6B64;&#x5546;&#x54C1;&#x9700;&#x8981;&#x7684;&#x7279;&#x6B8A;&#x6743;&#x9650;&#x8282;&#x70B9;&#xFF0C;&#x5982;&quot;vip.yxb.100&quot;</p>
        <p>&#x8BBE;&#x7F6E;&#x6B64;&#x9879;&#x540E;&#xFF0C;&#x7CFB;&#x7EDF;&#x5C06;&#x68C0;&#x6D4B;&#x73A9;&#x5BB6;&#x662F;&#x5426;&#x62E5;&#x6709;&#x6B64;&#x6743;&#x9650;&#x8282;&#x70B9;&#xFF0C;&#x6709;&#x624D;&#x53EF;&#x4EE5;&#x8D2D;&#x4E70;&#x6B64;&#x5546;&#x54C1;</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">cmd</td>
      <td style="text-align:left">
        <p>&#x5546;&#x54C1;&#x5177;&#x4F53;&#x5185;&#x5BB9;&#x6307;&#x4EE4;&#x96C6;&#xFF0C;&#x4E3A;&#x6570;&#x7EC4;&#x5F62;&#x5F0F;&#x3002;</p>
        <p>&#x5B9E;&#x4F8B;&#xFF1A;</p>
        <p>- &apos;say &#x73A9;&#x5BB6;&#x3010;{player}&#x3011;&#x5728;&#x6B64;&#x795D;&#x5168;&#x670D;&#x670B;&#x53CB;&#x65B0;&#x5E74;&#x5FEB;&#x4E50;~!
          &#x6211;&#x4E5F;&#x8981;&#x9001;:/bshop&apos;</p>
        <p>- &apos;give {player} 322:1 1&apos;</p>
        <p>- &apos;give {player} 264 20&apos;</p>
        <p></p>
        <p>&#x8BF7;&#x52A1;&#x5FC5;&#x6CE8;&#x610F;&#x7F29;&#x8FDB;&#x683C;&#x5F0F;&#x3002;&#x5EFA;&#x8BAE;&#x662F;&#x6839;&#x636E;&#x9ED8;&#x8BA4;config.yml&#x4E2D;&#x4FEE;&#x6539;&#xFF0C;&#x4E0D;&#x8981;&#x968F;&#x4FBF;&#x5220;&#x9664;&#x7A7A;&#x683C;&#x3002;</p>
      </td>
    </tr>
  </tbody>
</table>## Config.yml文件示例


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


