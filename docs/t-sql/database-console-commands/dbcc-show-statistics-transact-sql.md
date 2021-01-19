---
description: DBCC SHOW_STATISTICS (Transact-SQL)
title: DBCC SHOW_STATISTICS (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 12/18/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- SHOW_STATISTICS_TSQL
- DBCC SHOW_STATISTICS
- SHOW_STATISTICS
- DBCC_SHOW_STATISTICS_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- query optimization statistics [SQL Server], densities
- histograms [SQL Server]
- statistical information [SQL Server], viewing statistics objects
- current distribution statistics
- query optimization statistics [SQL Server], histograms
- DBCC SHOW_STATISTICS statement
- distribution statistics
- statistical information [SQL Server], densities
- statistics objects
- statistical information [SQL Server], histograms
- query optimization statistics [SQL Server], viewing statistics objects
- statistical information [SQL Server], distribution
- densities [SQL Server]
- displaying distribution statistics
ms.assetid: 12be2923-7289-4150-b497-f17e76a50b2e
author: pmasl
ms.author: umajay
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 710a6211b588ab0dd4745b41af1f330c81241294
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170501"
---
# <a name="dbcc-show_statistics-transact-sql"></a>DBCC SHOW_STATISTICS (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

DBCC SHOW_STATISTICS affiche les statistiques d'optimisation de la requête actuelle pour une table ou une vue indexée. L’optimiseur de requête utilise des statiques pour estimer la cardinalité ou le nombre de lignes dans le résultat de la requête, ce qui lui permet de créer un plan de requête de haute qualité. Par exemple, l'optimiseur de requête peut utiliser des estimations de cardinalité afin de choisir l'opérateur de recherche d'index plutôt que l'opérateur d'analyse d'index dans le plan de requête. Cela permettrait d'améliorer les performances des requêtes car le recours à une analyse d'index monopolisant de nombreuses ressources serait ainsi évité.
  
L'optimiseur de requête stocke des statistiques pour une table ou vue indexée dans un objet des statistiques. Pour une table, l'objet des statistiques est créé sur un index ou sur une liste de colonnes de table. L'objet des statistiques inclut un en-tête contenant des métadonnées sur les statistiques, un histogramme indiquant la distribution des valeurs dans la première colonne clé de l'objet des statistiques, et un vecteur de densité destiné à mesurer la corrélation entre les colonnes. Le [!INCLUDE[ssDE](../../includes/ssde-md.md)] peut calculer des estimations de cardinalité avec n’importe laquelle des données de l’objet des statistiques. Pour plus d’informations, consultez [Statistics](../../relational-databases/statistics/statistics.md) et [Estimation de la cardinalité (SQL Server)](../../relational-databases/performance/cardinality-estimation-sql-server.md).
  
DBCC SHOW_STATISTICS affiche l'en-tête, l'histogramme et le vecteur de densité en fonction des données stockées dans l'objet des statistiques. La syntaxe vous permet de spécifier une table ou une vue indexée ainsi qu’un nom d'index cible, un nom de statistique ou un nom de colonne. Cette rubrique explique comment afficher les statistiques et comment comprendre les résultats affichés.

> [!IMPORTANT]
> À compter de SP1 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], la vue de gestion dynamique [sys.dm_db_stats_properties](../../relational-databases/system-dynamic-management-views/sys-dm-db-stats-properties-transact-sql.md) est disponible pour récupérer par programme les informations d’en-tête contenues dans l’objet de statistiques pour les statistiques non incrémentielles.

> [!IMPORTANT]
> À compter de SP2 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et de SP1 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], la vue de gestion dynamique [sys.dm_db_stats_properties](../../relational-databases/system-dynamic-management-views/sys-dm-db-incremental-stats-properties-transact-sql.md) est disponible pour récupérer par programme les informations d’en-tête contenues dans l’objet de statistiques pour les statistiques non incrémentielles.

> [!IMPORTANT]
> À compter de SP1 CU2 [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)], la vue de gestion dynamique [sys.dm_db_stats_properties](../../relational-databases/system-dynamic-management-views/sys-dm-db-stats-histogram-transact-sql.md) est disponible pour récupérer par programme les informations d’histogramme contenues dans l’objet de statistiques.

