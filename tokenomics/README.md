# Tokenomics

SOFA.org has meticulously designed a dual-token model to operate ecosystem tokenomics. The protocol's native utility token is called $RCH, while the governance token is appropriately named $SOFA.

Let’s explore the vibrant ecosystem of SOFA.org, powered by $RCH and $SOFA tokens.

## 🤝 **Meet $RCH: The Utility Token**

$RCH is SOFA’s main token, and you can’t just buy it early or get it from the developers. To earn $RCH, you have to use SOFA.org’s services or join later referral programs. This ensures that only active users benefit, avoiding quick, harmful sell-offs seen in other projects. Each day, **users who transact on SOFA.org get $RCH based on their activity.** Not even the project developers have any ownership of $RCH on day 1 - how fair is THAT? This fair launch method ensures long-term success and benefits for the true users of the platform.

**Total Supply:** Capped at 37,000,000 $RCH.

**Pre-Minted Supply:** 67% (25 million) locked in a Uniswap liquidity pool with ETH.

**Airdrop Supply:** 33% released to users on a set schedule.

**Revenue Usage:** All SOFA protocol revenue buys and burns $RCH from Uniswap.

**Deflationary Mechanism:** Increased transactions lead to more $RCH burns, reducing supply and increasing value.

**Long-Term Value:** $RCH price rises with protocol usage, benefiting active users and holders.

At project launch, **67% of $RCH (25 million tokens) is pre-minted and placed into Uniswap's V3 pool on Ethereum along with ~725 ETH** (~$2.7M) as the initial liquidity pool

Please note, **this initial liquidity is not owned by anyone**. After depositing into the LP,** the corresponding Uniswap LP tokens are promptly destroyed, ensuring that the token’s initial liquidity pool can never be withdrawn.**

This ensures that the $RCH float available will eventually be far less than what was originally locked into the LP, **thereby setting an effective floor on the value of $RCH at its initial price.**

### 🪂 Did Someone Say Airdrops?!?!

Furthermore, **the remaining 33% (12 million tokens) of $RCH will be airdropped to ecosystem users** according to a predetermined release schedule.

Initially, **12,500 of $RCH tokens will be airdropped daily over the first 180 days.** After which, the airdropped amounts will decrease by an incremental 20% every 180 days ad infinitum.

| **Days after Launch** | **Daily Airdrop** |
| --------------------- | ----------------- |
| 0                     | 12,500            |
| 180                   | 10,000            |
| 360                   | 8,000             |
| 540                   | 6,400             |
| 720                   | 5,120             |
| 900                   | 4,096             |
| 1080                  | 3,276.8           |

### 📝Other $RCH Notes

**Positive Reflexivity: **More transactions increase $RCH price, making airdrops more valuable and encouraging more transactions.

**Self-Correcting Mechanism: **If $RCH price drops, more tokens get burned, stabilizing the price until transactions pick up again.

**No Exit Dumps: **$RCH's fair launch prevents sudden large sell-offs since no one gets $RCH at launch.

**Future Growth: **New DeFi projects can join the SOFA.org ecosystem as partners.

## Acquiring $RCH Tokens & Airdrop Math

**The best way to obtain $RCH is to transact** and execute transactions with the SOFA ecosystem.  A set amount of $RCH will be airdropped daily to our protocol users, with **received rewards to be split pro-rata based on the user's transaction volumes on the day**.

> 💰 RCH = User Executed Premium (on the day) / Total Premium Handled by SOFA (on the day) * [Vault Weight] * 95% (dApp Access)

## Option Premium Approximation

For **Earn based products**, given that the option premium will be funded by the deposit interest savings from Aave, only a **small portion of the user's deposit total will be considered "premium'** for airdrop calculations. On the contrary, the **entire ****purchase amount from Surge products** will be eligible for consideration, as it will be fully deployed to underlying option strategies.  An _approximate_ premium estimate is as follows:

