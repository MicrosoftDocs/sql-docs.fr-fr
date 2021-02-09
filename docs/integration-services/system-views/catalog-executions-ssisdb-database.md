---
description: catalog.executions (base de données SSISDB)
title: catalog.executions (base de données SSISDB) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
helpviewer_keywords:
- executions view [Integration Services]
- catalog.executions view [Integration Services]
ms.assetid: 879f13b0-331d-4dee-a079-edfaca11ae5b
author: chugugrace
ms.author: chugu
ms.openlocfilehash: a95232db301aa538a166e930c4dde830691adaac
ms.sourcegitcommit: 868c60aa3a76569faedd9b53187e6b3be4997cc9
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/08/2021
ms.locfileid: "99835103"
---
# <a name="catalogexecutions-ssisdb-database"></a>catalog.executions (base de données SSISDB)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

  Affiche les instances d'exécution du package dans le catalogue [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. Packages exécutés avec la tâche d'exécution du package dans la même instance d'exécution comme package parent.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|execution_id|**bigint**|Identificateur unique (ID) de l’instance d’exécution.|  
|folder_name|**sysname(nvarchar(128))**|Nom du dossier qui contient le projet.|  
|project_name|**sysname(nvarchar(128))**|Nom du projet.|  
|package_name|**nvarchar(260)**|Nom du premier package démarré pendant l'exécution.|  
|reference_id|**bigint**|Environnement référencé par l'instance d'exécution.|  
|reference_type|**char(1)**|Indique si l'environnement peut se trouver dans le même dossier que le projet (référence relative) ou dans un dossier différent (référence absolue). Lorsque la valeur est `R`, l'environnement est localisé à l'aide d'une référence relative. Lorsque la valeur est `A`, l'environnement est localisé à l'aide d'une référence absolue.|  
|environment_folder_name|**nvarchar(128)**|Nom du dossier qui contient l'environnement.|  
|environment_name|**nvarchar(128)**|Nom de l'environnement référencé pendant l'exécution.|  
|project_lsn|**bigint**|Version du projet qui correspond à l'instance d'exécution. Ce nombre n'est pas obligatoirement séquentiel.|  
|executed_as_sid|**varbinary(85)**|SID de l'utilisateur qui a démarré l'exécution.|  
|executed_as_name|**nvarchar(128)**|Nom du principal de la base de données utilisé pour démarrer l'instance d'exécution.|  
|use32bitruntime|**bit**|Indique si l'exécution 32 bits est utilisée pour exécuter le package sur un système d'exploitation 64 bits. Quand la valeur est `1`, l’exécution 32 bits est utilisée. Lorsque la valeur est `0`, l'exécution 64 bits est utilisée.|  
|object_type|**smallint**|Type d'objet. L'objet peut être un projet (`20`) ou un package (`30`).|  
|object_id|**bigint**|ID de l'objet affecté par l'opération.|  
|status|**int**|État de l'opération. Les valeurs possibles sont Créé (`1`), En cours d'exécution (`2`), Annulé (`3`), Échec (`4`), En attente (`5`), Arrêté prématurément (`6`), Opération réussie (`7`), Arrêt en cours (`8`) et Fin (`9`).|  
|start_time|**datetimeoffset**|Heure de démarrage de l'instance d'exécution.|  
|end_time|**datetimeoffsset**|Heure d'exécution de l'instance d'exécution.|  
|caller_sid|**varbinary(85)**|ID de sécurité (SID) de l'utilisateur si l'Authentification Windows a été utilisée pour se connecter.|  
|caller_name|**nvarchar(128)**|Nom du compte qui a effectué l'opération.|  
|process_id|**int**|ID de processus externe, le cas échéant.|  
|stopped_by_sid|**varbinary(85)**|ID de sécurité (SID) de l'utilisateur qui a arrêté l'instance d'exécution.|  
|stopped_by_name|**nvarchar(128)**|Nom de l'utilisateur qui a arrêté l'instance d'exécution.|  
|total_physical_memory_kb|**bigint**|Mémoire physique totale (en mégaoctets) sur le serveur au démarrage de l'exécution.|  
|available_physical_memory_kb|**bigint**|Mémoire physique disponible (en mégaoctets) sur le serveur au démarrage de l'exécution.|  
|total_page_file_kb|**bigint**|Mémoire de pages totale (en mégaoctets) sur le serveur au démarrage de l'exécution.|  
|available_page_file_kb|**bigint**|Mémoire de pages disponible (en mégaoctets) sur le serveur au démarrage de l'exécution.|  
|cpu_count|**int**|Nombre d'UC logiques sur le serveur au démarrage de l'exécution.|  
|nom_serveur|**nvarchar(128)**|Informations relatives à l'instance et au serveur Windows pour une instance spécifique de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|machine_name|**nvarchar(128)**|Nom de l'ordinateur sur lequel s'exécute l'instance du serveur.|  
|dump_id|**uniqueidentifier**|ID d'un vidage d'exécution.|  
  
## <a name="remarks"></a>Notes  
 Cette vue affiche une ligne pour chaque instance d'exécution dans le catalogue.  
  
## <a name="permissions"></a>Autorisations  
 Cette vue requiert l'une des autorisations suivantes :  
  
-   Autorisation READ sur l'instance d'exécution  
  
-   Appartenance au rôle de base de données **ssis_admin**  
  
-   Appartenance au rôle serveur **sysadmin**  
  
> [!NOTE]  
>  La sécurité au niveau de la ligne est imposée ; uniquement les lignes que vous avez l'autorisation d'afficher s'affichent.  
  
  
