# TxWasm

提供Wasm合约相关的操作行为Message的构建。

## 1、部署合约

```java
/**
* 创建消息结构---部署合约
*
* @param sender        用户地址
* @param wasmFile      编译文件 *.wasm
* @param instantiateByAddress  实例化用户地址, 为空任何用户可实例化
* @return MsgStoreCode 对象
*/
static Any NewMsgStoreCode(String sender, String wasmFile, String instantiateByAddress);
```

## 2、实例化合约

```java
/**
*  创建消息结构--- 实例化合约
*
* @param sender   用户地址
* @param codeID   合约代码ID
* @param initArgs 实例参数
* @param label    标签
* @param admin    管理员
* @param amounts  金额
* @return MsgInstantiateContract对象
*/
static Any NewMsgInstantiateContract(String sender, Long codeID, String initArgs, String label, String admin, List<Coin> amounts);
```

## 3、执行合约

```java
/**
*  创建消息结构--- 执行合约
*
* @param sender        用户地址
* @param contractAddr  合约地址
* @param execArgs      执行参数
* @param amounts  金额
* @return MsgExecuteContract对象
*/
static Any NewMsgExecuteContract(String sender, String contractAddr, String execArgs, List<Coin> amounts);
```



## 4、升级合约

```java
/**
*  创建消息结构--- 升级合约
*
* @param sender        用户地址
* @param contractAddr  合约地址
* @param codeID   合约代码ID
* @param migrateArgs 升级参数
* @return MsgMigrateContract
*/
static Any NewMsgMigrateContract(String sender, String contractAddr, Long codeID, String migrateArgs);
```

## 5、更新管理员

```java
/**
*  创建消息结构--- 更新管理者
*
* @param sender        用户地址
* @param contractAddr  合约地址
* @param admin         管理者地址
* @return MsgUpdateAdmin对象
*/
static Any NewMsgUpdateAdmin(String sender, String contractAddr, String admin);
```

## 6、移除管理员

```java
/**
*  创建消息结构--- 清空管理者
*
* @param sender        用户地址
* @param contractAddr  合约地址
* @return MsgClearAdmin对象
*/
static Any NewMsgClearAdmin(String sender, String contractAddr)
```

