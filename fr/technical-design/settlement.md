# Considérations sur le règlement en chaîne

## Logistique de règlement

> **Défis de planification des tâches dans le monde de la DeFi**

Avec des systèmes conventionnels, **la récupération de données programmée dans le temps est un défi pour la blockchain, car les contrats intelligents n'ont pas de planificateur de tâches 'Cron-like' natif**. Comment pouvons-nous concevoir un mécanisme automatisé pour garantir que les prix sont programmés et mis à jour à temps dans la crypto ?

Les 'algo-stables' crypto tels que Basis Cash ou ESD ont autrefois balayé le secteur de la DeFi, déployant une approche d'intervalle 'époque' qui nécessitait un déclenchement _manuel_ pour faire avancer les opérations à la fin de chaque époque et établir un nouveau prix de règlement. **Des mécanismes de récompense étaient intégrés dans le code des contrats intelligents pour inciter les bénévoles à 'déclencher l'avance'** et maintenir le bon fonctionnement des protocoles. De même, un projet populaire appelé Keep3r fondé par Andre Cronje (Yearn Finance, Fantom) a créé un tableau d'offres d'emploi virtuel où l'on pouvait publier des demandes d'avance et des récompenses de travail spécifiées pour ce but très spécifique.

Aucune des solutions n'était parfaite car elles **dépendaient finalement de l'intervention humaine** (intérêts personnels motivés), sans parler des **incertitudes concernant le temps d'exécution final réel**, compte tenu des efforts manuels requis. Cela peut conduire à des situations inconfortables où le règlement n'est pas mis à jour jusqu'à bien après l'expiration, entraînant une expérience utilisateur moins que satisfaisante.

Pourrions-nous créer et compter sur des solutions hors chaîne qui appellent à un déclenchement périodique des mises à jour de prix ? Bien que cela soit possible, **nous restons désintéressés par des 'raccourcis' centralisés** où le protocole pourrait être gravement compromis par des risques tels qu'une défaillance de serveur.

Comme couvert dans notre section oracle, SOFA utilise **le service d'automatisation de ChainLink comme notre source de prix de règlement**. Leur service permet l'exécution conditionnelle des fonctions de contrat intelligent via une plateforme d'automatisation fiable et décentralisée avec un réseau éprouvé d'opérations de nœuds externes, sécurisant actuellement des milliards en TVL.

Enfin, avec le **prix de règlement final en place, les utilisateurs et les teneurs de marché peuvent appeler le contrat pour brûler leur Position Token à tout moment et recevoir leur paiement dû**, dont les calculs sont gérés sans effort par le contrat intelligent avec des flux de données fiables provenant des oracles décentralisés de SOFA.

## Calculs de paiement

Le contrat pour le calcul des paiements des produits est complètement standardisé, et est indépendant du Vault et agnostique au collatéral. Au lieu de cela, **les contrats de calcul sont définis par le type sous-jacent de produit structuré** pour calculer les paiements modèles. Quelques exemples de paiements sont listés ci-dessous :

### _Plage limitée_

- $$Payoff{maker}=\begin{cases}X, HighSettlePrice\geq HighStrikePrice\vee LowSettlePrice\leq LowStrikePrice\\  0, HighSettlePrice<HisghStrikePrice\wedge LowSettlePrice>LowStrikePrice\end{cases}$$
- $$Payoff {user}=X - Payoff {maker}$$

### _Tendance intelligente_

#### _Tendance haussière_

- $$Payoff{maker}=\begin{cases}0, SettlePrice\geq HighStrikePrice\\
X\times\frac{HighStrikePrice-SettlePrice}{HighStrikePrice-LowStrikePrice},LowStrikePrice<SettlePrice<HighStrikePrice\\
X, SettlePrice\leq LowStrikePrice\end{cases}$$

- $$Payoff {user}=X - Payoff {maker}$$

#### _Tendance baissière_

- $$Payoff{maker}=\begin{cases}X, SettlePrice\geq HighStrikePrice\\

X\times\frac{SettlePrice-LowStrikePrice}{HighStrikePrice-LowStrikePrice},LowStrikePrice<SettlePrice<HighStrikePrice\\
0, SettlePrice\leq LowStrikePrice\end{cases}$$

- $$Payoff {user}=X - Payoff {maker}$$