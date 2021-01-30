---
description: sys.service_contract_usages (Transact-SQL)
title: sys.service_contract_usages (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- service_contract_usages
- sys.service_contract_usages
- sys.service_contract_usages_TSQL
- service_contract_usages_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.service_contract_usages catalog view
ms.assetid: 20af425e-1152-4a46-b1ac-94cff5fc9f02
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 235f3439f31284756d008a4eff1ae47ec115cb81
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211237"
---
# <a name="sysservice_contract_usages-transact-sql"></a>sys.service_contract_usages (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Cet affichage catalogue contient une ligne pour chaque paire (service, contrat).  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**service_id**|**int**|Identificateur du service utilisant le contrat. Cette colonne n'accepte pas la valeur NULL.|  
|**service_contract_id**|**int**|Identificateur du contrat utilisé par le service. Cette colonne n'accepte pas la valeur NULL.|  
  
## <a name="permissions"></a>Autorisations  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
  
