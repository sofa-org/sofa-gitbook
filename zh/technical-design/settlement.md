# 链上结算注意事项

## 结算物流

> **DeFi世界中的作业调度挑战**

对于传统系统来说微不足道的事情，**时间调度的数据检索对于区块链来说是一个挑战，因为智能合约没有原生的类似'Cron'的作业调度器**。我们如何设计一个自动化机制，以确保加密货币中的价格按时调度和更新？

加密货币中的“算法稳定币”如Basis Cash或ESD曾经席卷DeFi领域，采用“纪元”间隔方法，这需要在每个纪元结束时手动触发以推进操作并建立新的结算价格。**奖励机制被内置到智能合约代码中，以迫使志愿者“触发推进”**，并保持协议的顺利运行。同样，由Andre Cronje（Yearn Finance, Fantom）创立的一个名为Keep3r的热门项目创建了一个虚拟的工作板，人们可以在上面发布推进请求和指定的工作奖励，专门用于这个目的。

这两种解决方案都不太完美，因为它们**最终依赖于人为干预**（动机自利），更不用说由于需要手动操作而导致的**实际最终执行时间的不确定性**。这可能导致不愉快的情况，即结算修正直到到期后很久才更新，从而导致不太满意的用户体验。

我们能否创建和依赖于链下解决方案来定期触发价格更新？虽然可能，但**我们对集中化的“捷径”不感兴趣**，因为协议可能因服务器故障等风险而受到严重损害。

如我们在oracle部分所述，SOFA利用**ChainLink的自动化服务作为我们的结算定价来源**。他们的服务通过一个可靠且去中心化的自动化平台，利用经过验证的外部节点操作网络，当前保护着数十亿美元的TVL，实现智能合约功能的条件执行。

最后，随着**最终结算价格的确定，用户和市场做市商可以随时调用合约来销毁他们的头寸代币并接收应得的付款**，这些计算由智能合约无缝处理，并由SOFA的去中心化oracle提供可靠的数据馈送。

## 收益计算

用于计算产品收益的合约是完全标准化的，与Vault无关且与抵押品无关。相反，**计算合约是由结构化产品的基础类型定义的**，以计算模型支付。以下是一些收益示例：

### _区间限制_

- $$Payoff{maker}=\begin{cases}X, HighSettlePrice\geq HighStrikePrice\vee LowSettlePrice\leq LowStrikePrice\\  0, HighSettlePrice<HisghStrikePrice\wedge LowSettlePrice>LowStrikePrice\end{cases}$$
- $$Payoff {user}=X - Payoff {maker}$$

### _智能趋势_

#### _牛市趋势_

- $$Payoff{maker}=\begin{cases}0, SettlePrice\geq HighStrikePrice\\
X\times\frac{HighStrikePrice-SettlePrice}{HighStrikePrice-LowStrikePrice},LowSettlePrice<SettlePrice<HighStrikePrice\\
X, SettlePrice\leq LowStrikePrice\end{cases}$$

- $$Payoff {user}=X - Payoff {maker}$$

#### _熊市趋势_

- $$Payoff{maker}=\begin{cases}X, SettlePrice\geq HighStrikePrice\\
X\times\frac{SettlePrice-LowStrikePrice}{HighStrikePrice-LowStrikePrice},LowStrikePrice<SettlePrice<HighStrikePrice\\
0, SettlePrice\leq LowStrikePrice\end{cases}$$

- $$Payoff {user}=X - Payoff {maker}$$
