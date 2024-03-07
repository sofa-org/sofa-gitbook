# Settlement

## Settle Price

The concept of expiration date is crucial in options products. Users and market makers can settle their gains or losses after the product expires. Users and market makers can choose any time after expiration to execute the settlement operation, but the dependent Price(s) cannot be from any arbitrary time. They must be the price at the time of expiration or the price with the expiration time as the right boundary. Since smart contracts do not have a Cron-like mechanism, we need to design a system to ensure prices are updated on time. But how can we achieve this?

Algorithmic stablecoins once swept through the DeFi sector, like Basis Cash or ESD. They all had an epoch design, requiring someone to trigger the advance operation at the end of each epoch to rebase the price. These protocols designed reward mechanisms in the contract part; the person who triggers the advance would receive a reward exceeding their transaction costs, thereby encouraging others to actively trigger the contract's advance method. Andre Cronje had introduced a project called Keep3r, where one could pay some fees to post jobs similar to triggering the advance method, and keepers could earn rewards by completing jobs. However, these two solutions share a common flaw: they rely on the psychology that people, attracted by rewards, will trigger the function as soon as possible. But in reality, the execution time of transactions cannot be strictly guaranteed, leading to situations where the price may be updated a long time after the expiration.

Some might wonder, why not just periodically trigger the price update transaction ourselves? It's not that it's impossible, but it's too centralized; if the server running the program fails, the protocol would become unusable.

We utilize ChainLink's Automation service to execute the settle update price. 

>Chainlink Automation enables conditional execution of your smart contract functions through a hyper-reliable and decentralized automation platform that uses the same external network of node operators that secure billions in value.

At any time after the price update, users and market makers can call the contract to burn their position Token, and the contract will calculate the due Payoff based on the price updated by the Oracle, completing the payment.

## Calculate Payoff

The contract for calculating Payoff is independent of the Vault and unrelated to Collateral. The same type of product uses the same contract to calculate Payoff.

Burn X position tokens

### Double No Touch

- $$Payoff{maker}=\begin{cases}X, HighSettlePrice\geq HighStrikePrice\vee LowSettlePrice\leq LowStrikePrice\\  0, HighSettlePrice<HisghStrikePrice\wedge LowSettlePrice>LowStrikePrice\end{cases}$$
- $$Payoff {user}=X - Payoff {maker}$$

### Smart Trend

#### Smart Bull

- $$Payoff{maker}=\begin{cases}0, SettlePrice\geq HighStrikePrice\\
X\times\frac{HighStrikePrice-SettlePrice}{HighStrikePrice-LowStrikePrice},LowStrikePrice<SettlePrice<HighStrikePrice\\
X, SettlePrice\leq LowStrikePrice\end{cases}$$

- $$Payoff {user}=X - Payoff {maker}$$

#### Smart Bear

- $$Payoff{maker}=\begin{cases}X, SettlePrice\geq HighStrikePrice\\
X\times\frac{SettlePrice-LowStrikePrice}{HighStrikePrice-LowStrikePrice},LowStrikePrice<SettlePrice<HighStrikePrice\\
0, SettlePrice\leq LowStrikePrice\end{cases}$$

- $$Payoff {user}=X - Payoff {maker}$$

