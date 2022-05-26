# Staking

Staking模块提供了一系列查询staking状态和发送staking交易的命令。

## 可用命令

| 名称                           | 描述                                                         |
| ------------------------------ | ------------------------------------------------------------ |
| [validator]()                  | 查询某个验证者                                               |
| [validators]()                 | 查询所有的验证者                                             |
| [delegation]()                 | 基于委托者地址和验证者地址的委托查询                         |
| [delegations]()                | 基于委托者地址的所有委托查询                                 |
| [delegations-to]()             | 查询在某个验证人上的所有委托                                 |
| [unbonding-delegation]()       | 基于委托者地址和验证者地址的unbonding-delegation记录查询     |
| [unbonding-delegations]()      | 基于委托者地址的所有unbonding-delegation记录查询             |
| [unbonding-delegations-from]() | 基于验证者地址的所有unbonding-delegation记录查询             |
| [redelegations-from]()         | 基于某一验证者的所有转委托查询                               |
| [redelegation]()               | 基于委托者地址，原验证者地址和目标验证者地址的转委托记录查询 |
| [redelegations]()              | 基于委托者地址的所有转委托记录查询                           |
| [pool]()                       | 查询最新的权益池                                             |
| [params]()                     | 查询最新的权益参数信息                                       |
| [historical-info]()            | 查询给定高度的历史信息                                       |
| [create-validator]()           | 以自委托的方式创建一个新的验证者                             |
| [edit-validator]()             | 编辑已存在的验证者信息                                       |
| [delegate]()                   | 委托一定代币到某个验证者                                     |
| [unbond]()                     | 从指定的验证者解绑一定的股份                                 |
| [redelegate]()                 | 转委托一定的token从一个验证者到另一个验证者                  |

## gnchaind query staking validator

通过地址查询验证人

```bash
gnchaind query staking validator <iva...>
```

## gnchaind query staking validators

查询所有验证人

```bash
gnchaind query staking validators
```

## gnchaind query staking delegation

通过委托人地址和验证人地址查询委托交易。

```bash
gnchaind query staking delegation [delegator-addr] [validator-addr]
```

## gnchaind query staking delegations

查询某个委托人发起的所有委托记录。

```bash
gnchaind query staking delegations [delegator-address] [flags]
```

## gnchaind query staking delegations-to

查询某个验证人接受的所有委托。

```bash
gnchaind query staking delegations-to [validator-address] [flags]
```

## gnchaind query staking unbonding-delegation

通过委托人与验证人地址查询unbonding-delegation记录。

```bash
gnchaind query staking unbonding-delegation [delegator-addr] [validator-addr] [flags]
```

## gnchaind query staking unbonding-delegations

查询委托人的所有未绑定委托记录

```bash
gnchaind query staking unbonding-delegations <iaa...>
```

## gnchaind query staking unbonding-delegations-from

查询验证人的所有未绑定委托记录

```bash
gnchaind query staking unbonding-delegations-from <iva...>
```

## gnchaind query staking redelegations-from

查询验证人的所有转委托记录。

```bash
gnchaind query staking redelegations-from [validator-address] [flags]
```

## gnchaind query staking redelegation

通过委托人地址、原验证人地址、目标验证人地址查询转委托记录。

```bash
gnchaind query staking redelegation [delegator-addr] [src-validator-addr] [dst-validator-addr] [flags]
```

## gnchaind query staking redelegations

查询委托人的所有转委托记录

```bash
gnchaind query staking redelegations <iaa...>
```

## gnchaind query staking pool

查询当前权益池

```bash
gnchaind query staking pool
```

示例输出：

```bash
Pool:
  Loose Tokens:   1409493892.759816067399143966
  Bonded Tokens:  590526409.65743521209068061
  Token Supply:   2000020302.417251279489824576
  Bonded Ratio:   0.2952602076
```

## gnchaind query staking params

查询当前权益参数信息

```bash
gnchaind query staking params
```

## gnchaind query staking historical-info

