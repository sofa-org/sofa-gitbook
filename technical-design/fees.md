# Protocol Fees

At SOFA, we are committed to creating a fair and transparent financial ecosystem where users can democratically purchase in a wide gamut of structured products with transparent and fair pricing from market makers.  Furthermore, **as a fully decentralized project, we want to ensure that any protocol gains are primarily shared with our underlying users**, ensuring full incentive alignment rather than privileged payouts to special interest groups.  As such, we have devised a series of fee structures with **'fair-launch' tokenomics to ensure proper value-accrual and support long-term platform longevity**.  There will be no VC joy-riding or exit-liquidity dumps within SOFA.

## Protocol Fees

> 💰 SOFA will collect 15% of the user's option premium as a base trading fee.  Furthermore, in the event of a 'winning' payout occurring for the user, a further 5% settlement fee will be charged against the total Gross Upside Payout.

## Fee Calculations

For full transparency, please see the following example of how fees and payoffs are calculated in our protocols.

**Definitions:**

$$Premium = Notional_{Deposit} * (1 + AAVE_{1MonthAverage} - BaseYield)^{Full Days/365} - Notional_{Deposit}$$

_MM's Collateral = Vault-Locked Collateral from Market Maker _

**Observation Window:**

From the _next 16:00 (UTC+8) to Expiry Day 16:00 (UTC+8)_

**User Payoffs at Expiry:**

1. If Knocked-Out

    $$Payoff_{inUSDT} = Notional_{Deposit} + AAVEInterest_{Actual} - Premium * (1 + 0.15)$$

2. No Knocked-Out

    $$Payoff_{inUSDT} = Notional_{Deposit} + AAVEInterest_{Actual} + Premium * (1 - 0.15 - 0.05) + MM'sCollateral * (1 - 0.05)$$

### Numerical Example (Rangebound)

![](../static/fees_formula.png)

### Distribution Waterfall (USDT)

![](../static/distribution_waterfall.png)

### Native Token ($RCH) Buyback

> **Aligning protocol success with Token performance**

At SOFA, we believe that the** most straight-forward way to align user and hodler incentives are through token buybacks**, with $RCH being our native utility token.  We will **commit all protocol income to be exclusively used for $RCH buybacks**, creating a virtuous loop of token price gains being driven off protocol usage.

With a **fixed deflationary supply, methodical release schedule, and usage-driven airdops, the long-term value accretion for $RCH is largely a function of protocol revenue,** which in itself is a direct measure of adoption success.  Furthermore, with **Token airdrops exclusively limited to protocol users and supporters**, we can ensure that they stand best to benefit from SOFA's long-term success, staying true to DeFi's commerical ideals of 'giving back' to true core users and early adopters.

**The token buyback logic is an integral part of the SOFA protocol smart contract**.  Protocol administrators will regularly trigger this process to purchase and burn $RCH with protocol revenue through supported DEX venues.  Destroyed $RCH will be no longer enter circulation, and the total amount of $RCH will gradually decrease over time on rising protocol usage.

More details of our Tokenomics model will be covered in its own dedicated section below.
