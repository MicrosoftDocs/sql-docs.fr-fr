---
description: Développement d'une tâche personnalisée
title: Développement d’une tâche personnalisée | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
dev_langs:
- VB
- CSharp
helpviewer_keywords:
- custom tasks [Integration Services], about custom tasks
- Task class
- custom tasks [Integration Services]
- SSIS custom tasks
- SSIS custom tasks, about custom tasks
- IDtsTaskUI interface
- DtsTaskAttribute attribute
- tasks [Integration Services], custom
- TaskHost object
ms.assetid: dcbd8615-fa6d-4ddb-b8a5-0b19dddd6239
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 37a5d7a613acbe6a8a9894c985ba3aee8ade53f8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100337594"
---
# <a name="developing-a-custom-task"></a>Développement d'une tâche personnalisée

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] utilise des tâches pour effectuer des unités de travail en soutien à l'extraction, la transformation et le chargement de données. [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] inclut une variété de tâches qui effectuent les actions les plus fréquemment utilisées, allant de l'exécution d'une instruction SQL au téléchargement d'un fichier à partir d'un site FTP. Si les tâches incluses et les actions prises en charge ne répondent pas complètement à vos besoins, vous pouvez créer une tâche personnalisée.  
  
 Pour créer une tâche personnalisée, vous devez créer une classe qui hérite de la classe de base [Microsoft.SqlServer.Dts.Runtime.Task](/dotnet/api/microsoft.sqlserver.dts.runtime.task), appliquer l’attribut <xref:Microsoft.SqlServer.Dts.Runtime.DtsTaskAttribute> à votre nouvelle classe et remplacer les méthodes et propriétés importantes de la classe de base, notamment la méthode <xref:Microsoft.SqlServer.Dts.Runtime.Task.Execute%2A>.  
  
## <a name="in-this-section"></a>Dans cette section  
 Cette section explique comment créer, configurer et coder une tâche personnalisée et son interface utilisateur personnalisée facultative.  
  
 [Création d’une tâche personnalisée](../../../integration-services/extending-packages-custom-objects/task/creating-a-custom-task.md)  
 Décrit la première étape, à savoir la création de la tâche personnalisée.  
  
 [Codage d’une tâche personnalisée](../../../integration-services/extending-packages-custom-objects/task/coding-a-custom-task.md)  
 Décrit comment coder les méthodes principales d'une tâche personnalisée.  
  
 [Connexion à des sources de données dans une tâche personnalisée](../../../integration-services/extending-packages-custom-objects/task/connecting-to-data-sources-in-a-custom-task.md)  
 Décrit comment connecter une tâche personnalisée à une source de données.  
  
 [Déclenchement et définition d’événements dans une tâche personnalisée](../../../integration-services/extending-packages-custom-objects/task/raising-and-defining-events-in-a-custom-task.md)  
 Décrit comment déclencher des événements et définir des événements personnalisés à partir de la tâche personnalisée.  
  
 [Ajout de la prise en charge du débogage dans une tâche personnalisée](../../../integration-services/extending-packages-custom-objects/task/adding-support-for-debugging-in-a-custom-task.md)  
 Décrit comment créer des cibles de points d'arrêt dans la tâche personnalisée.  
  
 [Développement d’une interface utilisateur pour une tâche personnalisée](../../../integration-services/extending-packages-custom-objects/task/developing-a-user-interface-for-a-custom-task.md)  
 Explique comment créer une interface utilisateur qui s'affiche dans le Concepteur [!INCLUDE[ssIS](../../../includes/ssis-md.md)] pour configurer des propriétés sur la tâche personnalisée.  
  
## <a name="related-sections"></a>Sections connexes  
  
### <a name="information-common-to-all-custom-objects"></a>Informations communes à tous les objets personnalisés  
 Pour obtenir les informations communes à tous les types d'objets personnalisés que vous pouvez créer dans [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)], consultez les rubriques suivantes :  
  
 [Développement d’objets personnalisés pour Integration Services](../../../integration-services/extending-packages-custom-objects/developing-custom-objects-for-integration-services.md)  
 Décrit les étapes de base pour implémenter tous les types d'objets personnalisés pour [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)].  
  
 [Persistance des objets personnalisés](../../../integration-services/extending-packages-custom-objects/persisting-custom-objects.md)  
 Décrit la persistance personnalisée et explique les situations dans lesquelles elle est nécessaire.  
  
 [Génération, déploiement et débogage d’objets personnalisés](../../../integration-services/extending-packages-custom-objects/building-deploying-and-debugging-custom-objects.md)  
 Décrit les techniques permettant de générer, signer, déployer et déboguer des objets personnalisés.  
  
### <a name="information-about-other-custom-objects"></a>Informations sur les autres objets personnalisés  
 Pour plus d'informations sur les autres types d'objets personnalisés que vous pouvez créer dans [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)], consultez les rubriques suivantes :  
  
 [Développement d’un gestionnaire de connexions personnalisé](../../../integration-services/extending-packages-custom-objects/connection-manager/developing-a-custom-connection-manager.md)  
 Explique comment programmer des gestionnaires de connexions personnalisés.  
  
 [Développement d’un module fournisseur d’informations personnalisé](../../../integration-services/extending-packages-custom-objects/log-provider/developing-a-custom-log-provider.md)  
 Explique comment programmer des modules fournisseurs d'informations personnalisés.  
  
 [Développement d’un énumérateur ForEach personnalisé](../../../integration-services/extending-packages-custom-objects/foreach-enumerator/developing-a-custom-foreach-enumerator.md)  
 Décrit comment programmer des énumérateurs personnalisés.  
  
 [Développement d’un composant de flux de données personnalisé](../../../integration-services/extending-packages-custom-objects/data-flow/developing-a-custom-data-flow-component.md)  
 Explique comment programmer des sources, des transformations et des destinations de flux de données personnalisées.  
  
## <a name="see-also"></a>Voir aussi  
 [Extension du package à l’aide de la tâche de script](../../../integration-services/extending-packages-scripting/task/extending-the-package-with-the-script-task.md)   
 [Comparaison des solutions de script et des objets personnalisés](../../../integration-services/extending-packages-scripting/comparing-scripting-solutions-and-custom-objects.md)  
  
  
