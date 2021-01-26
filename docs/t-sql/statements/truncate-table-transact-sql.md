---
description: TRUNCATE TABLE (Transact-SQL)
title: TRUNCATE TABLE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/10/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- TRUNCATE
- TRUNCATE TABLE
- TRUNCATE_TSQL
- TRUNCATE_TABLE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- row removal [SQL Server], TRUNCATE TABLE statement
- table truncating [SQL Server]
- removing rows
- TRUNCATE TABLE statement
- deleting rows
- dropping rows
ms.assetid: 3d544eed-3993-4055-983d-ea334f8c5c58
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 63db7f43659c85fe0f338817b3efe061c984b872
ms.sourcegitcommit: 713e5a709e45711e18dae1e5ffc190c7918d52e7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/22/2021
ms.locfileid: "98688905"
---
# <a name="truncate-table-transact-sql"></a>TRUNCATE TABLE (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Supprime toutes les lignes d’une table ou des partitions spécifiées d’une table, sans journaliser les suppressions de ligne individuelles. L'instruction TRUNCATE TABLE est semblable à l'instruction DELETE sans la clause WHERE. Cependant, l'instruction TRUNCATE TABLE est plus rapide et utilise moins de ressources système et de ressources du journal des transactions.  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
TRUNCATE TABLE   
    { database_name.schema_name.table_name | schema_name.table_name | table_name }  
    [ WITH ( PARTITIONS ( { <partition_number_expression> | <range> }   
    [ , ...n ] ) ) ]  
[ ; ]  
  
<range> ::=  
<partition_number_expression> TO <partition_number_expression>  
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
TRUNCATE TABLE { database_name.schema_name.table_name | schema_name.table_name | table_name }  
[;]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *database_name*  
 Nom de la base de données.  
  
 *schema_name*  
 Nom du schéma auquel appartient la table.  
  
 *table_name*  
 Nom de la table à tronquer ou dont toutes les lignes sont supprimées. *table_name* doit être un littéral. *table_name* ne peut pas être la fonction **OBJECT_ID()** ou une variable.  
  
 WITH ( PARTITIONS ( { \<*partition_number_expression*> | \<*range*> } [ , ...n ] ) )    
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (de [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] à la [version actuelle](../../sql-server/what-s-new-in-sql-server-2016.md)).
  
 Spécifie les partitions à tronquer ou à partir desquelles toutes les lignes sont supprimées. Si la table n'est pas partitionnée, l'argument `WITH PARTITIONS` génère une erreur. Si la clause `WITH PARTITIONS` n’est pas fournie, la table entière sera tronquée.  
  
 *\<partition_number_expression>* peut être spécifié des manières suivantes : 
  
-   Spécifiez le numéro d’une partition, par exemple : `WITH (PARTITIONS (2))`  
  
-   Spécifiez les numéros de plusieurs partitions individuelles séparés par des virgules, par exemple : `WITH (PARTITIONS (1, 5))`  
  
-   Spécifiez à la fois des plages et des partitions individuelles, par exemple : `WITH (PARTITIONS (2, 4, 6 TO 8))`  
  
-   *\<range>* peut être spécifié sous la forme de numéros de partitions séparés par le mot **TO**, par exemple : `WITH (PARTITIONS (6 TO 8))`  
  
 Pour tronquer une table partitionnée, la table et les index doivent être alignés (partitionnés sur la même fonction de partition).  
  
## <a name="remarks"></a>Notes  
 Comparée à l’instruction DELETE, `TRUNCATE TABLE` offre les avantages suivants :  
  
-   Moindre espace du journal des transactions utilisé.  
  
     L'instruction DELETE supprime une ligne à la fois et enregistre une entrée dans le journal des transactions pour chaque ligne supprimée. `TRUNCATE TABLE` supprime les données en désallouant les pages de données utilisées pour stocker les données de la table et enregistre uniquement les désallocations de page dans le journal des transactions.  
  
-   Moins de verrous utilisés.  
  
     Lors de l'exécution de l'instruction DELETE à l'aide d'un verrou de ligne, chaque ligne dans la table est verrouillée pour être supprimée. `TRUNCATE TABLE` verrouille toujours la table (y compris un verrou de schéma SCH-M) et la page, mais pas les lignes.  
  
