---
description: sys.dm_io_backup_tapes (Transact-SQL)
title: sys.dm_io_backup_tapes (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_io_backup_tapes
- dm_io_backup_tapes_TSQL
- sys.dm_io_backup_tapes_TSQL
- dm_io_backup_tapes
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_io_backup_tapes dynamic management view
ms.assetid: 2e27489e-cf69-4a89-9036-77723ac3de66
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: bc4eb0de019ee40145c15c344850744d9fef3b15
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101579"
---
# <a name="sysdm_io_backup_tapes-transact-sql"></a>sys.dm_io_backup_tapes (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Renvoie la liste des périphériques à bandes et l'état des demandes de montage pour les sauvegardes.   
 
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**physical_device_name**|**nvarchar (520)**|Nom du périphérique physique sur lequel une sauvegarde peut être effectuée. N'accepte pas la valeur NULL.|  
|**logical_device_name**|**nvarchar (256)**|Nom spécifié par l’utilisateur pour le lecteur (à partir de **sys.backup_devices**). NULL si aucun nom spécifié par l'utilisateur n'est disponible. Autorise la valeur NULL.|  
|**statut**|**int**|État de la bande :<br /><br /> 1 = Ouverte et disponible<br /><br /> 2 = En attente de montage<br /><br /> 3 = Utilisée<br /><br /> 4 = Chargement<br /><br /> **Remarque :** Pendant le chargement d’une bande (**Status = 4**), l’étiquette du support n’est pas encore lue. Les colonnes qui copient les valeurs de l’étiquette de support, telles que **media_sequence_number**, affichent les valeurs anticipées, qui peuvent être différentes des valeurs réelles sur la bande. Une fois l’étiquette lue, l' **État** passe à **3** (en cours d’utilisation) et les colonnes de l’étiquette du support reflètent ensuite la bande qui est chargée.<br /><br /> N'accepte pas la valeur NULL.|  
|**status_desc**|**nvarchar (520)**|Description de l'état de la bande :<br /><br /> AVAILABLE<br /><br /> MOUNT PENDING<br /><br /> IN USE<br /><br /> LOADING MEDIA<br /><br /> N'accepte pas la valeur NULL.|  
|**mount_request_time**|**datetime**|Heure de la demande de montage. NULL si aucun montage n’est en attente (**Status ! = 2**). Autorise la valeur NULL.|  
|**mount_expiration_time**|**datetime**|Heure d'expiration de la demande de montage (dépassement de délai). NULL si aucun montage n’est en attente (**Status ! = 2**). Autorise la valeur NULL.|  
|**database_name**|**nvarchar (256)**|Base de données à sauvegarder sur ce périphérique. Autorise la valeur NULL.|  
|**spid**|**int**|ID de la session. Il identifie l'utilisateur de la bande. Autorise la valeur NULL.|  
|**command**|**int**|Commande qui effectue la sauvegarde. Autorise la valeur NULL.|  
|**command_desc**|**nvarchar(120)**|Description de la commande. Autorise la valeur NULL.|  
|**media_family_id**|**int**|Index de la famille de supports (1...*n*), *n* est le nombre de familles de supports dans le support de sauvegarde. Autorise la valeur NULL.|  
|**media_set_name**|**nvarchar (256)**|Nom du jeu de supports (s'il existe) tel qu'il a été spécifié par l'option MEDIANAME lors de la création du jeu de supports). Autorise la valeur NULL.|  
|**media_set_guid**|**uniqueidentifier**|Identificateur qui identifie spécifiquement le jeu de supports. Autorise la valeur NULL.|  
|**media_sequence_number**|**int**|Index du volume au sein d’une famille de supports (1...*n*). Autorise la valeur NULL.|  
|**tape_operation**|**int**|Opération sur bande en cours d’exécution :<br /><br /> 1 = Lecture<br /><br /> 2 = Formatage<br /><br /> 3 = Initialisation<br /><br /> 4 = Ajout<br /><br /> Autorise la valeur NULL.|  
|**tape_operation_desc**|**nvarchar(120)**|Opération en cours sur la bande :<br /><br /> READ<br /><br /> FORMAT<br /><br /> INIT<br /><br /> APPEND<br /><br /> Autorise la valeur NULL.|  
|**mount_request_type**|**int**|Type de la demande de montage :<br /><br /> 1 = Bande spécifique. La bande identifiée par les champs **media_ \** _ est requise.<br /><br /> 2 = Famille de supports suivante. Demande la famille de supports non encore restaurée suivante. S'utilise lorsque la restauration s'effectue à partir d'un nombre de périphériques inférieur au nombre de familles de supports.<br /><br /> 3 = Bandes magnétiques de sauvegarde consécutives. La famille de supports est étendue et il faut ajouter une bande consécutive.<br /><br /> Autorise la valeur NULL.|  
|_ *mount_request_type_desc**|**nvarchar(120)**|Type de la demande de montage :<br /><br /> SPECIFIC TAPE<br /><br /> NEXT MEDIA FAMILY<br /><br /> CONTINUATION VOLUME<br /><br /> Autorise la valeur NULL.|  
  
## <a name="permissions"></a>Autorisations  
 L'utilisateur doit disposer de l'autorisation VIEW SERVER STATE sur le serveur.  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Fonctions et vues de gestion dynamique liées aux e/s &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/i-o-related-dynamic-management-views-and-functions-transact-sql.md)  
  
  

