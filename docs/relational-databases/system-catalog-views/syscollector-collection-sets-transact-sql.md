---
description: syscollector_collection_sets (Transact-SQL)
title: syscollector_collection_sets (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- syscollector_collection_sets_TSQL
- syscollector_collection_sets
dev_langs:
- TSQL
helpviewer_keywords:
- data collector view
- syscollector_collection_sets view
ms.assetid: db0def92-f25b-45da-9709-eab972b33800
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: cc9b5754c1df9957f1a3e8b5e53d0465690a78a7
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094261"
---
# <a name="syscollector_collection_sets-transact-sql"></a>syscollector_collection_sets (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Fournit des informations sur un jeu d'éléments de collecte, y compris la planification, le mode de collecte et son état.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|collection_set_id|**int**|Identificateur local pour le jeu d'éléments de collection. N'accepte pas la valeur NULL.|  
|collection_set_uid|**uniqueidentifier**|Identificateur global unique du jeu d'éléments de collecte. N'accepte pas la valeur NULL.|  
|name|**nvarchar(4000)**|Nom du jeu d'éléments de collecte. Autorise la valeur NULL.|  
|target|**nvarchar(max)**|Identifie la cible pour le jeu d'éléments de collecte. Autorise la valeur NULL.|  
|is_system|**bit**|Activé (1) ou désactivé (0) pour indiquer si le jeu d'éléments de collecte a été inclus avec le collecteur de données ou s'il a été ajouté ultérieurement par le dc_admin. Il peut s'agir d'un jeu d'éléments de collecte personnalisé développé en interne ou par un tiers. N'accepte pas la valeur NULL.|  
|is_running|**bit**|Indique si le jeu d'éléments de collecte est en cours d'exécution ou non. N'accepte pas la valeur NULL.|  
|collection_mode|**smallint**|Spécifie le mode de collecte pour le jeu d'éléments de collecte. N'accepte pas la valeur NULL.<br /><br /> Le mode de collecte prend l'une des valeurs suivantes :<br /><br /> 0 - Mode mis en cache. La collecte et le téléchargement de données sont sur des planifications séparées.<br /><br /> 1 - Mode non mis en cache. La collecte et le téléchargement de données sont sur la même planification.|  
|proxy_id|**int**|Identifie le proxy qui est utilisé pour exécuter l'étape du travail du jeu d'éléments de collecte. Autorise la valeur NULL.|  
|schedule_uid|**uniqueidentifier**|Fournit un pointeur vers la planification du jeu d'éléments de collecte. Autorise la valeur NULL.|  
|collection_job_id|**uniqueidentifier**|Identifie le travail de collecte. Autorise la valeur NULL.|  
|upload_job_id|**uniqueidentifier**|Identifie le travail de téléchargement de collecte. Autorise la valeur NULL.|  
|Niveau de journalisation|**smallint**|Spécifie le niveau de journalisation (0, 1 ou 2). N'accepte pas la valeur NULL.|  
|days_until_expiration|**smallint**|Nombre de jours pendant lesquels les données collectées sont enregistrées dans l'entrepôt de données de gestion. N'accepte pas la valeur NULL.|  
|description|**nvarchar(4000)**|Décrit le jeu d'éléments de collecte. Autorise la valeur NULL.|  
|dump_on_any_error|**bit**|Activé (1) ou désactivé (0) pour indiquer s’il faut créer un [!INCLUDE[ssIS](../../includes/ssis-md.md)] fichier de vidage en cas d’erreur. N'accepte pas la valeur NULL.|  
|dump_on_codes|**nvarchar(max)**|Contient la liste des codes d'erreur [!INCLUDE[ssIS](../../includes/ssis-md.md)] utilisés pour déclencher le fichier de vidage. Autorise la valeur NULL.|  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation SELECT pour dc_operator, dc_proxy.  
  
## <a name="remarks"></a>Notes  
 L'API du collecteur de données vous permet uniquement de modifier ou de supprimer les jeux d'éléments de collecte que vous créez. Les jeux d'éléments de collecte fournis avec le système ne peuvent pas être modifiés ou supprimés. Toutefois, vous pouvez toujours activer ou désactiver un jeu d'éléments de collecte système et modifier sa configuration.  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées du collecteur de données &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/data-collector-stored-procedures-transact-sql.md)   
 [Vues du collecteur de données &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/data-collector-views-transact-sql.md)   
 [Collecte de données](../../relational-databases/data-collection/data-collection.md)  
  
  
