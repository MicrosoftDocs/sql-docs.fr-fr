---
description: Tables d’applications de la couche Données - sysdac_instances_internal
title: sysdac_instances_internal (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sysdac_instances_internal_TSQL
- sysdac_instances_internal
dev_langs:
- TSQL
helpviewer_keywords:
- sysdac_instances_internal
ms.assetid: d2d52cc4-3463-431a-b779-6fbfdeee1dfc
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 7ccda567989211074c8d151de7f2b3936ccd2b69
ms.sourcegitcommit: dd36d1cbe32cd5a65c6638e8f252b0bd8145e165
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/08/2020
ms.locfileid: "89544607"
---
# <a name="data-tier-application-tables---sysdac_instances_internal"></a>Tables d’applications de la couche Données - sysdac_instances_internal
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Affiche une ligne pour chaque instance d’application de la couche données (DAC) déployée sur une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)] . Cette table est stockée dans le schéma dbo de la base de données msdb.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|instance_id|**uniqueidentifier**|Identificateur de l'instance DAC.|  
|nom_instance|**sysname**|Nom de l'instance DAC spécifiée quand l'instance a été déployée.|  
|type_name|**sysname**|Nom de la DAC spécifiée quand le package DAC a été créé.|  
|type_version|**nvarchar (64)**|Version de la DAC spécifiée quand le package DAC a été créé.|  
|description|**nvarchar(4000)**|Description de la DAC écrite quand le package DAC a été créé.|  
|type_stream|**varbinary(max)**|Flux de données qui contient la représentation encodée des objets logiques, tels que les tables et vues, contenus dans la DAC.|  
|date_created|**datetime**|Date et heure de création de l'instance DAC.|  
|created_by|**sysname**|Connexion qui a créé l'instance DAC|  
  
## <a name="remarks"></a>Notes  
 L’accès en lecture seule à cette vue est disponible pour tous les utilisateurs disposant d’autorisations pour se connecter à la base de données Master.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle serveur fixe sysadmin.  
  
## <a name="see-also"></a>Voir aussi  
 [Applications de la couche Données](../../relational-databases/data-tier-applications/data-tier-applications.md)   
 [dbo.sysdac_instances &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/data-tier-application-views-dbo-sysdac-instances.md)  
  
  
