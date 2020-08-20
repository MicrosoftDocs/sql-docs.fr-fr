---
description: 'Leçon 1 : Préparation à la création de l’application de déploiement'
title: 'Leçon 1 : Préparation à la création du bundle de déploiement | Microsoft Docs'
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: tutorial
ms.assetid: b6fe283c-9856-4ba1-a497-e3912424fd18
author: chugugrace
ms.author: chugu
ms.openlocfilehash: c5ef2363d07fc6560199154633fc45d93eb10c13
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88461992"
---
# <a name="lesson-1-preparing-to-create-the-deployment-bundle"></a>Leçon 1 : Préparation à la création de l’application de déploiement

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]


Au cours de cette leçon, vous allez créer les dossiers de travail et les variables d'environnement qui prennent en charge le didacticiel, créer un projet [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] , ajouter plusieurs packages et leurs fichiers de prise en charge au projet et implémenter les configurations dans des packages.  
  
[!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] déploie des packages sur la base d’un projet. Par conséquent, la première étape de création de l’application de déploiement consiste à rassembler tous les packages et les dépendances de package dans un seul projet [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] . Il est souvent utile d'inclure d'autres informations dans les packages déployés : par exemple, vous allez aussi ajouter au projet un fichier Lisezmoi qui fournit la documentation de base pour ce groupe de packages.  
  
Après l'ajout des fichiers et des packages, vous allez ajouter des configurations aux packages qui n'utilisent pas encore de configurations. Les configurations mettent à jour les propriétés des packages et les objets de package au moment de l'exécution. Au cours d'une autre leçon, vous allez modifier les valeurs de ces configurations au cours du déploiement du package pour prendre en charge les packages dans l'environnement de déploiement.  
  
Une fois les configurations ajoutées, vous devez ouvrir les packages dans le Concepteur [!INCLUDE[ssIS](../includes/ssis-md.md)] , outil graphique [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] permettant d'élaborer des packages ETL, puis examiner les propriétés des packages et des éléments de package ainsi que les configurations de package pour mieux comprendre les problèmes que doit résoudre le déploiement. Par exemple, un des packages extrait des données de fichiers texte, il faut donc mettre à jour l'emplacement des fichiers de données avant d'exécuter correctement les packages déployés.  
  
**Durée estimée pour effectuer cette leçon :** 1 heure  
  
## <a name="lesson-tasks"></a>Tâches de la leçon  
Cette leçon contient les tâches suivantes :  
  
-   [Étape 1 : Création des variables d’environnement et des dossiers de travail](../integration-services/lesson-1-1-creating-working-folders-and-environment-variables.md)  
  
-   [Étape 2 : Création du projet de déploiement](../integration-services/lesson-1-2-creating-the-deployment-project.md)  
  
-   [Étape 3 : Ajout de packages et autres fichiers](../integration-services/lesson-1-3-adding-packages-and-other-files.md)  
  
-   [Étape 4 : Ajout de configurations au package](../integration-services/lesson-1-4-adding-package-configurations.md)  
  
-   [Étape 5 : Test des packages mis à jour](../integration-services/lesson-1-5-testing-the-updated-packages.md)  
  
## <a name="start-the-lesson"></a>Démarrer la leçon  
[Étape 1 : Création des variables d’environnement et des dossiers de travail](../integration-services/lesson-1-1-creating-working-folders-and-environment-variables.md)  
  
  
  
