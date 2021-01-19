---
description: Indicateurs (Transact-SQL) - Requête
title: Indicateurs de requête (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/27/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- Query_Hint_TSQL
- Query_TSQL
- Query
- Query Hint
- MAX_GRANT_PERCENT
- MAX_GRANT_PERCENT_TSQL
- MIN_GRANT_PERCENT
- MIN_GRANT_PERCENT_TSQL
- EXTERNALPUSHDOWN
- EXTERNALPUSHDOWN_TSQL
- NOLOCK_TSQL
- MAXDOP_TSQL
- USE_HINT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- REPORT PLAN query hint
- FORCE ORDER query hint
- HASH JOIN query hint
- query hints [SQL Server]
- OPTIMIZE FOR query hint
- FORCESCAN query hint
- RECOMPILE query hint
- MAXRECURSION query hint
- MERGE JOIN query hint
- HASH GROUP query hint
- KEEP PLAN query hint
- UNION query hint
- FORCESEEK query hint
- ORDER GROUP query hint
- LOOP JOIN query hint
- KEEPFIXED PLAN query hint
- FAST query hint
- MAXDOP query hint
- PARAMETERIZATION query hint
- hints [SQL Server], query
- JOIN query hint
- USE PLAN query hint
- EXPAND VIEWS query hint
- MAX_GRANT_PERCENT query hint
- MIN_GRANT_PERCENT query hint
- EXTERNALPUSHDOWN query hint
- USE HINT query hint
- QUERY_PLAN_PROFILE query hint
ms.assetid: 66fb1520-dcdf-4aab-9ff1-7de8f79e5b2d
author: pmasl
ms.author: pelopes
ms.openlocfilehash: 8f884c24acbb4499cbab2e7837367ec49e0c7cbb
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/13/2021
ms.locfileid: "98172321"
---
# <a name="hints-transact-sql---query"></a>Indicateurs (Transact-SQL) - Requête
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Les indicateurs de requête spécifient que les indicateurs mentionnés sont utilisés dans l’étendue d’une requête. Ils s’appliquent à tous les opérateurs de l’instruction. Si une clause UNION se trouve dans la requête principale, seule la dernière requête impliquant une opération UNION peut avoir la clause OPTION. Les indicateurs de requête sont spécifiés dans la [clause OPTION](../../t-sql/queries/option-clause-transact-sql.md). L’erreur 8622 se produit si un ou plusieurs indicateurs de requête empêchent l’optimiseur de requête de générer un plan valide.  
  
> [!CAUTION]  
> Comme l’optimiseur de requête [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sélectionne généralement le meilleur plan d’exécution pour une requête, nous recommandons de ne recourir aux indicateurs qu’en dernier ressort, et à condition d’être un développeur ou un administrateur de base de données expérimenté.  
  
**S’applique à :**  
  
[DELETE](../../t-sql/statements/delete-transact-sql.md)  
  
[INSERT](../../t-sql/statements/insert-transact-sql.md)  
  
[SELECT](../../t-sql/queries/select-transact-sql.md)  
  
[UPDATE](../../t-sql/queries/update-transact-sql.md)  
  
[MERGE](../../t-sql/statements/merge-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
<query_hint> ::=   
{ { HASH | ORDER } GROUP   
  | { CONCAT | HASH | MERGE } UNION   
  | { LOOP | MERGE | HASH } JOIN   
  | EXPAND VIEWS   
  | FAST <integer_value>   
  | FORCE ORDER   
  | { FORCE | DISABLE } EXTERNALPUSHDOWN
  | { FORCE | DISABLE } SCALEOUTEXECUTION
  | IGNORE_NONCLUSTERED_COLUMNSTORE_INDEX  
  | KEEP PLAN   
  | KEEPFIXED PLAN  
  | MAX_GRANT_PERCENT = <numeric_value>  
  | MIN_GRANT_PERCENT = <numeric_value>  
  | MAXDOP <integer_value>   
  | MAXRECURSION <integer_value>   
  | NO_PERFORMANCE_SPOOL   
  | OPTIMIZE FOR ( @variable_name { UNKNOWN | = <literal_constant> } [ , ...n ] )  
  | OPTIMIZE FOR UNKNOWN  
  | PARAMETERIZATION { SIMPLE | FORCED }   
  | QUERYTRACEON <integer_value>   
  | RECOMPILE  
  | ROBUST PLAN   
  | USE HINT ( <use_hint_name> [ , ...n ] )
  | USE PLAN N'<xml_plan>'  
  | TABLE HINT ( <exposed_object_name> [ , <table_hint> [ [, ]...n ] ] )  
}  
  
<table_hint> ::=  
{ NOEXPAND [ , INDEX ( <index_value> [ ,...n ] ) | INDEX = ( <index_value> ) ]  
  | INDEX ( <index_value> [ ,...n ] ) | INDEX = ( <index_value> )
  | FORCESEEK [ ( <index_value> ( <index_column_name> [,... ] ) ) ]  
  | FORCESCAN  
  | HOLDLOCK   
  | NOLOCK   
  | NOWAIT  
  | PAGLOCK   
  | READCOMMITTED   
  | READCOMMITTEDLOCK   
  | READPAST   
  | READUNCOMMITTED   
  | REPEATABLEREAD   
  | ROWLOCK   
  | SERIALIZABLE   
  | SNAPSHOT  
  | SPATIAL_WINDOW_MAX_CELLS = <integer_value>  
  | TABLOCK   
  | TABLOCKX   
  | UPDLOCK   
  | XLOCK  
}  

<use_hint_name> ::=
{ 'ASSUME_JOIN_PREDICATE_DEPENDS_ON_FILTERS'
  | 'ASSUME_MIN_SELECTIVITY_FOR_FILTER_ESTIMATES'
  | 'DISABLE_BATCH_MODE_ADAPTIVE_JOINS'
  | 'DISABLE_BATCH_MODE_MEMORY_GRANT_FEEDBACK'
  | 'DISABLE_DEFERRED_COMPILATION_TV'
  | 'DISABLE_INTERLEAVED_EXECUTION_TVF'
  | 'DISABLE_OPTIMIZED_NESTED_LOOP'
  | 'DISABLE_OPTIMIZER_ROWGOAL'
  | 'DISABLE_PARAMETER_SNIFFING'
  | 'DISABLE_ROW_MODE_MEMORY_GRANT_FEEDBACK'
  | 'DISABLE_TSQL_SCALAR_UDF_INLINING'
  | 'DISALLOW_BATCH_MODE'
  | 'ENABLE_HIST_AMENDMENT_FOR_ASC_KEYS'
  | 'ENABLE_QUERY_OPTIMIZER_HOTFIXES'
  | 'FORCE_DEFAULT_CARDINALITY_ESTIMATION'
  | 'FORCE_LEGACY_CARDINALITY_ESTIMATION'
  | 'QUERY_OPTIMIZER_COMPATIBILITY_LEVEL_n'
  | 'QUERY_PLAN_PROFILE' 
}
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
{ HASH | ORDER } GROUP  
Indique que les agrégations décrites par la clause GROUP BY ou DISTINCT de la requête doivent utiliser le hachage ou le tri.  
  
{ MERGE | HASH | CONCAT } UNION  
Indique que toutes les opérations UNION doivent être exécutées par fusion, hachage ou concaténation d'ensembles UNION. Si plusieurs indicateurs UNION sont spécifiés, l’optimiseur de requête sélectionne la stratégie la moins coûteuse parmi les indicateurs spécifiés.  
  
{ LOOP | MERGE | HASH } JOIN  
Indique que toutes les opérations de jointure doivent être effectuées par LOOP JOIN, MERGE JOIN ou HASH JOIN dans toute la requête. Si plusieurs indicateurs de jointure sont spécifiés, l'optimiseur sélectionne la stratégie la moins coûteuse parmi celles qui sont autorisées.  
  
Si vous spécifiez un indicateur de jointure dans la clause FROM de la même requête pour une paire de tables spécifique, il est prioritaire dans la jointure des deux tables. Les indicateurs de requête, toutefois, doivent quand même être respectés. L'indicateur de jointure de la paire de tables peut seulement restreindre la sélection des méthodes de jointure autorisées dans l'indicateur de requête. Pour plus d’informations, consultez [Indicateurs de jointure &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-join.md).  
  
EXPAND VIEWS  
Indique que les vues indexées doivent être développées, et que l’optimiseur de requête ne doit pas prendre en compte les vues indexées en remplacement d’une partie de la requête. Une vue est développée lorsque sa définition remplace son nom dans le texte de la requête.  
  
Cet indicateur de requête interdit virtuellement l'utilisation directe de vues indexées et d'index sur des vues indexées dans le plan de requête.  
  
> [!NOTE]
> La vue indexée reste condensée si une référence directe y est faite dans la partie SELECT de la requête, ou si WITH (NOEXPAND) ou WITH (NOEXPAND, INDEX( _<index\_value>_ [ **,** _...n_ ] ) ) est spécifié. Pour plus d’informations sur l’indicateur de requête NOEXPAND, consultez [Utilisation de NOEXPAND](../../t-sql/queries/hints-transact-sql-table.md#using-noexpand).  
  
L'indicateur n’a d’incidence que sur les vues de la partie SELECT des instructions, y compris celles figurant dans des instructions INSERT, UPDATE, MERGE et DELETE.  
  
FAST _<integer\_value>_  
Spécifie que la requête est optimisée pour la récupération rapide des _<integer\_value>_ premières lignes. Ce résultat est un entier non négatif. Une fois que les premières lignes définies par _<integer\_value_ ont été retournées, la requête se poursuit pour retourner un jeu de résultats complet.  
  
FORCE ORDER  
Spécifie que l'ordre de jointure spécifié dans la syntaxe de la requête est conservé au cours de l'optimisation de la requête. FORCE ORDER n’a aucun effet sur un éventuel comportement d’inversion des rôles de la part de l’optimiseur de requête.  
  
> [!NOTE]  
> Dans une instruction MERGE, il convient d'accéder à la table source avant la table cible comme ordre de jointure par défaut, à moins que la clause WHEN SOURCE NOT MATCHED ne soit spécifiée. La spécification de FORCE ORDER préserve ce comportement par défaut.  
  
{ FORCE | DISABLE } EXTERNALPUSHDOWN  
Force ou désactive la poussée vers le bas (pushdown) du calcul des expressions éligibles dans Hadoop. S’applique uniquement aux requêtes avec PolyBase. Ne s’applique pas au stockage Azure.  

{FORCE | DISABLE} SCALEOUTEXECUTION Force ou désactive l’exécution d’un scale-out des requêtes PolyBase qui utilisent des tables externes dans les Clusters Big Data SQL Server 2019. Cet indicateur n’est respecté que par une requête utilisant l’instance principale d’un cluster Big Data SQL. Le scale-out se produit dans le pool de calcul du cluster Big Data. 

KEEP PLAN  
Force l’optimiseur de requête à abaisser le seuil de recompilation estimé pour une requête. Le seuil de recompilation estimé lance une recompilation automatique de la requête lorsque le nombre estimé de modifications de colonnes indexées a été apporté à une table par exécution de l’une des instructions suivantes :

* UPDATE
* Suppression
* MERGE
* INSERT

KEEP PLAN permet de garantir qu'une requête n'est pas recompilée aussi fréquemment lorsque plusieurs mises à jour sont effectuées dans une table.  
  
KEEPFIXED PLAN  
Force l’optimiseur de requête à ne pas recompiler une requête en raison de modifications enregistrées au niveau des statistiques. KEEPFIXED PLAN permet de garantir qu’une requête n’est recompilée que si le schéma des tables sous-jacentes est modifié ou si **sp_recompile** s’exécute sur ces tables.  
  
IGNORE_NONCLUSTERED_COLUMNSTORE_INDEX       
**S’applique à :** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]).  
  
Empêche la requête d’utiliser un index columnstore non-cluster à mémoire optimisée. Si la requête contient l’indicateur de requête pour éviter l’utilisation de l’index columnstore et un indicateur d’index pour utiliser un index columnstore, les indicateurs sont en conflit et la requête retourne une erreur.  
  
MAX_GRANT_PERCENT = <numeric_value>     
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)]) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].  

