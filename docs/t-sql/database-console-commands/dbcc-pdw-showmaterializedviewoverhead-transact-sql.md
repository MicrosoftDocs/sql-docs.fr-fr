---
description: DBCC PDW_SHOWMATERIALIZEDVIEWOVERHEAD (Transact-SQL)
title: DBCC PDW_SHOWMATERIALIZEDVIEWOVERHEAD (Transact-SQL)
ms.custom: seo-dt-2019
ms.date: 07/03/2019
ms.prod: sql
ms.technology: data-warehouse
ms.prod_service: sql-data-warehouse, pdw
ms.reviewer: ''
ms.topic: language-reference
dev_langs:
- TSQL
author: XiaoyuMSFT
ms.author: xiaoyul
monikerRange: = azure-sqldw-latest
ms.openlocfilehash: 87f02544512f4cd33385242101204e3d30c81ffe
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643935"
---
# <a name="dbcc-pdw_showmaterializedviewoverhead-transact-sql"></a>DBCC PDW_SHOWMATERIALIZEDVIEWOVERHEAD (Transact-SQL)  

[!INCLUDE [asa](../../includes/applies-to-version/asa.md)]

Affiche le nombre de modifications incrémentielles dans les tables de base qui sont conservées pour les affichages matérialisés dans [!INCLUDE[ssSDW](../../includes/sssdw-md.md)]. Le taux de surcharge est calculé en tant que TOTAL_ROWS / MAX (1, BASE_VIEW_ROWS).

