---
description: CREATE STATISTICS (Transact-SQL)
title: CREATE STATISTICS (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/04/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- STATISTICS
- STATISTICS_TSQL
- CREATE STATISTICS
- CREATE_STATISTICS_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- query optimization statistics [SQL Server], creating
- indexed views [SQL Server], statistics
- FULLSCAN option
- CREATE STATISTICS statement
- filtered statistics [SQL Server]
- creating statistics [SQL Server]
- NORECOMPUTE clause
ms.assetid: b23e2f6b-076c-4e6d-9281-764bdb616ad2
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: d0e799f28d327204707298dc63a4fb1a8470fa2f
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170521"
---
# <a name="create-statistics-transact-sql"></a>CREATE STATISTICS (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Crée des statistiques d’optimisation de requête sur une ou plusieurs colonnes d’une table, d’une vue indexée ou d’une table externe. Pour la plupart des requêtes, l'optimiseur de requête génère déjà les statistiques utiles à un plan de requête de haute qualité ; dans certains cas, vous devez créer des statistiques supplémentaires avec CREATE STATISTICS ou modifier la conception des requêtes pour améliorer les performances des requêtes.  
  
 Pour plus d’informations, consultez [Statistiques](../../relational-databases/statistics/statistics.md).  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
-- Create statistics on an external table  
CREATE STATISTICS statistics_name   
ON { table_or_indexed_view_name } ( column [ ,...n ] )   
    [ WITH FULLSCAN ] ;  
  
-- Create statistics on a regular table or indexed view  
CREATE STATISTICS statistics_name   
ON { table_or_indexed_view_name } ( column [ ,...n ] )   
    [ WHERE <filter_predicate> ]  
    [ WITH   
        [ [ FULLSCAN   
            [ [ , ] PERSIST_SAMPLE_PERCENT = { ON | OFF } ]    
          | SAMPLE number { PERCENT | ROWS }   
            [ [ , ] PERSIST_SAMPLE_PERCENT = { ON | OFF } ]    
          | <update_stats_stream_option> [ ,...n ]    
        [ [ , ] NORECOMPUTE ]   
        [ [ , ] INCREMENTAL = { ON | OFF } ] 
        [ [ , ] MAXDOP = max_degree_of_parallelism ]
    ] ;  
  
<filter_predicate> ::=   
    <conjunct> [AND <conjunct>]  
  
<conjunct> ::=  
    <disjunct> | <comparison>  
  
<disjunct> ::=  
        column_name IN (constant ,...)  
  
<comparison> ::=  
        column_name <comparison_op> constant  
  
<comparison_op> ::=  
    IS | IS NOT | = | <> | != | > | >= | !> | < | <= | !<  
    
<update_stats_stream_option> ::=  
    [ STATS_STREAM = stats_stream ]  
    [ ROWCOUNT = numeric_constant ]  
    [ PAGECOUNT = numeric_contant ] 
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
CREATE STATISTICS statistics_name   
    ON { database_name.schema_name.table_name | schema_name.table_name | table_name }
    ( column_name  [ ,...n ] )   
    [ WHERE <filter_predicate> ]  
    [ WITH {  
           FULLSCAN   
           | SAMPLE number PERCENT   
      }  
    ]  
[;]  
  
<filter_predicate> ::=   
    <conjunct> [AND <conjunct>]  
  
<conjunct> ::=  
    <disjunct> | <comparison>  
  
<disjunct> ::=  
        column_name IN (constant ,...)  
  
<comparison> ::=  
        column_name <comparison_op> constant  
  
<comparison_op> ::=  
    IS | IS NOT | = | <> | != | > | >= | !> | < | <= | !<  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *statistics_name*  
 Nom des statistiques à créer.  
  
 *table_or_indexed_view_name*  
 Nom de la table, vue indexée ou table externe pour laquelle créer les statistiques. Pour créer des statistiques sur une autre base de données, spécifiez un nom de table qualifié.  
  
 *column [ ,...n]*  
 Une ou plusieurs colonnes à inclure dans les statistiques. Les colonnes doivent être spécifiées par ordre de priorité de gauche à droite. Seule la première colonne est utilisée pour la création de l’histogramme. Toutes les colonnes sont utilisées pour les statistiques de corrélation entre les colonnes appelées densités.  
  
 Vous pouvez indiquer comme base de calcul des statistiques toute colonne pouvant être spécifiée en tant que colonne de clé d'index, sauf pour les exceptions suivantes :  
  
-   Les colonnes **Xml**, de texte intégral et FILESTREAM ne peuvent pas être spécifiées.  
  
-   Les colonnes calculées ne peuvent être indiquées que si les paramètres de base de données ARITHABORT et QUOTED_IDENTIFIER ont la valeur ON.  
  
-   Toute colonne de type CLR définie par l'utilisateur peut être spécifiée si son type prend en charge l'ordre de tri binaire. Les colonnes calculées définies en tant qu'appels à des méthodes d'une colonne de type défini par l'utilisateur peuvent être précisées si les méthodes en question sont marquées comme étant déterministes.  
  
 WHERE \<filter_predicate> Spécifie une expression permettant de sélectionner un sous-ensemble des lignes à inclure lors de la création de l'objet de statistiques. Les statistiques créées avec un prédicat de filtre sont appelées des statistiques filtrées. Le prédicat de filtre utilise une logique de comparaison simple et ne peut pas référencer une colonne calculée, une colonne UDT, une colonne de type de données spatiales ou une colonne de type de données **hierarchyID**. Les comparaisons à l'aide de littéraux NULL ne sont pas autorisées avec les opérateurs de comparaison. Utilisez les opérateurs IS NULL et IS NOT NULL à la place.  
  
 Voici quelques exemples de prédicats de filtre pour la table Production.BillOfMaterials :  
  
 * `WHERE StartDate > '20000101' AND EndDate <= '20000630'`  
  
 * `WHERE ComponentID IN (533, 324, 753)`  
  
 * `WHERE StartDate IN ('20000404', '20000905') AND EndDate IS NOT NULL`  
  
 Pour plus d’informations sur les prédicats de filtre, consultez [Créer des index filtrés](../../relational-databases/indexes/create-filtered-indexes.md).  
  
 FULLSCAN  
 Calcule les statistiques en analysant toutes les lignes. FULLSCAN et SAMPLE 100 PERCENT ont les mêmes résultats. Cette option ne peut pas être utilisée avec l'option SAMPLE.  
  
 Si elle est omise, SQL Server utilise l’échantillonnage pour créer les statistiques, et détermine la taille d’échantillon nécessaire pour créer un plan de requête de haute qualité  
  
 SAMPLE *number* { PERCENT | ROWS }  
 Spécifie le pourcentage ou nombre de lignes approximatif dans la table ou vue indexée devant être utilisé par l'optimiseur de requête lors de la création des statistiques. Pour PERCENT, *number* peut être compris entre 0 et 100, et pour ROWS, *number* peut être compris entre 0 et le nombre total de lignes. Le pourcentage ou nombre de lignes réel échantillonné par l'optimiseur de requête peut ne pas correspondre au pourcentage ou nombre spécifié. Par exemple, l'optimiseur de requête analyse toutes les lignes d'une page de données.  
  
 SAMPLE est utile pour les cas spéciaux dans lesquels le plan de requête, basé sur l'échantillonnage par défaut, n'est pas optimal. Dans la plupart des situations, il n'est pas nécessaire de spécifier SAMPLE, car l'optimiseur de requête utilise déjà l'échantillonnage et détermine la taille d'échantillon statistiquement significative par défaut, comme requis pour créer des plans de requête de haute qualité.  
  
 SAMPLE ne peut pas être utilisé avec l'option FULLSCAN. Lorsque ni SAMPLE ni FULLSCAN n'est spécifié, l'optimiseur de requête utilise les données échantillonnées et calcule la taille d'échantillon par défaut.  
  
 Il est déconseillé de spécifier 0 PERCENT ou 0 ROWS. Lorsque 0 PERCENT ou ROWS est spécifié, l'objet de statistiques est créé mais ne contient pas de données de statistiques.  
 
 PERSIST_SAMPLE_PERCENT = { ON | OFF }  
 Si vous spécifiez **ON**, les statistiques conserveront le pourcentage d’échantillonnage de création pour les mises à jour ultérieures qui ne spécifient pas explicitement un pourcentage d’échantillonnage. Si vous spécifiez **OFF**, le pourcentage d’échantillonnage de statistiques sera réinitialisé à la valeur d’échantillonnage par défaut lors des mises à jour ultérieures qui ne spécifient pas explicitement un pourcentage d’échantillonnage. La valeur par défaut est **OFF**. 
 
 > [!NOTE]
 > Si la table est tronquée, toutes les statistiques générées sur le HoBT tronqué sont rétablies à l’aide du pourcentage d’échantillonnage par défaut.

 **S’applique à** : [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] (à partir de [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] SP1 CU4) et versions ultérieures (à partir de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU1).    
  
 STATS_STREAM **=** _stats_stream_  
 [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
 NORECOMPUTE  
 Désactive l’option de mise à jour automatique des statistiques, AUTO_UPDATE_STATISTICS, pour *statistics_name*. Si cette option est spécifiée, l’optimiseur de requête effectue les mises à jour des statistiques en cours d’exécution pour *statistics_name* et désactive les mises à jour ultérieures.  
  
 Pour réactiver la mise à jour des statistiques, supprimez les statistiques à l’aide de [DROP STATISTICS](../../t-sql/statements/drop-statistics-transact-sql.md), puis exécutez CREATE STATISTICS sans l’option NORECOMPUTE.  
  
> [!WARNING]  
> L'utilisation de cette option peut produire des plans de requête non optimaux. Nous recommandons d'utiliser cette option avec parcimonie et uniquement par un administrateur système qualifié.  
  
 Pour plus d’informations sur l’option AUTO_UPDATE_STATISTICS, consultez [Options ALTER DATABASE SET &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md). Pour plus d’informations sur la désactivation et la réactivation des mises à jour des statistiques, consultez [Statistiques](../../relational-databases/statistics/statistics.md).  
  
 INCREMENTAL = { ON | OFF }  
 Si la valeur **ON** est définie, les statistiques sont créées par partition. Si la valeur **OFF** est définie, les statistiques sont combinées pour toutes les partitions. La valeur par défaut est **OFF**.  
  
 Si les statistiques par partition ne sont pas prises en charge, une erreur est générée. Les statistiques incrémentielles ne sont pas prises en charge pour les types de statistiques suivants :  
  
-   statistiques créées avec des index qui ne sont pas alignés sur les partitions avec la table de base ;  
-   statistiques créées sur les bases de données secondaires lisibles Always On ;  
-   statistiques créées sur les bases de données en lecture seule ;  
-   statistiques créées sur les index filtrés ;  
-   statistiques créées sur les vues ;  
-   statistiques créées sur les tables internes ;  
-   Statistiques créées avec les index spatiaux ou les index XML.  
  
**S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.  
  
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
  
 \<update_stats_stream_option> [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  

## <a name="permissions"></a>Autorisations  
 Nécessite l’une de ces autorisations :  
  
-   ALTER TABLE  
-   L’utilisateur est le propriétaire de la table  
-   L’appartenance au rôle de base de données fixe **db_ddladmin**  
  
## <a name="general-remarks"></a>Remarques d'ordre général  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut utiliser tempdb pour trier les lignes échantillonnées avant de générer des statistiques.  
  
### <a name="statistics-for-external-tables"></a>Statistiques pour les tables externes  
 Lors de la création de statistiques de table externe, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] importe la table externe dans une table [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] temporaire, puis crée les statistiques. Pour les échantillons de statistiques, seules les lignes échantillonnées sont importées. Si vous avez une grande table externe, il est beaucoup plus rapide d’utiliser l’échantillonnage par défaut au lieu de l’option d’analyse complète.  
  
### <a name="statistics-with-a-filtered-condition"></a>Statistiques avec une condition filtrée  
 Les statistiques filtrées peuvent améliorer les performances des requêtes qui effectuent des sélections dans des sous-ensembles bien définis de données. Elles utilisent un prédicat de filtre dans la clause WHERE pour sélectionner le sous-ensemble de données qui est inclus dans les statistiques.  
  
### <a name="when-to-use-create-statistics"></a>Quand utiliser CREATE STATISTICS  
 Pour plus d’informations sur le moment où CREATE STATISTICS doit être utilisé, consultez [Statistiques](../../relational-databases/statistics/statistics.md).  
  
### <a name="referencing-dependencies-for-filtered-statistics"></a>Dépendances des références pour les statistiques filtrées  
 La vue de catalogue [sys.sql_expression_dependencies](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md) suit chaque colonne du prédicat de statistiques filtrées en tant que dépendance de référence. Réfléchissez aux opérations que vous effectuez sur les colonnes de table avant de créer des statistiques filtrées car vous ne pouvez pas supprimer, renommer, ni modifier la définition d'une colonne de table qui est définie dans un prédicat de statistiques filtrées.  
  
## <a name="limitations-and-restrictions"></a>Limitations et restrictions  
* La mise à jour des statistiques n’est pas prise en charge sur les tables externes. Pour mettre à jour des statistiques sur une table externe, supprimez et recréez les statistiques.  
* Vous pouvez afficher jusqu’à 64 colonnes par objet de statistiques.
* L’option MAXDOP n’est pas compatible avec les options STATS_STREAM, ROWCOUNT et PAGECOUNT.
* L’option MAXDOP est limitée par le paramètre MAX_DOP du groupe de charge de travail de Resource Governor, le cas échéant.
* L’utilisation de CREATE STATISTICS et DROP STATISTICS sur les tables externes n’est pas prise en charge dans Azure SQL Database.
  
## <a name="examples"></a>Exemples  

### <a name="examples-use-the-adventureworks-database"></a>Les exemples utilisent la base de données AdventureWorks.  

### <a name="a-using-create-statistics-with-sample-number-percent"></a>R. Utilisation de CREATE STATISTICS avec SAMPLE number PERCENT  
 L'exemple suivant crée les statistiques `ContactMail1`, à l'aide d'un exemple aléatoire de 5 pour cent des colonnes `BusinessEntityID` et `EmailPromotion` de la table `Person` de la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```sql  
CREATE STATISTICS ContactMail1  
    ON Person.Person (BusinessEntityID, EmailPromotion)  
    WITH SAMPLE 5 PERCENT;  
```  
  
### <a name="b-using-create-statistics-with-fullscan-and-norecompute"></a>B. Utilisation de CREATE STATISTICS avec FULLSCAN et NORECOMPUTE  
 L'exemple suivant crée les statistiques `NamePurchase` pour toutes les lignes des colonnes `BusinessEntityID` et `EmailPromotion` de la table `Person` et désactive le recalcul automatique des statistiques.  
  
```sql  
CREATE STATISTICS NamePurchase  
    ON AdventureWorks2012.Person.Person (BusinessEntityID, EmailPromotion)  
    WITH FULLSCAN, NORECOMPUTE;  
```  
  
### <a name="c-using-create-statistics-to-create-filtered-statistics"></a>C. Utilisation de CREATE STATISTICS pour créer des statistiques filtrées  
 L'exemple suivant crée les statistiques filtrées `ContactPromotion1`. Le [!INCLUDE[ssDE](../../includes/ssde-md.md)] échantillonne 50 pour cent des données, puis sélectionne toutes les lignes pour lesquelles `EmailPromotion` est égal à 2.  
  
```sql  
CREATE STATISTICS ContactPromotion1  
    ON Person.Person (BusinessEntityID, LastName, EmailPromotion)  
WHERE EmailPromotion = 2  
WITH SAMPLE 50 PERCENT;  
GO  
```  
  
### <a name="d-create-statistics-on-an-external-table"></a>D. Créer des statistiques sur une table externe  
 La seule décision que vous devez prendre quand vous créez des statistiques sur une table externe, en plus de fournir la liste des colonnes, est de savoir si vous allez créer les statistiques en échantillonnant les lignes ou en analysant toutes les lignes. L’utilisation de CREATE STATISTICS et DROP STATISTICS sur les tables externes n’est pas prise en charge dans Azure SQL Database.
  
 Étant donné que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] importe les données de la table externe vers une table temporaire pour créer les statistiques, l’option d’analyse complète prendra beaucoup plus de temps. Pour une table volumineuse, la méthode d’échantillonnage par défaut est généralement suffisante.  
  
```sql  
--Create statistics on an external table and use default sampling.  
CREATE STATISTICS CustomerStats1 ON DimCustomer (CustomerKey, EmailAddress);  
  
--Create statistics on an external table and scan all the rows  
CREATE STATISTICS CustomerStats1 ON DimCustomer (CustomerKey, EmailAddress) WITH FULLSCAN;  
```  

### <a name="e-using-create-statistics-with-fullscan-and-persist_sample_percent"></a>E. Utilisation de CREATE STATISTICS avec FULLSCAN et PERSIST_SAMPLE_PERCENT  
 L’exemple suivant crée les statistiques `NamePurchase` pour toutes les lignes des colonnes `BusinessEntityID` et `EmailPromotion` de la table `Person`, et définit un pourcentage d’échantillonnage égal à 100 pour toutes les mises à jour ultérieures qui ne spécifient pas explicitement un pourcentage d’échantillonnage.  
  
```sql  
CREATE STATISTICS NamePurchase  
    ON AdventureWorks2012.Person.Person (BusinessEntityID, EmailPromotion)  
    WITH FULLSCAN, PERSIST_SAMPLE_PERCENT = ON;  
```  
  
### <a name="examples-using-adventureworksdw-database"></a>Exemples avec la base de données AdventureWorksDW. 
  
### <a name="f-create-statistics-on-two-columns"></a>F. Créer des statistiques sur deux colonnes  
 L’exemple suivant crée les statistiques `CustomerStats1`, en fonction des colonnes `CustomerKey` et `EmailAddress` de la table `DimCustomer`. Les statistiques sont créées d’après un échantillon statistiquement significatif des lignes dans la table `Customer`.  
  
```sql  
CREATE STATISTICS CustomerStats1 ON DimCustomer (CustomerKey, EmailAddress);  
```  
  
### <a name="g-create-statistics-by-using-a-full-scan"></a>G. Créer des statistiques à l’aide d’une analyse complète  
 L’exemple suivant crée les statistiques `CustomerStatsFullScan`, en fonction de l’analyse de toutes les lignes dans la table `DimCustomer`.  
  
```sql  
CREATE STATISTICS CustomerStatsFullScan 
ON DimCustomer (CustomerKey, EmailAddress) WITH FULLSCAN;  
```  
  
### <a name="h-create-statistics-by-specifying-the-sample-percentage"></a>H. Créer des statistiques en spécifiant le pourcentage d’échantillonnage  
 L’exemple suivant crée les statistiques `CustomerStatsSampleScan`, en fonction de l’analyse de 50 pour cent des lignes dans la table `DimCustomer`.  
  
```sql  
CREATE STATISTICS CustomerStatsSampleScan 
ON DimCustomer (CustomerKey, EmailAddress) WITH SAMPLE 50 PERCENT;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Statistiques](../../relational-databases/statistics/statistics.md)   
 [UPDATE STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/update-statistics-transact-sql.md)   
 [sp_updatestats &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-updatestats-transact-sql.md)   
 [DBCC SHOW_STATISTICS &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-show-statistics-transact-sql.md)   
 [DROP STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/drop-statistics-transact-sql.md)   
 [sys.stats &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-stats-transact-sql.md)   
 [sys.stats_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-stats-columns-transact-sql.md)  
  
  

