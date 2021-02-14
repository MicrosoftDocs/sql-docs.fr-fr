---
title: Personnaliser les déploiements de bases de données à l’aide de contributeurs de déploiement
description: Découvrez comment modifier le comportement des projets de base de données. Affichez les ressources sur les contributeurs de build et de déploiement et consultez des exemples de scénarios qui les utilisent.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: fe2064bb-e01e-4a12-9f12-a99aa9a5203f
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 8c512a39da9a17a15e989816234a18266b5c2dfc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100066844"
---
# <a name="customize-database-build-and-deployment-by-using-build-and-deployment-contributors"></a>Personnaliser la génération et le déploiement de bases de données à l'aide de contributeurs de génération et de déploiement

Visual Studio fournit des points d'extensibilité que vous pouvez utiliser pour modifier le comportement des actions de génération et de déploiement pour les projets de base de données.  
  
## <a name="available-extensibility-points"></a>Points d'extensibilité disponibles  
Vous pouvez créer une extension pour les points d'extensibilité, comme indiqué dans le tableau suivant :  
  
|**Action**|**Type de contributeur**|**Remarques**|  
|--------------|------------------------|-------------|  
|Build|BuildContributor|Ce type d'extension est exécuté lorsque le projet SQL est généré une fois que le modèle de projet a été entièrement validé. Un contributeur de génération peut accéder au modèle terminé, ainsi qu'à toutes les propriétés de la tâche de génération et de tous les arguments personnalisés.|  
|Déployer|DeploymentPlanModifier|Ce type d'extension est exécuté lorsque le projet SQL est déployé, dans le cadre du pipeline de déploiement, une fois le plan de déploiement généré, mais avant son exécution. Vous pouvez utiliser un DeploymentPlanModifier pour modifier le plan de déploiement en ajoutant ou en supprimant des étapes. Les contributeurs de déploiement peuvent accéder au plan de déploiement, au résultat de la comparaison, ainsi qu'aux modèles source et cible.|  
|Déployer|DeploymentPlanExecutor|Ce type d'extension est exécuté lorsque le plan de déploiement est exécuté et autorise l'accès en lecture seule au plan de déploiement. Le DeploymentPlanExecutor effectue des actions basées sur le plan de déploiement.|  
  
### <a name="supported-extensibility-scenarios"></a>Scénarios d'extensibilité pris en charge  
Vous pouvez implémenter des contributeurs de génération et de déploiement pour activer les scénarios d'exemple suivant :  
  
-   **Générer la documentation d'une schéma lors de la génération du projet** : pour prendre en charge ce scénario, vous implémentez un [BuildContributor](/dotnet/api/microsoft.sqlserver.dac.deployment.buildcontributor) et remplacez la méthode OnExecute pour générer la documentation de schéma. Vous pouvez créer un fichier de cibles qui définit les arguments par défaut qui déterminent si l'extension est exécutée et qui spécifient le nom du fichier de sortie.  
  
-   **Créer un rapport de différence lorsqu'un projet SQL est déployé** : pour prendre en charge ce scénario, vous implémentez un [DeploymentPlanExecutor](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentplanexecutor), qui génère le fichier XML lorsque le projet SQL est déployé.  
  
-   **Modifier le plan de déploiement pour changer le moment du transfert des données** : pour prendre en charge ce scénario, vous implémentez un [DeploymentPlanModifier](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentplanmodifier) et l'itérez sur le plan de déploiement. Pour chaque SqlTableMigrationStep dans ce plan, vous examinez le résultat de la comparaison pour déterminer si cette opération doit être effectuée ou ignorée.  
  
-   **Copier les fichiers vers le dacpac généré lorsqu'un projet SQL est déployé** : pour prendre en charge ce scénario, vous implémentez un contributeur de déploiement et vous remplacez la méthode OnEstablishDeploymentConfiguration pour spécifier quels fichiers sont marqués comme DeploymentExtensionConfiguration par le système de projet. Ces fichiers doivent être copiés dans le dossier de sortie et ajouté dans le dacpac généré. Vous pouvez également modifier le contributeur pour qu'il fusionne plusieurs fichiers en un nouveau fichier qui est copié dans le dossier de sortie et ajouté au manifeste de déploiement. Lors du déploiement, vous pouvez appliquer la méthode OnApplyDeploymentConfiguration pour extraire ces fichiers du dacpac et les préparer pour les utiliser dans la méthode OnExecute.  
  
En outre, vous pouvez exposer des paires personnalisées d'arguments nom/valeurs de votre contributeur qui sont écrites dans le fichier de projet de base de données. Vous pouvez utiliser ces arguments pour activer un contributeur pour extraire des informations de Msbuild ou pour permettre à l'utilisateur final d'un contributeur de personnaliser le comportement. Par exemple, vous pouvez permettre aux utilisateurs de spécifier le nom d'un fichier d'entrée ou de sortie.  
  
## <a name="common-tasks"></a>Tâches courantes  
  
|**Tâches courantes**|**Contenu de prise en charge**|  
|--------------------|--------------------------|  
|**En savoir plus sur les points d’extensibilité :** vous pouvez vous documenter au sujet des classes de base que vous utilisez pour implémenter des contributeurs de génération et de déploiement.|[BuildContributor](/dotnet/api/microsoft.sqlserver.dac.deployment.buildcontributor)<br /><br />[DeploymentContributor](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentcontributor)|  
|**Créer des exemples de contributeurs :** apprenez les étapes nécessaires pour créer un contributeur de génération ou de déploiement. Si vous suivez ces procédures pas à pas, vous serez en mesure de :<br /><br />-   Créer un contributeur de génération qui génère un rapport répertoriant tous les éléments du modèle.<br />-   Créer un contributeur de déploiement qui modifie le plan de déploiement avant son exécution.<br />-   Créer un contributeur de déploiement qui génère un rapport de déploiement lorsque vous déployez un projet SQL.<br /><br />Vous pouvez créer tous les contributeurs dans un seul assembly ou sur plusieurs assemblys, selon la façon dont vous souhaitez qu'ils soient distribués à votre équipe.|[Procédure pas à pas : étendre la génération du projet de base de données à la génération de statistiques de modèle](../ssdt/walkthrough-extend-database-project-build-to-generate-model-statistics.md)<br /><br />[Procédure pas à pas : étendre le déploiement du projet de base de données pour modifier le plan de déploiement](../ssdt/walkthrough-extend-database-project-deployment-to-modify-the-deployment-plan.md)<br /><br />[Procédure pas à pas : étendre le déploiement du projet de base de données pour analyser le plan de déploiement](../ssdt/walkthrough-extend-database-project-deployment-to-analyze-the-deployment-plan.md)|  
  
## <a name="see-also"></a>Voir aussi  
[Définir des conditions personnalisées pour les tests unitaires SQL](/previous-versions/sql/sql-server-data-tools/jj860449(v=vs.103))  
