# McrmbDraw

## 简介

该插件用于定义服务器抽奖型商品及自动管理服务器内的VIP  
工作原理与McrmbShop插件一致，不同的地方在于增加了一些逻辑：

* 数量每次只能为1
* 到期之后可以根据预设的指令自动取消权限组 
* 根据预设的指令可以让玩家自行设置称号
* 称号中可以添加敏感词

## 指令

### 玩家

| 指令                           | 说明                               | 
|:-----------------------------|:---------------------------------| 
| /b lucky                     | 查询所有抽奖代码，GUI模式直接出GUI面板           | 
| /b lucky buy \<drawcode\>      | 使用指令购买抽奖（展示性）<br>drawcode = 抽奖代码 | 
| /b lucky buy \<drawcode\> sure | 确认购买抽奖                           | 
| /b lucky help                | 展示帮助信息                           | 


### 管理

| 指令 | 说明 |
| :--- | :--- |
| /b lucky clean \<drawcode\> | 清理某个抽奖项目的累计数据 |
| /b vip reload | 重新载入配置 |

## 权重计算方式

玩家抽奖的时候中某一个奖项的概率，等于该奖项的权重除以该抽奖所有奖项权重的总和。  
所以权重不需要保证加起来等于100，例如你设置了两个奖项，权重各为1，则他们的中奖几率都是1/2=50%

## Config.yml文件示范

含有配置说明，建议仔细阅读。cmd部分的定义，请参考 [McrmbShop插件中cmd定义说明](/sub-plugins/mcrmbshop?id=定义商品指令cmd)。

```yaml
## McrmbDraw插件前缀
prefix: §e【抽奖】

## 指令单词,请勿随意修改! 若修改必须同时修改plugin.yml
command: 'lucky'

##是否打开面板显示
gui_show: true

##帮助文档, <br>代表换行
help:  '
§e===================MCRMB抽奖系统帮助说明===================<br>
§e※抽奖列表： /{command}  输入此指令可以展示所有抽奖的代码<br>
§e※抽奖下单： /{command} buy <抽奖代码> 用此指令预抽奖(不会扣钱)<br>
§b※示范： /{command} buy xncj  使用此指令可查看购买1个xncj的花费!<br>
§e※抽奖下单： /{command} buy <抽奖代码> sure  用此指令正式抽奖<br>
§b※示范： /{command} buy xncj sure   使用此指令确认购买1个xncj!(会扣钱)<br>
§e※显示帮助： /{command} help 显示此帮助说明<br>
§e※重新加载： /{command} reload 管理员专用重载指令<br>
§e※清理抽奖数据： /{command} clean <抽奖代码> 管理员专用清理指令<br>
§e※点券中心： /b  查看点券插件相关指令(充卡,查余额,消费记录等)<br>
§e=================================================================='

##中奖提示, <br>代表换行
msg:  '
§e═════════════════════════════════════════════════════<br>
§e   恭喜玩家【{player}】在抽奖【{cjdm}】中抽中了【{win}】！<br>
§e═════════════════════════════════════════════════════'

#请全部使用小写,以免出错.
#第一层为抽奖代码, 例如下面的"xncj"
#name 为抽奖名称, 将会传输到MCRMB系统, 若开放公屏展示, 也将在公屏里出现.
#text 为抽奖简介
#yxb 为支付方式，true则为游戏币支付， false则为点券支付
#price 为一次抽奖价格 (花费点券或游戏币)
#brocast 必须是true或false, 意味着是否公告展示玩家的抽奖购买操作.
#probability 为没个奖项的中奖权重值，为正整数即可。     奖项中奖几率 = 奖项的权重 / 所有奖项的权重总和

# limit 为是否限制用户抽奖次数，为0 则不限

# itemid 为展示物品id，必须带单引号,冒号为英文冒号（半角！），
# slot 在面板上的位置，从0开始表示面板上的第一个位置，比如：0,1,2，面板每行有九个位置， 所以第一行是0-8，第二行是9-17~~~ 以此类推

# permission 允许服主设置一个权限节点，有这个权限节点的玩家，才可以抽这个奖。  注意，如果不需要，请删掉整行设置，不要留空

#award是关键部分, 每行代表一个指令, 可以协助你实现几乎任何功能. award内的{player}代表替换的玩家名，1、2、3等分别为中奖类型，与probability概率对应。
#例如XXX抽奖，奖品为50个钻石，若中奖，那么cmd内则是这样写: - 'give {player} 264 50'


draws:
  xncj:
    name: '新年抽奖'
    text: '猴年抽奖活动！！！100%中奖！，1%： 88个钻石+8888游戏币！，4%： 50个钻石+5000游戏币，95%： 10个钻石+2000游戏币'    #奖项之间可使用空格隔开
    yxb: false
    price: 6
    limit: 0
    brocast: false
    itemid: '138'
    slot: 0
    permission: 'mcrmb.lucky.test'
    awards:
      0:
        name: '88个钻石 + 8888个游戏币'   #告示内容
        brocast: true   #中奖后是否全服告示
        probability: 1   #权重
        cmd:
        - 'give {player} 264 88'
        - 'eco give {player} 8888'
      1:
        name: '50个钻石 + 5000游戏币'
        brocast: true  #中奖后是否全服告示
        probability: 4   #权重
        cmd:
        - 'give {player} 264 50'
        - 'eco give {player} 5000'
      2:
        name: '20个钻石 + 2000游戏币'
        brocast: true  #中奖后是否全服告示
        probability: 4   #权重
        cmd:
        - 'give {player} 264 20'
        - 'eco give {player} 2000'
  zncj:
    name: '游戏币抽钻石'
    text: '游戏币抽奖活动！！！100%中奖！，100游戏币随机抽取钻石，最多可得100个！，最少也得3个钻石！，安慰奖2个熟猪排~'
    yxb: true
    price: 100
    brocast: true
    limit: 3
    itemid: '264:0'
    slot: 1
    awards:
      0:
        name: '100个钻石'
        brocast: true   #中奖后是否全服告示
        probability: 1   #权重
        cmd:
        - 'give {player} 264 100'
      1:
        name: '50个钻石'
        brocast: true   #中奖后是否全服告示
        probability: 2   #权重
        cmd:
        - 'give {player} 264 50'
      2:
        name: '10个钻石'
        brocast: true   #中奖后是否全服告示
        probability: 50   #权重
        cmd:
        - 'give {player} 264 10'
      3:
        name: '3个钻石'
        brocast: false   #中奖后是否全服告示
        probability: 150   #权重
        cmd:
        - 'give {player} 264 3'
      4:
        name: '安慰2个熟猪排'
        brocast: false   #中奖后是否全服告示
        probability: 150   #权重
        cmd:
        - 'give {player} 320 2'
```

