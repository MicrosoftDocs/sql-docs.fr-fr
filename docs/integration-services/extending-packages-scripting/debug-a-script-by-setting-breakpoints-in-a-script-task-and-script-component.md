---
description: Déboguer un script en définissant des points d'arrêt dans une tâche de script et un composant de script
title: Déboguer un script en définissant des points d’arrêt dans une tâche de script et un composant de script| Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
helpviewer_keywords:
- breakpoints [Integration Services]
- scripts [Integration Services], breakpoints
ms.assetid: 6c03464f-3f7d-4882-b7f8-8e396f8e2944
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 01ac4ea26ca9cd21ba82341540df5a64c47d449b
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88425381"
---
# <a name="debug-a-script-by-setting-breakpoints-in-a-script-task-and-script-component"></a>Déboguer un script en définissant des points d'arrêt dans une tâche de script et un composant de script

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Cette section décrit la procédure de définition des points d'arrêt dans les scripts utilisés dans la tâche de script et le composant de script.  
  
 Après avoir défini les points d’arrêt dans le script, la boîte de dialogue **Définir les points d’arrêt - \<object name>** donne la liste des points d’arrêt avec les points d’arrêt intégrés.  
  
> [!IMPORTANT]  
>  Dans certains cas, les points d'arrêt de la tâche de script et le composant de script sont ignorés. Pour plus d’informations, consultez la section **Débogage de la tâche de script** dans [Codage et débogage de la tâche de script](../../integration-services/extending-packages-scripting/task/coding-and-debugging-the-script-task.md) et la section **Débogage du composant Script** dans [Codage et débogage du composant Script](../../integration-services/extending-packages-scripting/data-flow-script-component/coding-and-debugging-the-script-component.md).  
  
### <a name="to-set-a-breakpoint-in-script"></a>Pour définir un point d'arrêt dans un script  
  
1.  Dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], ouvrez le projet [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] contenant le package souhaité.  
  
2.  Double-cliquez sur le package qui contient le script dans lequel vous souhaitez définir des points d'arrêt.  
  
3.  Pour ouvrir la tâche de script, cliquez sur l’onglet **Flux de contrôle**, puis double-cliquez sur la tâche de script.  
  
4.  Pour ouvrir le composant de script, cliquez sur l’onglet **Flux de données**, puis double-cliquez sur le composant de script.  
  
5.  Cliquez sur **Script**, puis sur **Modifier le script**.  
  
6.  Dans [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] Tools for Applications (VSTA), recherchez la ligne de script sur laquelle vous voulez définir un point d’arrêt, cliquez dessus avec le bouton droit, pointez sur **Point d’arrêt**, puis sélectionnez **Insérer un point d’arrêt**.  
  
     L'icône du point d'arrêt apparaît sur la ligne de code.  
  
7.  Dans le menu **Fichier** , cliquez sur **Quitter**.  
  
8.  Cliquez sur **OK**.  
  
9. Pour enregistrer le package, cliquez sur **Enregistrer les éléments sélectionnés** dans le menu **Fichier** .  
  
  
