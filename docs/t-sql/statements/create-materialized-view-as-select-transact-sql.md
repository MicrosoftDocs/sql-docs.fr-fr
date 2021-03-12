---
description: CREATE MATERIALIZED VIEW AS SELECT (Transact-SQL)
title: CREATE MATERIALIZED VIEW AS SELECT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2020
ms.prod: sql
ms.prod_service: sql-data-warehouse
ms.reviewer: jrasnick
ms.technology: data-warehouse
ms.topic: reference
f1_keywords:
- CREATE VIEW
- VIEW_TSQL
- VIEW
- CREATE_VIEW_TSQL
- SCHEMABINDING_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- table creation [SQL Server], CREATE VIEW
- views [SQL Server], creating
- CREATE VIEW statement
- updatable partitioned views
- tables [SQL Server], virtual
- updatable views
- modifying partitioned views
- virtual tables [SQL Server]
- number of columns per view
- partitioned views [SQL Server], creating
- WITH ENCRYPTION clause
- WITH CHECK OPTION clause
- partitioned views [SQL Server], modifying
- partitioned views [SQL Server], replication
- distributed partitioned views [SQL Server]
- views [SQL Server], indexed views
- maximum number of columns per view
ms.assetid: aecc2f73-2ab5-4db9-b1e6-2f9e3c601fb9
author: XiaoyuMSFT
ms.author: xiaoyul
monikerRange: =azure-sqldw-latest
ms.openlocfilehash: 81073d458d69e28ee145c1bb1dd38f3474800b35
ms.sourcegitcommit: f10f0d604be1dce6c600a92aec4c095e7b52e19c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/11/2021
ms.locfileid: "102770537"
---
# <a name="create-materialized-view-as-select-transact-sql"></a>CREATE MATERIALIZED VIEW AS SELECT (Transact-SQL)  

[!INCLUDE [Azure Synapse Analytics](../../includes/applies-to-version/asa.md)]

Cet article explique l’instruction SQL CREATE MATERIALIZED VIEW AS SELECT T dans [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] pour développer des solutions. L’article fournit également des exemples de code.

Une vue matérialisée conserve les données renvoyées par la requête de définition de vue et est automatiquement mise à jour à mesure que les données changent dans les tables sous-jacentes.   Elle améliore les performances des requêtes complexes (généralement des requêtes contenant des jointures et agrégations) tout en offrant des opérations de maintenance simples.   Avec sa capacité d’automatching du plan d’exécution, un affichage matérialisé ne devra pas être référencés dans la requête pour que l’optimiseur prenne en compte l’affichage pour substitution.  Cette fonctionnalité permet aux ingénieurs des données d’implémenter des vues matérialisées comme un mécanisme pour améliorer les temps de réponse des requêtes, sans devoir modifier les requêtes.  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
CREATE MATERIALIZED VIEW [ schema_name. ] materialized_view_name
    WITH (  
      <distribution_option>
    )
    AS <select_statement>
[;]

<distribution_option> ::=
    {  
        DISTRIBUTION = HASH ( distribution_column_name )  
      | DISTRIBUTION = ROUND_ROBIN  
    }

<select_statement> ::=
    SELECT select_criteria

