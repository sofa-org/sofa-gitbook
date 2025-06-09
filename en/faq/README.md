# SOFA FAQs

## Background

### 1. What is [SOFA.org](https://www.sofa.org/)'s mission statement?

[SOFA.org](https://www.sofa.org/) is a **decentralized, non-profit organization** focused on advancing the DeFi ecosystem. Our modus operandi is to promote the highest DeFi standards, support high-quality projects, and promote adoption of blockchain technologies across mainstream finance.

### 2. What exactly is [SOFA.org](https://www.sofa.org/) trying to build?

We are trying to develop a base-layer, settlement framework to handle the settlement of all financial assets on-chain.  Our landing protocols will focus on crypto Structured Products as an asset class.

### 3. Structured What?

Structured products are really just packaged products where users get to enjoy high potential yield with strong capital protection.  They usually involve selling options and monetizing volatility, but it is too complicated for the average person.  So the entire structure is wrapped as a simple product for users to pick and choose from our easy to use dApp, and supported by any interested market makers.

### 4. Is it safe?  Is my money going to be stolen?

Your assets (and the market maker's) will always be safe and visible on-chain in transparent smart contract vaults.

The dApp simply connects the user to the product market maker, and an execution is not done until the user commits and the protocol confirms that both sides have the requisite assets.  Upon successful execution, all the assets will be locked in our ERC-1155 vaults on Ethereum.  All of our code is open source and verifiable by anyone.

### 5. Are the SOFA protocols audited?

The initial SOFA protocols have passed a full chain audit with Peckshield, Code4rena, and SigmaPrime.
  - [Peckshield](https://github.com/peckshield/publications/blob/master/audit_reports/PeckShield-Audit-Report-Sofa-v1.0.pdf)
  - [Code4rena](https://code4rena.com/reports/2024-05-sofa-pro-league)
  - [SigmaPrime](https://github.com/sigp/public-audits/blob/master/reports/sofa/review.pdf)

### 6. Is [SOFA.org](https://www.sofa.org/) a market maker?

No, we are infrastructure and protocol builders, pure and simple.

### 7. Aren't other projects doing something similar using exotic products?

Somewhat, but not quite to the scale of our ambition.

There are certainly other protocols which promise high yielding structures using deposit epochs and other passive methodologies.  We are taking a significantly different approach than most at the moment:

- SOFA is not a market maker.  We are simply the protocol builders.  We do not take or handle user assets at all.  All deposit flow is handled by the on-chain smart-contract and vaults.
- Prices are conducted 'RFQ'-style (Request for Quote) with external market makers, rather than arbitrary fixed epochs.
- Users have full discretion on their product parameters, transaction timing, and the ultimate final execution decision.
- The SOFA protocols are inclusive to and welcome all external market makers.
- Our vaults are designed under the ERC-1155 standard, allowing vital instrument parameters to be immutably stored at the contract level.
- Our authentic position risk tokens, representing secure and transferrable collateral claims that can be recognized on partner DeFi and CeFi platforms in the future.
- We provide open APIs for market makers and developments to encourage maximum participation and growth of the SOFA protocols.

### 8. Who are you guys anyway?

[SOFA.org](https://www.sofa.org/) is supported by an experienced team of finance veterans, technology experts, and repeat entrepreneurs who are looking to build a meaningful project that can stand the test of time.  Most of us have been blessed with relatively successful careers and understand the power of the 'long-game' over short-term gains.

Furthermore, many of us are also natural disrupters with a desire to shake-up the stale financial system, and DeFi offers us that perfect venue to showcase our work in a transparent and verifiable manner.  LFG!

### 9. Developer documentation / Git-hub?

Please see [https://docs.sofa.org/en/developer/](https://docs.sofa.org/en/developer/) and [https://github.com/sofa-org/sofa-protocol](https://github.com/sofa-org/sofa-protocol)

### Protocol Explanation

### 10. What's the protocol use-case?

Users are able to earn attractive yields / income on their deposits & purchases of structured products on the SOFA protocols.  There will be additional utility token airdrops rewarded exclusively to users as well.

### 11. What are your unique selling points?

Users have wide latitude over their product customization, pricing is fair due to openness to any and all 3rd party market maker, products cater to both risk-averse as well as speculative users across different structures, protocol integrity is ensured through multiple audits, and asset security is maximized through the use of transparent smart contract vaults.

Most importantly, protocol users are heavily rewarded with an innovative, user-centric token airdrop and a tokenomics model emphasizing long-term sustainability above short-term profits.

### 12. How do the SOFA protocols make money?

Settlement fees.  Please see [https://docs.sofa.org/en/technical-design/fees.html](https://docs.sofa.org/en/technical-design/fees.html)

The protocol charges users a base fee (based on their option premium) for using our dApp for execution and smart contracts for settlement.  The protocol makes an additional upside fee if (and only if) the user 'wins' from a further upside outcome.

### 13. What does the protocol do with its income?

All protocol earnings are fully recycled to burn RCH tokens daily; there is no protocol 'treasury', so to speak.

We are generous to a fault.  We know.  Wait until you see our tokenomics.

## Tokenomics

### 14. Did you really 'fair launch' the $RCH utility tokens?  Like, literally all of it?

Yeah we did.  No VCs, no insider allocations.  The core team basically worked _pro-bono_ to accomplish our long-term mission.  See question 8 above.

In fact, much of the core team even missed out on the early Uniswap purchases - (No) thanks to all you degens!!!!

Mint TX here:

[https://etherscan.io/tx/0x03c5d2569d9d7d9cf79111aa4afda3dea5b7e4e3bb77c580caf75a88fc08eeb2](https://etherscan.io/tx/0x03c5d2569d9d7d9cf79111aa4afda3dea5b7e4e3bb77c580caf75a88fc08eeb2)

### 15. What's this about burning 750 ETH of initial liquidity at launch?

Yeah, as if the fair launch wasn't enough.  The DAO agreed to burn the initial 750 ETH of Uniswap LP to lock-in the initial liquidity.

Burn TX here: [https://etherscan.io/tx/0x3c0ab9ce92e466d1882a83b79f34aa879ba7845bf028779f71390e882f6733e7](https://etherscan.io/tx/0x3c0ab9ce92e466d1882a83b79f34aa879ba7845bf028779f71390e882f6733e7)

### 16. You guys crazy.  How are you funded?

Out of pocket.  Initial development expenses were funded by our core founding DAO members.

### 17. ... Ok really.  What's the catch?

... We _really_ wish there was one.

### 18. OK fine.  What's this $RCH token good for?

Please see [https://docs.sofa.org/en/tokenomics/](https://docs.sofa.org/en/tokenomics/)

$RCH is our utility token.  Fixed deflationary supply, 67% minted on day 1 on uniswap, 33% to be airdropped exclusively to users on a fixed daily schedule over a long period.

The idea of the utility token is to reward our users and long term token hodlers, and we achieve it with a symbiotic and positive reinforcement loop with our structured product offerings.

- Buyers of SOFA structured products get $RCH airdrops _on-top_ of their normal yield
- SOFA earns settlement fees.  SOFA recycles 100% of earnings to burn $RCH on Uniswap daily.
- Supply goes down, price goes up.  $RCH supply is fixed after all.
- Higher $RCH, higher _excess return_ on our structured products, making them more attractive.
- More buying, virtuous loop repeats.

**Naturally, $RCH supply is long-term deflationary as long as the protocol buyback volumes exceed our daily airdrop emissions.**

**Said in another way, the long-term success of our token is contingent on accumulated use of the SOFA protocols in the long-run.  **Sounds pretty fair right?

### 19. Hey I actually read your docs.  What's this Dual Token system?

Please see [https://docs.sofa.org/en/tokenomics/](https://docs.sofa.org/en/tokenomics/)

$RCH rewards our users and long-term hodlers from protocol earnings being recycled to burn the coins.  Effectively, we are accruing all the protocol income directly to our end users and token holders by raising the scarcity of the remaining tokens.

$SOFA is our governance token which will have its own TGE later (<6 months hopefully).  As a decentralized, non-profit, open-source technology organization, all decisions within the [SOFA.org](https://www.sofa.org/) ecosystem are determined by $SOFA token votes.  _To be clear, as a pure governance token, $SOFA does not participate in any profit sharing within the ecosystem._

### 20. What can the governance tokens vote on?

Please see [https://docs.sofa.org/en/tokenomics/](https://docs.sofa.org/en/tokenomics/)

_A sample but non-exhaustive list of voting measures includes:_

1. Types of new products to be onboarded
2. Determination of the eligible basket of supported collateral (USDT etc.)
3. Admission of new partner protocols into the [SOFA.org](https://www.sofa.org/) ecosystem
4. Distribution mix % of daily $RCH airdrops between the various on-boarded products
5. Distribution mix % of daily $RCH airdrops amongst ecosystem protocols
6. Pace of $RCH airdrops
7. And other decisions to come as the ecosystem matures

### 21. How can I earn more $RCH and $SOFA

**$RCH:**

- Buy them on Uniswap.  You know you want to.
- Start using the SOFA protocols to earn yields AND airdrops!

**$SOFA:**

They were awarded to our early DAO members, builders, advisors, and core community members that worked tirelessly to make our project a success.  Please contact the SOFA team if you would like to join our ever-growing community if you think you can make a material and positive contribution to our cause.

### 22. How can I optimize $RCH airdrop farming?

TL;DR.  Keep purchasing products on the SOFA protocols!

No, really.  The daily airdrop allocation is allocated based on a pro-rata of a user's traded premium vs the protocol's total traded premium, calculated and reset daily.  There are too many moving parts for one to 'optimize' airdrop farming, and it's designed as such.

Please focus on buying the right product with the risk profile that suits your needs!

For more calculation details, please see [https://docs.sofa.org/en/tokenomics/](https://docs.sofa.org/en/tokenomics/)

### 23. Exchange Listing?

$RCH was fair-launched on Uniswap without any prior CEX-listing discussions.  With that being said, a number of CEX have started to list $RCH voluntarily out of their own will.  Please see the Coingecko website for a full list of current markets:

[https://www.coingecko.com/en/coins/rch-token](https://www.coingecko.com/en/coins/rch-token)

