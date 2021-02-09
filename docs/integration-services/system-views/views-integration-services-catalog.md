---
description: Vues (catalogue Integration Services)
title: Vues (catalogue Integration Services) | Microsoft Docs
ms.custom: ''
ms.date: 12/16/2016
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
helpviewer_keywords:
- views [Integration Services]
ms.assetid: d0294d43-4852-46dc-9afa-d0c19ea9aa03
author: chugugrace
ms.author: chugu
ms.openlocfilehash: b4e067dc38f6bd511237ab05491feda4307106c4
ms.sourcegitcommit: 868c60aa3a76569faedd9b53187e6b3be4997cc9
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/08/2021
ms.locfileid: "99835985"
---
# <a name="views-integration-services-catalog"></a>Vues (catalogue Integration Services)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

  Cette section décrit les vues [!INCLUDE[tsql](../../includes/tsql-md.md)] qui sont disponibles pour l'administration des projets [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] déployés dans une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Interrogez les vues [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] pour inspecter les objets, les paramètres et les données opérationnelles stockés dans le catalogue **SSISDB**.  
  
 Le nom par défaut du catalogue est SSISDB. Les objets stockés dans le catalogue incluent des projets, des packages, des paramètres, des environnements et l'historique opérationnel.  
  
 Vous pouvez utiliser directement les vues de base de données et les procédures stockées, ou écrire du code personnalisé qui appelle l'API managée. [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] et l'API managée interrogent les vues et appellent les procédures stockées décrites dans cette section pour effectuer de nombreuses tâches.  
  
## <a name="in-this-section"></a>Dans cette section  
 [catalog.catalog_properties &#40;base de données SSISDB&#41;](../../integration-services/system-views/catalog-catalog-properties-ssisdb-database.md)  
 Affiche les propriétés du catalogue [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
 [catalog.effective_object_permissions &#40;base de données SSISDB&#41;](../../integration-services/system-views/catalog-effective-object-permissions-ssisdb-database.md)  
 Affiche les autorisations efficaces pour l'entité actuelle pour tous les objets dans le catalogue [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
 [catalog.environment_variables &#40;Base de données SSISDB&#41;](../../integration-services/system-views/catalog-environment-variables-ssisdb-database.md)  
 Affiche les détails de la variable d'environnement pour tous les environnements dans le catalogue [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
 [catalog.environments &#40;Base de données SSISDB&#41;](../../integration-services/system-views/catalog-environments-ssisdb-database.md)  
 Affiche les détails de l'environnement pour tous les environnements dans le catalogue [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. Les environnements contiennent des variables qui peuvent être référencées par les projets [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
 [catalog.execution_parameter_values &#40;Base de données SSISDB&#41;](../../integration-services/system-views/catalog-execution-parameter-values-ssisdb-database.md)  
 Affiche les valeurs de paramètre effectives utilisées par les packages [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] pendant une instance d'exécution.  
  
 [catalog.executions &#40;Base de données SSISDB&#41;](../../integration-services/system-views/catalog-executions-ssisdb-database.md)  
 Affiche les instances d'exécution du package dans le catalogue [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. Packages exécutés avec la tâche d'exécution du package dans la même instance d'exécution comme package parent.  
  
 [catalog.explicit_object_permissions &#40;base de données SSISDB&#41;](../../integration-services/system-views/catalog-explicit-object-permissions-ssisdb-database.md)  
 Affiche uniquement les autorisations affectées explicitement à l'utilisateur.  
  
 [catalog.extended_operation_info &#40;Base de données SSISDB&#41;](../../integration-services/system-views/catalog-extended-operation-info-ssisdb-database.md)  
 Affiche les informations étendues pour toutes les opérations dans le catalogue [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
 [catalog.folders &#40;base de données SSISDB&#41;](../../integration-services/system-views/catalog-folders-ssisdb-database.md)  
 Affiche les dossiers dans le catalogue [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
 [catalog.object_parameters &#40;Base de données SSISDB&#41;](../../integration-services/system-views/catalog-object-parameters-ssisdb-database.md)  
 Affiche les paramètres de tous les packages et projets dans le catalogue [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
 [catalog.object_versions &#40;Base de données SSISDB&#41;](../../integration-services/system-views/catalog-object-versions-ssisdb-database.md)  
 Affiche les versions d'objets dans le catalogue [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. Dans cette version, seules les versions des projets sont prises en charge par cette vue.  
  
 [catalog.operation_messages &#40;Base de données SSISDB&#41;](../../integration-services/system-views/catalog-operation-messages-ssisdb-database.md)  
 Affiche des messages entrés pendant des opérations dans le catalogue [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
 [catalog.operations &#40;Base de données SSISDB&#41;](../../integration-services/system-views/catalog-operations-ssisdb-database.md)  
 Affiche les détails de toutes les opérations dans le catalogue [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
 [catalog.packages &#40;Base de données SSISDB&#41;](../../integration-services/system-views/catalog-packages-ssisdb-database.md)  
 Affiche les détails pour tous les packages qui s'affichent dans le catalogue [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
 [catalog.environment_references &#40;Base de données SSISDB&#41;](../../integration-services/system-views/catalog-environment-references-ssisdb-database.md)  
 Affiche les références environnementales pour tous les projets dans le catalogue [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
 [catalog.projects &#40;Base de données SSISDB&#41;](../../integration-services/system-views/catalog-projects-ssisdb-database.md)  
 Affiche les détails pour tous les projets qui s'affichent dans le catalogue [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
 [catalog.validations &#40;base de données SSISDB&#41;](../../integration-services/system-views/catalog-validations-ssisdb-database.md)  
 Affiche les détails de toutes les validations du package et du projet dans le catalogue [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
[catalog.master_properties &#40;base de données SSISDB&#41;](../../integration-services/system-views/catalog-master-properties-ssisdb-database.md)  
Affiche les propriétés d’[!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] Scale Out Master.

[catalog.worker_agents &#40;base de données SSISDB&#41;](../../integration-services/system-views/catalog-worker-agents-ssisdb-database.md)  
Affiche les informations d’[!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] Scale Out Worker.  
