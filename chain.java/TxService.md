# 发送交易

提供构建交易、签名交易、广播交易等功能。

```java
/**
* 创建TxService对象
*
* @param client         节点API
* @param keyDAO         账户私钥存储服务
* @param signAlgo       账户私钥使用的签名算法
* @param chainID        网络ChainID。 例： gnchain
* @param gasPrice       gas手续费。自动计算feeAmount时使用。 例： 0.00002ugnc
* @param gasAdjustment  gas调整系数。自动计算gasLimit时使用。例： 1.3
* @return TxService对象
*/
TxService(HttpClient client, IKeyDAO keyDAO, SignAlgo signAlgo, String chainID, DecCoin gasPrice, BigDecimal gasAdjustment);
```



构建交易option参数

> 必选字段
>
> > 消息体列表 msgs
```java
/**
* 构建交易参数
*
* @param msgs message列表
* @param memo 备注
* @param sender 交易发送者，未设置则为签名账户私钥的地址。只有签名账户跟发送者地址不一致时必须设置
* @param accountNumber 账户地址序列号，未设置自动获取
* @param sequence      账户地址发送交易序列号，未设置自动获取
* @param gasLimit      可消耗gas最大值，未设置自动获取
* @param feeAmount     手续费，未设置自动获取
* @return Tx 对象
*/
@Data
@Builder
public class BuildTxOptions {
    private List<Any> msgs;
    private String memo;    

    // Optional parameters
    @Builder.Default
    private String sender = "";
    @Builder.Default
    private Long accountNumber = 0L;
    @Builder.Default
    private Long sequence = 0L;
    @Builder.Default
    private Long gasLimit = 0L;
    @Builder.Default
    private CoinOuterClass.Coin feeAmount = null;
    @Builder.Default
    private String feeGranter = "";
    @Builder.Default
    private Long timeoutHeight = 0L;
}
```
> 规范
> > 消息java类与业务模块存在对应关系。比如Wasm模块， 状态数据查询的类名为QueryWasm，构建消息类的TxWasm。其他模块依次推之。


## 构建交易

```java
/**
* 构建交易
*
* @param options 构建交易参数
* @return Tx 对象
*/
public Tx build(BuildTxOptions options) throws Exception;
```

## 签名交易

```java
/**
* 签名交易
*
* @param name     账户名称
* @param password 账户密码
* @param tx       Tx对象
* @param accountNumber 账户地址序列号，为0自动获取
* @param sequence      账户地址发送交易序列号，为0自动获取
* @param overwriteSig  是否替换已有签名
* @return Tx 对象
*/
 public Tx sign(String name, String password, Tx tx, Long accountNumber, Long sequence, boolean overwriteSig) throws Exception;
```

## 广播交易

```java
/**
* 广播交易
* @param tx       Tx对象
* @param mode     广播模式 async异步(交易合法性检查前返回)、 sync同步(交易合法性检查后返回)、 block阻塞(记录在区块后)
* @return 交易哈希
*/
public String broadcast(Tx tx, BroadcastMode mode) throws Exception
```

## 构建+签名+广播交易(阻塞模式)

```java
 /**
 * 发送交易(构建、签名、广播）
 * @param name     账户名称
 * @param password 账户密码
 * @param options 构建交易参数
 * @return 交易哈希
 */
public String send(String name, String password, BuildTxOptions options) throws Exception;
```
