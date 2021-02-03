---
description: Utilisation des tables temporelles avec contrôle de version du système à mémoire optimisée
title: Utilisation des tables temporelles avec contrôle de version du système à mémoire optimisée | Microsoft Docs
ms.custom: ''
ms.date: 05/05/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
ms.assetid: 691d4f80-6754-43f5-8b43-d4facf08f6fc
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: acfdadf0216bd243f61339982615c4bf83796dfa
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99177975"
---
# <a name="working-with-memory-optimized-system-versioned-temporal-tables"></a>Utilisation des tables temporelles avec contrôle de version du système à mémoire optimisée


[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]


Cette rubrique décrit la façon dont l’utilisation d’une table temporelle avec contrôle de version du système à mémoire optimisée diffère de celle d’une table temporelle avec contrôle de version du système sur disque.

> [!NOTE]
> L’utilisation de la technologie Temporal avec des tables optimisées en mémoire s’applique uniquement à [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] et ne s’applique pas à [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].

## <a name="discovering-metadata"></a>Découverte des métadonnées

Pour découvrir les métadonnées relatives à une table temporelle avec version gérée par le système et optimisée en mémoire, vous devez combiner les informations de [sys.tables &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-tables-transact-sql.md) et [sys.internal_tables &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-internal-tables-transact-sql.md). Une table temporelle avec contrôle de version du système est présentée en tant que parent_object_id de la table de l’historique en mémoire interne.

Cet exemple indique comment exécuter une requête sur ces tables et joindre ces dernières.

```sql
SELECT SCHEMA_NAME (T1.schema_id) as TemporalTableSchema
    , OBJECT_NAME(IT.parent_object_id) as TemporalTableName
    , T1.object_id as TemporalTableObjectId
    , IT.Name as InternalHistoryStagingName
    , SCHEMA_NAME (T2.schema_id) as HistoryTableSchema
    , OBJECT_NAME (T1.history_table_id) as HistoryTableName
FROM sys.internal_tables IT
JOIN sys.tables T1
    ON IT.parent_object_id = T1.object_id
JOIN sys.tables T2
    ON T1.history_table_id = T2.object_id
WHERE T1.is_memory_optimized = 1 AND T1.temporal_type = 2

```

## <a name="modifying-data"></a>Modification de données

Les tables temporelles mémoire optimisées avec contrôle de version du système sont modifiables par le biais de procédures stockées compilées en mode natif, ce qui vous permet de convertir des tables mémoire optimisées non temporelles en tables avec contrôle de version du système et de conserver les procédures stockées en mode natif existantes.

Cet exemple indique la façon dont une table créée précédemment peut être modifiée dans un module compilé en mode natif.

```sql
CREATE PROCEDURE dbo.UpdateFXCurrencyPair
   (
      @ProviderID int
      , @CurrencyID1 int
      , @CurrencyID2 int
      , @BidRate decimal(8,4)
      , @AskRate decimal(8,4)
   )
WITH NATIVE_COMPILATION, SCHEMABINDING
   , EXECUTE AS OWNER
AS
   BEGIN ATOMIC WITH
   (TRANSACTION ISOLATION LEVEL = SNAPSHOT, LANGUAGE = N'English')
      UPDATE dbo.FXCurrencyPairs SET AskRate = @AskRate, BidRate = @BidRate
     WHERE ProviderID = @ProviderID AND CurrencyID1 = @CurrencyID1 AND CurrencyID2 = @CurrencyID2
END
GO ;

```

## <a name="see-also"></a>Voir aussi

- [Tables temporelles avec version gérée par le système avec tables à mémoire optimisée](../../relational-databases/tables/system-versioned-temporal-tables-with-memory-optimized-tables.md)
- [Création d’une table temporelle de contrôle de version du système à mémoire optimisée](../../relational-databases/tables/creating-a-memory-optimized-system-versioned-temporal-table.md)
- [Surveillance des tables temporelles avec contrôle de version du système à mémoire optimisée](../../relational-databases/tables/monitoring-memory-optimized-system-versioned-temporal-tables.md)
- [Considérations relatives avec Tables temporelles à mémoire optimisée et version système](../../relational-databases/tables/
- [Tables temporelles](../../relational-databases/tables/temporal-tables.md)
- [Vérifications de cohérence système des tables temporelles](../../relational-databases/tables/temporal-table-system-consistency-checks.md)
- [Gérer la rétention des données d’historique dans les tables temporelles avec contrôle de version par le système](../../relational-databases/tables/manage-retention-of-historical-data-in-system-versioned-temporal-tables.md)
- [Vues et fonctions de métadonnées de table temporelle](../../relational-databases/tables/temporal-table-metadata-views-and-functions.md)
