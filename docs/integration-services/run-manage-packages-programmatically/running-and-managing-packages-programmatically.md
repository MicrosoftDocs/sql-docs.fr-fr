---
description: Exécution et gestion de packages par programme
title: Exécution et gestion de packages par programmation | Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
ms.assetid: 1a08c75e-ce8c-45ee-81bd-32248bbdb2b2
author: chugugrace
ms.author: chugu
ms.openlocfilehash: dbee736c5f74a2fa4258e6271e5ddce98a7fc626
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88495514"
---
# <a name="running-and-managing-packages-programmatically"></a>Exécution et gestion de packages par programme

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Si vous devez gérer et exécuter des packages [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] hors de l'environnement de développement, vous pouvez manipuler les packages par programme. Cette méthode vous offre un choix d'options :  
  
-   Charger et exécuter un package existant sans modification.  
  
-   Charger un package existant, le reconfigurer (par exemple, spécifier une source de données différente) et l'exécuter.  
  
-   Créer un package, ajouter et configurer des composants objet par objet et propriété par propriété, l'enregistrer, puis l'exécuter.  
  
 Vous pouvez charger et exécuter un package existant à partir d'une application cliente en écrivant quelques lignes de code seulement.  
  
 Cette section décrit et explique comment exécuter un package existant par programme et comment accéder à la sortie du flux d'autres applications. Une option de programmation avancée vous permet de créer par programmation un package [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] ligne par ligne, comme indiqué dans la rubrique [Génération de packages par programmation](../../integration-services/building-packages-programmatically/building-packages-programmatically.md).  
  
 Cette section présente également d'autres tâches d'administration que vous pouvez effectuer par programme pour gérer des packages stockés, des packages en cours d'exécution et des rôles de package.  
  
## <a name="running-packages-on-the-integration-services-server"></a>Exécution de packages sur le serveur Integration Services  
 Lorsque vous déployez des packages sur le serveur [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], vous pouvez exécuter les packages par programme à l'aide de l'espace de noms <xref:Microsoft.SqlServer.Management.IntegrationServices>. L'assembly Microsoft.SqlServer.Management.IntegrationServices est compilé avec le .NET Framework 3.5. Si vous générez une application .NET Framework 4.0, vous devrez peut-être ajouter la référence d'assembly directement dans votre fichier projet.  
  
 Vous pouvez également utiliser l'espace de noms pour déployer et gérer des projets [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] sur le serveur [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Pour obtenir une vue d'ensemble de l'espace de noms et des extraits de code, consultez l'entrée de blog, [Aperçu du modèle d'objet géré du catalogue SSIS](https://techcommunity.microsoft.com/t5/sql-server-integration-services/a-glimpse-of-the-ssis-catalog-managed-object-model/ba-p/387892), sur blogs.msdn.com.  
  
## <a name="in-this-section"></a>Dans cette section  
 [Présentation des différences entre l'exécution locale et l'exécution distante](../../integration-services/run-manage-packages-programmatically/understanding-the-differences-between-local-and-remote-execution.md)  
 Examine les différences essentielles entre l'exécution d'un package localement ou sur le serveur.  
  
 [Chargement et exécution d'un package local par programme](../../integration-services/run-manage-packages-programmatically/loading-and-running-a-local-package-programmatically.md)  
 Décrit comment exécuter un package existant à partir d'une application cliente sur l'ordinateur local.  
  
 [Chargement et exécution d'un package distant par programme](../../integration-services/run-manage-packages-programmatically/loading-and-running-a-remote-package-programmatically.md)  
 Décrit comment exécuter un package existant à partir d'une application cliente et s'assurer que le package s'exécute sur le serveur.  
  
 [Chargement de la sortie d'un package local](../../integration-services/run-manage-packages-programmatically/loading-the-output-of-a-local-package.md)  
 Décrit comment exécuter un package sur l'ordinateur local et charger la sortie du flux de données dans une application cliente à l'aide de la destination DataReader et de l'espace de noms DtsClient.  
  
 [Énumération des packages disponibles par programme](../../integration-services/run-manage-packages-programmatically/enumerating-available-packages-programmatically.md)  
 Décrit comment découvrir les packages disponibles gérés par le service [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
 [Gestion des packages et des dossiers par programme](../../integration-services/run-manage-packages-programmatically/managing-packages-and-folders-programmatically.md)  
 Décrit comment créer, renommer et supprimer des packages et des dossiers.  
  
 [Gestion des packages en cours d'exécution par programme](../../integration-services/run-manage-packages-programmatically/managing-running-packages-programmatically.md)  
 Décrit comment répertorier les packages en cours d'exécution, examiner leurs propriétés et arrêter un package exécuté.  
  
 [Gestion par programmation des rôles de package &#40;Service SSIS&#41;](../../integration-services/run-manage-packages-programmatically/managing-package-roles-programmatically-ssis-service.md)  
 Explique comment obtenir ou définir des informations sur les rôles attribués à un package ou un dossier.  
  
## <a name="reference"></a>Informations de référence  
 [Guide de référence des erreurs et des messages propres à Integration Services](../../integration-services/integration-services-error-and-message-reference.md)  
 Répertorie les codes d'erreur [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] prédéfinis avec leur nom symbolique et leur description.  
  
## <a name="related-sections"></a>Sections connexes  
 [Extension de packages avec des scripts](../../integration-services/extending-packages-scripting/extending-packages-with-scripting.md)  
 Explique comment étendre le flux de contrôle à l'aide de la tâche de script, et comment étendre le flux de données à l'aide du composant Script.  
  
 [Extension de packages avec des objets personnalisés](../../integration-services/extending-packages-custom-objects/extending-packages-with-custom-objects.md)  
 Explique comment créer des tâches personnalisées de programme, des composants de flux de données et d'autres objets de package à utiliser dans plusieurs packages.  
  
 [Génération de packages par programme](../../integration-services/building-packages-programmatically/building-packages-programmatically.md)  
 Explique comment créer, configurer et enregistrer des packages [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] par programme.  
  
## <a name="see-also"></a>Voir aussi  
 [SQL Server Integration Services](../../integration-services/sql-server-integration-services.md)  
  
  
