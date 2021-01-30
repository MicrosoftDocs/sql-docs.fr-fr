---
description: managed_backup.fn_backup_db_config managed_backup (Transact-SQL)
title: managed_backup managed_backup.fn_backup_db_config (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- smart_admin.fn_backup_db_config
- smart_admin.fn_backup_db_config_TSQL
- fn_backup_db_config
- fn_backup_db_config_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- smart_admin.fn_backup_db_config
- fn_backup_db_config
ms.assetid: 7c755d8a-64dd-44b2-be5e-735d30758900
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 01d2ecc1fffc66d92508b71951ca05d33a224cff
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207426"
---
# <a name="managed_backupfn_backup_db_config-transact-sql"></a>managed_backup.fn_backup_db_config managed_backup (Transact-SQL)
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

  Retourne 0, 1 ou plus de lignes avec les paramètres de configuration de la [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)]. Retourne une ligne pour la base de données spécifiée, ou retourne les informations de toutes les bases de données configurées avec la [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] sur l'instance.  
  
 Utilisez cette procédure stockée pour consulter ou déterminer les paramètres de configuration de la [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] pour une base de données ou toutes les bases de données sur une instance de SQL Server.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```sql  
managed_backup.fn_backup_db_config ('database_name' | '' | NULL)  
```  
  
##  <a name="arguments"></a><a name="Arguments"></a> Arguments  
 @db_name  
 Nom de la base de données. Le @db_name paramètre est de **type sysname**. Si une chaîne vide ou une valeur NULL est passée à ce paramètre, les informations de toutes les bases de données sur l'instance de SQL Server sont retournées.  
  
## <a name="table-returned"></a>Table retournée  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|db_name|SYSNAME|Nom de la base de données.|  
|db_guid|UNIQUEIDENTIFIER|Identificateur qui identifie la base de données de façon unique.|  
|is_availability_database|BIT|Indique si la base de données participe à un groupe de disponibilité. La valeur 1 indique que la base de données est une base de données de disponibilité, et la valeur 0 indique le contraire.|  
|is_dropped|BIT|La valeur 1 indique que c'est une base de données supprimée.|  
|credential_name|SYSNAME|Nom des informations d'identification SQL utilisées pour authentifier le compte de stockage. Une valeur NULL indique qu'aucune information d'identification SQL n'a été définie.|  
|retention_days|INT|Période de rétention actuelle, en jours. Une valeur NULL indique que la [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] n'a jamais été configurée pour cette base de données.|  
|is_managed_backup_enabled|INT|Indique si la [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] est actuellement activée pour cette base de données. Une valeur 1 indique que la [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] est actuellement activée, et une valeur 0 indique que la [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] est désactivée pour cette base de données.|  
|storage_url|NVARCHAR (1024)|URL du compte de stockage.|  
|Encryption_algorithm|NCHAR (20)|Retourne l'algorithme de chiffrement actuel à utiliser lors du chiffrement de la sauvegarde.|  
|Encryptor_type|NCHAR(15)|Retourne le paramètre de chiffreur : certificat ou clé asymétrique.|  
|Encryptor_name|NCHAR(max_length_of_cert/asymm_key_name)|Nom du certificat ou de la clé asymétrique.|  
  
## <a name="security"></a>Sécurité  
  
### <a name="permissions"></a>Autorisations  
 Requiert l’appartenance au rôle de base de données **db_backupoperator** avec les autorisations **ALTER ANY CREDENTIAL** . L’utilisateur ne doit pas être autorisé à **afficher les autorisations de définition** .  
  
## <a name="examples"></a>Exemples  
 L’exemple suivant retourne la [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] configuration de « TestDB »  
  
 Pour chaque extrait de code, sélectionnez « tsql » dans le champ d'attribut de langage.  
  
```  
Use msdb  
GO  
SELECT * FROM managed_backup.fn_backup_db_config('TestDB')  
```  
  
 L'exemple suivant renvoie la configuration de la [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] pour toutes les bases de données sur l'instance de SQL Server sur laquelle elle est exécutée.  
  
```  
Use msdb  
GO  
SELECT * FROM managed_backup.fn_backup_db_config (NULL)  
```  
  
  
