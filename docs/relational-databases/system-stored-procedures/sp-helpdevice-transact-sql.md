---
description: sp_helpdevice (Transact-SQL)
title: sp_helpdevice (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_helpdevice
- sp_helpdevice_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_helpdevice
ms.assetid: 1a5eafa7-384e-4691-ba05-978eb73bbefb
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 95c63d822851d7d1ccf9493e3e0d4cc9beedd624
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204738"
---
# <a name="sp_helpdevice-transact-sql"></a>sp_helpdevice (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Transmet des informations sur les unités de sauvegarde Microsoft® SQL Server™.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]Nous vous recommandons d’utiliser à la place l’affichage catalogue [sys.backup_devices](../../relational-databases/system-catalog-views/sys-backup-devices-transact-sql.md)  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_helpdevice [ [ @devname = ] 'name' ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @devname = ] 'name'` Nom de l’unité de sauvegarde pour laquelle les informations sont signalées. La valeur de *Name* est toujours de **type sysname**.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**device_name**|**sysname**|Nom de périphérique logique.|  
|**physical_name**|**nvarchar(260)**|Nom de fichier physique.|  
|**description**|**nvarchar(255)**|Description du périphérique.|  
|**statut**|**int**|Numéro qui correspond à la description de l’État dans la colonne **Description** .|  
|**cntrltype**|**smallint**|Type de contrôleur du périphérique :<br /><br /> 2 = unité de disque<br /><br /> 5 = périphérique à bandes|  
|**size**|**int**|Taille du périphérique (en pages de 2 Ko).|  
  
## <a name="remarks"></a>Notes  
 Si le *nom* est spécifié, **sp_helpdevice** affiche des informations sur l’unité de vidage spécifiée. Si le *nom* n’est pas spécifié, **sp_helpdevice** affiche des informations sur tous les périphériques de vidage dans l’affichage catalogue **sys.backup_devices** .  
  
 Les périphériques de vidage sont ajoutés au système à l’aide de **sp_addumpdevice**.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle **public** .  
  
## <a name="examples"></a>Exemples  
 Cet exemple vous renseigne sur toutes les unités de vidage d'une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
```  
EXEC sp_helpdevice;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sp_addumpdevice &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addumpdevice-transact-sql.md)   
 [sp_dropdevice &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropdevice-transact-sql.md)   
 [Moteur de base de données des procédures stockées &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
