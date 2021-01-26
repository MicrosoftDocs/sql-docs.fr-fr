---
description: DBCC INDEXDEFRAG (Transact-SQL)
title: DBCC INDEXDEFRAG (Transact-SQL)
ms.custom: ''
ms.date: 07/16/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- INDEXDEFRAG
- DBCC INDEXDEFRAG
- DBCC_INDEXDEFRAG_TSQL
- INDEXDEFRAG_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- defragmenting indexes
- DBCC INDEXDEFRAG statement
- leaf level defragmenting
- fragmentation [SQL Server]
- index defragmenting [SQL Server]
ms.assetid: 3c7df676-4843-44d0-8c1c-a9ab7e593b70
author: pmasl
ms.author: umajay
ms.openlocfilehash: 7bc27698a8ddb0cffe294a9caf07ec2b307961fe
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596637"
---
# <a name="dbcc-indexdefrag-transact-sql"></a>DBCC INDEXDEFRAG (Transact-SQL)

[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Défragmente les index de la table ou de la vue spécifiée.
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)] Utilisez à la place [ALTER INDEX](../../t-sql/statements/alter-index-transact-sql.md).  
  
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (de [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] à la [version actuelle](../../sql-server/what-s-new-in-sql-server-2016.md)).
  
![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
DBCC INDEXDEFRAG  
(  
    { database_name | database_id | 0 }   
    , { table_name | table_id | view_name | view_id }   
    [ , { index_name | index_id } [ , { partition_number | 0 } ] ]  
)  
    [ WITH NO_INFOMSGS ]   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *database_name* \| *database_id* \| 0  
 Base de données contenant l'index à défragmenter. Si 0 est spécifié, la base de données active est utilisée. Les noms de base de données doivent suivre les règles applicables aux [identificateurs](../../relational-databases/databases/database-identifiers.md).  
  
 *table_name* \| *table_id* \| *view_name* \| *view_id*  
 Table ou vue contenant l'index à défragmenter. Les noms des tables et des vues doivent suivre les règles applicables aux identificateurs.  
  
 *index_name* \| *index_id*  
 Nom ou ID de l'index à défragmenter. Si aucun ID n'est spécifié, l'instruction défragmente tous les index pour la table ou la vue indiquées. Les noms d'index doivent respecter les règles applicables aux identificateurs.  
  
 *partition_number* \| 0  
 Numéro de la partition de l'index à défragmenter. S'il n'est pas spécifié ou si la valeur 0 est spécifié, l'instruction défragmente toutes les partitions dans l'index indiqué.  
  
 WITH NO_INFOMSGS  
 Supprime tous les messages d'information dont les niveaux de gravité sont compris entre 0 et 10.  
  
## <a name="remarks"></a>Notes  
DBCC INDEXDEFRAG défragmente un index au niveau feuille afin que l'ordre physique des pages corresponde à l'ordre logique (de gauche à droite) des nœuds feuilles, améliorant ainsi les performances d'analyse de l'index.
  
> [!NOTE]  
>  Lors de l'exécution de DBCC INDEXDEFRAG, la défragmentation de l'index se produit par séries. Cela signifie que l'opération est effectuée sur un seul index à l'aide d'un thread unique. Il n'y a aucun parallélisme. En outre, les opérations sur plusieurs index sont effectuées à partir de la même instruction DBCC INDEXDEFRAG, sur un index à la fois.  
  
DBCC INDEXDEFRAG compacte également les pages d'un index, en tenant compte du facteur de remplissage spécifié lors de la création de l'index. Toute page vide issue de ce compactage est supprimée. Pour plus d’informations, consultez [Spécifier un facteur de remplissage pour un index](../../relational-databases/indexes/specify-fill-factor-for-an-index.md).
  
Si un index s'étend sur plusieurs fichiers, DBCC INDEXDEFRAG défragmente un fichier à la fois. Les pages ne migrent pas d'un fichier à l'autre.
  
Toutes les cinq minutes, DBCC INDEXDEFRAG fait une estimation du pourcentage d'achèvement dans un rapport. DBCC INDEXDEFRAG peut être arrêté à tout moment et n'importe quel travail alors achevé sera conservé.
  
À l'inverse de DBCC DBREINDEX ou de la génération d'index en règle générale, DBCC INDEXDEFRAG est une opération en ligne. En effet, les verrous ne sont pas maintenus à long terme. Ainsi, DBCC INDEXDEFRAG ne bloque pas les requêtes ou les mises à jour en cours d'exécution. Un index relativement non fragmenté peut être défragmenté plus rapidement que la construction d'un nouvel index, car le temps de défragmentation dépend du volume de fragmentation. Un index très fragmenté peut être sensiblement plus long à défragmenter qu'à reconstruire.
  
La défragmentation est toujours complètement enregistrée, quel que soit le paramètre du mode de récupération de la base de données. Pour plus d’informations, consultez [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md). La défragmentation d'un index très fragmenté peut générer un journal plus important que la création d'un index complètement enregistré. Toutefois, la défragmentation s'effectue sous la forme d'une série de transactions courtes et ne requiert donc pas un journal volumineux si des sauvegardes de fichier journal sont effectuées fréquemment ou que le paramètre du mode de récupération est SIMPLE.
  
## <a name="restrictions"></a>Restrictions  
DBCC INDEXDEFRAG mélange les pages feuilles de l'index en place. Ainsi, si un index est entrelacé avec d'autres sur le disque, l'exécution de DBCC INDEXDEFRAG sur cet index ne rend pas toutes ses pages feuilles contiguës. Pour améliorer le clustering des pages, régénérez l'index.
DBCC INDEXDEFRAG ne peut pas être utilisé pour défragmenter les index suivants :
-   un index désactivé ;  
-   un index dont le verrouillage de page est désactivé (OFF) ;  
-   un index spatial.  
  
DBCC INDEXDEFRAG n'est pas géré pour une utilisation sur les tables système.
  
## <a name="result-sets"></a>Jeux de résultats  
DBCC INDEXDEFRAG retourne l'ensemble de résultats suivant (les valeurs peuvent varier) si un index est spécifié dans une instruction (sauf si WITH NO_INFOMSGS est défini) :
  
```
Pages Scanned Pages Moved Pages Removed  
------------- ----------- -------------  
359           346         8  
  
(1 row(s) affected)  
  
DBCC execution completed. If DBCC printed error messages, contact your system administrator.  
```  
  
## <a name="permissions"></a>Autorisations  
L’appelant doit être propriétaire de la table ou être membre du rôle serveur fixe **sysadmin**, du rôle de base de données fixe **db_owner** ou du rôle de base de données fixe **db_ddladmin**.
  
## <a name="examples"></a>Exemples  
### <a name="a-using-dbcc-indexdefrag-to-defragment-an-index"></a>R. Utilisation de DBCC INDEXDEFRAG pour défragmenter un index  
L’exemple suivant défragmente toutes les partitions de l’index `PK_Product_ProductID` dans la table `Production.Product` de la base de données `AdventureWorks`.
  
```sql  
DBCC INDEXDEFRAG (AdventureWorks2012, 'Production.Product', PK_Product_ProductID);  
GO  
```  
  
### <a name="b-using-dbcc-showcontig-and-dbcc-indexdefrag-to-defragment-the-indexes-in-a-database"></a>B. Utilisation de DBCC SHOWCONTIG et de DBCC INDEXDEFRAG pour défragmenter les index d'une base de données  
 L'exemple suivant illustre une méthode simple de défragmentation de tous les index d'une base de données fragmentés au-delà d'un seuil déclaré.  
  
```sql  
/*Perform a 'USE <database name>' to select the database in which to run the script.*/  
-- Declare variables  
SET NOCOUNT ON;  
DECLARE @tablename VARCHAR(255);  
DECLARE @execstr   VARCHAR(400);  
DECLARE @objectid  INT;  
DECLARE @indexid   INT;  
DECLARE @frag      DECIMAL;  
DECLARE @maxfrag   DECIMAL;  
  
-- Decide on the maximum fragmentation to allow for.  
SELECT @maxfrag = 30.0;  
  
-- Declare a cursor.  
DECLARE tables CURSOR FOR  
   SELECT TABLE_SCHEMA + '.' + TABLE_NAME  
   FROM INFORMATION_SCHEMA.TABLES  
   WHERE TABLE_TYPE = 'BASE TABLE';  
  
-- Create the table.  
CREATE TABLE #fraglist (  
   ObjectName CHAR(255),  
   ObjectId INT,  
   IndexName CHAR(255),  
   IndexId INT,  
   Lvl INT,  
   CountPages INT,  
   CountRows INT,  
   MinRecSize INT,  
   MaxRecSize INT,  
   AvgRecSize INT,  
   ForRecCount INT,  
   Extents INT,  
   ExtentSwitches INT,  
   AvgFreeBytes INT,  
   AvgPageDensity INT,  
   ScanDensity DECIMAL,  
   BestCount INT,  
   ActualCount INT,  
   LogicalFrag DECIMAL,  
   ExtentFrag DECIMAL);  
  
-- Open the cursor.  
OPEN tables;  
  
-- Loop through all the tables in the database.  
FETCH NEXT  
   FROM tables  
   INTO @tablename;  
  
WHILE @@FETCH_STATUS = 0  
BEGIN  
-- Do the showcontig of all indexes of the table  
   INSERT INTO #fraglist   
   EXEC ('DBCC SHOWCONTIG (''' + @tablename + ''')   
      WITH FAST, TABLERESULTS, ALL_INDEXES, NO_INFOMSGS');  
   FETCH NEXT  
      FROM tables  
      INTO @tablename;  
END;  
  
-- Close and deallocate the cursor.  
CLOSE tables;  
DEALLOCATE tables;  
  
-- Declare the cursor for the list of indexes to be defragged.  
DECLARE indexes CURSOR FOR  
   SELECT ObjectName, ObjectId, IndexId, LogicalFrag  
   FROM #fraglist  
   WHERE LogicalFrag >= @maxfrag  
      AND INDEXPROPERTY (ObjectId, IndexName, 'IndexDepth') > 0;  
  
-- Open the cursor.  
OPEN indexes;  
  
-- Loop through the indexes.  
FETCH NEXT  
   FROM indexes  
   INTO @tablename, @objectid, @indexid, @frag;  
  
WHILE @@FETCH_STATUS = 0  
BEGIN  
   PRINT 'Executing DBCC INDEXDEFRAG (0, ' + RTRIM(@tablename) + ',  
      ' + RTRIM(@indexid) + ') - fragmentation currently '  
       + RTRIM(CONVERT(varchar(15),@frag)) + '%';  
   SELECT @execstr = 'DBCC INDEXDEFRAG (0, ' + RTRIM(@objectid) + ',  
       ' + RTRIM(@indexid) + ')';  
   EXEC (@execstr);  
  
   FETCH NEXT  
      FROM indexes  
      INTO @tablename, @objectid, @indexid, @frag;  
END;  
  
-- Close and deallocate the cursor.  
CLOSE indexes;  
DEALLOCATE indexes;  
  
-- Delete the temporary table.  
DROP TABLE #fraglist;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
[DBCC &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-transact-sql.md)  
[sys.dm_db_index_physical_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-physical-stats-transact-sql.md)  
[CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)  
[ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)  
[ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md)
  
