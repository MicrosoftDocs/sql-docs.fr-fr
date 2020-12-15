---
description: Calculs pushdown dans PolyBase
title: Calculs pushdown dans PolyBase | Microsoft Docs
dexcription: Enable pushdown computation to improve performance of queries on your Hadoop cluster. You can select a subset of rows/columns in an external table for pushdown.
ms.date: 11/17/2020
ms.prod: sql
ms.technology: polybase
ms.topic: conceptual
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: ''
monikerRange: '>= sql-server-2016'
ms.openlocfilehash: 844ab3f2a13e2bd5a9e3ea26d41012a51e7329a4
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/14/2020
ms.locfileid: "97416520"
---
# <a name="pushdown-computations-in-polybase"></a>Calculs pushdown dans PolyBase

[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

Le calcul pushdown améliore les performances des requêtes sur les sources de données externes. À compter de SQL Server 2016, les calculs pushdown étaient disponibles pour les sources de données externes Hadoop. SQL Server 2019 introduit des calculs pushdown pour d’autres types de sources de données externes.

## <a name="enable-pushdown-computation"></a>Activer le calcul pushdown

Les articles suivants contiennent des informations sur la configuration du calcul pushdown pour des types spécifiques de sources de données externes :

- [Activer le calcul pushdown dans Hadoop](polybase-configure-hadoop.md#pushdown)
- [Configurer PolyBase pour accéder à des données externes dans Oracle](polybase-configure-oracle.md)
- [Configurer PolyBase pour accéder à des données externes dans Teradata](polybase-configure-teradata.md)
- [Configurer PolyBase pour accéder à des données externes dans MongoDB](polybase-configure-mongodb.md)
- [Configurer PolyBase pour accéder à des données externes avec des types génériques ODBC](polybase-configure-odbc-generic.md)
- [Configurer PolyBase pour accéder à des données externes dans SQL Server](polybase-configure-sql-server.md)

## <a name="select-a-subset-of-rows"></a>Sélectionner un sous-ensemble de lignes

Utilisez une poussée vers le bas de prédicat pour améliorer les performances d’une requête qui sélectionne un sous-ensemble de lignes d’une table externe.

Dans cet exemple, SQL Server 2016 lance un travail MapReduce pour récupérer les lignes qui correspondent au prédicat `customer.account_balance < 200000` sur Hadoop. Comme la requête peut s’effectuer correctement sans analyser toutes les lignes de la table, seules les celles qui répondent aux critères du prédicat sont copiées sur SQL Server. Cette opération permet de gagner un temps considérable et nécessite moins d’espace de stockage temporaire quand le nombre de soldes clients inférieur à < 200000 est faible par rapport au nombre de clients ayant des soldes de compte supérieurs à 200000.

```sql
SELECT * FROM customer WHERE customer.account_balance < 200000
SELECT * FROM SensorData WHERE Speed > 65;  
```

### <a name="select-a-subset-of-columns"></a>Sélectionner un sous-ensemble de colonnes

Utilisez une poussée vers le bas de prédicat pour améliorer les performances d’une requête qui sélectionne un sous-ensemble de colonnes d’une table externe.

Dans cette requête, SQL Server lance une tâche Map/Reduce pour prétraiter le fichier texte délimité Hadoop afin que seules les données pour les deux colonnes, customer.name et customer.zip_code, soient copiées dans SQL Server.

```sql
SELECT customer.name, customer.zip_code
FROM customer
WHERE customer.account_balance < 200000
```

### <a name="pushdown-for-basic-expressions-and-operators"></a>Poussée vers le bas pour les opérateurs et expressions de base

SQL Server autorise les opérateurs et expressions de base suivants pour une poussée vers le bas de prédicat.

- Opérateurs de comparaison binaire (`<`, `>`, `=`, `!=`, `<>`, `>=`, `<=`) pour les valeurs numériques, d’heure et de date.
- Opérateurs arithmétiques (`+`, `-`, `*`, `/`, `%`).
- Opérateurs logiques (`AND`, `OR`).
- Opérateurs unaires (`NOT`, `IS NULL`, `IS NOT NULL`).

Les opérateurs `BETWEEN`, `NOT`, `IN` et `LIKE` peuvent être refoulés. Le comportement réel dépend de la façon dont l’optimiseur de requête réécrit les expressions des opérateurs sous la forme d’une série d’instructions qui utilisent des opérateurs relationnels de base.

La requête de cet exemple comporte plusieurs prédicats pouvant être refoulés vers Hadoop. SQL Server peut placer des travaux MapReduce dans Hadoop pour exécuter le prédicat `customer.account_balance <= 200000`. L’expression `BETWEEN 92656 AND 92677` est également constituée d’opérations binaires et logiques qui peuvent être empilées vers Hadoop. Le **AND** logique dans `customer.account_balance AND customer.zipcode` est une expression finale.

Étant donnée cette combinaison de prédicats, les travaux MapReduce peuvent exécuter l’ensemble de la clause WHERE. Seules les données qui répondent aux critères `SELECT` seront recopiées dans SQL Server.

```sql
SELECT * FROM customer 
WHERE customer.account_balance <= 200000 
    AND customer.zipcode BETWEEN 92656 AND 92677
```

## <a name="examples"></a>Exemples

### <a name="force-pushdown"></a>Forcer la poussée vers le bas

```sql
SELECT * FROM [dbo].[SensorData]
WHERE Speed > 65
OPTION (FORCE EXTERNALPUSHDOWN);
```

### <a name="disable-pushdown"></a>Désactiver la poussée vers le bas

```sql
SELECT * FROM [dbo].[SensorData]
WHERE Speed > 65
OPTION (DISABLE EXTERNALPUSHDOWN);
```

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur PolyBase, consultez [Qu’est-ce que PolyBase ?](polybase-guide.md).
