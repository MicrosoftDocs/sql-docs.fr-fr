---
title: Démarrer et utiliser l’Assistant Paramétrage du moteur de base de données | Microsoft Docs
description: Découvrez comment démarrer et utiliser l’Assistant Paramétrage du moteur de base de données dans SQL Server pour créer des charges de travail, paramétrer des bases de données et créer des fichiers d’entrée XML.
ms.custom: ''
ms.date: 01/09/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
f1_keywords:
- sql13.dta.workload.f1
- sql13.dta.general.f1
- sql13.dta.advancedtuningoptions.f1
- sql13.dta.tuningoptions.f1
- sql13.dta.progress.f1
- sql13.dta.options.f1
helpviewer_keywords:
- Database Engine Tuning Advisor [SQL Server], starting
ms.assetid: a4e3226a-3917-4ec8-bdf0-472879d231c9
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 9bf93bf9b59df77efe905a6c0debb722c63c3bbb
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643042"
---
# <a name="start-and-use-the-database-engine-tuning-advisor"></a>Démarrer et utiliser l'Assistant Paramétrage du moteur de base de données
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Cette rubrique décrit comment démarrer et utiliser l'Assistant Paramétrage du moteur de base de données dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]. Pour plus d’informations sur l’affichage et l’utilisation des résultats après avoir paramétré une base de données, consultez [Afficher et utiliser la sortie de l’Assistant Paramétrage du moteur de base de données](../../relational-databases/performance/view-and-work-with-the-output-from-the-database-engine-tuning-advisor.md).  
  
##  <a name="initialize-the-database-engine-tuning-advisor"></a><a name="Initialize"></a> Initialiser l'Assistant Paramétrage du moteur de base de données  
 Pour la première utilisation, un utilisateur membre du rôle serveur fixe **sysadmin** doit initialiser l'Assistant Paramétrage du moteur de base de données. Cela est dû au fait que plusieurs tables système doivent être créées dans la base de données **msdb** pour prendre en charge les opérations de paramétrage. L’initialisation permet aussi aux utilisateurs membres du rôle de base de données fixe **db_owner** de paramétrer des charges de travail sur les tables des bases de données dont ils sont propriétaires.  
  
 Un utilisateur disposant d'autorisations d'administrateur système doit exécuter l'une des actions suivantes :  
  
-   Utiliser l'interface utilisateur graphique de l'Assistant Paramétrage du moteur de base de données pour se connecter à une instance de [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]. Pour plus d'informations, consultez [Démarrer l'Assistant Paramétrage du moteur de base de données](#Start) plus loin dans cette rubrique.  
  
-   Recourir à l’utilitaire **dta** pour paramétrer la première charge de travail. Pour plus d'informations, consultez [Utilisation de l'utilitaire dta](#dta) , plus loin dans cette rubrique.  
  
##  <a name="start-the-database-engine-tuning-advisor"></a><a name="Start"></a> Démarrer l'Assistant Paramétrage du moteur de base de données  
 Vous pouvez démarrer l'interface utilisateur graphique de l'Assistant Paramétrage du moteur de base de données de plusieurs façons différentes pour prendre en charge le paramétrage des bases de données dans de nombreux scénarios. L'Assistant Paramétrage du moteur de base de données peut être démarré : à partir du menu **Démarrer** , du menu **Outils** dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], de l'éditeur de requête dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], et du menu **Outils** dans [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]. Lorsque vous démarrez l'Assistant Paramétrage du moteur de base de données pour la première fois, l'application affiche une boîte de dialogue **Se connecter au serveur** dans laquelle vous pouvez spécifier l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à laquelle vous souhaitez vous connecter.  
  
> [!WARNING]  
>  Ne démarrez pas l'Assistant Paramétrage du moteur de base de données lorsque [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est exécuté en mode mono-utilisateur. Si vous tentez de le démarrer alors que le serveur est en mode mono-utilisateur, une erreur est renvoyée et l'Assistant ne démarre pas. Pour plus d’informations sur le mode mono-utilisateur, consultez [Démarrer SQL Server en mode mono-utilisateur](../../database-engine/configure-windows/start-sql-server-in-single-user-mode.md).  
  
#### <a name="to-start-database-engine-tuning-advisor-from-the-windows-start-menu"></a>Pour démarrer l'Assistant Paramétrage du moteur de base de données à partir du menu Démarrer de Windows  
  
1.  Dans le menu **Démarrer** , pointez sur **Tous les programmes**, sur **Microsoft SQL Server**, sur **Outils de performances**, puis cliquez sur **Assistant Paramétrage du moteur de base de données**.  
  
#### <a name="to-start-the-database-engine-tuning-advisor-in-sql-server-management-studio"></a>Pour démarrer l'Assistant Paramétrage du moteur de base de données dans SQL Server Management Studio  
  
1.  Dans le menu **Outils** de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], cliquez sur **Assistant Paramétrage du moteur de base de données**.  
  
#### <a name="to-start-the-database-engine-tuning-advisor-from-the-sql-server-management-studio-query-editor"></a>Pour démarrer l'Assistant Paramétrage du moteur de base de données à partir de l'Éditeur de requête SQL Server Management Studio  
  
