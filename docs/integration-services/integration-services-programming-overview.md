---
description: Vue d'ensemble de la programmation Integration Services
title: Vue d’ensemble de la programmation Integration Services | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
helpviewer_keywords:
- Integration Services, programming
- architecture [Integration Services]
- assemblies [Integration Services]
- SSIS, programming
- SQL Server Integration Services, programming
- run-time engine [Integration Services]
- packages [Integration Services], programming
- data flow engine [Integration Services]
- languages [Integration Services]
ms.assetid: 262babc6-eea5-4609-bc65-07d64cbcfee9
author: chugugrace
ms.author: chugu
ms.openlocfilehash: ae083f7e594025449da685ce5f18ed777f88a8a1
ms.sourcegitcommit: 868c60aa3a76569faedd9b53187e6b3be4997cc9
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/08/2021
ms.locfileid: "99835105"
---
# <a name="integration-services-programming-overview"></a>Vue d'ensemble de la programmation Integration Services

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]


  [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] a une architecture qui sépare le déplacement et la transformation de données du flux de contrôle et de la gestion de packages. Cette architecture se définit par deux moteurs distincts qui peuvent être automatisés et étendus lors de la programmation [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)]. Le moteur d'exécution implémente l'infrastructure de flux de contrôle et de gestion de packages qui permet aux développeurs de contrôler le flux d'exécution et de définir des options pour la journalisation, les gestionnaires d'événements et les variables. Le moteur de flux de données est un moteur spécialisé, hautement performant, exclusivement dédié à l'extraction, la transformation et le chargement de données. La programmation [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] s'effectue à partir de ces deux moteurs.  
  
 L'image suivante représente l'architecture d'[!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)].  
  
 ![Architecture Integration Services](../integration-services/media/mw-dts-01.gif "Architecture Integration Services")  
  
## <a name="integration-services-run-time-engine"></a>Moteur d'exécution Integration Services  
 Le moteur d'exécution [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] contrôle la gestion et l'exécution de packages, en implémentant l'infrastructure qui active l'ordre d'exécution, la journalisation, les variables et la gestion d'événements. La programmation du moteur d'exécution [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] permet aux développeurs d'automatiser la création, la configuration et l'exécution de packages et de créer des tâches personnalisées et d'autres extensions.  
  
 Pour plus d’informations, consultez [Extension du package à l’aide de la tâche de script](../integration-services/extending-packages-scripting/task/extending-the-package-with-the-script-task.md), [Développement d’une tâche personnalisée](../integration-services/extending-packages-custom-objects/task/developing-a-custom-task.md) et [Génération de packages par programmation](../integration-services/building-packages-programmatically/building-packages-programmatically.md).  
  
## <a name="integration-services-data-flow-engine"></a>Moteur de flux de données Integration Services  
 Le moteur de flux de données gère la tâche de flux de données, qui est une tâche spécialisée, hautement performante, destinée à déplacer et transformer les données provenant de sources disparates. Contrairement à d'autres tâches, la tâche de flux de données contient des objets supplémentaires, appelés composants de flux de données, qui peuvent être des sources, des transformations ou des destinations. Ces composants constituent les principaux éléments en mouvement de la tâche. Ils définissent le déplacement et la transformation des données. La programmation du moteur de flux de données permet aux développeurs d'automatiser la création et la configuration des composants dans une tâche de flux de données et de créer des composants personnalisés.  
  
 Pour plus d’informations, consultez [Extension du flux de données avec le composant Script](../integration-services/extending-packages-scripting/data-flow-script-component/extending-the-data-flow-with-the-script-component.md), [Développement d’un composant de flux de données personnalisé](../integration-services/extending-packages-custom-objects/data-flow/developing-a-custom-data-flow-component.md) et [Génération de packages par programmation](../integration-services/building-packages-programmatically/building-packages-programmatically.md).  
  
