---
description: DBCC SHOWCONTIG (Transact-SQL)
title: DBCC SHOWCONTIG (Transact-SQL)
ms.custom: ''
ms.date: 07/17/2017
ms.prod: sql
ms.prod_service: sql-database
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DBCC_SHOWCONTIG_TSQL
- DBCC SHOWCONTIG
- SHOWCONTIG
- SHOWCONTIG_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- displaying defragmentation information
- DBCC SHOWCONTIG statement
- defragmenting indexes
- leaf level defragmenting
- fragmentation [SQL Server]
- index defragmenting [SQL Server]
ms.assetid: 1df2123a-1197-4fff-91a3-25e3d8848aaa
author: pmasl
ms.author: umajay
ms.openlocfilehash: ed1ba4d3fb6114a74dbf972ff0a04f84a463af06
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/20/2021
ms.locfileid: "98597085"
---
# <a name="dbcc-showcontig-transact-sql"></a>DBCC SHOWCONTIG (Transact-SQL)

[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Affiche les informations de fragmentation pour les données et les index de la table ou vue spécifiée.
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)] Utilisez [sys.dm_db_index_physical_stats](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-physical-stats-transact-sql.md) à la place.  
  
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (de [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] à la [version actuelle](../../sql-server/what-s-new-in-sql-server-2016.md)).
  