查询给定高度的历史信息

```bash
gnchaind query staking historical-info <height>
```

## gnchaind tx staking create-validator

发送交易申请成为验证人，并委托一定数量的token到该验证人。

```bash
gnchaind tx staking create-validator [flags]
```

**标志：**

| 名称，速记                   | 类型   | 必须 | 默认  | 描述                                |
| ---------------------------- | ------ | ---- | ----- | ----------------------------------- |
| --amount                     | string | 是   |       | 委托金额                            |
| --commission-rate            | float  | 是   | 0.0   | 初始佣金比例                        |
| --commission-max-rate        | float  |      | 0.0   | 最大佣金比例                        |
| --commission-max-change-rate | float  |      | 0.0   | 最高佣金变更率百分比（每天）        |
| --min-self-delegation        | string |      |       | 验证人要求的最小抵押                |
| --details                    | string |      |       | 验证人节点的详细信息                |
| --genesis-format             | bool   |      | false | 是否以genesis transaction的方式导出 |
| --identity                   | string |      |       | 身份信息的签名                      |
| --ip                         | string |      |       | 验证人节点的IP                      |
| --node-id                    | string |      |       | 节点ID                              |
| --moniker                    | string | 是   |       | 验证人节点名称                      |
| --pubkey                     | string | 是   |       | Amino编码的验证人公钥               |
| --website                    | string |      |       | 验证人节点的网址                    |
| --security-contact           | string |      |       | 验证人（可选）的安全联系电子邮件    |

- 创建验证人

```bash
gnchaind tx staking create-validator --chain-id=gnchain --from=<key-name> --fees=0.3gnchaind --pubkey=<validator-pubKey> --commission-rate=0.1 --amount=100gnchaind --moniker=<validator-name>
```

## gnchaind tx staking edit-validator

修改验证的的参数，包括佣金比率，验证人节点名称以及其他描述信息。

```bash
gnchaind tx staking edit-validator [flags]
```

**标志：**

| 名称，速记            | 类型   | 必须 | 默认 | 描述                             |
| --------------------- | ------ | ---- | ---- | -------------------------------- |
| --commission-rate     | float  |      | 0.0  | 佣金比率                         |
| --moniker             | string |      |      | 验证人名称                       |
| --identity            | string |      |      | 身份签名                         |
| --website             | string |      |      | 网址                             |
| --details             | string |      |      | 验证人节点详细信息               |
| --security-contact    | string |      |      | 验证人（可选）的安全联系电子邮件 |
| --min-self-delegation | string |      |      | 验证人要求的最小抵押             |

- 编辑验证人信息

```bash
gnchaind tx staking edit-validator --from=<key-name> --chain-id=gnchain --fees=0.3gnchaind --commission-rate=0.10 --moniker=<validator-name>
```

## gnchaind tx staking delegate

向验证人委托通证。

```bash
gnchaind tx staking delegate [validator-addr] [amount] [flags]
gnchaind tx staking delegate <iva...> <amount> --chain-id=gnchain --from=<key-name> --fees=0.3ugnc
```

## gnchaind tx staking unbond

从验证人解委托通证。

```bash
gnchaind tx staking unbond [validator-addr] [amount] [flags]
```

- 从验证人中解委托一定数量的代币

```bash
gnchaind tx staking unbond <iva...> 10gnchaind --from=<key-name> --chain-id=gnchain --fees=0.3ugnc
```

## gnchaind tx staking redelegate

把某个委托的一部分或者全部从一个验证人转移到另外一个验证人。

提示

转委托没有`unbonding time`，所以你不会错过奖励。但是对每个验证人的转委托，在一个`unbonding time`区间内只能操作一次。

```bash
gnchaind tx staking redelegate [src-validator-addr] [dst-validator-addr] [amount] [flags]
```

- 转委托一定数量代币到其他验证人

```bash
gnchaind tx staking redelegate <iva...> <iva...> 10gnchaind --chain-id=gnchain --from=<key-name> --fees=0.3i
```