---
description: catalog.extended_operation_info (base de données SSISDB)
title: catalog.extended_operation_info (base de données SSISDB) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: db299b45-557d-4c62-8e14-355cdb051f63
author: chugugrace
ms.author: chugu
ms.openlocfilehash: d08a1df29dbea83704165122a16d8daaff110e76
ms.sourcegitcommit: 868c60aa3a76569faedd9b53187e6b3be4997cc9
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/08/2021
ms.locfileid: "99835206"
---
# <a name="catalogextended_operation_info-ssisdb-database"></a>catalog.extended_operation_info (base de données SSISDB)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

  Affiche les informations étendues pour toutes les opérations dans le catalogue [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|info_id|**bigint**|Identificateur unique (ID) des informations étendues.|  
|operation_id|**bigint**|ID unique de l'opération qui correspond aux informations étendues.|  
|object_name|**nvarchar(260)**|Nom de l'objet.|  
|object_type|**smallint**|Type d'objet affecté par l'opération. L'objet peut être un dossier (`10`), un projet (`20`), un package (`30`), un environnement (`40`) ou une instance d'exécution (`50`).|  
|reference_id|**bigint**|ID unique de la référence utilisée dans l'opération.|  
|status|**int**|État de l'opération. Les valeurs possibles sont Créé (`1`), En cours d'exécution (`2`), Annulé (`3`), Échec (`4`), En attente (`5`), Arrêté prématurément (`6`), Opération réussie (`7`), Arrêt en cours (`8`) et Fin (`9`).|  
|start_time|**datetimeoffset(7)**|Date et heure du début de l'opération.|  
|end_time|**datetimeoffset(7)**|Date et heure auxquelles l'opération s'est terminée.|  
  
## <a name="remarks"></a>Notes  
 Une seule opération peut avoir plusieurs lignes d'informations étendues.  
  
## <a name="permissions"></a>Autorisations  
 Cette vue requiert l'une des autorisations suivantes :  
  
-   Autorisation READ sur l'opération  
  
-   Appartenance au rôle de base de données **ssis_admin**  
  
-   Appartenance au rôle serveur **sysadmin**  
  
> [!NOTE]  
>  Lorsque vous avez l'autorisation pour effectuer une opération sur le serveur, vous avez également l'autorisation pour consulter les informations de l'opération. La sécurité au niveau de la ligne est imposée ; uniquement les lignes que vous avez l'autorisation d'afficher s'affichent.  
  
  
