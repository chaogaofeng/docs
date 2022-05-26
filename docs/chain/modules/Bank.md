# Bank模块

该模块主要用于账户之间转账、查询账户余额，同时提供了通用的离线签名与交易广播方法。

## 用户操作
- 账户查询 

查询指定账户的余额。
```shell
gnchaind query bank balances [address] --denom=[denom]
```

- 账户间转账 

该命令包括了交易构造，签名，广播的所有操作。 如从账户A转账ugnc给账户B：
```shell
gnchaind tx bank send [A] [B] [10ugnc] --fees=0.3ugnc --chain-id=gnchain-45_1
```

- 构建未签名交易

为了提高账户安全性，支持交易离线签名保护账户的私钥。在任意交易中，使用参数--generate-only可以构建一个未签名的交易。使用转账交易作为示例：
```shell
gnchaind tx bank send [from_key_or_address] [to_address] [amount] --fees=0.3ugnc --generate-only
```
以上命令将构建一未签名交易：
```shell
{
    "type": "auth/StdTx",
    "value": {
        "msg": [ "txMsg" ],
        "fee": "fee",
        "signatures": null,
        "memo": ""
    }
}
```
将结果保存到文件<file>。

-  交易签名

对上述的离线交易进行签名：
```shell
gnchaind tx sign <file> --chain-id=gnchain_45-1 --from=<key-name>
```
将返回已签名的交易：
```shell
{
    "type": "auth/StdTx",
    "value": {
    "msg": [ "txMsg" ],
    "fee": "fee",
    "signatures": [
        {
        "pub_key": {
            "type": "tendermint/PubKeySecp256k1",
            "value": "A+qXW5isQDb7blT/KwEgQHepji8RfpzIstkHpKoZq0kr"
        },
        "signature": "5hxk/R81SWmKAGi4kTW2OapezQZpp6zEnaJbVcyDiWRfgBm4Uejq8+CDk6uzk0aFSgAZzz06E014UkgGpelU7w==",
        "account_number": "0",
        "sequence": "11"
        }
    ],
    "memo": ""
    }
}
```
将结果保存到文件<file>。

- 广播交易

广播离线产生的已签名的交易，在这里，你可以使用上面的sign命令生成的交易。当然，您可以通过任何方法生成已签名的交易。
```shell
gnchaind tx broadcast <file>
```
该交易将广播并执行。