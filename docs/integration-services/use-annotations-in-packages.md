---
description: Utiliser des annotations dans les packages
title: Utiliser des annotations dans les packages | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- self-documenting packages
- adding annotations
- annotations [Integration Services]
ms.assetid: 48c8ed9a-b10d-490c-9ba7-4b77aa44e3dd
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 88a927a23696e5de7a1e079cb517078dc318b69c
ms.sourcegitcommit: cfa04a73b26312bf18d8f6296891679166e2754d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/19/2020
ms.locfileid: "92193761"
---
# <a name="use-annotations-in-packages"></a>Utiliser des annotations dans les packages

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]


  Le concepteur [!INCLUDE[ssIS](../includes/ssis-md.md)] propose des annotations que vous pouvez utiliser afin de faire en sorte que les packages s'auto-documentent et soient ainsi plus faciles à comprendre et à gérer. Vous pouvez ajouter des annotations au flux de contrôle, au flux de données et aux surfaces de dessin du gestionnaire d'événements du concepteur [!INCLUDE[ssIS](../includes/ssis-md.md)] . Les annotations peuvent contenir n'importe quel type de texte et sont utiles pour ajouter des étiquettes, des commentaires et autres informations descriptives à un package. Les annotations sont disponibles uniquement au moment de la conception. Par exemple, elles ne sont pas écrites dans les fichiers journaux.  
  
 Lorsque vous appuyez sur Entrée, le texte est renvoyé à la ligne suivante. La taille de la zone d'annotation augmente automatiquement lorsque vous ajoutez des lignes de texte supplémentaires. Les annotations de package sont conservées en texte clair dans la section CDATA du fichier de package.  
  
 Pour plus d’informations sur les modifications du format du fichier de package, consultez [Format de package SSIS](./integration-services-ssis-packages.md).  
  
 Lorsque vous enregistrez le package, le concepteur [!INCLUDE[ssIS](../includes/ssis-md.md)] enregistre les annotations dans le package.  
  
## <a name="add-an-annotation-to-a-package"></a>Ajouter une annotation à un package  
  
1.  Dans [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull-md.md)], ouvrez le projet [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] contenant le package auquel vous voulez ajouter une annotation.  
  
2.  Dans l'Explorateur de solutions, double-cliquez sur le package pour l'ouvrir.  
  
3.  Dans le concepteur [!INCLUDE[ssIS](../includes/ssis-md.md)] , cliquez avec le bouton droit n’importe où sur la surface de dessin de l’onglet **Flux de contrôle**, **Flux de données**ou **Gestionnaires d’événements** , puis cliquez sur **Ajouter une annotation**. Un bloc de texte apparaît sur la surface de dessin de l'onglet.  
  
4.  Ajoutez le texte.  
  
    > [!NOTE]  
    >  Si vous n'ajoutez pas de texte, le bloc de texte est supprimé lorsque vous cliquez en dehors.  
  
5.  Pour modifier la taille ou le format du texte de l’annotation, cliquez avec le bouton droit sur l’annotation, puis cliquez sur **Définir la police de l’annotation de texte**.  
  
6.  Pour ajouter une ligne de texte supplémentaire, appuyez sur ENTRÉE.  
  
     La taille de la zone d'annotation augmente automatiquement lorsque vous ajoutez des lignes de texte supplémentaires.  
  
7.  Pour ajouter une annotation à un groupe, cliquez avec le bouton droit sur l’annotation, puis cliquez sur **Groupe**.  
  
8.  Pour enregistrer le package mis à jour, dans le menu **Fichier** , cliquez sur **Enregistrer tout**.