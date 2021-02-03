---
description: STATS_DATE (Transact-SQL)
title: STATS_DATE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 12/18/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STATS_DATE_TSQL
- STATS_DATE
dev_langs:
- TSQL
helpviewer_keywords:
- statistical information [SQL Server], last time updated
- STATS_DATE function
- query optimization statistics [SQL Server], last time updated
- last time statistics updated
- stats update date
ms.assetid: f9ec3101-1e41-489d-b519-496a0d6089fb
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ca416d3de8d295cd9f7f535ab2d3b8b65df150f6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181249"
---
# <a name="stats_date-transact-sql"></a>STATS_DATE (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Retourne la date de la mise à jour la plus récente des statistiques pour une table ou vue indexée.  
  
 Pour plus d’informations sur la mise à jour de statistiques, consultez [Statistiques](../../relational-databases/statistics/statistics.md).  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
STATS_DATE ( object_id , stats_id )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *object_id*  
 ID de la table ou vue indexée avec les statistiques.  
  
 *stats_id*  
 ID de l'objet de statistiques.  
  
## <a name="return-types"></a>Types de retour  
 Renvoie **datetime** en cas de réussite. Renvoie **NULL** si un objet blob de statistiques n’a pas été créé.  
  
## <a name="remarks"></a>Remarques  
 Les fonctions système peuvent être utilisées dans la liste de sélection, dans la clause WHERE et partout où une expression peut être utilisée.  
 
 La date de mise à jour des statistiques est stockée dans l’[objet blob de statistiques](../../relational-databases/statistics/statistics.md#DefinitionQOStatistics) avec l’[histogramme](../../relational-databases/statistics/statistics.md#histogram) et le [vecteur de densité](../../relational-databases/statistics/statistics.md#density), et non dans les métadonnées. Quand aucune donnée n’est lue pour générer des données de statistiques, l’objet blob de statistiques n’est pas créé et la date n’est pas disponible. C’est le cas pour les statistiques filtrées pour lesquelles le prédicat ne renvoie aucune ligne, ou pour les nouvelles tables vides.
 
 Si les statistiques correspondent à un index, la valeur *stats_id* dans l’affichage catalogue [sys.stats](../../relational-databases/system-catalog-views/sys-stats-transact-sql.md) est identique à la valeur *index_id* dans l’affichage catalogue [sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md).
  
## <a name="permissions"></a>Autorisations  
 Requiert l'appartenance au rôle de base de données fixe db_owner ou l'autorisation d'afficher les métadonnées pour la table ou vue indexée.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-return-the-dates-of-the-most-recent-statistics-for-a-table"></a>R. Retourner les dates des statistiques les plus récentes pour une table  
 L'exemple suivant retourne la date de la mise à jour la plus récente de chaque objet de statistiques dans la table `Person.Address`.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT name AS stats_name,   
    STATS_DATE(object_id, stats_id) AS statistics_update_date  
FROM sys.stats   
WHERE object_id = OBJECT_ID('Person.Address');  
GO  
```  
  
 Si les statistiques correspondent à un index, la valeur *stats_id* dans la vue de catalogue [sys.stats](../../relational-databases/system-catalog-views/sys-stats-transact-sql.md) est identique à la valeur *index_id* dans la vue de catalogue [sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md), et la requête suivante retourne les mêmes résultats que la requête précédente. Si les statistiques ne correspondent pas à un index, elles figurent dans les résultats sys.stats mais pas dans les résultats sys.indexes.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT name AS index_name,   
    STATS_DATE(object_id, index_id) AS statistics_update_date  
FROM sys.indexes   
WHERE object_id = OBJECT_ID('Person.Address');  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Exemples : [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="b-learn-when-a-named-statistics-was-last-updated"></a>B. Découvrir quand une statistique nommée a été mise à jour pour la dernière fois  
 L’exemple suivant crée des statistiques sur la colonne LastName de la table DimCustomer. Il exécute alors une requête pour afficher la date des statistiques. Ensuite, il met à jour les statistiques et réexécute la requête pour afficher la date de mise à jour.  
  
```sql
--First, create a statistics object  
USE AdventureWorksPDW2012;  
GO  
CREATE STATISTICS Customer_LastName_Stats  
ON AdventureWorksPDW2012.dbo.DimCustomer (LastName)  
WITH SAMPLE 50 PERCENT;  
GO  
  
--Return the date when Customer_LastName_Stats was last updated  
USE AdventureWorksPDW2012;  
GO  
SELECT stats_id, name AS stats_name,   
    STATS_DATE(object_id, stats_id) AS statistics_date  
FROM sys.stats s  
WHERE s.object_id = OBJECT_ID('dbo.DimCustomer')  
    AND s.name = 'Customer_LastName_Stats';  
GO  
  
--Update Customer_LastName_Stats so it will have a different timestamp in the next query  
GO  
UPDATE STATISTICS dbo.dimCustomer (Customer_LastName_Stats);  
  
--Return the date when Customer_LastName_Stats was last updated.  
SELECT stats_id, name AS stats_name,   
    STATS_DATE(object_id, stats_id) AS statistics_date  
FROM sys.stats s  
WHERE s.object_id = OBJECT_ID('dbo.DimCustomer')  
    AND s.name = 'Customer_LastName_Stats';  
GO    
```  
  
### <a name="c-view-the-date-of-the-last-update-for-all-statistics-on-a-table"></a>C. Afficher la date de la dernière mise à jour de toutes les statistiques d’une table  
 Cet exemple retourne la date à laquelle chaque objet de statistiques sur la table DimCustomer a été mis à jour pour la dernière fois.  
  
```sql  
--Return the dates all statistics on the table were last updated.  
SELECT stats_id, name AS stats_name,   
    STATS_DATE(object_id, stats_id) AS statistics_date  
FROM sys.stats s  
WHERE s.object_id = OBJECT_ID('dbo.DimCustomer');  
GO  
```  
  
 Si les statistiques correspondent à un index, la valeur *stats_id* dans la vue de catalogue [sys.stats](../../relational-databases/system-catalog-views/sys-stats-transact-sql.md) est identique à la valeur *index_id* dans la vue de catalogue [sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md), et la requête suivante retourne les mêmes résultats que la requête précédente. Si les statistiques ne correspondent pas à un index, elles figurent dans les résultats sys.stats mais pas dans les résultats sys.indexes.  
  
```sql  
USE AdventureWorksPDW2012;  
GO  
SELECT name AS index_name,   
    STATS_DATE(object_id, index_id) AS statistics_update_date  
FROM sys.indexes   
WHERE object_id = OBJECT_ID('dbo.DimCustomer');  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions système &#40;Transact-SQL&#41;](../../relational-databases/system-functions/system-functions-category-transact-sql.md)   
 [UPDATE STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/update-statistics-transact-sql.md)   
 [sp_autostats &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-autostats-transact-sql.md)   
 [Statistiques](../../relational-databases/statistics/statistics.md)    
 [sys.dm_db_stats_properties &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-stats-properties-transact-sql.md)   
 [sys.stats](../../relational-databases/system-catalog-views/sys-stats-transact-sql.md)   
  
  

