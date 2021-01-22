---
description: 'Leçon 3-2 : Ajouter et configurer la journalisation'
title: 'Étape 2 : Ajouter et configurer la journalisation | Microsoft Docs'
ms.custom: ''
ms.date: 01/04/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: tutorial
ms.assetid: 56105f3f-e500-4669-8c8e-acf434527727
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 32daaf012159bd42a6f73330bc8371157e459609
ms.sourcegitcommit: 1f826eb3f73bd4d94bc9638b9cdd60991a2e2fa0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/12/2021
ms.locfileid: "98125612"
---
# <a name="lesson-3-2-add-and-configure-logging"></a>Leçon 3-2 : Ajouter et configurer la journalisation

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]



Au cours de cette tâche, vous allez activer la journalisation du flux de données dans le package Lesson 3.dtsx. Vous allez ensuite configurer un module fournisseur d’informations pour les fichiers texte pour journaliser les événements PipelineExecutionPlan et PipelineExecuteTrees. Le module fournisseur d’informations pour les fichiers texte crée des journaux faciles à consulter et à déplacer. La simplicité de ces fichiers journaux les rend utiles pendant la phase de test de base d’un package. Vous pouvez également consulter les entrées de journal dans la fenêtre **Journaux d’événements** du Concepteur [!INCLUDE[ssIS](../includes/ssis-md.md)].  
  
## <a name="add-logging-to-the-package"></a>Ajouter la journalisation au package  
  
1.  Dans le menu **SSIS**, sélectionnez **Journalisation**.  
    Dans Visual Studio 2019, le menu **SSIS** se trouve sous Extensions -> SSIS. Assurez-vous que l’onglet **Flux de données** est sélectionné à la place de **Flux de contrôle**
  
2.  Dans la boîte de dialogue **Configurer les journaux SSIS**, dans le volet **Conteneurs**, vérifiez que l’objet le plus élevé est sélectionné. Cet objet représente le package de la leçon 3.
  
3.  Sous l’onglet **Fournisseurs et journaux**, dans la zone **Type de fournisseur**, sélectionnez **Module fournisseur d’informations SSIS pour les fichiers texte**, puis **Ajouter**.  
  
    Integration Services ajoute un nouveau module fournisseur d’informations pour les fichiers texte au package avec le nom par défaut **Module fournisseur d’informations SSIS pour les fichiers texte**. Vous pouvez maintenant configurer le nouveau module fournisseur d'informations.  
  
4.  Dans la colonne **Nom**, entrez **Fichier journal de la leçon 3**.  
  
5.  Modifiez éventuellement les informations figurant dans **Description**.  
  
6.  Dans la colonne **Configuration**, sélectionnez **\<New Connection>** pour spécifier l’emplacement où [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] enregistre les informations de journal.  
  
    Dans la boîte de dialogue **Éditeur du gestionnaire de connexions de fichiers**, pour **Type d’utilisation**, sélectionnez **Créer un fichier**, puis **Parcourir**. Par défaut, la boîte de dialogue **Sélectionner un fichier** ouvre le dossier du projet, mais il est possible d’enregistrer les informations de journal n’importe où ailleurs.  
  
7.  Dans la boîte de dialogue **Sélectionner un fichier**, dans la zone **Nom de fichier**, entrez **TutorialLog.log** et sélectionnez **Ouvrir**.
  
8.  Sélectionnez **OK** pour fermer la boîte de dialogue **Éditeur du gestionnaire de connexions de fichiers**.  
  
9. Dans le volet **Conteneurs** , développez tous les nœuds de la hiérarchie des conteneurs du package, puis décochez toutes les cases, notamment la case **Extract Sample Currency Data** . Maintenant, cochez la case **Extract Sample Currency Data** pour extraire uniquement les événements de ce nœud.  
  
    > [!NOTE]  
    > Si la case **Extract Sample Currency Data** n’est pas cochée mais grisée, la tâche utilise les paramètres du journal du conteneur parent et vous ne pouvez pas activer les événements de journal propres à cette tâche. Pour résoudre ce problème, décochez la case du parent.
  
10. Dans le volet **Détails** , dans la colonne **Événements** , sélectionnez les événements **PipelineExecutionPlan** et **PipelineExecutionTrees** .  
  
11. Sélectionnez **Avancé** pour vérifier les informations détaillées que le module de fournisseur d’informations enregistre dans le journal pour chaque événement. Par défaut, toutes les catégories d'informations sont automatiquement sélectionnées pour les événements que vous avez spécifiés.  
  
12. Sélectionnez **De base** pour masquer les catégories d’informations.  
  
13. Sous l’onglet **Fournisseurs et journaux** , dans la colonne **Nom** , sélectionnez **Fichier Journal Leçon 3**. Après avoir créé un module fournisseur d’informations pour votre package, vous pouvez éventuellement le désélectionner pour désactiver la journalisation, sans avoir à le supprimer ni à en recréer un.  
  
14. Sélectionnez **OK**.  
  
## <a name="go-to-next-task"></a>Passer à la tâche suivante  
[Étape 3 : Tester le package de la leçon 3](../integration-services/lesson-3-3-testing-the-lesson-3-tutorial-package.md)  
  
