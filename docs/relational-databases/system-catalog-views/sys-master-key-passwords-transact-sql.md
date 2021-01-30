---
description: sys.master_key_passwords (Transact-SQL)
title: sys.master_key_passwords (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 04/06/2020
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.master_key_passwords_TSQL
- master_key_passwords_TSQL
- sys.master_key_passwords
- master_key_passwords
dev_langs:
- TSQL
helpviewer_keywords:
- sys.master_key_passwords catalog view
ms.assetid: b8e18cff-a9e6-4386-98ce-1cd855506e03
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: e5aeeb3cf62d687b6f59a93ba07ed19b5f7e3fce
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191349"
---
# <a name="sysmaster_key_passwords-transact-sql"></a>sys.master_key_passwords (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Retourne une ligne pour chaque mot de passe de clé principale de base de données ajouté à l’aide de la procédure stockée **sp_control_dbmasterkey_password** . Les mots de passe utilisés pour protéger les clés principales sont stockés dans la banque d'informations d'identification. Le nom des informations d’identification respecte le format suivant : # #DBMKEY_<database_family_guid>_<random_password_guid> # #. Le mot de passe est stocké en tant que secret des informations d'identification. Pour chaque mot de passe ajouté à l’aide de **sp_control_dbmasterkey_password**, il existe une ligne dans **sys. Credentials**.  
  
 Chaque ligne de cette vue affiche une **credential_id** et la **family_guid** d’une base de données dont la clé principale est protégée par le mot de passe associé à ces informations d’identification. Une jointure avec **sys. Credentials** sur le **credential_id** retourne des champs utiles, tels que le **create_date** et le nom des informations d’identification.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**credential_id**|**int**|ID de l'information d'identification à laquelle appartient le mot de passe. Cet ID est unique dans l'instance du serveur.|  
|**family_guid**|**uniqueidentifier**|ID unique de la base de données d'origine lors de sa création. Ce GUID reste identique après la restauration ou l'association de la base de données, même si le nom de la base de données est modifié.<br /><br /> Si le déchiffrement automatique par la clé principale du service échoue, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilise le **family_guid** pour identifier les informations d’identification qui peuvent contenir le mot de passe utilisé pour protéger la clé principale de la base de données.|  
  
## <a name="permissions"></a>Autorisations  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [sp_control_dbmasterkey_password &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-control-dbmasterkey-password-transact-sql.md)   
 [Affichages catalogue de sécurité &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)   
 [CREATE SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-symmetric-key-transact-sql.md)   
 [Hiérarchie de chiffrement](../../relational-databases/security/encryption/encryption-hierarchy.md)  
  
  
