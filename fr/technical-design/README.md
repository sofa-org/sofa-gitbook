## Portée du produit

Le protocole SOFA sera **initialement axé sur le règlement et la tokenisation des produits structurés crypto** en tant que preuve de concept. Nous prenons le 'chemin moins fréquenté' pour attaquer d'abord les produits les plus complexes, où un déploiement réussi verra une expansion rapide dans toutes les autres classes d'actifs crypto. Le protocole SOFA sera lancé sur **Ethereum et d'autres blockchains L1 EVM** pour commencer.

## Flux de travail du protocole

![](../../static/draw4.png)

Un aperçu de haut niveau d'une exécution de trade typique via SOFA est le suivant :

- Les teneurs de marché institutionnels participants diffusent en continu des prix exécutables sur des produits structurés vers l'application décentralisée du protocole
- L'utilisateur sélectionne et exécute un achat de produit structuré particulier basé sur le prix affiché
- Les actifs engagés de l'utilisateur sont envoyés et verrouillés dans le coffre DeFi du produit
- L'exposition maximale de prime du teneur de marché est également envoyée et verrouillée dans le coffre
  - _Remarque : la transaction ne s'exécutera pas si l'une ou l'autre des parties ne parvient pas à poster ses actifs requis à ce stade_
- Des revendications de jetons de position correspondants (référencant les détails des actifs) seront frappées pour l'utilisateur et le teneur de marché, librement transférables comme tout autre jeton ERC-20 conventionnel vers toute autre destination de portefeuille

![](../../static/TnMSbh4G7oO4fDxf7FbuTkh2sbe.png)

- **Pour les structures 'Earn' uniquement,** la garantie dans le coffre serait mise en jeu dans des protocoles de rendement matures et sûrs comme Compound, AAVE, etc. pour générer un niveau de base d'intérêt pour les utilisateurs
  - Une attention particulière sera portée à cette étape, avec des destinations éligibles votées par les détenteurs de jetons de gouvernance

![](../../static/Stosbf6jcoxtvyxnO3OuSb9XsPf.png)

- Enfin, à l'expiration du produit, le paiement pertinent sera libéré et réclamable dans le coffre par l'utilisateur et le teneur de marché
  - Si les jetons de position respectifs sont transférés vers un nouveau portefeuille, le propriétaire de l'adresse pourra alors réclamer l'actif à tout moment après l'expiration

## Normes de jetons (ERC-1155)

**Le protocole SOFA tokenise les positions verrouillées sur la chaîne des utilisateurs via la norme de jeton multi-ERC-1155, enregistrant des détails vitaux de position tels que [Expiration], [Prix d'ancrage], [Bascule Maker/Taker], et d'autres champs pertinents pour l'instrument en question**.

![](../../static/UhIbbGdnioqb4pxRiouubc9fsOg.png)

Comparé aux normes de jetons à actif unique conventionnelles, l'ERC-1155 permet la création et la gestion de plusieurs types de jetons au sein d'un seul contrat, avec un support pour chaque type de jeton ayant des propriétés différentes. Les positions avec les mêmes paramètres peuvent toujours être fusionnées ou divisées, tout en permettant des transferts aussi facilement que des jetons ERC-20 standard. **Cette innovation trouve un équilibre entre une large compatibilité des actifs, une grande flexibilité et une efficacité en gaz**.

![](../../static/DkgrbQ5FDo5ZyxxdZvmuoixCsee.png)

Les jetons de position avec le même prix d'exercice et le même temps d'expiration sont fongibles comme tout jeton standard, et peuvent être réglés en lot en une seule opération pour permettre des économies de gaz significatives.