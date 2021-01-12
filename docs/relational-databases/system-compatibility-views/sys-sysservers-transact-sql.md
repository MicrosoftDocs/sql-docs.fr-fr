---
description: sys.sysservers (Transact-SQL)
title: Serveurs sys.sys(Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.sysservers
- sysservers_TSQL
- sysservers
- sys.sysservers_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysservers system table
- sys.sysservers compatibility view
ms.assetid: d02f186f-c00f-44a6-b38d-dc78a3d2145b
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 1cf2a9649745eeeb5fbf10667b9849c0c1de5dec
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095326"
---
# <a name="syssysservers-transact-sql"></a>sys.sysservers (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contient une ligne pour chaque serveur auquel une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut accéder en tant que source de données OLE DB.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssnoteCompView](../../includes/ssnotecompview-md.md)]  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**srvid**|**smallint**|ID (pour utilisation locale uniquement) du serveur distant.|  
|**srvstatus**|**smallint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**srvname**|**sysname**|Nom du serveur.|  
|**srvproduct**|**sysname**|Nom de produit du serveur distant|  
|**désigne**|**nvarchar(128)**|Nom du fournisseur OLE DB pour l'accès au serveur.|  
|**Mustek**|**nvarchar(4000)**|Valeur de source de données OLE DB.|  
|**location**|**nvarchar(4000)**|Valeur d'emplacement OLE DB.|  
|**ProviderString**|**nvarchar(4000)**|Valeur de chaîne de fournisseur OLE DB.|  
|**schemadate**|**datetime**|Date de la dernière mise à jour de la ligne.|  
|**topologyx**|**int**|Non utilisé.|  
|**topologyy**|**int**|Non utilisé.|  
|**catalog**|**sysname**|Catalogue utilisé lors de la connexion à un fournisseur OLE DB.|  
|**srvcollation**|**sysname**|Classement du serveur.|  
|**ConnectTimeout**|**int**|Paramètre de délai d'expiration de la connexion serveur.|  
|**QueryTimeout**|**int**|Paramètre de délai d'expiration des requêtes envoyées au serveur.|  
|**srvnetname**|**char(30)**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**isremote**|**bit**|1 = serveur distant<br /><br /> 0 = serveur lié|  
|**Call**|**bit**|1 = **sp_serveroption \@ RPC** défini à **true** ou **à on**.<br /><br /> 0 = **sp_serveroption \@ RPC** défini à **false** ou à **off**.|  
|**pub**|**bit**|1 = **sp_serveroption \@ pub** a la valeur **true** ou **on**.<br /><br /> 0 = **sp_serveroption \@ pub** défini à **false** ou **off**.|  
|**sub**|**bit**|1 = **sp_serveroption \@ sous** -ensemble à la **valeur true** ou **on**.<br /><br /> 0 = **sp_serveroption \@ sous** -ensemble à **false** ou **off**.|  
|**dist**|**bit**|1 = **sp_serveroption \@ dist** définie à **true** ou **à on**.<br /><br /> 0 = **sp_serveroption \@ dist** définie à **false** ou **off**.|  
|**dpub**|**bit**|1 = **sp_serveroption \@ dpub** a la valeur **true** ou **on**.<br /><br /> 0 = **sp_serveroption \@ dpub** a la valeur **false** ou **off**.|  
|**rpcout**|**bit**|1 = **sp_serveroption \@ RPC out** défini à **true** ou **à on**.<br /><br /> 0 = **sp_serveroption \@ RPC out** défini à **false** ou à **off**.|  
|**dataaccess**|**bit**|1 = **sp_serveroption \@ accès aux données** a la valeur **true** ou **on**.<br /><br /> 0 = **sp_serveroption \@ accès aux données** a la valeur **false** ou **off**.|  
|**collationcompatible**|**bit**|1 = **sp_serveroption \@ compatibilité de classement avec** la valeur **true** ou **on**.<br /><br /> 0 = **sp_serveroption \@ compatibilité de classement** définie à **false** ou à **off**.|  
|**système**|**bit**|1 = **sp_serveroption \@ système** a la valeur **true** ou **on**.<br /><br /> 0 = **sp_serveroption \@ système** défini sur la valeur **false** ou **off**.|  
|**useremotecollation**|**bit**|1 = **sp_serveroption le \@ classement distant** a la valeur **true** ou **on**.<br /><br /> 0 = **sp_serveroption le \@ classement distant** est défini sur **false** ou **off**.|  
|**lazyschemavalidation**|**bit**|1 = **sp_serveroption la \@ validation du schéma paresseux** a la valeur **true** ou **on**.<br /><br /> 0 = **sp_serveroption la \@ validation du schéma paresseux** est définie à **false** ou **off**.|  
|**classement**|**sysname**|Classement du serveur défini par **sp_serveroption \@ nom du classement**.|  
|**nonsqlsub**|bit|0 : le serveur est une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]<br /><br /> 1 : le serveur n'est pas une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|  
  
## <a name="see-also"></a>Voir aussi  
 [Mappage de tables système à des vues système &#40;Transact-SQL&#41;](../../relational-databases/system-tables/mapping-system-tables-to-system-views-transact-sql.md)   
 [Affichages de compatibilité &#40;Transact-SQL&#41;](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)  
  
  