1.  Ouvrez un fichier de script [!INCLUDE[tsql](../../includes/tsql-md.md)] dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Pour plus d’informations, consultez [Éditeurs de texte et de requête &#40;SQL Server Management Studio&#41;](../../ssms/f1-help/database-engine-query-editor-sql-server-management-studio.md).  
  
2.  Sélectionnez une requête dans le script [!INCLUDE[tsql](../../includes/tsql-md.md)] ou sélectionnez le script en entier, cliquez avec le bouton droit sur la sélection, puis choisissez **Analyser la requête dans l’Assistant Paramétrage de base de données**. L'interface utilisateur graphique de l'Assistant Paramétrage du moteur de base de données s'ouvre et importe le script sous forme de charge de travail de fichier XML. Vous pouvez spécifier un nom de session et des options de paramétrage pour paramétrer les requêtes [!INCLUDE[tsql](../../includes/tsql-md.md)] sélectionnées en tant que charge de travail.  
  
#### <a name="to-start-the-database-engine-tuning-advisor-in-sql-server-profiler"></a>Pour démarrer l'Assistant Paramétrage du moteur de base de données dans le Générateur de profils SQL Server Profiler  
  
1.  Dans le menu **Outils** du Générateur de profils SQL Server Profiler, cliquez sur **Assistant Paramétrage du moteur de base de données**.  
  
##  <a name="create-a-workload"></a><a name="Create"></a> Créer une charge de travail  
 Une charge de travail est un ensemble d'instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] qui s'exécute sur une ou plusieurs bases de données que vous souhaitez paramétrer. L'Assistant Paramétrage du moteur de base de données analyse ces charges de travail pour recommander des index ou des stratégies de partitionnement qui amélioreront les performances des requêtes de votre serveur.  
  
 Vous pouvez créer une charge de travail à l'aide de l'une des méthodes suivantes.  
  
-   Utilisez le [Magasin de requêtes](../../relational-databases/performance/how-query-store-collects-data.md) en tant que charge de travail. Vous pouvez ainsi éviter d'avoir à créer manuellement une charge de travail. Pour plus d’informations, consultez [Paramétrage de base de données à l’aide des charges de travail du Magasin de requêtes](../../relational-databases/performance/tuning-database-using-workload-from-query-store.md).  

      ||  
      |-|  
      |**S’applique à** : [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] et versions ultérieures.|  

  
-   Utilisez le cache du plan comme charge de travail. Vous pouvez ainsi éviter d'avoir à créer manuellement une charge de travail. Pour plus d'informations, consultez [Paramétrer une base de données](#Tune) , plus loin dans cette rubrique.  
  
-   Utilisez l'Éditeur de requête de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou votre éditeur de texte préféré pour créer manuellement des charges de travail par script [!INCLUDE[tsql](../../includes/tsql-md.md)] .  
  
-   Utilisez [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] pour créer des charges de travail par fichier de trace ou par table de trace.  
  
    > [!NOTE]  
    >  Lors de l'utilisation d'une table de trace comme charge de travail, cette table doit exister sur le même serveur que celui où l'Assistant Paramétrage du moteur de base de données effectue le paramétrage. Si vous créez une table de trace sur un autre serveur, alors déplacez-la sur celui où l'Assistant Paramétrage du moteur de base de données effectue le paramétrage.  
  
-   Les charges de travail peuvent également être intégrées à un fichier d'entrée XML, où vous pouvez également spécifier un poids pour chaque événement. Pour plus d'informations sur la spécification des charges de travail incorporées, consultez [Créer un fichier d'entrée XML](#XMLInput) plus loin dans cette rubrique.  
  
###  <a name="to-create-transact-sql-script-workloads"></a><a name="SSMS"></a> Pour créer des charges de travail par script Transact-SQL  
  
1.  Lancez l'éditeur de requête dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Pour plus d’informations, consultez [Éditeurs de texte et de requête &#40;SQL Server Management Studio&#41;](../../ssms/f1-help/database-engine-query-editor-sql-server-management-studio.md).  
  
2.  Tapez votre script [!INCLUDE[tsql](../../includes/tsql-md.md)] dans l'éditeur de requête. Ce script doit contenir un ensemble d'instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] qui s'exécutent sur la ou les bases de données que vous voulez paramétrer.  
  
3.  Enregistrez le fichier avec une extension **.sql** . L’interface utilisateur graphique de l’Assistant Paramétrage du moteur de base de données et l’utilitaire en ligne de commande **dta** peuvent utiliser ce script [!INCLUDE[tsql](../../includes/tsql-md.md)] comme charge de travail.  
  
###  <a name="to-create-trace-file-and-trace-table-workloads"></a><a name="Profiler"></a> Pour créer des charges de travail par fichier de trace et par table de trace  
  
1.  Lancez le [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] au moyen de l'une des méthodes suivantes :  
  
    -   Dans le menu **Démarrer** , pointez sur **Tous les programmes**, sur **Microsoft SQL Server**, sur **Outils de performances**, puis cliquez sur **SQL Server Profiler**.  
  
    -   Dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], cliquez sur le menu **Outils** , puis sur **SQL Server Profiler**.  
  
2.  Créez un fichier ou une table de trace comme décrit dans les procédures suivantes, qui utilise le modèle **Réglage** de [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] :  
  
    -   [Créer une trace &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/create-a-trace-sql-server-profiler.md)  
  
    -   [Enregistrer des résultats d’une trace dans un fichier &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/save-trace-results-to-a-file-sql-server-profiler.md)  
  
         L'Assistant Paramétrage du moteur de base de données suppose que le fichier de trace de la charge de travail est un fichier de substitution. Pour plus d'informations sur les fichiers de substitution, consultez [Limit Trace File and Table Sizes](../../relational-databases/sql-trace/limit-trace-file-and-table-sizes.md).  
  
    -   [Enregistrer des résultats d’une trace dans une table &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/save-trace-results-to-a-table-sql-server-profiler.md)  
  
         Vérifiez que le suivi de trace s'est arrêté avant d'utiliser une table de trace en tant que charge de travail.  
  
 Il est recommandé d'utiliser le modèle Tuning de SQL Server Profiler pour capturer les charges de travail pour l'Assistant Paramétrage du moteur de base de données.  
  
 Si vous souhaitez utiliser votre propre modèle, vérifiez que les événements de trace suivants sont capturés :  
  
-   **RPC:Completed**  
  
-   **SQL:BatchCompleted**  
  
-   **SP:StmtCompleted**  
  
 Vous pouvez également utiliser les versions **Starting** de ces événements trace. Par exemple, **SQL:BatchStarting**. Toutefois, les versions **Completed** de ces événements trace contiennent la colonne **Duration** , qui permet à l'Assistant Paramétrage du moteur de base de données de paramétrer plus efficacement la charge de travail. L'Assistant Paramétrage du moteur de base de données ne paramètre pas d'autres types d'événements trace. Pour plus d'informations sur ces événements trace, consultez [Stored Procedures Event Category](../../relational-databases/event-classes/stored-procedures-event-category.md) et [TSQL Event Category](../../relational-databases/event-classes/tsql-event-category.md). Pour plus d’informations sur l’utilisation des procédures stockées Trace SQL pour créer une charge de travail par fichier de trace, consultez [Créer une trace &#40;Transact-SQL&#41;](../../relational-databases/sql-trace/create-a-trace-transact-sql.md).  
  
### <a name="trace-file-or-trace-table-workloads-that-contain-the-loginname-data-column"></a>Charges de travail de fichiers ou de tables de trace contenant la colonne de données LoginName  
 L'Assistant Paramétrage du moteur de base de données soumet les requêtes Showplan dans le cadre du processus de paramétrage. Lorsqu'un fichier ou une table de trace contenant la colonne de données **LoginName** est consommé comme charge de travail, l'Assistant Paramétrage du moteur de base de données emprunte l'identité de l'utilisateur spécifié dans **LoginName**. Si cet utilisateur ne détient pas l'autorisation SHOWPLAN qui lui permet d'exécuter et de produire des Showplans pour les instructions contenues dans la trace, l'Assistant Paramétrage du moteur de base de données ne paramètre pas ces instructions.  
  
##### <a name="to-avoid-granting-the-showplan-permission-to-each-user-specified-in-the-loginname-column-of-the-trace"></a>Pour éviter d'avoir à octroyer l'autorisation SHOWPLAN à chaque utilisateur spécifié dans la colonne LoginName de la trace  
  
1.  Paramétrez la charge de travail de fichier de trace et de table de trace. Pour plus d'informations, consultez [Paramétrer une base de données](#Tune) , plus loin dans cette rubrique.  
  
2.  Contrôlez dans le journal de paramétrage quelles instructions n'ont pas été paramétrées en raison d'autorisations inadéquates. Pour plus d’informations, consultez [Afficher et utiliser la sortie de l’Assistant Paramétrage du moteur de base de données](../../relational-databases/performance/view-and-work-with-the-output-from-the-database-engine-tuning-advisor.md).  
  
3.  Créez une nouvelle charge de travail en supprimant la colonne **LoginName** dans les événements qui n'ont pas été paramétrés, puis sauvegardez uniquement les événements non paramétrés dans un nouveau fichier de trace ou une nouvelle table de trace. Pour plus d’informations sur la suppression des colonnes de données à partir d’une trace, consultez [Spécifier les événements et les colonnes de données d’un fichier de trace &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/specify-events-and-data-columns-for-a-trace-file-sql-server-profiler.md) ou [Modifier une trace existante &#40;Transact-SQL&#41;](../../relational-databases/sql-trace/modify-an-existing-trace-transact-sql.md).  
  
4.  Soumettez de nouveau la nouvelle charge de travail sans la colonne **LoginName** à l'Assistant Paramétrage du moteur de base de données.  
  
 L'Assistant Paramétrage du moteur de base de données va procéder au paramétrage de cette nouvelle charge de travail, parce qu'aucune information de connexion n'est spécifiée dans la trace. S’il n’existe pas de **LoginName** pour une instruction, l’Assistant Paramétrage du moteur de base de données paramètre cette instruction en empruntant l’identité de l’utilisateur qui a démarré la session de paramétrage (membre du rôle serveur fixe **sysadmin** ou du rôle de base de données fixe **db_owner** ).  
  
##  <a name="tune-a-database"></a><a name="Tune"></a> Paramétrer une base de données  
 Pour paramétrer une base de données, vous pouvez travailler dans l'interface utilisateur graphique de l'Assistant Paramétrage du moteur de base de données ou dans l'utilitaire **dta** .  
  
> [!NOTE]  
>  Assurez-vous que la trace est terminée avant d'utiliser une table de trace en tant que charge de travail pour l'Assistant Paramétrage du moteur de base de données. L'Assistant Paramétrage du moteur de base de données ne permet pas d'utiliser comme charge de travail une table de trace dans laquelle des événements de trace sont en cours d'écriture.  
  
### <a name="use-the-database-engine-tuning-advisor-graphical-user-interface"></a>Travailler dans l'interface utilisateur graphique de l'Assistant Paramétrage du moteur de base de données  
 Dans l'interface utilisateur graphique de l'Assistant Paramétrage du moteur de base de données, vous pouvez paramétrer une base de données au moyen du cache du plan, de fichiers de charge de travail ou de tables de charge de travail. Vous pouvez utiliser l'interface utilisateur graphique de l'Assistant Paramétrage du moteur de base de données pour visualiser de façon simple les résultats de votre session de paramétrage en cours et des sessions précédentes. Pour plus d'informations sur les options de l'interface utilisateur, consultez [Descriptions de l'interface utilisateur](#UI) plus loin dans cette rubrique. Pour plus d’informations sur l’utilisation des résultats après avoir paramétré une base de données, consultez [Afficher et utiliser la sortie de l’Assistant Paramétrage du moteur de base de données](../../relational-databases/performance/view-and-work-with-the-output-from-the-database-engine-tuning-advisor.md).  

####  <a name="to-tune-a-database-by-using-the-query-store"></a><a name="PlanCache"></a> Pour paramétrer une base de données à l’aide du Magasin de requêtes
Pour plus d’informations, consultez [Paramétrage de base de données à l’aide des charges de travail du Magasin de requêtes](../../relational-databases/performance/tuning-database-using-workload-from-query-store.md).
  
####  <a name="to-tune-a-database-by-using-the-plan-cache"></a><a name="PlanCache"></a> Pour paramétrer une base de données à l'aide du cache du plan  
  
1.  Lancez l'Assistant Paramétrage du moteur de base de données, puis connectez-vous à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d'informations, consultez [Démarrer l'Assistant Paramétrage du moteur de base de données](#Start) plus haut dans cette rubrique.  
  
2.  Sous l'onglet **Général** , tapez un nom dans la zone **Nom de session** pour créer une nouvelle session de paramétrage. Vous devez configurer les champs figurant sous l'onglet **Général** avant de démarrer une session de paramétrage. Il n'est pas nécessaire de modifier les paramètres de l'onglet **Options de paramétrage** avant de lancer une session de paramétrage.  
  
3.  Sélectionnez **Cache du plan** comme option de charge de travail. L'Assistant Paramétrage du moteur de base de données sélectionne les 1 000 principaux événements du cache du plan à utiliser pour l'analyse.  
  
4.  Sélectionnez la ou les bases de données à paramétrer et, éventuellement, choisissez une ou plusieurs tables de chaque base de données dans **Tables sélectionnées**. Pour inclure des entrées du cache pour toutes les bases de données, dans **Options de paramétrage**, cliquez sur **Options avancées** et activez la case à cocher **Inclure les événements du cache du plan de toutes les bases de données**.  
  
5.  Activez la case à cocher **Enregistrer le journal de paramétrage** pour enregistrer une copie du journal de paramétrage. Désactivez-la si vous ne souhaitez pas en enregistrer une copie.  
  
     Vous pouvez consulter le journal de paramétrage en ouvrant la session et en sélectionnant l'onglet **Progression** .  
  
6.  Cliquez sur l'onglet **Options de paramétrage** , puis effectuez une sélection parmi les options proposées.  
  
7.  Cliquez sur **Démarrer l'analyse**.  
  
     Si vous souhaitez arrêter la session de paramétrage après son démarrage, choisissez une des options suivantes dans le menu **Actions** :  
  
    -   **Arrêter l’analyse (avec recommandations)** arrête la session de paramétrage et vous demande de spécifier si l’Assistant Paramétrage du moteur de base de données doit générer des recommandations sur la base de l’analyse réalisée jusqu’à ce point.  
  
    -   **Arrêter l'analyse** arrête la session de paramétrage sans générer de recommandation.  
  
> [!NOTE]  
>  La suspension de l'Assistant Paramétrage du moteur de base de données n'est pas acceptée. Si vous cliquez sur le bouton de barre d’outils **Démarrer l’analyse** après avoir cliqué soit sur **Arrêter l’analyse** soit sur **Arrêter l’analyse (avec recommandations)** , l’Assistant Paramétrage du moteur de base de données démarre une nouvelle session de paramétrage.  
  
#### <a name="to-tune-a-database-using-a-workload-file-or-table-as-input"></a>Pour paramétrer une base de données en utilisant un fichier ou une table de charge de travail comme entrée  
  
1.  Déterminez les fonctionnalités de base de données (index, vues indexées, partitionnement) que l'Assistant Paramétrage du moteur de base de données doit ajouter, supprimer ou conserver pendant l'analyse.  
  
2.  Créez une charge de travail. Pour plus d'informations, consultez [Créer une charge de travail](#Create) plus haut dans cette rubrique.  
  
3.  Lancez l’Assistant Paramétrage du moteur de base de données, puis connectez-vous à une instance de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d'informations, consultez [Démarrer l'Assistant Paramétrage du moteur de base de données](#Start) plus haut dans cette rubrique.  
  
4.  Sous l'onglet **Général** , tapez un nom dans la zone **Nom de session** pour créer une nouvelle session de paramétrage.  
  
5.  Choisissez **Fichier de charge de travail** ou **Table de charge de travail** , puis tapez le chemin d'accès au fichier ou le nom de la table dans la zone de texte adjacente.  
  
     La table doit être spécifiée dans le format suivant :  
  
    ```  
    database_name.schema_name.table_name  
    ```  
  
     Pour rechercher un fichier ou une table de charge de travail, cliquez sur **Parcourir**. L'Assistant Paramétrage du moteur de base de données part de l'hypothèse que les fichiers de charge de travail sont des fichiers de substitution. Pour plus d'informations sur les fichiers de substitution, consultez [Limit Trace File and Table Sizes](../../relational-databases/sql-trace/limit-trace-file-and-table-sizes.md).  
  
     Lorsque vous utilisez une table de trace comme charge de travail, cette table doit exister sur le serveur en cours de paramétrage par l'Assistant Paramétrage du moteur de base de données. Si vous créez la table de trace sur un autre serveur, déplacez-le sur le serveur que paramètre l'Assistant Paramétrage du moteur de base de données avant de l'utiliser comme charge de travail.  
  
6.  Sélectionnez les bases de données et tables auxquelles vous souhaitez appliquer la charge de travail sélectionnée à l'étape 5. Pour sélectionner les tables, cliquez sur la flèche **Tables sélectionnées** .  
  
7.  Activez la case à cocher **Enregistrer le journal de paramétrage** pour enregistrer une copie du journal de paramétrage. Désactivez-la si vous ne souhaitez pas en enregistrer une copie.  
  
     Vous pouvez consulter le journal de paramétrage en ouvrant la session et en sélectionnant l'onglet **Progression** .  
  
8.  Cliquez sur l'onglet **Options de paramétrage** , puis effectuez une sélection parmi les options proposées.  
  
9. Cliquez sur le bouton **Démarrer l'analyse** situé dans la barre d'outils.  
  
     Si vous souhaitez arrêter la session de paramétrage après son démarrage, choisissez une des options suivantes dans le menu **Actions** :  
  
    -   **Arrêter l’analyse (avec recommandations)** arrête la session de paramétrage et vous demande de spécifier si l’Assistant Paramétrage du moteur de base de données doit générer des recommandations sur la base de l’analyse réalisée jusqu’à ce point.  
  
    -   **Arrêter l'analyse** arrête la session de paramétrage sans générer de recommandation.  
  
> [!NOTE]  
>  La suspension de l'Assistant Paramétrage du moteur de base de données n'est pas acceptée. Si vous cliquez sur le bouton de barre d’outils **Démarrer l’analyse** après avoir cliqué soit sur **Arrêter l’analyse** soit sur **Arrêter l’analyse (avec recommandations)** , l’Assistant Paramétrage du moteur de base de données démarre une nouvelle session de paramétrage.  
  
###  <a name="use-the-dta-utility"></a><a name="dta"></a> Utilisation de l'utilitaire dta  
 L' [utilitaire dta](../../tools/dta/dta-utility.md) fournit un fichier exécutable d'invite de commandes qui vous permet de paramétrer des bases de données. Il vous permet d'utiliser l'Assistant Paramétrage du moteur de base de données dans les scripts et les fichiers de commandes. L’utilitaire **dta** accepte les entrées de cache du plan, les fichiers de trace, les tables de trace et les scripts [!INCLUDE[tsql](../../includes/tsql-md.md)] en tant que charges de travail. Il accepte également les entrées XML qui sont conformes aux schéma XML de l'Assistant Paramétrage du moteur de base de données, qui est disponible sur ce [site Web de Microsoft](https://go.microsoft.com/fwlink/?linkid=43100).  
  
 Prenez en considération les points suivants avant de paramétrer une charge de travail à l’aide de l’utilitaire **dta** :  
  
-   Lorsque vous utilisez une table de trace comme charge de travail, cette table doit exister sur le serveur en cours de paramétrage par l'Assistant Paramétrage du moteur de base de données. Si vous créez la table de trace sur un autre serveur, vous devez ensuite la déplacer sur le serveur que l'Assistant Paramétrage du moteur de base de données est en train de paramétrer.  
  
-   Assurez-vous que la trace est terminée avant d'utiliser une table de trace en tant que charge de travail pour l'Assistant Paramétrage du moteur de base de données. L'Assistant Paramétrage du moteur de base de données ne permet pas d'utiliser comme charge de travail une table de trace dans laquelle des événements de trace sont en cours d'écriture.  
  
-   Si une session de paramétrage s’exécute plus longtemps que prévu, vous pouvez appuyer sur Ctrl+C pour l’arrêter et générer des recommandations basées sur l’analyse effectuée à ce stade par l’utilitaire **dta** . Un message vous demande de déterminer si vous souhaitez générer ou non des recommandations. Appuyez de nouveau sur CTRL+C pour arrêter la session de réglage sans générer de recommandations.  
  
 Pour plus d’informations sur la syntaxe de l’utilitaire **dta** ainsi que des exemples, voir [dta Utility](../../tools/dta/dta-utility.md).  
  
#### <a name="to-tune-a-database-by-using-the-plan-cache"></a>Pour paramétrer une base de données à l'aide du cache du plan  
  
1.  Spécifiez l’option **-ip** . Les 1 000 premiers événements du cache du plan pour les bases de données sélectionnées sont analysés.  
  
     À partir d'une invite de commandes, saisissez la commande suivante :  
  
    ```  
    dta -E -D DatabaseName -ip -s SessionName  
    ```  
  
2.  Pour modifier le nombre d’événements à utiliser pour l’analyse, spécifiez l’option **-n**. L'exemple suivant augmente le nombre d'entrées du cache à 2 000.  
  
    ```  
    dta -E -D DatabaseName -ip -n 2000-s SessionName1  
    ```  
  
3.  Pour analyser les événements pour toutes les bases de données de l’instance, spécifiez l’option **-ipf** .  
  
    ```  
    dta -E -D DatabaseName -ip -ipf -n 2000 -s SessionName2  
    ```  
  
#### <a name="to-tune-a-database-by-using-a-workload-and-dta-utility-default-settings"></a>Pour paramétrer une base de données en utilisant les paramètres par défaut d'une charge de travail et de l'utilitaire dta  
  
1.  Déterminez les fonctionnalités de base de données (index, vues indexées, partitionnement) que l'Assistant Paramétrage du moteur de base de données doit ajouter, supprimer ou conserver pendant l'analyse.  
  
2.  Créez une charge de travail. Pour plus d'informations, consultez [Créer une charge de travail](#Create) plus haut dans cette rubrique.  
  
3.  À partir d'une invite de commandes, saisissez la commande suivante :  
  
    ```  
    dta -E -D DatabaseName -if WorkloadFile -s SessionName  
    ```  
  
     où `-E` spécifie que la session de paramétrage utilise une connexion approuvée (au lieu d'un ID de connexion et d'un mot de passe), et `-D` indique le nom de la base de données à paramétrer. Par défaut, l'utilitaire se connecte à l'instance par défaut de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur l'ordinateur local. (Utilisez l'option `-S` pour spécifier une base de données distante, comme le montre la procédure suivante, ou pour spécifier une instance nommée.) L'option `-if` spécifie le nom et le chemin d'accès d'un fichier de charge de travail (qui peut être un script [!INCLUDE[tsql](../../includes/tsql-md.md)] ou un fichier de trace) tandis que l'option `-s` spécifie un nom pour la session de paramétrage.  
  
     Les quatre options indiquées ici (nom de base de données, charge de travail, type de connexion et nom de session) sont obligatoires.  
  
#### <a name="to-tune-a-remote-database-or-a-named-instance-for-a-specific-duration"></a>Pour paramétrer une base de données distante ou une instance nommée pour une durée spécifique  
  
1.  Déterminez les fonctionnalités de base de données (index, vues indexées, partitionnement) que l'Assistant Paramétrage du moteur de base de données doit ajouter, supprimer ou conserver pendant l'analyse.  
  
2.  Créez une charge de travail. Pour plus d'informations, consultez [Créer une charge de travail](#Create) plus haut dans cette rubrique.  
  
3.  À partir d'une invite de commandes, saisissez la commande suivante :  
  
    ```  
    dta -S ServerName\Instance -D DatabaseName -it WorkloadTableName   
    -U LoginID -P Password -s SessionName -A TuningTimeInMinutes  
    ```  
  
     où l'option `-S` spécifie un nom de serveur distant et une instance distante (ou une instance nommée sur le serveur local) et l'option `-D` indique le nom de la base de données à paramétrer. L'option `-it` spécifie le nom de la table de charge de travail, les options `-U` et `-P` définissent l'ID de connexion et le mot de passe permettant d'accéder à la base de données distante, l'option `-s` indique le nom de la session de paramétrage tandis que l'option `-A` précise, en minutes, la durée de la session de paramétrage. Par défaut, l’utilitaire **dta** utilise une durée de paramétrage de 8 heures. Si vous voulez que l’Assistant Paramétrage du moteur de base de données paramètre une charge de travail pour une durée illimitée, spécifiez **0** (zéro) pour l’option `-A` .  
  
#### <a name="to-tune-a-database-using-an-xml-input-file"></a>Pour paramétrer une base de données à l'aide d'un fichier d'entrée XML  
  
1.  Déterminez les fonctionnalités de base de données (index, vues indexées, partitionnement) que l'Assistant Paramétrage du moteur de base de données doit ajouter, supprimer ou conserver pendant l'analyse.  
  
2.  Créez une charge de travail. Pour plus d'informations, consultez [Créer une charge de travail](#Create) plus haut dans cette rubrique.  
  
3.  Créez un fichier d'entrée XML. Pour plus d'informations, consultez [Créer des fichiers d'entrée XML](#XMLInput) plus loin dans cette rubrique.  
  
4.  À partir d'une invite de commandes, saisissez la commande suivante :  
  
    ```  
    dta -E -S ServerName\Instance -s SessionName -ix PathToXMLInputFile  
    ```  
  
     où l'option `-E` spécifie une connexion approuvée, l'option `-S` définit un serveur distant et une instance distante, ou une instance nommée sur le serveur local, l'option `-s` indique le nom d'une session de paramétrage, tandis que l'option `-ix` spécifie le fichier d'entrée XML à utiliser pour la session de paramétrage.  
  
5.  Une fois que l'utilitaire a terminé de paramétrer la charge de travail, vous pouvez visualiser les résultats des sessions de paramétrage à l'aide de l'interface utilisateur graphique de l'Assistant Paramétrage du moteur de base de données. Par ailleurs, à l’aide de l’option **-ox** , vous pouvez aussi demander à ce que les recommandations de paramétrage soient écrites dans un fichier XML. Pour plus d’informations, consultez [dta Utility](../../tools/dta/dta-utility.md).  
  
##  <a name="create-an-xml-input-file"></a><a name="XMLInput"></a> Créer un fichier d'entrée XML  
 Si vous êtes développeur XML expérimenté, vous pouvez créer des fichiers formatés en XML que l'Assistant Paramétrage du [!INCLUDE[ssDE](../../includes/ssde-md.md)] peut utiliser pour paramétrer les charges de travail. Pour créer ces fichiers XML, utilisez vos outils XML préférés pour modifier un exemple de fichier ou pour générer une instance à partir du schéma XML de l'Assistant Paramétrage du [!INCLUDE[ssDE](../../includes/ssde-md.md)] .  
  
 Le schéma XML de l’Assistant Paramétrage du [!INCLUDE[ssDE](../../includes/ssde-md.md)] est disponible dans votre installation [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à l’emplacement suivant :  
  
 `C:\Program Files\Microsoft SQL Server\100\Tools\Binn\schemas\sqlserver\2004\07\dta\dtaschema.xsd` 
  
 Le schéma XML de l'Assistant Paramétrage du [!INCLUDE[ssDE](../../includes/ssde-md.md)] est également disponible en ligne sur ce [site Web de Microsoft](https://go.microsoft.com/fwlink/?linkid=43100&clcid=0x409).  
  
 Cette adresse URL pointe vers une page contenant de nombreux schémas XML [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Parcourez la page pour atteindre la ligne correspondant à l'Assistant Paramétrage du [!INCLUDE[ssDE](../../includes/ssde-md.md)] .  
  
#### <a name="to-create-an-xml-input-file-to-tune-workloads"></a>Pour créer un fichier d'entrée XML pour paramétrer les charges de travail  
  
1.  Créez une charge de travail. Vous pouvez utiliser un fichier ou une table de trace à l'aide du modèle de paramétrage de [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)], ou bien créer un script [!INCLUDE[tsql](../../includes/tsql-md.md)] qui reproduit une charge de travail représentative de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d'informations, consultez [Créer une charge de travail](#Create) plus haut dans cette rubrique.  
  
2.  Créez un fichier d'entrée XML par l'une des méthodes suivantes :  
  
    -   Copiez et collez l’un des [exemples de fichier d’entrée XML &#40;Assistant Paramétrage de base de données&#41;](../../tools/dta/xml-input-file-samples-dta.md) dans votre éditeur XML préféré. Modifiez les valeurs pour qu'elles traduisent les arguments appropriés à votre installation [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , puis enregistrez les fichiers XML.  
  
    -   À l'aide de votre outil XML préféré, générez une instance à partir du schéma XML de l'Assistant Paramétrage du [!INCLUDE[ssDE](../../includes/ssde-md.md)] .  
  
3.  Après avoir créé le fichier d’entrée XML, utilisez-le comme entrée dans l’utilitaire en ligne de commande **dta** pour paramétrer la charge de travail. Pour plus d'informations sur l'utilisation de fichiers d'entrée XML avec cet utilitaire, consultez la section [Utiliser l'utilitaire dta](#dta) plus haut dans cette rubrique.  
  
> [!NOTE]  
>  Si vous voulez utiliser une charge de travail inline, qui est une charge de travail spécifiée directement dans le fichier d’entrée XML, utilisez l’[exemple de fichier d’entrée XML avec une charge de travail Inline &#40;Assistant Paramétrage de base de données&#41;](../../tools/dta/xml-input-file-sample-with-inline-workload-dta.md).  
  
##  <a name="user-interface-descriptions"></a><a name="UI"></a> Descriptions de l'interface utilisateur  
  
### <a name="tools-menuoptions-page"></a>Menu Outils/page Options  
 Utilisez cette boîte de dialogue pour spécifier les paramètres de configuration généraux de l'Assistant Paramétrage du moteur de base de données.  
  
 **Au démarrage**  
 Indiquez l’action que doit effectuer l’Assistant Paramétrage du moteur de base de données pendant son démarrage : s’ouvrir sans connexion de base de données, afficher une boîte de dialogue **Nouvelle connexion** , afficher une nouvelle session ou charger la dernière session.  
  
 **Modifier la police**  
 Spécifiez la police d'affichage utilisée pour les tables de l'Assistant Paramétrage du moteur de base de données.  
  
 **Nombre d'éléments dans les listes des derniers fichiers utilisés**  
 Indiquez le nombre de sessions ou de fichiers à afficher sous **Sessions récentes** ou **Fichiers récents** dans le menu **Fichier** .  
  
 **Mémoriser mes dernières options de paramétrage**  
 Conserve les options de paramétrage d'une session à l'autre. Option sélectionnée par défaut. Désactivez cette case à cocher pour toujours débuter avec les paramètres par défaut de l'Assistant Paramétrage du moteur de base de données.  
  
 **Demander avant de supprimer définitivement les sessions**  
 Affiche une boîte de dialogue de confirmation avant de supprimer des sessions.  
  
 **Demander avant d'arrêter l'analyse de la session**  
 Affiche une boîte de dialogue de confirmation avant d'arrêter l'analyse d'une charge de travail.  
  
#### <a name="general-tab-options"></a>Options de l'onglet Général  
 Vous devez configurer les champs figurant sous l'onglet **Général** avant de démarrer une session de paramétrage. Vous n'avez pas à modifier les paramètres de l'onglet **Options de paramétrage** avant de lancer une session de paramétrage.  
  
 **Nom de session**  
 Donnez un nom à la session. Le nom de session associe un nom à la session de paramétrage. Vous pouvez faire référence à ce nom pour revenir ultérieurement à la session de paramétrage.  
  
 **File**  
 Spécifiez un fichier de script ou de trace .sql pour une charge de travail. Indiquez le chemin d'accès et le nom de fichier dans la zone de texte correspondante. L'Assistant Paramétrage du moteur de base de données suppose que le fichier de trace de la charge de travail est un fichier de substitution. Pour plus d'informations sur les fichiers de substitution, consultez [Limit Trace File and Table Sizes](../../relational-databases/sql-trace/limit-trace-file-and-table-sizes.md).  
  
 **Table**  
 Spécifiez une table de trace pour une charge de travail. Indiquez le nom complet de la table de trace dans la zone de texte correspondante sous la forme suivante :  
  
```  
database_name.owner_name.table_name  
```  
  
-   Vérifiez que le suivi de trace s'est arrêté avant d'utiliser une table de trace en tant que charge de travail.  
  
-   La table de trace doit se trouver sur le serveur que l'Assistant Paramétrage du moteur de base de données est en train de paramétrer. Si vous créez la table de trace sur un autre serveur, vous devez ensuite la déplacer sur le serveur que l'Assistant Paramétrage du moteur de base de données est en train de paramétrer.  
  
 **Cache du plan**  
 Spécifiez le cache du plan comme charge de travail. Vous pouvez ainsi éviter d'avoir à créer manuellement une charge de travail. L'Assistant Paramétrage du moteur de base de données sélectionne les 1 000 événements principaux à utiliser pour l'analyse.  
  
 **Xml**  
 Ce champ n'apparaît pas sauf si vous importez une requête de charge de travail à partir de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
 Pour importer une requête de charge de travail à partir de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]:  
  
1.  Tapez une requête dans l'éditeur de requête et mettez-la en surbrillance.  
  
2.  Cliquez avec le bouton droit sur la requête en surbrillance et cliquez sur **Analyser la requête dans l’Assistant Paramétrage de base de données**.  
  
 **Rechercher [un fichier ou une table] de charge de travail**  
 Quand **Fichier** ou **Table** est sélectionné en tant que source de charge de travail, utilisez ce bouton d’exploration pour sélectionner la cible.  
  
 **Afficher un aperçu de la charge de travail XML**  
 Affichez une charge de travail XML qui a été importée à partir de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
 **Base de données pour l'analyse de la charge de travail**  
 Spécifiez la première base de données à laquelle l'Assistant Paramétrage du moteur de base de données se connecte lors du paramétrage d'une charge de travail. Dès que le paramétrage commence, l'Assistant Paramétrage du moteur de base de données se connecte aux bases de données spécifiées par les instructions `USE DATABASE` contenues dans la charge de travail.  
  
 **Sélectionnez les bases de données et les tables à analyser**  
 Spécifiez les bases de données et les tables à paramétrer. Pour sélectionner toutes les bases de données, activez la case à cocher de l'en-tête de la colonne **Nom** . Pour sélectionner seulement certaines bases de données, activez la case à cocher en regard du nom de chaque base de données à paramétrer. Par défaut, toutes les tables des bases de données sélectionnées sont automatiquement incluses dans la session de paramétrage. Pour exclure certaines tables, cliquez sur la flèche de la colonne **Tables sélectionnées** , puis désactivez la case à cocher en regard des tables que vous ne voulez pas paramétrer.  
  
 Flèche de déroulement **Tables sélectionnées**  
 Développe la liste des tables pour vous permettre de sélectionner chacune des tables à paramétrer.  
  
 **Enregistrer le journal de paramétrage**  
 Créez un journal et enregistrez les erreurs pendant la session de paramétrage.  
  
> [!NOTE]  
>  L'Assistant Paramétrage du moteur de base de données ne met pas automatiquement à jour les informations sur les lignes pour les tables qui sont affichées sous l'onglet **Général** . Il se base à la place sur les métadonnées de la base de données. Si vous pensez que les informations sur les lignes ne sont plus à jour, exécutez la commande DBCC UPDATEUSAGE pour les objets concernés.  
  
##### <a name="tuning-tab-options"></a>Options de l'onglet Paramétrage  
 Utilisez l'onglet **Options de paramétrage** pour modifier les paramètres par défaut des options de paramétrage générales. Vous n'avez pas à modifier les paramètres de l'onglet **Options de paramétrage** avant de lancer une session de paramétrage.  
  
 **Limiter la durée du paramétrage**  
 Limite la durée de la session de paramétrage actuelle. L'augmentation de la durée du paramétrage améliore la qualité des recommandations. Pour obtenir des recommandations optimales, n'activez pas cette option.  
  
> [!NOTE]  
> [!INCLUDE[ssDE](../../includes/ssde-md.md)] consomme des ressources système pendant l'analyse. Utilisez l'option **Limiter la durée du paramétrage** pour arrêter le paramétrage avant des périodes de lourdes charges de travail anticipées sur le serveur qui est paramétré.  
  
 **Options avancées**  
 Utilisez la boîte de dialogue **Options de paramétrage avancées** pour configurer l’espace maximal, le nombre de colonnes clés maximal et les recommandations d’index en ligne.  
  
 **Définir une quantité d'espace max. pour les recommandations (Mo)**  
 Tapez la quantité d'espace maximale pouvant être utilisée par les structures PDS (Physical Design Structures) recommandées par l'Assistant Paramétrage du moteur de base de données.  
  
 Si aucune valeur n'est entrée dans cette zone, l'Assistant Paramétrage du moteur de base de données prend en compte la plus petite limite d'espace suivante :  
  
-   Le triple de la taille actuelle des données brutes, qui inclut la taille totale des segments et les index cluster des tables de la base de données.  
  
-   L'espace libre disponible sur tous les lecteurs de disques connectés, plus la taille des données brutes.  
  
 **Inclure les événements du cache du plan de toutes les bases de données**  
 Spécifiez que les événements du cache du plan de toutes les bases de données sont analysés.  
  
 **Nombre de colonnes max. par index**  
 Spécifiez le nombre maximal de colonnes à inclure dans un index quelconque. La valeur par défaut est 1023.  
  
 **Toutes les recommandations sont hors connexion**  
 Génère les meilleures recommandations possibles, mais sans recommander que les structures PDS (Physical Design Structure) soient créées en ligne.  
  
 **Générer des recommandations en ligne dès que possible**  
 Lorsque vous créez des instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] pour exécuter les recommandations, choisissez des méthodes pouvant être exécutées avec le serveur en ligne et ce, même si une méthode plus rapide est disponible.  
  
 **Générer uniquement des recommandations en ligne**  
 Génère uniquement des recommandations qui permettent au serveur de rester en ligne.  
  
 **Arrêter à**  
 Spécifiez la date et l'heure d'arrêt de l'Assistant Paramétrage du [!INCLUDE[ssDE](../../includes/ssde-md.md)] .  
  
 **Index et vues indexées**  
 Activez cette case à cocher pour inclure des recommandations sur l'ajout d'index cluster, d'index non-cluster et de vues indexées.  
  
 **Vues indexées**  
 Incluez uniquement des recommandations relatives à l'ajout de vues indexées. Les index cluster et non-cluster ne sont pas concernés par les recommandations.  
  
 **Inclure les index filtrés**  
 Incluez des recommandations relatives à l'ajout d'index filtrés. Cette option est disponible si vous sélectionnez l’une de ces structures d’étude de réalisation : **Index et vues indexées**, **Index** ou **Index non-cluster**.  
  
 **Index**  
 Incluez uniquement des recommandations relatives à l'ajout d'index cluster et non-cluster. Les vues indexées ne sont pas concernées par les recommandations.  
  
 **Index non cluster**  
 Incluez des recommandations pour les index non-cluster uniquement. Les index cluster et les vues indexées ne sont pas concernés par les recommandations.  
  
 **Évaluer l'utilisation des structures PDS (Physical Design Structures) existantes uniquement**  
 Évaluez l'efficacité des index actuels, mais n'incluez pas de recommandation pour des index ou des vues indexées supplémentaires.  
  
 **Aucun partitionnement**  
 Ne recommandez pas le partitionnement.  
  
 **Partitionnement complet**  
 Incluez des recommandations pour le partitionnement.  
  
 **Partitionnement aligné**  
 Les nouveaux partitionnements recommandés sont alignés pour faciliter la gestion des partitionnements.  
  
 **Ne pas conserver de structures PDS (Physical Design Structures) existantes**  
 Recommandez la suppression des index, vues et partitionnements existants inutiles. Si une structure PDS existante est utile à la charge de travail, l'Assistant Paramétrage du [!INCLUDE[ssDE](../../includes/ssde-md.md)] ne recommande pas sa suppression.  
  
 **Conserver les index uniquement**  
 Conservez tous les index existants, mais recommandez la suppression des vues indexées et des partitionnements inutiles.  
  
 **Conserver toutes les structures PDS (Physical Design Structures) existantes**  
 Conservez tous les index, vues indexées et partitionnements existants.  
  
 **Conserver les index cluster uniquement**  
 Conservez tous les index cluster existants, mais recommandez la suppression des vues indexées, des partitionnements et des index non-cluster inutiles.  
  
 **Conserver le partitionnement aligné**  
 Conservez les structures de partitionnement qui sont actuellement alignées, mais recommandez la suppression des vues indexées, des index et des partitionnements non alignés inutiles. Tout partitionnement supplémentaire recommandé sera aligné sur le schéma de partitionnement actuel.  
  
###### <a name="progress-tab-options"></a>Options de l'onglet Progression  
 L'onglet **Progression** de l'Assistant Paramétrage du moteur de base de données apparaît lorsque l'Assistant commence l'analyse d'une charge de travail.  
  
 Si vous souhaitez arrêter la session de paramétrage après son démarrage, choisissez une des options suivantes dans le menu **Actions** :  
  
-   **Arrêter l’analyse (avec recommandations)** arrête la session de paramétrage et vous demande de spécifier si l’Assistant Paramétrage du moteur de base de données doit générer des recommandations sur la base de l’analyse réalisée jusqu’à ce point.  
  
-   **Arrêter l'analyse** arrête la session de paramétrage sans générer de recommandation.  
  
 **Progression du paramétrage**  
 Indique l'état actuel de la progression. Cette option contient le nombre d'actions exécutées ainsi que le nombre d'erreurs, de réussites et de messages d'avertissement reçus.  
  
 **Détails**  
 Contient une icône indiquant l'état.  
  
 **Action**  
 Affiche les étapes en cours d'exécution.  
  
 **État**  
 Affiche l'état de l'étape de l'action.  
  
 **Message**  
 Contient tous les messages éventuellement renvoyés par les étapes de l'action.  
  
 **Journal des paramétrages**  
 Contient des informations relatives à la session de paramétrage actuelle. Pour imprimer le journal, cliquez avec le bouton droit sur celui-ci, puis cliquez sur **Imprimer**.  
  
## <a name="see-also"></a>Voir aussi  
 [Afficher et utiliser la sortie de l'Assistant Paramétrage du moteur de base de données](../../relational-databases/performance/view-and-work-with-the-output-from-the-database-engine-tuning-advisor.md)   
 [Utilitaire dta](../../tools/dta/dta-utility.md)    
 [Tutoriel : Assistant Paramétrage du moteur de base de données](../../tools/dta/tutorial-database-engine-tuning-advisor.md)