![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
DBCC SHOWCONTIG   
[ (   
    { table_name | table_id | view_name | view_id }   
    [ , index_name | index_id ]   
) ]   
    [ WITH   
        {   
         [ , [ ALL_INDEXES ] ]   
         [ , [ TABLERESULTS ] ]   
         [ , [ FAST ] ]  
         [ , [ ALL_LEVELS ] ]   
         [ NO_INFOMSGS ]  
         }  
    ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *table_name* \| *table_id* \| *view_name* \| *view_id*  
 Table ou vue dont les informations de fragmentation doivent être vérifiées. Sans aucune précision, toutes les tables et les vues indexées de la base de données active sont contrôlées. Pour obtenir l’ID de la table ou de la vue, utilisez la fonction [OBJECT_ID](../../t-sql/functions/object-id-transact-sql.md).  
  
 *index_name* \| *index_id*  
 Index dont les informations de fragmentation doivent être vérifiées. Si aucune option n'est spécifiée, l'instruction traite l'index de base de la table ou de la vue indiquée. Pour obtenir l’ID de l’index, utilisez l’affichage catalogue [sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md).  
  
 WITH  
 Spécifie les options relatives au type d'informations renvoyées par l'instruction DBCC.  
  
 FAST  
 Indique si une analyse rapide de l'index doit être effectuée et un minimum d'informations renvoyées. Une analyse rapide ne lit pas les pages des niveaux feuille ou données de l'index.  
  
 ALL_INDEXES  
 Affiche les résultats de tous les index des tables et vues spécifiées, même si un index particulier est spécifié.  
  
 TABLERESULTS  
 Affiche les résultats sous la forme d'un ensemble de lignes, avec des informations complémentaires.  
  
 ALL_LEVELS  
 Conservé pour compatibilité descendante uniquement. Même si ALL_LEVELS est spécifié, seul le niveau feuille de l'index ou le niveau données de la table est traité.  
  
 NO_INFOMSGS  
 Supprime tous les messages d'information dont les niveaux de gravité sont compris entre 0 et 10.  
  
## <a name="result-sets"></a>Jeux de résultats  
Le tableau suivant décrit les informations du jeu de résultats.
  
|Statistique|Description|  
|---|---|
|**Pages analysées**|Nombre de pages dans la table ou l'index.|  
|**Extensions analysées**|Nombre d'extensions dans la table ou l'index|  
|**Commutateurs d’extension**|Nombre de fois où l'instruction DBCC est passée d'une extension à l'autre en parcourant les pages de l'index ou de la table.|  
|**Durée Moyenne des pages par extension**|Nombre de pages par extension dans la chaîne de pages.|  
|**Densité d’analyse [Meilleur nombre: Nombre réel]**|Il s'agit d'un pourcentage. Il représente le ratio **Meilleur nombre** sur **Nombre réel**. Cette valeur est de 100 si tout est contigu et elle est inférieure à 100 si certaines fragmentations existent.<br /><br /> **Meilleur nombre** correspond au nombre idéal de modifications d’extension si tout est lié en contigu. **Nombre réel** est le nombre effectif de modifications d’extension.|  
|**Fragmentation d’analyse logique**|Pourcentage de pages hors service renvoyées après l'analyse des pages de feuilles d'un index. Cette valeur n'est pas pertinente pour les segments. Une page non ordonnée est une page pour laquelle la page physique suivante allouée à l’index n’est pas la page désignée par le pointeur de pag *e* suivante dans la page feuille actuelle.|  
|**Fragmentation d’analyse d’extension**|Pourcentage d'extensions déclassées lors de l'analyse des pages feuilles d'un index. Cette valeur n'est pas pertinente pour les segments. Une extension hors service est une extension qui contient la page active pour un index, mais n'est pas physiquement l'extension qui suit celle contenant la page précédente pour un index.<br /><br /> Remarque : Ce nombre n’a aucune signification lorsque l’index s’étend sur plusieurs fichiers.|  
|**Durée Moyenne d’octets libres par page**|Nombre moyen d'octets libres sur les pages analysées. Plus le nombre est élevé et moins les pages sont remplies. Les faibles valeurs sont préférables si l'index ne doit pas recevoir beaucoup d'insertions aléatoires. Ce nombre est également affecté par la taille des lignes : plus elle sera élevée, plus le nombre le sera également.|  
|**Durée Densité de page moyenne (complète)**|Densité de page moyenne (pourcentage). Cette valeur prend en compte la taille des lignes. C'est donc une indication plus précise sur le remplissage de vos pages. Plus le pourcentage est élevé et mieux c'est.|  
  
Quand *table_id* et FAST sont spécifiés, DBCC SHOWCONTIG retourne un jeu de résultats contenant uniquement les colonnes suivantes.
-   **Pages analysées**  
-   **Commutateurs d’extension**  
-   **Densité d’analyse [Meilleur nombre:Nombre réel]**  
-   **Fragmentation d’analyse d’extension**  
-   **Fragmentation d’analyse logique**  
  
Lorsque TABLERESULTS est spécifié, DBCC SHOWCONTIG renvoie les colonnes suivantes et également les neuf colonnes décrites dans la table précédente.
  
|Statistique|Description|  
|---|---|
|**Nom de l’objet**|Nom de la table ou de la vue traitée.|  
|**ObjectId**|ID du nom d'objet.|  
|**IndexName**|Nom de l'index traité. A la valeur NULL pour un segment.|  
|**IndexId**|Identificateur de l'index. A la valeur 0 pour un segment.|  
|**Niveau**|Niveau de l'index. Le niveau 0 correspond au niveau feuille ou données de l'index.<br /><br /> Un segment a le niveau 0.|  
|**Pages**|Nombre de pages constituant ce niveau d'index ou segment entier.|  
|**Lignes**|Nombre d'enregistrements de données ou d'index situés à ce niveau de l'index. Pour un segment, cette valeur représente le nombre d'enregistrements de données dans le segment entier.<br /><br /> Pour un segment de mémoire, le nombre d'enregistrements retournés par cette fonction peut ne pas correspondre au nombre des lignes retournées en exécutant une requête SELECT COUNT(*) sur le segment. Cela est dû au fait qu'une ligne peut contenir plusieurs enregistrements. Par exemple, lors de certaines mises à jour, une ligne de segment unique peut comporter un enregistrement de transfert et un enregistrement transféré suite à l'opération de mise à jour. Par ailleurs, la plupart des lignes LOB de grande taille sont fractionnées en plusieurs enregistrements dans le stockage LOB_DATA.|  
|**MinimumRecordSize**|Taille minimum des enregistrements dans ce niveau d'index ou le segment entier.|  
|**MaximumRecordSize**|Taille maximum des enregistrements dans ce niveau d'index ou le segment entier.|  
|**AverageRecordSize**|Taille moyenne des enregistrements dans ce niveau d'index ou le segment entier.|  
|**ForwardedRecords**|Nombre d'enregistrements transférés dans ce niveau d'index ou le segment entier.|  
|**Extents**|Nombre d'extensions dans ce niveau d'index ou le segment entier.|  
|**ExtentSwitches**|Nombre de fois où l'instruction DBCC est passée d'une extension à l'autre en parcourant les pages de l'index ou de la table.|  
|**AverageFreeBytes**|Nombre moyen d'octets libres sur les pages analysées. Plus le nombre est élevé et moins les pages sont remplies. Les faibles valeurs sont préférables si l'index ne doit pas recevoir beaucoup d'insertions aléatoires. Ce nombre est également affecté par la taille des lignes : plus elle sera élevée, plus le nombre le sera également.|  
|**AveragePageDensity**|Densité de page moyenne (pourcentage). Cette valeur prend en compte la taille des lignes. C'est donc une indication plus précise sur le remplissage de vos pages. Plus le pourcentage est élevé et mieux c'est.|  
|**ScanDensity**|Il s'agit d'un pourcentage. Il représente le ratio **BestCount** sur **ActualCount**. Cette valeur est de 100 si tout est contigu et elle est inférieure à 100 si certaines fragmentations existent.|  
|**BestCount**|Il s'agit du nombre idéal de modifications d'extension si tout est lié en contigu.|  
|**ActualCount**|C'est le nombre effectif de modifications d'extension.|  
|**LogicalFragmentation**|Pourcentage de pages hors service renvoyées après l'analyse des pages de feuilles d'un index. Cette valeur n'est pas pertinente pour les segments. Une page non ordonnée est une page pour laquelle la page physique suivante allouée à l’index n’est pas la page désignée par le pointeur de pag *e* suivante dans la page feuille actuelle.|  
|**ExtentFragmentation**|Pourcentage d'extensions déclassées lors de l'analyse des pages feuilles d'un index. Cette valeur n'est pas pertinente pour les segments. Une extension hors service est une extension qui contient la page active pour un index, mais n'est pas physiquement l'extension qui suit celle contenant la page précédente pour un index.<br /><br /> Remarque : Ce nombre n’a aucune signification lorsque l’index s’étend sur plusieurs fichiers.|  
  
Lorsque WITH TABLERESULTS et FAST sont spécifiés, l'ensemble de résultats est le même que lorsque WITH TABLERESULTS est spécifié, excepté que les colonnes suivantes auront des valeurs NULL :

| Lignes| Étendues |
|---|---|
|**MinimumRecordSize**|**AverageFreeBytes**|  
|**MaximumRecordSize**|**AveragePageDensity**|  
|**AverageRecordSize**|**ExtentFragmentation**|  
|**ForwardedRecords**||  
  
## <a name="remarks"></a>Notes  
L’instruction DBCC SHOWCONTIG parcourt la chaîne de la page au niveau feuille de l’index spécifié quand *index_id* est précisé. Si seule la valeur de *table_id* est précisée ou si la valeur de *index_id* correspond à 0, les pages de données de la table indiquée sont analysées. L'opération ne nécessite qu'un verrou de table intent-partagé. De cette manière, toutes les mises à jour et insertions peuvent être effectuées, sauf celles nécessitant un verrou de table exclusif (X). Cela permet un compromis entre la vitesse d'exécution et aucune réduction de la concurrence avec le nombre de statistiques renvoyées. Cependant, si la commande n'est utilisée que pour évaluer la fragmentation, nous vous recommandons d'appliquer l'option WITH FAST pour des performances optimales. Une analyse rapide ne lit pas les pages des niveaux feuille ou données de l'index. L'option WITH FAST ne s'applique pas à un segment.
  
## <a name="restrictions"></a>Restrictions  
DBCC SHOWCONTIG n’affiche pas les données de type **ntext**, **text** et **image**. Cela est dû au fait que les index de texte qui stockent des données de texte et d'image n'existent plus.
  
En outre, DBCC SHOWCONTIG ne prend pas en charge certaines fonctionnalités nouvelles. Par exemple :
-   Si la table ou l'index spécifié est partitionné, DBCC SHOWCONTIG n'affiche que la première partition de la table ou de l'index spécifié.  
-   DBCC SHOWCONTIG n’affiche pas d’informations de stockage de dépassement de lignes et d’autres types nouveaux de données hors ligne tels que **nvarchar(max)** , **varchar(max)** , **varbinary(max)** et **xml**.  
-   Les index spatiaux ne sont pas pris en charge par DBCC SHOWCONTIG.  
  
Toutes les nouvelles fonctionnalités sont entièrement prises en charge par la vue de gestion dynamique [sys.dm_db_index_physical_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-physical-stats-transact-sql.md).
  
## <a name="table-fragmentation"></a>Fragmentation de la table  
DBCC SHOWCONTIG détermine si la table est fragmentée de manière importante ou non. La fragmentation de la table a lieu lors du processus de modification de données (instructions INSERT, UPDATE et DELETE) effectuées sur la table. Comme ces modifications ne sont pas généralement distribuées équitablement entre les lignes de la table, le remplissage de chaque page peut varier dans le temps. Pour les requêtes qui balaient une partie ou la totalité d'une table, une telle fragmentation de table peut provoquer des lectures de page supplémentaires. Cela perturbe l'analyse parallèle des données.
  
Lorsqu'un index est très fragmenté, vous disposez des méthodes alternatives suivantes pour réduire la fragmentation :
-   Supprimez puis créez de nouveau un index cluster.  
     La nouvelle création d'un index cluster permet de réorganiser les données, ce qui entraîne des pages de données remplies entièrement. Vous pouvez configurer le niveau de remplissage à l'aide de l'option FILLFACTOR de l'instruction CREATE INDEX. Cette méthode présente deux inconvénients : l'index est en mode hors connexion pendant la phase de suppression et de recréation, et l'opération est atomique. Si la création de l'index est interrompue, l'index n'est pas recréé.  
-   Réorganisez les pages de niveau feuille de l'index selon un ordre logique.  
     Utilisez l’instruction ALTER INDEX...REORGANIZE pour réorganiser les pages de niveau feuille de l’index selon un ordre logique. Comme il s'agit d'une opération en ligne, l'index est disponible lorsque l'instruction est exécutée. L'interruption de cette opération entraîne la perte du travail effectué. L'inconvénient de cette méthode est que la réorganisation des données est moins efficace que celle obtenue par l'opération de suppression et de recréation d'un index cluster.  
-   Reconstruisez l'index.  
     Utilisez ALTER INDEX avec REBUILD pour reconstruire l'index. Pour plus d’informations, consultez [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md).  
  
Les statistiques **Moyenne d’octets libres par page** et **Densité de page moyenne (complète)** dans le jeu de résultats indiquent le remplissage des pages d’index. Les statistiques **Moyenne d’octets libres par page** doit être faible et celle de **Densité de page moyenne (complète)** doit être élevée pour un index qui ne recevra pas beaucoup d’insertions aléatoires. La suppression et la recréation d'un index avec l'option FILLFACTOR spécifiée peut améliorer ces statistiques. Par ailleurs, la commande ALTER INDEX avec REORGANIZE comprimera un index en tenant compte de son option FILLFACTOR, ce qui améliore les statistiques.
  
> [!NOTE]  
>  Un index possédant de nombreuses insertions aléatoires et des pages très remplies aura un nombre accru de fractionnements de page. Cela implique une fragmentation plus importante.  
  
Le niveau de fragmentation d'un index peut être déterminé des manières suivantes :
-   En comparant les valeurs **Commutateurs d’extension** et **Extensions analysées**.  
     La valeur **Commutateurs d’extension** doit être la plus proche possible de celle **d’Extensions analysées**. Ce ratio est calculé comme la valeur de **Densité d’analyse**. Cette valeur doit être aussi élevée que possible et peut être améliorée en réduisant la fragmentation de l'index.  
  
    > [!NOTE]  
    >  Cette méthode ne fonctionne pas si l'index concerne un grand nombre de fichiers.  
  
-   En comprenant les valeurs **Fragmentation d’analyse logique** et **Fragmentation d’analyse d’extension**.  
     Les valeurs **Fragmentation d’analyse logique** et, dans une proportion moindre, **Fragmentation d’analyse d’extension** fournissent la meilleure indication du niveau de fragmentation d’une table. Ces deux valeurs doivent être le plus proche possible de zéro. Toutefois, une valeur de 0 à 10 % est tolérée.  
  
    > [!NOTE]  
    >  La valeur **Fragmentation d’analyse d’extension** est élevée si l’index s’étend sur plusieurs fichiers. Pour réduire ces valeurs, vous devez réduire la fragmentation de l'index.  
  
## <a name="permissions"></a>Autorisations  
L’utilisateur doit être propriétaire de la table ou être membre du rôle serveur fixe **sysadmin**, du rôle de base de données fixe **db_owner** ou du rôle de base de données fixe **db_ddladmin**.
  
## <a name="examples"></a>Exemples  
### <a name="a-displaying-fragmentation-information-for-a-table"></a>R. Affichage des informations de fragmentation d'une table  
L'exemple suivant affiche les informations de fragmentation de la table `Employee`.
  
```sql  
USE AdventureWorks2012;  
GO  
DBCC SHOWCONTIG ('HumanResources.Employee');  
GO  
```  
  
### <a name="b-using-object_id-to-obtain-the-table-id-and-sysindexes-to-obtain-the-index-id"></a>B. Utilisation de OBJECT_ID pour obtenir l'ID de table et sys.indexes pour obtenir l'ID d'index  
L’exemple suivant utilise `OBJECT_ID` et l’affichage catalogue `sys.indexes` pour obtenir l’ID de table et l’ID d’index de l’index `AK_Product_Name` de la table `Production.Product` de la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].
  
```sql  
USE AdventureWorks2012;  
GO  
DECLARE @id INT, @indid INT  
SET @id = OBJECT_ID('Production.Product')  
SELECT @indid = index_id   
FROM sys.indexes  
WHERE object_id = @id   
   AND name = 'AK_Product_Name'  
DBCC SHOWCONTIG (@id, @indid);  
GO  
```  
  
### <a name="c-displaying-an-abbreviated-result-set-for-a-table"></a>C. Affichage d'un jeu de résultats abrégés pour une table  
L’exemple suivant retourne un jeu de résultats abrégé pour la table `Product` dans la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].
  
