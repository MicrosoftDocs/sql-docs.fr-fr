---
description: UPDATE STATISTICS (Transact-SQL)
title: UPDATE STATISTICS (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/04/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- UPDATE STATISTICS
- UPDATE_STATISTICS_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- updating statistics
- query optimization statistics [SQL Server], updating
- UPDATE STATISTICS statement
- statistical information [SQL Server], updating
ms.assetid: 919158f2-38d0-4f68-82ab-e1633bd0d308
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 3680ba07e290f8b531ab46576f76bd3ffeee3460
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170441"
---
# <a name="update-statistics-transact-sql"></a>UPDATE STATISTICS (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Met à jour les statistiques d'optimisation de requête d'une table ou d'une vue indexée. Par défaut, l’optimiseur de requête met à jour les statistiques en fonction des besoins afin d’améliorer le plan de requête. Dans certains cas, vous pouvez optimiser les performances des requêtes en utilisant `UPDATE STATISTICS` ou la procédure stockée [sp_updatestats](../../relational-databases/system-stored-procedures/sp-updatestats-transact-sql.md) pour mettre à jour les statistiques plus fréquemment qu’avec les mises à jour par défaut.  
  
La mise à jour des statistiques est l'assurance que les requêtes sont compilées avec des statistiques à jour. Toutefois, la mise à jour des statistiques entraîne une recompilation des requêtes. À ce titre, il est déconseillé de mettre à jour les statistiques de façon trop régulière eu égard aux performances. Un compromis doit être trouvé entre le souhait d'améliorer les plans de requête et le temps nécessaire à la recompilation des requêtes. Ce compromis peut varier en fonction de votre application. `UPDATE STATISTICS` peut utiliser tempdb pour trier l’échantillon de lignes à des fins statistiques.  
  
![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
UPDATE STATISTICS table_or_indexed_view_name   
    [   
        {   
            { index_or_statistics__name }  
          | ( { index_or_statistics_name } [ ,...n ] )   
                }  
    ]   
    [    WITH   
        [  
            FULLSCAN   
              [ [ , ] PERSIST_SAMPLE_PERCENT = { ON | OFF } ]    
            | SAMPLE number { PERCENT | ROWS }   
              [ [ , ] PERSIST_SAMPLE_PERCENT = { ON | OFF } ]    
            | RESAMPLE   
              [ ON PARTITIONS ( { <partition_number> | <range> } [, ...n] ) ]  
            | <update_stats_stream_option> [ ,...n ]  
        ]   
        [ [ , ] [ ALL | COLUMNS | INDEX ]   
        [ [ , ] NORECOMPUTE ]   
        [ [ , ] INCREMENTAL = { ON | OFF } ] 
        [ [ , ] MAXDOP = max_degree_of_parallelism ] 
    ] ;  
  
<update_stats_stream_option> ::=  
    [ STATS_STREAM = stats_stream ]  
    [ ROWCOUNT = numeric_constant ]  
    [ PAGECOUNT = numeric_contant ]  
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
UPDATE STATISTICS [ schema_name . ] table_name   
    [ ( { statistics_name | index_name } ) ]  
    [ WITH   
       {  
              FULLSCAN   
            | SAMPLE number PERCENT   
            | RESAMPLE   
        }  
    ]  
[;]  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

## <a name="arguments"></a>Arguments
 *table_or_indexed_view_name*  
 Nom de la table ou de la vue indexée qui contient l’objet de statistique.  
  
 *index_or_statistics_name*  
 Nom de l'index dont les statistiques doivent être mises à jour ou nom des statistiques à mettre à jour. Si *index_or_statistics_name* n’est pas spécifié, l’optimiseur de requête met à jour toutes les statistiques pour la table ou vue indexée. Cela inclut les statistiques créées à l'aide de l'instruction CREATE STATISTICS, les statistiques de colonnes uniques créées lorsque AUTO_CREATE_STATISTICS a la valeur ON, ainsi que les statistiques créées pour les index.  
  
 Pour plus d’informations sur l’option AUTO_CREATE_STATISTICS, consultez [Options ALTER DATABASE SET &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md). Pour afficher tous les index d’une table ou d’une vue, vous pouvez utiliser [sp_helpindex](../../relational-databases/system-stored-procedures/sp-helpindex-transact-sql.md).  
  
 FULLSCAN  
 Calcule les statistiques en analysant toutes les lignes dans la table ou vue indexée. FULLSCAN et SAMPLE 100 PERCENT ont les mêmes résultats. Cette option ne peut pas être utilisée avec l'option SAMPLE.  
  
 SAMPLE *number* { PERCENT | ROWS }  
 Spécifie le pourcentage ou nombre de lignes approximatif dans la table ou vue indexée à utiliser par l'optimiseur de requête lors de la mise à jour des statistiques. Pour PERCENT, *number* peut être compris entre 0 et 100, et pour ROWS, *number* peut être compris entre 0 et le nombre total de lignes. Le pourcentage ou nombre de lignes réel échantillonné par l'optimiseur de requête peut ne pas correspondre au pourcentage ou nombre spécifié. Par exemple, l'optimiseur de requête analyse toutes les lignes d'une page de données.  
  
 SAMPLE est utile pour les cas spéciaux dans lesquels le plan de requête, basé sur l'échantillonnage par défaut, n'est pas optimal. Dans la plupart des situations, il n'est pas nécessaire de spécifier SAMPLE car l'optimiseur de requête utilise l'échantillonnage et détermine la taille d'échantillon statistiquement significative par défaut, comme requis pour créer des plans de requête de haute qualité. 
 
À partir de [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)], l’échantillonnage des données pour générer des statistiques est effectué en parallèle, lors de l’utilisation du niveau de compatibilité 130, afin d’améliorer les performances de collecte des statistiques. L’optimiseur de requête utilise des échantillons de statistiques parallèles chaque fois que la taille d’une table dépasse un certain seuil. 
   
 SAMPLE ne peut pas être utilisé avec l'option FULLSCAN. Lorsque ni SAMPLE ni FULLSCAN n'est spécifié, l'optimiseur de requête utilise les données échantillonnées et calcule la taille d'échantillon par défaut.  
  
 Il est déconseillé de spécifier 0 PERCENT ou 0 ROWS. Lorsque 0 PERCENT ou ROWS est spécifié, l'objet de statistiques est mis à jour mais ne contient pas de données de statistiques.  
  
 Pour la plupart des charges de travail, une analyse complète n’est pas nécessaire, et l’échantillonnage par défaut suffit.  
Toutefois, certaines charges de travail qui sont sensibles aux distributions de données à grande variation peuvent nécessiter une taille d’échantillon plus élevée, voire une analyse complète.  
Pour plus d’informations, consultez le [blog CSS SQL Escalation Services](/archive/blogs/psssql/sampling-can-produce-less-accurate-statistics-if-the-data-is-not-evenly-distributed).  
  
 RESAMPLE  
 Met à jour chaque statistique à l'aide de son taux d'échantillonnage le plus récent.  
  
 L'utilisation de RESAMPLE peut entraîner une analyse complète de la table. Par exemple, les statistiques relatives aux index utilisent une analyse de table complète pour leur taux d'échantillonnage. Si aucune option d'échantillonnage (SAMPLE, FULLSCAN, RESAMPLE) n'est spécifiée, l'optimiseur de requête échantillonne les données et calcule la taille d'échantillon par défaut.  

PERSIST_SAMPLE_PERCENT = { ON | OFF }  
Si vous spécifiez **ON**, les statistiques conserveront le pourcentage d’échantillonnage défini pour les mises à jour ultérieures qui ne spécifient pas explicitement un pourcentage d’échantillonnage. Si vous spécifiez **OFF**, le pourcentage d’échantillonnage de statistiques sera réinitialisé à la valeur d’échantillonnage par défaut lors des mises à jour ultérieures qui ne spécifient pas explicitement un pourcentage d’échantillonnage. La valeur par défaut est **OFF**. 
 
 > [!NOTE]
 > Si l’option AUTO_UPDATE_STATISTICS est exécutée, elle utilise le pourcentage d’échantillonnage persistant s’il est disponible, ou le pourcentage d’échantillonnage par défaut dans le cas contraire.
 > Le comportement de RESAMPLE n’est pas affecté par cette option.
 
 > [!NOTE]
 > Si la table est tronquée, toutes les statistiques générées sur le HoBT tronqué sont rétablies à l’aide du pourcentage d’échantillonnage par défaut.
 
 > [!TIP] 
 > [DBCC SHOW_STATISTICS](../../t-sql/database-console-commands/dbcc-show-statistics-transact-sql.md) et [sys.dm_db_stats_properties](../../relational-databases/system-dynamic-management-views/sys-dm-db-stats-properties-transact-sql.md) exposent la valeur de pourcentage d’échantillonnage persistante pour la statistique sélectionnée.
 
 **S’applique à** : [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] (à partir de [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] SP1 CU4) et versions ultérieures (à partir de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU1).  
 
 ON PARTITIONS ( { \<partition_number> | \<range> } [, ...n] ) ] Impose le recalcul des statistiques au niveau feuille couvrant les partitions spécifiées dans la clause ON PARTITIONS, puis leur fusion afin de générer des statistiques globales. WITH RESAMPLE est nécessaire car les statistiques de partitions créées avec différents taux d'échantillonnage ne peuvent pas être fusionnées ensemble.  
  
**S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et ultérieur
  
 ALL | COLUMNS | INDEX  
 Met à jour toutes les statistiques existantes, les statistiques créées sur une ou plusieurs colonnes, ou les statistiques créées pour les index. Si aucune option n'est spécifiée, l'instruction UPDATE STATISTICS met à jour toutes les statistiques de la table ou vue indexée.  
  
 NORECOMPUTE  
 Désactive l'option de mise à jour automatique des statistiques, AUTO_UPDATE_STATISTICS, pour les statistiques spécifiées. Si cette option est spécifiée, l'optimiseur de requête effectue cette mise à jour des statistiques et désactive les mises à jour ultérieures.  
  
 Pour réactiver le comportement de l’option AUTO_UPDATE_STATISTICS, réexécutez UPDATE STATISTICS sans l’option NORECOMPUTE ou exécutez **sp_autostats**.  
  
> [!WARNING]  
> L'utilisation de cette option peut produire des plans de requête non optimaux. Nous recommandons d'utiliser cette option avec parcimonie et uniquement par un administrateur système qualifié.  
  
 Pour plus d’informations sur l’option AUTO_UPDATE_STATISTICS, consultez [Options ALTER DATABASE SET &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md).  
  
 INCREMENTAL = { ON | OFF }  
 Quand la valeur **ON** est définie, les statistiques sont recréées par partition. Si la valeur est **OFF**, l’arborescence des statistiques est supprimée et [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] recalcule les statistiques. La valeur par défaut est **OFF**.  
  
 Si les statistiques par partition ne sont pas prises en charge, une erreur est générée. Les statistiques incrémentielles ne sont pas prises en charge pour les types de statistiques suivants :  
  
-   statistiques créées avec des index qui ne sont pas alignés sur les partitions avec la table de base ;  
-   statistiques créées sur les bases de données secondaires lisibles Always On ;  
-   statistiques créées sur les bases de données en lecture seule ;  
-   statistiques créées sur les index filtrés ;  
-   statistiques créées sur les vues ;  
-   statistiques créées sur les tables internes ;  
-   Statistiques créées avec les index spatiaux ou les index XML.  
  
**S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et ultérieur

MAXDOP = *max_degree_of_parallelism*  
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] SP2 et [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3).  
  
 Remplace l’option de configuration **max degree of parallelism** pendant la durée de l’opération statistique. Pour plus d’informations, consultez [Configurer l’option de configuration du serveur max degree of parallelism](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md). Utilisez MAXDOP pour limiter le nombre de processeurs utilisés dans une exécution de plan parallèle. Le nombre maximal de processeurs est égal à 64.  
  
 *max_degree_of_parallelism* peut avoir la valeur :  
  
 1  
 Supprime la création de plans parallèles.  
  
 \>1  
 Limite le nombre maximal de processeurs utilisés dans une opération statistique parallèle au nombre défini ou à un nombre inférieur en fonction de la charge de travail actuelle du système.  
  
 0 (valeur par défaut)  
 Utilise le nombre réel de processeurs ou un nombre de processeurs inférieur en fonction de la charge de travail actuelle du système.  
  
 \<update_stats_stream_option> 
 
 [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  

## <a name="remarks"></a>Notes  
  
### <a name="when-to-use-update-statistics"></a>Quand utiliser UPDATE STATISTICS  
 Pour plus d’informations sur les cas où utiliser `UPDATE STATISTICS`, consultez [Statistiques](../../relational-databases/statistics/statistics.md).  

### <a name="limitations-and-restrictions"></a>Limitations et restrictions  
* La mise à jour des statistiques n’est pas prise en charge sur les tables externes. Pour mettre à jour des statistiques sur une table externe, supprimez et recréez les statistiques.  
* L’option `MAXDOP` n’est pas compatible avec les options `STATS_STREAM`, `ROWCOUNT` et `PAGECOUNT`.
* L’option `MAXDOP` est limitée par le paramètre `MAX_DOP` du groupe de charge de travail de Resource Governor, s’il est utilisé.

### <a name="updating-all-statistics-with-sp_updatestats"></a>Mise à jour de toutes les statistiques avec sp_updatestats  
Pour plus d’informations sur la mise à jour des statistiques pour toutes les tables définies par l’utilisateur et les tables internes de la base de données, consultez la procédure stockée [sp_updatestats &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-updatestats-transact-sql.md). Par exemple, la commande suivante appelle sp_updatestats pour mettre à jour toutes les statistiques de la base de données.  
  
```sql  
EXEC sp_updatestats;  
```  

### <a name="automatic-index-and-statistics-management"></a>Gestion automatique des index et des statistiques
Tirez parti de solutions comme [Adaptive Index Defrag](https://github.com/Microsoft/tigertoolbox/tree/master/AdaptiveIndexDefrag) pour gérer automatiquement la défragmentation des index et les mises à jour des statistiques pour une ou plusieurs bases de données. Cette procédure choisit automatiquement s’il faut reconstruire ou réorganiser un index en fonction de son niveau de fragmentation, entre autres, et mettre à jour les statistiques avec un seuil linéaire.
  
### <a name="determining-the-last-statistics-update"></a>Détermination de la dernière mise à jour des statistiques  
 Pour déterminer la date de la dernière mise à jour des statistiques, utilisez la fonction [STATS_DATE](../../t-sql/functions/stats-date-transact-sql.md) .  
  
### <a name="pdw--azure-synapse-analytics"></a>PDW/Azure Synapse Analytics  
 La syntaxe suivante n’est pas prise en charge par [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] / [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]  
  
```sql
UPDATE STATISTICS t1 (a,b);   
```  
  
```sql  
UPDATE STATISTICS t1 (a) WITH SAMPLE 10 ROWS;  
```  
  
```sql  
UPDATE STATISTICS t1 (a) WITH NORECOMPUTE;  
```  
  
```sql  
UPDATE STATISTICS t1 (a) WITH INCREMENTAL = ON;  
```  
  
```sql  
UPDATE STATISTICS t1 (a) WITH stats_stream = 0x01;  
```  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l’autorisation `ALTER` sur la table ou la vue.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-update-all-statistics-on-a-table"></a>R. Mettre à jour toutes les statistiques d'une table  
 L’exemple suivant met à jour les statistiques de tous les index de la table `SalesOrderDetail`.  
  
```sql  
USE AdventureWorks2012;  
GO  
UPDATE STATISTICS Sales.SalesOrderDetail;  
GO  
```  
  
### <a name="b-update-the-statistics-for-an-index"></a>B. Mettre à jour les statistiques d'un index  
 L'exemple suivant illustre la mise à jour des statistiques pour l'index `AK_SalesOrderDetail_rowguid` de la table `SalesOrderDetail`.  
  
```sql  
USE AdventureWorks2012;  
GO  
UPDATE STATISTICS Sales.SalesOrderDetail AK_SalesOrderDetail_rowguid;  
GO  
```  
  
### <a name="c-update-statistics-by-using-50-percent-sampling"></a>C. Mettre à jour des statistiques avec un échantillonnage de 50 pour cent  
 L'exemple suivant crée, puis met à jour les statistiques des colonnes `Name` et `ProductNumber` de la table `Product`.  
  
```sql  
USE AdventureWorks2012;  
GO  
CREATE STATISTICS Products  
    ON Production.Product ([Name], ProductNumber)  
    WITH SAMPLE 50 PERCENT  
-- Time passes. The UPDATE STATISTICS statement is then executed.  
UPDATE STATISTICS Production.Product(Products)   
    WITH SAMPLE 50 PERCENT;  
```  
  
### <a name="d-update-statistics-by-using-fullscan-and-norecompute"></a>D. Mettre à jour des statistiques avec FULLSCAN et NORECOMPUTE  
 L'exemple suivant met à jour les statistiques de `Products` dans la table `Product`, force l'analyse complète de toutes les lignes de la table `Product` et désactive la mise à jour automatique des statistiques pour les statistiques de `Products`.  
  
```sql  
USE AdventureWorks2012;  
GO  
UPDATE STATISTICS Production.Product(Products)  
    WITH FULLSCAN, NORECOMPUTE;  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Exemples : [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="e-update-statistics-on-a-table"></a>E. Mettre à jour les statistiques d’une table  
 L’exemple suivant met à jour les statistiques `CustomerStats1` de la table `Customer`.  
  
```sql  
UPDATE STATISTICS Customer ( CustomerStats1 );  
```  
  
### <a name="f-update-statistics-by-using-a-full-scan"></a>F. Mettre à jour des statistiques à l’aide d’une analyse complète  
 L’exemple suivant met à jour les statistiques `CustomerStats1`, en fonction de l’analyse de toutes les lignes dans la table `Customer`.  
  
```sql  
UPDATE STATISTICS Customer (CustomerStats1) WITH FULLSCAN;  
```  
  
### <a name="g-update-all-statistics-on-a-table"></a>G. Mettre à jour toutes les statistiques d'une table  
 L’exemple suivant met à jour toutes les statistiques de la table `Customer`.  
  
```sql  
UPDATE STATISTICS Customer;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Statistiques](../../relational-databases/statistics/statistics.md)   
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [CREATE STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/create-statistics-transact-sql.md)   
 [DBCC SHOW_STATISTICS &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-show-statistics-transact-sql.md)   
 [DROP STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/drop-statistics-transact-sql.md)   
 [sp_autostats &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-autostats-transact-sql.md)   
 [sp_updatestats &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-updatestats-transact-sql.md)   
 [STATS_DATE &#40;Transact-SQL&#41;](../../t-sql/functions/stats-date-transact-sql.md)  
 [sys.dm_db_stats_properties &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-stats-properties-transact-sql.md)    
 [sys.dm_db_stats_histogram &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-stats-histogram-transact-sql.md)