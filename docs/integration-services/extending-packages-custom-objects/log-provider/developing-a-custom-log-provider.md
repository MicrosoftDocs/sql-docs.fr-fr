---
description: Développement d'un module fournisseur d'informations personnalisé
title: Développement d’un module fournisseur d’informations personnalisé | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
helpviewer_keywords:
- SSIS packages, log providers
- custom log providers [Integration Services]
- SQL Server Integration Services packages, log providers
- log providers [Integration Services]
- packages [Integration Services], logs
- Integration Services packages, log providers
ms.assetid: 3f715b95-7074-4f5c-8ae2-246998052e78
author: chugugrace
ms.author: chugu
ms.openlocfilehash: ed384b5c9c83a1233167ff3c1caeaf3b37fb8070
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354340"
---
# <a name="developing-a-custom-log-provider"></a>Développement d'un module fournisseur d'informations personnalisé

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] possède des fonctions de journalisation étendues qui permettent de capturer les événements qui se produisent pendant l'exécution de package. [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] inclut divers modules fournisseurs d’informations qui permettent de créer et de stocker des journaux dans différents formats, tels que XML, texte et base de données ou dans le journal des événements Windows. Si les modules fournisseurs d'informations et les formats de sortie fournis ne répondent pas totalement à vos besoin, vous pouvez créer un module fournisseur d'informations personnalisé.  
  
 Pour créer un module fournisseur d'informations personnalisé, vous devez créer une classe qui hérite de la classe de base <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase>, appliquer l'attribut <xref:Microsoft.SqlServer.Dts.Runtime.DtsLogProviderAttribute> à la nouvelle classe et substituer les méthodes et propriétés importantes de la classe de base, notamment la propriété <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.ConfigString%2A> et la méthode <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.Log%2A>.  
  
## <a name="in-this-section"></a>Dans cette section  
 Cette section explique comment créer, configurer et coder un module fournisseur d'informations personnalisé.  
  
 [Création d’un module fournisseur d’informations personnalisé](../../../integration-services/extending-packages-custom-objects/log-provider/creating-a-custom-log-provider.md)  
 Explique comment créer les classes d'un projet de module fournisseur d'informations personnalisé.  
  
 [Codage d'un module fournisseur d'informations personnalisé](../../../integration-services/extending-packages-custom-objects/log-provider/coding-a-custom-log-provider.md)  
 Décrit comment implémenter un module fournisseur d'informations personnalisé en substituant les méthodes et propriétés de la classe de base.  
  
 [Développement d’une interface utilisateur pour un module fournisseur d’informations personnalisé](../../../integration-services/extending-packages-custom-objects/log-provider/developing-a-user-interface-for-a-custom-log-provider.md)  
 Les interfaces utilisateur personnalisées des modules fournisseurs d’informations personnalisés ne sont pas prises en charge dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)].  
  
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
  
 [Développement d’un énumérateur ForEach personnalisé](../../../integration-services/extending-packages-custom-objects/foreach-enumerator/developing-a-custom-foreach-enumerator.md)  
 Décrit comment programmer des énumérateurs personnalisés.  
  
 [Développement d’un composant de flux de données personnalisé](../../../integration-services/extending-packages-custom-objects/data-flow/developing-a-custom-data-flow-component.md)  
 Explique comment programmer des sources, des transformations et des destinations de flux de données personnalisées.  
  
