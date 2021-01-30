---
description: sys.linked_logins (Transact-SQL)
title: sys.linked_logins (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.linked_logins
- sys.linked_logins_TSQL
- linked_logins_TSQL
- linked_logins
dev_langs:
- TSQL
helpviewer_keywords:
- sys.linked_logins catalog view
ms.assetid: af57bf0c-a265-410f-9bab-63b78569b4a6
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: cd771e7199ce5a120ad0ef6c4c5f0546cfdb426c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191381"
---
# <a name="syslinked_logins-transact-sql"></a>sys.linked_logins (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Retourne une ligne par mappage de connexion de serveur lié dans le but d'utiliser ce mappage dans les requêtes RPC et les requêtes distribuées en provenance du serveur local et à destination du serveur lié correspondant.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**server_id**|**int**|ID du serveur dans **sys.** Servers.|  
|**local_principal_id**|**int**|Serveur principal auquel s'applique le mappage.<br /><br /> 0 = générique ou public.|  
|**uses_self_credential**|**bit**|Si la valeur de la colonne est égale à 1, le mappage indique que la session doit utiliser ses propres informations d'identification ; la valeur 0 indique que la session utilise les nom et mot de passe fournis.|  
|**remote_name**|**sysname**|Nom utilisateur distant servant à la connexion. Le mot de passe est également stocké mais n'est pas exposé dans les interfaces des vues de catalogue.|  
|**modify_date**|**datetime**|Date de la dernière modification de la connexion liée.|  
  
## <a name="permissions"></a>Autorisations  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Affichages catalogue des serveurs liés &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/linked-servers-catalog-views-transact-sql.md)  
  
  
