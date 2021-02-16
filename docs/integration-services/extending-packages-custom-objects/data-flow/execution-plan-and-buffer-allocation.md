---
description: Plan d'exécution et allocation de mémoire tampon
title: Plan d’exécution et allocation de mémoire tampon | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
dev_langs:
- VB
- CSharp
helpviewer_keywords:
- custom data flow components [Integration Services], buffer allocations
- custom data flow components [Integration Services], execution plans
- buffer allocations [Integration Services]
- data flow components [Integration Services], buffer allocations
- data flow components [Integration Services], execution plans
- execution plans [Integration Services]
ms.assetid: 679d9ff0-641e-47c3-abb8-d1a7dcb279dd
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 5b9827f4943e21b117353af4aaa4ea825cef19d5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343697"
---
# <a name="execution-plan-and-buffer-allocation"></a>Plan d'exécution et allocation de mémoire tampon

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  Avant l'exécution, la tâche de flux de données examine ses composants et génère un plan d'exécution pour chaque séquence de composants. Cette section fournit des détails sur le plan d'exécution et son mode d'affichage, ainsi que sur l'allocation de mémoires tampons d'entrée et de sortie en fonction du plan d'exécution.  
  
## <a name="understanding-the-execution-plan"></a>Fonctionnement du plan d'exécution  
 Un plan d'exécution contient des threads sources et des threads de travail. Chaque thread contient des listes de travaux qui spécifient des listes de travaux de sortie pour les threads sources ou des listes de travaux d'entrée et de sortie pour les threads de travail. Les threads sources d’un plan d’exécution, qui représentent les composants sources du flux de données, sont identifiés dans le plan d’exécution par *SourceThreadn*, où *n* correspond au numéro (commençant à zéro) du thread source.  
  
 Chaque thread source crée une mémoire tampon, définit un écouteur et appelle la méthode <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PrimeOutput%2A> sur le composant source. C'est de cet emplacement que démarre l'exécution et que proviennent les données, pendant que le composant source commence à ajouter des lignes aux mémoires tampons de sortie que lui fournit la tâche de flux de données. Une fois que les threads sources sont exécutés, la charge de travail est répartie entre les threads de travail.  
  
 Un thread de travail, qui peut contenir des listes de travaux d’entrée et de sortie, est identifié dans le plan d’exécution par *WorkThreadn*, où *n* correspond au numéro (commençant à zéro) du thread de travail. Ces threads contiennent des listes de travaux de sortie lorsque le graphique contient un composant à sorties asynchrones.  
  
 L'exemple de plan d'exécution suivant représente un flux de données qui contient un composant source connecté à une transformation à sortie asynchrone connectée à un composant de destination. Dans cet exemple, WorkThread0 contient une liste des travaux de sortie car le composant de transformation possède une sortie asynchrone.  
  
```  
SourceThread0   
    Influences: 72 158   
    Output Work List   
        CreatePrimeBuffer of type 1 for output id 10   
        SetBufferListener: "WorkThread0" for input ID 73   
        CallPrimeOutput on component "OLE DB Source" (1)   
    End Output Work List   
    This thread drives 0 distributors   
End SourceThread0   
WorkThread0   
    Influences: 72 158   
    Input Work list, input ID 73   
        CallProcessInput on input ID 73 on component "Sort" (72) for view type 2   
    End Input Work list for input 73   
    Output Work List   
        CreatePrimeBuffer of type 3 for output id 74   
        SetBufferListener: "WorkThread1" for input ID 171with internal handoff   
        CallPrimeOutput on component "Sort" (72)   
    End Output Work List   
    This thread drives 0 distributors   
End WorkThread0   
WorkThread1   
    Influences: 158   
    Input Work list, input ID 171  
        CallProcessInput on input ID 171 on component "OLE DB Destination" (158) for view type 4  
    End Input Work list for input 171   
    Output Work List   
    End Output Work List   
    This thread drives 0 distributors   
End WorkThread1  
```  
  
> [!NOTE]  
>  Le plan d’exécution est généré chaque fois qu’un package est exécuté. Il peut être capturé en ajoutant un module fournisseur d’informations au package, en activant la journalisation et en sélectionnant l’événement **PipelineExecutionPlan**.  
  
## <a name="understanding-buffer-allocation"></a>Fonctionnement de l'allocation de mémoire tampon  
 En fonction du plan d'exécution, la tâche de flux de données crée des mémoires tampons qui contiennent les colonnes définies dans les sorties des composants de flux de données. La mémoire tampon est réutilisée tandis que les données transitent par la séquence de composants, jusqu'à ce qu'un composant à sorties asynchrones soit trouvé. Puis, une nouvelle mémoire tampon est créée, qui contient les colonnes de sortie de la sortie asynchrone et les colonnes de sortie des composants en aval.  
  
 Pendant l'exécution, les composants ont accès à la mémoire tampon dans le thread source ou de travail actuel. Il peut s'agir d'une mémoire tampon d'entrée, fournie par la méthode <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A>, ou d'une mémoire tampon de sortie, fournie par la méthode <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PrimeOutput%2A>. La propriété <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.Mode%2A> de <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer> identifie également chaque mémoire tampon en tant que mémoire tampon d'entrée ou de sortie.  
  
 Les composants de transformation à sorties asynchrones reçoivent la mémoire tampon d'entrée existante via la méthode <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A> et la nouvelle mémoire tampon de sortie via la méthode <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PrimeOutput%2A>. Les composants de transformation à sorties asynchrones sont les seuls types de composants de flux de données qui reçoivent à la fois une mémoire tampon d'entrée et de sortie.  
  
 Comme la mémoire tampon fournie à un composant est susceptible de contenir un nombre de colonnes supérieur à celui que possède le composant dans ses collections de colonnes d’entrée et de sortie, les développeurs de composants peuvent appeler la méthode <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSBufferManager100.FindColumnByLineageID%2A> pour rechercher une colonne dans la mémoire tampon en spécifiant son **LineageID**.  
  
