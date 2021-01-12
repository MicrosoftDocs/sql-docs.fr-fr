---
description: DROP INDEX (Transact-SQL)
title: DROP INDEX (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/11/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DROP_INDEX_TSQL
- DROP INDEX
dev_langs:
- TSQL
helpviewer_keywords:
- nonclustered indexes [SQL Server], removing
- MAXDOP index option, DROP INDEX statement
- index removal [SQL Server]
- spatial indexes [SQL Server], dropping
- removing indexes
- deleting indexes
- dropping indexes
- MOVE TO clause
- clustered indexes, removing
- indexes [SQL Server], dropping
- filtered indexes [SQL Server], dropping
- ONLINE option
- indexes [SQL Server], moving
- XML indexes [SQL Server], dropping
- DROP INDEX statement
ms.assetid: 2b1464c8-934c-405f-8ef7-2949346b5372
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 4e6da2b809301b38c4d0a60b82c39bd9e3901641
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100801"
---
# <a name="drop-index-transact-sql"></a>DROP INDEX (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Supprime un ou plusieurs index relationnels, spatiaux ou XML de la base de données active. Vous pouvez supprimer un index cluster et déplacer la table résultante vers un autre groupe de fichiers ou schéma de partition dans une transaction unique en spécifiant l'option MOVE TO.  
  
 L'instruction DROP INDEX ne s'applique pas aux index créés en définissant des contraintes PRIMARY KEY ou UNIQUE. Pour supprimer la contrainte et l’index correspondant, utilisez [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md) avec la clause DROP CONSTRAINT.  
  
> [!IMPORTANT]
>  La syntaxe définie dans `<drop_backward_compatible_index>` sera supprimée dans une version ultérieure de [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Évitez d'utiliser cette syntaxe dans de nouveaux travaux de développement et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Utilisez plutôt la syntaxe spécifiée sous `<drop_relational_or_xml_index>`. Les index XML ne peuvent pas être supprimés à l'aide d'une syntaxe à compatibilité descendante.  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
-- Syntax for SQL Server (All options except filegroup and filestream apply to Azure SQL Database.)  
  
DROP INDEX [ IF EXISTS ]   
{ <drop_relational_or_xml_or_spatial_index> [ ,...n ]   
| <drop_backward_compatible_index> [ ,...n ]  
}  
  
<drop_relational_or_xml_or_spatial_index> ::=  
    index_name ON <object>   
    [ WITH ( <drop_clustered_index_option> [ ,...n ] ) ]  
  
<drop_backward_compatible_index> ::=  
    [ owner_name. ] table_or_view_name.index_name  
  
<object> ::=  
{ database_name.schema_name.table_or_view_name | schema_name.table_or_view_name | table_or_view_name }  
  
<drop_clustered_index_option> ::=  
{  
    MAXDOP = max_degree_of_parallelism  
  | ONLINE = { ON | OFF }  
  | MOVE TO { partition_scheme_name ( column_name )   
            | filegroup_name  
            | "default"   
            }  
  [ FILESTREAM_ON { partition_scheme_name   
            | filestream_filegroup_name   
            | "default" } ]  
}  
```  
  
```syntaxsql
-- Syntax for Azure SQL Database  
  
DROP INDEX  
{ <drop_relational_or_xml_or_spatial_index> [ ,...n ]   
}  
  
<drop_relational_or_xml_or_spatial_index> ::=   
    index_name ON <object>  
  
<object> ::=   
{ database_name.schema_name.table_or_view_name | schema_name.table_or_view_name | table_or_view_name }  
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
DROP INDEX index_name ON { database_name.schema_name.table_name | schema_name.table_name | table_name }  
[;]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *IF EXISTS*  
 **S’applique à**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (de [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] à la [version actuelle](https://go.microsoft.com/fwlink/p/?LinkId=299658)).  
  
 Supprime, de manière conditionnelle, l’index uniquement s’il existe déjà.  
  
 *index_name*  
 Nom de l'index à supprimer.  
  
 *database_name*  
 Nom de la base de données.  
  
 *schema_name*  
 Nom du schéma auquel la table ou la vue appartient.  
  
 *table_or_view_name*  
 Nom de la table ou de la vue associée à l'index. Les index spatiaux sont pris en charge uniquement dans les tables.  
  
 Pour afficher un rapport des index relatifs à un objet, utilisez la vue de catalogue [sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md).  
  
 Azure SQL Database prend en charge le format de nom en trois parties nom_bd.[nom_schéma].nom_objet lorsque nom_bd correspond à la base de données active ou lorsque nom_bd est tempdb et nom_objet commence par #.  
  
 \<drop_clustered_index_option>  
 **S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures, [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].  
  
 Contrôle les options d'index cluster. Ces options ne peuvent pas être utilisées avec d'autres types d'index.  
  
 MAXDOP = *max_degree_of_parallelism*  
 **S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures, [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] (niveaux de performance P2 et P3 uniquement).  
  
 Remplace l’option de configuration **max degree of parallelism** pendant la durée de l’opération d’index. Pour plus d’informations, consultez [Configurer l’option de configuration du serveur max degree of parallelism](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md). Utilisez MAXDOP pour limiter le nombre de processeurs utilisés dans une exécution de plan parallèle. Le nombre maximal de processeurs est égal à 64.  
  
> [!IMPORTANT]  
>  MAXDOP n'est pas autorisé pour les index XML ou spatiaux.  
  
 *max_degree_of_parallelism* peut avoir la valeur :  
  
 1  
 Supprime la création de plans parallèles.  
  
 \>1  
 Limite au nombre spécifié le nombre maximal de processeurs utilisés dans le traitement en parallèle des index.  
  
 0 (valeur par défaut)  
 Utilise le nombre réel de processeurs ou un nombre de processeurs inférieur en fonction de la charge de travail actuelle du système.  
  
 Pour plus d’informations, consultez [Configurer des opérations d’index parallèles](../../relational-databases/indexes/configure-parallel-index-operations.md).  
  
> [!NOTE]  
>  Les opérations d'index parallèles ne sont pas disponibles dans toutes les éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour obtenir la liste des fonctionnalités prises en charge par les éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez [Fonctionnalités prises en charge par les éditions de SQL Server 2016](../../sql-server/editions-and-supported-features-for-sql-server-2016.md).  
  
 ONLINE = ON | **OFF**  
 **S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].  
  
 Indique si les tables sous-jacentes et les index associés sont disponibles pour les requêtes et la modification de données pendant l'opération d'index. La valeur par défaut est OFF.  
  
 ACTIVÉ  
 Les verrous de table à long terme ne sont pas maintenus. Cela permet aux requêtes ou mises à jour de la table sous-jacente de continuer.  
  
 OFF  
 Les verrous de table sont appliqués et la table est indisponible pendant la durée de l'opération d'index.  
  
 L'option ONLINE ne peut être spécifiée que lorsque vous supprimez des index clusters. Pour plus d'informations, consultez la section Notes.  
  
> [!NOTE]  
>  Les opérations d'index en ligne ne sont pas disponibles dans toutes les éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour obtenir la liste des fonctionnalités prises en charge par les éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez [Fonctionnalités prises en charge par les éditions de SQL Server 2016](../../sql-server/editions-and-supported-features-for-sql-server-2016.md).  
  
 MOVE TO { _partition\_scheme\_name_ **(** _column\_name_ **)**  | _filegroup\_name_ |  **"** default **"**  
 **S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures. [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] prend en charge « default » comme nom de groupe de fichiers.  
  
 Spécifie un emplacement pour déplacer les lignes de données qui se trouvent actuellement au niveau feuille de l'index cluster. Les données sont déplacées vers le nouvel emplacement sous la forme d'un segment de mémoire. Vous pouvez spécifier un schéma de partition ou un groupe de fichiers déjà existants comme nouvel emplacement. MOVE TO n'est pas valide pour les vues non indexées ou les index non cluster. Si aucun schéma de partition ou groupe de fichiers n'est spécifié, la table résultante sera située sur le même schéma de partition ou groupe de fichiers que celui défini pour l'index cluster.  
  
 Si un index cluster est supprimé à l'aide de MOVE TO, tous les index non cluster sur la table de base sont recréés, mais ils restent dans leur schéma de partition ou groupe de fichiers d'origine. Si la table de base est déplacée vers un schéma de partition ou groupe de fichiers différent, les index non cluster ne sont pas déplacés pour coïncider avec le nouvel emplacement de la table de base (segment de mémoire). Par conséquent, même si les index non cluster étaient précédemment alignés avec l'index cluster, ils peuvent ne plus être alignés avec le segment de mémoire. Pour plus d’informations sur les index partitionnés, consultez [Index et tables partitionnés](../../relational-databases/partitions/partitioned-tables-and-indexes.md).  
  
 _partition_scheme_name_ **(** _column_name_ **)**  
 **S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures, [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].  
  
 Spécifie un schéma de partition comme emplacement de la table résultante. Le schéma de partition doit déjà avoir été créé en exécutant soit [CREATE PARTITION SCHEME](../../t-sql/statements/create-partition-scheme-transact-sql.md), soit [ALTER PARTITION SCHEME](../../t-sql/statements/alter-partition-scheme-transact-sql.md). Si aucun emplacement n'est spécifié et que la table est partitionnée, la table est incluse dans le même schéma de partition que l'index cluster existant.  
  
 Le nom de la colonne dans le schéma n'est pas limité aux colonnes dans la définition d'index. Toute colonne dans la table de base peut être spécifiée.  
  
 *filegroup_name*  
 **S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.  
  
 Spécifie un groupe de fichiers comme emplacement de la table résultante. Si aucun emplacement n'est spécifié et que la table n'est pas partitionnée, la table résultante est incluse dans le même groupe de fichiers que l'index cluster. Le groupe de fichiers doit déjà exister.  
  
 **"** default **"**  
 Spécifie l'emplacement par défaut de la table résultante.  
  
> [!NOTE]
>  L'élément « default » n'est pas un mot clé dans ce contexte. Il s’agit d’un identificateur du groupe de fichiers par défaut qui doit être délimité, comme dans MOVE TO **"** default **"** or MOVE TO **[** default **]** . Si **"** default **"** est spécifié, l’option QUOTED_IDENTIFIER doit être ON pour la session active. Il s'agit du paramètre par défaut. Pour plus d’informations, consultez [SET QUOTED_IDENTIFIER &#40;Transact-SQL&#41;](../../t-sql/statements/set-quoted-identifier-transact-sql.md).  
  
 FILESTREAM_ON { *partition_scheme_name* | *filestream_filegroup_name* |  **"** default **"** }  
 **S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.  
  
 Spécifie un emplacement pour déplacer la table FILESTREAM qui se trouve actuellement au niveau feuille de l'index cluster. Les données sont déplacées vers le nouvel emplacement sous la forme d'un segment de mémoire. Vous pouvez spécifier un schéma de partition ou un groupe de fichiers déjà existants comme nouvel emplacement. FILESTREAM ON n'est pas valide pour les vues indexées ou les index non cluster. Si aucun schéma de partition n'est spécifié, les données sont placées dans le même schéma de partition que celui qui a été défini pour l'index cluster.  
  
 *partition_scheme_name*  
 Spécifie un schéma de partition pour les données FILESTREAM. Le schéma de partition doit déjà avoir été créé en exécutant soit [CREATE PARTITION SCHEME](../../t-sql/statements/create-partition-scheme-transact-sql.md), soit [ALTER PARTITION SCHEME](../../t-sql/statements/alter-partition-scheme-transact-sql.md). Si aucun emplacement n'est spécifié et que la table est partitionnée, la table est incluse dans le même schéma de partition que l'index cluster existant.  
  
 Si vous spécifiez un schéma de partition pour MOVE TO, vous devez utiliser le même schéma de partition pour FILESTREAM ON.  
  
 *filestream_filegroup_name*  
 Spécifie un groupe de fichiers FILESTREAM pour les données FILESTREAM. Si aucun emplacement n'est défini et que la table n'est pas partitionnée, les données sont incluses dans le groupe de fichiers FILESTREAM par défaut.  
  
 **"** default **"**  
 Spécifie l'emplacement par défaut des données FILESTREAM.  
  
> [!NOTE]
>  L'élément « default » n'est pas un mot clé dans ce contexte. Il s’agit d’un identificateur du groupe de fichiers par défaut qui doit être délimité, comme dans MOVE TO **"** default **"** or MOVE TO **[** default **]** . Si "default" est spécifié, l'option QUOTED_IDENTIFIER doit être activée (ON) pour la session active. Il s'agit du paramètre par défaut. Pour plus d’informations, consultez [SET QUOTED_IDENTIFIER &#40;Transact-SQL&#41;](../../t-sql/statements/set-quoted-identifier-transact-sql.md).  
  
## <a name="remarks"></a>Notes  
 Lorsqu'un index non cluster est supprimé, la définition d'index est supprimée des métadonnées et les pages de données d'index (l'arborescence binaire ou arbre B) sont supprimées des fichiers de base de données. Lorsqu'un index cluster est supprimé, la définition d'index est supprimée des métadonnées et les lignes de données qui étaient stockées au niveau feuille de l'index cluster sont stockées dans la table non triée résultante, un segment de mémoire. Tout l'espace précédemment occupé par l'index est récupéré. Cet espace peut ensuite être réaffecté à n'importe quel objet de la base de données.  
  
 Un index ne peut pas être supprimé si le groupe de fichiers dans lequel il se trouve est hors connexion ou défini comme étant en lecture seule.  
  
 Lorsque l'index cluster d'une vue indexée est supprimé, tous les index non cluster et les statistiques créées automatiquement sur la même vue sont automatiquement supprimés. Les statistiques créées manuellement ne sont pas supprimées.  
  
 La syntaxe _table\_or\_view\_name_ **.** _index\_name_ est conservée à des fins de compatibilité descendante. Un index XML ou index spatial ne peut pas être supprimé à l'aide de la syntaxe à compatibilité descendante.  
  
 Lors de la suppression d'index contenant au moins 128 étendues, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] diffère les désallocations de pages ainsi que les verrous qui y sont associés jusqu'à ce que la transaction soit validée.  
  
 Des index peuvent parfois être supprimés et recréés pour réorganiser ou reconstruire l'index, par exemple pour appliquer un nouveau taux de remplissage ou pour réorganiser les données après un chargement en masse. Pour ce faire, l’utilisation de [ALTER INDEX](../../t-sql/statements/alter-index-transact-sql.md) est plus efficace, en particulier pour les index clusters. ALTER INDEX REBUILD possède des optimisations permettant d'éviter la surcharge liée à la reconstruction des index non cluster.  
  
## <a name="using-options-with-drop-index"></a>Utilisations d'options avec DROP INDEX  
 Vous pouvez définir les options d’index suivantes lorsque vous supprimez un index cluster : MAXDOP, ONLINE, et MOVE TO.  
  
 Utilisez MOVE TO pour supprimer l'index cluster et déplacer la table résultante vers un autre groupe de fichiers ou schéma de partition dans une transaction unique.  
  
 Lorsque vous spécifiez ONLINE = ON, les requêtes et modifications portant sur les données sous-jacentes et index non cluster associés ne sont pas bloqués par la transaction DROP INDEX. Vous pouvez modifier un seul index cluster en ligne à la fois. Pour obtenir une description complète de l’option ONLINE, consultez [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md).  
  
 Vous ne pouvez pas supprimer un index cluster en ligne s’il est désactivé sur une vue ou contient les colonnes **text**, **ntext**, **image**, **varchar(max)** , **nvarchar(max)** , **varbinary(max)** ou **xml** sur les lignes de données de niveau feuille.  
  
 L'utilisation des options ONLINE = ON et MOVE TO nécessite une espace disque temporaire supplémentaire.  
  
 Après la suppression d’un index, le segment de mémoire résultant apparaît dans la vue de catalogue **sys.indexes** avec la valeur NULL dans la colonne **name**. Pour afficher le nom de la table, joignez **sys.indexes** à **sys.tables** sur **object_id**. Pour un exemple de requête, reportez-vous à l'exemple D.  
  
 Sur les ordinateurs multiprocesseurs qui exécutent [!INCLUDE[ssEnterpriseEd2005](../../includes/ssenterpriseed2005-md.md)] ou version ultérieure, DROP INDEX peut, à l'instar d'autres requêtes, utiliser davantage de processeurs pour réaliser les opérations d'analyse et de tri associées à la suppression de l'index cluster. Vous pouvez configurer manuellement le nombre de processeurs utilisés pour exécuter l'instruction DROP INDEX en spécifiant l'option d'index MAXDOP. Pour plus d’informations, consultez [Configurer des opérations d’index parallèles](../../relational-databases/indexes/configure-parallel-index-operations.md).  
  
 Lorsqu'un index cluster est supprimé, les partitions des segments de mémoire correspondants conservent leur paramètre de compression des données, à moins que le schéma de partitionnement soit modifié. Si le schéma de partitionnement est modifié, toutes les partitions sont reconstruites dans un état non compressé (DATA_COMPRESSION = NONE). Pour supprimer un index cluster et modifier le schéma de partitionnement, vous devez effectuer les deux opérations suivantes :  
  
1.  supprimer l'index cluster ;  
  
2.  modifier la table à l’aide d’une option ALTER TABLE ... REBUILD ... spécifiant l’option de compression.  
  
Lorsqu'un index cluster est supprimé HORS CONNEXION, seuls les niveaux supérieurs des index clusters sont supprimés ; cette opération est donc très rapide. Quand un index cluster est supprimé EN LIGNE, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] reconstruit le segment de mémoire deux fois, une fois pour l’étape 1 et une fois pour l’étape 2. Pour plus d’informations sur la compression de données, consultez [Compression des données](../../relational-databases/data-compression/data-compression.md).  
  
## <a name="xml-indexes"></a>Index XML  
 Les options ne peuvent pas être spécifiées quand vous supprimez un index XML. En outre, vous ne pouvez pas utiliser la syntaxe _table\_or\_view\_name_ **.** _index\_name_. Lorsqu'un index XML primaire est supprimé, tous les index XML secondaires associés sont également supprimés. Pour plus d’informations, consultez [Index XML &#40;SQL Server&#41;](../../relational-databases/xml/xml-indexes-sql-server.md).  
  
## <a name="spatial-indexes"></a>Index spatiaux  
 Les index spatiaux sont pris en charge uniquement dans les tables. Quand vous supprimez un index spatial, vous ne pouvez pas spécifier d’options ni utiliser **.** _index\_name_. La syntaxe correcte est la suivante :  
  
 DROP INDEX *spatial_index_name* ON *spatial_table_name*;  
  
 Pour plus d’informations sur les index spatiaux, consultez [Vue d’ensemble des index spatiaux](../../relational-databases/spatial/spatial-indexes-overview.md).  
  
## <a name="permissions"></a>Autorisations  
 L'exécution de DROP INDEX nécessite au moins une autorisation ALTER sur la table ou la vue. L’autorisation est accordée par défaut au rôle serveur fixe **sysadmin** et aux rôles de base de données fixes **db_ddladmin** et **db_owner** .  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-dropping-an-index"></a>R. Suppression d'un index  
 L’exemple suivant supprime l’index `IX_ProductVendor_VendorID` sur la table `ProductVendor` de la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```sql  
DROP INDEX IX_ProductVendor_BusinessEntityID   
    ON Purchasing.ProductVendor;  
GO  
```  
  
### <a name="b-dropping-multiple-indexes"></a>B. Suppression de plusieurs index  
 L'exemple suivant supprime deux index en une seule transaction dans la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```sql  
DROP INDEX  
    IX_PurchaseOrderHeader_EmployeeID ON Purchasing.PurchaseOrderHeader,  
    IX_Address_StateProvinceID ON Person.Address;  
GO  
```  
  
### <a name="c-dropping-a-clustered-index-online-and-setting-the-maxdop-option"></a>C. Suppression d'un index cluster en ligne et configuration de l'option MAXDOP  
 L'exemple suivant supprime un index cluster en affectant à l'option `ONLINE` la valeur `ON` et à `MAXDOP` la valeur `8`. L'option MOVE TO n'étant pas spécifiée, la table résultante est stockée dans le même groupe de fichiers que l'index. Cet exemple utilise la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures, [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].  
  
```sql  
DROP INDEX AK_BillOfMaterials_ProductAssemblyID_ComponentID_StartDate   
    ON Production.BillOfMaterials WITH (ONLINE = ON, MAXDOP = 2);  
GO  
```  
  
### <a name="d-dropping-a-clustered-index-online-and-moving-the-table-to-a-new-filegroup"></a>D. Suppression d'un index cluster en ligne et déplacement de la table vers un nouveau groupe de fichiers  
 L'exemple suivant supprime un index cluster en ligne et déplace la table résultante (segment de mémoire) vers le groupe de fichiers `NewGroup` en utilisant la clause `MOVE TO` . Les affichages catalogue `sys.indexes`, `sys.tables`et `sys.filegroups` sont interrogés pour vérifier le placement de l'index et de la table dans les groupes de fichiers avant et après l'opération de déplacement. (Depuis [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)], vous pouvez utiliser la syntaxe DROP INDEX IF EXISTS.)  
  
**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.  
  
```sql  
--Create a clustered index on the PRIMARY filegroup if the index does not exist.  
CREATE UNIQUE CLUSTERED INDEX  
    AK_BillOfMaterials_ProductAssemblyID_ComponentID_StartDate   
        ON Production.BillOfMaterials (ProductAssemblyID, ComponentID,   
        StartDate)  
    ON 'PRIMARY';  
GO  
-- Verify filegroup location of the clustered index.  
SELECT t.name AS [Table Name], i.name AS [Index Name], i.type_desc,  
    i.data_space_id, f.name AS [Filegroup Name]  
FROM sys.indexes AS i  
    JOIN sys.filegroups AS f ON i.data_space_id = f.data_space_id  
    JOIN sys.tables as t ON i.object_id = t.object_id  
        AND i.object_id = OBJECT_ID(N'Production.BillOfMaterials','U')  
GO  
--Create filegroup NewGroup if it does not exist.  
IF NOT EXISTS (SELECT name FROM sys.filegroups  
                WHERE name = N'NewGroup')  
    BEGIN  
    ALTER DATABASE AdventureWorks2012  
        ADD FILEGROUP NewGroup;  
    ALTER DATABASE AdventureWorks2012  
        ADD FILE (NAME = File1,  
            FILENAME = 'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\DATA\File1.ndf')  
        TO FILEGROUP NewGroup;  
    END  
GO  
--Verify new filegroup  
SELECT * from sys.filegroups;  
GO  
-- Drop the clustered index and move the BillOfMaterials table to  
-- the Newgroup filegroup.  
-- Set ONLINE = OFF to execute this example on editions other than Enterprise Edition.  
DROP INDEX AK_BillOfMaterials_ProductAssemblyID_ComponentID_StartDate   
    ON Production.BillOfMaterials   
    WITH (ONLINE = ON, MOVE TO NewGroup);  
GO  
-- Verify filegroup location of the moved table.  
SELECT t.name AS [Table Name], i.name AS [Index Name], i.type_desc,  
    i.data_space_id, f.name AS [Filegroup Name]  
FROM sys.indexes AS i  
    JOIN sys.filegroups AS f ON i.data_space_id = f.data_space_id  
    JOIN sys.tables as t ON i.object_id = t.object_id  
        AND i.object_id = OBJECT_ID(N'Production.BillOfMaterials','U');  
GO  
```  
  
### <a name="e-dropping-a-primary-key-constraint-online"></a>E. Suppression d'une contrainte PRIMARY KEY en ligne  
 Les index créés suite à la création de contraintes PRIMARY KEY ou UNIQUE ne peuvent être supprimés qu'à l'aide de DROP INDEX. Ils sont supprimés à l'aide de l'instruction ALTER TABLE DROP CONSTRAINT. Pour plus d’informations, consultez [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md).  
  
 L'exemple suivant supprime un index cluster avec une contrainte PRIMARY KEY en supprimant la contrainte. La table `ProductCostHistory` ne comporte aucune contrainte FOREIGN KEY. Si cela avait été le cas, ces contraintes auraient d'abord dû être supprimées.  
  
```sql  
-- Set ONLINE = OFF to execute this example on editions other than Enterprise Edition.  
ALTER TABLE Production.TransactionHistoryArchive  
DROP CONSTRAINT PK_TransactionHistoryArchive_TransactionID  
WITH (ONLINE = ON);  
```  
  
### <a name="f-dropping-an-xml-index"></a>F. Suppression d'un index XML  
 L'exemple suivant supprime un index XML dans la table `ProductModel` de la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```sql  
DROP INDEX PXML_ProductModel_CatalogDescription   
    ON Production.ProductModel;  
```  
  
### <a name="g-dropping-a-clustered-index-on-a-filestream-table"></a>G. Suppression d'un index cluster sur une table FILESTREAM  
 L'exemple suivant supprime un index cluster en ligne et déplace la table résultante (segment de mémoire) et les données FILESTREAM vers le schéma de partitionnement `MyPartitionScheme` en utilisant les clauses `MOVE TO` et `FILESTREAM ON`.  
  
**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.  
  
```sql  
DROP INDEX PK_MyClusteredIndex   
    ON dbo.MyTable   
    WITH (MOVE TO MyPartitionScheme,  
          FILESTREAM_ON MyPartitionScheme);  
GO  
```  

  
## <a name="see-also"></a>Voir aussi  
 [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md)   
 [ALTER PARTITION SCHEME &#40;Transact-SQL&#41;](../../t-sql/statements/alter-partition-scheme-transact-sql.md)   
 [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)   
 [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md)   
 [CREATE PARTITION SCHEME &#40;Transact-SQL&#41;](../../t-sql/statements/create-partition-scheme-transact-sql.md)   
 [CREATE SPATIAL INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-spatial-index-transact-sql.md)   
 [CREATE XML INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-xml-index-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)   
 [sys.indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)   
 [sys.tables &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-tables-transact-sql.md)   
 [sys.filegroups &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-filegroups-transact-sql.md)   
 [sp_spaceused &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-spaceused-transact-sql.md)  
  
  