Taille maximale d’allocation de mémoire en pourcentage de la limite de mémoire configurée. La requête ne peut pas dépasser cette limite. La limite réelle peut être inférieure si le paramètre de Resource Governor est inférieur à la valeur spécifiée par cet indicateur. Les valeurs valides sont comprises entre 0,0 et 100,0.  
  
MIN_GRANT_PERCENT = <numeric_value>        
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)]) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].   

Taille minimale d’allocation de mémoire en pourcentage de la limite de mémoire configurée. La requête est assurée d’avoir au moins `MAX(required memory, min grant)`, car la mémoire nécessaire est le minimum requis pour pouvoir démarrer une requête. Les valeurs valides sont comprises entre 0,0 et 100,0.  
 
MAXDOP <integer_value>      
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].  
  
Remplace l’option de configuration **Degré maximal de parallélisme** de **sp_configure**. Remplace également Resource Governor pour la requête spécifiant cette option. L'indicateur de requête MAXDOP peut dépasser la valeur configurée avec sp_configure. Si MAXDOP dépasse la valeur configurée avec Resource Governor, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] utilise la valeur MAXDOP de Resource Governor, décrite dans [ALTER WORKLOAD GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/alter-workload-group-transact-sql.md). Toutes les règles sémantiques utilisées avec l’option de configuration **max degree of parallelism** sont applicables quand vous utilisez l’indicateur de requête MAXDOP. Pour plus d’informations, consultez [Configurer l’option de configuration du serveur max degree of parallelism](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md).  
  
