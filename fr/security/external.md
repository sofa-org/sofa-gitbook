# Contingences Externes & Facteurs de Risque

Autant que nous avons tenté de concevoir les protocoles SOFA pour qu'ils soient 'sans risque', nous sommes conscients que des risques imprévus de type "force majeure" sont toujours une possibilité lointaine. De plus, chaque décision de conception s'accompagne naturellement d'un certain compromis en tant que trade-off, et nous avons fait de notre mieux pour énumérer ici certains facteurs de risque attendus et 'cas limites'.

1. **Résultats de Paiement Défavorable & Variabilité**
    - Pour les produits basés sur le Surge, si le mouvement réel du prix de l'actif évolue défavorablement par rapport aux termes du produit (par exemple, en dehors des barrières), il y a un risque de perdre l'intégralité du capital.
    - Pour les produits basés sur le Earn, le paiement d'intérêt exact de la structure dépend du mouvement de prix de l'actif sous-jacent et de sa valeur à la date d'expiration.

2. **Considérations de Retrait Anticipé**
    - Pour des produits spécifiques avec une fonctionnalité de résiliation anticipée (c'est-à-dire si le prix touche ou dépasse l'une des barrières, également appelée Knock-out), comme avec le produit Rangebound, il y a une chance que la transaction soit 'résiliée anticipativement' avant la date d'échéance indiquée.
    - Si l'utilisateur décide de retirer son capital déposé tôt du coffre, et nous le réitérons - c'est une option, **pas** une obligation - il y a une chance que le capital retourné soit inférieur au rendement de base attendu, et dans certains cas rares, même en dessous du montant déposé initialement.
    - Le paiement dérivé est entièrement déterminé par le moment où la transaction est 'knock-out' par rapport à la durée de maturité, ainsi que par le taux d'intérêt en vigueur à Aave pour le calcul des intérêts.

3. **Risques d'Extensibilité DeFi avec des Protocoles Externes**
    - SOFA.org adopte une approche très conservatrice et mesurée en utilisant des protocoles réputés, sécurisés et éprouvés tels qu'Aave pour nos activités de staking ; cependant, des risques et des exploits de tiers externes pourraient toujours survenir avec ces protocoles établis, au-delà de notre contrôle.
    - Le réseau de protocoles partenaires est susceptible de croître avec l'écosystème SOFA et l'industrie DeFi en général.

4. **Déficit de Rendement de Staking**
    - Pour la plupart des produits basés sur le Earn, le rendement 'Base Yield' et la prime d'option intégrée sont directement financés par les revenus de staking gagnés auprès d'Aave. Bien que les protocoles SOFA n'utiliseront qu'une _partie_ des revenus attendus pour financer la prime d'option en tant que tampon, le produit pourrait encore souffrir d'un déficit de revenus si les rendements d'Aave s'effondrent à 0 % dans un événement inimaginable de type 'cygne noir'.
    - Dans ce cas, le dépôt retourné à l'utilisateur pourrait être légèrement inférieur à ce qu'il a mis, la différence étant attribuable au manque à gagner d'Aave.

5. **Risques Systémiques de Blockchain**
    - Le protocole SOFA initial sera déployé sur les blockchains Ethereum et compatibles EVM, et dépend naturellement de leurs cadres de Proof-of-Staking.
    - Un compromis inimaginable dans la sécurité de la blockchain ou d'autres hacks compromettrait l'intégrité de tous les protocoles DeFi, y compris SOFA.

6. **Intégrité du Code des Contrats Intelligents**
    - [SOFA.org](http://SOFA.org) a engagé et engagera les entreprises de sécurité les plus réputées et professionnelles de l'industrie pour auditer nos contrats. Nous considérons l'audit comme un processus continu et continuerons d'inviter davantage d'auditeurs à mesure que le protocole évolue.
    - Les protocoles SOFA initiaux ont passé un audit complet de la chaîne.
      - [Code4rena](https://code4rena.com/reports/2024-05-sofa-pro-league)
      - [Peckshield](https://github.com/peckshield/publications/blob/master/audit_reports/PeckShield-Audit-Report-Sofa-v1.0.pdf)
      - [SigmaPrime](https://github.com/sigp/public-audits/blob/master/reports/sofa/review.pdf)