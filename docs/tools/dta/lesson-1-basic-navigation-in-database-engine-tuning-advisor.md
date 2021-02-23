---
title: Navigation de base dans DTA
description: L’Assistant Paramétrage du moteur de base de données (DTA) fournit une interface graphique utilisateur (GUI) qui permet d’afficher des sessions de paramétrage et des rapports de recommandations de paramétrage.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- Database Engine Tuning Advisor [SQL Server], tutorials
ms.assetid: ad49b2e0-a5e3-49d2-80fd-9f4eaa3652cb
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-dt-2019
ms.date: 03/01/2017
ms.openlocfilehash: e8c4f32eab8fa7d7791a5f4718d8d771cb1dd6cd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354469"
---
# <a name="lesson-1-basic-navigation-in-database-engine-tuning-advisor-dta"></a>Leçon 1 : Navigation de base dans l’Assistant Paramétrage du moteur de base de données

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

L'Assistant Paramétrage du moteur de base de données fournit une interface graphique utilisateur qui permet d'afficher des sessions de paramétrage et des rapports de recommandations de paramétrage. Cette leçon explique comment démarrer cet outil et comment configurer l'affichage. À la fin de la leçon vous connaîtrez les différentes façons de démarrer cet outil et de configurer son affichage pour l'adapter aux tâches de paramétrage que vous effectuez régulièrement.  

## <a name="prerequisites"></a>Prérequis 

Pour suivre ce tutoriel, vous avez besoin de SQL Server Management Studio, de l’accès à un serveur qui exécute SQL Server et d’une base de données AdventureWorks.

