---
description: Déclenchement d'événements dans le composant Script
title: Déclenchement d’événements dans le composant Script | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
helpviewer_keywords:
- Script component [Integration Services], raising events
ms.assetid: bb389073-e1d0-4794-8d29-c8b293b6a5e3
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 7881dcb05fb615b4045efb505a164496e7a8ef5f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354951"
---
# <a name="raising-events-in-the-script-component"></a>Déclenchement d'événements dans le composant Script

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  Les événements offrent un moyen de signaler des erreurs, des avertissements et d'autres informations, telles que la progression ou l'état d'une tâche, au package conteneur. Le package fournit des gestionnaires d'événements pour gérer les notifications d'événements. Le composant Script peut déclencher des événements en appelant des méthodes sur la propriété <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptComponent.ComponentMetaData%2A> de la classe **ScriptMain**. Pour plus d’informations sur la manière dont les packages [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] gèrent les événements, consultez [Gestionnaires d’événements Integration Services &#40;SSIS&#41;](../../../integration-services/integration-services-ssis-event-handlers.md).  
  
 Les événements peuvent être journalisés dans tout module fournisseur d'informations activé dans le package. Les modules fournisseurs d'informations stockent des informations à propos des événements dans une banque de données. Le composant Script peut également utiliser la méthode <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptComponent.Log%2A> pour journaliser des informations dans un module fournisseur d'informations sans déclencher d'événement. Pour plus d'informations sur la manière d'utiliser la méthode <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptComponent.Log%2A>, consultez la section suivante.  
  
 Pour déclencher un événement, la tâche de script appelle l'une des méthodes suivantes de l'interface <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100> exposée par la propriété <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptComponent.ComponentMetaData%2A> :  
  
|Événement|Description|  
|-----------|-----------------|  
|<xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.FireCustomEvent%2A>|Déclenche un événement personnalisé défini par l'utilisateur dans le package.|  
|<xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.FireError%2A>|Informe le package d'une condition d'erreur.|  
|<xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.FireInformation%2A>|Fournit des informations à l'utilisateur.|  
|<xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.FireProgress%2A>|Informe le package de la progression du composant.|  
|<xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.FireWarning%2A>|Informe le package que le composant est dans un état qui garantit la notification de l'utilisateur, mais qui n'est pas une condition d'erreur.|  
  
 Voici un exemple simple de génération d'un événement d'erreur :  
  
 `Dim myMetadata as IDTSComponentMetaData100`  
  
 `myMetaData = Me.ComponentMetaData`  
  
 `myMetaData.FireError(...)`  
  
## <a name="see-also"></a>Voir aussi  
 [Gestionnaires d’événements Integration Services &#40;SSIS&#41](../../../integration-services/integration-services-ssis-event-handlers.md)   
 [Ajouter un gestionnaire d’événements à un package](../../integration-services-ssis-event-handlers.md)  
  
