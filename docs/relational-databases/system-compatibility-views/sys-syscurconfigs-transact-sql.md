---
description: sys.syscurconfigs (Transact-SQL)
title: sys.syscurconfigs (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.syscurconfigs
- sys.syscurconfigs_TSQL
- syscurconfigs
- syscurconfigs_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.syscurconfigs compatibility view
- syscurconfigs system table
ms.assetid: 454ab849-07a5-4b50-ba0a-6b1b14721f77
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: e8bd6413388f5445e11fb6b418a279e6e0c249f5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99158760"
---
# <a name="syssyscurconfigs-transact-sql"></a>sys.syscurconfigs (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contient une entrée pour chaque option de configuration courante. De plus, cette vue contient quatre entrées décrivant la structure de la configuration. **syscurconfigs** est généré dynamiquement lorsqu’il est interrogé par un utilisateur. Pour plus d’informations, consultez [sys.sysconfigure &#40;&#41;Transact-SQL ](../../relational-databases/system-compatibility-views/sys-sysconfigures-transact-sql.md).  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssnoteCompView](../../includes/ssnotecompview-md.md)]  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**value**|**int**|Valeur modifiable par l'utilisateur pour la variable. Utilisée par le [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] uniquement en cas d'exécution de RECONFIGURE.|  
|**config**|**smallint**|Numéro de variable de configuration.|  
|**Commentaire**|**nvarchar(255)**|Explication de l'option de configuration.|  
|**statut**|**smallint**|Bitmap indiquant l'état de l'option. Il peut prendre les valeurs suivantes :<br /><br /> 0 = Statique. Le paramètre prend effet au redémarrage du serveur.<br /><br /> 1 = Dynamique. La variable prend effet lorsque l'instruction RECONFIGURE est exécutée.<br /><br /> 2 = Avancé. La variable s’affiche uniquement lorsque l' **option Afficher les options avancées** est définie.<br /><br /> 3 = Dynamique et avancé.|  
  
## <a name="see-also"></a>Voir aussi  
 [Mappage de tables système à des vues système &#40;Transact-SQL&#41;](../../relational-databases/system-tables/mapping-system-tables-to-system-views-transact-sql.md)   
 [Affichages de compatibilité &#40;Transact-SQL&#41;](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)  
  
  
