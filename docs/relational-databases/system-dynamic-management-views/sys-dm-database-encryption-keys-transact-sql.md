---
description: sys.dm_database_encryption_keys (Transact-SQL)
title: sys.dm_database_encryption_keys (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/27/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_database_encryption_keys
- sys.dm_database_encryption_keys_TSQL
- dm_database_encryption_keys
- dm_database_encryption_keys_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_database_encryption_keys dynamic management view
ms.assetid: 56fee8f3-06eb-4fff-969e-abeaa0c4b8e4
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 96bf6756f26e0bc8b82c6f7f008203fcb206c22f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184344"
---
# <a name="sysdm_database_encryption_keys-transact-sql"></a>sys.dm_database_encryption_keys (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Retourne des informations sur l'état de chiffrement d'une base de données et de ses clés de chiffrement de base de données associées. Pour plus d’informations sur le chiffrement des bases de données, consultez [Chiffrement transparent des données &#40;TDE&#41;](../../relational-databases/security/encryption/transparent-data-encryption.md).  
 
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|database_id|**int**|ID de la base de données.|  
|encryption_state|**int**|Indique si la base de données est chiffrée ou non chiffrée.<br /><br /> 0 = aucune clé de chiffrement de base de données présente, pas de chiffrement<br /><br /> 1 = non chiffré<br /><br /> 2 = chiffrement en cours<br /><br /> 3 = chiffrée.<br /><br /> 4 = modification de clé en cours<br /><br /> 5 = déchiffrement en cours<br /><br /> 6 = modification de la protection en cours (Le certificat ou la clé asymétrique qui chiffre la clé de chiffrement de base de données est en cours de modification.)|  
|create_date|**datetime**|Affiche la date (au format UTC) à laquelle la clé de chiffrement a été créée.|  
|regenerate_date|**datetime**|Affiche la date (au format UTC) à laquelle la clé de chiffrement a été régénérée.|  
|modify_date|**datetime**|Affiche la date (au format UTC) à laquelle la clé de chiffrement a été modifiée.|  
|set_date|**datetime**|Affiche la date (au format UTC) à laquelle la clé de chiffrement a été appliquée à la base de données.|  
|opened_date|**datetime**|Indique la date et l’heure de la dernière ouverture de la clé de base de données (au format UTC).|  
|key_algorithm|**nvarchar(32)**|Affiche l'algorithme utilisé pour la clé.|  
|key_length|**int**|Affiche la longueur de la clé.|  
|encryptor_thumbprint|**varbinary(20)**|Affiche l'empreinte numérique du chiffreur.|  
|encryptor_type|**nvarchar(32)**|**S'applique à**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] via la [version actuelle](/troubleshoot/sql/general/determine-version-edition-update-level)).<br /><br /> Décrit le chiffreur.|  
|percent_complete|**real**|Pourcentage accompli de la modification de l'état de chiffrement de la base de données. La valeur 0 indique aucune modification d'état.|
|encryption_state_desc|**nvarchar(32)**|**S’applique à** : [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] et versions ultérieures.<br><br> Chaîne qui indique si la base de données est chiffrée ou non chiffrée.<br><br>Aucune<br><br>NON chiffrés<br><br>COMMUNICATIONS<br><br>DECRYPTION_IN_PROGRESS<br><br>ENCRYPTION_IN_PROGRESS<br><br>KEY_CHANGE_IN_PROGRESS<br><br>PROTECTION_CHANGE_IN_PROGRESS|
|encryption_scan_state|**int**|**S’applique à** : [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] et versions ultérieures.<br><br>Indique l’état actuel de l’analyse de chiffrement. <br><br>0 = aucune analyse n’a été lancée, TDE n’est pas activé<br><br>1 = analyse en cours.<br><br>2 = l’analyse est en cours mais a été suspendue, l’utilisateur peut la reprendre.<br><br>3 = l’analyse a été abandonnée pour une raison quelconque, une intervention manuelle est nécessaire. Pour obtenir de l’aide, contactez Support Microsoft.<br><br>4 = l’analyse a été effectuée avec succès, TDE est activé et le chiffrement est terminé.|
|encryption_scan_state_desc|**nvarchar(32)**|**S’applique à** : [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] et versions ultérieures.<br><br>Chaîne qui indique l’état actuel de l’analyse de chiffrement.<br><br> Aucune<br><br>RUNNING<br><br>SUSPENDED<br><br>ABORTED<br><br>Remplissez|
|encryption_scan_modify_date|**datetime**|**S’applique à** : [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] et versions ultérieures.<br><br> Affiche la date (au format UTC) de la dernière modification de l’état de l’analyse de chiffrement.|
  
## <a name="permissions"></a>Autorisations

Sur [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] , requiert l' `VIEW SERVER STATE` autorisation.   
Sur SQL Database objectifs de service de base, S0 et S1, et pour les bases de données dans des pools élastiques, le `Server admin` ou un `Azure Active Directory admin` compte est requis. Pour tous les autres SQL Database objectifs de service, l' `VIEW DATABASE STATE` autorisation est requise dans la base de données.   

## <a name="see-also"></a>Voir aussi  

 [Fonctions et vues de gestion dynamique relatives à la sécurité &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/security-related-dynamic-management-views-and-functions-transact-sql.md)   
 [Chiffrement transparent des données &#40;TDE&#41;](../../relational-databases/security/encryption/transparent-data-encryption.md)   
 [Chiffrement SQL Server](../../relational-databases/security/encryption/sql-server-encryption.md)   
 [SQL Server et clés de chiffrement de base de données &#40;moteur de base de données&#41;](../../relational-databases/security/encryption/sql-server-and-database-encryption-keys-database-engine.md)   
 [Hiérarchie de chiffrement](../../relational-databases/security/encryption/encryption-hierarchy.md)   
 [Options ALTER DATABASE SET &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md)   
 [CREATE DATABASE ENCRYPTION KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-database-encryption-key-transact-sql.md)   
 [ALTER DATABASE ENCRYPTION KEY &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-encryption-key-transact-sql.md)   
 [DROP DATABASE ENCRYPTION KEY &#40;Transact-SQL&#41;](../../t-sql/statements/drop-database-encryption-key-transact-sql.md)  
  
