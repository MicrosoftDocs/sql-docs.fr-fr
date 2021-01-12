---
description: sys.service_message_types (Transact-SQL)
title: sys.service_message_types (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.service_message_types
- service_message_types
- sys.service_message_types_TSQL
- service_message_types_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.service_message_types catalog view
ms.assetid: 6a38709a-60fe-46f6-89da-718f74f15600
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: ab7a81f1f288ad533fa6a32ccec6e1de8dbad9d7
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097868"
---
# <a name="sysservice_message_types-transact-sql"></a>sys.service_message_types (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Cet affichage catalogue contient une ligne par type de message inscrit dans Service Broker.
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**name**|**sysname**|Nom du type de message, unique dans la base de données. Cette colonne n'accepte pas la valeur NULL.|  
|**message_type_id**|**int**|Identificateur du type de message, unique dans la base de données. Cette colonne n'accepte pas la valeur NULL.|  
|**principal_id**|**int**|Identificateur du principal de base de données propriétaire de ce type de message. Accepte la valeur NULL.|  
|**validation**|**char(2)**|Validation effectuée par Service Broker avant l'envoi de messages de ce type. Cette colonne n'accepte pas la valeur NULL. Valeurs possibles :<br /><br /> N = aucun<br /><br /> X = XML<br /><br /> E = vide|  
|**validation_desc**|**nvarchar(60)**|Description de la validation effectuée par Service Broker avant l'envoi de messages de ce type. Accepte la valeur NULL. Valeurs possibles :<br /><br /> NONE<br /><br /> XML<br /><br /> EMPTY|  
|**xml_collection_id**|**int**|Pour une validation qui utilise un schéma XML, l'identificateur de la collection de schéma utilisée.<br /><br /> Sinon, NULL.|  
  
## <a name="permissions"></a>Autorisations  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
  
