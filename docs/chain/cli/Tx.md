# Tx

签名、广播、查询交易

## 可用命令

| 名称          | 描述                     |
| ------------- | ------------------------ |
| [sign]()      | 签名生成的离线交易文件   |
| [broadcast]() | 广播一个已签名交易到网络 |
| [multisig]()  | 用多个账户为同一交易签名 |
| [tx]()        | 使用交易hash查询交易     |
| [txs]()       | 使用Tag查询交易          |

## gnchaind tx sign

签名生成的离线交易文件。该文件由`generate-only`标志生成。

```bash
gnchaind tx sign <file> [flags]
```

标志

| 命令，速记       | 类型   | 必须 | 默认  | 描述                             |
| ---------------- | ------ | ---- | ----- | -------------------------------- |
| -h，--help       |        |      |       | 打印帮助                         |
| --append         | bool   |      | true  | 将签名附加到现有签名             |
| --multisig       | string |      | true  | 代表交易签名的multisig帐户的地址 |
| --from           | string | 是   |       | 用于签名的私钥名称               |
| --offline        | bool   |      | false | 离线模式                         |
| --signature-only | bool   |      | false | 仅打印生成的签名，然后退出       |

## gnchaind tx broadcast

这个命令用于广播已离线签名的交易到网络。

```bash
gnchaind tx broadcast signed.json --chain-id=gnchain
```

## gnchaind tx multisign

用多个账户为一个交易签名。这个交易只有在签名数满足multisig-threshold时才可以广播。

```bash
gnchaind tx multisign <file> <key-name> <[signature]...> [flags]
```

## gnchaind query tx

```bash
gnchaind query tx [hash] [flags]
```

## gnchaind query txs

```bash
gnchaind query txs --events 'message.sender=<iaa...>&message.action=xxxx' --page 1 --limit 30
```

其中`message.action`可取值：

| module       | Msg                                       | action               |
| ------------ | ----------------------------------------- | -------------------- |
| bank         | cosmos-sdk/MsgSend                        | transfer             |
|              | cosmos-sdk/MsgMultiSend                   | transfer             |
| distribution | cosmos-sdk/MsgModifyWithdrawAddress       | set_withdraw_address |
|              | cosmos-sdk/MsgWithdrawValidatorCommission | withdraw_commission  |
|              | cosmos-sdk/MsgWithdrawDelegatorReward     | withdraw_rewards     |
| gov          | cosmos-sdk/MsgSubmitProposal              | submit_proposal      |
|              | cosmos-sdk/MsgDeposit                     | proposal_deposit     |
|              | cosmos-sdk/MsgVote                        | proposal_vote        |
| stake        | cosmos-sdk/MsgCreateValidator             | create_validator     |
|              | cosmos-sdk/MsgEditValidator               | edit_validator       |
|              | cosmos-sdk/MsgDelegate                    | delegate             |
|              | cosmos-sdk/MsgBeginRedelegate             | redelegate           |
|              | cosmos-sdk/MsgUndelegate                  | unbond               |
| slashing     | cosmos-sdk/MsgUnjail                      | unjai                |
|              |                                           |                      |
|              |                                           |                      |
|              |                                           |                      |
|              |                                           |                      |
|              |                                           |                      |
|              |                                           |                      |
|              |                                           |                      |
|              |                                           |                      |
|              |                                           |                      |
|              |                                           |                      |
|              |                                           |                      |
|              |                                           |                      |
|              |                                           |                      |
|              |                                           |                      |
|              |                                           |                      |