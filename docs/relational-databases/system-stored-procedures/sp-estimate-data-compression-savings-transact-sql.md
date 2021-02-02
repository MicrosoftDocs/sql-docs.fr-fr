---
description: sp_estimate_data_compression_savings (Transact-SQL)
title: sp_estimate_data_compression_savings (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/26/2019
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_estimate_data_compression_savings_TSQL
- sp_estimate_data_compression_savings
dev_langs:
- TSQL
helpviewer_keywords:
- compression [SQL Server], estimating
- sp_estimate_data_compression_savings
ms.assetid: 6f6c7150-e788-45e0-9d08-d6c2f4a33729
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 93d169d364369e87f1c363297c2a5c73c58e442b
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237850"
---
# <a name="sp_estimate_data_compression_savings-transact-sql"></a>sp_estimate_data_compression_savings (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Retourne la taille actuelle de l'objet demandé et estime la taille de l'objet pour l'état de compression demandé. La compression peut être évaluée pour des tables entières ou des parties de tables. Cela comprend les tas, les index cluster, les index non cluster, les index ColumnStore, les vues indexées et les partitions de table et d’index. Les objets peuvent être compressés à l’aide de la compression d’archive Row, page, ColumnStore ou ColumnStore. Si la table, l'index ou la partition sont déjà compressés, vous pouvez utiliser cette procédure pour estimer la taille de la table, de l'index ou de la partition s'ils sont décompressés.  
  
> [!NOTE]
> La compression et les **sp_estimate_data_compression_savings** ne sont pas disponibles dans toutes les éditions de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Pour obtenir la liste des fonctionnalités prises en charge par les éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez [Fonctionnalités prise en charge par les éditions de SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).  
  
 Pour estimer la taille de l'objet s'il devait utiliser le paramètre de compression demandé, cette procédure stockée échantillonne l'objet source et charge ces données dans une table et un index équivalents, créés dans tempdb. La table ou l'index créés dans tempdb sont ensuite compressés au paramètre demandé et les gains de compression estimés sont calculés.  
  
 Pour modifier l’état de compression d’une table, d’un index ou d’une partition, utilisez les instructions [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md) ou [ALTER index](../../t-sql/statements/alter-index-transact-sql.md) . Pour obtenir des informations générales sur la compression, consultez [compression des données](../../relational-databases/data-compression/data-compression.md).  
  
> [!NOTE]  
> Si les données existantes sont fragmentées, vous pouvez être en mesure de réduire leur taille sans utiliser la compression en reconstruisant l'index. Pour les index, le facteur de remplissage sera appliqué pendant une reconstruction d'index. Cela pourrait augmenter la taille de l'index.  

 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
sp_estimate_data_compression_savings   
     [ @schema_name = ] 'schema_name'    
   , [ @object_name = ] 'object_name'   
   , [ @index_id = ] index_id   
   , [ @partition_number = ] partition_number   
   , [ @data_compression = ] 'data_compression'   
[;]  
```  
  
## <a name="arguments"></a>Arguments  
 [ @schema_name =] '*schema_name*'  
 Nom du schéma de base de données qui contient la table ou la vue indexée. *schema_name* est de **type sysname**. Si *schema_name* a la valeur null, le schéma par défaut de l’utilisateur actuel est utilisé.  
  
 [ @object_name =] '*object_name*'  
 Nom du schéma de la table ou de la vue indexée sur laquelle se trouve l'index. *object_name* est de type **sysname**.  
  
 [ @index_id =] *index_id*  
 Identificateur de l’index. *index_id* est de **type int** et peut prendre l’une des valeurs suivantes : le numéro d’ID d’un index, NULL ou 0 si *object_id* est un segment de mémoire. Pour retourner des informations sur tous les index d'une table de base ou d'une vue, spécifiez la valeur NULL. Si vous spécifiez NULL, vous devez également spécifier NULL pour *partition_number*.  
  
 [ @partition_number =] *partition_number*  
 Numéro de partition dans l'objet. *partition_number* est de **type int** et peut prendre l’une des valeurs suivantes : le numéro de partition d’un index ou d’un segment de mémoire, null ou 1 pour un index ou un segment de mémoire non partitionné.  
  
 Pour spécifier la partition, vous pouvez également spécifier la fonction [$partition](../../t-sql/functions/partition-transact-sql.md) . Pour retourner des informations pour toutes les partitions de l'objet propriétaire, spécifiez NULL.  
  
 [ @data_compression =] '*DATA_COMPRESSION*'  
 Type de compression à évaluer. *DATA_COMPRESSION* peut prendre l’une des valeurs suivantes : None, Row, page, COLUMNSTORE ou COLUMNSTORE_ARCHIVE.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 Le jeu de résultats suivant est retourné pour fournir la taille actuelle et estimée de la table, de l'index ou de la partition.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|object_name|**sysname**|Nom de la table ou de la vue indexée.|  
|schema_name|**sysname**|Schéma de la table ou de la vue indexée.|  
|index_id|**int**|ID d'index d'un index :<br /><br /> 0 = Segment de mémoire<br /><br /> 1 = index cluster<br /><br /> > 1 = Index non-cluster|  
|partition_number|**int**|Numéro de partition. Retourne 1 pour une table ou un index non partitionnés.|  
|size_with_current_compression_setting (Ko)|**bigint**|Taille de la table, de l'index ou de la partition demandés tels qu'ils existent actuellement.|  
|size_with_requested_compression_setting (Ko)|**bigint**|Taille estimée de la table, de l'index ou de la partition qui utilise le paramètre de compression demandé et, le cas échéant, le facteur de remplissage existant, en supposant qu'il n'y a pas de fragmentation.|  
|sample_size_with_current_compression_setting (Ko)|**bigint**|Taille de l'exemple avec le paramètre de compression actuel. Cela inclut toute fragmentation éventuelle.|  
|sample_size_with_requested_compression_setting (Ko)|**bigint**|Taille de l'échantillon créé à l'aide du paramètre de compression demandé et, le cas échéant, du facteur de remplissage existant, sans fragmentation.|  
  
## <a name="remarks"></a>Remarques  
 Utilisez `sp_estimate_data_compression_savings` pour estimer les économies qui peuvent se produire lorsque vous activez une table ou une partition pour la compression d’archive Row, page, ColumnStore ou ColumnStore. Par exemple, si la taille moyenne de la ligne peut être réduite de 40 %, vous pouvez réduire la taille de l'objet de 40 %. Vous n'économiserez peut-être pas d'espace car cela dépend du facteur de remplissage et de la taille de la ligne. Par exemple, si vous avez une ligne d’une longueur de 8 000 octets et que vous réduisez sa taille de 40%, vous pouvez toujours faire tenir une seule ligne dans une page de données. Vous ne bénéficiez d'aucun gain.  
  
 Si les résultats de l'exécution de `sp_estimate_data_compression_savings` indiquent que la taille de la table augmentera, cela signifie que de nombreuses lignes de la table utilisent presque toute la précision des types de données, et que l'ajout de la faible surcharge requise pour le format compressé dépasse les gains dérivés de la compression. Dans ce cas très peu fréquent, n'activez pas la compression.  
  
 Si une table est activée pour la compression, utilisez `sp_estimate_data_compression_savings` pour estimer la taille moyenne de la ligne si la table est décompressée.  
  
 Un verrou (IS) est acquis sur la table pendant cette opération. Si un verrou (IS) ne peut pas être obtenu, la procédure sera bloquée. La table est analysée sous le niveau d'isolement de lecture validée.  
  
 Si le paramètre de compression demandé est identique au paramètre de compression actuel, la procédure stockée retournera la taille estimée sans fragmentation de données et en utilisant le facteur de remplissage existant.  
  
 Si l'ID d'index ou de partition n'existe pas, aucun résultat n'est retourné.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation `SELECT` sur la table.  
  
## <a name="limitations-and-restrictions"></a>Limitations et restrictions  
 Avant SQL Server 2019, cette procédure ne s’appliquait pas aux index ColumnStore et n’acceptait donc pas les paramètres de compression de données COLUMNSTORE et COLUMNSTORE_ARCHIVE.  À compter de SQL Server 2019, les index ColumnStore peuvent être utilisés à la fois comme un objet source pour l’estimation et comme type de compression demandé.

 > [!IMPORTANT]
 > Lorsque [les métadonnées tempdb à mémoire optimisée](../databases/tempdb-database.md#memory-optimized-tempdb-metadata) sont activées dans [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] , la création d’index ColumnStore sur des tables temporaires n’est pas prise en charge. En raison de cette limitation, sp_estimate_data_compression_savings n’est pas pris en charge avec les paramètres de compression de données COLUMNSTORE et COLUMNSTORE_ARCHIVE lorsque Memory-Optimized métadonnées TempDB sont activées.

## <a name="considerations-for-columnstore-indexes"></a>Remarques pour les index columnstore
 À compter de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] , `sp_estimate_compression_savings` prend en charge l’estimation de la compression d’archive ColumnStore et ColumnStore. Contrairement à la compression de page et de ligne, l’application de la compression ColumnStore à un objet requiert la création d’un nouvel index ColumnStore. Pour cette raison, lorsque vous utilisez les options COLUMNSTORE et COLUMNSTORE_ARCHIVE de cette procédure, le type de l’objet source fourni à la procédure détermine le type d’index COLUMNSTORE utilisé pour l’estimation de la taille compressée. Le tableau suivant illustre les objets de référence utilisés pour estimer les économies de compression pour chaque type d’objet source lorsque le @data_compression paramètre est défini sur COLUMNSTORE ou COLUMNSTORE_ARCHIVE.

 |Objet source|Objet de référence|
 |-----------------|---------------|
 |Segment de mémoire (heap)|Index columnstore cluster|
 |Index cluster|Index columnstore cluster|
 |Index non cluster|Index ColumnStore non-cluster (y compris les colonnes clés et toutes les colonnes incluses de l’index non cluster fourni, ainsi que la colonne de partition de la table, le cas échéant)|
 |index columnstore non cluster|Index ColumnStore non-cluster (y compris les mêmes colonnes que l’index ColumnStore non cluster fourni)|
 |Index columnstore cluster|Index columnstore cluster|

> [!NOTE]  
> Lorsque vous évaluez la compression ColumnStore à partir d’un objet source rowstore (index cluster, index non cluster ou segment de mémoire), si des colonnes de l’objet source ont un type de données qui n’est pas pris en charge dans un index ColumnStore, sp_estimate_compression_savings échoue avec une erreur.

 De même, lorsque le `@data_compression` paramètre a la valeur `NONE` , `ROW` ou `PAGE` et que l’objet source est un index ColumnStore, le tableau suivant présente les objets de référence utilisés.

 |Objet source|Objet de référence|
 |-----------------|---------------|
 |Index columnstore cluster|Segment de mémoire (heap)|
 |index columnstore non cluster|Index non cluster (y compris les colonnes contenues dans l’index ColumnStore non-cluster en tant que colonnes clés et la colonne de partition de la table, le cas échéant, en tant que colonne incluse)|

> [!NOTE]  
> Lorsque vous évaluez la compression rowstore (aucune, ligne ou PAGE) à partir d’un objet source ColumnStore, assurez-vous que l’index source ne contient pas plus de 32 colonnes, car il s’agit de la limite prise en charge dans un index rowstore (non cluster).
  
## <a name="examples"></a>Exemples  
 L'exemple ci-dessous estime la taille de la table `Production.WorkOrderRouting` si elle est compressée à l'aide de la compression `ROW`.  
  
```sql  
USE AdventureWorks2016;  
GO  
EXEC sp_estimate_data_compression_savings 'Production', 'WorkOrderRouting', NULL, NULL, 'ROW' ;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)   
 [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md)   
 [sys.partitions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-partitions-transact-sql.md)   
 [Moteur de base de données des procédures stockées &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [Implémentation de la compression Unicode](../../relational-databases/data-compression/unicode-compression-implementation.md)  
  
  
