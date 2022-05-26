# Bech32 前缀

Bech32是由Pieter Wuille和Greg Maxwel提出的新比特币地址格式。除了比特币之外，bech32可以编码任何短二进制数据。私钥和地址可能指的是一些在网络中不同的角色，例如普通账户和验证人账户等。使用Bech32地址格式来提供对数据鲁棒的完整性检查。用户可读部分（human readable part）可帮助用户有效理解地址和阅读错误信息。Bech32更多细节见 [bip-0173](https://github.com/bitcoin/bips/blob/master/bip-0173.mediawiki)。

## 用户可读部分表

| HRP           | Definition                      |
| ------------- | :------------------------------ |
| gnc           | Account Address                 |
| gncpub        | Account Public Key              |
| gncvaloper    | Validator's Operator Address    |
| gncvaloperpub | Validator's Operator Public Key |
| gncvalcons    | Tendermint Consensus Address    |
| gncvalconspub | Tendermint Consensus Public Key |

## 账户密钥示例

一旦创建一个新的账户，你将会看到以下信息：

```bash
NAME:    TYPE:           ADDRESS:                                PUBKEY:
test1    local    gnc1lw6cr7wy6fvf8uxlggv39pl2r2l737sks5w996    '{"@type":"/cosmos.crypto.sm2.PubKey","key":"AheeIdreFnLLs/LgsDf4CH+fkje+nYS+cqcW5/UQSL5c"}'
```

这意味着你创建了一个新账户地址 `gnc1lw6cr7wy6fvf8uxlggv39pl2r2l737sks5w996`， 它的用户可读部分是 `gnc`。

## 验证人密钥示例

在执行 `gnchaind init`命令时回自动产生一个Tendermint的共识密钥给该节点。你可以通过以下命令查询：

```bash
gnchaind  tendermint show-validator --home=$HOME/.gnchain
```

示例输出：

```bash
{"@type":"/cosmos.crypto.sm2.PubKey","key":"A48j8tj18XHvEkZtFNMGYyTbDl08w/2h0WAiisCOSbbR"}
```