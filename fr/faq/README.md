# FAQ SOFA

## Contexte

### 1. Quelle est la déclaration de mission de [SOFA.org](https://www.sofa.org/) ?

[SOFA.org](https://www.sofa.org/) est une **organisation décentralisée à but non lucratif** axée sur l'avancement de l'écosystème DeFi. Notre modus operandi est de promouvoir les normes DeFi les plus élevées, de soutenir des projets de haute qualité et de favoriser l'adoption des technologies blockchain dans la finance traditionnelle.

### 2. Que cherche exactement à construire [SOFA.org](https://www.sofa.org/) ?

Nous essayons de développer un cadre de règlement de couche de base pour gérer le règlement de tous les actifs financiers sur la chaîne. Nos protocoles de destination se concentreront sur les produits structurés crypto en tant que classe d'actifs.

### 3. Produits Structurés Quoi ?

Les produits structurés ne sont en réalité que des produits emballés où les utilisateurs peuvent bénéficier d'un rendement potentiel élevé avec une forte protection du capital. Ils impliquent généralement la vente d'options et la monétisation de la volatilité, mais c'est trop compliqué pour la personne moyenne. Ainsi, l'ensemble de la structure est enveloppé en tant que produit simple que les utilisateurs peuvent choisir dans notre dApp facile à utiliser, et soutenu par des teneurs de marché intéressés.

### 4. Est-ce sûr ? Mon argent va-t-il être volé ?

Vos actifs (et ceux du teneur de marché) seront toujours en sécurité et visibles sur la chaîne dans des coffres de contrats intelligents transparents.

La dApp connecte simplement l'utilisateur au teneur de marché du produit, et une exécution n'est pas effectuée tant que l'utilisateur ne s'engage pas et que le protocole ne confirme pas que les deux parties disposent des actifs requis. Après une exécution réussie, tous les actifs seront verrouillés dans nos coffres ERC-1155 sur Ethereum. Tout notre code est open source et vérifiable par quiconque.

### 5. Les protocoles SOFA sont-ils audités ?

Les protocoles SOFA initiaux ont passé un audit complet de la chaîne avec Peckshield, Code4rena et SigmaPrime.
  - [Peckshield](https://github.com/peckshield/publications/blob/master/audit_reports/PeckShield-Audit-Report-Sofa-v1.0.pdf)
  - [Code4rena](https://code4rena.com/reports/2024-05-sofa-pro-league)
  - [SigmaPrime](https://github.com/sigp/public-audits/blob/master/reports/sofa/review.pdf)

### 6. [SOFA.org](https://www.sofa.org/) est-elle un teneur de marché ?

Non, nous sommes des constructeurs d'infrastructure et de protocoles, tout simplement.

### 7. D'autres projets ne font-ils pas quelque chose de similaire en utilisant des produits exotiques ?

En quelque sorte, mais pas tout à fait à l'échelle de notre ambition.

Il existe certainement d'autres protocoles qui promettent des structures à haut rendement utilisant des époques de dépôt et d'autres méthodologies passives. Nous adoptons une approche significativement différente de la plupart en ce moment :

- SOFA n'est pas un teneur de marché. Nous sommes simplement les constructeurs de protocoles. Nous ne prenons ni ne gérons les actifs des utilisateurs. Tous les flux de dépôt sont gérés par le contrat intelligent et les coffres sur la chaîne.

- Les prix sont établis selon le style 'RFQ' (Request for Quote) avec des teneurs de marché externes, plutôt que des époques fixes arbitraires.
- Les utilisateurs ont une pleine discrétion sur leurs paramètres de produit, le moment de la transaction et la décision finale d'exécution.
- Les protocoles SOFA sont inclusifs et accueillent tous les teneurs de marché externes.
- Nos coffres sont conçus selon la norme ERC-1155, permettant de stocker de manière immuable des paramètres d'instrument vitaux au niveau du contrat.
- Nos véritables jetons de risque de position, représentant des revendications de garantie sécurisées et transférables qui pourront être reconnues sur des plateformes DeFi et CeFi partenaires à l'avenir.
- Nous fournissons des API ouvertes pour les teneurs de marché et les développements afin d'encourager une participation maximale et la croissance des protocoles SOFA.

### 8. Qui êtes-vous, au fait ?

[SOFA.org](https://www.sofa.org/) est soutenu par une équipe expérimentée de vétérans de la finance, d'experts en technologie et d'entrepreneurs répétitifs qui cherchent à construire un projet significatif capable de résister à l'épreuve du temps. La plupart d'entre nous ont eu la chance d'avoir des carrières relativement réussies et comprennent le pouvoir du 'long-game' par rapport aux gains à court terme.

De plus, beaucoup d'entre nous sont également des perturbateurs naturels avec le désir de secouer le système financier stagnant, et DeFi nous offre ce lieu parfait pour présenter notre travail de manière transparente et vérifiable. LFG !

### 9. Documentation pour développeurs / Git-hub ?

Veuillez consulter [https://docs.sofa.org/en/developer/](https://docs.sofa.org/en/developer/) et [https://github.com/sofa-org/sofa-protocol](https://github.com/sofa-org/sofa-protocol)

### Explication du protocole

### 10. Quel est le cas d'utilisation du protocole ?

Les utilisateurs peuvent gagner des rendements / revenus attractifs sur leurs dépôts et achats de produits structurés sur les protocoles SOFA. Il y aura également des airdrops de jetons utilitaires supplémentaires récompensés exclusivement aux utilisateurs.

### 11. Quels sont vos points de vente uniques ?

Les utilisateurs ont une grande latitude sur la personnalisation de leurs produits, les prix sont équitables grâce à l'ouverture à tous les teneurs de marché tiers, les produits s'adressent à la fois aux utilisateurs averses au risque et aux utilisateurs spéculatifs à travers différentes structures, l'intégrité du protocole est assurée par plusieurs audits, et la sécurité des actifs est maximisée grâce à l'utilisation de coffres de contrats intelligents transparents.

Plus important encore, les utilisateurs du protocole sont fortement récompensés par un airdrop de jetons innovant et centré sur l'utilisateur et un modèle de tokenomics mettant l'accent sur la durabilité à long terme plutôt que sur les profits à court terme.

### 12. Comment les protocoles SOFA gagnent-ils de l'argent ?

Frais de règlement. Veuillez consulter [https://docs.sofa.org/en/technical-design/fees.html](https://docs.sofa.org/en/technical-design/fees.html)

Le protocole facture aux utilisateurs des frais de base (basés sur leur prime d'option) pour l'utilisation de notre dApp pour l'exécution et des contrats intelligents pour le règlement. Le protocole perçoit des frais supplémentaires si (et seulement si) l'utilisateur 'gagne' d'un résultat supplémentaire.

### 13. Que fait le protocole de ses revenus ?

Tous les bénéfices du protocole sont entièrement recyclés pour brûler des jetons RCH quotidiennement ; il n'y a pas de 'trésorerie' du protocole, pour ainsi dire.

Nous sommes généreux à l'excès. Nous le savons. Attendez de voir notre tokenomics.

## Tokenomics

### 14. Avez-vous vraiment lancé équitablement les jetons utilitaires $RCH ? Comme, littéralement tout ?

Oui, nous l'avons fait. Pas de capital-risque, pas d'allocations internes. L'équipe de base a essentiellement travaillé _pro-bono_ pour accomplir notre mission à long terme. Voir la question 8 ci-dessus.

En fait, une grande partie de l'équipe de base a même raté les premiers achats sur Uniswap - (Non) merci à tous vous degens!!!!

Mint TX ici :

[https://etherscan.io/tx/0x03c5d2569d9d7d9cf79111aa4afda3dea5b7e4e3bb77c580caf75a88fc08eeb2](https://etherscan.io/tx/0x03c5d2569d9d7d9cf79111aa4afda3dea5b7e4e3bb77c580caf75a88fc08eeb2)

### 15. Qu'est-ce que c'est que cette histoire de brûler 750 ETH de liquidité initiale au lancement ?

Oui, comme si le lancement équitable n'était pas suffisant. Le DAO a convenu de brûler les 750 ETH initiaux de LP Uniswap pour verrouiller la liquidité initiale.

Burn TX ici : [https://etherscan.io/tx/0x3c0ab9ce92e466d1882a83b79f34aa879ba7845bf028779f71390e882f6733e7](https://etherscan.io/tx/0x3c0ab9ce92e466d1882a83b79f34aa879ba7845bf028779f71390e882f6733e7)

### 16. Vous êtes fous. Comment êtes-vous financés ?

De notre poche. Les dépenses de développement initiales ont été financées par nos membres fondateurs du DAO.

### 17. ... D'accord vraiment. Quel est le piège ?

... Nous _souhaitons vraiment_ qu'il y en ait un.

### 18. D'accord, très bien. À quoi sert ce jeton $RCH ?

Veuillez consulter [https://docs.sofa.org/en/tokenomics/](https://docs.sofa.org/en/tokenomics/)

$RCH est notre jeton utilitaire. Offre déflationniste fixe, 67 % frappés le jour 1 sur Uniswap, 33 % seront airdropés exclusivement aux utilisateurs selon un calendrier quotidien fixe sur une longue période.

L'idée du jeton utilitaire est de récompenser nos utilisateurs et les détenteurs de jetons à long terme, et nous y parvenons avec une boucle de renforcement positif et symbiotique grâce à nos offres de produits structurés.

- Les acheteurs de produits structurés SOFA reçoivent des airdrops de $RCH _en plus_ de leur rendement normal
- SOFA gagne des frais de règlement. SOFA recycle 100 % des bénéfices pour brûler $RCH sur Uniswap quotidiennement.
- L'offre diminue, le prix augmente. L'offre de $RCH est fixe après tout.
- Plus de $RCH, plus de _rendement excédentaire_ sur nos produits structurés, les rendant plus attractifs.
- Plus d'achats, la boucle vertueuse se répète.

**Naturellement, l'offre de $RCH est déflationniste à long terme tant que les volumes de rachat du protocole dépassent nos émissions quotidiennes d'airdrops.**

**Dit autrement, le succès à long terme de notre token dépend de l'utilisation accumulée des protocoles SOFA sur le long terme.** Ça semble assez juste, non ?

### 19. Hé, j'ai en fait lu vos documents. Quel est ce système de Double Token ?

Veuillez consulter [https://docs.sofa.org/en/tokenomics/](https://docs.sofa.org/en/tokenomics/)

$RCH récompense nos utilisateurs et hodlers à long terme grâce aux revenus du protocole qui sont recyclés pour brûler les pièces. Effectivement, nous accumulons tous les revenus du protocole directement pour nos utilisateurs finaux et détenteurs de tokens en augmentant la rareté des tokens restants.

$SOFA est notre token de gouvernance qui aura son propre TGE plus tard (dans moins de 6 mois, espérons-le). En tant qu'organisation technologique décentralisée, à but non lucratif et open-source, toutes les décisions au sein de l'écosystème [SOFA.org](https://www.sofa.org/) sont déterminées par les votes des tokens $SOFA. _Pour être clair, en tant que token de gouvernance pur, $SOFA ne participe à aucun partage de bénéfices au sein de l'écosystème._

### 20. Sur quoi les tokens de gouvernance peuvent-ils voter ?

Veuillez consulter [https://docs.sofa.org/en/tokenomics/](https://docs.sofa.org/en/tokenomics/)

_Une liste d'exemples mais non exhaustive des mesures de vote comprend :_

1. Types de nouveaux produits à intégrer
2. Détermination du panier éligible de garanties supportées (USDT, etc.)
3. Admission de nouveaux protocoles partenaires dans l'écosystème [SOFA.org](https://www.sofa.org/)
4. Répartition % des airdrops quotidiens de $RCH entre les différents produits intégrés
5. Répartition % des airdrops quotidiens de $RCH entre les protocoles de l'écosystème
6. Rythme des airdrops de $RCH
7. Et d'autres décisions à venir à mesure que l'écosystème mûrit

### 21. Comment puis-je gagner plus de $RCH et $SOFA ?

**$RCH :**

- Achetez-les sur Uniswap. Vous savez que vous en avez envie.
- Commencez à utiliser les protocoles SOFA pour gagner des rendements ET des airdrops !

**$SOFA :**

Ils ont été attribués à nos premiers membres de la DAO, bâtisseurs, conseillers et membres de la communauté centrale qui ont travaillé sans relâche pour faire de notre projet un succès. Veuillez contacter l'équipe SOFA si vous souhaitez rejoindre notre communauté en pleine croissance si vous pensez pouvoir apporter une contribution matérielle et positive à notre cause.

### 22. Comment puis-je optimiser le farming des airdrops de $RCH ?

TL;DR. Continuez à acheter des produits sur les protocoles SOFA !

Non, vraiment. L'allocation quotidienne d'airdrop est répartie en fonction d'un pro-rata du premium échangé par un utilisateur par rapport au premium total échangé du protocole, calculé et réinitialisé quotidiennement. Il y a trop de variables pour qu'on puisse 'optimiser' la culture d'airdrop, et c'est conçu de cette manière.

Veuillez vous concentrer sur l'achat du bon produit avec le profil de risque qui correspond à vos besoins !

Pour plus de détails sur les calculs, veuillez consulter [https://docs.sofa.org/en/tokenomics/](https://docs.sofa.org/en/tokenomics/)

### 23. Inscription sur les échanges ?

Le $RCH a été lancé équitablement sur Uniswap sans aucune discussion préalable sur une inscription en CEX. Cela dit, un certain nombre de CEX ont commencé à lister le $RCH volontairement de leur propre volonté. Veuillez consulter le site de Coingecko pour une liste complète des marchés actuels :

[https://www.coingecko.com/en/coins/rch-token](https://www.coingecko.com/en/coins/rch-token)