```

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]
  
## <a name="arguments"></a>Arguments

*schema_name*     
 Nom du schéma auquel appartient la vue.  
  
*materialized_view_name*   
Nom de la vue. Les noms des vues doivent se conformer aux règles applicables aux identificateurs. Vous n'êtes pas tenu de spécifier le nom du propriétaire de la vue.  

*option de distribution*     
Seules les distributions HASH et ROUND_ROBIN sont prises en charge.

*select_statement*   
La liste SELECT dans la définition de l’affichage matérialisé doit respecter au moins un de ces deux critères :
- La liste SELECT contient une fonction d’agrégation.
- GROUP BY est utilisé dans la définition de l’affichage matérialisé et toutes les colonnes dans GROUP BY sont incluses dans la liste SELECT.  Il est possible d’utiliser jusqu’à 32 colonnes dans la clause GROUP BY.

Les fonctions d’agrégation sont requises dans la liste SELECT de la définition de l’affichage matérialisé.  Les agrégations prises en charge incluent MAX, MIN, AVG, COUNT, COUNT_BIG, SUM, VAR, STDEV.

Lorsque les agrégats MIN/MAX sont utilisés dans la liste SELECT de la définition de l’affichage matérialisé, les conditions suivantes s’appliquent :
 
- FOR_APPEND est requis.  Par exemple :
  ```sql 
  CREATE MATERIALIZED VIEW mv_test2  
  WITH (distribution = hash(i_category_id), FOR_APPEND)  
  AS
  SELECT MAX(i.i_rec_start_date) as max_i_rec_start_date, MIN(i.i_rec_end_date) as min_i_rec_end_date, i.i_item_sk, i.i_item_id, i.i_category_id
  FROM syntheticworkload.item i  
  GROUP BY i.i_item_sk, i.i_item_id, i.i_category_id
  ```

- L’affichage matérialisé sera désactivé lorsque UPDATE ou DELETE se produit dans les tables de base référencées.  Cette restriction ne s’applique pas aux instructions INSERT.  Pour réactiver l’affichage matérialisé, exécutez ALTER MATERIALIZED VIEW avec REBUILD.
  
## <a name="remarks"></a>Notes

Une vue matérialisée dans l’entrepôt de données Azure est similaire à une vue indexée dans SQL Server.Il partage presque les mêmes restrictions que la vue indexée (consultez [Créer des vues indexées](../../relational-databases/views/create-indexed-views.md) pour plus d’informations), à ceci près qu’un affichage matérialisé prend en charge des fonctions d’agrégation.   

>[!Note]
>Bien que CREATE MATERIALIZED VIEW ne prenne pas en charge COUNT, DISTINCT, COUNT(DISTINCT expression) ou COUNT_BIG (DISTINCT expression), les requêtes SELECT avec ces fonctions peuvent tirer parti des vues matérialisées afin d’améliorer les performances, car l’optimiseur Synapse SQL peut réécrire automatiquement ces agrégations dans la requête utilisateur pour les faire correspondre à des vues matérialisées existantes.  Pour plus d’informations, consultez la section Exemple de cet article. 

APPROX_COUNT_DISTINCT n’est pas pris en charge dans CREATE MATERIALIZED VIEW AS SELECT.

Seul CLUSTERED COLUMNSTORE INDEX est pris en charge par l’affichage matérialisé. 

Une vue matérialisée ne peut pas référencer d’autres vues.  

Une vue matérialisée ne peut pas être créée sur une table avec un masquage dynamique des données, même si la colonne avec masquage dynamique des données ne fait pas partie de la vue matérialisée.  Si une colonne de table fait partie d’une vue matérialisée active ou d’une vue matérialisée désactivée, le masquage dynamique des données ne peut pas être ajouté à cette colonne.  

Vous ne pouvez pas créer une vue matérialisée sur une table pour laquelle la sécurité au niveau des lignes est activée.

Les affichages matérialisés peuvent être créés sur les tables partitionnées.  Les opérations SPLIT/MERGE sur les partitions sont prises en charge sur les tables de base des vues matérialisées ; l’opération SWITCH sur des partitions n’est pas prise en charge.  
 
ALTER TABLE SWITCH n’est pas pris en charge sur les tables référencées dans les affichages matérialisés. Désactiver ou déposer les affichages matérialisés avant d’utiliser ALTER TABLE SWITCH. Dans les scénarios suivants, la création de l’affichage matérialisé nécessite l’ajout de nouvelles colonnes à l’affichage matérialisé :

|Scénario|Nouvelles colonnes à ajouter à l’affichage matérialisé|Commentaire|  
|-----------------|---------------|-----------------|
|COUNT_BIG() est manquant dans la liste SELECT d’une définition de vue matérialisée| COUNT_BIG (*) |Ajouté automatiquement par la création de l’affichage matérialisé.  Aucune action de l'utilisateur n'est requise.|
|SUM(a) est spécifié par les utilisateurs dans la liste SELECT d’une définition d’affichage matérialisé ET « a » est une expression nullable |COUNT_BIG (a) |Les utilisateurs doivent ajouter l’expression « a » manuellement dans la définition de l’affichage matérialisé.|
|AVG(a) est spécifié par les utilisateurs dans la liste SELECT d’une définition d’affichage matérialisé où « a » est une expression.|SUM(a), COUNT_BIG(a)|Ajouté automatiquement par la création de l’affichage matérialisé.  Aucune action de l'utilisateur n'est requise.|
|STDEV(a) est spécifié par les utilisateurs dans la liste SELECT d’une définition d’affichage matérialisé où « a » est une expression.|SUM(a), COUNT_BIG(a), SUM(square(a))|Ajouté automatiquement par la création de l’affichage matérialisé.  Aucune action de l'utilisateur n'est requise. |
| | | |

Après la création, des affichages matérialisés sont visibles dans SQL Server Management Studio sous le dossier des affichages de l’instance [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)].

Les utilisateurs peuvent exécuter [SP_SPACEUSED](../../relational-databases/system-stored-procedures/sp-spaceused-transact-sql.md?view=azure-sqldw-latest&preserve-view=true) et [DBCC PDW_SHOWSPACEUSED](../database-console-commands/dbcc-pdw-showspaceused-transact-sql.md?view=azure-sqldw-latest&preserve-view=true) pour déterminer l’espace consommé par une vue matérialisée.  

Un affichage matérialisé peut être déposé par le biais de DROP VIEW.  Vous pouvez utiliser ALTER MATERIALIZED VIEW pour désactiver ou régénérer un affichage matérialisé.   

La vue matérialisée est un mécanisme d’optimisation automatique des requêtes.  Les utilisateurs n’ont pas besoin d’interroger une vue matérialisée directement.  Quand une requête utilisateur est soumise, le moteur vérifie les autorisations de l’utilisateur sur les objets de requête et fait échouer la requête sans exécution si l’utilisateur n’a pas accès aux tables ou aux vues normales de la requête.  Si l’autorisation de l’utilisateur a été vérifiée, l’optimiseur utilise automatiquement une vue matérialisée correspondante pour exécuter la requête et accélérer le traitement.  Les utilisateurs obtiennent les mêmes données, que la requête soit traitée en interrogeant les tables de base ou la vue matérialisée.  

Le plan EXPLAIN et le Plan d’exécution graphique estimé dans SQL Server Management Studio peuvent indiquer si un affichage matérialisé est pris en compte par l’optimiseur de requête pour l’exécution des requêtes. et le Plan d’exécution graphique estimé dans SQL Server Management Studio peuvent indiquer si un affichage matérialisé est pris en compte par l’optimiseur de requête pour l’exécution des requêtes.

Pour déterminer si une instruction SQL peut bénéficier d’un nouvel affichage matérialisé, exécutez la commande `EXPLAIN` avec `WITH_RECOMMENDATIONS`.  Pour plus d’informations, consultez [EXPLAIN (Transact-SQL)](../queries/explain-transact-sql.md?view=azure-sqldw-latest&preserve-view=true).

## <a name="ownership"></a>Propriété
- Une vue matérialisée ne peut pas être créée si les tables de base et la vue matérialisée à créer n’ont pas le même propriétaire.
- Une vue matérialisée et ses tables de base peuvent résider dans des schémas différents. Quand la vue matérialisée est créée, le propriétaire du schéma de la vue devient automatiquement le propriétaire de la vue matérialisée et la propriété de cette vue ne peut pas changer.     

## <a name="permissions"></a>Autorisations
En plus de respecter les conditions de propriété de l’objet, un utilisateur doit disposer des autorisations suivantes pour pouvoir créer une vue matérialisée : 
1) Autorisation CREATE VIEW dans la base de données
2) Autorisation SELECT sur les tables de base de la vue matérialisée
3) Autorisation REFERENCES sur le schéma contenant les tables de base
4) Autorisation ALTER sur le schéma contenant la vue matérialisée 


## <a name="example"></a>Exemple
R. Cet exemple montre comment l’optimiseur Synapse SQL utilise automatiquement des vues matérialisées pour exécuter une requête afin d’améliorer les performances, même quand la requête utilise des fonctions non prises en charge dans CREATE MATERIALIZED VIEW, telles que COUNT(DISTINCT expression). Une requête utilisateur dont l’exécution prenait habituellement plusieurs secondes dure désormais moins d’une seconde, sans qu’elle doive subir de modification.   

``` sql 

