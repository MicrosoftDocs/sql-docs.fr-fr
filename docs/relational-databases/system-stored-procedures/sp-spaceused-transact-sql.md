---
description: sp_spaceused (Transact-SQL)
title: sp_spaceused (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_spaceused_TSQL
- sp_spaceused
dev_langs:
- TSQL
helpviewer_keywords:
- sp_spaceused
ms.assetid: c6253b48-29f5-4371-bfcd-3ef404060621
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 480b61493dc38ea91679e590f3abe63bde3a48c0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189304"
---
# <a name="sp_spaceused-transact-sql"></a>sp_spaceused (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Affiche le nombre de lignes, l'espace disque réservé et l'espace disque utilisé par une table, une vue indexée ou une file d'attente [!INCLUDE[ssSB](../../includes/sssb-md.md)] de la base de données active, ou affiche l'espace disque réservé et utilisé par l'ensemble de la base de données.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql  
sp_spaceused [[ @objname = ] 'objname' ]   
[, [ @updateusage = ] 'updateusage' ]  
[, [ @mode = ] 'mode' ]  
[, [ @oneresultset = ] oneresultset ]  
[, [ @include_total_xtp_storage = ] include_total_xtp_storage ]
```  
[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]
  
## <a name="arguments"></a>Arguments  

Pour [!INCLUDE[sssdw-md](../../includes/sssdw-md.md)] et [!INCLUDE[sspdw-md](../../includes/sspdw-md.md)] , `sp_spaceused` doit spécifier des paramètres nommés (par exemple, `sp_spaceused (@objname= N'Table1');` au lieu de s’appuyer sur la position ordinale des paramètres. 

`[ @objname = ] 'objname'`
   
 Nom qualifié ou non qualifié de la table, de la vue indexée ou de la file d'attente pour laquelle des informations d'utilisation d'espace sont demandées. Les guillemets ne sont nécessaires que si un nom d'objet qualifié est spécifié. Si un nom d'objet complet (incluant un nom de base de données) est fourni, le nom de la base de données doit être celui de la base de données actuelle.  
Si *nom_d’nom_d* 'n’est pas spécifié, les résultats sont retournés pour l’ensemble de la base de données.  
*nomobj* est de type **nvarchar (776)**, avec NULL comme valeur par défaut.  
> [!NOTE]  
> [!INCLUDE[sssdw-md](../../includes/sssdw-md.md)] et [!INCLUDE[sspdw-md](../../includes/sspdw-md.md)] prennent uniquement en charge les objets de base de données et de table.
  
`[ @updateusage = ] 'updateusage'` Indique que DBCC UPDATEUSAGE doit être exécuté pour mettre à jour les informations sur l’utilisation de l’espace. Quand *nomobj* n’est pas spécifié, l’instruction est exécutée sur l’ensemble de la base de données ; dans le cas contraire, l’instruction est exécutée sur *nomobj*. Les valeurs peuvent être **true** ou **false**. *UPDATEUSAGE* est de type **varchar (5)**, avec **false** comme valeur par défaut.  
  
`[ @mode = ] 'mode'` Indique l’étendue des résultats. Pour une table ou une base de données étirée, le paramètre *mode* vous permet d’inclure ou d’exclure la partie distante de l’objet. Pour plus d'informations, consultez [Stretch Database](../../sql-server/stretch-database/stretch-database.md).  
  
 L’argument *mode* peut avoir les valeurs suivantes :  
  
|Valeur|Description|  
|-----------|-----------------|  
|ALL|Retourne les statistiques de stockage de l’objet ou de la base de données, y compris la partie locale et la partie distante.|  
|LOCAL_ONLY|Retourne les statistiques de stockage de la partie locale de l’objet ou de la base de données. Si l’objet ou la base de données ne prend pas en charge Stretch, retourne les mêmes statistiques que quand @mode = All.|  
|REMOTE_ONLY|Retourne les statistiques de stockage de la partie distante uniquement de l’objet ou de la base de données. Cette option génère une erreur lorsque l’une des conditions suivantes est remplie :<br /><br /> La table n’est pas activée pour Stretch.<br /><br /> La table est activée pour Stretch, mais vous n’avez jamais activé la migration des données. Dans ce cas, la table distante n’a pas encore de schéma.<br /><br /> L’utilisateur a supprimé manuellement la table distante.<br /><br /> L’approvisionnement de l’archive de données distante a retourné un état de réussite, mais en fait, elle a échoué.|  
  
 le *mode* est **varchar (11)**, avec **n’All** comme valeur par défaut.  
  
`[ @oneresultset = ] oneresultset` Indique s’il faut retourner un seul jeu de résultats. L’argument *oneresultset* peut avoir les valeurs suivantes :  
  
|Valeur|Description|  
|-----------|-----------------|  
|0|Quand *\@ nomobj* a la valeur null ou qu’il n’est pas spécifié, deux jeux de résultats sont retournés. Deux jeux de résultats sont le comportement par défaut.|  
|1|Quand *\@ nomobj* = null ou n’est pas spécifié, un seul jeu de résultats est retourné.|  
  
 *oneresultset* est de valeur de **bit**, avec **0** comme valeur par défaut.  

`[ @include_total_xtp_storage] 'include_total_xtp_storage'`
**S’applique à :** [!INCLUDE[sssql17-md](../../includes/sssql17-md.md)] , [!INCLUDE[sssds-md](../../includes/sssds-md.md)] .  
  
 Lorsque @oneresultset = 1, le paramètre @include_total_xtp_storage détermine si le jeu de résultats unique comprend des colonnes pour le stockage MEMORY_OPTIMIZED_DATA. La valeur par défaut est 0, autrement dit, par défaut (si le paramètre est omis), les colonnes XTP ne sont pas incluses dans le jeu de résultats.  

## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 Si *nom_d* 'objet est omis et que la valeur de *oneresultset* est 0, les jeux de résultats suivants sont retournés pour fournir les informations de taille de la base de données actuelle.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**database_name**|**nvarchar(128)**|Nom de la base de données en cours.|  
|**database_size**|**varchar (18)**|Taille de la base de données actuelle en mégaoctets. **database_size** contient à la fois des fichiers de données et des fichiers journaux.|  
|**espace non alloué**|**varchar (18)**|Espace de la base de données qui n'a pas été réservé pour des objets de base de données.|  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**réservé**|**varchar (18)**|Quantité totale d'espace allouée par les objets dans la base de données.|  
|**data**|**varchar (18)**|Quantité totale d'espace qu'occupent les données.|  
|**index_size**|**varchar (18)**|Quantité totale d'espace qu'occupent les index.|  
|**inutilisé**|**varchar (18)**|Quantité totale d'espace réservée pour les objets dans la base de données, mais non encore utilisé.|  
  
 Si *nomobj* est omis et que la valeur de *oneresultset* est 1, le jeu de résultats unique suivant est retourné pour fournir les informations de taille de la base de données actuelle.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**database_name**|**nvarchar(128)**|Nom de la base de données en cours.|  
|**database_size**|**varchar (18)**|Taille de la base de données actuelle en mégaoctets. **database_size** contient à la fois des fichiers de données et des fichiers journaux.|  
|**espace non alloué**|**varchar (18)**|Espace de la base de données qui n'a pas été réservé pour des objets de base de données.|  
|**réservé**|**varchar (18)**|Quantité totale d'espace allouée par les objets dans la base de données.|  
|**data**|**varchar (18)**|Quantité totale d'espace qu'occupent les données.|  
|**index_size**|**varchar (18)**|Quantité totale d'espace qu'occupent les index.|  
|**inutilisé**|**varchar (18)**|Quantité totale d'espace réservée pour les objets dans la base de données, mais non encore utilisé.|  
  
 Si *nom_d* 'objet est spécifié, le jeu de résultats suivant est retourné pour l’objet spécifié.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**name**|**nvarchar(128)**|Nom de l'objet pour lequel ont été demandées les informations relatives à l'utilisation de l'espace.<br /><br /> Le nom de schéma de l'objet n'est pas renvoyé. Si le nom de schéma est requis, utilisez la [sys.dm_db_partition_stats](../../relational-databases/system-dynamic-management-views/sys-dm-db-partition-stats-transact-sql.md) ou [sys.dm_db_index_physical_stats](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-physical-stats-transact-sql.md) vues de gestion dynamique pour obtenir des informations de taille équivalentes.|  
|**rows**|**Char (20)**|Nombre de lignes existant dans la table. Si l'objet spécifié est une file d'attente [!INCLUDE[ssSB](../../includes/sssb-md.md)], cette colonne indique le nombre de messages en file d'attente.|  
|**réservé**|**varchar (18)**|Quantité totale d’espace réservé pour *nomobj*.|  
|**data**|**varchar (18)**|Quantité totale d’espace utilisée par les données dans *nomobj*.|  
|**index_size**|**varchar (18)**|Quantité totale d’espace utilisée par les index dans *nomobj*.|  
|**inutilisé**|**varchar (18)**|Quantité totale d’espace réservée pour *nomobj* , mais pas encore utilisée.|  
 
Il s’agit du mode par défaut, quand aucun paramètre n’est spécifié. Les jeux de résultats suivants sont retournés en détail des informations sur la taille des bases de données sur disque. 

|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**database_name**|**nvarchar(128)**|Nom de la base de données en cours.|  
|**database_size**|**varchar (18)**|Taille de la base de données actuelle en mégaoctets. **database_size** contient à la fois des fichiers de données et des fichiers journaux. Si la base de données a un groupe de fichiers MEMORY_OPTIMIZED_DATA, cela inclut la taille totale sur disque de tous les fichiers de point de contrôle dans le groupe de fichiers.|  
|**espace non alloué**|**varchar (18)**|Espace de la base de données qui n'a pas été réservé pour des objets de base de données. Si la base de données a un groupe de fichiers MEMORY_OPTIMIZED_DATA, cela inclut la taille totale sur disque des fichiers de point de contrôle dont l’État est précréé dans le groupe de fichiers.|  

Espace utilisé par les tables dans la base de données : (ce ResultSet ne reflète pas les tables mémoire optimisées, car il n’y a aucune comptabilité par table de l’utilisation du disque) 

|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**réservé**|**varchar (18)**|Quantité totale d'espace allouée par les objets dans la base de données.|  
|**data**|**varchar (18)**|Quantité totale d'espace qu'occupent les données.|  
|**index_size**|**varchar (18)**|Quantité totale d'espace qu'occupent les index.|  
|**inutilisé**|**varchar (18)**|Quantité totale d'espace réservée pour les objets dans la base de données, mais non encore utilisé.|

Le jeu de résultats suivant est retourné **uniquement si** la base de données a un groupe de MEMORY_OPTIMIZED_DATA avec au moins un conteneur : 

|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**xtp_precreated**|**varchar (18)**|Taille totale des fichiers de point de contrôle dont l’État est précréé, en Ko. Compte l’espace non alloué dans la base de données dans son ensemble. [Par exemple, s’il y a 600 000 Ko de fichiers de point de contrôle précréés, cette colonne contient « 600000 Ko »]|  
|**xtp_used**|**varchar (18)**|Taille totale des fichiers de point de contrôle avec les États sous CONSTRUCTION, ACTIVE et cible de fusion, en Ko. Il s’agit de l’espace disque activement utilisé pour les données dans les tables optimisées en mémoire.|  
|**xtp_pending_truncation**|**varchar (18)**|Taille totale des fichiers de point de contrôle avec l’État WAITING_FOR_LOG_TRUNCATION, en Ko. Il s’agit de l’espace disque utilisé pour les fichiers de point de contrôle en attente de nettoyage. une fois la troncation du journal effectuée.|

Si *nom_d* 'objet est omis, la valeur de oneresultset est 1, et *include_total_xtp_storage* est 1, le jeu de résultats unique suivant est retourné pour fournir les informations de taille de la base de données actuelle. Si `include_total_xtp_storage` est égal à 0 (valeur par défaut), les trois dernières colonnes sont omises. 

|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**database_name**|**nvarchar(128)**|Nom de la base de données en cours.|  
|**database_size**|**varchar (18)**|Taille de la base de données actuelle en mégaoctets. **database_size** contient à la fois des fichiers de données et des fichiers journaux. Si la base de données a un groupe de fichiers MEMORY_OPTIMIZED_DATA, cela inclut la taille totale sur disque de tous les fichiers de point de contrôle dans le groupe de fichiers.|
|**espace non alloué**|**varchar (18)**|Espace de la base de données qui n'a pas été réservé pour des objets de base de données. Si la base de données a un groupe de fichiers MEMORY_OPTIMIZED_DATA, cela inclut la taille totale sur disque des fichiers de point de contrôle dont l’État est précréé dans le groupe de fichiers.|  
|**réservé**|**varchar (18)**|Quantité totale d'espace allouée par les objets dans la base de données.|  
|**data**|**varchar (18)**|Quantité totale d'espace qu'occupent les données.|  
|**index_size**|**varchar (18)**|Quantité totale d'espace qu'occupent les index.|  
|**inutilisé**|**varchar (18)**|Quantité totale d'espace réservée pour les objets dans la base de données, mais non encore utilisé.|
|**xtp_precreated**|**varchar (18)**|Taille totale des fichiers de point de contrôle dont l’État est précréé, en Ko. Cela compte dans l’ensemble de l’espace non alloué dans la base de données. Retourne NULL si la base de données n’a pas de groupe de fichiers memory_optimized_data avec au moins un conteneur. *Cette colonne est incluse uniquement si @include_total_xtp_storage = 1*.| 
|**xtp_used**|**varchar (18)**|Taille totale des fichiers de point de contrôle avec les États sous CONSTRUCTION, ACTIVE et cible de fusion, en Ko. Il s’agit de l’espace disque activement utilisé pour les données dans les tables optimisées en mémoire. Retourne NULL si la base de données n’a pas de groupe de fichiers memory_optimized_data avec au moins un conteneur. *Cette colonne est incluse uniquement si @include_total_xtp_storage = 1*.| 
|**xtp_pending_truncation**|**varchar (18)**|Taille totale des fichiers de point de contrôle avec l’État WAITING_FOR_LOG_TRUNCATION, en Ko. Il s’agit de l’espace disque utilisé pour les fichiers de point de contrôle en attente de nettoyage. une fois la troncation du journal effectuée. Retourne NULL si la base de données n’a pas de groupe de fichiers memory_optimized_data avec au moins un conteneur. Cette colonne est incluse uniquement si `@include_total_xtp_storage=1` .|

## <a name="remarks"></a>Notes  
 la valeur de **database_size** est généralement supérieure à la somme de l'   +  **espace non alloué** réservé, car elle comprend la taille des fichiers journaux, mais **réservée** et **unallocated_space** prendre en compte uniquement les pages de données. Dans certains cas, avec Azure Synapse Analytics, cette instruction ne peut pas être vraie. 
  
 Les pages utilisées par les index XML et les index de recherche en texte intégral sont incluses dans **index_size** pour les deux jeux de résultats. Lorsque *nom_d* 'objet est spécifié, les pages des index XML et des index de recherche en texte intégral de l’objet sont également comptées dans le résultat total **réservé** et **index_size** .  
  
 Si l’utilisation de l’espace est calculée pour une base de données ou un objet qui a un index spatial, les colonnes de taille d’espace, telles que **database_size**, **réservé** et **index_size**, incluent la taille de l’index spatial.  
  
 Lorsque la valeur *UPDATEUSAGE* est spécifiée, le [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] analyse les pages de données de la base de données et effectue toutes les corrections nécessaires au **sys.allocation_units** et aux affichages catalogue **sys. partitions** concernant l’espace de stockage utilisé par chaque table. Il existe des cas, par exemple après la suppression d'un index, où les informations d'espace de la table peuvent ne plus être à jour. *UPDATEUSAGE* peut prendre un certain temps pour s’exécuter sur de grandes tables ou bases de données. Utilisez *UPDATEUSAGE* uniquement lorsque vous suspectez que des valeurs incorrectes sont retournées et lorsque le processus n’aura pas d’effet négatif sur d’autres utilisateurs ou processus de la base de données. Il est également possible d'exécuter DBCC UPDATEUSAGE séparément.  
  
> [!NOTE]  
>  Lorsque vous supprimez ou reconstruisez des index volumineux ou lorsque vous supprimez ou tronquez des tables volumineuses, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] diffère les désallocations des pages actives et de leurs blocs associés jusqu'à ce que la transaction soit validée. Les opérations de suppression différées ne libèrent pas immédiatement l'espace alloué. Par conséquent, les valeurs retournées par **sp_spaceused** immédiatement après la suppression ou la troncation d’un objet volumineux peuvent ne pas refléter l’espace disque réellement disponible.  
  
## <a name="permissions"></a>Autorisations  
 L’autorisation d’exécuter **sp_spaceused** est accordée au rôle **public** . Seuls les membres du rôle de base de données fixe **db_owner** peuvent spécifier la paramètre **\@updateusage**.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-displaying-disk-space-information-about-a-table"></a>R. Affichage d'informations sur l'espace disque occupé par une table  
 L'exemple qui suit donne des informations sur l'espace disque pour la table `Vendor` et ses index.  
  
```sql  
USE AdventureWorks2012;  
GO  
EXEC sp_spaceused N'Purchasing.Vendor';  
GO  
```  
  
### <a name="b-displaying-updated-space-information-about-a-database"></a>B. Affichage d'informations mises à jour sur l'espace occupé par une base de données  
 L'exemple suivant récapitule l'espace utilisé dans la base de données actuelle et utilise le paramètre facultatif `@updateusage` pour forcer le retour de valeurs actualisées.  
  
```sql  
USE AdventureWorks008R2;  
GO  
EXEC sp_spaceused @updateusage = N'TRUE';  
GO  
```  
  
### <a name="c-displaying-space-usage-information-about-the-remote-table-associated-with-a-stretch-enabled-table"></a>C. Affichage des informations sur l’utilisation de l’espace de la table distante associée à une table avec extension Stretch  
 L’exemple suivant résume l’espace utilisé par la table distante associée à une table avec extension Stretch à l’aide de l’argument **\@ mode** pour spécifier la cible distante. Pour plus d'informations, consultez [Stretch Database](../../sql-server/stretch-database/stretch-database.md).  
  
```sql  
USE StretchedAdventureWorks2016  
GO  
EXEC sp_spaceused N'Purchasing.Vendor', @mode = 'REMOTE_ONLY'  
```  
  
### <a name="d-displaying-space-usage-information-for-a-database-in-a-single-result-set"></a>D. Affichage des informations sur l’utilisation de l’espace pour une base de données dans un seul jeu de résultats  
 L’exemple suivant résume l’utilisation de l’espace pour la base de données active dans un seul jeu de résultats.  
  
```sql  
USE AdventureWorks2016  
GO  
EXEC sp_spaceused @oneresultset = 1  
```  

### <a name="e-displaying-space-usage-information-for-a-database-with-at-least-one-memory_optimized-file-group-in-a-single-result-set"></a>E. Affichage des informations sur l’utilisation de l’espace pour une base de données avec au moins un groupe de fichiers MEMORY_OPTIMIZED dans un seul jeu de résultats 
 L’exemple suivant résume l’utilisation de l’espace pour la base de données active avec au moins un MEMORY_OPTIMIZED groupe de fichiers dans un seul jeu de résultats.
 
```sql
USE WideWorldImporters
GO
EXEC sp_spaceused @updateusage = 'FALSE', @mode = 'ALL', @oneresultset = '1', @include_total_xtp_storage = '1';
GO
``` 

### <a name="f-displaying-space-usage-information-for-a-memory_optimized-table-object-in-a-database"></a>F. Affichage des informations sur l’utilisation de l’espace pour un objet de table MEMORY_OPTIMIZED dans une base de données.
 L’exemple suivant résume l’utilisation de l’espace pour un objet de table MEMORY_OPTIMIZED dans la base de données active avec au moins un groupe de fichiers MEMORY_OPTIMIZED.
 
```sql
USE WideWorldImporters
GO
EXEC sp_spaceused
@objname = N'VehicleTemparatures',
@updateusage = 'FALSE',
@mode = 'ALL',
@oneresultset = '0',
@include_total_xtp_storage = '1';
GO
```  

## <a name="see-also"></a>Voir aussi  
 [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md)   
 [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)   
 [DBCC UPDATEUSAGE &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-updateusage-transact-sql.md)   
 [SQL Server Service Broker](../../database-engine/configure-windows/sql-server-service-broker.md)   
 [sys.allocation_units &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-allocation-units-transact-sql.md)   
 [sys.indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)   
 [sys.index_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-index-columns-transact-sql.md)   
 [sys.objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)   
 [sys.partitions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-partitions-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
