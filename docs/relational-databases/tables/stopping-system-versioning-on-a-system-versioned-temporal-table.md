---
description: Arrêt du contrôle de version par le système sur une table temporelle à version contrôlée par le système
title: Arrêt du contrôle de version du système sur une table temporelle avec contrôle de version par le système | Microsoft Docs
ms.custom: ''
ms.date: 04/28/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
ms.assetid: dddd707e-bfb1-44ff-937b-a84c5e5d1a94
author: CarlRabeler
ms.author: carlrab
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: bfcd77b23a34fb7f892b99fb84ccedcc01b3abc6
ms.sourcegitcommit: 331b8495e4ab37266945c81ff5b93d250bdaa6da
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/20/2020
ms.locfileid: "88646553"
---
# <a name="stopping-system-versioning-on-a-system-versioned-temporal-table"></a>Arrêt du versioning du système sur une table temporelle avec contrôle de version par le système


[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]


Vous pouvez interrompre le contrôle des versions sur votre table temporelle temporairement ou définitivement. Pour ce faire, vous pouvez affecter à la clause **SYSTEM_VERSIONING** la valeur **OFF**.

## <a name="setting-system_versioning--off"></a>Configuration de SYSTEM_VERSIONING = OFF

Arrêtez le contrôle de version du système d’exploitation si vous souhaitez effectuer des opérations de maintenance spécifiques sur la table temporelle ou si vous n’avez plus besoin d’une table avec version. Suite à cette opération, vous obtiendrez deux tables indépendantes :

- La table actuelle avec définition de la période

- Une table historique sous la forme d’une table normale

### <a name="important-remarks"></a>Remarques importantes

- La table d’historique va **arrêter** la capture des mises à jour pendant la durée de **SYSTEM_VERSIONING = OFF**.
- Aucune perte de données ne se produit sur la **table temporaire** quand vous définissez **SYSTEM_VERSIONING = OFF** ou supprimez la période **SYSTEM_TIME**.
- Quand vous définissez **SYSTEM_VERSIONING = OFF** sans supprimer la période **SYSTEM_TIME** , le système continue à mettre à jour les colonnes de période pour chaque opération d’insertion et de mise à jour. Les suppressions effectuées sur la table actuelle sont définitives.
- Supprimez la période **SYSTEM_TIME** pour supprimer les colonnes de période définitivement.
- Quand vous définissez **SYSTEM_VERSIONING = OFF**, tous les utilisateurs disposant des autorisations suffisantes peuvent modifier le schéma et le contenu de la table historique, et même supprimer définitivement la table historique.
- Vous ne pouvez pas définir **SYSTEM_VERSIONING = OFF** si d’autres objets ont été créés avec SCHEMABINDING à l’aide d’extensions de requête temporelles (telles que le référencement de **SYSTEM_TIME**). Cette restriction empêche l’échec de ces objets si vous définissez **SYSTEM_VERSIONING = OFF**.

### <a name="permanently-remove-system_versioning"></a>Supprimer définitivement SYSTEM_VERSIONING

Cet exemple supprime définitivement SYSTEM_VERSIONING et complètement les colonnes de période. La suppression des colonnes de période est facultative.

```sql
ALTER TABLE dbo.Department SET (SYSTEM_VERSIONING = OFF);
/*Optionally, DROP PERIOD if you want to revert temporal table to a non-temporal*/
ALTER TABLE dbo.Department
DROP PERIOD FOR SYSTEM_TIME;
```

### <a name="temporarily-remove-system_versioning"></a>Supprimer temporairement SYSTEM_VERSIONING

Voici la liste des opérations qui nécessitent la définition du contrôle de version par le système avec la valeur **OFF**:

- Suppression des données superflues de l’historique (**DELETE** ou **TRUNCATE**)
- Suppression des données de la table actuelle sans contrôle de version (**DELETE**, **TRUNCATE**)
- Partition **SWITCH OUT** à partir de la table actuelle
- Partition **SWITCH IN** dans la table historique

Cet exemple arrête temporairement SYSTEM_VERSIONING pour que vous puissiez effectuer des opérations de maintenance spécifiques. Si vous arrêtez le contrôle de version temporairement à des fins de maintenance de tables, nous vous recommandons vivement d’effectuer cette opération dans une transaction pour préserver la cohérence des données.

> [!NOTE]
> Quand vous réactivez la gestion système des versions, n’oubliez pas de spécifier l’argument HISTORY_TABLE. Sinon, une nouvelle table d’historique sera générée et associée à la table actuelle. La table d’historique initiale sera conservée comme une table standard, mais elle ne sera pas associée à la table actuelle.

```sql
BEGIN TRAN
ALTER TABLE dbo.Department SET (SYSTEM_VERSIONING = OFF);
TRUNCATE TABLE [History].[DepartmentHistory]
WITH (PARTITIONS (1,2))
ALTER TABLE dbo.Department SET
(
SYSTEM_VERSIONING = ON (HISTORY_TABLE = History.DepartmentHistory)
);
COMMIT ;
```

## <a name="next-steps"></a>Étapes suivantes

- [Tables temporelles](../../relational-databases/tables/temporal-tables.md)
- [Prise en main des tables temporelles avec versions gérées par le système](../../relational-databases/tables/getting-started-with-system-versioned-temporal-tables.md)
- [Gérer la rétention des données d’historique dans les tables temporelles avec contrôle de version par le système](../../relational-databases/tables/manage-retention-of-historical-data-in-system-versioned-temporal-tables.md)
- [Tables temporelles avec version gérée par le système avec tables à mémoire optimisée](../../relational-databases/tables/system-versioned-temporal-tables-with-memory-optimized-tables.md)
- [Création d’une table temporelle avec versions gérées par le système](../../relational-databases/tables/creating-a-system-versioned-temporal-table.md)
- [Modification des données dans une table temporelle avec version gérée par le système](../../relational-databases/tables/modifying-data-in-a-system-versioned-temporal-table.md)
- [Interrogation des données dans une table temporelle avec version gérée par le système](../../relational-databases/tables/querying-data-in-a-system-versioned-temporal-table.md)
- [Modification du schéma d’une table temporelle à version contrôlée par le système](../../relational-databases/tables/changing-the-schema-of-a-system-versioned-temporal-table.md)
