---
description: catalog.environment_variables (base de données SSISDB)
title: catalog.environment_variables (base de données SSISDB) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: 45f5aacd-505a-443b-8fc2-c7929e78cff8
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 1a6894bc1dee54ec5fa66014ed363409ebb150b6
ms.sourcegitcommit: 868c60aa3a76569faedd9b53187e6b3be4997cc9
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/08/2021
ms.locfileid: "99835277"
---
# <a name="catalogenvironment_variables-ssisdb-database"></a>catalog.environment_variables (base de données SSISDB)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

  Affiche les détails de la variable d'environnement pour tous les environnements dans le catalogue [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|variable_id|**bigint**|Identificateur (ID) unique de la variable d'environnement.|  
|environment_id|**bigint**|ID unique de l'environnement auquel la variable est associée.|  
|name|**sysname**|Nom de la variable d'environnement.|  
|description|**nvarchar(1024)**|Description de la variable d'environnement.|  
|type|**nvarchar(128)**|Type de données de la variable d'environnement.|  
|sensible|**bit**|Lorsque la valeur est `1`, la variable est sensible et est chiffrée lorsqu'elle est stockée. Lorsque la valeur est `0`, la variable n'est pas sensible et la valeur est stockée dans en texte en clair.|  
|value|**sql_variant**|Valeur de la variable d’environnement. Quand sensitive est `0`, la valeur en texte en clair est indiquée. Quand sensitive est `1`, la valeur **NULL** s’affiche.|  
  
## <a name="remarks"></a>Notes  
 Cette vue affiche une ligne pour chaque variable d'environnement dans le catalogue.  
  
## <a name="permissions"></a>Autorisations  
 Cette vue requiert l'une des autorisations suivantes :  
  
-   Autorisation READ sur l'environnement correspondant  
  
-   Appartenance au rôle de base de données **ssis_admin**  
  
-   Appartenance au rôle serveur **sysadmin**  
  
> [!NOTE]  
>  Lorsque vous avez l'autorisation pour effectuer une opération sur le serveur, vous avez également l'autorisation pour consulter les informations de l'opération. La sécurité au niveau de la ligne est imposée ; uniquement les lignes que vous avez l'autorisation d'afficher s'affichent.  
  
  