| **Protocol** | **Premium (Approximation)**                                                             | **Comment**                                                                                                     |
| ------------ | --------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------- |
| Earn         | (Aave’s Savings Rate – Product Base Yield) * Maturity (in Days) / 365 * Deposit Value | SOFA uses only a portion of Aave’s interest income to risk as option premium to maintain a minimum base return |
| Surge        | The Entire Purchased Amount                                                             | The entire purchase is spent as an option premium to speculate on high upsides.                                 |

Moreover, as an attempt to incentivize activities across both protocols, we have included a **[Vault Weight]** at the end of the formula, currently set to [50] for Earn, and [1] for Surge at our initial launch.  Finally, a final [95%] adjustment factor is applied to account for the airdrop haircut paid to the **dApp broker**.

As SOFA grows and additional protocols come in, you can appreciate how important this variable will be in driving TVL, and this is going to be one of the vital parameters that can be voted on by our **$SOFA governance token** holders.

## 🤝 **Meet $SOFA: The Governance Token**

The **$SOFA token is the governance token of the SOFA.org ecosystem**. As a decentralized, non-profit, open-source technology organization, all decisions within the SOFA.org ecosystem are determined by votes from $SOFA token holders. **As a pure governance token, $SOFA does not participate in any profit sharing within the ecosystem.**

## 🎤 The Voice of the Community

**Subject Matters Pertaining to Governance Votes:**

1. Types of financial products to be onboarded
2. Determination of the eligible basket of supported collateral (USDT etc.)
3. Admission of new partner protocols into the SOFA.org ecosystem
4. Distribution mix % of daily $RCH airdrops between the various on-boarded products
5. Distribution mix % of daily $RCH airdrops amongst ecosystem protocols
6. Pace of $RCH airdrops
7. And other decisions to come as the ecosystem matures

## Acquiring $RCH Tokens & Airdrop Math

**The best way to obtain $RCH is to transact** and execute transactions with the SOFA ecosystem.  A set amount of $RCH will be airdropped daily to our protocol users, with **received rewards to be split pro-rata based on the user's transaction volumes on the day**.

> 💰 RCH = (User Executed Premium (on the day) * [Vault Weight]) / Total Vault Weighted Premium Handled by SOFA (on the day) * 95% (dApp Access)

> 💰 Total Vault Weighted Premium = (Total Earn Premium * Earn Weight) + (Total Surge Premium * Surge Weight)

### Option Premium Approximation

_Please be sure to read the [protocol fees section](../technical-design/fees.md) for more information on premium calculations._

For **Earn based products**, given that the option premium will be funded by the deposit interest savings from Aave, only a **small portion of the user's deposit total will be considered 'premium'** for airdrop calculations. On the contrary, the **entire purchase amount from Surge products** will be eligible for consideration, as it will be fully deployed to underlying option strategies.  An _approximate_ premium estimate is as follows:

| **Protocol** | **Premium (Approximation)**                                                             | **Comment**                                                                                                     |
| ------------ | --------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------- |
| Earn         | (Aave’s Savings Rate – Product Base Yield) * Maturity (in Days) / 365 * Deposit Value | SOFA uses only a portion of Aave’s interest income to risk as option premium to maintain a minimum base return |
| Surge        | The Entire Purchased Amount                                                             | The entire purchase is spent as an option premium to speculate on high upsides.                                 |

Moreover, as an attempt to incentivize activities across both protocols, we have included a **[Vault Weight]** at the end of the formula, currently set to [50] for Earn, and [1] for Surge at our initial launch.  Finally, a final [95%] adjustment factor is applied to account for the airdrop haircut paid to the [dApp broker](https://ainxbawxjd.larksuite.com/docx/Nh5VdSBNaoiSG7xX0lQuukPWsWd#Ahowd455xoIOMfxyAa3uXxrVsnd).

As SOFA grows and additional protocols come in, you can appreciate how important this variable will be in driving TVL, and this is going to be one of the vital parameters that can be voted on by our [$SOFA governance token](https://ainxbawxjd.larksuite.com/docx/Nh5VdSBNaoiSG7xX0lQuukPWsWd#doxusfrl6Z1n2pCyGcT7NdRNLWb) holders.

_Note: The system captures daily snapshots. $RCH tokens are available for claim every day at UTC 08:00 following the execution of your trade._
