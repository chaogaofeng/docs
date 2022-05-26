# 命令行客户端

`gnchaind`是GNC网络的命令行客户端。用户可以使用`gnchaind`发送交易和查询区块链数据。`gnchaind`命令允许您初始化，启动，重置节点或生成创世文件等。

| Name                                                         | Description                                                  |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [init]()                                                     | 初始化验证人，p2p，创世纪和应用程序配置文件                  |
| [add-genesis-account]()                                      | 将创世帐户添加到genesis.json                                 |
| [gentx]()                                                    | 生成自抵押的创始TX                                           |
| [collect-gentxs]()                                           | 收集创世txs并输出到genesis.json文件                          |
| [start]()                                                    | 启动全节点                                                   |
| [unsafe-reset-all]()                                         | 重置区块链数据库，删除address book，并将priv_validator.json重置为创始状态 |
| [tendermint]()                                               | Tendermint子命令                                             |
| [testnet]() | 初始化测试网                                                 |
| [reset]()                                                    | 将app状态重置到指定的高度                                    |
| [export]()                                                   | 将状态导出为JSON                                             |
| version                                                      | 显示版本信息                                                 |

## 工作目录

`gnchaind`默认工作目录是`$HOME/.gnchain`，主要用于保存配置文件和区块数据、密钥数据。您也可以通过`--home`指定`gnchaind`工作目录。

`gnchaind init`命令负责初始化指定的`--home`目录并创建默认配置文件。除`gnchaind init`命令外，任何其他`gnchaind`子命令使用的主目录必须初始化，否则将报错。

所有配置文件都存储在工作目录下`config`目录中：

### genesis.json

genesis.json定义了创世块数据，该数据定义了系统参数，例如chain_id，共识参数，初始帐户通证分配，验证人的创建以及各模块的参数。

### node_key.json

node_key.json用于存储节点的密钥。`gnchaind tendermint show-node-id`查询的节点ID由该密钥派生，该ID是节点的唯一标识。它用于p2p连接。

### priv_validator.json

pri_validator.json是Tendermint Key文件，验证人节点将在每轮共识投票中使用该文件来签署Pre-vote/Pre-commit。随着共识的进行，tendermint共识引擎将不断更新`last_height` /`last_round` /`last_step`值。

### config.toml

config.toml是节点的非共识配置。不同的节点可以根据自己的情况进行配置。常见的修改是`persistent_peers`、`moniker`、`laddr`

### app.toml

app.toml提供了基础配置、监控配置、API配置、同步状态配置和gRPC配置。

## 连接全节点

`--node`用来指定连接节点的rpc地址，交易和查询的消息都发送到监听这个端口的节点进程。默认为`tcp://localhost:26657`。

## 全局标识

### GET 请求

所有GET请求都有以下全局标识：

| 名称，速记 | 类型   | 必需 | 默认值         | 描述                     |
| ---------- | ------ | ---- | -------------- | ------------------------ |
| --chain-id | string |      | gnchain-45_1   | 节点`Chain ID`           |
| --home     | string |      | $HOME/.gnchain | 配置和数据的目录         |
| --trace    | bool   |      | false          | 打印出错时的完整堆栈跟踪 |

### POST 请求

所有POST请求都有以下全局标识：

| 名称，速记        | 类型   | 必需 | 默认值                | 描述                                                         |
| ----------------- | ------ | ---- | --------------------- | ------------------------------------------------------------ |
| --account-number  | int    |      | 0                     | 发起交易的账户编号                                           |
| --broadcast-mode  | string |      | sync                  | 交易广播模式                                                 |
| --dry-run         | bool   |      | false                 | 模拟执行交易，并返回消耗的`gas`。`--gas`指定的值会被忽略     |
| --fees            | string |      |                       | 交易费（指定交易费的上限）                                   |
| --from            | string |      |                       | 签名交易的私钥名称                                           |
| --gas             | string |      | 50000                 | 交易的gas上限；设置为`auto`将自动计算该值                    |
| --gas-adjustment  | float  |      | 1.5                   | gas调整因子，这个值降乘以模拟执行消耗的`gas`，计算的结果返回给用户；如果`--gas`的值不是`auto`，这个标志将被忽略 |
| --gas-prices      | string |      |                       | gas以十进制格式确定交易费                                    |
| --generate-only   | bool   |      | false                 | 是否仅仅构建一个未签名的交易便返回                           |
| --help, -h        | string |      |                       | 打印帮助信息                                                 |
| --keyring-backend | string |      | os                    | 选择密钥管理后端类型                                         |
| --ledger          | bool   |      | false                 | 使用ledger设备                                               |
| --memo            | string |      |                       | 指定交易的memo字段                                           |
| --node            | string |      | tcp://localhost:26657 | 节点的rpc地址                                                |
| --offline         | string |      |                       | 离线节点                                                     |
| --sequence        | int    |      | 0                     | 发起交易的账户sequence                                       |
| --sign-mode       | string |      |                       | 选择签名模式                                                 |
| --yes             | bool   |      | true                  | 跳过交易广播提示确认                                         |
| --chain-id        | string |      |                       | 节点的`Chain ID`                                             |
| --home            | string |      |                       | 配置文件和数据文件目录 (默认 "$HOME/.gnchain")               |
| --trace           | bool   |      | false                 | 打印出完整的堆栈跟踪错误                                     |

## 模块列表

| **子命令**                            | **描述**                  |
| ------------------------------------- | ------------------------- |
| [auth](chain/cli/Auth.md)             | 用于查询帐户的子命令      |
| [bank](chain/cli/Bank.md)             | 用于查询帐户余额的子命令  |
| [debug](chain/cli/Debug.md)           | 调试的子命令              |
| [gov](chain/cli/Gov.md)               | 治理和投票的子命令        |
| [keys](chain/cli/Keys.md)             | 密钥管理的子命令          |
| [staking](chain/cli/Staking.md)       | Staking 的子命令          |
| [status](chain/cli/Status.md)         | 查询节点的状态的子命令    |
| [tendermint](chain/cli/Tendermint.md) | Tendermint 状态查询子命令 |
| [tx](chain/cli/Tx.md)                 | Tx的 子命令               |
| [upgrade](chain/cli/Upgrade.md)       | 软件升级的子命令          |

