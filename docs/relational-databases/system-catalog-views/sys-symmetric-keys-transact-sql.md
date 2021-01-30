---
description: sys.symmetric_keys (Transact-SQL)
title: sys.symmetric_keys (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- symmetric_keys
- sys.symmetric_keys
- sys.symmetric_keys_TSQL
- symmetric_keys_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.symmetric_keys catalog view
ms.assetid: d410eae1-3a52-45de-b9a1-52d2bd93a8eb
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 52ea043fb9d8274e30eba1f2d68962e6ec999187
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202003"
---
# <a name="syssymmetric_keys-transact-sql"></a>sys.symmetric_keys (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Renvoie une ligne pour chaque clé symétrique créée avec l'instruction CREATE SYMMETRIC KEY.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**name**|**sysname**|Nom de la clé. Unique dans la base de données.|  
|**principal_id**|**int**|ID du principal de la base de données propriétaire de la clé.|  
|**symmetric_key_id**|**int**|ID de la clé. Unique dans la base de données.|  
|**key_length**|**int**|Longueur de la clé en bits.|  
|**key_algorithm**|**char(2)**|Algorithme utilisé avec la clé :<br /><br /> R2 = RC2<br /><br /> R4 = RC4<br /><br /> D = DES<br /><br /> D3 = Triple DES<br /><br /> DT = TRIPLE_DES_3KEY<br /><br /> DX = DESX<br /><br /> A1 = AES 128<br /><br /> A2 = AES 192<br /><br /> A3 = AES 256<br /><br /> NA = EKM Key|  
|**algorithm_desc**|**nvarchar(60)**|Description de l'algorithme utilisé avec la clé :<br /><br /> RC2<br /><br /> RC4<br /><br /> DES<br /><br /> Triple_DES<br /><br /> TRIPLE_DES_3KEY<br /><br /> DESX<br /><br /> AES_128<br /><br /> AES_192<br /><br /> AES_256<br /><br /> NULL (algorithmes de gestion de clés extensible uniquement)|  
|**create_date**|**datetime**|Date de création de la clé.|  
|**modify_date**|**datetime**|Date de modification de la clé.|  
|**key_guid**|**uniqueidentifier**|GUID (Globally Unique Identifier) associé à la clé. Il est créé automatiquement pour les clés persistantes. Les GUID des clés provisoires sont dérivées de la phrase secrète fournie par l'utilisateur.|  
|**key_thumbprint**|**sql_variant**|Hachage SHA-1 de la clé. Hachage globalement unique. Pour les clés non-EKM (Gestion de clés extensible), cette valeur sera NULL.|  
|**provider_type**|**nvarchar(120)**|Type du fournisseur de chiffrement.<br /><br /> CRYPTOGRAPHIC PROVIDER = Clés EKM (Gestion de clés extensible)<br /><br /> NULL = Clés non-EKM (Gestion de clés extensible)|  
|**cryptographic_provider_guid**|**uniqueidentifier**|GUID du fournisseur de chiffrement. Pour les clés non-EKM (Gestion de clés extensible), cette valeur sera NULL.|  
|**cryptographic_provider_algid**|**sql_variant**|ID d'algorithme pour le fournisseur de chiffrement. Pour les clés non-EKM (Gestion de clés extensible), cette valeur sera NULL.|  
  
## <a name="permissions"></a>Autorisations  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="remarks"></a>Notes  
 L'algorithme RC4 est déconseillé. [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)]  
  
> [!NOTE]  
>  L'algorithme RC4 est uniquement pris en charge pour des raisons de compatibilité descendante. Le nouveau matériel ne peut être chiffré à l'aide de RC4 ou de RC4_128 que lorsque la base de données se trouve dans le niveau de compatibilité 90 ou 100. (Non recommandé.) Utilisez à la place un algorithme plus récent, tel qu'un des algorithmes AES. Dans [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], le matériel chiffré à l'aide de RC4 ou de RC4_128 peut être déchiffré dans n'importe quel niveau de compatibilité.  
  
 **Éclaircissement concernant les algorithmes DES :**  
  
-   DESX a été nommé incorrectement. Les clés symétriques créées avec ALGORITHM = DESX utilisent en fait le chiffrement TRIPLE DES avec une clé de 192 bits. L'algorithme DESX n'est pas fourni. [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  
  
-   Les clés symétriques créées avec ALGORITHM = TRIPLE_DES_3KEY utilisent TRIPLE DES avec une clé de 192 bits.  
  
-   Les clés symétriques créées avec ALGORITHM = TRIPLE_DES utilisent TRIPLE DES avec une clé de 128 bits.  
  
## <a name="see-also"></a>Voir aussi  
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Gestion de clés extensible &#40;EKM&#41;](../../relational-databases/security/encryption/extensible-key-management-ekm.md)   
 [Affichages catalogue de sécurité &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)   
 [Hiérarchie de chiffrement](../../relational-databases/security/encryption/encryption-hierarchy.md)   
 [CREATE SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-symmetric-key-transact-sql.md)  
  
  
