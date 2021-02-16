---
description: Création d'un module fournisseur d'informations personnalisé
title: Création d’un module fournisseur d’informations personnalisé | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
dev_langs:
- VB
- CSharp
helpviewer_keywords:
- custom log providers [Integration Services], creating
ms.assetid: fc20af96-9eb8-4195-8d3f-8a4d7c753f24
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 1f6de779a67df0bdbd81086c1b34992d0fe896ae
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354374"
---
# <a name="creating-a-custom-log-provider"></a>Création d'un module fournisseur d'informations personnalisé

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  L'environnement d'exécution [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] possède des fonctionnalités de journalisation complètes. Un journal vous permet de capturer des événements qui se produisent pendant l'exécution d'un package. [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] inclut divers modules fournisseurs d'informations qui permettent de créer et stocker des journaux dans plusieurs formats, notamment XML, texte et base de données ou dans le journal des événements Windows. Si l'un de ces fournisseurs ou formats de sortie ne répond pas à vos besoins, vous pouvez créer un module fournisseur d'informations personnalisé.  
  
 Les étapes à suivre pour créer un module fournisseur d'informations personnalisé sont similaires à celles qui permettent de créer tout autre objet personnalisé pour [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] :  
  
-   Créer une classe qui hérite de la classe de base. Pour un module fournisseur d'informations, la classe de base est <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase>.  
  
-   Appliquer l'attribut qui identifie le type d'objet auprès de la classe. Pour un module fournisseur d'informations, l'attribut est <xref:Microsoft.SqlServer.Dts.Runtime.DtsLogProviderAttribute>.  
  
-   Substituer l'implémentation des méthodes et des propriétés de la classe de base. Pour un module fournisseur d'informations, celles-ci incluent la propriété <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.ConfigString%2A> et les méthodes <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.OpenLog%2A>, <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.Log%2A> et <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.CloseLog%2A>.  
  
-   Les interfaces utilisateur personnalisées des modules fournisseurs d’informations personnalisés ne sont pas implémentées dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)].  
  
## <a name="getting-started-with-a-custom-log-provider"></a>Mise en route d'un module fournisseur d'informations personnalisé  
  
### <a name="creating-projects-and-classes"></a>Création de projets et de classes  
 Puisque tous les modules fournisseurs d'informations managés dérivent de la classe de base <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase>, la première étape de création d'un module fournisseur d'informations personnalisé consiste à créer un projet Bibliothèque de classes dans votre langage de programmation managé par défaut, puis à créer une classe qui hérite de la classe de base. Dans cette classe dérivée, vous devez substituer les méthodes et les propriétés de la classe de base pour implémenter vos fonctionnalités personnalisées.  
  
 Configurez le projet pour signer l'assembly qui sera généré avec un fichier de clé de nom fort.  
  
> [!NOTE]  
>  De nombreux modules fournisseurs d’informations [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] ont une interface utilisateur personnalisée qui implémente <xref:Microsoft.SqlServer.Dts.Runtime.Design.IDtsLogProviderUI> et remplace la zone de texte **Configuration** dans la boîte de dialogue **Configurer les journaux SSIS** par la liste déroulante filtrée des gestionnaires de connexions disponibles. Toutefois, les interfaces utilisateur personnalisées des modules fournisseurs d'informations personnalisés ne sont pas implémentées dans [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)].  
  
### <a name="applying-the-dtslogprovider-attribute"></a>Application de l'attribut DtsLogProvider  
 Appliquez l'attribut <xref:Microsoft.SqlServer.Dts.Runtime.DtsLogProviderAttribute> à la classe que vous avez créée pour l'identifier en tant que module fournisseur d'informations. Cet attribut fournit des informations de conception, telles que le nom et la description du module fournisseur d'informations. Les propriétés **DisplayName** et **Description** de l’attribut correspondent aux colonnes **Nom** et **Description** qui apparaissent dans l’éditeur **Configurer les journaux SSIS**, qui s’affiche lors de la configuration de la journalisation d’un package dans [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)].  
  
> [!IMPORTANT]  
>  La propriété <xref:Microsoft.SqlServer.Dts.Runtime.DtsLogProviderAttribute.LogProviderType%2A> de l'attribut n'est pas utilisée. Toutefois, vous devez entrer une valeur pour cette propriété, sans quoi le module fournisseur d'informations personnalisé n'apparaîtra pas dans la liste des modules fournisseurs d'informations disponibles.  
  
> [!NOTE]  
>  Étant donné que les interfaces utilisateur personnalisées des modules fournisseurs d'informations personnalisés ne sont pas implémentées dans [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)], la spécification d'une valeur pour la propriété <xref:Microsoft.SqlServer.Dts.Runtime.DtsLogProviderAttribute.UITypeName%2A> de l'objet <xref:Microsoft.SqlServer.Dts.Runtime.DtsLogProviderAttribute> n'a aucun effet.  
  
```vb  
<DtsLogProvider(DisplayName:="MyLogProvider", Description:="A simple log provider.", LogProviderType:="Custom")> _  
Public Class MyLogProvider  
     Inherits LogProviderBase  
    ' TODO: Override the base class methods.  
End Class  
```  
  
```csharp  
[DtsLogProvider(DisplayName="MyLogProvider", Description="A simple log provider.", LogProviderType="Custom")]  
public class MyLogProvider : LogProviderBase  
{  
    // TODO: Override the base class methods.  
}  
```  
  
## <a name="building-deploying-and-debugging-a-custom-log-provider"></a>Génération, déploiement et débogage d'un module fournisseur d'informations personnalisé  
 Les étapes permettant de générer, déployer et déboguer un module fournisseur d'informations personnalisé dans [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] sont très similaires à celles requises pour les autres types d'objets personnalisés. Pour plus d’informations, consultez [Génération, déploiement et débogage d’objets personnalisés](../../../integration-services/extending-packages-custom-objects/building-deploying-and-debugging-custom-objects.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Codage d’un module fournisseur d’informations personnalisé](../../../integration-services/extending-packages-custom-objects/log-provider/coding-a-custom-log-provider.md)   
 [Développement d’une interface utilisateur pour un module fournisseur d’informations personnalisé](../../../integration-services/extending-packages-custom-objects/log-provider/developing-a-user-interface-for-a-custom-log-provider.md)  
  
  
