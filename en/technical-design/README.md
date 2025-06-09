## Product Scope

The SOFA protocol will** initially be focused on settling and tokenizing crypto structured products** as our proof-of-concept.  We are taking the 'road less travelled' to attack the most complex products first, where a successful deployment will see rapid expansion into all other crypto asset classes. The SOFA protocol will be launched on **Ethereum and other L1 EVM blockchains** to start.

## Protocol Workflow

![](../../static/draw4.png)

A high-level run-down of a typical trade execution via SOFA is as follows:

- Participating institutional market makers continuously stream executable prices on structured products to the protocol's dApp
- User selects and executes a particular structured product purchase based on the price shown
- User's committed assets are sent and locked in the product's DeFi vault
- The market maker's maximum premium exposure is also sent and locked in the vault
  - _Note: the transaction will not execute if either side fails to post their required assets at this point_
- Corresponding Position Tokens claims (referencing asset details) will be minted to both the user and market maker, freely transferrable like any conventional ERC-20 token to any other wallet destination

![](../../static/TnMSbh4G7oO4fDxf7FbuTkh2sbe.png)

- **For 'Earn' structures only,** the collateral in the vault would be staked into mature and safe yield earn protocols like Compound, AAVE and etc. to earn a base level of interest for users
  - Extra careful scrutiny will be placed on this step, with eligible destinations voted on by the Governance Token holders

![](../../static/Stosbf6jcoxtvyxnO3OuSb9XsPf.png)

- Finally, upon product expiry, the relevant payout will be released and claimable in the vault by both the user and the market maker
  - Should the respective Position Tokens be transferred to a new wallet, the owner of the address will instead be able to claim the asset at anytime post-expiry

## Token Standards (ERC-1155)

**The SOFA protocol tokenizes users' chain-locked positions via the ERC-1155 multi-token standard, recording vital position details such as [Expiry], [Anchor Prices], [Maker/Taker Toggle], and other fields as relevant to the instrument in question**.

![](../../static/UhIbbGdnioqb4pxRiouubc9fsOg.png)

Compared to conventional single-asset token standards, ERC-1155 allows the creation and management of multiple types of tokens within a single contract, with support for each type of token having different properties.  Positions with the same parameters can still be merged or split, while enabling transfers as conveniently as standard ERC-20 tokens.  **This innovation strikes a balance between wide asset compatibility, high flexibility, and gas efficiency**.

![](../../static/DkgrbQ5FDo5ZyxxdZvmuoixCsee.png)

Position Tokens with the same Strike Price and Expiry Time are fungible like any standard token, and can be batch settled in a single operation to allow significant gas savings.