-- Create a table with ~536 million rows
create table t(a int not null, b int not null, c int not null) with (distribution=hash(a), clustered columnstore index);

insert into t values(1,1,1);

declare @p int =1;
while (@P < 30)
    begin
    insert into t select a+1,b+2,c+3 from t;  
    select @p +=1;
end

-- A SELECT query with COUNT_BIG (DISTINCT expression) took multiple seconds to complete and it reads data directly from the base table a. 
select a, count_big(distinct b) from t group by a;

-- Create two materialized views, not using COUNT_BIG(DISTINCT expression).
create materialized view V1 with(distribution=hash(a)) as select a, b from dbo.t group by a, b;

-- Clear all cache.

DBCC DROPCLEANBUFFERS;
DBCC freeproccache;

-- Check the estimated execution plan in SQL Server Management Studio.  It shows the SELECT query is first step (GET operator) is to read data from the materialized view V1, not from base table a.
select a, count_big(distinct b) from t group by a;

-- Now execute this SELECT query.  This time it took sub-second to complete because Synapse SQL engine automatically matches the query with materialized view V1 and uses it for faster query execution.  There was no change in the user query.

DECLARE @timerstart datetime2, @timerend datetime2;
SET @timerstart = sysdatetime();

select a, count_big(distinct b) from t group by a;

