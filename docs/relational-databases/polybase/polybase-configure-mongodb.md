---
title: 'Accéder aux données externes : MongoDB - PolyBase'
description: L’article explique comment utiliser PolyBase sur une instance SQL Server pour interroger des données externes dans MongoDB. Créez des tables externes pour référencer les données externes.
ms.date: 12/13/2019
ms.metadata: seo-lt-2019
ms.prod: sql
ms.technology: polybase
ms.topic: conceptual
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mikeray
monikerRange: '>= sql-server-linux-ver15 || >= sql-server-ver15'
ms.openlocfilehash: 306feebece733cf382f486dc686117016800f4d1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351790"
---
# <a name="configure-polybase-to-access-external-data-in-mongodb"></a>Configurer PolyBase pour accéder à des données externes dans MongoDB

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

L’article explique comment utiliser PolyBase sur une instance SQL Server pour interroger des données externes dans MongoDB.

## <a name="prerequisites"></a>Prérequis

Si vous n’avez pas installé PolyBase, consultez [Installation de PolyBase](polybase-installation.md).

La [clé principale](../../t-sql/statements/create-master-key-transact-sql.md) doit être créée avant les informations d’identification incluses dans l’étendue de la base de données. 
    

## <a name="configure-a-mongodb-external-data-source"></a>Configurer une source de données externes MongoDB

Pour interroger les données d’une source de données MongoDB, vous devez créer des tables externes pour référencer les données externes. Cette section fournit un exemple de code pour créer ces tables externes.

Les commandes Transact-SQL suivantes sont utilisées dans cette section :

- [CREATE DATABASE SCOPED CREDENTIAL (Transact-SQL)](../../t-sql/statements/create-database-scoped-credential-transact-sql.md)
- [CREATE EXTERNAL DATA SOURCE (Transact-SQL)](../../t-sql/statements/create-external-data-source-transact-sql.md) 
- [CREATE STATISTICS (Transact-SQL)](../../t-sql/statements/create-statistics-transact-sql.md)

1. Créez des informations d’identification incluses dans l’étendue de la base de données pour accéder à la source MongoDB.

    ```sql
    /*  specify credentials to external data source
    *  IDENTITY: user name for external source. 
    *  SECRET: password for external source.
    */
    CREATE DATABASE SCOPED CREDENTIAL credential_name WITH IDENTITY = 'username', Secret = 'password';
    ```
    
   > [!IMPORTANT] 
   > Le connecteur ODBC MongoDB pour Polybase prend uniquement en charge l’authentification de base (l’authentification Kerberos n’est pas prise en charge).    
    
1. Créez une source de données externe avec [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md).

    ```sql
    /*  LOCATION: Location string should be of format '<type>://<server>[:<port>]'.
    *  PUSHDOWN: specify whether computation should be pushed down to the source. ON by default.
    *CONNECTION_OPTIONS: Specify driver location
    *  CREDENTIAL: the database scoped credential, created above.
    */
    CREATE EXTERNAL DATA SOURCE external_data_source_name
    WITH (LOCATION = 'mongodb://<server>[:<port>]',
    -- PUSHDOWN = ON | OFF,
    CREDENTIAL = credential_name);
    ```

1. **Facultatif :** Créez des statistiques sur une table externe.

    Pour des performances de requêtes optimales, nous vous recommandons de créer des statistiques sur les colonnes de table externe, en particulier celles utilisées pour les jointures, les filtres et les agrégats.

    ```sql
    CREATE STATISTICS statistics_name ON customer (C_CUSTKEY) WITH FULLSCAN; 
    ```

>[!IMPORTANT] 
>Une fois que vous avez créé une source de données externes, vous pouvez utiliser la commande [CREATE EXTERNAL TABLE](../../t-sql/statements/create-external-table-transact-sql.md) afin de créer une table requêtable sur cette source.
>
>Pour obtenir un exemple, consultez [Créer une table externe pour MongoDB](../../t-sql/statements/create-external-table-transact-sql.md#k-create-an-external-table-for-mongodb).

## <a name="flattening"></a>Aplanissement
L’aplanissement est activé pour les données imbriquées et répétées depuis des collections de document MongoDB. L’utilisateur doit activer `create an external table` et spécifier explicitement un schéma relationnel sur des collections de documents MongoDB dont les données peuvent être imbriquées et/ou répétées. Les types de données JSON imbriquées/répétées seront aplanis comme suit :

* Objets : collection de clés/valeurs non ordonnées mises entre accolades (imbriquées)

   - SQL Server crée une colonne de table pour chaque clé d’objet

     * Nom de colonne : objectname_keyname

* Tableau : valeurs ordonnées, séparées par des virgules, mises entre crochets (répétées)

   - SQL Server ajoute une nouvelle ligne de table pour chaque élément de tableau

   - SQL Server crée une colonne par tableau pour stocker l’index d’élément de tableau

     * Nom de colonne : arrayname_index

     * Type de données : bigint

Cette technique peut occasionner plusieurs problèmes, notamment les deux suivants :

* Un champ répété vide masque de facto les données contenues dans les champs plats du même enregistrement

* La présence de plusieurs champs répétés peut entraîner une explosion du nombre de lignes générées

À titre d’exemple, SQL Server évalue la collection de restaurants de l’exemple de jeu de données MongoDB stockée dans un format JSON non relationnel. Chaque restaurant dispose d’un champ d’adresse imbriqué et d’un tableau de notes (« grades ») attribuées à des jours différents. La figure ci-dessous illustre un restaurant type avec une adresse imbriquée et des notes imbriquées/répétées.

![Aplatissement MongoDB](../../relational-databases/polybase/media/mongo-flattening.png "Aplatissement de MongoDB Restaurant")

L’adresse de l’objet sera aplanie comme suit :

* Le champ imbriqué restaurant.address.building devient restaurant.address_building
* Le champ imbriqué restaurant.address.coord devient restaurant.address_coord
* Le champ imbriqué restaurant.address.street devient restaurant.address_street
* Le champ imbriqué restaurant.address.zipcode devient restaurant.address_zipcode

Les notes du tableau seront aplanies comme suit :

| grades_date | grades_grade  | games_score | 
| ------------- | ------------------------- | -------------- |
|1393804800000 |Un |2|
|1378857600000|Un |6|
|135898560000 |Un |10|
|1322006400000|Un |9|
|1299715200000 |B |14|

## <a name="cosmos-db-connection"></a>Connexion Cosmos DB

À l’aide de l’API Mongo Cosmos DB et du connecteur PolyBase MongoDB, vous pouvez créer une table externe pour une **instance Cosmos DB**. Pour cela, vous devez suivre les étapes listées ci-dessus. Vérifiez que les informations d’identification incluses dans l’étendue de la base de données, l’adresse du serveur, le port et la chaîne d’emplacement sont bien ceux du serveur Cosmos DB. 

## <a name="next-steps"></a>Étapes suivantes

Pour en savoir plus sur PolyBase, consultez [Vue d’ensemble de SQL Server PolyBase](polybase-guide.md).
