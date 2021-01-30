---
description: sp_filestream_force_garbage_collection (Transact-SQL)
title: sp_filestream_force_garbage_collection (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/22/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_filestream_force_garbage_collection
- sp_filestream_force_garbage_collection_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- FILESTREAM [SQL Server]
- sp_filestream_force_garbage_collection
ms.assetid: 9d1efde6-8fa4-42ac-80e5-37456ffebd0b
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 0c234c5c03b00b25babbec0a4fbc197f235c966d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99165252"
---
# <a name="sp_filestream_force_garbage_collection-transact-sql"></a>sp_filestream_force_garbage_collection (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Force le garbage collector FILESTREAM à s'exécuter, en supprimant tous fichiers FILESTREAM inutiles.  
  
 Un conteneur FILESTREAM ne peut pas être supprimé tant que tous les fichiers supprimés qu'il contient n'ont pas été nettoyés par le garbage collector. Le garbage collector FILESTREAM s'exécute automatiquement. Toutefois, si vous devez supprimer un conteneur avant l’exécution du garbage collector, vous pouvez utiliser sp_filestream_force_garbage_collection pour exécuter le garbage collector manuellement.  
  
  
## <a name="syntax"></a>Syntaxe  
  
```  
sp_filestream_force_garbage_collection
    [ @dbname = ]  'database_name'
    [ , [ @filename = ] 'logical_file_name' ]
```  
  
## <a name="arguments"></a>Arguments  
 `[ @dbname = ]  'database_name'`  
 Représente le nom de la base de données dans laquelle exécuter le garbage collector.  
  
> [!NOTE]  
> `@dbname` est de type **sysname**. En l'absence de spécification, c'est la base de données actuelle qui est utilisée.  
  
 `[ @filename = ] 'logical_file_name'`  
 Spécifie le nom logique du conteneur FILESTREAM dans lequel exécuter le garbage collector. `@filename` est facultatif. Si aucun nom de fichier logique n’est spécifié, le garbage collector nettoie tous les conteneurs FILESTREAM dans la base de données spécifiée.  
  
## <a name="return-code-values"></a>Codet de retour  
  
| Valeur | Description |
| ----- | ----------- |   
|0|Réussite de l'opération|  
|1|Échec de l'opération|  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Valeur|Description|  
|-----------|-----------------|  
|*file_name*|Indique le nom de conteneur FILESTREAM|  
|*num_collected_items*|Indique le nombre d'éléments FILESTREAM (fichiers/répertoires) qui ont été récupérés par le garbage collector (supprimés) dans ce conteneur.|  
|*num_marked_for_collection_items*|Indique le nombre d'éléments FILESTREAM (fichiers/répertoires) qui ont été marqués pour le garbage collection dans ce conteneur. Ces éléments n’ont pas encore été supprimés, mais peuvent être supprimés à la suite de la phase garbage collection.|  
|*num_unprocessed_items*|Indique le nombre d'éléments FILESTREAM éligibles (fichiers ou répertoires) qui n'ont pas été traités pour le garbage collection dans ce conteneur FILESTREAM. Les éléments peuvent être inexploités pour différentes raisons, notamment :<br /><br /> Fichiers qui doivent être épinglés car la sauvegarde de journal ou le point de contrôle n'a pas été accepté.<br /><br /> Fichiers en mode de récupération FULL ou BULK_LOGGED.<br /><br /> Il existe une transaction active de longue durée.<br /><br /> Le travail du lecteur du journal de réplication n’a pas été exécuté. Pour plus d’informations, consultez le livre blanc [stockage FILESTREAM dans SQL Server 2008](/previous-versions/sql/sql-server-2008/hh461480(v=msdn.10)) .|  
|*last_collected_xact_seqno*|Retourne le dernier numéro séquentiel dans le journal correspondant (LSN) jusqu'aux fichiers récupérés par le garbage collector pour le conteneur FILESTREAM spécifié.|  
  
## <a name="remarks"></a>Notes  
 Exécute explicitement la tâche de Garbage collector FILESTREAM à l'achèvement sur la base de données demandée (et le conteneur FILESTREAM). Les fichiers qui ne sont plus nécessaires sont supprimés par le processus de garbage collection. La durée d'exécution de cette opération varie selon la taille des données FILESTREAM dans cette base de données ou ce conteneur, ainsi qu'en fonction de l'activité DML qui s'est produite récemment sur les données FILESTREAM. Bien que cette opération puisse être exécutée avec la base de données en ligne, cela peut affecter les performances de la base de données pendant son exécution en raison des différentes activités d'entrées/sorties effectuées par le processus de garbage collection.  
  
> [!NOTE]  
>  Il est recommandé de n'exécuter cette opération que lorsque cela est nécessaire et en dehors des heures d'activité habituelles.  
  
Plusieurs appels de cette procédure stockée peuvent être exécutés simultanément uniquement sur des conteneurs distincts ou des bases de données distinctes.  

En raison des opérations en 2 phases, la procédure stockée doit être exécutée deux fois pour supprimer les fichiers FILESTREAM sous-jacents.  

Le garbage collection (GC) s’appuie sur la troncation du journal. Par conséquent, si des fichiers ont été supprimés récemment sur une base de données à l’aide du mode de récupération complète, ils sont GC uniquement après une sauvegarde de fichier journal de ces portions du journal des transactions et la partie du journal est marquée comme inactive. Sur une base de données utilisant le mode de récupération simple, une troncation du journal se produit après `CHECKPOINT` l’émission d’un sur la base de données.  


## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle de base de données db_owner.  
  
## <a name="examples"></a>Exemples  
 Les exemples suivants exécutent le garbage collector pour les conteneurs FILESTREAM dans la base de données `FSDB`.  
  
### <a name="a-specifying-no-container"></a>R. Absence de spécification d'un conteneur  
  
```sql  
USE FSDB;  
GO  
EXEC sp_filestream_force_garbage_collection @dbname = N'FSDB';  
```  
  
### <a name="b-specifying-a-container"></a>B. Spécification d'un conteneur  
  
```sql  
USE FSDB;  
GO  
EXEC sp_filestream_force_garbage_collection @dbname = N'FSDB',
    @filename = N'FSContainer';  
```  
  
## <a name="see-also"></a>Voir aussi  
[FileStream](../../relational-databases/blob/filestream-sql-server.md)
<br>[Filetables](../../relational-databases/blob/filetables-sql-server.md)
<br>[Vues de gestion dynamiques Filestream et FileTable (Transact-SQL)](../system-dynamic-management-views/filestream-and-filetable-dynamic-management-views-transact-sql.md)
<br>[Vues de catalogue Filestream et FileTable (Transact-SQL)](../system-catalog-views/filestream-and-filetable-catalog-views-transact-sql.md)
<br>[sp_kill_filestream_non_transacted_handles (Transact-SQL)](filestream-and-filetable-sp-kill-filestream-non-transacted-handles.md)
  
