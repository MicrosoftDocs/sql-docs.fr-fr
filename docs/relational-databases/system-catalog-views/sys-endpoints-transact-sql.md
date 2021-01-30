---
description: sys.endpoints (Transact-SQL)
title: sys. Endpoints (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- endpoints
- sys.endpoints
- endpoints_TSQL
- sys.endpoints_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.endpoints catalog view
ms.assetid: e6dafa4e-e47e-43ec-acfc-88c0af53c1a1
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: ee59a374da6b17f595adebceec0c816d1b72862a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203883"
---
# <a name="sysendpoints-transact-sql"></a>sys.endpoints (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contient une ligne par point de terminaison créé dans le système. Il existe toujours un point de terminaison SYSTEM.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**name**|**sysname**|Nom du point de terminaison, unique dans le serveur. N'accepte pas la valeur NULL.|  
|**endpoint_id**|**int**|ID du point de terminaison, unique dans le serveur. Un point de terminaison doté d'un ID inférieur à 65 536 est un point de terminaison système. N'accepte pas la valeur NULL.|  
|**principal_id**|**int**|Identificateur du principal qui a créé ce point de terminaison et qui en est propriétaire. Autorise la valeur NULL.|  
|**protocol**|**tinyint**|Protocole du point de terminaison.<br /><br /> 1 = HTTP<br /><br /> 2 = TCP<br /><br /> 3 = Canaux nommés<br /><br /> 4 = Mémoire partagée<br /><br /> 5 = VIA (Virtual Interface Architecture)<br /><br /> N'accepte pas la valeur NULL.|  
|**protocol_desc**|**nvarchar(60)**|Description du protocole du point de terminaison. Accepte la valeur NULL. Une des valeurs suivantes :<br /><br /> **HTTP**<br /><br /> **TCP**<br /><br /> **NAMED_PIPES**<br /><br /> **SHARED_MEMORY**<br /><br /> **Via** Remarque : le protocole VIA est déconseillé. [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]|  
|**type**|**tinyint**|Type de charge utile du point de terminaison.<br /><br /> 1 = SOAP<br /><br /> 2 = TSQL<br /><br /> 3 = SERVICE_BROKER<br /><br /> 4 = DATABASE_MIRRORING<br /><br /> N'accepte pas la valeur NULL.|  
|**type_desc**|**nvarchar(60)**|Description du type de charge utile du point de terminaison. Autorise la valeur NULL. Une des valeurs suivantes :<br /><br /> **SOAP**<br /><br /> **TSQL**<br /><br /> **SERVICE_BROKER**<br /><br /> **DATABASE_MIRRORING**|  
|**state**|**tinyint**|État du point de terminaison.<br /><br /> 0 = DÉMARRÉ : à l'écoute et en cours de traitement des demandes.<br /><br /> 1 = ARRÊTÉ : à l'écoute mais pas en cours de traitement des demandes.<br /><br /> 2 = DÉSACTIVÉ : pas à l'écoute.<br /><br /> L'état par défaut est 1. Autorise la valeur NULL.|  
|**state_desc**|**nvarchar(60)**|Description de l'état du point de terminaison.<br /><br /> DÉMARRÉ : à l'écoute et en cours de traitement des demandes.<br /><br /> ARRÊTÉ : à l'écoute mais pas en cours de traitement des demandes.<br /><br /> DÉSACTIVÉ : pas à l'écoute.<br /><br /> L'état par défaut est ARRÊTÉ.<br /><br /> Autorise la valeur NULL.|  
|**is_admin_endpoint**|**bit**|Indique si le point de terminaison est destiné à des tâches d'administration.<br /><br /> 0 = Point de terminaison destiné à l'administration.<br /><br /> 1 = Le point de terminaison n'est pas destiné à l'administration.<br /><br /> N'accepte pas la valeur NULL.|  
  
## <a name="permissions"></a>Autorisations  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Vues de catalogue des points de terminaison &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/endpoints-catalog-views-transact-sql.md)   
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)  
  
  
