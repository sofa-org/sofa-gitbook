# Settlement

## Settle Price

到期日是期权产品里很重要的一个概念。用户和做市商在产品到期后就可以结算他们的收益或损失。用户和做市商可以选择到期后的任意时间执行结算操作，但结算依赖的 Price(s)绝不能任意时间的。他（们）必须是到期时的价格或者以到期时间为右边界的价格。因为智能合约没有 Cron 之类的设计，所以就要求我们设计一套机制保证价格可以按时被更新。那如何实现呢？

算法稳定币曾经在 DeFi 领域风靡一时，比如 Basis Cash 或者 ESD。他们都有 epoch 的设计，每个 epoch 结束时都需要有人去触发 advance 操作实现价格的 rebase。这些协议在合约部分设计了奖励机制，触发 advance 的人会获得超过其交易成本的额外奖励从而鼓励其他人赖主动触发合约的 advance 方法。Andre Cronje 曾推出过项目 Keep3r，项目放可以花费一些费用在上面发布类似于触发 advance 方法的 job，而 keeper 通过完成 job 可获得奖励。不过这两种方案有一个相同的缺陷，利用奖励吸引第三方来 trigger function，依赖的是人们为了争取到奖励肯定会第一时间来 trigger 的心理。但事实上交易被执行的时间无法严格保证，会出现过了 expire 时间很久之后价格才会被更新的情况。

或许有人会问，自己去定期触发更新价格的交易不可以吗？不是不行，但这样就太中心化了，万一运行程序的服务器出现故障，协议将会变得不可用。

我们使用 ChainLink 提供的 Automation 服务执行 settle 更新 price。Chainlink Automation enables conditional execution of your smart contract functions through a hyper-reliable and decentralized automation platform that uses the same external network of node operators that secures billions in value.

价格更新后的任意时间，用户和做市商都可以调用合约 burn 自己的头寸 Token，合约会根据 Oracle 更新的价格计算应得 Payoff，完成支付。

## Calculate Payoff

Payoff 的计算合约是独立于 Vault 的，和 Collateral 无关。同一种类型的产品使用同一个合约计算 Payoff。

Burn X position tokens

### Double No Touch

- $$
  Payoff{maker} = \begin{cases}X, \quad HighSettlePrice\geq HighStrikePrice or LowSettlePrice\leq LowStrikePrice\\  0, \quad HighSettlePrice<HisghStrikePrice and LowSettlePrice>LowStrikePrice\end{cases}
  $$
- $$
  Payoff {user}=X - Payoff {maker}
  $$

### Smart Trend

#### Smart Bull

- $$
  Payoff{maker} = \begin{cases}0, \quad SettlePrice\geq HighStrikePrice\\
  $$

X\times\frac{HighStrikePrice-SettlePrice}{HighStrikePrice-LowStrikePrice}, \quad LowStrikePrice<SettlePrice<HighStrikePrice\\
X,\quad SettlePrice\leq LowStrikePrice\end{cases}$$

- $$
  Payoff {user}=X - Payoff {maker}
  $$

#### Smart Bear

- $$
  Payoff{maker} = \begin{cases}X, \quad SettlePrice\geq HighStrikePrice\\
  $$

X\times\frac{SettlePrice-LowStrikePrice}{HighStrikePrice-LowStrikePrice}, \quad LowStrikePrice<SettlePrice<HighStrikePrice\\
0, \quad SettlePrice\leq LowStrikePrice\end{cases}$$

- $$
  Payoff {user}=X - Payoff {maker}
  $$