![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique")[Conventions de la syntaxe Transact-SQL &#40;Transact-SQL&#41;](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntaxe

```syntaxsql
DBCC PDW_SHOWMATERIALIZEDVIEWOVERHEAD ( " [ schema_name .] materialized_view_name  " )
[;]
```

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

## <a name="arguments"></a>Arguments

 *schema_name*     
 Nom du schéma auquel appartient la vue.

*materialized_view_name*   
Est le nom de l’affichage matérialisé.

## <a name="remarks"></a>Notes

Pour garantir l’actualisation des vues matérialisées avec les modifications de données effectuées dans les tables de base, le moteur de l’entrepôt de données ajoute des lignes de suivi à chaque vue impactée pour refléter les modifications. La sélection à partir d’une vue matérialisée inclut l’analyse de l’index columnstore cluster de la vue et l’application des modifications incrémentielles éventuelles.Les lignes de suivi (TOTAL_ROWS - BASE_VIEW_ROWS) sont conservées jusqu’à ce que les utilisateurs regénèrent (REBUILD) la vue matérialisée.  

La valeur overhead_ratio se calcule ainsi : TOTAL_ROWS/MAX(1, BASE_VIEW_ROWS).  Une valeur élevée dégrade les performances SELECT.Les utilisateurs peuvent regénérer la vue matérialisée pour réduire son taux de charge.

## <a name="permissions"></a>Autorisations  
  
Requiert l'autorisation VIEW DATABASE STATE.  

## <a name="examples"></a>Exemples  

### <a name="a-this-example-returns-the-overhead-ratio-of-a-materialized-view"></a>R. Cet exemple retourne le taux de charge d’une vue matérialisée.

```sql
DBCC PDW_SHOWMATERIALIZEDVIEWOVERHEAD ( "dbo.MyIndexedView" )
```

Sortie :

|OBJECT_ID|BASE_VIEW_ROWS|TOTAL_ROWS|OVERHEAD_RATIO|
|--------|--------|--------|--------|  
|1234|1|3 |3.0 |

</br>

### <a name="b-this-example-shows-how-the-materialized-view-overhead-increases-as-data-changes-in-base-tables"></a>B. Cet exemple montre l’augmentation de la charge de la vue matérialisée au fur et à mesure que des données sont modifiées dans les tables de base

Créer une table
```sql
CREATE TABLE t1 (c1 INT NOT NULL, c2 INT NOT NULL, c3 INT NOT NULL)
```
Ajouter cinq lignes à t1
```sql
INSERT INTO t1 VALUES (1, 1, 1)
INSERT INTO t1 VALUES (2, 2, 2) 
INSERT INTO t1 VALUES (3, 3, 3) 
INSERT INTO t1 VALUES (4, 4, 4) 
INSERT INTO t1 VALUES (5, 5, 5) 
```
Créer des vues matérialisées MV1
```sql
CREATE MATERIALIZED VIEW MV1 
WITH (DISTRIBUTION = HASH(c1))  
AS
SELECT c1, COUNT(*) total_number 
FROM dbo.t1 WHERE c1 < 3
GROUP BY c1  
```
La sélection à partir de la vue matérialisée retourne deux lignes.

|c1|total_number|
|--------|--------| 
|1|1| 
|2|1|

Vérifiez la charge de la vue matérialisée avant toute modification de données dans la table de base.
```sql
DBCC PDW_SHOWMATERIALIZEDVIEWOVERHEAD ("dbo.mv1")
```
Sortie :

|OBJECT_ID|BASE_VIEW_ROWS|TOTAL_ROWS|OVERHEAD_RATIO|
|--------|--------|--------|--------|  
|587149137|2|2 |1.00000000000000000 |

Mettez à jour la table de base.  Cette requête met à jour la même colonne dans la même ligne 100 fois avec la même valeur.  Le contenu de la vue matérialisée ne change pas.
```sql
DECLARE @p INT
SELECT @p = 1
WHILE (@p < 101)
BEGIN
UPDATE t1 SET c1 = 1 WHERE c1 = 1
SELECT @p = @p+1
END  
```

La sélection à partir de la vue matérialisée retourne le même résultat qu’avant.  

|c1|total_number|
|--------|--------| 
|1|1| 
|2|1|

La sortie de DBCC PDW_SHOWMATERIALIZEDVIEWOVERHEAD ("dbo.mv1") est présentée ci-dessous.  100 lignes sont ajoutées à la vue matérialisée (total_row - base_view_rows) et la valeur overhead_ratio est augmentée. 

|OBJECT_ID|BASE_VIEW_ROWS|TOTAL_ROWS|OVERHEAD_RATIO|
|--------|--------|--------|--------|  
|587149137|2|102 |51.00000000000000000 |

Après la regénération de la vue matérialisée, toutes les lignes de suivi des modifications incrémentielles de données sont supprimées, et le taux de charge de la vue est réduit.  

```sql
ALTER MATERIALIZED VIEW dbo.MV1 REBUILD
GO
DBCC PDW_SHOWMATERIALIZEDVIEWOVERHEAD ("dbo.mv1")
```
Sortie

|OBJECT_ID|BASE_VIEW_ROWS|TOTAL_ROWS|OVERHEAD_RATIO|
|--------|--------|--------|--------|  
|587149137|2|2 |1.00000000000000000 |

## <a name="see-also"></a>Voir aussi

[Réglage des performances avec une vue matérialisée](/azure/sql-data-warehouse/performance-tuning-materialized-views)   
[CREATE MATERIALIZED VIEW AS SELECT &#40;Transact-SQL&#41;](../statements/create-materialized-view-as-select-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)   
[ALTER MATERIALIZED VIEW &#40;Transact-SQL&#41;](../statements/alter-materialized-view-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)   
[EXPLAIN &#40;Transact-SQL&#41;](../queries/explain-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)   
[sys.pdw_materialized_view_column_distribution_properties &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-pdw-materialized-view-column-distribution-properties-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)   
[sys.pdw_materialized_view_distribution_properties &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-pdw-materialized-view-distribution-properties-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)   
[sys.pdw_materialized_view_mappings &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-pdw-materialized-view-mappings-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)   
[Vues de catalogue Azure Synapse Analytics et Parallel Data Warehouse](../../relational-databases/system-catalog-views/sql-data-warehouse-and-parallel-data-warehouse-catalog-views.md)   
[Vues système prises en charge dans Azure Synapse Analytics](/azure/sql-data-warehouse/sql-data-warehouse-reference-tsql-system-views)   
[Instructions T-SQL prises en charge dans Azure Synapse Analytics](/azure/sql-data-warehouse/sql-data-warehouse-reference-tsql-statements)