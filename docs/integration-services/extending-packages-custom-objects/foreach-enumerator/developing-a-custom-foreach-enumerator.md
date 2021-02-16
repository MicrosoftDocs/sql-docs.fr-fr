---
description: Développement d'un énumérateur ForEach personnalisé
title: Développement d’un énumérateur ForEach personnalisé | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
helpviewer_keywords:
- custom foreach enumerators [Integration Services]
- custom foreach enumerators [Integration Services], about custom foreach enumerators
- foreach enumerators [Integration Services]
ms.assetid: bffe26e0-1b9a-47ad-bae6-6b708cb4cf4f
author: chugugrace
ms.author: chugu
ms.openlocfilehash: f86fb73d17a36a0ace8accd4ca494cfc449f67af
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350272"
---
# <a name="developing-a-custom-foreach-enumerator"></a>Développement d'un énumérateur ForEach personnalisé

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] utilise des énumérateurs Foreach pour parcourir les éléments d'une collection et effectuer les mêmes tâches pour chaque élément. [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] inclut divers énumérateurs Foreach qui prennent en charge les collections les plus couramment utilisées, telles que tous les fichiers inclus dans un dossier, toutes les tables incluses dans une base de données ou tous les éléments d'une liste stockée dans une variable de package. Si les énumérateurs foreach et les collections fournies ne répondent pas totalement à vos besoins, vous pouvez créer un énumérateur foreach personnalisé.  
  
 Pour créer un énumérateur foreach personnalisé, vous devez créer une classe qui hérite de la classe de base <xref:Microsoft.SqlServer.Dts.Runtime.ForEachEnumerator>, appliquer l'attribut <xref:Microsoft.SqlServer.Dts.Runtime.DtsForEachEnumeratorAttribute> à votre nouvelle classe et remplacer les méthodes et propriétés importantes de la classe de base, notamment la méthode <xref:Microsoft.SqlServer.Dts.Runtime.ForEachEnumerator.GetEnumerator%2A>.  
  
## <a name="in-this-section"></a>Dans cette section  
 Cette section explique comment créer, configurer et coder un énumérateur foreach personnalisé et son interface utilisateur personnalisée.  
  
 [Création d'un énumérateur Foreach personnalisé](../../../integration-services/extending-packages-custom-objects/foreach-enumerator/creating-a-custom-foreach-enumerator.md)  
 Explique comment créer les classes d'un projet d'énumérateur foreach personnalisé.  
  
 [Codage d'un énumérateur Foreach personnalisé](../../../integration-services/extending-packages-custom-objects/foreach-enumerator/coding-a-custom-foreach-enumerator.md)  
 Décrit comment implémenter un énumérateur foreach personnalisé en remplaçant les méthodes et propriétés de la classe de base.  
  
 [Développement d'une interface utilisateur pour un énumérateur ForEach personnalisé](../../../integration-services/extending-packages-custom-objects/foreach-enumerator/developing-a-user-interface-for-a-custom-foreach-enumerator.md)  
 Explique comment implémenter la classe d'interface utilisateur et le formulaire servant à configurer l'énumérateur foreach personnalisé.  
  
## <a name="related-topics"></a>Rubriques connexes  
  
### <a name="information-common-to-all-custom-objects"></a>Informations communes à tous les objets personnalisés  
 Pour obtenir les informations communes à tous les types d'objets personnalisés que vous pouvez créer dans [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)], consultez les rubriques suivantes :  
  
 [Développement d’objets personnalisés pour Integration Services](../../../integration-services/extending-packages-custom-objects/developing-custom-objects-for-integration-services.md)  
 Décrit les étapes de base permettant d’implémenter tous les types d’objets personnalisés pour [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)].  
  
 [Persistance des objets personnalisés](../../../integration-services/extending-packages-custom-objects/persisting-custom-objects.md)  
 Décrit la persistance personnalisée et explique les situations dans lesquelles elle est nécessaire.  
  
 [Génération, déploiement et débogage d’objets personnalisés](../../../integration-services/extending-packages-custom-objects/building-deploying-and-debugging-custom-objects.md)  
 Décrit les techniques permettant de générer, signer, déployer et déboguer des objets personnalisés.  
  
### <a name="information-about-other-custom-objects"></a>Informations sur les autres objets personnalisés  
 Pour plus d’informations sur les autres types d’objets personnalisés que vous pouvez créer dans [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)], consultez les rubriques suivantes :  
  
 [Développement d'une tâche personnalisée](../../../integration-services/extending-packages-custom-objects/task/developing-a-custom-task.md)  
 Explique comment programmer des tâches personnalisées.  
  
 [Développement d’un gestionnaire de connexions personnalisé](../../../integration-services/extending-packages-custom-objects/connection-manager/developing-a-custom-connection-manager.md)  
 Explique comment programmer des gestionnaires de connexions personnalisés.  
  
 [Développement d’un module fournisseur d’informations personnalisé](../../../integration-services/extending-packages-custom-objects/log-provider/developing-a-custom-log-provider.md)  
 Explique comment programmer des modules fournisseurs d'informations personnalisés.  
  
 [Développement d’un composant de flux de données personnalisé](../../../integration-services/extending-packages-custom-objects/data-flow/developing-a-custom-data-flow-component.md)  
 Explique comment programmer des sources, des transformations et des destinations de flux de données personnalisées.  
 