> [!WARNING]     
> Si MAXDOP a la valeur zéro, le serveur choisit le degré maximal de parallélisme.  
  
MAXRECURSION <integer_value>     
Spécifie le nombre maximal de récursivités autorisé pour cette requête. _number_ est un entier non négatif compris entre 0 et 32 767. Lorsque 0 est spécifié, aucune limite n'est appliquée. Si cette option n'est pas spécifiée, la limite par défaut du serveur est 100.  
  
Lorsque la limite par défaut ou spécifiée de MAXRECURSION est atteinte au cours de l'exécution d'une requête, cette requête se termine en retournant une erreur.  
  
À cause de cette erreur, tous les effets de l'instruction sont annulés. S'il s'agit d'une instruction SELECT, les résultats retournés sont partiels ou aucun résultat n'est retourné. Il se peut que parmi les résultats partiels éventuellement retournés ne figurent pas toutes les lignes des niveaux de récursivité supérieurs au niveau de récursivité maximal spécifié.  
  
Pour plus d’informations, consultez [WITH common_table_expression &#40;Transact-SQL&#41;](../../t-sql/queries/with-common-table-expression-transact-sql.md).     
  
NO_PERFORMANCE_SPOOL    
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)]) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].   
  
Empêche l’ajout d’un opérateur de spool aux plans de requête (à l’exception des plans où un spool est nécessaire pour garantir la validité de la sémantique de mise à jour). L’opérateur de spool est susceptible de diminuer les performances dans certains scénarios. Par exemple, du fait que le spool utilise tempdb, une contention de tempdb peut se produire quand un grand nombre de requêtes simultanées sont exécutées avec les opérations de spool.  
  
OPTIMIZE FOR ( _\@variable\_name_ { UNKNOWN | = <literal_constant> }_ [ **,** ..._n_ ] )     
Indique à l’optimiseur de requête d’attribuer à une variable locale une valeur déterminée lors de la compilation et de l’optimisation de la requête. Cette valeur n'est utilisée que pendant l'optimisation de la requête, et non pas lors de son exécution.  
  
_\@nom\_variable_  
Nom d'une variable locale utilisée dans une requête, à laquelle une valeur peut être attribuée pour être utilisée avec l'indicateur de requête OPTIMIZE FOR.  
  
_UNKNOWN_  
Spécifie que l’optimiseur de requête utilise des données statistiques à la place de la valeur initiale pour déterminer la valeur d’une variable locale pendant l’optimisation de requête.  
  
