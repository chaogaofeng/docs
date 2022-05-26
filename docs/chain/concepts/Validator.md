# 验证人

>  提示
>
>  基础概念请参考 [基础概念](chain/concepts/General.md)

## 常见问题

### 验证人节点的状态有哪些

通过`create-validator`交易创建验证人后，它们可能会处于以下三种状态：

- `bonded`：验证人在活跃验证人集合中，可参与共识并获得奖励，若行为不当会导致抵押的部分通证被罚没。
- `unbonding`：
  - 验证人节点行为异常且已被监禁（Jail），即被移出活跃验证人集合。监禁时长是确定的，超过监禁时长后，验证人可以发送`unjail`交易以解除监禁。
  - 验证人抵押的GNC数量（包括受委托）脱离了前100名，因此而成为候选人。可以通过抵押或委托更多的GNC使自己进入前100名，他将即时重新获得验证人身份。
- `unbonded`：验证人不在活跃验证人集合中，因此不能参与共识。此类验证人不会受到惩罚，也不会获得任何奖励。但仍然可以接受委托。

### 有哪些不同类型的密钥

简而言之，有两种类型的密钥：

#### Tendermint 密钥

这是用于共识投票签名的唯一密钥。

- 由`gnchaind init`创建节点时生成

- 查询bech32前缀为`gncval`的共识公钥， 

  ```bash
  gnchaind tendermint show-validator
  ```

- 查询bech32前缀为 `gncvalcons` 的节点地址

  ```bash
  gnchaind tendermint show-address
  ```

- 查询对应的节点Hex地址

  ```bash
  gnchaind status | jq .ValidatorInfo.Address
  ```

它也存储在`gnchaind`工作目录`config/priv_validator.json`中。

#### 应用程序密钥

该密钥可以通过`gnchaind`创建，用于签名交易。应用程序密钥与以`gncpub`为前缀的公钥和以`gnc`为前缀的地址相关联。两者都是由`gnchaind keys add`生成的帐户密钥派生出来的。

注意：验证人操作员的密钥直接与应用程序密钥绑定，地址和公钥分别使用保留的前缀：`gncval`和`gncvalpub`。

### 如何备份验证人节点

安全备份验证人节点私钥非常**重要**，这是恢复验证人节点的唯一方法。请注意，这里指的是**Tendermint密钥**.

如果您使用的是软件签名（tendermint的默认签名方法），则您的**Tendermint密钥**位于`HOME/.gnchain/config/priv_validator.json`中。最简单的方法是备份整个config文件夹。

### 如何迁移/恢复验证人节点

迁移验证人的方法有很多，最推荐的方法是：

1.在新服务器上运行运行全节点

2.追赶上最新区块之后，停止验证人节点和全节点

3.将全节点的 `config` 文件夹替换为验证人的

4.启动新的验证人节点

### 什么是“自抵押”？我如何增加“自抵押”

自抵押就是给自己验证人节点的委托。可以通过从用于创建验证人的操作员帐户发送“委托”交易来增加此金额。

### 想要成为活跃的验证人最少要抵押多少GNC

最低抵押 `1GNC` 即可创建验证人，但能否成为活跃的验证人取决于您的抵押（包括受委托）数量是否超过第100名验证人。

### 验证人可以卷走委托人的资金吗

通过委托GNC给一个验证人节点，用户同时委托了对应的投票权。验证人的投票权越高，他们在共识和治理流程中的权重就越大。但这并不意味着验证人可以托管其委托人的资金。 **验证人绝不可能动用其委托人的资金**。

即使验证人无法窃取委托资金，但如果验证人行为不当，委托人仍会被动承担责任和惩罚。

### 多久选择一次验证人来提议下一个区块？它是否与抵押的GNC数量有关

被选择来提议下一个区块的验证人称为提议者。验证人被选为提议者的概率是确定的，与验证人的投票权（即绑定的GNC数量）成正比。例如，如果所有验证人的投票权总额为 100 GNC，而其中一个验证人的投票权为10，则此验证人将提议出约10％的区块。

### 如何查询我的验证人地址

验证人地址有2种：

- 验证人操作员地址，即用来创建验证人节点的**应用程序密钥**

查询验证人的操作员地址（gncval...）和pubkey（gncvalpub...）：

```bash
gnchaind keys show alice --bech=val
```

- 验证人节点地址，即[Tendermint密钥]()

查询关联的地址（gncvalcons...）和共识pubkey（gncvalconspub...），请参考[Tendermint密钥]()

## 常见错误

### 验证人的投票权为0

可能是您的验证人被监禁或由于抵押（包括受委托）的数量排到了前100名以外。

如果您的验证人被监禁，可以按以下步骤操作来恢复：

- 如果`gnchaind`没有运行，请重新启动：

  ```bash
  gnchaind start
  ```

- 等待节点赶上最新的区块，检查验证人会被监禁到什么时间（了解[监禁时长]()）：

  ```bash
  # 查询验证人节点共识公钥
  gnchaind tendermint show-validator --home=$HOME/.gnchain
  
  # 使用共识公钥查询节点状态
  gnchaind query slashing signing-info [validator-conspub]
  ```

  您将可以看到 `Jailed Until` 的时间，只有在该时间之后，您才可以执行接下来的步骤。

- 如果当前时间已经超过了 `Jailed Until`，即可执行[解禁]()操作：

  ```bash
  gnchaind tx stake unjail --from=<key-name> --fees=0.3ugnc --chain-id=gnchain
  ```

- 再次检查您的验证人，看看您的投票权是否恢复。

  ```bash
  gnchaind status
  ```

  您可能会注意到您的投票权比以前低，那是因为你被惩罚了。

### 异常退出：too many open files

Linux可以打开（每个进程）的默认文件数是 `1024`，而 `gnchaind` 进程会打开超过1024个文件，进而导致进程崩溃。一个快速的解决方法是执行 `ulimit -n 4096`（增加允许的打开文件数量，仅对当前会话有效），然后使用 `gnchaindstart` 重新启动。如果您使用的是systemd或其他进程管理器来启动 `gnchaind`，则最好在该级别进行一些配置。

- 示例`systemd`配置：

  ```toml
  # /etc/systemd/system/gnchain.service
  [Unit]
  Description=gnchain node
  After=network.target
  
  [Service]
  Type=simple
  User=ubuntu
  WorkingDirectory=/home/ubuntu
  ExecStart=/home/ubuntu/go/bin/gnchaind start
  Restart=on-failure
  RestartSec=3
  LimitNOFILE=65535
  
  [Install]
  WantedBy=multi-user.target
  ```

- 在Ubuntu系统中修改全局ulimit示例：

  ```bash
  # Edit limits.conf
  vim /etc/security/limits.conf
  
  # Append the following lines at the bottom
  * hard nofile 65535
  * soft nofile 65535
  root hard nofile 65535
  root soft nofile 65535
  
  # Reboot the system
  reboot
  
  # Re-login & Check whether ulimit is updated to 65535
  ulimit -n
  ```