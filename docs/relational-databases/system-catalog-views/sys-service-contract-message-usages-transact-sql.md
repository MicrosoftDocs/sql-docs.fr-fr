---
description: sys.service_contract_message_usages (Transact-SQL)
title: sys.service_contract_message_usages (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- service_contract_message_usages_TSQL
- sys.service_contract_message_usages
- sys.service_contract_message_usages_TSQL
- service_contract_message_usages
dev_langs:
- TSQL
helpviewer_keywords:
- sys.service_contract_message_usages catalog view
ms.assetid: f783e662-126c-4595-8e22-f9d05191f5d0
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 33677382cd6dcf71bc6da8525747712fe8b83721
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99161373"
---
# <a name="sysservice_contract_message_usages-transact-sql"></a>sys.service_contract_message_usages (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Cet affichage catalogue contient une ligne par paire contrat/type de message.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**service_contract_id**|**int**|ID du contrat qui utilise le type de message. Cette colonne n'accepte pas la valeur NULL.|  
|**message_type_id**|**int**|ID du type de message utilisé par le contrat. Cette colonne n'accepte pas la valeur NULL.|  
|**is_sent_by_initiator**|**bit**|Le type de message peut être envoyé par l'initiateur de la conversation. Cette colonne n'accepte pas la valeur NULL.|  
|**is_sent_by_target**|**bit**|Le type de message peut être envoyé par la cible de la conversation. Cette colonne n'accepte pas la valeur NULL.|  
  
## <a name="permissions"></a>Autorisations  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
  