## <a name="supported-languages"></a>Langues prises en charge  
 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] prend entièrement en charge le [!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../includes/dnprdnshort-md.md)]. Les développeurs peuvent ainsi programmer [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] dans les langages compatibles .NET de leur choix. Bien qu'ils soient écrits en code natif, le moteur d'exécution et le moteur de flux de données sont accessibles par le biais d'un modèle objet entièrement managé.  
  
 Vous pouvez programmer des packages, des tâches personnalisées et des composants [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] dans [!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[vsprvs](../includes/vsprvs-md.md)] ou dans un autre code ou éditeur de texte. [!INCLUDE[vsprvs](../includes/vsprvs-md.md)] offre de nombreux outils et fonctionnalités au développeur pour simplifier et accélérer les cycles itératifs du codage, du débogage et du test. [!INCLUDE[vsprvs](../includes/vsprvs-md.md)] facilite également le déploiement. Toutefois, vous n'avez pas besoin de [!INCLUDE[vsprvs](../includes/vsprvs-md.md)] pour compiler et générer des projets de code [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)]. Le Kit de développement [!INCLUDE[dnprdnshort](../includes/dnprdnshort-md.md)] SDK inclut les compilateurs et les outils connexes [!INCLUDE[vbprvb](../includes/vbprvb-md.md)] et [!INCLUDE[csprcs](../includes/csprcs-md.md)].  
  
> [!IMPORTANT]  
>  Le [!INCLUDE[dnprdnshort](../includes/dnprdnshort-md.md)] est installé par défaut avec [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], mais pas le Kit de développement [!INCLUDE[dnprdnshort](../includes/dnprdnshort-md.md)] SDK. Les liens vers les rubriques relatives au Kit de développement figurant dans cette section ne fonctionnent que si le Kit de développement est installé sur l'ordinateur et que la documentation qui lui est propre figure dans la documentation en ligne. Après avoir installé le SDK [!INCLUDE[dnprdnshort](../includes/dnprdnshort-md.md)], vous pouvez ajouter la documentation le concernant à la documentation en ligne et à la table des matières en suivant les instructions figurant dans [Ajouter ou supprimer la documentation du produit SQL Server](https://www.microsoft.com/download/details.aspx?id=42557).  
  
 La tâche de script et le composant Script [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] utilisent [!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[vsprvs](../includes/vsprvs-md.md)] Tools for Applications (VSTA) comme environnement de script incorporé. VSTA prend en charge [!INCLUDE[msCoName](../includes/msconame-md.md)] Visual Basic et [!INCLUDE[msCoName](../includes/msconame-md.md)] Visual C#.  
  
> [!NOTE]  
>  Les interfaces de programmation d'applications [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] sont incompatibles avec les langages de script COM, tels que VBScript.  
  
## <a name="locating-assemblies"></a>Recherche d'assemblys  
 Dans [!INCLUDE[ssSQL19](../includes/sssql19-md.md)], les assemblys [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] ont été mis à niveau vers le .NET 4.0. Il existe un Global Assembly Cache distinct pour .NET 4, situé dans *\<drive>* :\Windows\Microsoft.NET\assembly. Vous trouverez tous les assemblys [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] sous ce chemin d'accès, en général dans le dossier GAC_MSIL.  
  
 Comme dans les versions antérieures de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], les principaux fichiers .dll d’extensibilité [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] se trouvent également dans *\<drive>* :\Program files\Microsoft SQL Server\100\SDK\Assemblies.  
  
## <a name="commonly-used-assemblies"></a>Assemblys couramment utilisés  
 Le tableau suivant répertorie les assemblys fréquemment utilisés lors de la programmation [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] à l'aide du [!INCLUDE[dnprdnshort](../includes/dnprdnshort-md.md)].  
  
|Assembly|Description|  
|--------------|-----------------|  
|Microsoft.SqlServer.ManagedDTS.dll|Contient le moteur d'exécution managé.|  
|Microsoft.SqlServer.RuntimeWrapper.dll|Contient l'assembly PIA (Primary Interop Assembly), ou wrapper, du moteur d'exécution natif.|  
|Microsoft.SqlServer.PipelineHost.dll|Contient le moteur de flux de données managé.|  
|Microsoft.SqlServer.PipelineWrapper.dll|Contient l'assembly PIA (Primary Interop Assembly), ou wrapper, du moteur de flux de données natif.|  
  
  
