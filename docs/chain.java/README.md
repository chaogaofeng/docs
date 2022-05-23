# Java SDK

Java SDK 提供对GNC网络核心数据结构、序列化、密钥管理和API请求的封装。可用于与GNC区块链交互的Java应用程序。 
SDK提供了私钥存储、私钥管理、以及与GNC节点交互的交易发送、数据查询等功能。

> 重要提示 
> > - 助记词可以推出私钥，私钥不可推出助记词。助记词一定要备份！！！
> > - 构建消息java类、数据查询java类与模块有对应关系。比如Wasm模块， 构建消息类TxWasm，数据查询类QueryWasm。其他依次推之。

## 功能介绍
### [私钥存储接口类](chain.java/KeyDAO.md)
定义了私钥存储、读取、加密、解密的函数，为私钥管理接口类提供私钥的存储管理。
```java
import com.glodnet.chain.keys.IKeyDAO;
```
  - 存储私钥
  - 读取私钥
  - 删除私钥
  - 加密私钥
  - 解密私钥

### [私钥管理接口类](chain.java/KeyService.md) 
定义了私钥的生成、导入、导出、签名等函数，提供账户私钥管理，为交易发送类提供交易签名。
```java
import com.glodnet.chain.keys.IKeyService;
```
  - 随机生成私钥
  - 导入私钥助记词
  - 导入私钥keystore文件
  - 导出私钥keystore文件
  - 删除私钥
  - 获取地址
  - 私钥签名 
  - 私钥签名交易

### [交易发送类](chain.java/TxService.md)
提供与GNC节点交互的交易发送功能
```java
import com.glodnet.chain.TxService;
```
  - 构建交易
  - 签名交易
  - 广播交易
  - 构建+签名+广播交易

### 消息构建类
- [Staking权益相关的消息构建类](chain.java/TxStaking.md)
- [Wasm合约相关的消息构建类](chain.java/TxWasm.md)

### 数据查询类
提供与GNC节点交互的数据查询功能
  - [节点相关的查询类](chain.java/QueryNode.md)
  - [账户相关的查询类](chain.java/QueryAuth.md)
  - [账户金额的查询类](chain.java/QueryBank.md) 
  - [Wasm合约相关的查询类](chain.java/QueryWasm.md)

## 演示用例

### 资产转移

```java
// 节点api
HttpClient client = new HttpClient("http://127.0.0.1:1317") // 节点api访问地址
// 发送交易服务
TxService tx = new TxService(client, null, SignAlgo.SM2,"gnchain-45_1", CoinOuterClass.DecCoin.newBuilder().build(), new BigDecimal(1.5));
// 私钥管理服务
IKeyService keyService = tx.getKeyService();

// 助记词
String mnemonic = "apology false junior asset sphere puppy upset dirt miracle rice horn spell ring vast wrist crisp snake oak give cement pause swallow barely clever";
// 导入助记词，创建账户alice
keyService.recoverKey("alice", "123456", mnemonic, true, 0, "");
// 账户alice地址
String aliceAddress = keyService.showAddress("alice");

// 助记词
String mnemonic2 = "brown surface body omit unique usual bean dance kidney spider treat decrease friend exhaust exercise guitar quick clinic cotton depend giggle allow fitness master";
// 导入助记词，创建账户node
keyService.recoverKey("node", "123456", mnemonic2, true, 0, "");
// 账户node地址
String nodeAddress = keyService.showAddress("node");

//转账金额列表
List<CoinOuterClass.Coin> amounts = new ArrayList<>();
amounts.add(CoinOuterClass.Coin.newBuilder().setDenom("ugnc").setAmount("1000000").build());

// 操作行为的消息列表
List<Any> msgs = new ArrayList<>();
// 资产转移消息构建
msgs.add(TxBank.NewMsgSend(aliceAddress, nodeAddress, amounts));
// 发送交易
String hash = tx.send("alice", "123456", BuildTxOptions.builder()
                      .sender(aliceAddress)
                      .msgs(msgs)
                      .memo("")
                      .build());
// 交易哈希
System.out.println(hash);
```

### 合约部署

 ```java
 // 节点api
 HttpClient client = new HttpClient("http://127.0.0.1:1317")
 // 发送交易服务
 TxService tx = new TxService(client, null, SignAlgo.SM2,"gnchain-45_1", CoinOuterClass.DecCoin.newBuilder().build(), new BigDecimal(1.5));
 // 私钥管理服务
 IKeyService keyService = tx.getKeyService();
 
 // 助记词
 String mnemonic = "apology false junior asset sphere puppy upset dirt miracle rice horn spell ring vast wrist crisp snake oak give cement pause swallow barely clever";
 // 导入助记词，创建账户alice
 keyService.recoverKey("alice", "123456", mnemonic, true, 0, "");
 // 账户alice地址
 String aliceAddress = keyService.showAddress("alice");
 
 // 操作行为的消息列表
 List<Any> msgs = new ArrayList<>();
 // 部署合约消息构建
 msgs.add(TxWasm.NewMsgStoreCode(aliceAddress, "cw_nameservice.wasm", ""));
 // 发送交易
 String hash = tx.send("alice", "123456", BuildTxOptions.builder()
                       .sender(aliceAddress)
                       .msgs(msgs)
                       .memo("")
                       .build());
 // 交易哈希
 System.out.println(hash);
 ```

### 合约实例化

