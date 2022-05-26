# Gov

此模块提供治理的基本功能。

## 可用命令

| 名称                | 描述                                                   |
| ------------------- | ------------------------------------------------------ |
| [proposal]()        | 查询单个提案的详细信息                                 |
| [proposals]()       | 使用可选过滤器提案                                     |
| [vote]()            | 查询一次投票的详细信息                                 |
| [votes]()           | 查询提案的投票                                         |
| [deposit]()         | 查询摸个抵押人在某个提案的抵押信息                     |
| [deposits]()        | 查询提案的所有抵押信息                                 |
| [tally]()           | 汇总提案投票                                           |
| [param]()           | 查询参数                                               |
| [params]()          | 查询治理流程的参数                                     |
| [proposer]()        | 通过提议ID查询提案的发起人地址                         |
| [submit-proposal]() | 提交提案                                               |
| [deposit]()         | 为有效的提案抵押代币                                   |
| [vote]()            | 为活跃的提案投票，可选值： yes/no/no_with_veto/abstain |

## gnchaind query gov proposal

查询单个提案的详细信息。

```bash
gnchaind query gov proposal [proposal-id] [flags]
```

例如：

```bash
// 查询单个提案
gnchaind query gov proposal <proposal-id>
```

## gnchaind query gov proposals

使用可选过滤器提案。

```bash
gnchaind query gov proposals [flags]
```

**标识：**

| 名称，速记  | 类型    | 必须 | 默认 | 描述                                  |
| ----------- | ------- | ---- | ---- | ------------------------------------- |
| --depositor | Address |      |      | 按抵押人地址过滤提案                  |
| --limit     | uint    |      |      | 返回最新[数量]个提案。 默认为所有提案 |
| --status    | string  |      |      | 按状态过滤提案                        |
| --voter     | Address |      |      | 按投票人地址过滤提案                  |

例如：

```bash
// 查询所有提案
gnchaind query gov proposals
```

例如：

```bash
// 按条件查询提案
gnchaind query gov proposals --limit=3 --status=Passed --depositor=<iaa...>
```

## gnchaind query gov vote

查询一次投票的详细信息。

```bash
gnchaind query gov vote [proposal-id] [voter-addr] [flags]
```

例如：

```bash
// 查询单个投票的信息
gnchaind query gov vote <proposal-id> <iaa...>
```

## gnchaind query gov votes

查询提案的投票信息。

```bash
gnchaind query gov votes [proposal-id] [flags]
```

例如：

```bash
// 查询提案的所有投票
gnchaind query gov votes <proposal-id>
```

## gnchaind query gov deposit

通过提案ID查询提案中的某个抵押人的抵押信息。

```bash
gnchaind query gov deposit [proposal-id] [depositer-addr] [flags]
```

例如：

```bash
// 查询单个抵押信息
gnchaind query gov deposit <proposal-id> <iaa...>
```

## gnchaind query gov deposits

查询提案中所有抵押信息。

```bash
gnchaind query gov deposits [proposal-id] [flags]
```

例如：

```bash
// 查询提案的所有抵押信息
gnchaind query gov deposits <proposal-id>
```

## gnchaind query gov tally

查询提案的计票结果。 您可以通过运行`gnchaind query gov proposal`来查询提案ID。

```bash
gnchaind query gov tally [proposal-id] [flags]
```

例如：

```bash
// 查询提案统计信息
gnchaind query gov tally <proposal-id>
```

## gnchaind query gov param

查询治理过程的参数 （`voting` |`tallying`| `deposit`）。

```bash
gnchaind query gov param [param-type] [flags]
```

例如：

```bash
> gnchaind query gov param voting
> gnchaind query gov param tallying
> gnchaind query gov param deposit
```

## gnchaind query gov params

查询治理过程的所有参数。

```bash
gnchaind query gov params [flags]
```

## gnchaind query gov proposer

通过提议ID查询提案的发起人地址。

```bash
gnchaind query gov proposer [proposal-id] [flags]
```

