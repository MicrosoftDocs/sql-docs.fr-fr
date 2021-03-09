---
description: sys.database_recovery_status (Transact-SQL)
title: sys.database_recovery_status (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/12/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- database_recovery_status_TSQL
- database_recovery_status
- sys.database_recovery_status
- sys.database_recovery_status_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.database_recovery_status catalog view
ms.assetid: 46fab234-1542-49be-8edf-aa101e728acf
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 91698461a94705f43da35b72339d1ed71c37bcce
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/08/2021
ms.locfileid: "102463984"
---
# <a name="sysdatabase_recovery_status-transact-sql"></a>sys.database_recovery_status (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contient une ligne par base de données. Si la base de données n'est pas ouverte, [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] tente de la démarrer.  
  
 Pour afficher la ligne d’une base de données autre que **Master** ou **tempdb**, l’une des conditions suivantes doit s’appliquer :  
  
-   Être le propriétaire de la base de données.  
  
-   Posséder des autorisations au niveau du serveur ALTER ANY DATABASE ou VIEW ANY DATABASE.  
  
-   Disposer de l’autorisation CREATe DATABASE dans la base de données **Master** .    
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**database_id**|**int**|ID de la base de données, unique dans une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**database_guid**|**uniqueidentifier**|Utilisé pour associer ensemble tous les fichiers de base de données d'une base de données. Tous les fichiers possèdent ce GUID dans leur page d'en-tête afin que la base de données démarre comme prévu. Une seule base de données doit posséder ce GUID, mais des doublons peuvent être créés en copiant et en joignant des bases de données. RESTORE génère toujours un nouveau GUID lorsque vous restaurez une base de données qui n'existe pas encore.<br /><br /> NULL= Base de données hors connexion, ou la base de données ne démarrera pas.|  
|**family_guid**|**uniqueidentifier**|Identificateur de la « famille de sauvegarde » de la base de données pour détecter les états de restauration correspondants.<br /><br /> NULL = la base de données est hors connexion ou la base de données ne démarre pas.|  
|**last_log_backup_lsn**|**numeric(25,0)**|Numéro séquentiel dans le journal de début de la prochaine sauvegarde de journal.<br /><br /> Si la valeur est NULL, la sauvegarde du journal des transactions ne peut pas être effectuée parce que la base de données est en récupération SIMPLE ou qu’il n’existe aucune sauvegarde de base de données actuelle.|  
|**recovery_fork_guid**|**uniqueidentifier**|Identifie la fourchette de récupération en cours sur laquelle la base de données est actuellement active.<br /><br /> NULL= Base de données hors connexion, ou la base de données ne démarrera pas.|  
|**first_recovery_fork_guid**|**uniqueidentifier**|Identificateur de la fourchette de récupération de début.<br /><br /> NULL= Base de données hors connexion, ou la base de données ne démarrera pas.|  
|**fork_point_lsn**|**numeric(25,0)**|Si **first_recovery_fork_guid** n’est pas égal à ( ! =) pour **recovery_fork_guid**, **fork_point_lsn** est le numéro séquentiel dans le journal du point de branchement actuel. Dans le cas contraire, la valeur est NULL.|  
  
## <a name="permissions"></a>Autorisations  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Affichages catalogue de bases de données et de fichiers &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/databases-and-files-catalog-views-transact-sql.md)   
 [RESTORE HEADERONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-headeronly-transact-sql.md)   
 [Questions fréquentes sur l'interrogation des catalogues système de SQL Server](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.yml)  
  
  
