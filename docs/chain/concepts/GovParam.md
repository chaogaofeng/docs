# 治理参数

在网络中存在一些特殊的参数，它们可通过链上治理被修改。持有通证的用户都可以参与到参数修改的链上治理。 如果社区对某些可修改的参数不满意，可以发起参数修改提案，社区投票通过后即可在线自动完成修改。

## Auth 模块可治理参数

| 字段                          | 描述                               | 有效范围                  | 当前值 |
| ----------------------------- | ---------------------------------- | ------------------------- | ------ |
| `auth/MaxMemoCharacters`      | 交易的memo字段的最大字符数         | (0, 18446744073709551615] | 256    |
| `auth/TxSigLimit`             | 每个交易的最大签名数               | (0, 18446744073709551615] | 7      |
| `auth/TxSizeCostPerByte`      | 交易每个字节消耗的gas量            | (0, 18446744073709551615] | 10     |
| `auth/SigVerifyCostED25519`   | 在ED25519算法签名验证上花费的gas   | (0, 18446744073709551615] | 590    |
| `auth/SigVerifyCostSecp256k1` | 在Secp256k1算法签名验证上花费的gas | (0, 18446744073709551615] | 1000   |

## Bank 模块可治理参数

| 字段                      | 描述                    | 有效范围     | 当前值 |
| ------------------------- | ----------------------- | ------------ | ------ |
| `bank/SendEnabled`        | 支持transfer的token集合 |              | []     |
| `bank/DefaultSendEnabled` | 默认是否支持转账功能    | {true,false} | true   |

## Gov 模块可治理参数

| 字段                | 描述                   | 有效范围                                          | 当前值                                                       |
| ------------------- | ---------------------- | ------------------------------------------------- | ------------------------------------------------------------ |
| `gov/depositparams` | 提议抵押阶段的相关参数 | max_deposit_period:(0, 9223372036854775807]       | {"min_deposit": [{"denom": "ugnc", "amount": "1000000000"}], "max_deposit_period": "86400s" }, |
| `gov/votingparams`  | 提议投票阶段的相关参数 | voting_period:(0, 9223372036854775807]            | {"voting_period": "432000s"}                                 |
| `gov/tallyparams`   | 投票统计阶段的相关参数 | quorum:[0,1] threshold:(0,1] veto_threshold:(0,1] | {"quorum":"0.500000000000000000","threshold": "0.500000000000000000","veto_threshold": "0.330000000000000000"} |

## Staking 模块可治理参数

| 字段                        | 描述                   | 有效范围                 | 当前值   |
| --------------------------- | ---------------------- | ------------------------ | -------- |
| `staking/UnbondingTime`     | 抵押解绑时间           | (0, 9223372036854775807] | 1814400s |
| `staking/MaxValidators`     | 最大验证人数量         | (0, 4294967295]          | 100      |
| `staking/MaxEntries`        | 解绑、转委托的最大数量 | (0, 4294967295]          | 7        |
| `staking/BondDenom`         | 可抵押的代币           |                          | ugnc     |
| `staking/HistoricalEntries` | 历史条目               | [0, 4294967295]          | 10000    |