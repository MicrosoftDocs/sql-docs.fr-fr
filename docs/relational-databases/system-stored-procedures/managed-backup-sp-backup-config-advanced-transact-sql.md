---
description: managed_backup.sp_backup_config_advanced managed_backup (Transact-SQL)
title: managed_backup managed_backup.sp_backup_config_advanced (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_backup_config_optional
- sp_backup_config_optional_TSQL
- managed_backup.sp_backup_config_optional_TSQL
- managed_backup.sp_backup_config_optional
dev_langs:
- TSQL
helpviewer_keywords:
- sp_backup_config_optional
- managed_backup.sp_backup_config_optional
ms.assetid: 4fae8193-1f88-48fd-a94a-4786efe8d6af
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 77b56d757d685e30798c8763c948dbb72e89c2d9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203729"
---
# <a name="managed_backupsp_backup_config_advanced-transact-sql"></a>managed_backup.sp_backup_config_advanced managed_backup (Transact-SQL)
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

  Configure les paramètres avancés pour [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] .  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```sql  
EXEC managed_backup.sp_backup_config_advanced   
    [@database_name = ] 'database_name'  
    ,[@encryption_algorithm = ] 'name of the encryption algorithm'  
    ,[@encryptor_type = ] {'CERTIFICATE' | 'ASYMMETRIC_KEY'}  
    ,[@encryptor_name = ] 'name of the certificate or asymmetric key'  
    ,[@local_cache_path = ] 'NOT AVAILABLE'  
```  
  
##  <a name="arguments"></a><a name="Arguments"></a> Arguments  
 @database_name  
 Nom de la base de données pour activer la sauvegarde managée sur une base de données spécifique. Si la valeur est NULL ou *, cette sauvegarde managée s’applique à toutes les bases de données sur le serveur.  
  
 @encryption_algorithm  
 Nom de l'algorithme de chiffrement utilisé lors de la sauvegarde pour chiffrer le fichier de sauvegarde. @encryption_algorithmEst de **type sysname**. C'est un paramètre requis lorsque vous configurez la [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] pour la première fois pour la base de données. Spécifiez **NO_ENCRYPTION** si vous ne souhaitez pas chiffrer le fichier de sauvegarde. Lorsque vous modifiez les [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] paramètres de configuration, ce paramètre est facultatif. Si le paramètre n’est pas spécifié, les valeurs de configuration existantes sont conservées. Les valeurs autorisées pour ce paramètre sont :  
  
-   AES_128  
  
-   AES_192  
  
-   AES_256  
  
-   TRIPLE_DES_3KEY  
  
-   NO_ENCRYPTION  
  
 Pour plus d'informations sur les algorithmes de chiffrement, consultez [Choose an Encryption Algorithm](../../relational-databases/security/encryption/choose-an-encryption-algorithm.md).  
  
 @encryptor_type  
 Type de chiffreur, qui peut être « CERTIFICATe » ou « ASYMMETRIC_KEY ». Est de type @encryptor_type **nvarchar (32)**. Ce paramètre est facultatif si vous spécifiez NO_ENCRYPTION pour le @encryption_algorithm paramètre.  
  
 @encryptor_name  
 Nom d'un certificat ou d'une clé asymétrique qui existe, utilisé pour chiffrer la sauvegarde. @encryptor_nameEst de **type sysname**. Si vous utilisez une clé asymétrique, elle doit être configurée avec la gestion de clés extensible (EKM). Ce paramètre est facultatif si vous spécifiez NO_ENCRYPTION pour le @encryption_algorithm paramètre.  
  
 Pour plus d’informations, consultez [gestion de clés Extensible &#40;EKM&#41;](../../relational-databases/security/encryption/extensible-key-management-ekm.md).  
  
 @local_cache_path  
 Ce paramètre n’est pas encore pris en charge.  
  
## <a name="return-code-value"></a>Valeur du code de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="security"></a>Sécurité  
  
### <a name="permissions"></a>Autorisations  
 Requiert l’appartenance au rôle de base de données **db_backupoperator** , avec les autorisations **ALTER ANY CREDENTIAL** et **Execute** sur **sp_delete_backuphistory** procédure stockée.  
  
## <a name="examples"></a>Exemples  
 L’exemple suivant définit des options de configuration avancées pour [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] pour l’instance de SQL Server.  
  
```sql
Use msdb;  
Go  
   EXEC managed_backup.sp_backup_config_advanced  
                @encryption_algorithm ='AES_128'  
                ,@encryptor_type = 'CERTIFICATE'  
                ,@encryptor_name = 'MyTestDBBackupEncryptCert'  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [managed_backup.sp_backup_config_basic managed_backup (Transact-SQL)](../../relational-databases/system-stored-procedures/managed-backup-sp-backup-config-basic-transact-sql.md)   
 [managed_backup.sp_backup_config_schedule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/managed-backup-sp-backup-config-schedule-transact-sql.md)  
  
  
