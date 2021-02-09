---
description: catalog.catalog_properties (base de données SSISDB)
title: catalog.catalog_properties (base de données SSISDB) | Microsoft Docs
ms.custom: ''
ms.date: 12/11/2018
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: e604a382-95c8-4764-b268-742eb5c6d4cf
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 4028247051220e1e45d054e400da050601254fb2
ms.sourcegitcommit: 868c60aa3a76569faedd9b53187e6b3be4997cc9
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/08/2021
ms.locfileid: "99835845"
---
# <a name="catalogcatalog_properties-ssisdb-database"></a>catalog.catalog_properties (base de données SSISDB)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

  Affiche les propriétés du catalogue [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|property_name|**nvarchar (256)**|Nom de la propriété de catalogue.|  
|property_value|**nvarchar (256)**|Valeur de la propriété de catalogue.|  
  
## <a name="remarks"></a>Notes  
 Cette vue affiche une ligne pour chaque propriété de catalogue.
  
|Nom de la propriété|Description|  
|-------------------|-----------------|  
|**DEFAULT_EXECUTION_MODE**|Mode d’exécution par défaut à l’échelle du serveur pour les packages : `Server` (0) ou `Scale Out` (1). |
|**ENCRYPTION_ALGORITHM**|Type d'algorithme de chiffrement utilisé pour chiffrer des données sensibles. Les valeurs prises en charge incluent : `DES`, `TRIPLE_DES`, `TRIPLE_DES_3KEY`, `DESX`, `AES_128`, `AES_192` et `AES_256`. Remarque : la base de données de catalogue doit être en mode mono-utilisateur pour modifier cette propriété.|
|**IS_SCALEOUT_ENABLED**|Si la valeur est `True`, la fonctionnalité SSIS Scale Out est activée. Si vous n’avez pas activé Scale Out, cette propriété peut ne pas apparaître dans la vue.|
|**MAX_PROJECT_VERSIONS**|Nombre de nouvelles versions du projet qui sont conservées pour un projet unique. Quand le nettoyage de version est permis, les versions antérieures au-delà de ce nombre sont supprimées.|  
|**OPERATION_CLEANUP_ENABLED**|Quand la valeur est `TRUE`, les détails et les messages de l’opération plus anciens que **RETENTION_WINDOW** (jours) sont supprimés du catalogue. Lorsque la valeur est `FALSE`, tous les détails et les messages de l'opération sont stockés dans le catalogue. Remarque : un travail [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] effectue le nettoyage de l'opération.|  
|**RETENTION_WINDOW**|Nombre de jours que les détails et les messages de l'opération sont stockés dans le catalogue. Lorsque la valeur est `-1`, la période de conservation est infinie. Remarque : Si aucun nettoyage n’est souhaité, affectez à **OPERATION_CLEANUP_ENABLED** la valeur **FALSE**.|
|**SCHEMA_BUILD**|Numéro de build du schéma de la base de données du catalogue SSISDB. Ce numéro change chaque fois que le catalogue SSISDB est créé ou mis à niveau.|
|**SCHEMA_VERSION**|Numéro de version principale du schéma de la base de données du catalogue SSISDB. Ce numéro change chaque fois que le catalogue SSISDB est créé ou que la version principale est mise à niveau.|
|**VALIDATION_TIMEOUT**|Les validations sont arrêtées si elles ne s’achèvent pas dans le nombre de secondes spécifié par cette propriété.|  
|**SERVER_CUSTOMIZED_LOGGING_LEVEL**|Niveau de journalisation personnalisée par défaut pour le serveur [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. Si vous n’avez pas créé de niveaux de journalisation personnalisée, cette propriété peut ne pas apparaître dans la vue.|
|**SERVER_LOGGING_LEVEL**|Niveau de journalisation par défaut pour le serveur [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].|
|**SERVER_OPERATION_ENCRYPTION_LEVEL**|Quand la valeur est 1 (`PER_EXECUTION`), le certificat et la clé symétrique utilisés pour la protection des paramètres d’exécution sensibles et des journaux d’exécution sont créés pour chaque *exécution*. Quand la valeur est 2 (`PER_PROJECT`), le certificat et la clé symétrique sont créés une fois par *projet*. Pour plus d’informations sur cette propriété, consultez la section Notes sur la procédure stockée SSIS [catalog.cleanup_server_log](../system-stored-procedures/catalog-cleanup-server-log.md#remarks).|
|**VERSION_CLEANUP_ENABLED**|Quand la valeur est `TRUE`, seul le nombre de versions de projet **MAX_PROJECT_VERSIONS** est stocké dans le catalogue et toutes les autres versions de projet seront supprimées. Quand la valeur est **FALSE**, toutes les versions du projet sont stockées dans le catalogue. Remarque : un travail [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] effectue le nettoyage de l'opération.|
|||
  
## <a name="permissions"></a>Autorisations  
 Cette vue requiert l'une des autorisations suivantes :  
  
-   Appartenance au rôle de base de données **ssis_admin**  
  
-   Appartenance au rôle serveur **sysadmin**  
  
  