- Installez [SQL Server Management Studio.](../../ssms/download-sql-server-management-studio-ssms.md)
- Installez [SQL Server 2017 Developer Edition.](https://www.microsoft.com/sql-server/sql-server-downloads)
- Téléchargez les [exemples de bases de données AdventureWorks2017.](../../samples/adventureworks-install-configure.md)


Les instructions de restauration de bases de données dans SSMS se trouvent ici : [Restaurer une base de données.](../../relational-databases/backup-restore/restore-a-database-backup-using-ssms.md)

  >[!NOTE]
  > Ce tutoriel est destiné aux utilisateurs familiarisés avec l’utilisation de SQL Server Management Studio et les tâches de base d’administration de base de données. 
  

## <a name="launch-database-tuning-advisor"></a>Lancement de l’Assistant Paramétrage de base de données 
Pour commencer, ouvrez l’interface utilisateur graphique de l’Assistant Paramétrage du moteur de base de données (DTA). Pour la première utilisation, un membre du rôle serveur fixe **sysadmin** doit lancer l’Assistant Paramétrage du moteur de base de données pour initialiser l’application. Après l’initialisation, les membres du rôle de base de données fixe **db_owner** peuvent utiliser l’Assistant Paramétrage du moteur de base de données pour paramétrer les bases de données dont ils sont propriétaires. Pour plus d’informations sur l’initialisation de l’Assistant Paramétrage du moteur de base de données, consultez [Démarrer et utiliser l’Assistant Paramétrage du moteur de base de données](../../relational-databases/performance/start-and-use-the-database-engine-tuning-advisor.md).  
  
1. Démarrez SSMS (SQL Server Management Studio). Dans le **menu Démarrer** de Windows, pointez sur **Tous les programmes** et recherchez **SQL Server Management Studio**. 
2. Une fois SSMS ouvert, sélectionnez le menu **Outils** et sélectionnez **Assistant Paramétrage de base de données**. 

  ![lancer DTA à partir de SSMS](media/dta-tutorials/launch-dta.png)

3. L’Assistant Paramétrage de base de données démarre et ouvre la boîte de dialogue **Se connecter au serveur**. Vérifiez les paramètres par défaut, puis sélectionnez **Se connecter** pour vous connecter à votre serveur SQL Server.  
  
Par défaut, l'Assistant Paramétrage du moteur de base de données s'ouvre avec la configuration représentée dans l'illustration suivante :  
  
![Fenêtre par défaut de l'Assistant Paramétrage du moteur de base de données](media/dta-tutorials/dta-default-gui.png)
  
> [!NOTE]  
> L’onglet **Moniteur de session** affiche le nom de la session, qui est le nom de l’utilisateur connecté et la date actuelle. 
  
Lorsque l'Assistant Paramétrage du moteur de base de données s'ouvre pour la première fois, deux volets principaux s'affichent.  
  
-   Le volet gauche contient le moniteur de session qui présente la liste de toutes les sessions de paramétrage réalisées sur cette instance de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Lorsque vous ouvrez l'Assistant Paramétrage du moteur de base de données, il affiche une nouvelle session en haut du volet. Vous pouvez attribuer un nom à cette session dans le volet voisin. Au départ, seule une session par défaut figure dans la liste. Il s'agit de la session par défaut que l'Assistant Paramétrage du moteur de base de données crée automatiquement pour vous. Une fois les bases de données paramétrées, toutes les sessions de paramétrage réalisées pour l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à laquelle vous êtes connecté sont indiquées sous la nouvelle session. Vous pouvez cliquer avec le bouton droit sur une session de paramétrage pour la renommer, la fermer, la supprimer ou la dupliquer. Si vous cliquez avec le bouton droit dans la liste, vous pouvez trier les sessions sur le nom, l'état, l'heure de création ou créer une nouvelle session. La section inférieure de ce volet présente des informations détaillées sur la session de paramétrage sélectionnée. Vous pouvez choisir d’afficher ces informations par catégorie en utilisant le bouton **Par catégorie** , ou vous pouvez les afficher par ordre alphabétique au moyen du bouton **Alphabétique** . Vous pouvez également masquer le moniteur de session en faisant glisser le bord du volet droit sur le côté gauche de la fenêtre. Pour le faire réapparaître, faites glisser le bord du volet vers la droite. Le moniteur de session permet d'afficher les sessions de paramétrage précédentes ou de les utiliser pour créer de nouvelles sessions avec des définitions similaires. Vous pouvez également utiliser le moniteur de session pour étudier les recommandations de paramétrage. Pour plus d’informations, consultez [Afficher et utiliser la sortie de l’Assistant Paramétrage du moteur de base de données](../../relational-databases/performance/view-and-work-with-the-output-from-the-database-engine-tuning-advisor.md). Utilisez le bouton **Précédent** de votre navigateur pour revenir au didacticiel.  
  
-   Le volet droit contient les onglets **Général** et **Options de paramétrage** . Ces onglets permettent de définir votre session de paramétrage du moteur de base de données. Sous l’onglet **Général** , vous pouvez taper le nom de votre session de paramétrage, spécifier le fichier de charge de travail ou la table à utiliser, et sélectionner les bases de données et les tables à paramétrer au cours de cette session. Une charge de travail est un ensemble d'instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] qui s'exécute sur une ou plusieurs bases de données que vous souhaitez paramétrer. L'Assistant Paramétrage du moteur de base de données utilise des fichiers de trace et des tables de trace, des scripts [!INCLUDE[tsql](../../includes/tsql-md.md)] ou des fichiers XML comme entrée de charge de travail pour le paramétrage des bases de données. Sous l’onglet **Options de paramétrage** , vous pouvez sélectionner les structures de création de base de données physiques (index ou vues indexées) et la stratégie de partitionnement que l’Assistant Paramétrage du moteur de base de données doit prendre en compte au cours de son analyse. Dans cet onglet, vous pouvez également spécifier la durée maximale pendant laquelle l'Assistant Paramétrage du moteur de base de données paramètre une charge de travail. La durée par défaut est d'une heure.  
  
> [!NOTE]
> L’Assistant Paramétrage du moteur de base de données peut accepter les fichiers XML comme entrée quand un script [!INCLUDE[tsql](../../includes/tsql-md.md)] est importé à partir de l’Éditeur de requête [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] . Pour plus d’informations, consultez la section sur le lancement de l’Assistant Paramétrage du moteur de base de données à partir de l’Éditeur de requête [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] dans [Démarrer et utiliser l’Assistant Paramétrage du moteur de base de données](../../relational-databases/performance/start-and-use-the-database-engine-tuning-advisor.md).  
  
## <a name="configure-tool-options-and-layout"></a>Configurer les options et la disposition des outils 

1.  Dans le menu **Outils** , cliquez sur **Options**.  

   ![Options DTA](media/dta-tutorials/dta-settings.png) 
  
2.  Dans la boîte de dialogue **Options** , passez en revue les options suivantes :  
  
    -   Développez la liste **Au démarrage** pour voir ce que l'Assistant Paramétrage du moteur de base de données affiche au démarrage. Par défaut, **Afficher une nouvelle session** est sélectionné.  
  
    -   Cliquez sur **Modifier la police** pour voir les polices que vous pouvez choisir dans les listes des bases de données et des tables dans l'onglet **Général** . Les polices choisies pour cette option sont également utilisées dans les grilles de recommandations et les rapports de l'Assistant Paramétrage du moteur de base de données une fois le paramétrage effectué. Par défaut, l'Assistant Paramétrage du moteur de base de données utilise les polices système.  
  
    -   Vous pouvez affecter à l'option **Nombre d'éléments dans les listes des derniers fichiers utilisés** une valeur comprise entre **1** et **10**. Cela permet de définir le nombre maximal d'éléments qui s'affichent dans les listes lorsque vous cliquez sur **Sessions récentes** ou sur **Fichiers récents** dans le menu **Fichier** . Par défaut, la valeur affectée à cette option est **4**.  
  
    -   Lorsque l'option **Mémoriser mes dernières options de paramétrage** est activée, l'Assistant Paramétrage du moteur de base de données utilise par défaut les options de paramétrage que vous avez spécifiées au cours de la dernière session de paramétrage pour la session de paramétrage suivante. Désactivez cette case à cocher pour utiliser les valeurs par défaut des options de paramétrage de l'Assistant Paramétrage du moteur de base de données. Cette option est activée par défaut.  
  
    -   Par défaut, l'option **Demander avant de supprimer définitivement les sessions** est activée pour éviter la suppression accidentelle des sessions de paramétrage.  
  
    -   Par défaut, l'option **Demander avant d'arrêter l'analyse de la session** est activée pour éviter d'arrêter accidentellement une session de paramétrage avant que l'Assistant Paramétrage du moteur de base de données n'ait terminé d'analyser une charge de travail.  
  
## <a name="next-lesson"></a>Leçon suivante  
[Leçon 2 : Utilisation de l'Assistant Paramétrage du moteur de base de données](../../tools/dta/lesson-2-using-database-engine-tuning-advisor.md)  
  
  