-   Sans exception, les pages zéros demeurent dans la table.  
  
     Après l'exécution d'une instruction DELETE, la table peut encore contenir des pages vides. Par exemple, des pages vides dans un segment ne peuvent pas être désallouées sans au moins un verrou de table exclusif (LCK_M_X). Si la suppression n'utilise pas de verrou de table, la table (le segment) contiendra beaucoup de pages vides. La suppression peut laisser des pages vides pour les index, bien que ces pages soient désallouées rapidement par un processus de nettoyage en arrière-plan.  
  
 `TRUNCATE TABLE` supprime toutes les lignes d’une table, mais la structure de la table et ses colonnes, contraintes, index, etc. sont conservés. Pour supprimer la définition de la table en plus de ses données, utilisez l’instruction `DROP TABLE`.  
  
 Si la table contient une colonne d'identité, le compteur pour celle-ci est redéfini sur sa valeur de départ. Si aucune valeur de départ n'est définie, la valeur par défaut 1 est utilisée. Utilisez plutôt l'instruction DELETE pour conserver le compteur d'identité.  
 
 > [!NOTE]
 > Une opération `TRUNCATE TABLE` peut être restaurée.
  
## <a name="restrictions"></a>Restrictions  
 Vous ne pouvez pas utiliser `TRUNCATE TABLE` sur des tables qui :  
  
-   sont référencées par une contrainte FOREIGN KEY ; Vous pouvez tronquer une table qui a une clé étrangère qui fait référence à elle-même. 
  
-   participent à une vue indexée ;  
  
-   sont publiées à l'aide d'une réplication transactionnelle ou de fusion.  

-   sont temporelles avec versions gérées ;

-   sont référencées par une contrainte EDGE.  
  
 Utilisez plutôt l'instruction DELETE pour les tables ayant au moins une de ces caractéristiques.  
  
 L'instruction TRUNCATE TABLE ne peut pas activer un déclencheur car l'opération ne consigne pas les suppressions de lignes dans le journal. Pour plus d’informations, consultez [CREATE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/create-trigger-transact-sql.md). 
 
 Dans [!INCLUDE[sssdwfull](../../includes/sssdwfull-md.md)] et [!INCLUDE[sspdw](../../includes/sspdw-md.md)] :

- L’instruction `TRUNCATE TABLE` n’est pas autorisée dans l’instruction EXPLAIN.

- L’instruction `TRUNCATE TABLE` ne peut pas être exécutée à l’intérieur d’une transaction.
  
## <a name="truncating-large-tables"></a>Troncation de grandes tables  
 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] permet de supprimer ou de tronquer des tables dotées de plus de 128 extensions, sans maintenir de verrous simultanés sur toutes les extensions exigées pour la suppression.  
  
## <a name="permissions"></a>Autorisations  
 L’autorisation minimale exigée est `ALTER` sur *table_name*. Les autorisations `TRUNCATE TABLE` sont accordées par défaut au propriétaire de la table, aux membres du rôle serveur fixe `sysadmin`, aux rôles de base de données fixes `db_owner` et `db_ddladmin` et ne sont pas transférables. Toutefois, vous pouvez incorporer l’instruction `TRUNCATE TABLE` dans un module, par exemple une procédure stockée, et accorder les autorisations appropriées au module à l’aide de la clause `EXECUTE AS`.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-truncate-a-table"></a>R. Tronquer une table  
 L'exemple suivant supprime toutes les données de la table `JobCandidate`. Des instructions `SELECT` sont incluses avant et après l'instruction `TRUNCATE TABLE` pour comparer les résultats.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT COUNT(*) AS BeforeTruncateCount   
FROM HumanResources.JobCandidate;  
GO  
TRUNCATE TABLE HumanResources.JobCandidate;  
GO  
SELECT COUNT(*) AS AfterTruncateCount   
FROM HumanResources.JobCandidate;  
GO  
```  
  
### <a name="b-truncate-table-partitions"></a>B. Tronquer les partitions de table  
  
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (de [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] à la [version actuelle](../../sql-server/what-s-new-in-sql-server-2016.md)).
  
 L'exemple suivant tronque les partitions spécifiées d'une table partitionnée. La syntaxe `WITH (PARTITIONS (2, 4, 6 TO 8))` provoque la troncation des numéros de partitions 2, 4, 6, 7 et 8.  
  
```sql  
TRUNCATE TABLE PartitionTable1   
WITH (PARTITIONS (2, 4, 6 TO 8));  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [DELETE &#40;Transact-SQL&#41;](../../t-sql/statements/delete-transact-sql.md)   
 [DROP TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-table-transact-sql.md)   
 [IDENTITY &#40;propriété&#41; &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql-identity-property.md)  
