---
description: sys.sysconfigures (Transact-SQL)
title: sys.sysconfigure (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.sysconfigures
- sysconfigures
- sys.sysconfigures_TSQL
- sysconfigures_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sysconfigures compatibility view
- sysconfigures system table
ms.assetid: 146bf10a-c898-4676-a2a1-673fb1cee7a2
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 109c17040f52392df2518ddccb47491d2b0a9527
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99180703"
---
# <a name="syssysconfigures-transact-sql"></a>sys.sysconfigures (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contient une ligne pour chaque option de configuration définie par un utilisateur. **sysconfigures** contient les options de configuration qui sont définies avant le dernier démarrage de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , ainsi que toutes les options de configuration dynamiques définies depuis.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssnoteCompView](../../includes/ssnotecompview-md.md)]  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**value**|**int**|Valeur modifiable par l'utilisateur pour la variable. Utilisée par le [!INCLUDE[ssDE](../../includes/ssde-md.md)] uniquement en cas d'exécution de RECONFIGURE.|  
|**config**|**int**|Numéro de variable de configuration.|  
|**Commentaire**|**nvarchar(255)**|Explication de l'option de configuration.|  
|**statut**|**smallint**|Bitmap qui indique l’état de l’option. Il peut prendre les valeurs suivantes :<br /><br /> 0 = Statique. Le paramètre prend effet au redémarrage du serveur.<br /><br /> 1 = Dynamique. La variable prend effet lorsque l'instruction RECONFIGURE est exécutée.<br /><br /> 2 = Avancé. La variable s’affiche uniquement lorsque l' **option Afficher les options avancées** est définie. Le paramètre prend effet au redémarrage du serveur.<br /><br /> 3 = Dynamique et avancé.|  
  
## <a name="see-also"></a>Voir aussi  
 [Mappage de tables système à des vues système &#40;Transact-SQL&#41;](../../relational-databases/system-tables/mapping-system-tables-to-system-views-transact-sql.md)   
 [Affichages de compatibilité &#40;Transact-SQL&#41;](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)  
  
  
