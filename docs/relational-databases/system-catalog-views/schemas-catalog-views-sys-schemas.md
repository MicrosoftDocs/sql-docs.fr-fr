---
description: Affichages catalogue de schémas-sys. schemas
title: sys. schemas (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- schemas_TSQL
- sys.schemas_TSQL
- schemas
- sys.schemas
dev_langs:
- TSQL
helpviewer_keywords:
- sys.schemas catalog view
ms.assetid: 29af5ce5-2af7-4103-8f08-3ec92603ba05
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: d41745e41db18f720f85fbc4aef0b8dcec89892f
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100657"
---
# <a name="schemas-catalog-views---sysschemas"></a>Affichages catalogue de schémas-sys. schemas
[!INCLUDE [sql-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdbmi-asa-pdw.md)]

  Contient une ligne pour chaque schéma de base de données.  
  
> [!NOTE]  
>  Les schémas de base de données diffèrent des schémas XML, qui sont utilisés pour définir le modèle de contenu des documents XML.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**name**|**sysname**|Nom du schéma. Unique dans la base de données.|  
|**schema_id**|**int**|Identificateur du schéma. Unique dans la base de données.|  
|**principal_id**|**int**|Identificateur du principal qui possède ce schéma.|  
  
## <a name="remarks"></a>Notes  
Les schémas de base de données jouent le rôle d’espaces de noms ou de conteneurs pour les objets, tels que les tables, les vues, les procédures et les fonctions, qui se trouvent dans l’affichage catalogue **sys. Objects** .  

Chaque schéma a un propriétaire. Le propriétaire est un [principal](../../relational-databases/security/authentication-access/principals-database-engine.md)de sécurité.
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle **public** . Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Voir aussi  
[Principaux](../../relational-databases/security/authentication-access/principals-database-engine.md)

[Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   

[Affichages catalogue de schémas &#40;Transact-SQL&#41;](./catalog-views-transact-sql.md)   

[sys.objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)  
  
