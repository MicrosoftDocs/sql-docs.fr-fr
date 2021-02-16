---
description: Développement d'une interface utilisateur pour un énumérateur ForEach personnalisé
title: Développement d’une interface utilisateur pour un énumérateur ForEach personnalisé | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
helpviewer_keywords:
- custom user interface [Integration Services], custom foreach enumerators
- custom foreach enumerators [Integration Services], developing custom user interface
ms.assetid: 8aa4aa80-c9ba-42b3-ba87-ae5ea5d3cac3
author: chugugrace
ms.author: chugu
ms.openlocfilehash: e9344b0f3192d422d61da991de6fd7c3ca71344c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350316"
---
# <a name="developing-a-user-interface-for-a-custom-foreach-enumerator"></a>Développement d'une interface utilisateur pour un énumérateur ForEach personnalisé

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  Après avoir remplacé l'implémentation des propriétés et méthodes de la classe de base afin de fournir vos fonctionnalités personnalisées, vous pouvez créer une interface utilisateur personnalisée pour votre énumérateur Foreach. Si vous ne créez pas d'interface utilisateur personnalisée, les utilisateurs peuvent configurer uniquement le nouvel énumérateur Foreach personnalisé en utilisant la fenêtre Propriétés.  
  
 Dans un projet ou assembly d'interface utilisateur personnalisée, vous créez une classe qui implémente l'objet <xref:Microsoft.SqlServer.Dts.Runtime.ForEachEnumeratorUI>. Cette classe provient de System.Windows.Forms.UserControl, qui est généralement utilisé pour créer un contrôle composite afin d’héberger d’autres contrôles Windows Forms. Le contrôle que vous créez s’affiche sous l’onglet **Collection** de l’**Éditeur de boucle Foreach**, dans la zone **Configuration de l’énumérateur**.  
  
> [!IMPORTANT]  
>  Après avoir signé et généré votre interface utilisateur personnalisée, puis l’avoir installée dans le Global Assembly Cache comme décrit dans [Génération, déploiement et débogage d’objets personnalisés](../../../integration-services/extending-packages-custom-objects/building-deploying-and-debugging-custom-objects.md), n’oubliez pas de fournir le nom complet de cette classe dans la propriété <xref:Microsoft.SqlServer.Dts.Runtime.DtsForEachEnumeratorAttribute.UITypeName%2A> de la classe<xref:Microsoft.SqlServer.Dts.Runtime.DtsForEachEnumeratorAttribute>.  
  
## <a name="coding-the-user-interface-control-class"></a>Codage de la classe de contrôle de l'interface utilisateur  
  
### <a name="initializing-the-user-interface"></a>Initialisation de l'interface utilisateur  
 Vous remplacez la méthode <xref:Microsoft.SqlServer.Dts.Runtime.ForEachEnumeratorUI.Initialize%2A> pour mettre en cache des références à l'objet hôte et aux collections de gestionnaires de connexion et variables définies dans le package.  
  
### <a name="setting-properties-on-the-user-interface-control"></a>Définition de propriétés sur le contrôle de l'interface utilisateur  
 La classe UserControl, de laquelle provient la classe de l’interface utilisateur, est destinée à être utilisée comme contrôle composite afin d’héberger d’autres contrôles Windows Forms. Parce que cette classe héberge d'autres contrôles, vous pouvez concevoir votre interface utilisateur personnalisée en faisant glisser des contrôles, en les réorganisant, en définissant leurs propriétés et en répondant au moment de l'exécution à leurs événements comme dans toute application Windows Forms.  
  
### <a name="saving-settings"></a>Enregistrement des paramètres  
 Vous remplacez la méthode <xref:Microsoft.SqlServer.Dts.Runtime.ForEachEnumeratorUI.SaveSettings%2A> pour copier les valeurs sélectionnées par l'utilisateur depuis les contrôles vers les propriétés de l'énumérateur lorsque l'utilisateur ferme l'éditeur.  
  
## <a name="see-also"></a>Voir aussi  
 [Création d’un énumérateur Foreach personnalisé](../../../integration-services/extending-packages-custom-objects/foreach-enumerator/creating-a-custom-foreach-enumerator.md)   
 [Codage d'un énumérateur Foreach personnalisé](../../../integration-services/extending-packages-custom-objects/foreach-enumerator/coding-a-custom-foreach-enumerator.md)  
  
  