```java
// 节点api
HttpClient client = new HttpClient("http://127.0.0.1:1317") // 节点api访问地址
// 发送交易服务
TxService tx = new TxService(client, null, SignAlgo.SM2,"gnchain-45_1", CoinOuterClass.DecCoin.newBuilder().build(), new BigDecimal(1.5));
// 私钥管理服务
IKeyService keyService = tx.getKeyService();

// 助记词
String mnemonic = "apology false junior asset sphere puppy upset dirt miracle rice horn spell ring vast wrist crisp snake oak give cement pause swallow barely clever";
// 导入助记词，创建账户alice
keyService.recoverKey("alice", "123456", mnemonic, true, 0, "");
// 账户alice地址
String aliceAddress = keyService.showAddress("alice");

// 操作行为的消息列表
List<Any> msgs = new ArrayList<>();
// 合约实例化参数
String initArgs = "{\"purchase_price\":{\"amount\":\"100\",\"denom\":\"ugnc\"},\"transfer_price\":{\"amount\":\"999\",\"denom\":\"ugnc\"}}";
// 合约实例化消息构建
msgs.add(TxWasm.NewMsgInstantiateContract(aliceAddress, 15L, initArgs, "label", "", new ArrayList<>()));
// 发送交易
String hash = tx.send("alice", "123456", BuildTxOptions.builder()
                      .sender(aliceAddress)
                      .msgs(msgs)
                      .memo("")
                      .build());
// 交易哈希
System.out.println(hash);
```

### 合约升级

```java
// 节点api
HttpClient client = new HttpClient("http://127.0.0.1:1317") // 节点api访问地址
// 发送交易服务
TxService tx = new TxService(client, null, SignAlgo.SM2,"gnchain-45_1", CoinOuterClass.DecCoin.newBuilder().build(), new BigDecimal(1.5));
// 私钥管理服务
IKeyService keyService = tx.getKeyService();

// 助记词
String mnemonic = "apology false junior asset sphere puppy upset dirt miracle rice horn spell ring vast wrist crisp snake oak give cement pause swallow barely clever";
// 导入助记词，创建账户alice
keyService.recoverKey("alice", "123456", mnemonic, true, 0, "");
// 账户alice地址
String aliceAddress = keyService.showAddress("alice");

// 操作行为的消息列表
List<Any> msgs = new ArrayList<>();
// 合约地址
String contract = "" 
// 合约升级参数
String migrateArgs = "{\"purchase_price\":{\"amount\":\"100\",\"denom\":\"ugnc\"},\"transfer_price\":{\"amount\":\"999\",\"denom\":\"ugnc\"}}";
// 合约实例化消息构建
msgs.add(TxWasm.NewMsgMigrateContract(aliceAddress, contract, 15L, migrateArgs, new ArrayList<>()));
// 发送交易
String hash = tx.send("alice", "123456", BuildTxOptions.builder()
                      .sender(aliceAddress)
                      .msgs(msgs)
                      .memo("")
                      .build());
// 交易哈希
System.out.println(hash);
```

### 非验证节点升级为验证节点

```java
// 节点api
HttpClient client = new HttpClient("http://127.0.0.1:1317") // 节点api访问地址
// 发送交易服务
TxService tx = new TxService(client, null, SignAlgo.SM2,"gnchain-45_1", CoinOuterClass.DecCoin.newBuilder().build(), new BigDecimal(1.5));
// 私钥管理服务
IKeyService keyService = tx.getKeyService();

// 助记词
String mnemonic = "gossip wheel net riot retreat arrest ozone dragon funny undo bulb visa victory label slim domain network wage suit peanut tattoo text venture answer";
// 导入助记词，创建账户node
keyService.recoverKey("node", "123456", mnemonic, true, 0, "");
// 账户node地址
String delegator_address = keyService.showAddress("validator");

// 操作行为的消息列表
List<Any> msgs = new ArrayList<>();
// 创建验证人消息构建
msgs.add(TxStaking.NewMsgCreateValidator(delegator_address, keyService.PubKey(keyService.showPubKey("alice")), CoinOuterClass.Coin.newBuilder().setAmount("1000000").setDenom("ugnc").build(), "ddd", ""));

// 发送交易
String hash = tx.send("validator", "123456", BuildTxOptions.builder()
                      .sender(delegator_address)
                      .msgs(msgs)
                      .memo("")
                      .build());
// 交易哈希
System.out.println(hash);
```

### 验证节点降级为非验证节点

```java
// 节点api
HttpClient client = new HttpClient("http://127.0.0.1:1317") // 节点api访问地址
// 发送交易服务
TxService tx = new TxService(client, null, SignAlgo.SM2,"gnchain-45_1", CoinOuterClass.DecCoin.newBuilder().build(), new BigDecimal(1.5));
// 私钥管理服务
IKeyService keyService = tx.getKeyService();

// 助记词
String mnemonic = "gossip wheel net riot retreat arrest ozone dragon funny undo bulb visa victory label slim domain network wage suit peanut tattoo text venture answer";
// 导入助记词，创建账户node
keyService.recoverKey("node", "123456", mnemonic, true, 0, "");
// 账户node地址
String delegator_address = keyService.showAddress("validator");
// 账户node验证人操作地址
String validator_address = Bech32Utils.valoperBech32(delegator_address);

// 操作行为的消息列表
List<Any> msgs = new ArrayList<>();
// 取消委托消息构建
msgs.add(TxStaking.NewMsgUndelegate(delegator_address, validator_address, CoinOuterClass.Coin.newBuilder().setAmount("1000000").setDenom("ugnc").build()));
// 发送交易
String hash = tx.send("validator", "123456", BuildTxOptions.builder()
                      .sender(delegator_address)
                      .msgs(msgs)
                      .memo("")
                      .build());
// 交易哈希
System.out.println(hash);
```