_constante\_littérale_  
Valeur de constante littérale à assigner à _\@nom\_variable_ pour l’utiliser avec l’indicateur de requête OPTIMIZE FOR. _constante\_littérale_ n’est utilisé que pendant l’optimisation de la requête, et non comme valeur de _\@nom\_variable_ lors de l’exécution de la requête. _constante\_littérale_ peut être de n’importe quel type de données système [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui peut être exprimé sous forme de constante littérale. Le type de données de _constante\_littérale_ doit être implicitement convertible dans le type de données auquel _\@nom\_variable_ fait référence dans la requête.  
  
OPTIMIZE FOR peut contrecarrer le comportement de détection des paramètres par défaut de l’optimiseur. Utilisez également OPTIMIZE FOR pour créer des repères de plan. Pour plus d’informations, consultez [Recompiler une procédure stockée](../../relational-databases/stored-procedures/recompile-a-stored-procedure.md).  
  
OPTIMIZE FOR UNKNOWN  
Indique à l’optimiseur de requête d’utiliser la sélectivité moyenne du prédicat sur toutes les valeurs de colonne au lieu d’utiliser la valeur du paramètre de runtime lorsque la requête est compilée et optimisée.  
  
Si vous utilisez OPTIMIZE FOR @variable_name = _constante\_littérale_ et OPTIMIZE FOR UNKNOWN dans le même indicateur de requête, l’optimiseur de requête utilise la _constante\_littérale_ spécifiée pour une valeur spécifique, et UNKNOWN pour les autres valeurs des variables. Les valeurs ne sont utilisées que pendant l'optimisation de la requête, et non pas lors de son exécution.  

PARAMETERIZATION { SIMPLE | FORCED }     
Spécifie les règles de paramétrage que l’optimiseur de requête [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doit appliquer à la requête lors de sa compilation.  
  
> [!IMPORTANT]  
> L’indicateur de requête PARAMETERIZATION peut uniquement être spécifié à l’intérieur d’un repère de plan pour remplacer le paramètre actuel de l’option SET de base de données PARAMETERIZATION. Il n’est pas possible de le spécifier directement dans une requête.    
> Pour plus d’informations, consultez [Spécifier le comportement du paramétrage de requêtes grâce aux repères de plan](../../relational-databases/performance/specify-query-parameterization-behavior-by-using-plan-guides.md).
  
SIMPLE indique à l’optimiseur de requête de tenter un paramétrage simple. FORCED indique à l’optimiseur de requête de tenter un paramétrage forcé. Pour plus d’informations, consultez [Paramétrage forcé dans le Guide d’architecture de traitement des requêtes](../../relational-databases/query-processing-architecture-guide.md#ForcedParam) et [Paramétrage simple dans le Guide d’architecture de traitement des requêtes](../../relational-databases/query-processing-architecture-guide.md#SimpleParam).  

QUERYTRACEON <integer_value>    
Cette option vous permet d’activer un indicateur de trace affectant le plan uniquement pendant la compilation d’une requête unique. Comme d’autres options de niveau requête, vous pouvez l’utiliser avec des repères de plan pour faire correspondre le texte d’une requête en cours d’exécution à partir de n’importe quelle session, et appliquer automatiquement un indicateur de trace affectant le plan lorsque cette requête est en cours de compilation. L’option QUERYTRACEON est prise en charge seulement pour les indicateurs de trace de l’optimiseur de requête. Pour plus d’informations, consultez l’article [Indicateurs de trace](../database-console-commands/dbcc-traceon-trace-flags-transact-sql.md). 

> [!NOTE]  
> L’utilisation de cette option ne retourne pas d’erreur ni d’avertissement si un numéro d’indicateur de trace non pris en charge est utilisé. Si l’indicateur de trace spécifié n’est pas celui qui affecte un plan d’exécution de requête, l’option est ignorée en mode silencieux.

> [!NOTE]  
> Pour utiliser plusieurs indicateurs de trace dans une requête, spécifiez un indicateur QUERYTRACEON pour chaque numéro d’indicateur de trace.

RECOMPILE  
Envoie à [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] l’instruction de générer un nouveau plan temporaire pour la requête et de l’abandonner juste après la fin d’exécution de la requête. Le plan de requête généré ne remplace pas un plan stocké en cache lorsque la même requête s’exécute sans l’indicateur RECOMPILE. Si RECOMPILE n’est pas spécifié, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] met en cache les plans de requête et les réutilise. Lors de la compilation de plans de requête, l’indicateur de requête RECOMPILE utilise les valeurs actuelles des éventuelles variables locales dans la requête. Si la requête se trouve à l’intérieur d’une procédure stockée, les valeurs actuelles sont transmises aux paramètres.  
  
RECOMPILE est utile pour ne pas avoir à créer une procédure stockée. RECOMPILE utilise la clause WITH RECOMPILE lorsqu'il s'agit seulement de recompiler un sous-ensemble de requêtes dans la procédure stockée, et non la totalité de celle-ci. Pour plus d’informations, consultez [Recompiler une procédure stockée](../../relational-databases/stored-procedures/recompile-a-stored-procedure.md). RECOMPILE s'avère également utile pour créer des repères de guides.  
  
ROBUST PLAN  
Force l’optimiseur de requête à essayer un plan capable de prendre en charge la taille maximale potentielle des lignes, éventuellement aux dépens des performances. Lors du traitement de la requête, les tables et les opérateurs intermédiaires peuvent avoir à stocker et à traiter des lignes plus grandes que n'importe quelle ligne d'entrée. Elles sont parfois si grandes que l'opérateur particulier ne peut pas les traiter. Dans ce cas, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] génère une erreur lors de l'exécution de la requête. ROBUST PLAN permet d’indiquer à l’optimiseur de requête de ne considérer aucun plan de requête susceptible de présenter ce problème.  
  
Si un tel plan n’est pas possible, l’optimiseur de requête retourne une erreur plutôt que de différer la détection de l’erreur au moment de l’exécution de la requête. Les lignes peuvent contenir des colonnes de longueur variable. Le [!INCLUDE[ssDE](../../includes/ssde-md.md)] permet de définir des lignes d’une taille maximale potentielle que le [!INCLUDE[ssDE](../../includes/ssde-md.md)] n’est pas en mesure de traiter. En règle générale, en dépit de la taille maximale potentielle, une application stocke des lignes dont la taille réelle est comprise dans les limites gérées par le [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Si [!INCLUDE[ssDE](../../includes/ssde-md.md)] rencontre une ligne trop longue, il retourne une erreur d'exécution.  
 
<a name="use_hint"></a> USE HINT ( **'** _nom\_indicateur_ **'** )    
 **S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (depuis [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] SP1) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].
 
Fournit au processeur de requêtes un ou plusieurs indicateurs supplémentaires, spécifiés par un nom **entre guillemets simples**.   

Les noms d’indicateur suivants sont pris en charge :    
 
*  'ASSUME_JOIN_PREDICATE_DEPENDS_ON_FILTERS' <a name="use_hint_join_containment"></a>       
   Indique à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de générer un plan de requête qui utilise l’hypothèse de relation contenant-contenu simple, au lieu de l’hypothèse par défaut de relation contenant-contenu de base pour les jointures, avec le modèle [d’estimation de la cardinalité](../../relational-databases/performance/cardinality-estimation-sql-server.md) de l’optimiseur de requête fourni dans [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] ou ultérieur. Ce nom d’indicateur équivaut à l’[indicateur de trace](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) 9476. 
*  'ASSUME_MIN_SELECTIVITY_FOR_FILTER_ESTIMATES' <a name="use_hint_correlation"></a>      
   Indique à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de générer un plan qui utilise la sélectivité minimale lors de l’évaluation des prédicats AND des filtres pour la prise en compte de la corrélation. Ce nom d’indicateur équivaut à l’[indicateur de trace](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) 4137 avec le modèle d’estimation de la cardinalité fourni dans [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et les versions antérieures, ou à l’[indicateur de trace](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) 9471 avec le modèle d’estimation de la cardinalité disponible dans [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] ou versions ultérieures.
*  'DISABLE_BATCH_MODE_ADAPTIVE_JOINS'       
   Désactive les jointures adaptatives en mode batch. Pour plus d’informations, consultez [Jointures adaptatives en mode batch](../../relational-databases/performance/intelligent-query-processing.md#batch-mode-adaptive-joins).     
   **S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].   
*  'DISABLE_BATCH_MODE_MEMORY_GRANT_FEEDBACK'       
   Désactive les retours d’allocation de mémoire en mode batch. Pour plus d’informations, consultez [Retour d’allocation de mémoire en mode batch](../../relational-databases/performance/intelligent-query-processing.md#batch-mode-memory-grant-feedback).     
   **S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].   
* 'DISABLE_DEFERRED_COMPILATION_TV'    
  Désactive la compilation différée de variable de table. Pour plus d'informations, consultez [Compilation différée de variable de table](../../relational-databases/performance/intelligent-query-processing.md#table-variable-deferred-compilation).     
  **S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)]) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].   
*  'DISABLE_INTERLEAVED_EXECUTION_TVF'      
   Désactive l’exécution entrelacée pour les fonctions table à instructions multiples. Pour plus d’informations, voir [Exécution entrelacée pour les fonctions table à instructions multiples](../../relational-databases/performance/intelligent-query-processing.md#interleaved-execution-for-mstvfs).     
   **S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].   
*  'DISABLE_OPTIMIZED_NESTED_LOOP'      
   Indique au processeur de requêtes de ne pas appliquer d’opération de tri (tri par lots) sur les jointures de boucles imbriquées optimisées au moment de la génération d’un plan de requête. Ce nom d’indicateur équivaut à l’[indicateur de trace](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) 2340.
*  'DISABLE_OPTIMIZER_ROWGOAL' <a name="use_hint_rowgoal"></a>      
   Indique à SQL Server de générer un plan qui n’utilise pas les modifications de l’objectif des lignes avec des requêtes contenant ces mots clés : 
   
   * Haut de la page
   * OPTION (FAST N)
   * IN
   * EXISTS
   
   Ce nom d’indicateur équivaut à l’[indicateur de trace](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) 4138.
