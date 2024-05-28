# On-Chain Settlement Considerations

## Settlement Logistics

> **Job-scheduling challenges in the world of DeFi**

With trivial for conventional systems, **time-scheduled data retrievals are challenging for blockchain, as smart contracts do not have a native 'Cron-like' job scheduler**.  How can we design an automated mechanism to ensure that prices are scheduled and updated on time in crypto?

Crypto 'algo-stables' such as Basis Cash or ESD once swept through the DeFi sector, deploying an 'epoch' interval approach which required a _manual_ trigger to advance operations at the end of each epoch and establish a new settlement price.  **Reward mechanisms were built-in to smart contract code to compel volunteers to 'trigger the advance'** and keep the protocols in smooth operation.  Similarly, a popular project called Keep3r founded by Andre Cronje (Yearn Finance, Fantom) created a virtual job board where one would post advance requests and specified job awards for this very specific purpose.

Neither solution was far from perfect as they **ultimately relied on human intervention** (motivated self-interests), not to mention **uncertainties over the actual final execution time**, given the manual efforts required.  This can lead to uncomfortable situations where the settlement fixing isn't updated until well after the expiry, leading to a less than satisfactory user-experience.

Could we create and rely on off-chain solutions that call for a periodic triggering of price updates?  While possible, **we remain disinterested in centralized 'shortcuts'** where the protocol could be critically impaired by risks such as a server failure.

As covered in our oracle section, SOFA utilizes** ChainLink's Automation service as our settlement pricing source**.  Their service enables conditional execution of smart contract functions through a reliable and decentralized automation platform with a proven network of external node operations, currently securing over billions in TVL.

Finally, with the** final settlement price in place, users and market makers can call the contract to burn their Position Token at anytime and receive their due payment**, which calculations seamlessly handled by the smart contract with reliable data feeds from SOFA's decentralized oracles.

## Payoff Calculations

The contract for calculating product payoffs is completely standardized, and is Vault-independent and collateral agnostic.  Instead, **calculation contracts are defined by the underlying type of structured product** to calculate the model payouts.  Some payoff examples are listed below:

### _Rangebound_

- $$Payoff{maker}=\begin{cases}X, HighSettlePrice\geq HighStrikePrice\vee LowSettlePrice\leq LowStrikePrice\\  0, HighSettlePrice<HisghStrikePrice\wedge LowSettlePrice>LowStrikePrice\end{cases}$$
- $$Payoff {user}=X - Payoff {maker}$$

### _Smart Trend_

#### _Bull Trend_

- $$Payoff{maker}=\begin{cases}0, SettlePrice\geq HighStrikePrice\\
X\times\frac{HighStrikePrice-SettlePrice}{HighStrikePrice-LowStrikePrice},LowStrikePrice<SettlePrice<HighStrikePrice\\
X, SettlePrice\leq LowStrikePrice\end{cases}$$

- $$Payoff {user}=X - Payoff {maker}$$

#### _Bear Trend_

- $$Payoff{maker}=\begin{cases}X, SettlePrice\geq HighStrikePrice\\
X\times\frac{SettlePrice-LowStrikePrice}{HighStrikePrice-LowStrikePrice},LowStrikePrice<SettlePrice<HighStrikePrice\\
0, SettlePrice\leq LowStrikePrice\end{cases}$$

- $$Payoff {user}=X - Payoff {maker}$$
