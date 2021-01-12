---
description: sys.sysusers (Transact-SQL)
title: Utilisateurs sys.sys(Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.sysusers
- sys.sysusers_TSQL
- sysusers_TSQL
- sysusers
dev_langs:
- TSQL
helpviewer_keywords:
- sysusers system table
- sys.sysusers compatibility view
ms.assetid: 5f0e6a8d-c983-44f6-97e9-aab5bff67d18
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 812aca2816b9df05596509e4a61b50d2e4e59d56
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095322"
---
# <a name="syssysusers-transact-sql"></a>sys.sysusers (Transact-SQL)
[!INCLUDE [sql-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdbmi-asa-pdw.md)]

  Contient une ligne pour chaque [!INCLUDE[msCoName](../../includes/msconame-md.md)] utilisateur Windows, groupe Windows, [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilisateur ou [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] rôle dans la base de données.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssnoteCompView](../../includes/ssnotecompview-md.md)]  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**uid**|**smallint**|ID d'utilisateur, unique dans cette base de données.<br /><br /> 1 = Propriétaire de la base de données<br /><br /> Déborde ou retourne la valeur NULL si le nombre d'utilisateurs et de rôles dépasse 32 767.|  
|**statut**|**smallint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**name**|**sysname**|Nom d'utilisateur ou de groupe, unique dans cette base de données.|  
|**sid**|**varbinary(85)**|Identificateur de sécurité pour cette entrée.|  
|**rôles**|**varbinary(2048)**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**création**|**datetime**|Date de l'ajout du compte.|  
|**updatedate**|**datetime**|Date de la dernière modification du compte.|  
|**altuid**|**smallint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]<br /><br /> Déborde ou retourne la valeur NULL si le nombre d'utilisateurs et de rôles dépasse 32 767.|  
|**mot de passe**|**varbinary(256)**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**gid**|**smallint**|ID du groupe auquel l'utilisateur appartient. Si **UID** est identique à **GID**, cette entrée définit un groupe. Déborde ou retourne la valeur NULL si le nombre total d'utilisateurs et de groupes dépasse 32 767.|  
|**environ**|**varchar(255)**|Réservé.|  
|**hasdbaccess**|**int**|1 = Le compte a accès à la base de données.|  
|**islogin**|**int**|1 = Le compte est un groupe Windows ou un utilisateur Windows ou [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] bénéficiant d'un compte de connexion.|  
|**isntname**|**int**|1 = Le compte est un utilisateur ou groupe Windows.|  
|**isntgroup**|**int**|1 = Le compte est un groupe Windows.|  
|**isntuser**|**int**|1 = Le compte est un utilisateur Windows.|  
|**issqluser**|**int**|1 = Le compte est un utilisateur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**isaliased**|**int**|1 = Le compte est l'alias d'un autre utilisateur.|  
|**issqlrole**|**int**|1 = Le compte est un rôle [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**isapprole**|**int**|1 = Le compte est un rôle d’application.|  
  
## <a name="see-also"></a>Voir aussi  
 [Mappage de tables système à des vues système &#40;Transact-SQL&#41;](../../relational-databases/system-tables/mapping-system-tables-to-system-views-transact-sql.md)   
 [Affichages de compatibilité &#40;Transact-SQL&#41;](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)  
  
  