*  'DISABLE_PARAMETER_SNIFFING'      
   Indique à l’optimiseur de requête d’utiliser la distribution moyenne des données lors de la compilation d’une requête comportant un ou plusieurs paramètres. Cette instruction rend le plan de requête indépendant de la valeur du paramètre utilisée initialement lors de la compilation de la requête. Ce nom d’indicateur équivaut à l’[indicateur de trace](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) 4136 ou au paramètre de [configuration au niveau base de données](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md)`PARAMETER_SNIFFING = OFF`.
* 'DISABLE_ROW_MODE_MEMORY_GRANT_FEEDBACK'    
  Désactive la rétroaction d’allocation de mémoire en mode ligne. Pour plus d’informations, consultez [Rétroaction d’allocation de mémoire en mode ligne](../../relational-databases/performance/intelligent-query-processing.md#row-mode-memory-grant-feedback).      
  **S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)]) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].     
* 'DISABLE_TSQL_SCALAR_UDF_INLINING'    
  Désactive l’incorporation des fonctions UDF scalaires. Pour plus d’informations, consultez [Incorporation des fonctions UDF scalaires](../../relational-databases/user-defined-functions/scalar-udf-inlining.md).     
  **S’applique à :** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)]).    
* 'DISALLOW_BATCH_MODE'    
  Désactive l’exécution en mode batch. Pour plus d’informations, consultez [Modes d’exécution](../../relational-databases/query-processing-architecture-guide.md#execution-modes).     
  **S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)]) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].     
*  'ENABLE_HIST_AMENDMENT_FOR_ASC_KEYS'      
   Active automatiquement la génération de statistiques rapides (modification de l’histogramme) pour les colonnes d’index de début où l’estimation de la cardinalité est nécessaire. L’histogramme utilisé pour estimer la cardinalité est ajusté au moment de la compilation des requêtes pour prendre en compte la valeur minimale ou maximale réelle de chaque colonne. Ce nom d’indicateur équivaut à l’[indicateur de trace](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) 4139. 
*  'ENABLE_QUERY_OPTIMIZER_HOTFIXES'     
   Active les correctifs de l’optimiseur de requête (modifications publiées dans les Service Packs et mises à jour cumulatives de SQL Server). Ce nom d’indicateur équivaut à l’[indicateur de trace](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) 4199 ou au paramètre de [configuration au niveau base de données](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md)`QUERY_OPTIMIZER_HOTFIXES = ON`.
*  'FORCE_DEFAULT_CARDINALITY_ESTIMATION'      
   Force l’optimiseur de requête à utiliser le modèle [d’estimation de la cardinalité](../../relational-databases/performance/cardinality-estimation-sql-server.md) qui correspond au niveau de compatibilité de la base de données. Cet indicateur remplace le paramètre de [configuration au niveau base de données](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md)`LEGACY_CARDINALITY_ESTIMATION = ON` ou l’[indicateur de trace](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) 9481.
*  'FORCE_LEGACY_CARDINALITY_ESTIMATION' <a name="use_hint_ce70"></a>      
   Force l’optimiseur de requête à utiliser le modèle [Estimation de cardinalité](../../relational-databases/performance/cardinality-estimation-sql-server.md) fourni dans [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et les versions antérieures. Ce nom d’indicateur équivaut à l’[indicateur de trace](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) 9481 ou au paramètre de [configuration au niveau base de données](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md)`LEGACY_CARDINALITY_ESTIMATION = ON`.
*  'QUERY_OPTIMIZER_COMPATIBILITY_LEVEL_n'          
 Force le comportement de l’optimiseur de requête au niveau de la requête, comme si celle-ci était compilée avec le niveau de compatibilité de la base de données _n_, où _n_ est un niveau pris en charge (par exemple 100, 130, etc.). Consultez [sys.dm_exec_valid_use_hints](../../relational-databases/system-dynamic-management-views/sys-dm-exec-valid-use-hints-transact-sql.md) pour obtenir la liste des valeurs actuellement prises en charge pour _n_.      
   **S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à partir de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU10).    

   > [!NOTE]
   > L’indicateur QUERY_OPTIMIZER_COMPATIBILITY_LEVEL_n ne remplace pas le paramètre d’estimation de la cardinalité hérité ou par défaut, s’il est forcé par le biais de la configuration de portée de base de données, l’indicateur de trace ou un autre indicateur de requête comme QUERYTRACEON.   
   > Cet indicateur affecte uniquement le comportement de l’optimiseur de requête. Il n’a aucun effet sur les autres fonctionnalités de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] susceptibles de dépendre du [niveau de compatibilité de la base de données](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md), comme la disponibilité de certaines fonctionnalités de la base de données.  
   > Pour en savoir plus sur cet indicateur, consultez [Developer’s Choice: Hinting Query Execution model](/archive/blogs/sql_server_team/developers-choice-hinting-query-execution-model).
    
*  'QUERY_PLAN_PROFILE'      
 Permet un profilage léger pour la requête. À la fin d’une requête contenant ce nouvel indicateur, un nouvel événement étendu, query_plan_profile, est déclenché. Cet événement étendu expose les statistiques d’exécution et le plan d’exécution réel XML semblable à l’événement étendu query_post_execution_showplan, mais uniquement pour les requêtes qui contiennent le nouvel indicateur.    
   **S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à partir de [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] SP2CU3 et [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU11). 

   > [!NOTE]
   > Si vous activez la collecte de l’événement étendu query_post_execution_showplan, cette opération ajoutera l’infrastructure de profilage standard à chaque requête qui est en cours d’exécution sur le serveur et pourra, par conséquent, affecter les performances globales du serveur.      
   > Si vous activez la collection de l’événement étendu *query_thread_profile* pour utiliser l’infrastructure à profilage léger à la place, la performance sera beaucoup plus faible, mais les performances globales du serveur seront quand même affectées.       
   > Si vous activez l’événement étendu query_plan_profile, cela activera uniquement l’infrastructure de profilage léger pour une requête exécutée avec le QUERY_PLAN_PROFILE et, par conséquent, n’affectera pas d’autres charges de travail sur le serveur. Utilisez cet indicateur pour profiler une requête spécifique sans affecter d’autres parties de la charge de travail du serveur.
   > Pour en savoir plus sur le profilage léger, consultez [Infrastructure du profilage de requête](../../relational-databases/performance/query-profiling-infrastructure.md).
 
Vous pouvez obtenir la liste de tous les noms d’indicateur USE HINT pris en charge en effectuant une requête sur la vue de gestion dynamique [sys.dm_exec_valid_use_hints](../../relational-databases/system-dynamic-management-views/sys-dm-exec-valid-use-hints-transact-sql.md).    

