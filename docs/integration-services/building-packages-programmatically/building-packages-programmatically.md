---
description: Génération de packages par programme
title: Génération de packages par programmation | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
ms.assetid: 7474b1f4-7607-4f28-a6fd-67f7db1dd3f8
author: chugugrace
ms.author: chugu
ms.openlocfilehash: daf54aa01dcea6d5c63fba21a3b4ffaf61f2cdd9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351008"
---
# <a name="building-packages-programmatically"></a>Génération de packages par programme

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Si vous devez créer des packages de manière dynamique, ou gérer et exécuter des packages [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] à l'extérieur de l'environnement de développement, vous pouvez manipuler les packages par programme. Cette méthode vous offre une gamme continue d'options :  
  
-   Charger et exécuter un package existant sans modification.  
  
-   Charger un package existant, le reconfigurer (par exemple, pour une source de données différente) et l'exécuter.  
  
-   Créer un package, ajouter et configurer des composants objet par objet et propriété par propriété, l'enregistrer, puis l'exécuter.  
  
 Vous pouvez utiliser le modèle d'objet [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] pour écrire du code qui crée, configure et exécute des packages dans n'importe quel langage de programmation managé. Par exemple, vous pouvez créer des packages, pilotés par des métadonnées, qui configurent leurs connexions ou leurs sources de données, transformations et destinations selon la source de données sélectionnée, ainsi que ses tables et ses colonnes.  
  
 Cette section montre comment créer et configurer un package par programme, ligne par ligne. L’option de programmation de package la moins complexe vous permet simplement de charger et d’exécuter un package existant sans modification, comme décrit dans [Exécution et gestion de packages par programmation](../../integration-services/run-manage-packages-programmatically/running-and-managing-packages-programmatically.md).  
  
 Une option intermédiaire, qui n'est pas décrite dans cet article, consiste à charger un package existant en tant que modèle, le reconfigurer (par exemple, pour une source de données différente) et l'exécuter. Vous pouvez également utiliser les informations de cette section pour modifier les objets existants dans un package.  
  
> [!NOTE]  
>  Lorsque vous utilisez un package existant comme modèle et modifiez des colonnes existantes dans le flux de données, vous pourriez devoir supprimer les colonnes existantes et appeler la méthode <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ReinitializeMetaData%2A> des composants affectés.  
  
## <a name="in-this-section"></a>Dans cette section  
 [Création d'un package par programme](../../integration-services/building-packages-programmatically/creating-a-package-programmatically.md)  
 Explique comment créer un package par programme.  
  
 [Ajout de tâches par programmation](../../integration-services/building-packages-programmatically/adding-tasks-programmatically.md)  
 Explique comment ajouter des tâches au package.  
  
 [Connexion de tâches par programme](../../integration-services/building-packages-programmatically/connecting-tasks-programmatically.md)  
 Décrit comment contrôler l'exécution des conteneurs et des tâches dans un package en fonction du résultat de l'exécution d'une tâche ou d'un conteneur précédent.  
  
 [Ajout de connexions par programme](../../integration-services/building-packages-programmatically/adding-connections-programmatically.md)  
 Décrit comment ajouter des gestionnaires de connexions à un package.  
  
 [Utilisation de variables par programmation](../../integration-services/building-packages-programmatically/working-with-variables-programmatically.md)  
 Décrit comment ajouter et utiliser des variables pendant l'exécution d'un package.  
  
 [Gestion d'événements par programme](../../integration-services/building-packages-programmatically/handling-events-programmatically.md)  
 Décrit comment gérer des événements de package et de tâche.  
  
 [Activation de la journalisation par programme](../../integration-services/building-packages-programmatically/enabling-logging-programmatically.md)  
 Décrit comment activer la journalisation d'un package ou d'une tâche et appliquer des filtres personnalisés à des événements de journal.  
  
 [Ajout de la tâche de flux de données par programmation](../../integration-services/building-packages-programmatically/adding-the-data-flow-task-programmatically.md)  
 Décrit comment ajouter et configurer la tâche de flux de données et ses composants.  
  
 [Découverte des composants de flux de données par programme](../../integration-services/building-packages-programmatically/discovering-data-flow-components-programmatically.md)  
 Décrit comment détecter les composants installés sur l'ordinateur local.  
  
 [Ajout de composants de flux de données par programme](../../integration-services/building-packages-programmatically/adding-data-flow-components-programmatically.md)  
 Décrit comment ajouter un composant à une tâche de flux de données.  
  
 [Connexion de composants de flux de données par programme](../../integration-services/building-packages-programmatically/connecting-data-flow-components-programmatically.md)  
 Décrit comment connecter deux composants de flux de données.  
  
 [Sélection de colonnes d'entrée par programme](../../integration-services/building-packages-programmatically/selecting-input-columns-programmatically.md)  
 Décrit comment sélectionner des colonnes d'entrée parmi celles fournies à un composant par les composants situés en amont du flux de données.  
  
 [Enregistrement d'un package par programme](../../integration-services/building-packages-programmatically/saving-a-package-programmatically.md)  
 Explique comment enregistrer un package par programme.  
  
## <a name="reference"></a>Informations de référence  
 [Guide de référence des erreurs et des messages propres à Integration Services](../../integration-services/integration-services-error-and-message-reference.md)  
 Répertorie les codes d'erreur [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] prédéfinis avec leur nom symbolique et leur description.  
  
## <a name="related-sections"></a>Sections connexes  
 [Extension de packages avec des scripts](../../integration-services/extending-packages-scripting/extending-packages-with-scripting.md)  
 Explique comment étendre le flux de contrôle à l'aide de la tâche de script, et comment étendre le flux de données à l'aide du composant Script.  
  
 [Extension de packages avec des objets personnalisés](../../integration-services/extending-packages-custom-objects/extending-packages-with-custom-objects.md)  
 Explique comment créer des tâches personnalisées de programme, des composants de flux de données et d'autres objets de package à utiliser dans plusieurs packages.  
  
 [Exécution et gestion de packages par programme](../../integration-services/run-manage-packages-programmatically/running-and-managing-packages-programmatically.md)  
 Explique comment énumérer, exécuter et gérer des packages et les dossiers dans lesquels ils sont stockés.  
  
## <a name="external-resources"></a>Ressources externes  
  
-   Exemples de code CodePlex, [Integration Services Product Samples](https://go.microsoft.com/fwlink/?LinkID=131204), sur www.codeplex.com/MSFTISProdSamples  
  
-   Entrée de blog, [Performance profiling your custom extensions](https://techcommunity.microsoft.com/t5/sql-server-integration-services/performance-profiling-your-custom-extensions/ba-p/387490), sur le site blogs.msdn.com.  

## <a name="see-also"></a>Voir aussi  
 [SQL Server Integration Services](../../integration-services/sql-server-integration-services.md)  
  
  
