---
description: BACKUP SERVICE MASTER KEY (Transact-SQL)
title: BACKUP SERVICE MASTER KEY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- BACKUP SERVICE MASTER KEY
- DUMP_SERVICE_MASTER_KEY_TSQL
- DUMP SERVICE MASTER KEY
- BACKUP_SERVICE_MASTER_KEY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- backing up service master keys [SQL Server]
- BACKUP SERVICE MASTER KEY statement
- cryptography [SQL Server], Service Master Key
- exporting Service Master Keys
- encryption [SQL Server], Service Master Key
- service master key [SQL Server], exporting
ms.assetid: f8356683-6680-4f1c-9eaf-5c29a9a9020d
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 578919b8583090494b762400b7ad49541d824320
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199368"
---
# <a name="backup-service-master-key-transact-sql"></a>BACKUP SERVICE MASTER KEY (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Permet d'exporter la clé principale de base de service.  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
BACKUP SERVICE MASTER KEY TO FILE = 'path_to_file'   
    ENCRYPTION BY PASSWORD = 'password'  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 FILE **='** _chemin_vers_fichier_*_'_*  
 Spécifie le chemin d'accès complet, y compris le nom de fichier, du fichier dans lequel la clé principale de service sera exportée. Cela peut être un chemin d'accès local ou le chemin UNC d'un emplacement réseau.  
  
 PASSWORD **='** _mot de passe_*_'_*  
 Mot de passe utilisé pour chiffrer la clé principale de service dans le fichier de sauvegarde. Ce mot de passe est sujet à des vérifications de complexité. Pour plus d'informations, consultez [Password Policy](../../relational-databases/security/password-policy.md).  
  
## <a name="remarks"></a>Notes  
 La clé principale de service doit être sauvegardée et stockée en lieu sûr, en dehors de votre lieu de travail. La création de cette sauvegarde doit être l'une des premières actions administratives effectuées sur le serveur.  
  
## <a name="permissions"></a>Autorisations  
 Requiert l'autorisation CONTROL SERVER sur le serveur.  
  
## <a name="examples"></a>Exemples  
 Dans l'exemple ci-dessous la clé principale de service est sauvegardée dans un fichier.  
  
```sql  
BACKUP SERVICE MASTER KEY TO FILE = 'c:\temp_backups\keys\service_master_key' ENCRYPTION BY PASSWORD = '3dH85Hhk003GHk2597gheij4';  
```  
  
## <a name="see-also"></a>Voir aussi  
 [ALTER SERVICE MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/alter-service-master-key-transact-sql.md)   
 [RESTORE SERVICE MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-service-master-key-transact-sql.md)  
  
  
