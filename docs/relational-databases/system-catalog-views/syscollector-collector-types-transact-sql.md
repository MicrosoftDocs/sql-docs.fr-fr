---
description: syscollector_collector_types (Transact-SQL)
title: syscollector_collector_types (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- syscollector_collector_types
- syscollector_collector_types_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- data collector view
- syscollector_collector_types view
ms.assetid: d5cd30bb-89fd-4814-a7e8-9074f043f90f
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: c680c9bb24136e9029540a71b6eaa8dc97520a75
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199809"
---
# <a name="syscollector_collector_types-transact-sql"></a>syscollector_collector_types (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Fournit des informations sur un type de collecteur pour un élément de collecte.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**collector_type_uid**|**uniqueidentifer**|GUID pour un type de collecte. N'accepte pas la valeur NULL.|  
|**name**|**sysname**|Nom du type de collecte. N'accepte pas la valeur NULL.|  
|**parameter_schema**|**xml**|Schéma XML qui décrit l'aspect de la configuration pour le type de collecteur spécifié. Ce schéma XML est utilisé pour valider la configuration XML réelle associée à l'instance d'un élément de collecte particulier. Autorise la valeur NULL.|  
|**parameter_formatter**|**xml**|Détermine le modèle à utiliser pour transformer le XML utilisé dans la page de propriétés du jeu d'éléments de collecte. Autorise la valeur NULL.|  
|**collection_package_id**|**uniqueidentifer**|GUID pour un package de collecte. N'accepte pas la valeur NULL.|  
|**collection_package_path**|**nvarchar(4000)**|Fournit le chemin d'accès au package de collecte. Autorise la valeur NULL.|  
|**collection_package_name**|**sysname**|Nom du package de collecte. N'accepte pas la valeur NULL.|  
|**upload_package_id**|**uniqueidentifer**|GUID pour le package à télécharger. N'accepte pas la valeur NULL.|  
|**upload_package_path**|**nvarchar(4000)**|Fournit le chemin d'accès au package à télécharger. Autorise la valeur NULL.|  
|**upload_package_name**|**sysname**|Nom du package à télécharger. N'accepte pas la valeur NULL.|  
|**is_system**|**bit**|Activé (1) ou désactivé (0) pour indiquer si le type de collecteur a été fourni avec le collecteur de données ou s’il a été ajouté ultérieurement par le **dc_admin**. Il peut s'agir d'un type personnalisé développé en interne ou par un tiers. N'accepte pas la valeur NULL.|  
  
## <a name="permissions"></a>Autorisations  
 Requiert SELECT pour **dc_operator**, **dc_proxy**.  
  
## <a name="change-history"></a>Historique des modifications  
  
|Mise à jour du contenu|  
|---------------------|  
|Mise à jour **collection_type_uid** nom de colonne à **collector_type_uid**.|  
|Correction de la description de la colonne **parameter_schema** pour indiquer que la valeur est Nullable.|  
|Ajout de la colonne **parameter_formatter** .|  
|Correction du type de données pour la colonne **collection_package_path** et mise à jour de la description pour indiquer que la valeur est Nullable.|  
|Correction du type de données pour la colonne **upload_package_path** et mise à jour de la description pour indiquer que la valeur est Nullable.|  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées du collecteur de données &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/data-collector-stored-procedures-transact-sql.md)   
 [Vues du collecteur de données &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/data-collector-views-transact-sql.md)   
 [Collecte de données](../../relational-databases/data-collection/data-collection.md)  
  
  
