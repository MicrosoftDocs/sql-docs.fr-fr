---
description: catalog.packages (base de données SSISDB)
title: catalog.packages (base de données SSISDB) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
helpviewer_keywords:
- packages view [Integration Services]
- catalog.packages view [Integration Services]
ms.assetid: a634e94d-f492-4dfd-9611-a35f545106a1
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 7d506b377970879c865ed807a4377a0c1bc1241b
ms.sourcegitcommit: 868c60aa3a76569faedd9b53187e6b3be4997cc9
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/08/2021
ms.locfileid: "99835238"
---
# <a name="catalogpackages-ssisdb-database"></a>catalog.packages (base de données SSISDB)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

  Affiche les détails pour tous les packages qui s’affichent dans le catalogue **SSISDB**.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|package_id|**bigint**|Identificateur (ID) unique du package.|  
|name|**nvarchar (256)**|Nom unique du package.|  
|package_guid|**uniqueidentifier**|Identificateur global unique (GUID) qui identifie le package.|  
|description|**nvarchar(1024)**|Description facultative du package.|  
|package_format_version|**int**|Version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilisée pour développer le package.|  
|version_major|**int**|Version principale du package.|  
|version_minor|**int**|Version secondaire du package.|  
|version_build|**int**|Version de la build du package.|  
|version_comments|**nvarchar(1024)**|Commentaires facultatifs sur la version du package.|  
|version_guid|**uniqueidentifier**|GUID qui identifie de manière unique la version du package.|  
|project_id|**bigint**|ID unique du projet.|  
|entry_point|**bit**|Une valeur de `1` signifie que le package est destiné à être démarré directement. Une valeur `0` signifie que le package est destiné à être démarré par un autre package avec la tâche d'exécution du package. La valeur par défaut est `1`.|  
|validation_status|**char(1)**|État de la validation.|  
|last_validation_time|**datetimeoffset(7)**|Heure de la dernière validation.|  
  
## <a name="remarks"></a>Notes  
 Cette vue affiche une ligne pour chaque package dans le catalogue.  
  
## <a name="permissions"></a>Autorisations  
 Cette vue requiert l'une des autorisations suivantes :  
  
-   Autorisation READ sur le projet correspondant  
  
-   Appartenance au rôle de base de données **ssis_admin**  
  
-   Appartenance au rôle serveur **sysadmin**.  
  
> [!NOTE]  
>  Si vous avez l'autorisation READ sur un projet, vous avez également l'autorisation READ sur toutes les packages et les références environnementales associés à ce projet. La sécurité au niveau de la ligne est imposée ; uniquement les lignes que vous avez l'autorisation d'afficher s'affichent.  
  
  
