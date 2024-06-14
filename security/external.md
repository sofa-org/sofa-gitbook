# External Contingencies & Risk Factors

As much as we have endeavoured to design the SOFA protocols to be 'risk-free', we are cognizant that unforeseen "force majeure" type of risks are always a distant possibility.  Furthermore, every design decision naturally comes with some form of compromise as a trade-off, and we have tried our best to list out some expected and 'edge case' risk factors here below.

1. **Unfavourable Payoff Outcomes & Variability**
    - For Surge-based products, if the actual asset price movement moves unfavourably against the terms of product (e.g. outside the barriers), there is a risk of losing the entire principal.
    - For Earn-based products, the exact interest payout of the structure is dependent on the underlying asset's price movement and value at expiry date.

2. **Early Withdrawal Considerations**
    - For specific products with an early termination feature (i.e. if the price touches or exceeds either barrier, aka. Knock-out), such as with the Rangebound product, there is a chance that the transaction will 'early-terminate' before the stated maturity date.
    - Should the user decide to withdraw their deposited capital early from the vault, and we reiterate - it's an option, **not** an obligation - there's a chance for the returned capital to be less than the expected base return, and in some rare cases even below the original deposited amount.
    - The derived payout is wholly determined by when the transaction is 'knocked-out' versus the maturity length, as well as what the prevailing interest rate is at Aave for the purpose of the interest calculation.

3. **DeFi Extensibility Risks with External Protocols**
    - SOFA.org adopts a very conservative and measured approach with using reputable, secure, and battle-tested protocols such as Aave for our staking activities; however, external 3rd party risks and exploits could always happen with these established protocols beyond our control.
    - The network of partner protocols will likely grow along with the SOFA ecosystem and DeFi industry in general.

4. **Staking Yield Deficit**
    - For most of the Earn-based products, the 'Base Yield' return and the embedded option premium is directly funded by the staking-income earned from Aave.  While the SOFA protocols will only use a _portion _of the expected income to fund the option premium as buffer, the product could still suffer from an income deficit should Aave yields collapse to say 0% as an unimaginable, 'black-swan' type of event.
    - In that case, the deposit returned to the user could be very slightly less than what they put in, with the difference being attributable to the Aave yield short-fall.

5. **Blockchain Systemic Risks**
    - The initial SOFA protocol will be initially deployed on Etherum and EVM compatible blockchains, and is naturally reliant on their Proof-of-Staking frameworks
    - An unfamothable compromise in blockchain security or other hacks would jeopardize the integrity of all DeFi protocols, including SOFA.

6. **Smart Contract Code Integrity**
    - [SOFA.org](http://SOFA.org) has engaged and will be engaging with the most reputable and professional security firms in the industry to audit our contracts. We view auditing as an ongoing process and will continue to invite more auditors as the protocol evolves.
    - The initial SOFA protocols have passed a full chain audit.
      - [Peckshield](https://github.com/peckshield/publications/blob/master/audit_reports/PeckShield-Audit-Report-Sofa-v1.0.pdf)
      - [Code4rena](https://code4rena.com/reports/2024-05-sofa-pro-league)
      - [SigmaPrime](https://github.com/sigp/public-audits/blob/master/reports/sofa/review.pdf)


