# 插件层接口/事件文档

使用这些接口，您可以自己开发出合适服务器的附属插件~

***

## 示例插件
本插件中使用了本文档中提到的方法及事件，已经是一个完整可直接放服务器里跑的插件，建议新手阅读源码并测试。  
[https://gitee.com/mcrmb/mcrmb_sub_plugin_test](https://gitee.com/mcrmb/mcrmb_sub_plugin_test)


## 可用方法

以下方法均位于 `com.mcrmb.PayApi`，可以直接静态调用。  

| 作用 | 方法 | 返回值 |
| :--- | :--- | :--- |
| 查询玩家余额 | look\(String playername\) | int |
| 查询玩家累计充值 | allcharge\(String playername\) | int |
| 查询玩家累计消费 | allpay\(String playername\) | int |
| 玩家消费接口 | Pay\(String playername,String point,String useto,boolean brocast\) | boolean |
| 手工操作玩家点券 | Manual\(String playername,int type,String point,String text\) | boolean |


!> 因为需要走网络，所以请在调用时使用异步包裹！不知道怎么包裹请参考示例插件。  
打开 `/b debug` 模式下，会显示网络请求是否位于主线程，`非主线程` 则说明已经走了异步。 

### 参数值说明

| 参数名        | 说明                                   | 
|:-----------|:-------------------------------------| 
| playername | 玩家名，大小写均可                            | 
| point      | 点卷数量，注意，请转成String类型                  | 
| useto      | 消费说明                                 | 
| brocast    | 是否发公屏                                | 
| type       | 手工操作点卷类型<br>1=加款<br>2=扣款<br>3=重设点券金额 | 
| text       | 点券操作说明                               | 


### 参考代码

```java
import com.mcrmb.PayApi;
...
getServer().getScheduler().runTaskAsynchronously(this, new Runnable(){
    public void run() {
        String playername = "tester";
        System.out.print(PayApi.look(playername)); //打印玩家余额
    }
});
...
```

***

## 可用事件

| 事件 | 说明 | 
| :--- | :--- |
| com.mcrmb.event.McrmbPayEvent | 支付事件
| com.mcrmb.CmdEvent | 主插件指令事件

### 事件内方法表

事件 | 方法名 | 返回类型 | 说明 
:---| :--- | :--- | :--- 
McrmbPayEvent | getPlayer() | String | 玩家名字符串 
McrmbPayEvent | getMoney() | int | 成功消费的点券数量 
McrmbPayEvent | getReason() | String | 消费说明 
McrmbPayEvent | isBroadcast() | boolean | 是否进行公屏 
McrmbPayEvent | getLatestBalance() | int | 最新的点券余额 
McrmbPayEvent | getResponse() | Response | 返回状态，参见下表 
CmdEvent | getCmd() | String | 返回指令/b 后的 首个参数，用于判断是否应该进入逻辑
CmdEvent | getCmds() | String[] | 返回指令的参数数组。 
CmdEvent | getSender() | CommandSender | 返回指令的发送对象 

### Response状态释义

| 状态enum | 说明 |
| :--- | :--- |
| SUCCESS | 支付成功 |
| MONEY\_NOT\_ENOUGH | 点券数量不足 |
| NOT\_ACCOUNT | 点券数量不足（玩家无账户，未充值过） |
| UNKNOWN | 未知的返回状态代码 |
| ERROR | 请求接口过程错误 |

### 参考代码
```java
import com.mcrmb.CmdEvent;
import com.mcrmb.event.McrmbPayEvent;
import org.bukkit.event.EventHandler;
import org.bukkit.event.Listener;

public class test implements Listener {
    @EventHandler
    public void test_pay_event(McrmbPayEvent payEvent){
        payEvent.getPlayer(); //玩家名 String
        payEvent.getMoney(); //消费点券 int
        payEvent.getLatestBalance(); //点券余额 int
        payEvent.getReason(); //获取消费原因
        payEvent.isBroadcast(); //是否公屏
        payEvent.getResponse(); //获取消费结果
    }

    @EventHandler
    public void test_cmd_event(CmdEvent cmdEvent){
        cmdEvent.getCmd(); //第一个参数
        cmdEvent.getCmds(); //参数数组
        cmdEvent.getSender(); //命令发送人
        if(cmdEvent.getCmd().equalsIgnoreCase("nb")){
            // 用户执行了 /b nb xxx xxx 指令，插件处理的逻辑内容。
        }
    }

}
```