## gnchaind tx gov submit-proposal

提交提案并附带初始委托。 提案标题、描述、类型和抵押可以直接提供，也可以通过JSON文件提供。 可用命令：`community-pool-spend`，`param-change`，`software-upgrade`，`cancel-software-upgrade` 。

## gnchaind tx gov submit-proposal community-pool-spend

提交提案并附带初始委托，提案详细信息必须通过JSON文件提供。

```bash
gnchaind tx gov submit-proposal community-pool-spend <path/to/proposal.json> --from=<key_or_address>
```

`proposal.json` 包含：

```json
{
    "title": "Community Pool Spend",
    "description": "Pay me some Atoms!",
    "recipient": "iaa1mjk4p68mmulwla3x5uzlgjwsc3zrms448rel3q",
    "amount": "1000ugnc",
    "deposit": "1000ugnc"
}
```

## gnchaind tx gov submit-proposal param-change

提交参数修改提案。提案详细信息必须通过JSON文件提供。 对于包含的值，只有非空字段将被更新。

重要说明：目前，对参数更改进行了评估，但尚未验证，因此，对于任何“值”更改，对其各自的参数都是有效的（即正确的类型和范围之内），例如 `MaxValidators`应为整数而不是十进制。

对参数更改建议进行适当的审核应防止这种情况的发生（在治理过程中不应出现任何沉积物），但无论如何都应注意。

```bash
gnchaind tx gov submit-proposal param-change <path/to/proposal.json> --from=<key_or_address>
```

`proposal.json` 包含：

```json
{
    "title": "Staking Param Change",
    "description": "Update max validators",
    "changes": [
        {
            "subspace": "staking",
            "key": "MaxValidators",
            "value": 105
        }
    ],
    "deposit": "1000ugnc"
}
```

## gnchaind tx gov submit-proposal software-upgrade

提交软件升级提案，指定唯一的名称和高度或时间，以使升级生效。

```bash
gnchaind tx gov submit-proposal software-upgrade [name] (--upgrade-height [height] | --upgrade-time [time]) (--upgrade-info [info]) [flags]
```

**标识：**

| 名称，速记       | 类型   | 必须 | 默认 | 描述                                                   |
| ---------------- | ------ | ---- | ---- | ------------------------------------------------------ |
| --deposit        | Coin   | Yes  |      | 提案抵押的代币                                         |
| --title          | string | Yes  |      | 提案的标题                                             |
| --description    | string | Yes  |      | 提案的描述                                             |
| --upgrade-height | int64  |      |      | 升级必须发生的高度（不要与`--upgrade-time`一起使用）   |
| --time           | string |      |      | 升级必须发生的时间（不要与`--upgrade-height`一起使用） |
| --info           | string |      |      | 计划升级的可选信息，例如提交哈希等。                   |

## gnchaind tx gov submit-proposal cancel-software-upgrade

取消软件升级提案。

```bash
gnchaind tx gov submit-proposal cancel-software-upgrade [flags]
```

**标识：**

| 名称，速记    | 类型   | 必须 | 默认 | 描述           |
| ------------- | ------ | ---- | ---- | -------------- |
| --deposit     | Coin   | Yes  |      | 提案抵押的代币 |
| --title       | string | Yes  |      | 提案的标题     |
| --description | string | Yes  |      | 提案的描述     |

## gnchaind tx gov deposit

为某个提案抵押代币。 您可以通过运行`gnchaind query gov proposal`来查询提案ID。

```bash
gnchaind tx gov deposit [proposal-id] [deposit] [flags]
```

例如：

```bash
// 为有效的提案抵押
gnchaind tx gov deposit [proposal-id] [deposit]
```

## gnchaind tx gov vote

为一个活跃的提案投票，可选值：yes/no/no_with_veto/abstain。

```bash
gnchaind tx gov vote [proposal-id] [option] [flags]
```

例如：

```bash
// 为活跃的提案投票
gnchaind tx gov vote <proposal-id> <option> --from=<key-name> --fees=0.3ugnc
```