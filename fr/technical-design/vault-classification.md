# Classification des Coffres

Le contrat Vault utilise la norme ERC-1155 pour **supporter la fongibilité des Tokens de Position avec le même prix d'exercice et la même date d'expiration**. Les tokens avec des prix d'exercice ou des dates d'expiration différents auront des IDs de Token différents (c'est-à-dire comme un NFT) tout en étant contenus sous le même contrat Vault.

Le protocole SOFA est conçu avec la flexibilité **de supporter tout type de produit structuré et toute dénomination de garantie**, bien qu'une distinction soit faite sur la question de savoir si le produit sous-jacent contient ou non une fonctionnalité de 'protection du principal'. En tant que tel, nos **conceptions de Coffres sont divisées en 2 grandes catégories de produits explicites appelées 'Gagner' et 'Surge'**.

Exemple de Taxonomie des Coffres au Lancement de SOFA

![](../../static/NON1bznnEocSeJxK0mAuxBR0sqh.png)

## Produits Disponibles au Lancement

En tant que preuve de concept inaugurale, nous nous concentrerons initialement sur trois structures de produits populaires appelées **'Rangebound' et 'Trend'**. De plus, tous ces produits sont disponibles dans les protocoles Gagner ou Surge. De plus, d'autres types de produits seront continuellement ajoutés en fonction de la demande des utilisateurs et des retours de l'écosystème.

## Produits Rangebond

### Aperçu du Produit

Les produits Rangebound sont un type de produit structuré basé sur des limites de prix. **Les déposants peuvent réaliser un profit si l'actif sous-jacent ne touche pas les barrières prédéfinies pendant la période d'observation avant l'échéance**. Ces produits conviennent aux utilisateurs qui s'attendent à ce que le marché reste dans une phase de consolidation latérale avec une volatilité modérée.

En se référant au diagramme de paiement ci-dessous, un utilisateur croit que le BTC sera coincé entre une plage définie par les barrières inférieure et supérieure, mais ne veut pas encourir beaucoup de pertes même si cette opinion s'avère incorrecte. Le produit Rangebound garantira à l'utilisateur un rendement de base minimum de (A) même si le prix du Range sort de chaque côté, mais l'utilisateur aura droit à un retour de profit supplémentaire équivalent à (A+B) si les prix restent contenus dans les barrières.

Pour les options de personnalisation, **l'utilisateur est libre d'ajuster la largeur de la plage de prix, ce qui entraînerait un profil différent de profits de base et de profits supplémentaires selon les niveaux choisis**. Naturellement, spécifier une plage de prix plus étroite représente un pari plus agressif sur une faible volatilité, ce qui entraînerait des rendements supplémentaires plus élevés. À l'inverse, une plage de prix plus large réduirait les profits excédentaires en retour d'une probabilité plus élevée de gagner. Enfin, pour faciliter la gestion, le produit peut être spécifié pour être automatiquement 're-roulé' à l'échéance comme un pari de continuation lors d'une mise à niveau ultérieure du protocole. (La fonction 're-roll' sera déployée dans la prochaine phase de la mise à niveau).

![](../../static/Yfu7bNF7soTStDxRAp7u4g31sTe.png)

## Produits Trend

### Aperçu du Produit

Le **produit Trend est adapté aux déposants qui s'attendent à ce que le marché subisse bientôt une tendance douce et régulière, mais qui désire tout de même une certaine protection contre les baisses**. En définissant à la fois une barrière de prix inférieure et supérieure, les utilisateurs commenceront à accumuler des gains de revenus dès que l'instrument sous-jacent franchit le seuil de prix initial, avec des paiements croissants jusqu'à atteindre un maximum à la 2ème barrière.

De plus, contrairement au produit Rangebound, le produit Trend n'observe qu'au moment du règlement si le prix de l'actif sous-jacent se situe dans la plage définie pour déterminer le profit final (c'est-à-dire option européenne). **Ce produit est disponible via des expressions haussières et baissières**.

Par exemple, si un utilisateur est haussier sur le BTC et prédit que dans les sept prochains jours, le prix du BTC restera au-dessus de 68 500 $, mais ne dépassera pas 71 500 $, il peut choisir d'acheter un produit Bull Trend, en fixant 68 500 $ comme limite inférieure et 71 500 $ comme limite supérieure de la plage de profit. En se référant au diagramme ci-dessous, au moment du règlement, l'utilisateur sera éligible à un paiement entre un profit de base de (A) et une limite supérieure de (A+B), selon ce que sera le prix du BTC à ce moment-là.

![](../../static/VPCFbWcRso4WYsxeEExu2Wrjsyv.png)

### Scénarios Applicables

Le produit pourrait être **adapté aux déposants qui croient qu'un actif connaîtra une forte réaction à un événement économique spécifique, mais qui s'inquiètent également du risque d'une baisse de prix s'ils se trompent** dans leur pronostic de marché.

Par exemple, l'utilisateur pourrait désirer spéculer sur les prix du BTC lors de la prochaine décision du FOMC, l'événement de halving, ou la prochaine annonce d'approbation d'ETF. Cette stratégie est **appropriée pour les utilisateurs ayant une forte conviction sur le marché concernant ces événements importants mais risqués, et qui souhaitent rester disciplinés dans la gestion des pertes potentielles** si son avis s'avère erroné.

## Aileron de requin (Pour plus tard)

### Aperçu du produit

Comparé aux produits Rangebound et Trend, le produit 'Aileron de requin' est relativement plus connu sur les marchés TradFi grâce à son nom mémorable. Son design et son rendement sont similaires à ceux des produits Trend (c'est-à-dire une option européenne), avec la **principale exception que le déposant spécule de manière beaucoup plus agressive sur les prix se situant dans une plage particulière**.

Dit autrement, le produit Aileron de requin **adopte une vision plus conservatrice de la volatilité du marché en sacrifiant plus de potentiel à la hausse lors d'un grand mouvement du marché, en échange de profits plus agressifs dans les limites de prix inférieures**.

Par exemple, supposons qu'un utilisateur soit haussier sur le BTC, mais qu'il ait une forte conviction qu'il ne franchira pas une certaine limite supérieure à un niveau de résistance technique fort. Dans un tel scénario, l'utilisateur peut choisir d'acheter un produit Aileron de requin haussier avec un profil de rendement comme indiqué dans le diagramme ci-dessous. À la liquidation du produit, l'utilisateur recevra un retour allant de (A) à (A+B) en fonction de l'endroit où se trouve le BTC au comptant ; cependant, contrairement aux produits Trend, notez que le retour _diminue_ à (A+C) en cas de franchissement de la barrière de prix supérieure, offrant un compromis en échange de profits plus agressifs dans la plage précédente.

![](../../static/LaiabPyJAokoogxs2luuq7hsslb.png)