```sql  
USE AdventureWorks2012;  
GO  
DBCC SHOWCONTIG ('Production.Product', 1) WITH FAST;  
GO  
```  
  
### <a name="d-displaying-the-full-result-set-for-every-index-on-every-table-in-a-database"></a>D. Affichage du jeu de résultats complet pour chaque index de chaque table dans une base de données  
L'exemple suivant retourne un jeu de résultats de table entière pour chaque index de chaque table de la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].
  
```sql  
USE AdventureWorks2012;  
GO  
DBCC SHOWCONTIG WITH TABLERESULTS, ALL_INDEXES;  
GO  
```  
  
### <a name="e-using-dbcc-showcontig-and-dbcc-indexdefrag-to-defragment-the-indexes-in-a-database"></a>E. Utilisation de DBCC SHOWCONTIG et de DBCC INDEXDEFRAG pour défragmenter les index d'une base de données  
L'exemple suivant illustre une méthode simple de défragmentation de tous les index d'une base de données fragmentée au-delà d'un seuil déclaré.
  
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
[ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md)  
[CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md)  
[DBCC &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-transact-sql.md)  
[DROP INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/drop-index-transact-sql.md)  
[sys.dm_db_index_physical_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-physical-stats-transact-sql.md)  
[OBJECT_ID &#40;Transact-SQL&#41;](../../t-sql/functions/object-id-transact-sql.md)  
[sys.indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)
  
