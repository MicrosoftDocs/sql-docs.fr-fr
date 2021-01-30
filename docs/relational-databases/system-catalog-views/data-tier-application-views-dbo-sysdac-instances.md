---
description: Affichages des applications de la couche données-dbo.sysdac_instances
title: dbo.sysdac_instances (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dbo.sysdac_instances_TSQL
- sysdac_instances
- sysdac_instances_TSQL
- dbo.sysdac_instances
dev_langs:
- TSQL
helpviewer_keywords:
- dbo.sysdac_instances
- sysdac_instances
ms.assetid: 28285f3d-3889-439f-8b24-3bdef08e46b4
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: b548d5a92b20df8256b4e54b566dd8e6615b754a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201428"
---
# <a name="data-tier-application-views---dbosysdac_instances"></a>Affichages des applications de la couche données-dbo.sysdac_instances
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Affiche une ligne pour chaque instance d’application de la couche données (DAC) déployée sur une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)] . sysdac_instances appartient au schéma dbo dans la base de données msdb. Le tableau suivant décrit les colonnes de la vue sysdac_instances.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|instance_id|**uniqueidentifier**|Identificateur de l'instance DAC.|  
|nom_instance|**sysname**|Nom de l'instance DAC spécifiée quand la DAC a été déployée.|  
|type_name|**sysname**|Nom de la DAC spécifiée quand le package DAC a été créé.|  
|type_version|**nvarchar (64)**|Version de la DAC spécifiée quand le package DAC a été créé.|  
|description|**nvarchar(4000)**|Description de la DAC écrite quand le package DAC a été créé.|  
|type_stream|**varbinary(max)**|Flux de données qui contient une représentation encodée des objets logiques, tels que les tables et vues, contenus dans la DAC.|  
|date_created|**datetime**|Date et heure de création de l'instance DAC.|  
|created_by|**sysname**|Connexion qui a créé l'instance DAC.|  
|database_name|**sysname**|Nom de la base de données créée pour l'instance DAC.|  
  
## <a name="remarks"></a>Notes  
 Une DAC inclut un type DAC, à savoir une définition des objets de couche Données logiques utilisés par une application, tels que des tables et des vues. Un package DAC est un fichier utilisé pour déployer une DAC. Le package DAC contient une représentation de tous les objets logiques contenus dans le type DAC. Le package DAC peut être utilisé pour déployer une ou plusieurs copies, ou instances, de la DAC vers une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Chaque instance DAC déployée à partir du même package DAC partage le même type, mais reçoit un nom d'instance et un identificateur uniques.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle serveur fixe sysadmin pour afficher toutes les colonnes. Les membres du rôle public peuvent consulter les colonnes instance_name, description et type_version.  
  
## <a name="see-also"></a>Voir aussi  
 [Applications de la couche Données](../../relational-databases/data-tier-applications/data-tier-applications.md)   
 [Affichages des applications de la couche données &#40;Transact-SQL&#41;]()  
  
