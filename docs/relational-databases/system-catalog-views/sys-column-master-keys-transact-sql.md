---
description: sys.column_master_keys (Transact-SQL)
title: sys.column_master_keys (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/15/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- column_master_key_definitions_TSQL
- column_master_key_definitions
- sys.column_master_key_definitions_TSQL
- sys.column_master_key_definitions
- column_master_keys_TSQL
- column_master_keys
- sys.column_master_keys_TSQL
- sys.column_master_keys
dev_langs:
- TSQL
helpviewer_keywords:
- sys.column_master_key_definitions catalog view
- sys.column_master_keys catalog view
ms.assetid: fbec2efa-5fe9-4121-9b34-60497b0b2aca
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 33682106df449dbdd1dd1c4244dffe89118a2964
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99161418"
---
# <a name="syscolumn_master_keys-transact-sql"></a>sys.column_master_keys (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

  Retourne une ligne pour chaque clé principale de base de données ajoutée à l’aide de l’instruction [Create Master Key](../../t-sql/statements/create-column-master-key-transact-sql.md) . Chaque ligne représente une clé principale de colonne unique (CMK).  
    
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**name**|**sysname**|Nom du CMK.|  
|**column_master_key_id**|**int**|ID de la clé principale de colonne.|  
|**create_date**|**datetime**|Date de création de la clé principale de colonne.|  
|**modify_date**|**datetime**|Date de la dernière modification de la clé principale de colonne.|  
|**key_store_provider_name**|**sysname**|Nom du fournisseur pour le magasin de clés principales de colonne qui contient CMK. Les valeurs autorisées sont les suivantes :<br /><br /> MSSQL_CERTIFICATE_STORE-si le magasin de clés principales de colonne est un magasin de certificats.<br /><br /> Valeur définie par l’utilisateur, si le magasin de clés principales de colonne est d’un type personnalisé.|  
|**key_path**|**nvarchar(4000)**|Chemin d’accès spécifique au magasin de clés principales de colonne pour la clé. Le format du chemin d’accès dépend du type de magasin de clés principales de colonne. Exemple :<br /><br /> `'CurrentUser/Personal/'<thumbprint>`<br /><br /> Pour un magasin de clés principales de colonne personnalisé, le développeur est chargé de définir ce qu’est un chemin d’accès de clé pour le magasin de clés principales de colonne personnalisé.|  
|**allow_enclave_computations**|**bit**|Indique si la clé principale de colonne est activée pour l’enclave, (si les clés de chiffrement de colonne, chiffrées avec cette clé principale, peuvent être utilisées pour les calculs dans les enclaves sécurisées côté serveur). Pour plus d’informations, consultez [Always Encrypted avec enclaves sécurisées](../../relational-databases/security/encryption/always-encrypted-enclaves.md).|  
|**signature**|**varbinary(max)**|Une signature numérique de **key_path** et **allow_enclave_computations** produite à l’aide de la clé principale de colonne, référencée par **key_path**.|


  
## <a name="permissions"></a>Autorisations  
 Nécessite l’autorisation **afficher une clé principale de colonne** .  
  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Voir aussi  
 [CREATE COLUMN MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-column-master-key-transact-sql.md)   
 [Affichages catalogue de sécurité &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)   
 [sys.column_encryption_key_values &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-column-encryption-key-values-transact-sql.md)  
 [Always Encrypted](../../relational-databases/security/encryption/always-encrypted-database-engine.md)   
 [Vue d’ensemble de la gestion des clés pour Always Encrypted](../../relational-databases/security/encryption/overview-of-key-management-for-always-encrypted.md)   
 [Gérer des clés pour Always Encrypted avec enclaves sécurisées](../../relational-databases/security/encryption/always-encrypted-enclaves-manage-keys.md)   
 
  
  
