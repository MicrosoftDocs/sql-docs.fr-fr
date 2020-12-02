---
description: 'Leçon 1-2 : Création du projet de déploiement'
title: 'Étape 2 : Création du projet de déploiement | Microsoft Docs'
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: tutorial
ms.assetid: 59990fe2-7036-4e9c-8efc-6ece9e66eda7
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 025e601dfb6f062246d1edcb1799c1daf2b69886
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/26/2020
ms.locfileid: "88449677"
---
# <a name="lesson-1-2---creating-the-deployment-project"></a>Leçon 1-2 : Création du projet de déploiement

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]


Dans [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)], l'unité déployable est un projet [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] . Avant de pouvoir déployer les packages, vous devez créer un nouveau projet [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] et ajouter à ce projet tous les packages et les fichiers annexes que vous souhaitez déployer avec les packages.  
  
### <a name="to-create-the-integration-services-project"></a>Pour créer le projet Integration Services  
  
1.  Cliquez sur **Démarrer**, pointez sur **Tous les programmes**, sur **Microsoft SQL Server**, puis cliquez sur **SQL Server SQL Server Data Tools**.  
  
2.  Dans le menu **Fichier** , pointez sur **Nouveau**, puis cliquez sur **Projet** pour créer un projet [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] .  
  
3.  Dans la boîte de dialogue **Nouveau projet** , sélectionnez **Projet Integration Services** dans le volet **Modèles** .  
  
4.  Dans la zone **Nom** , remplacez le nom par défaut par **Didacticiel de déploiement**. Vous pouvez éventuellement désactiver la case à cocher **Créer le répertoire pour la solution** .  
  
5.  Acceptez l’emplacement par défaut ou cliquez sur **Parcourir** pour accéder au dossier que vous souhaitez utiliser.  
  
6.  Dans la boîte de dialogue **Emplacement du projet** , cliquez sur le dossier, puis sur **Ouvrir**.  
  
7.  Cliquez sur **OK**.  
  
8.  Par défaut, un package vide, nommé Package.dtsx, est créé et ajouté à votre projet. Cependant, vous ne pourrez pas utiliser ce package ; à la place, vous allez ajouter des packages existants au projet. Dans la mesure où tous les packages d'un projet sont inclus dans le déploiement, vous devez supprimer le fichier Package.dtsx. Pour ce faire, cliquez dessus avec le bouton droit, puis cliquez sur **Supprimer**.  
  
## <a name="next-task-in-lesson"></a>Tâche suivante de la leçon  
[Étape 3 : Ajout de packages et autres fichiers](../integration-services/lesson-1-3-adding-packages-and-other-files.md)  
  
## <a name="see-also"></a>Voir aussi  
[Projets Integration Services &#40;SSIS&#41;](~/integration-services/integration-services-ssis-projects-and-solutions.md)  
  
  
  

