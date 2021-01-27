---
description: sys.syslogins (Transact-SQL)
title: Connexions sys.sys(Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/08/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- syslogins_TSQL
- syslogins
- sys.syslogins
- sys.syslogins_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.syslogins compatibility view
- syslogins system table
ms.assetid: 4cb34f17-a4bb-469f-a218-71f074e6308f
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 47cb0caefaa3ac0c9cdface4cb7c655eb0d373be
ms.sourcegitcommit: 00be343d0f53fe095a01ea2b9c1ace93cdcae724
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/26/2021
ms.locfileid: "98813349"
---
# <a name="syssyslogins-transact-sql"></a>sys.syslogins (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contient une ligne pour chaque compte de connexion.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssnoteCompView](../../includes/ssnotecompview-md.md)]  
  
**S’applique à**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (de [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] à la [version actuelle](/troubleshoot/sql/general/determine-version-edition-update-level)).  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**sid**|**varbinary(85)**|Identificateur de sécurité.|  
|**statut**|**smallint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**création**|**datetime**|Date d'ajout de la connexion.|  
|**updatedate**|**datetime**|Date de mise à jour de la connexion.|  
|**accdate**|**datetime**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**totcpu**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**totio**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**spacelimit**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**timelimit**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**resultlimit**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**name**|**sysname**|Nom de connexion de l’utilisateur.|  
|**@**|**sysname**|Nom de la base de données par défaut de l'utilisateur lorsqu'une connexion est établie.|  
|**mot de passe**|**nvarchar(128)**|Renvoie NULL.|  
|**language**|**sysname**|Langue par défaut de l'utilisateur.|  
|**denylogin**|**int**|1 = La connexion concerne un utilisateur ou un groupe [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows et l'accès a été refusé.|  
|**hasaccess**|**int**|1 = La connexion possède les droits d'accès au serveur.|  
|**isntname**|**int**|1 = La connexion est un utilisateur ou un groupe Windows.<br /><br /> 0 = Il s'agit d'une connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**isntgroup**|**int**|1 = La connexion est un groupe Windows.|  
|**isntuser**|**int**|1 = La connexion est un utilisateur Windows.|  
|**sysadmin**|**int**|1 = la connexion est membre du rôle de serveur **sysadmin** .|  
|**securityadmin**|**int**|1 = la connexion est membre du rôle de serveur **securityadmin** .|  
|**serveradmin**|**int**|1 = la connexion est membre du rôle serveur fixe **ServerAdmin** .|  
|**setupadmin**|**int**|1 = la connexion est membre du rôle serveur fixe **setupadmin** .|  
|**processadmin**|**int**|1 = la connexion est membre du rôle serveur fixe **processadmin** .|  
|**diskadmin**|**int**|1 = la connexion est membre du rôle serveur fixe **diskadmin** .|  
|**dbcreator**|**int**|1 = la connexion est membre du rôle serveur fixe **dbcreator** .|  
|**bulkadmin**|**int**|1 = la connexion est membre du rôle serveur fixe **bulkadmin** .|  
|**LoginName**|**nvarchar(128)**|Nom de connexion de l’utilisateur. Fourni pour la compatibilité ascendante.|  
  
## <a name="see-also"></a>Voir aussi  
 [Mappage de tables système à des vues système &#40;Transact-SQL&#41;](../../relational-databases/system-tables/mapping-system-tables-to-system-views-transact-sql.md)   
 [Affichages de compatibilité &#40;Transact-SQL&#41;](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)  
  
