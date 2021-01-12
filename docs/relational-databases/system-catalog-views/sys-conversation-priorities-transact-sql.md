---
description: sys.conversation_priorities (Transact-SQL)
title: sys.conversation_priorities (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- conversation_priorities_TSQL
- conversation_priorities
- sys.conversation_priorities_TSQL
- sys.conversation_priorities
dev_langs:
- TSQL
helpviewer_keywords:
- conversations [Service Broker], priorities
- Service Broker, conversations
- sys.conversation_priorities catalog view
ms.assetid: 7cbb9171-3310-4aae-8458-755c882d6462
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 76972ee7aac973ef6d7ce4276f95a99454c872c1
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102839"
---
# <a name="sysconversation_priorities-transact-sql"></a>sys.conversation_priorities (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contient une ligne pour chaque priorité de conversation créée dans la base de données active, comme indiqué dans le tableau suivant : 
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|Priority_id|**int**|Nombre qui identifie la priorité de conversation de façon unique. Cette colonne n'accepte pas la valeur NULL.|  
|name|**sysname**|Nom de la priorité de conversation. Cette colonne n'accepte pas la valeur NULL.|  
|service_contract_id|**int**|Identificateur du contrat spécifié pour la priorité de conversation. Cette colonne peut être jointe sur la colonne service_contract_id dans sys.service_contracts. Accepte la valeur NULL.|  
|local_service_id|**int**|Identificateur du service spécifié en tant que service local pour la priorité de conversation. Cette colonne peut être jointe sur la colonne service_id dans sys.services. Accepte la valeur NULL.|  
|remote_service_name|**nvarchar (256)**|Nom du service spécifié en tant que service distant pour la priorité de conversation. Accepte la valeur NULL.|  
|priority|**tinyint**|Niveau de priorité spécifié dans cette priorité de conversation. Cette colonne n'accepte pas la valeur NULL.|  
  
## <a name="permissions"></a>Autorisations  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant répertorie les priorités de conversation à l'aide de jointures pour afficher le nom du contrat et celui du service local.  
  
```  
SELECT scp.name AS priority_name,  
       ssc.name AS contract_name,  
       ssvc.name AS local_service_name,  
       scp.remote_service_name,  
       scp.priority AS priority_level  
FROM sys.conversation_priorities AS scp  
    INNER JOIN sys.service_contracts AS ssc  
       ON scp.service_contract_id = ssc.service_contract_id  
    INNER JOIN sys.services AS ssvc  
       ON scp.local_service_id = ssvc.service_id  
ORDER BY priority_name, contract_name,  
         local_service_name, remote_service_name;  
  
```  
  
## <a name="see-also"></a>Voir aussi  
 [ALTER BROKER PRIORITY &#40;Transact-SQL&#41;](../../t-sql/statements/alter-broker-priority-transact-sql.md)   
 [CREATE BROKER PRIORITY &#40;Transact-SQL&#41;](../../t-sql/statements/create-broker-priority-transact-sql.md)   
 [DROP BROKER PRIORITY &#40;Transact-SQL&#41;](../../t-sql/statements/drop-broker-priority-transact-sql.md)   
 [sys. services &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-services-transact-sql.md)   
 [sys.service_contracts &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-service-contracts-transact-sql.md)  
  
  
