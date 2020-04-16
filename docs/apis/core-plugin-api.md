# 插件层接口/事件文档

使用这些接口，您可以自己开发出合适服务器的附属插件~

## 可用方法

以下方法均位于 `com.mcrmb.PayApi`，可以直接静态调用。因为需要走网络，所以请在调用时使用异步包裹~ `/b debug` 开启模式下，可以看到每个MCRMB的网络请求是否处于主线程内。

| 作用 | 方法 | 返回值 |
| :--- | :--- | :--- |
| 查询玩家余额 | look\(String playername\) | int |
| 查询玩家累计充值 | allcharge\(String playername\) | int |
| 查询玩家累计消费 | allpay\(String playername\) | int |
| 玩家消费接口 | Pay\(String playername,String point,String useto,boolean brocast\) | boolean |
| 手工操作玩家点券 | Manual\(String playername,int type,String point,String text\) | boolean |

### 参数值说明

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#x53C2;&#x6570;&#x540D;</th>
      <th style="text-align:left">&#x8BF4;&#x660E;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">playername</td>
      <td style="text-align:left">&#x73A9;&#x5BB6;&#x540D;</td>
    </tr>
    <tr>
      <td style="text-align:left">point</td>
      <td style="text-align:left">&#x70B9;&#x5377;&#x6570;&#x91CF;</td>
    </tr>
    <tr>
      <td style="text-align:left">useto</td>
      <td style="text-align:left">&#x6D88;&#x8D39;&#x8BF4;&#x660E;</td>
    </tr>
    <tr>
      <td style="text-align:left">brocast</td>
      <td style="text-align:left">&#x662F;&#x5426;&#x53D1;&#x516C;&#x5C4F;</td>
    </tr>
    <tr>
      <td style="text-align:left">type</td>
      <td style="text-align:left">
        &#x624B;&#x5DE5;&#x64CD;&#x4F5C;&#x70B9;&#x5377;&#x7C7B;&#x578B;<br>1=&#x52A0;&#x6B3E;<br>2=&#x6263;&#x6B3E;<br>3=&#x91CD;&#x8BBE;&#x70B9;&#x5238;&#x91D1;&#x989D;
      </td>
    </tr>
    <tr>
      <td style="text-align:left">text</td>
      <td style="text-align:left">&#x70B9;&#x5238;&#x64CD;&#x4F5C;&#x8BF4;&#x660E;</td>
    </tr>
  </tbody>
</table>## 

## 可用事件

| 事件 | 说明 | 返回参数 |
| :--- | :--- | :--- |
| com.mcrmb.event.McrmbPayEvent | 支付完成事件 | 见下表 |

### 参数表

| 参数 | 类型 | 说明 |
| :--- | :--- | :--- |
| player | String | 玩家名字符串 |
| money | int | 成功消费的点券数量 |
| reason | String | 消费说明 |
| broadcast | boolean | 是否进行公屏 |
| latestBalance | int | 最新的点券余额 |
| response | Response | 返回状态，参见下表 |

### Response状态释义

| 状态enum | 说明 |
| :--- | :--- |
| SUCCESS | 支付成功 |
| MONEY\_NOT\_ENOUGH | 点券数量不足 |
| NOT\_ACCOUNT | 点券数量不足（玩家无账户，未充值过） |
| UNKNOWN | 未知的返回状态代码 |
| ERROR | 请求接口过程错误 |