> [!TIP]
> Les noms d’indicateur respectent la casse.   
  
> [!IMPORTANT] 
> Certains indicateurs USE HINT peuvent être en conflit avec des indicateurs de trace activés au niveau global ou session, ou avec des paramètres de configuration au niveau base de données. Dans ce cas, l’indicateur de niveau requête (USE HINT) est toujours prioritaire. En présence d’un conflit entre l’indicateur USE HINT et un autre indicateur de requête ou un indicateur de trace activé au niveau requête (par exemple, par QUERYTRACEON), [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] génère une erreur quand vous tentez d’exécuter la requête. 

<a name="use-plan"></a> USE PLAN N' _<xml\_plan>_ '  
 Force l’optimiseur de requête à utiliser un plan de requête existant pour une requête spécifiée par **’** _xml\_plan_ **’** . USE PLAN ne peut pas être spécifié avec les instructions INSERT, UPDATE, MERGE ou DELETE.  
  
TABLE HINT **(** _<exposed\_object\_name>_ [ **,** \<table_hint> [ [ **,** ]..._n_ ] ] **)** Applique l’indicateur de table spécifié à la table ou à la vue correspondant à _exposed\_object\_name_. Nous vous recommandons d’utiliser un indicateur de table comme indicateur de requête uniquement dans le contexte d’un [repère de plan](../../relational-databases/performance/plan-guides.md).  
  
 _<exposed\_object\_name>_ peut être l’une des références suivantes :  
  
-   Quand un alias est utilisé pour la table ou la vue dans la clause [FROM](../../t-sql/queries/from-transact-sql.md) de la requête, _nom\_objet\_exposé_ est cet alias.  
  
-   Si aucun alias n’est utilisé, _nom\_objet\_exposé_ est la correspondance à l’exact de la table ou de la vue à laquelle il est fait référence dans la clause FROM. Par exemple, si la référence à la table ou à la vue consiste en un nom en deux parties, _nom\_objet\_exposé_ correspond au même nom en deux parties.  
  
 Si _nom\_objet\_exposé_ est spécifié sans indicateur de table, tous les index indiqués dans la requête dans le cadre d’un indicateur de table de l’objet sont ignorés. L’optimiseur de requête détermine ensuite l’utilisation de l’index. Vous pouvez utiliser cette technique pour éliminer l'effet d'un indicateur de table INDEX lorsque vous ne pouvez pas modifier la requête d'origine. Voir l'exemple J.  
  
**\<table_hint> ::=** { NOEXPAND [ , INDEX ( _<index\_value>_ [ ,...n ] ) | INDEX = ( _<index\_value>_ ) ] \| INDEX ( _<index\_value>_ [ ,...n ] ) | INDEX = ( _<index\_value>_ ) \| FORCESEEK [ **(** _<index\_value>_ **(** _<index\_column\_name>_ [ **,** ... ] **))** ] \| FORCESCAN \| HOLDLOCK \| NOLOCK \| NOWAIT \| PAGLOCK \| READCOMMITTED \| READCOMMITTEDLOCK \| READPAST \| READUNCOMMITTED \| REPEATABLEREAD \| ROWLOCK \| SERIALIZABLE \| SNAPSHOT \| SPATIAL_WINDOW_MAX_CELLS = _<integer\_value>_ \| TABLOCK \| TABLOCKX \| UPDLOCK \| XLOCK }    
Indicateur de table à appliquer à la table ou la vue qui correspond à *exposed_object_name* en tant qu'indicateur de requête. Pour obtenir une description de ces indicateurs, consultez [Indicateurs de table &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-table.md).  
  
 Les indicateurs de table autres que INDEX, FORCESCAN et FORCESEEK sont interdits comme indicateurs de requête, à moins que la requête n'ait déjà une clause WITH qui spécifie l'indicateur de table. Pour plus d'informations, consultez la [section Remarques](#remarks).  
  
> [!CAUTION]
> Le fait de spécifier FORCESEEK avec des paramètres limite davantage le nombre de plans qui peuvent être considérés par l'optimiseur de requête que le fait de spécifier FORCESEEK sans paramètres. Cela peut provoquer une erreur « Impossible de générer le plan » dans davantage de cas. Dans une version ultérieure, il se peut que des modifications internes de l’optimiseur de requête autorisent la prise en considération de davantage de plans.  
  
## <a name="remarks"></a>Notes  
 Il n’est pas possible de spécifier des indicateurs de requête dans une instruction INSERT sauf si celle-ci contient une clause SELECT.  
  
 Les indicateurs de requête ne peuvent être spécifiés que dans une requête de niveau supérieur et non pas dans des sous-requêtes. Lorsqu’un indicateur de table est spécifié comme indicateur de requête, il peut se trouver dans la requête de premier niveau ou dans une sous-requête. Toutefois, la valeur spécifiée pour _<exposed\_object\_nem>_ dans la clause TABLE HINT doit correspondre exactement au nom exposé dans la requête ou la sous-requête.  
  
## <a name="specifying-table-hints-as-query-hints"></a>Spécification d'indicateurs de table comme indicateurs de requête  
 Nous vous recommandons d’utiliser l’indicateur de table INDEX, FORCESCAN ou FORCESEEK comme indicateur de requête uniquement dans le contexte d’un [repère de plan](../../relational-databases/performance/plan-guides.md). Les repères de plan sont utiles lorsqu’il n’est pas possible de modifier la requête d'origine, par exemple s’il s'agit d'une application tierce. L'indicateur de requête spécifié dans le repère de plan est ajouté à la requête avant sa compilation et son optimisation. Pour les requêtes ad hoc, utilisez la clause TABLE HINT uniquement lors du test des instructions de repère de plan. Pour toutes les autres requêtes ad hoc, nous recommandons de spécifier ces indicateurs uniquement comme indicateurs de table.  
  
 Lorsqu'ils sont spécifiés comme indicateurs de requête, les indicateurs de table INDEX, FORCESCAN et FORCESEEK sont valides pour les objets suivants :  
  