![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntaxe

```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
DBCC SHOW_STATISTICS ( table_or_indexed_view_name , target )   
[ WITH [ NO_INFOMSGS ] < option > [ , n ] ]  
< option > :: =  
    STAT_HEADER | DENSITY_VECTOR | HISTOGRAM | STATS_STREAM  
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  

DBCC SHOW_STATISTICS ( table_name , target )   
    [ WITH { STAT_HEADER | DENSITY_VECTOR | HISTOGRAM } [ ,...n ] ]  
[;]
```

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments

 *table_or_indexed_view_name*  
 Nom de la table ou de la vue indexée dont les informations statistiques doivent être affichées.  
  
 *table_name*  
 Nom de la table qui contient les statistiques à afficher. La table ne peut pas être une table externe.  
  
 *cible*  
 Nom de l'index, de la statistique ou de la colonne dont les informations statistiques doivent être affichées. *target* est placé entre crochets, guillemets simples, guillemets doubles, ou se présente tel quel. Si *target* est le nom d’une statistique ou d’un index existant dans une table ou une vue indexée, les informations statistiques concernant cette cible sont retournées. Si *target* est le nom d’une colonne existante et que celle-ci contient une statistique créée automatiquement, les informations sur cette dernière sont retournées. S'il n'existe pas de statistique créée automatiquement pour une cible de colonne, le message d'erreur 2767 est retourné.  
 Dans [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)], *target* ne peut pas être un nom de colonne.  
  
 NO_INFOMSGS  
 Supprime tous les messages d'information dont les niveaux de gravité sont compris entre 0 et 10.  
  
 STAT_HEADER \| DENSITY_VECTOR \| HISTOGRAM \| STATS_STREAM [ **,** _n_ ] La spécification d’une ou de plusieurs de ces options limite les jeux de résultats retournés par l’instruction aux options spécifiées. Si aucune option n'est spécifiée, toutes les informations statistiques sont retournées.  

 STATS_STREAM est [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
## <a name="result-sets"></a>Jeux de résultats

Le tableau suivant décrit les colonnes retournées dans le jeu de résultats lorsque STAT_HEADER est spécifié.
  
|Nom de la colonne|Description|  
|-----------------|-----------------|  
|Nom|Nom de l'objet de statistiques.|  
|Mis à jour|Date et heure de la dernière mise à jour des statistiques. La fonction [STATS_DATE](../../t-sql/functions/stats-date-transact-sql.md) offre une autre manière de récupérer ces informations. Pour plus d’informations, consultez la section [Notes](#Remarks) dans cette page.|  
|Lignes|Nombre total de lignes dans la table ou la vue indexée au moment de la dernière mise à jour des statistiques. Si les statistiques sont filtrées ou correspondent à un index filtré, le nombre de lignes peut être inférieur à celui de la table. Pour plus d’informations, consultez [Statistiques](../../relational-databases/statistics/statistics.md).|  
|Lignes échantillonnées|Nombre total de lignes échantillonnées pour le calcul des statistiques. Si Rows Sampled < Rows, l'histogramme et les résultats de densité affichés sont des estimations basées sur les lignes échantillonnées.|  
|Étapes|Nombre d'étapes dans l'histogramme. Chaque étape couvre une plage de valeurs de colonnes suivie d'une valeur de colonne de limite supérieure. Les étapes d'histogramme sont définies sur la première colonne clé des statistiques. Le nombre maximal d'étapes est 200.|  
|Densité|La formule 1 / *valeurs distinctes* est utilisée pour toutes les valeurs de la première colonne clé de l’objet de statistiques, à l’exception des valeurs limites de l’histogramme. Cette valeur de Densité n'est pas utilisée par l'optimiseur de requête ; elle est affichée pour la compatibilité descendante avec les versions antérieures à [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)].|  
|Longueur moyenne d'une clé|Nombre moyen d'octets par valeur pour toutes les colonnes clés de l'objet de statistiques.|  
|String Index|La valeur Yes indique que l'objet de statistiques contient des statistiques de résumé de chaîne pour améliorer les estimations de cardinalité des prédicats de requête qui utilisent l'opérateur LIKE ; c'est le cas par exemple de `WHERE ProductName LIKE '%Bike'`. Les statistiques de résumé de chaîne sont stockées à l’écart de l’histogramme et créées sur la première colonne clé de l’objet des statistiques quand il est de type **char**, **varchar**, **nchar**, **nvarchar**, **varchar(max)** , **nvarchar(max)** , **text** ou **ntext**.|  
|Expression de filtre|Prédicat pour le sous-ensemble des lignes de table incluses dans l'objet de statistiques. NULL = statistiques non filtrées. Pour plus d’informations sur les prédicats filtrés, consultez [Créer des index filtrés](../../relational-databases/indexes/create-filtered-indexes.md). Pour plus d’informations sur les statistiques filtrées, consultez [Statistiques](../../relational-databases/statistics/statistics.md).|  
|Lignes non filtrées|Nombre total de lignes dans la table avant l'application de l'expression de filtre. Si Expression de filtre a la valeur NULL, Lignes non filtrées est égal à Lignes.|  
|Pourcentage d’échantillon persistant|Pourcentage d’échantillon persistant utilisé pour les mises à jour des statistiques qui ne spécifient pas explicitement un pourcentage d’échantillonnage. Si la valeur est zéro, aucun pourcentage d’échantillon persistant n’est défini pour cette statistique.<br /><br /> **S’applique à :** [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] SP1 CU4| 
  
Le tableau suivant décrit les colonnes retournées dans le jeu de résultats lorsque DENSITY_VECTOR est spécifié.
  
|Nom de la colonne|Description|  
|-----------------|-----------------|  
|Toutes les densités|La densité est calculée selon la formule 1 / *valeurs distinctes*. Les résultats affichent la densité pour chaque préfixe des colonnes de l'objet de statistiques, à raison d'une ligne par densité. Une valeur distincte est une liste distincte des valeurs de colonnes par ligne et par préfixe de colonne. Par exemple, si l'objet de statistiques contient des colonnes clés (A, B, C), les résultats rapportent la densité des listes distinctes de valeurs dans chacun des préfixes de colonnes suivants : (A), (A,B) et (A, B, C). Avec le préfixe (A, B, C), chacune des listes suivantes est une liste de valeurs distincte : (3, 5, 6), (4, 4, 6), (4, 5, 6), (4, 5, 7). Avec le préfixe (A, B) les listes de valeurs distinctes suivantes sont associées aux mêmes valeurs de colonnes : (3, 5), (4, 4) et (4, 5)|  
|Longueur moyenne|Longueur moyenne, en octets, pour le stockage d'une liste des valeurs de colonnes pour le préfixe de colonne. Par exemple, si les valeurs dans la liste (3, 5, 6) nécessitent 4 octets chacune, la longueur est égale à 12 octets.|  
|Colonnes|Noms des colonnes dans le préfixe dont les valeurs Toutes les densités et Longueur moyenne sont affichées.|  
  
Le tableau suivant décrit les colonnes retournées dans le jeu de résultats lorsque l'option HISTOGRAM est spécifiée.
  
|Nom de la colonne|Description|  
|---|---|
|RANGE_HI_KEY|Valeur de colonne de limite supérieure pour une étape d'histogramme. La valeur de colonne est également appelée « valeur de clé ».|  
|RANGE_ROWS|Nombre estimé de lignes dont la valeur de colonne est comprise dans une étape d'histogramme, à l'exception de la limite supérieure.|  
|EQ_ROWS|Nombre estimé de lignes dont la valeur de colonne est égale à la limite supérieure de l'étape d'histogramme.|  
|DISTINCT_RANGE_ROWS|Nombre estimé de lignes ayant une valeur de colonne distincte dans une étape d'histogramme, à l'exception de la limite supérieure.|  
|AVG_RANGE_ROWS|Nombre moyen de lignes ayant des valeurs de colonnes dupliquées dans une étape d’histogramme, à l’exception de la limite supérieure. Quand DISTINCT_RANGE_ROWS est supérieur à 0, AVG_RANGE_ROWS est calculé en divisant RANGE_ROWS par DISTINCT_RANGE_ROWS. Quand DISTINCT_RANGE_ROWS est 0, AVG_RANGE_ROWS retourne 1 pour l’étape d’histogramme.| 
  
## <a name="remarks"></a><a name="Remarks"></a> Notes 

La date de mise à jour des statistiques est stockée dans l’[objet blob de statistiques](../../relational-databases/statistics/statistics.md#DefinitionQOStatistics) avec l’[histogramme](#histogram) et le [vecteur de densité](#density), et non dans les métadonnées. Quand aucune donnée n’est lue pour générer des données de statistiques, l’objet blob des statistiques n’est pas créé, la date n’est pas disponible et la colonne *mise à jour* a la valeur Null. C’est le cas pour les statistiques filtrées pour lesquelles le prédicat ne renvoie aucune ligne, ou pour les nouvelles tables vides.
  
## <a name="histogram"></a><a name="histogram"></a> Histogramme

Un histogramme mesure la fréquence des occurrences de chaque valeur distincte dans un jeu de données. L'optimiseur de requête calcule un histogramme sur les valeurs de colonnes de la première colonne clé de l'objet de statistiques, en sélectionnant les valeurs de colonnes au moyen d'un échantillonnage statistique des lignes ou d'une analyse complète de toutes les lignes dans la table ou la vue. Si l'histogramme est créé à partir d'un jeu de lignes échantillonnées, les totaux stockés pour le nombre de lignes et le nombre de valeurs distinctes sont des estimations et ne doivent pas nécessairement être des nombres entiers.
  
Pour créer l'histogramme, l'optimiseur de requête trie les valeurs de colonnes, calcule le nombre de valeurs qui correspondent à chaque valeur de colonne distincte, puis regroupe les valeurs de colonnes dans 200 étapes d'histogramme contiguës au maximum. Chaque étape inclut une plage de valeurs de colonnes suivie d'une valeur de colonne de limite supérieure. La plage comprend toutes les valeurs de colonnes possibles entre des valeurs limites, à l'exception des valeurs limites elles-mêmes. La plus basse des valeurs de colonnes triées est la valeur de limite supérieure pour la première étape d'histogramme.
  
Le diagramme suivant illustre un histogramme avec six étapes : La zone située à gauche de la première valeur limite supérieure représente la première étape.
  
![Histogramme](../../relational-databases/system-dynamic-management-views/media/a0ce6714-01f4-4943-a083-8cbd2d6f617a.gif "a0ce6714-01f4-4943-a083-8cbd2d6f617a")
  
Pour chaque étape d'histogramme :
-   La ligne en gras représente la valeur limite supérieure (RANGE_HI_KEY) et le nombre d'occurrences (EQ_ROWS) correspondant.  
-   La zone pleine située à gauche de RANGE_HI_KEY représente la plage de valeurs de colonnes et le nombre moyen d'occurrences de chacune des valeurs de colonnes (AVG_RANGE_ROWS). La valeur AVG_RANGE_ROWS pour la première étape d'histogramme est toujours 0.  
-   Les lignes pointillées représentent les valeurs échantillonnées utilisées pour estimer le nombre total de valeurs distinctes dans la plage (DISTINCT_RANGE_ROWS) et le nombre total de valeurs dans la plage (RANGE_ROWS). L'optimiseur de requête utilise les valeurs RANGE_ROWS et DISTINCT_RANGE_ROWS pour calculer la valeur AVG_RANGE_ROWS ; il ne stocke pas les valeurs échantillonnées.  
  
L'optimiseur de requête définit les étapes d'histogramme en fonction de leur importance statistique. Il utilise un algorithme de nombre maximal de différences pour réduire le nombre d'étapes dans l'histogramme tout en augmentant la différence entre les valeurs limites. Le nombre maximal d'étapes est 200. Le nombre d'étapes d'histogramme peut être inférieur au nombre de valeurs distinctes, même pour les colonnes comportant moins de 200 points de limite. Par exemple, une colonne avec 100 valeurs distinctes peut avoir un histogramme comportant moins de 100 points de limite.
  
## <a name="density-vector"></a><a name="density"></a> Vecteur de densité  
L'optimiseur de requête utilise des densités afin d'améliorer les estimations de cardinalité pour les requêtes qui retournent plusieurs colonnes à partir de la même table ou vue indexée. Le vecteur de densité contient une densité pour chaque préfixe des colonnes dans l'objet de statistiques. Par exemple, si un objet de statistiques contient les colonnes clés `CustomerId`, `ItemId` et `Price`, la densité est calculée à partir des préfixes de colonnes suivants :
  
|Préfixe de colonne|Densité calculée sur|  
|---|---|
|(CustomerId)|Lignes avec des valeurs correspondantes pour CustomerID|  
|(CustomerId, ItemId)|Lignes avec des valeurs correspondantes pour CustomerId et ItemId|  
|(CustomerId, ItemId, Price)|Lignes avec des valeurs correspondantes pour CustomerId, ItemId et Price|  
  
## <a name="restrictions"></a>Restrictions  
 DBCC SHOW_STATISTICS ne fournit pas de statistiques pour les index spatiaux ni columnstore à mémoire optimisée.  
  
## <a name="permissions-for-ssnoversion-and-sssds"></a>Autorisations pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et [!INCLUDE[ssSDS](../../includes/sssds-md.md)]  
Pour afficher l’objet de statistiques, l’utilisateur doit disposer de l’autorisation `SELECT` sur la table.
Remarquez que les conditions suivantes doivent être remplies pour que les autorisations SELECT permettent d'exécuter la commande :
-   Les utilisateurs doivent posséder des autorisations sur toutes les colonnes dans l'objet de statistiques.  
-   Les utilisateurs doivent posséder une autorisation sur toutes les colonnes dans une condition de filtre (le cas échéant).  
-   La table ne peut pas avoir une stratégie de sécurité de niveau ligne.
-   Si l’une des colonnes d’un objet de statistiques est masquée par des règles Dynamic Data Masking, l’utilisateur doit disposer, en plus de l’autorisation `SELECT`, de l’autorisation `UNMASK`.

Dans les versions antérieures à [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP1, l’utilisateur doit être propriétaire de la table ou être membre du rôle serveur fixe `sysadmin`, du rôle de base de données fixe `db_owner` ou du rôle de base de données fixe `db_ddladmin`.

 > [!NOTE]
 > Pour rétablir le comportement antérieur à [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP1, utilisez l’indicateur de trace 9485.
  
## <a name="permissions-for-sssdw-and-sspdw"></a>Autorisations pour [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
DBCC SHOW_STATISTICS exige l’autorisation `SELECT` sur la table ou l’appartenance au rôle serveur fixe `sysadmin`, au rôle de base de données fixe `db_owner` ou au rôle de base de données fixe `db_ddladmin`.  
  
## <a name="limitations-and-restrictions-for-sssdw-and-sspdw"></a>Limitations et restrictions pour [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
DBCC SHOW_STATISTICS affiche les statistiques stockées dans la base de données shell au niveau du nœud de contrôle. Il n’affiche pas les statistiques créées automatiquement par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur les nœuds de calcul.
  
DBCC SHOW_STATISTICS n’est pas pris en charge sur les tables externes.
  
## <a name="examples-ssnoversion-and-sssds"></a>Exemples : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et [!INCLUDE[ssSDS](../../includes/sssds-md.md)]  
### <a name="a-returning-all-statistics-information"></a>R. Retour de toutes les informations statistiques  
L’exemple suivant affiche toutes les informations statistiques relatives à l’index `AK_Address_rowguid` de la table `Person.Address` dans la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].
  
```sql
DBCC SHOW_STATISTICS ("Person.Address", AK_Address_rowguid);  
GO  
```  
  
### <a name="b-specifying-the-histogram-option"></a>B. Utilisation de l'option HISTOGRAM  
Cela limite les informations statistiques affichées pour Customer_LastName aux données HISTOGRAM.
  
```sql
DBCC SHOW_STATISTICS ("dbo.DimCustomer",Customer_LastName) WITH HISTOGRAM;  
GO  
```  
  
## <a name="examples-sssdw-and-sspdw"></a>Exemples : [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
### <a name="c-display-the-contents-of-one-statistics-object"></a>C. Afficher le contenu d’un objet des statistiques  
 L’exemple suivant affiche le contenu des statistiques Customer_LastName sur la table DimCustomer.  
  
```sql
-- Uses AdventureWorks  
--First, create a statistics object  
CREATE STATISTICS Customer_LastName   
ON AdventureWorksPDW2012.dbo.DimCustomer (LastName);  
GO  
DBCC SHOW_STATISTICS ("dbo.DimCustomer",Customer_LastName);  
GO  
```  
  
Les résultats indiquent l’en-tête, le vecteur de densité et une partie de l’histogramme.
  
![DBCC SHOW_STATISTICS (résultats)](../../t-sql/database-console-commands/media/aps-sql-dbccshow-statistics.JPG "DBCC SHOW_STATISTICS (résultats)")
  
## <a name="see-also"></a>Voir aussi  
[Statistiques](../../relational-databases/statistics/statistics.md)  
[CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md)  
[CREATE STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/create-statistics-transact-sql.md)  
[DROP STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/drop-statistics-transact-sql.md)  
[sp_autostats &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-autostats-transact-sql.md)  
[sp_createstats &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-createstats-transact-sql.md)  
[STATS_DATE &#40;Transact-SQL&#41;](../../t-sql/functions/stats-date-transact-sql.md)  
[UPDATE STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/update-statistics-transact-sql.md)  
[sys.dm_db_stats_properties (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-db-stats-properties-transact-sql.md)  
[sys.dm_db_stats_histogram (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-db-stats-histogram-transact-sql.md)   
[sys.dm_db_incremental_stats_properties (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-db-incremental-stats-properties-transact-sql.md)   