SET @timerend = sysdatetime()
select DATEDIFF(ms,@timerstart,@timerend);

```

B. Dans cet exemple, User2 crée une vue matérialisée sur des tables dont le propriétaire est User1.  La vue matérialisée est la propriété de User1.
```sql
/****************************************************************
Setup:
SchemaX owner = DBO
SchemaX.T1 owner = User1
SchemaX.T2 owner = User1
SchemaY owner = User1
*****************************************************************/
CREATE USER User1 WITHOUT LOGIN ;
CREATE USER User2 WITHOUT LOGIN ;
CREATE SCHEMA SchemaX;  
CREATE SCHEMA SchemaY AUTHORIZATION User1;
GO
CREATE TABLE [SchemaX].[T1] (   [vendorID] [varchar](255) Not NULL, [totalAmount] [float] Not NULL, [puYear] [int] NULL );
CREATE TABLE [SchemaX].[T2] (   [vendorID] [varchar](255) Not NULL, [totalAmount] [float] Not NULL, [puYear] [int] NULL);
GO
ALTER AUTHORIZATION ON OBJECT::SchemaX.[T1] TO User1;
ALTER AUTHORIZATION ON OBJECT::SchemaX.[T2] TO User1;

/*****************************************************************************
For user2 to create a MV in SchemaY on SchemaX.T1 and SchemaX.T2, user2 needs:
1. CREATE VIEW permission in the database
2. REFERENCES permission on the schema1
3. SELECT permission on base table T1, T2  
4. ALTER permission on SchemaY
******************************************************************************/
GRANT CREATE VIEW to User2;
GRANT REFERENCES ON SCHEMA::SchemaX to User2;  
GRANT SELECT ON OBJECT::SchemaX.T1 to User2; 
GRANT SELECT ON OBJECT::SchemaX.T2 to User2;
GRANT ALTER ON SCHEMA::SchemaY to User2; 
GO
EXECUTE AS USER = 'User2';  
GO
CREATE materialized VIEW [SchemaY].MV_by_User2 with(distribution=round_robin) 
as 
        select A.vendorID, sum(A.totalamount) as S, Count_Big(*) as T 
        from [SchemaX].[T1] A
        inner join [SchemaX].[T2] B on A.vendorID = B.vendorID group by A.vendorID ;
GO
revert;
GO
```

## <a name="see-also"></a>Voir aussi

[Réglage des performances avec une vue matérialisée](/azure/sql-data-warehouse/performance-tuning-materialized-views)   
[ALTER MATERIALIZED VIEW &#40;Transact-SQL&#41;](./alter-materialized-view-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)      
[DROP VIEW](./drop-view-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)  
[EXPLAIN &#40;Transact-SQL&#41;](../queries/explain-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)   
[sys.pdw_materialized_view_column_distribution_properties &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-pdw-materialized-view-column-distribution-properties-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)   
[sys.pdw_materialized_view_distribution_properties &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-pdw-materialized-view-distribution-properties-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)   
[sys.pdw_materialized_view_mappings &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-pdw-materialized-view-mappings-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)   
[DBCC PDW_SHOWMATERIALIZEDVIEWOVERHEAD &#40;Transact-SQL&#41;](../database-console-commands/dbcc-pdw-showmaterializedviewoverhead-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)   
[Vues de catalogue [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]](../../relational-databases/system-catalog-views/sql-data-warehouse-and-parallel-data-warehouse-catalog-views.md)   
[Vues système prises en charge dans Azure [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]](/azure/sql-data-warehouse/sql-data-warehouse-reference-tsql-system-views)   
[Instructions T-SQL prises en charge dans Azure [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]](/azure/sql-data-warehouse/sql-data-warehouse-reference-tsql-statements)
