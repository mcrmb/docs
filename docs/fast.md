# 快速部署MCRMB

很多服主使用了PlayerPoints（后续简称PP）作为服务器点券管理插件，早期，Mcrmb提供了魔改版的PlayerPoints供服主接入使用，目前最新的核心插件版本已经支持转入原生PP模式。


<!-- tabs:start -->

#### ** 自定义转入模式 **

本功能需要 **[MCRMB核心插件2.0b17](http://ci.mcrmb.com/job/MCRMB/90/artifact/pg/)**  
类似`转入原生PlayerPoints模式`，兼容性更强，可以自定义指令内容。  
只要你的本地点券系统有转入指令，即可实现转入，3种方式执行指令（后台、玩家、玩家临时OP）。  
指令定义教程请查阅[【核心插件说明】](/core-plugin)，搜索`transfer_mode_cmds`配置项。  
在该模式下，Mcrmb云平台的玩家余额仅起到桥梁作用：

* 玩家充值，MCRMB平台余额增加
* 玩家 `登陆服务器` 或 在 `服务器内查询点券余额` 或 `2.0b17以上版本核心插件内充值完成后`
* 插件启动转入逻辑，将余额提取并 `执行你定义好的指令` ，并 `扣减云平台余额`
* 完成点券转入

#### ** 转入原生PlayerPoints **

该模式下，Mcrmb云平台的玩家余额仅起到桥梁作用：

* 玩家充值，云平台余额增加
* 玩家 `登陆服务器` 或 在 `服务器内查询点券余额` 或 `2.0b17以上版本核心插件内充值完成后`
* 插件启动转入逻辑，将余额 `转入PlayerPoints` ，并 `扣减云平台余额`
* 完成点券转入

#### ** 魔改PlayerPoints **

该模式下，PlayerPoints的点券查询逻辑被嫁接至MCRMB云平台：

* 玩家充值，云平台余额增加
* 玩家在游戏内消费，相关插件调用PlayerPoints，PlayerPoints调用Mcrmb核心插件接口，完成操作。

<!-- tabs:end -->


?>三种模式如何选择：  
**魔改模式：**点券上云，可轻松实现跨服点券同步；但因为PlayerPoints的发起逻辑处于主线程内，网络不佳时将导致Mcrmb的联网查询请求卡服。  
**转入模式：**配置更快速。且解决了魔改模式的卡服问题，所有联网请求剥离主线程，但因点券不存在云上，所以服务器如果需要多服同步点券，需要使用PlayerPoints或你的本地点券系统的MYSQL。
本文简单介绍如何快速接入MCRMB，使用的是 `转入原生PP` 模式。

## 1、注册平台账号

https://www.mcrmb.com/User/showReg

## 2、新增一个服务器

![](.gitbook/assets/image%20%284%29.png)

## 3、取得服务器的SID和KEY

![](.gitbook/assets/image%20%286%29.png)

!>SID与KEY请不要泄露，如不慎泄露，请到服务器管理中点击秘钥右侧的橙色按钮重置秘钥。
## 4、下载核心插件

核心插件请在此处下载：  
[http://ci.mcrmb.com/job/MCRMB/](http://ci.mcrmb.com/job/MCRMB/)

![](.gitbook/assets/image%20%281%29.png)

将最新版本的核心插件放置于服务器的`plugins`目录，启动服务器。  
确保服务器中已有`PlayerPoints原版`插件。

## 5、配置SID与KEY进行对接

启动完毕后，插件会提示为配置`sid`与`key`。

![](.gitbook/assets/image%20%289%29.png)

根据提示，将您的`sid`与`key`按照指令格式输入，回车执行，即可完成对接。

![](.gitbook/assets/image%20%288%29.png)

?> 因为一些服务器文件权限等问题，`/b setup` 指令可能无法修改 config.yml 文件，持续提示SID错误。  
这种情况下请自行修改 `plugins/Mcrmb/config.yml` 文件配置SID及KEY，并使用 `/b reload` 载入配置。  
如何判断SID已经配置并使用： 使用 `/b status` 指令可以查看插件状态信息。

## 6、修改配置为PlayerPoints转入模式

打开`plugins`目录中的`Mcrmb`文件夹，打开 `config.yml` 文件

找到playerpoints选项，将`false`改为`true`，注意不要删掉冒号后的空格。

![](.gitbook/assets/image%20%285%29.png)

改为

![](.gitbook/assets/image%20%2810%29.png)

回到服务器后台，输入 `b reload` 重载核心插件。

![](.gitbook/assets/image%20%2812%29.png)

现在，一切就绪！你的服务器玩家可以在游戏内完成充值点券到PlayerPoints了！

## 附：玩家充值方式

* 使用网页充值，链接请看MCRMB后台
* 游戏内扫码充值
  * 微信： /b wx &lt;金额&gt;
  * 支付宝： /b zfb &lt;金额&gt;
  * QQ钱包：/b qq &lt;金额&gt;
* 游戏内提交卡密充值
  * 查询卡密类型： /b cz
  * 卡密： /b cz &lt;卡密类型&gt; &lt;卡号&gt; &lt;密码&gt;
  
![](.gitbook/assets/20200416231926.png)

?>2.0b17及以上版本，扫码支付请求发起后，玩家如果在5分钟内完成支付，转入逻辑将自动运行并执行转入。  
2.0b17之前的版本，玩家充值完毕后需手动输入 `/b money` 即可转换点券到PlayerPoints账户中，该逻辑在玩家加入服务器时亦会执行一次。
