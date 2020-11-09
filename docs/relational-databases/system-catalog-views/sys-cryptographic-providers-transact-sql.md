---
description: sys.cryptographic_providers (Transact-SQL)
title: sys.cryptographic_providers (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- cryptographic_providers
- sys.cryptographic_providers
- sys.cryptographic_providers_TSQL
- cryptographic_providers_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.cryptographic_providers catalog view
ms.assetid: 9da0da95-792e-48b4-9f60-47f0729c279c
author: VanMSFT
ms.author: vanto
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 305ea4a72855dd5ba136740dcc9b4321826384ad
ms.sourcegitcommit: 49ee3d388ddb52ed9cf78d42cff7797ad6d668f2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/09/2020
ms.locfileid: "94384797"
---
# <a name="syscryptographic_providers-transact-sql"></a>sys.cryptographic_providers (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Retourne une ligne pour chaque fournisseur de chiffrement inscrit.  
    
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**provider_id**|**int**|Numéro d'identification du fournisseur de services de chiffrement.|  
|**name**|**sysname**|Nom du fournisseur de chiffrement.|  
|**guid**|**uniqueidentifier**|GUID unique du fournisseur.|  
|**version**|**nvarchar(50)**|Version du fournisseur au format' *AA.bb.CCCC.DD* '.|  
|**dll_path**|**nvarchar(512)**|Chemin d'accès à la DLL qui implémente l'interface de programmation d'applications (API, Application Program Interface) EKM (Extensible Key Management).|  
|**is_enabled**|**bit**|Indique si le fournisseur est activé sur le serveur ou non.<br /><br /> 0 = non activé (valeur par défaut)<br /><br /> 1 = activé|  
  
## <a name="permissions"></a>Autorisations  
 La vue **sys.cryptographic_providers** est visible par le public.  
  
## <a name="see-also"></a>Voir aussi  
 [Affichages catalogue de sécurité &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)   
 [Hiérarchie de chiffrement](../../relational-databases/security/encryption/encryption-hierarchy.md)   
 [Gestion de clés extensible &#40;EKM&#41;](../../relational-databases/security/encryption/extensible-key-management-ekm.md)   
 [CREATE CRYPTOGRAPHIC PROVIDER &#40;Transact-SQL&#41;](../../t-sql/statements/create-cryptographic-provider-transact-sql.md)  
  
  
