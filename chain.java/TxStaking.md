# TxStaking

提供操作权益相关的行为Message的构建。

## 1、创建验证者并质押权益

```java
/**
* 创建消息结构--- 创建验证者
*
* @param delegator_address  委托人地址
* @param amount 质押金额
* @param moniker 名称
* @param website 网站地址
* @return MsgCreateValidator 对象
*/
static Any NewMsgCreateValidator(String delegator_address, Any pubKey, Coin amount, String moniker, String website);
```

## 2、更新验证者信息

 ```java
 /**
 * 创建消息结构--- 更新验证者信息
 *
 * @param validator_address 被委托人地址
 * @param moniker 名称
 * @param website 网站地址
 * @return MsgEditValidator 对象
 */
 static Any NewMsgEditValidator(String validator_address, String moniker, String website);
 ```



## 2、委托权益

```java
/**
* 创建消息结构--- 委托权益
*
* @param delegator_address  委托人地址
* @param validator_address  被委托人地址
* @param amount 委托金额
* @return MsgDelegate 对象
*/
static Any NewMsgDelegate(String delegator_address, String validator_address, Coin amount);
```

## 3、重新委托

```java
/**
* 创建消息结构--- 变更被委托人
*
* @param delegator_address  委托人地址
* @param validator_src_address 被委托人地址
* @param validator_dst_address 被委托人地址
* @param amount 委托金额
* @return MsgBeginRedelegate 对象
*/
static Any NewMsgRedelegate(String delegator_address, String validator_src_address, String validator_dst_address, Coin amount);
```

## 4、取消委托

```java
 /**
 * 创建消息结构--- 取消委托权益
 *
 * @param delegator_address  委托人地址
 * @param validator_address  被委托人地址
 * @param amount 委托金额
 * @return NewMsgUndelegate 对象
 */
static Any NewMsgUndelegate(String delegator_address, String validator_address, Coin amount);
```