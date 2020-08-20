---
description: 'SSIS : comment créer un package ETL'
title: SSIS Guide pratique pour créer un package ETL | Microsoft Docs
ms.custom: ''
ms.date: 08/20/2018
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: quickstart
helpviewer_keywords:
- SSIS, tutorials
- packages [Integration Services], tutorials
- Integration Services, tutorials
- SQL Server Integration Services, tutorials
- logs [Integration Services], tutorials
- walkthroughs [Integration Services]
ms.assetid: d6d5bb1f-4cb1-4605-9cd6-f60b858382c4
author: chugugrace
ms.author: chugu
ms.openlocfilehash: f83cb45dc060bc2877cf316e4d19baa073a9e6e0
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88457029"
---
# <a name="ssis-how-to-create-an-etl-package"></a>SSIS : comment créer un package ETL

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]



Dans ce tutoriel, vous découvrez comment utiliser le Concepteur [!INCLUDE[ssIS](../includes/ssis-md.md)] pour créer un package [!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] simple. Ce package extrait les données d'un fichier plat, les reformate et les insère dans une table de faits. Dans les leçons suivantes, ce package est développé pour illustrer le bouclage, les configurations des packages, la journalisation et le flux des erreurs.  
  
Quand vous installez les exemples de données utilisés par le didacticiel, vous installez également les versions finales des packages créés au cours de chaque leçon du didacticiel. En utilisant les packages finaux, vous pouvez à votre guise passer outre une leçon et débuter à partir d'une leçon ultérieure du didacticiel. Si c’est la première fois que vous travaillez avec des packages ou le nouvel environnement de développement, nous vous recommandons de commencer par la leçon 1 de ce didacticiel.  

## <a name="what-is-sql-server-integration-services-ssis"></a>Qu’est-ce que SQL Server Integration Services (SSIS) ?

[!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] (SSIS) est une plateforme conçue pour créer des solutions d’intégration de données à hautes performances, notamment des packages d’extraction, de transformation et de chargement (ETL) pour l’entreposage de données. SSIS propose des outils graphiques et des assistants permettant de créer et de déboguer des packages ; des tâches permettant de réaliser des fonctions de flux de travail comme les opérations FTP, l'exécution d'instructions SQL et l'envoi de messages électroniques ; des sources de données et des destinations permettant d'extraire et de charger des données ; des transformations permettant de nettoyer, d'agréger, de fusionner et de copier des données ; une base de données de gestion, `SSISDB`, permettant d'administrer l'exécution et le stockage des packages, et des API (Application Programming Interface) permettant de programmer le modèle objet [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)].  

## <a name="what-you-learn"></a>Contenu du didacticiel  
Le meilleur moyen de se familiariser avec les nouveaux outils et les nouvelles commandes et fonctionnalités de [!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] est de les utiliser. Ce didacticiel vous guide dans le Concepteur [!INCLUDE[ssIS](../includes/ssis-md.md)] pour créer un package ETL simple qui inclut le bouclage, des configurations, la logique du flux des erreurs et la journalisation.  
  
## <a name="prerequisites"></a>Prérequis  
Ce tutoriel s’adresse aux utilisateurs qui sont familiers avec les principales opérations de base de données, mais qui ont une connaissance limitée des nouvelles fonctionnalités disponibles dans [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)].  

Pour exécuter ce tutoriel, les composants suivants doivent être installés :  
  
-   Voir [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] et [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)]. Pour installer SQL Server et SSIS, consultez [Installer Integration Services](install-windows/install-integration-services.md).

-   L’exemple de base de données **AdventureWorksDW2012**. Pour télécharger la base de données **AdventureWorksDW2012**, téléchargez `AdventureWorksDW2012.bak` à partir des [exemples de bases de données AdventureWorks](https://github.com/Microsoft/sql-server-samples/releases/tag/adventureworks) et restaurez la sauvegarde.  

-   Les fichiers d’**exemples de données**. Ces données exemple sont incluses dans les packages de leçons [!INCLUDE[ssIS](../includes/ssis-md.md)] . Pour télécharger les exemples de données et les packages de leçons dans un fichier Zip, consultez [SQL Server Integration Services Tutorial Files](https://www.microsoft.com/download/details.aspx?id=56827).

    - La plupart des fichiers dans le fichier zip sont des fichiers en lecture seule afin d’empêcher des modifications par inadvertance. Pour écrire la sortie dans un fichier ou la modifier, vous devrez désactiver l’attribut en lecture seule dans les propriétés du fichier.
    - Les exemples de packages supposent que les fichiers de données se trouvent dans le dossier `C:\Program Files\Microsoft SQL Server\100\Samples\Integration Services\Tutorial\Creating a Simple ETL Package`. Si vous le décompressez le fichier téléchargé vers un autre emplacement, vous devrez peut-être mettre à jour le chemin d’accès du fichier à plusieurs endroits dans les exemples de packages.

## <a name="lessons-in-this-tutorial"></a>Leçons du didacticiel  
[Leçon 1 : Créer un projet et un package de base avec SSIS](../integration-services/lesson-1-create-a-project-and-basic-package-with-ssis.md)  
Dans cette leçon, vous allez créer un package ETL simple qui extrait des données d’un fichier plat, transforme ces données en utilisant des transformations de recherche et charge le résultat dans une destination de table de faits.  
  
[Leçon 2 : Ajout d’un bouclage avec SSIS](../integration-services/lesson-2-adding-looping-with-ssis.md)  
Dans cette leçon, vous développez le package créé au cours de la leçon 1 pour utiliser les nouvelles fonctionnalités de bouclage et extraire des données de plusieurs fichiers plats dans un même processus de flux de données.  
  
[Leçon 3 : Ajouter la journalisation avec SSIS](../integration-services/lesson-3-add-logging-with-ssis.md)  
Dans cette leçon, vous développez le package que vous avez créé au cours de la leçon 2 pour utiliser les nouvelles fonctions de journalisation.  
  
[Leçon 4 : Ajouter une redirection de flux d’erreurs avec SSIS](../integration-services/lesson-4-add-error-flow-redirection-with-ssis.md)  
Dans cette leçon, vous développez le package que vous avez créé au cours de la leçon 3 pour utiliser les nouvelles configurations de sortie d’erreur.  
  
[Leçon 5 : Ajouter des configurations de package SSIS pour le modèle de déploiement de package](../integration-services/lesson-5-add-ssis-package-configurations-for-the-package-deployment-model.md)  
Dans cette leçon, vous développez le package que vous avez créé au cours de la leçon 4 pour utiliser de nouvelles options de configuration de package.  
  
[Leçon 6 : Utilisation des paramètres avec le modèle de déploiement de projet dans SSIS](../integration-services/lesson-6-using-parameters-with-the-project-deployment-model-in-ssis.md)  
Dans cette leçon, vous développez le package que vous avez créé au cours de la leçon 5 pour tirer parti des nouveaux paramètres dans le modèle de déploiement de projet.  
  
  
  
