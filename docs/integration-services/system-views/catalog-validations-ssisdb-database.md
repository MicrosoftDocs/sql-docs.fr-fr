---
description: catalog.validations (base de données SSISDB)
title: catalog.validations (base de données SSISDB) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: dbafe110-b480-48f3-b45f-31d71ca68f62
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 92a6fa341d231a4337b4377aab429b5d97eed5d4
ms.sourcegitcommit: 868c60aa3a76569faedd9b53187e6b3be4997cc9
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/08/2021
ms.locfileid: "99835147"
---
# <a name="catalogvalidations-ssisdb-database"></a>catalog.validations (base de données SSISDB)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

  Affiche les détails de toutes les validations du package et du projet dans le catalogue [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|validation_id|**bigint**|Identificateur (ID) unique de la validation.|  
|environment_scope|**Char (1)**|Indique les références environnementales considérées par la validation. Lorsque la valeur est `A`, toutes les références environnementales associées au projet sont incluses dans la validation. Lorsque la valeur est `S`, seule une référence environnementale unique est incluse. Lorsque la valeur est `D`, aucune référence environnementale n'est incluse et chaque paramètre doit avoir une valeur par défaut littérale pour passer la validation.|  
|validate_type|**Char (1)**|Type de validation à réaliser. Les types de validation possibles sont la validation de dépendance (`D`) ou la validation complète (`F`). La validation du package est toujours une validation complète.|  
|folder_name|**nvarchar(128)**|Nom du dossier qui contient le projet correspondant.|  
|project_name|**nvarchar(128)**|Nom du projet.|  
|project_lsn|**bigint**|Version du projet validé.|  
|use32bitruntime|**bit**|Indique si l'exécution 32 bits est utilisée pour exécuter le package sur un système d'exploitation 64 bits. Quand la valeur est `1`, l’exécution 32 bits est utilisée. Lorsque la valeur est `0`, l'exécution 64 bits est utilisée.|  
|reference_id|**bigint**|ID unique de la référence d'environnement de projet utilisée par le projet pour référencer un environnement.|  
|operation_type|**smallint**|Type d’opération. Les opérations affichées dans cette vue incluent la validation de projet (`300`) et la validation du package (`301`).|  
|object_name|**nvarhcar(260)**|Nom de l'objet.|  
|object_type|**smallint**|Type d'objet. L'objet peut être un projet (`20`) ou un package (`30`).|  
|object_id|**bigint**|ID de l'objet affecté par l'opération.|  
|start_time|**datetimeoffset(7)**|Heure de début de l'opération, si disponible.|  
|end_time|**datetimeoffsset(7)**|Heure à laquelle l'opération s'est terminée.|  
|status|**int**|État de l'opération. Les valeurs possibles sont Créé (`1`), En cours d'exécution (`2`), Annulé (`3`), Échec (`4`), En attente (`5`), Arrêté prématurément (`6`), Opération réussie (`7`), Arrêt en cours (`8`) et Fin (`9`).|  
|caller_sid|**varbinary(85)**|ID de sécurité (SID) de l'utilisateur si l'Authentification Windows a été utilisée pour se connecter.|  
|caller_name|**nvarchar(128)**|Nom du compte qui a effectué l'opération.|  
|process_id|**int**|ID de processus externe, le cas échéant.|  
|stopped_by_sid|**varbinary(85)**|SID de l'utilisateur qui a arrêté l'opération.|  
|stopped_by_name|**nvarchar(128)**|Nom de l'utilisateur qui a arrêté l'opération.|  
|nom_serveur|**nvarchar(128)**|Informations relatives à l'instance et au serveur Windows pour une instance spécifique de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|machine_name|**nvarchar(128)**|Nom de l'ordinateur sur lequel s'exécute l'instance du serveur.|  
|dump_id|**uniqueidentifier**|ID du vidage d'exécution.|  
  
## <a name="remarks"></a>Notes  
 Cette vue affiche une ligne pour chaque validation dans le catalogue [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
## <a name="permissions"></a>Autorisations  
 Cette vue requiert l'une des autorisations suivantes :  
  
-   Autorisation READ sur l'opération correspondante  
  
-   Appartenance au rôle de base de données **ssis_admin**  
  
-   Appartenance au rôle serveur **sysadmin**  
  
> [!NOTE]  
>  Lorsque vous avez l'autorisation pour effectuer une opération sur le serveur, vous avez également l'autorisation pour consulter les informations de l'opération. La sécurité au niveau de la ligne est imposée ; uniquement les lignes que vous avez l'autorisation d'afficher s'affichent.  
  
  
