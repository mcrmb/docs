# 快速部署MCRMB

很多服主使用了PlayerPoints（后续简称PP）作为服务器点券管理插件，早期，Mcrmb提供了魔改版的PlayerPoints供服主接入使用，目前最新的核心插件版本已经支持转入原生PP模式。


<!-- tabs:start -->

#### ** 转入原生PlayerPoints **

该模式下，Mcrmb云平台的玩家余额仅起到桥梁作用：

* 玩家充值，云平台余额增加
* 玩家 `登陆服务器` 或 在 `服务器内查询点券余额`
* 插件启动转入逻辑，将余额 `转入PlayerPoints` ，并 `扣减云平台余额`
* 完成点券转入

#### ** 魔改PlayerPoints **

该模式下，PlayerPoints的点券查询逻辑被嫁接至MCRMB云平台：

* 玩家充值，云平台余额增加
* 玩家在游戏内消费，相关插件调用PlayerPoints，PlayerPoints调用Mcrmb核心插件接口，完成操作。

<!-- tabs:end -->


?>两种模式各有特点。  
**魔改模式：**点券上云，可轻松实现跨服点券同步；但因为PlayerPoints的发起逻辑处于主线程内，网络不佳时将导致Mcrmb的联网查询请求卡服。  
**转入模式：**解决了魔改模式的卡服问题，所有联网请求剥离主线程，但因点券非存在云上，所以服务器如果需要多服同步点券，需要使用PlayerPoints的MYSQL。
本文简单介绍如何快速接入MCRMB，使用的是 `转入原生PP` 模式。

## 注册平台账号

https://www.mcrmb.com/User/showReg

## 新增一个服务器

![](.gitbook/assets/image%20%284%29.png)

## 得到服务器的SID和KEY

![](.gitbook/assets/image%20%286%29.png)

!>SID与KEY请不要泄露，如不慎泄露，请到服务器管理中点击秘钥右侧的橙色按钮重置秘钥。
## 下载核心插件

核心插件请在此处下载：  
[http://ci.mcrmb.com/job/MCRMB/](http://ci.mcrmb.com/job/MCRMB/)

![&#x5982;&#x56FE;&#xFF0C;&#x62C9;&#x52A8;&#x9875;&#x9762;&#x5230;&#x6700;&#x4E0B;&#x65B9;&#xFF0C;&#x4E0B;&#x8F7D;&#x6700;&#x7EC8;&#x6210;&#x529F;&#x6784;&#x5EFA;&#x4E2D;&#x7684;jar&#x6587;&#x4EF6;&#x3002;](.gitbook/assets/image%20%281%29.png)

将最新版本的核心插件放置于服务器的`plugins`目录，启动服务器。  
确保服务器中已有`PlayerPoints原版`插件。

## 配置SID与KEY进行对接

启动完毕后，插件会提示为配置`sid`与`key`。

![](.gitbook/assets/image%20%289%29.png)

根据提示，将您的`sid`与`key`按照指令格式输入，回车执行，即可完成对接。

![](.gitbook/assets/image%20%288%29.png)

## 修改配置为PlayerPoints转入模式

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

?>充值完成后，输入/b money 可以转换点券到PlayerPoints账户中，该逻辑在玩家加入服务器时亦会执行一次，建议结合菜单插件把这些指令做成按钮。
