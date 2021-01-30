---
description: dbo.sysdownloadlist (Transact-SQL)
title: dbo.sysdownloadlist (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dbo.sysdownloadlist
- sysdownloadlist_TSQL
- dbo.sysdownloadlist_TSQL
- sysdownloadlist
dev_langs:
- TSQL
helpviewer_keywords:
- sysdownloadlist system table
ms.assetid: 71087a4c-e829-488e-aa7d-a9476e2b4779
author: cawrites
ms.author: chadam
ms.openlocfilehash: 8670a2b3ca9e8033b3097b06fe96de040a4fb7c4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99158367"
---
# <a name="dbosysdownloadlist-transact-sql"></a>dbo.sysdownloadlist (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contient la file d'attente des instructions de téléchargement pour l'ensemble des serveurs cibles.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**instance_id**|**int**|Colonne d'identité indiquant la séquence naturelle d'insertion des lignes|  
|**source_server**|**sysname**|Nom du serveur source|  
|**operation_code**|**tinyint**|Code d'opération pour le travail :<br /><br /> **1** = ins (insérer)<br /><br /> **2** = UPD (mise à jour)<br /><br /> **3** = del (supprimer)<br /><br /> **4** = démarrer<br /><br /> **5** = arrêter|  
|**object_type**|**tinyint**|Code du type d'objet.|  
|**object_id** <sup>1</sup>|**uniqueidentifier**|Numéro d'identification de l'objet.|  
|**target_server**|**sysname**|Nom du serveur cible|  
|**error_message**|**nvarchar(1024)**|Message d'erreur si le serveur cible rencontre une erreur lors du traitement de la ligne spécifique|  
|**date_posted**|**datetime**|Date et heure à laquelle le travail a été posté vers le serveur cible|  
|**date_downloaded**|**datetime**|Date et heure à laquelle le travail a été téléchargé pour la dernière fois|  
|**statut**|**tinyint**|État du travail :<br /><br /> **0** = pas encore téléchargé<br /><br /> **1** = téléchargé avec succès|  
|**deleted_object_name**|**sysname**|Nom de l'objet supprimé.|  
  
 <sup>1</sup> la colonne **object_id** peut avoir la valeur **-1**, qui correspond à la valeur All si l' **operation_code** colonne est la valeur Delete.  
  
  