-   Tables  
-   Les vues  
-   Vues indexées  
-   Expressions de table communes (l'indicateur doit être spécifié dans l'instruction SELECT dont le jeu de résultats remplit l'expression de table commune)  
-   Vues de gestion dynamique
-   Sous-requêtes nommées  
  
Il est possible de spécifier des indicateurs de table INDEX, FORCESCAN et FORCESEEK comme indicateurs de requête pour une requête ne disposant pas d’indicateurs de table. Vous pouvez également les utiliser pour remplacer respectivement des indicateurs INDEX, FORCESCAN et FORCESEEK existants dans la requête. 

Les indicateurs de table autres que INDEX, FORCESCAN et FORCESEEK sont interdits comme indicateurs de requête, à moins que la requête n'ait déjà une clause WITH qui spécifie l'indicateur de table. Dans ce cas, il faut également spécifier un indicateur correspondant comme indicateur de requête. Utilisez pour cela TABLE HINT dans la clause OPTION. Cette spécification préserve la sémantique de la requête. Par exemple, si la requête contient l’indicateur de table NOLOCK, la clause OPTION dans le paramètre **\@hints** du repère de plan doit également contenir l’indicateur NOLOCK. Voir l'exemple K. 
  
## <a name="examples"></a>Exemples  
  
### <a name="a-using-merge-join"></a>R. Utilisation de MERGE JOIN  
 L’exemple suivant spécifie que MERGE JOIN exécute l’opération JOIN dans la requête. L'exemple utilise la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
```sql  
SELECT *   
FROM Sales.Customer AS c  
INNER JOIN Sales.CustomerAddress AS ca ON c.CustomerID = ca.CustomerID  
WHERE TerritoryID = 5  
OPTION (MERGE JOIN);  
GO    
```  
  
### <a name="b-using-optimize-for"></a>B. Utilisation de OPTIMIZE FOR  
 L’exemple suivant indique à l’optimiseur de requête d’utiliser la valeur `'Seattle'` pour `@city_name` et d’utiliser la sélectivité moyenne du prédicat sur toutes les valeurs de colonne pour `@postal_code` lors de l’optimisation de la requête. L'exemple utilise la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
```sql  
CREATE PROCEDURE dbo.RetrievePersonAddress
@city_name NVARCHAR(30),  
 @postal_code NVARCHAR(15)
AS
SELECT * FROM Person.Address  
WHERE City = @city_name AND PostalCode = @postal_code  
OPTION ( OPTIMIZE FOR (@city_name = 'Seattle', @postal_code UNKNOWN) );  
GO
```  
  
### <a name="c-using-maxrecursion"></a>C. Utilisation de MAXRECURSION  
 MAXRECURSION peut être utilisé pour empêcher une expression de table commune récursive mal rédigée d'entrer dans une boucle infinie. L’exemple suivant créée intentionnellement une boucle infinie et utilise l’indicateur MAXRECURSION pour limiter le nombre de niveaux de récursivité à deux. L'exemple utilise la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
```sql  
--Creates an infinite loop  
WITH cte (CustomerID, PersonID, StoreID) AS  
(  
    SELECT CustomerID, PersonID, StoreID  
    FROM Sales.Customer  
    WHERE PersonID IS NOT NULL  
  UNION ALL  
    SELECT cte.CustomerID, cte.PersonID, cte.StoreID  
    FROM cte   
    JOIN  Sales.Customer AS e   
        ON cte.PersonID = e.CustomerID  
)  
--Uses MAXRECURSION to limit the recursive levels to 2  
SELECT CustomerID, PersonID, StoreID  
FROM cte  
OPTION (MAXRECURSION 2);  
GO  
```  
  
 Une fois l'erreur de codage corrigée, MAXRECURSION n'est plus nécessaire.  
  
### <a name="d-using-merge-union"></a>D. Utilisation de MERGE UNION  
 L’exemple suivant utilise l’indicateur de requête MERGE UNION. L'exemple utilise la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
```sql  
SELECT *  
FROM HumanResources.Employee AS e1  
UNION  
SELECT *  
FROM HumanResources.Employee AS e2  
OPTION (MERGE UNION);  
GO  
```  
  
### <a name="e-using-hash-group-and-fast"></a>E. Utilisation de HASH GROUP et de FAST  
 L’exemple suivant utilise les indicateurs de requête HASH GROUP et FAST. L'exemple utilise la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
```sql  
SELECT ProductID, OrderQty, SUM(LineTotal) AS Total  
FROM Sales.SalesOrderDetail  
WHERE UnitPrice < $5.00  
GROUP BY ProductID, OrderQty  
ORDER BY ProductID, OrderQty  
OPTION (HASH GROUP, FAST 10);  
GO    
```  
  
### <a name="f-using-maxdop"></a>F. Utilisation de MAXDOP  
 L’exemple suivant utilise l’indicateur de requête MAXDOP. L'exemple utilise la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
```sql  
SELECT ProductID, OrderQty, SUM(LineTotal) AS Total  
FROM Sales.SalesOrderDetail  
WHERE UnitPrice < $5.00  
GROUP BY ProductID, OrderQty  
ORDER BY ProductID, OrderQty  
OPTION (MAXDOP 2);    
GO
```  
  
### <a name="g-using-index"></a>G. Utilisation de INDEX  
 Les exemples suivants utilisent l'indicateur INDEX. Le premier exemple spécifie un index unique. Le deuxième exemple spécifie plusieurs index pour une référence de table individuelle. Dans les deux exemples, étant donné que l'indicateur INDEX est appliqué à une table qui utilise un alias, la clause TABLE HINT doit également spécifier le même alias que le nom d'objet exposé. L'exemple utilise la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
```sql  
EXEC sp_create_plan_guide   
    @name = N'Guide1',   
    @stmt = N'SELECT c.LastName, c.FirstName, e.Title  
              FROM HumanResources.Employee AS e   
              JOIN Person.Contact AS c ON e.ContactID = c.ContactID  
              WHERE e.ManagerID = 2;',   
    @type = N'SQL',  
    @module_or_batch = NULL,   
    @params = NULL,   
    @hints = N'OPTION (TABLE HINT(e, INDEX (IX_Employee_ManagerID)))';  
GO  
EXEC sp_create_plan_guide   
    @name = N'Guide2',   
    @stmt = N'SELECT c.LastName, c.FirstName, e.Title  
              FROM HumanResources.Employee AS e  
              JOIN Person.Contact AS c ON e.ContactID = c.ContactID  
              WHERE e.ManagerID = 2;',   
    @type = N'SQL',  
    @module_or_batch = NULL,   
    @params = NULL,   
    @hints = N'OPTION (TABLE HINT(e, INDEX(PK_Employee_EmployeeID, IX_Employee_ManagerID)))';  
GO    
```  
  
### <a name="h-using-forceseek"></a>H. Utilisation de FORCESEEK  
 L'exemple suivant utilise l'indicateur de table FORCESEEK. La clause TABLE HINT doit également spécifier le même nom en deux parties que le nom d’objet exposé. Indiquez ce nom lorsque vous appliquez l’indicateur INDEX à une table qui utilise un nom en deux parties. L'exemple utilise la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
```sql  
EXEC sp_create_plan_guide   
    @name = N'Guide3',   
    @stmt = N'SELECT c.LastName, c.FirstName, HumanResources.Employee.Title  
              FROM HumanResources.Employee  
              JOIN Person.Contact AS c ON HumanResources.Employee.ContactID = c.ContactID  
              WHERE HumanResources.Employee.ManagerID = 3  
              ORDER BY c.LastName, c.FirstName;',   
    @type = N'SQL',  
    @module_or_batch = NULL,   
    @params = NULL,   
    @hints = N'OPTION (TABLE HINT( HumanResources.Employee, FORCESEEK))';  
GO    
```  
  
### <a name="i-using-multiple-table-hints"></a>I. Utilisation de plusieurs indicateurs de table  
 L'exemple suivant applique l'indicateur INDEX à une table et l'indicateur FORCESEEK à une autre. L'exemple utilise la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
```sql  
EXEC sp_create_plan_guide   
    @name = N'Guide4',   
    @stmt = N'SELECT e.ManagerID, c.LastName, c.FirstName, e.Title  
              FROM HumanResources.Employee AS e   
              JOIN Person.Contact AS c ON e.ContactID = c.ContactID  
              WHERE e.ManagerID = 3;',   
    @type = N'SQL',  
    @module_or_batch = NULL,   
    @params = NULL,   
    @hints = N'OPTION (TABLE HINT (e, INDEX( IX_Employee_ManagerID))   
                       , TABLE HINT (c, FORCESEEK))';  
GO  
```  
  
### <a name="j-using-table-hint-to-override-an-existing-table-hint"></a>J. Utilisation de TABLE HINT pour substituer un indicateur de table existant  
L’exemple suivant montre comment se servir de l’indicateur TABLE HINT. Vous pouvez l’utiliser sans spécifier d’indicateur pour remplacer le comportement de l’indicateur de table INDEX spécifié dans la clause FROM de la requête. L'exemple utilise la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
```sql  
EXEC sp_create_plan_guide   
    @name = N'Guide5',   
    @stmt = N'SELECT e.ManagerID, c.LastName, c.FirstName, e.Title  
              FROM HumanResources.Employee AS e WITH (INDEX (IX_Employee_ManagerID))  
              JOIN Person.Contact AS c ON e.ContactID = c.ContactID  
              WHERE e.ManagerID = 3;',   
    @type = N'SQL',  
    @module_or_batch = NULL,   
    @params = NULL,   
    @hints = N'OPTION (TABLE HINT(e))';  
GO    
```  
  
### <a name="k-specifying-semantics-affecting-table-hints"></a>K. Spécification d'indicateurs de table affectant la sémantique  
L’exemple suivant contient deux indicateurs de table dans la requête : NOLOCK, qui affecte la sémantique, et INDEX, qui n’affecte pas la sémantique. Pour préserver la sémantique de la requête, l'indicateur NOLOCK est spécifié dans la clause OPTIONS du repère de plan. En parallèle de l'indicateur NOLOCK, spécifiez les indicateurs INDEX et FORCESEEK pour remplacer l'indicateur INDEX sans effet sur la sémantique dans la requête lors de la compilation et de l’optimisation de l'instruction. L'exemple utilise la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
```sql  
EXEC sp_create_plan_guide   
    @name = N'Guide6',   
    @stmt = N'SELECT c.LastName, c.FirstName, e.Title  
              FROM HumanResources.Employee AS e   
                   WITH (NOLOCK, INDEX (PK_Employee_EmployeeID))  
              JOIN Person.Contact AS c ON e.ContactID = c.ContactID  
              WHERE e.ManagerID = 3;',  
    @type = N'SQL',  
    @module_or_batch = NULL,   
    @params = NULL,   
    @hints = N'OPTION (TABLE HINT (e, INDEX(IX_Employee_ManagerID), NOLOCK, FORCESEEK))';  
GO    
```  
  
L'exemple suivant indique une autre méthode pour préserver la sémantique de la requête et permettre à l'optimiseur de choisir un index autre que l'index spécifié dans l'indicateur de table. Autorisez l’optimiseur à choisir en spécifiant l’indicateur NOLOCK dans la clause OPTIONS. En effet, cet indicateur affecte la sémantique. Ensuite, indiquez le mot clé TABLE HINT avec seulement une référence de table, sans indicateur INDEX. L'exemple utilise la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
```sql  
EXEC sp_create_plan_guide   
    @name = N'Guide7',   
    @stmt = N'SELECT c.LastName, c.FirstName, e.Title  
              FROM HumanResources.Employee AS e   
                   WITH (NOLOCK, INDEX (PK_Employee_EmployeeID))  
              JOIN Person.Contact AS c ON e.ContactID = c.ContactID  
              WHERE e.ManagerID = 2;',  
    @type = N'SQL',  
    @module_or_batch = NULL,   
    @params = NULL,   
    @hints = N'OPTION (TABLE HINT (e, NOLOCK))';  
GO  
```  

### <a name="l-using-use-hint"></a>L. Utilisation d’indicateurs USE HINT  
 L’exemple suivant utilise les indicateurs de requête RECOMPILE et USE HINT. L'exemple utilise la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
```sql  
SELECT * FROM Person.Address  
WHERE City = 'SEATTLE' AND PostalCode = 98104
OPTION (RECOMPILE, USE HINT ('ASSUME_MIN_SELECTIVITY_FOR_FILTER_ESTIMATES', 'DISABLE_PARAMETER_SNIFFING')); 
GO  
```  

### <a name="m-using-querytraceon-hint"></a>M. Utilisation de QUERYTRACEON HINT  
 L’exemple suivant utilise les indicateurs de requête QUERYTRACEON. L'exemple utilise la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]. Vous pouvez activer tous les correctifs affectant le plan contrôlés par l’indicateur de trace 4199 pour une requête particulière avec la requête suivante :
  
```sql  
SELECT * FROM Person.Address  
WHERE City = 'SEATTLE' AND PostalCode = 98104
OPTION (QUERYTRACEON 4199);
```  

 Vous pouvez également utiliser plusieurs indicateurs de trace comme dans la requête suivante :

```sql
SELECT * FROM Person.Address  
WHERE City = 'SEATTLE' AND PostalCode = 98104
OPTION  (QUERYTRACEON 4199, QUERYTRACEON 4137);
```

## <a name="see-also"></a>Voir aussi  
[Indicateurs &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql.md)   
[sp_create_plan_guide &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-create-plan-guide-transact-sql.md)   
[sp_control_plan_guide &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-control-plan-guide-transact-sql.md)  
[Indicateurs de trace](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md)       
[Conventions syntaxiques de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)      
