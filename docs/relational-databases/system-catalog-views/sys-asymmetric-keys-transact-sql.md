---
description: sys.asymmetric_keys (Transact-SQL)
title: sys.asymmetric_keys (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- asymmetric_keys
- sys.asymmetric_keys_TSQL
- asymmetric_keys_TSQL
- sys.asymmetric_keys
dev_langs:
- TSQL
helpviewer_keywords:
- sys.asymmetric_keys catalog view
ms.assetid: bbca796a-9bb5-4a62-9ca8-1d255984553d
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 29748e3c349618a79f4cab3ec0f0355a82963b7c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99158154"
---
# <a name="sysasymmetric_keys-transact-sql"></a>sys.asymmetric_keys (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Renvoie une ligne pour chaque clé asymétrique.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**name**|**sysname**|Nom de la clé. Unique dans la base de données.|  
|**principal_id**|**int**|ID du principal de la base de données propriétaire de la clé.|  
|**asymmetric_key_id**|**int**|ID de la clé. Unique dans la base de données.|  
|**pvt_key_encryption_type**|**char(2)**|Mode de chiffrement de la clé.<br /><br /> NA = Non chiffrée<br /><br /> MK = La clé est chiffrée par la clé principale<br /><br /> PW = La clé est chiffrée par un mot de passe défini par l'utilisateur<br /><br /> SK = La clé est chiffrée par la clé principale du service.|  
|**pvt_key_encryption_type_desc**|**nvarchar(60)**|Description du mode de chiffrement de la clé privée.<br /><br /> NO_PRIVATE_KEY<br /><br /> ENCRYPTED_BY_MASTER_KEY<br /><br /> ENCRYPTED_BY_PASSWORD<br /><br /> ENCRYPTED_BY_SERVICE_MASTER_KEY|  
|**d**|**varbinary(32)**|Hachage SHA-1 de la clé. Hachage globalement unique.|  
|**utilisé**|**char(2)**|Algorithme utilisé avec la clé.<br /><br /> 1R = RSA 512 bits<br /><br /> 2R = RSA 1024 bits<br /><br /> 3R = RSA 2048 bits|  
|**algorithm_desc**|**nvarchar(60)**|Description de l'algorithme utilisé avec la clé.<br /><br /> RSA_512<br /><br /> RSA_1024<br /><br /> RSA_2048|  
|**key_length**|**int**|Longueur en bits de la clé.|  
|**sid**|**varbinary(85)**|SID de connexion pour cette clé. Pour les clés EKM (Gestion de clés extensible), cette valeur sera NULL.|  
|**string_sid**|**nvarchar(128)**|Représentation de chaîne du SID de connexion de la clé. Pour les clés EKM (Gestion de clés extensible), cette valeur sera NULL.|  
|**public_key**|**varbinary(max)**|Clé publique.|  
|**attested_by**|**nvarchar(260)**|Utilisation réservée au système.|  
|**provider_type**|**nvarchar(120)**|Type du fournisseur de chiffrement.<br /><br /> CRYPTOGRAPHIC PROVIDER = Clés EKM (Gestion de clés extensible)<br /><br /> NULL = Clés non-EKM (Gestion de clés extensible)|  
|**cryptographic_provider_guid**|**uniqueidentifier**|GUID du fournisseur de chiffrement. Pour les clés non-EKM (Gestion de clés extensible), cette valeur sera NULL.|  
|**cryptographic_provider_algid**|**sql_variant**|ID d'algorithme pour le fournisseur de chiffrement. Pour les clés non-EKM (Gestion de clés extensible), cette valeur sera NULL.|  
  
## <a name="permissions"></a>Autorisations  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Affichages catalogue de sécurité &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)   
 [Gestion de clés extensible &#40;EKM&#41;](../../relational-databases/security/encryption/extensible-key-management-ekm.md)   
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Hiérarchie de chiffrement](../../relational-databases/security/encryption/encryption-hierarchy.md)   
 [CREATE ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-asymmetric-key-transact-sql.md)  
  
  
