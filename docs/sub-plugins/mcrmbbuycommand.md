# McrmbBuyCommand

## 简介

该插件的作用接近于McrmbShop，主要功能为定义商品的发放。  
不同之处是McrmbShop插件需要在配置文件里定义好商品信息，而McrmbBuyCommand插件的商品信息定义在指令当中，**指令内容就是配置内容**。

!>**因此，该插件的指令绝对不能让普通玩家直接有权限执行。**  
通常情况下由其他插件来执行McrmbBuyCommand的指令，或服主在后台测试指令时可以直接执行，最新版本的McrmbBuyCommand插件已经限制非后台执行指令。

## 指令结构
/bbuy <目标玩家> <点券价格> <消费说明/商品名> <商品命令，使用中文分号`；`分割>
![](../.gitbook/assets/image%20%2811%29.png)

### 指令参数说明
| 参数 | 说明 |
|:---|:---|
| 目标玩家 | 玩家名，该操作执行后的目标玩家名 |
| 点券价格 | 该操作/商品需要消耗目标玩家多少个点券？ |
| 商品名/消费说明 | 商品名字，公屏展示时出现 |
| 商品命令 | 使用中文分号；分割的多个预设指令，其中{player}为玩家名变量。<br>以op:开头的指令将临时给予玩家权限并由玩家执行。<br><br>例：<br>`op:say hi；eco give {player} 100`<br>以上指令，玩家将以OP身份发布全服公告“hi”（由玩家身份执行），并获得100个金币（由后台执行） |


###  指令示范1

`/bbuy wbb 50 钻石剑与一百金币 give {player} 276 1；eco give {player} 100` 

该指令执行之后，会作出以下操作：

* 检测玩家wbb是否有50点券并扣款 
* 扣款成功则后台执行2个指令： give wbb 276 1 及 eco give wbb 100 
* 扣款失败则不执行任何指令，通知玩家wbb提示点券不足。 

####  后台效果

（注意，本插件实际运用应结合其他可以【执行后台指令】的插件，而不是服主手工在后台打指令。）

![](../.gitbook/assets/69605896gy1fsz1o1axnbj20qm0423yk.jpg)

#### 前台效果

![](../.gitbook/assets/69605896gy1fsz1mg3gknj20i80310t1.jpg)



### 指令示范2

`/bbuy wbb 100 公屏HI公屏Hello op:say HI；op:say Hello` 

该指令执行之后，会作出以下操作：

* 检测玩家wbb是否有100点券并扣款
* 扣款成功则后台执行2个指令： 给wbb临时op，强制其执行say HI； 给wbb临时op，强制其执行say Hello 
* 扣款失败则不执行任何指令，通知玩家wbb提示点券不足。

#### 后台效果

![](../.gitbook/assets/image%20%283%29.png)

#### 前台效果

![](../.gitbook/assets/image.png)





## 配合其他插件用法（请阅读）

### 配合 ChestCommands 灵活运用

```yaml
COMMAND: 'console: bbuy {player} 50 购买钻石剑与100金币 give {player} 276 1；eco give {player} 100'
```

这样配置可以实现玩家点击按钮之后进行点券消费购买道具及金币。

?>注意，ChestCommand的玩家名变量为{player}，虽然与McrmbBuyCommand一样，但不会冲突，不必担心。
### 配合 Bossshop 灵活运用

```yaml
RewardType: command
Reward:
- bbuy %player% 50 购买钻石剑与100金币 give {player} 276 1；eco give {player} 100
```

这样配置可以实现玩家点击按钮之后进行点券消费购买道具及金币。

?> 注意，Bossshop的玩家名变量为 %player%，但McrmbBuyCommand自身有变量名可以获取玩家名，所以上面的指令，后面的2个 {player} 写成 %player% ，其实也是可以的。  
写{player}，玩家点击后，服务器会执行 【bbuy 玩家名 50 购买钻石剑与100金币 give {player} 276 1;eco give {player} 100】，实现玩家的购买。  
写%player%，玩家点击后，服务器会执行【bbuy 玩家名 50 购买钻石剑与100金币 give 玩家名 276 1;eco give 玩家名 100】，依然实现玩家的购买。  

## 插件下载

http://ci.mcrmb.com/job/McrmbBuyCommand/

?>CI持续构建平台（ci.mcrmb.com）看上去很复杂，其实只需要点进相应的项目，拉到下方，找到最终成功构建，下载jar文件即可，Mcrmb平台各插件均在CI发布。
