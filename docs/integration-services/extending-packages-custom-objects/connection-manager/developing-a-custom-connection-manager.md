---
description: Développement d'un gestionnaire de connexions personnalisé
title: Développement d’un gestionnaire de connexions personnalisé | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
helpviewer_keywords:
- packages [Integration Services], connections
- custom connection managers [Integration Services], about custom connection managers
- connection managers [Integration Services], custom
- Integration Services packages, connection managers
- SSIS packages, connection managers
- SQL Server Integration Services packages, connection managers
- custom connection managers [Integration Services]
ms.assetid: bda0b29e-57f5-4879-b04d-1396dc56daa8
author: chugugrace
ms.author: chugu
ms.openlocfilehash: d92ee03836944fd3a98f89c86369d2c442ee6ea5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348922"
---
# <a name="developing-a-custom-connection-manager"></a>Développement d'un gestionnaire de connexions personnalisé

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] utilise des gestionnaires de connexions pour encapsuler les informations nécessaires pour se connecter à une source de données externe. [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] inclut divers gestionnaires de connexions qui prennent en charge les connexions aux sources de données les plus couramment utilisées, allant des bases de données d'entreprise aux fichiers texte et feuilles de calcul Excel. Si les gestionnaires de connexions et les sources de données externes pris en charge par [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] ne répondent pas totalement à vos besoins, vous pouvez créer un gestionnaire de connexions personnalisé.  
  
 Pour créer un gestionnaire de connexions personnalisé, vous devez créer une classe qui hérite de la classe de base <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManagerBase>, appliquer l'attribut <xref:Microsoft.SqlServer.Dts.Runtime.DtsConnectionAttribute> à votre nouvelle classe et remplacer les méthodes et propriétés importantes de la classe de base, notamment la propriété <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManagerBase.ConnectionString%2A> et la méthode <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManagerBase.AcquireConnection%2A>.  
  
> [!IMPORTANT]  
>  Une grande partie des tâches, sources et destinations intégrées à [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] utilisent uniquement des types spécifiques de gestionnaires de connexions intégrés. Avant de développer un gestionnaire de connexions personnalisé afin de l'utiliser avec des tâches et composants intégrés, vérifiez si ces composants limitent la liste des gestionnaires de connexions disponibles à un type spécifique. Si votre solution requiert un gestionnaire de connexions personnalisé, vous devrez peut-être développer également une tâche personnalisée ou une source/destination personnalisée à utiliser avec le gestionnaire de connexions.  
  
## <a name="in-this-section"></a>Dans cette section  
 Cette section décrit comment créer, configurer et coder un gestionnaire de connexions personnalisé et son interface utilisateur personnalisée facultative. Les extraits de code présentés dans cette section sont tirés de l'exemple de gestionnaire de connexions personnalisé SQL Server.  
  
 [Création d'un gestionnaire de connexions personnalisé](../../../integration-services/extending-packages-custom-objects/connection-manager/creating-a-custom-connection-manager.md)  
 Explique comment créer les classes d'un projet de gestionnaire de connexions personnalisé.  
  
 [Codage d'un gestionnaire de connexions personnalisé](../../../integration-services/extending-packages-custom-objects/connection-manager/coding-a-custom-connection-manager.md)  
 Décrit comment implémenter un gestionnaire de connexions personnalisé en remplaçant les méthodes et propriétés de la classe de base.  
  
 [Développement d'une interface utilisateur pour un gestionnaire de connexions personnalisé](../../../integration-services/extending-packages-custom-objects/connection-manager/developing-a-user-interface-for-a-custom-connection-manager.md)  
 Explique comment implémenter la classe d'interface utilisateur et le formulaire servant à configurer le gestionnaire de connexions personnalisé.  
  
## <a name="related-sections"></a>Sections connexes  
  
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
  
 [Développement d’un module fournisseur d’informations personnalisé](../../../integration-services/extending-packages-custom-objects/log-provider/developing-a-custom-log-provider.md)  
 Explique comment programmer des modules fournisseurs d'informations personnalisés.  
  
 [Développement d’un énumérateur ForEach personnalisé](../../../integration-services/extending-packages-custom-objects/foreach-enumerator/developing-a-custom-foreach-enumerator.md)  
 Décrit comment programmer des énumérateurs personnalisés.  
  
 [Développement d’un composant de flux de données personnalisé](../../../integration-services/extending-packages-custom-objects/data-flow/developing-a-custom-data-flow-component.md)  
 Explique comment programmer des sources, des transformations et des destinations de flux de données personnalisées.  
  
