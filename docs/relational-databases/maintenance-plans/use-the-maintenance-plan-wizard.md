---
title: Utiliser l’Assistant Plan de maintenance | Microsoft Docs
description: Découvrez comment créer un plan de maintenance pour un ou plusieurs serveurs à l’aide de l’Assistant Plan de maintenance dans SQL Server.
ms.date: 06/20/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.custom: ''
ms.technology: supportability
ms.topic: conceptual
f1_keywords:
- sql13.ag.maintwiz.integrity.f1
- sql13.ag.maintwiz.order.f1
- sql13.ag.maintwiz.report.f1
- sql13.ag.maintwiz.updatestats.f1
- sql13.ag.maintwiz.indexdefrag.f1
- sql13.ag.maintwiz.progress.f1
- sql13.ag.maintwiz.maintcleanup.f1
- sql13.ag.maintwiz.backupfull.f1
- sql13.ag.maintwiz.task.f1
- sql13.ag.maintwiz.server.f1
- sql13.ag.maintwiz.shrinkdb.f1
- sql13.ag.maintwiz.execagentjob.f1
- sql13.ag.maintwiz.summary.f1
- sql13.ag.maintwiz.welcome.f1
- sql13.ag.maintwiz.planprop.f1
- sql13.ag.maintwiz.reindex.f1
- sql13.ag.maintwiz.histcleanup.f1
- sql13.ag.maintwiz.backuplog.f1
- sql13.ag.maintwiz.backupdiff.f1
helpviewer_keywords:
- Maintenance Plan Wizard
- Database Maintenance Plan Wizard
- Database Maintenance Plan Wizard, starting
ms.assetid: db65c726-9892-480c-873b-3af29afcee44
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 68c3ff265fbf1bf56fb2d7f0ba42aa6f76dd72b2
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/25/2021
ms.locfileid: "98766244"
---
# <a name="use-the-maintenance-plan-wizard"></a>Utiliser l'Assistant Plan de maintenance
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Cette rubrique explique comment créer un plan de maintenance pour un ou plusieurs serveurs à l’aide de l’Assistant Plan de maintenance dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]. L’Assistant Plan de maintenance crée un plan de maintenance que [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent peut exécuter régulièrement. Vous pouvez ainsi réaliser, en fonction d'intervalles spécifiés, diverses tâches d'administration de base de données, notamment des sauvegardes, l'exécution de contrôles d'intégrité de la base de données ou les mises à jour des statistiques de la base de données.  
    
 
##  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitations et restrictions  
  
-   Pour créer un plan de maintenance multiserveurs, vous devez configurer un environnement multiserveurs avec un serveur maître et un ou plusieurs serveurs cibles. Les plans de maintenance multiserveurs doivent être créés et conservés sur le serveur maître. Vous pouvez afficher les plans sur les serveurs cibles.   

-   Les membres du rôle **db_ssisadmin** et du rôle **dc_admin** peuvent être en mesure d’élever leurs privilèges à **sysadmin**. Cette élévation de privilège est possible, car ces rôles peuvent modifier les packages [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , qui sont exécutables par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] avec le contexte de sécurité **sysadmin** de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent. 

Pour empêcher cette élévation de privilège durant l’exécution de plans de maintenance, de jeux d’éléments de collecte de données et d’autres packages [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , configurez les travaux de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent qui exécutent des packages de façon à utiliser un compte proxy doté de privilèges limités ou ajoutez uniquement des membres **sysadmin** aux rôles **db_ssisadmin** et **dc_admin** .  

##  <a name="prerequisites"></a><a name="Prerequisite"></a> Conditions préalables 
Vous devez activer [Agent XPs (option de configuration de serveur)](../../database-engine/configure-windows/agent-xps-server-configuration-option.md).
  
  
##  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Pour créer ou gérer des plans de maintenance, vous devez être membre du rôle serveur fixe **sysadmin** . L'Explorateur d'objets affiche uniquement le nœud **Plans de maintenance** pour les utilisateurs membres du rôle serveur fixe **sysadmin** .  
  
##  <a name="use-the-maintenance-plan-wizard"></a><a name="SSMSProcedure"></a> Utiliser l'Assistant Plan de maintenance  
  
**Démarrer l'Assistant** 

  
1.  Développez le serveur pour lequel vous souhaitez créer votre plan de gestion.  
  
2.  Développez le dossier **Gestion** .  
  
3.  Cliquez avec le bouton droit sur le dossier **Plans de maintenance** et sélectionnez **Assistant Plan de maintenance**.  
  
4.  Dans la page **Assistant Plan de maintenance SQL Server** , cliquez sur **Suivant**.  
  
5.  Dans la page **Sélectionner les propriétés de plan** , procédez comme suit :  
  
    1.  Dans la zone **Nom** , entrez le nom du plan de maintenance que vous allez créer.  
  
    2.  Dans la zone **Description** , décrivez brièvement votre plan de maintenance.  
  
    3.  Dans la liste **Exécuter en tant que** , spécifiez les informations d'authentification utilisées par Microsoft SQL Server Agent lors de l'exécution du plan de maintenance.  
  
    4.  Sélectionnez **Planification distincte pour chaque tâche** ou **Planification unique pour la totalité du plan ou pas de planification** pour spécifier la planification périodique du plan de maintenance.  
  
        > **REMARQUE :** Si vous sélectionnez **Planification distincte pour chaque tâche**, vous devez suivre les étapes de la section **e.** ci-dessous pour chaque tâche de votre plan de maintenance.  
  
    5.  Si vous avez sélectionné **Planification unique pour la totalité du plan ou pas de planification** sous **Planification**, cliquez sur **Modifier**.  
  
        1.  Dans la boîte de dialogue **Nouvelle planification du travail**, dans la zone **Nom**, entrez le nom de la planification du travail.  
  
        2.  Dans la liste **Type de planification** , sélectionnez le type de la planification :  
  
            -   **Lancer automatiquement au démarrage de SQL Server Agent**  
  
            -   **Démarrer dès que les processeurs sont inactifs**  
  
            -   **Périodique**. Il s'agit de la sélection par défaut.  
  
            -   **Une fois**  
  
        3.  Activez ou désactivez la case à cocher **Activé** pour activer ou désactiver la planification.  
  
        4.  Si vous sélectionnez **Périodique**:  
  
            1.  Sous **Fréquence**, dans la liste **Périodicité** , spécifiez la fréquence d'occurrence :  
  
                -   Si vous sélectionnez **Quotidienne**, dans la zone **Répéter toutes les** , entrez la fréquence de répétition du travail de planification en jours.  
  
                -   Si vous sélectionnez **Hebdomadaire**, dans la zone **Répéter toutes les** , entrez la fréquence de répétition du travail de planification en semaines. Sélectionnez les jours de la semaine pendant lesquels la planification du travail est exécutée.  
  
                -   Si vous sélectionnez **Mensuelle**, sélectionnez **Jour** ou **Le**.  
  
                    -   Si vous sélectionnez **Jour**, entrez la date du mois à laquelle vous souhaitez que la planification du travail s'exécute, ainsi que la fréquence de répétition de la planification du travail en mois. Par exemple, si vous souhaitez que la planification du travail s’exécute le 15 du mois un mois sur deux, sélectionnez **Jour**, puis entrez « 15 » dans la première zone et « 2 » dans la deuxième zone. Notez que le nombre maximum autorisé dans la deuxième zone est « 99 ».  
  
                    -   Si vous sélectionnez **Le**, sélectionnez le jour spécifique de la semaine et du mois pendant lequel vous voulez que la planification du travail s'exécute et la fréquence à laquelle la planification du travail doit se répéter en mois. Par exemple, si vous souhaitez que la planification du travail s’exécute le dernier jour de la semaine un mois sur deux, sélectionnez **Jour**, puis **dernier** dans la première liste, **jour ouvrable** dans la deuxième liste et « 2 » dans la dernière zone. Vous pouvez également sélectionner **premier**, **deuxième**, **troisième** ou **quatrième**, ainsi que des jours de la semaine spécifiques (par exemple, dimanche ou mercredi) dans les deux premières listes. Notez que le nombre maximum autorisé dans la dernière zone est « 99 ».  
  
            2.  Sous **Fréquence quotidienne**, spécifiez la fréquence à laquelle la planification du travail se répète le jour de son exécution :  
  
                -   Si vous sélectionnez **Une fois à**, entrez l'heure spécifique à laquelle la planification du travail doit s'exécuter dans la zone **Une fois à** . Entrez l'heure, les minutes et les secondes du jour, ainsi que AM ou PM.  
  
                -   Si vous sélectionnez **Toutes les**, spécifiez la fréquence à laquelle la planification du travail s'exécute pendant la journée choisie sous **Fréquence**. Par exemple, si vous souhaitez que la planification du travail se répète toutes les 2 heures le jour d’exécution de la planification du travail, sélectionnez **Toutes les**, entrez « 2 » dans la première zone, puis sélectionnez **heure(s)** dans la liste. Dans cette liste, vous pouvez également sélectionner **minute(s)** et **seconde(s)** . Notez que le nombre maximum autorisé dans la première zone est « 100 ».  
  
                     Dans la zone **Début** , entrez l'heure à laquelle l'exécution de la planification du travail doit démarrer. Dans la zone **Fin** , entrez l'heure à laquelle la planification du travail doit s'arrêter. Entrez l'heure, les minutes et les secondes du jour, ainsi que AM ou PM.  
  
            3.  Sous **Durée**, dans la zone **Date de début**, entrez la date à laquelle vous souhaitez que l'exécution de la planification du travail commence. Sélectionnez **Date de fin** ou **Aucune date de fin** pour indiquer à quel moment l'exécution de la planification du travail doit s'arrêter. Si vous sélectionnez **Date de fin**, entrez la date à laquelle l'exécution de la planification du travail doit s'arrêter.  
  
        5.  Si vous sélectionnez **Une fois** sous **Une seule occurrence**, dans la zone **Date** , entrez la date à laquelle la planification du travail est exécutée. Dans la zone **Heure** , entrez l'heure à laquelle la planification du travail sera exécutée. Entrez l'heure, les minutes et les secondes du jour, ainsi que AM ou PM.  
  
        6.  Sous **Résumé**, dans **Description**, vérifiez que tous les paramètres de planification du travail sont corrects.  
  
        7.  Cliquez sur **OK**.  
  
    6.  Cliquez sur **Suivant**.  
  
6.  Sur la page **Sélectionner des serveurs cibles** , sélectionnez les serveurs sur lesquels vous souhaitez exécuter le plan de maintenance. Cette page est uniquement visible sur des instances [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui sont configurées comme serveurs maîtres.  
  
    > **REMARQUE :** Pour créer un plan de maintenance multiserveurs, vous devez configurer un environnement multiserveurs contenant un serveur maître et un ou plusieurs serveurs cibles, et le serveur local doit être configuré comme serveur maître. Dans les environnements multiserveurs, cette page affiche le serveur maître **(local)** et tous les serveurs cibles correspondants.  
  
7.  Dans la page **Sélectionner des tâches de maintenance** , sélectionnez une ou plusieurs tâches de maintenance à ajouter au plan. Lorsque vous avez sélectionné toutes les tâches nécessaires, cliquez sur **Suivant**.  
  
    > **REMARQUE :** Les tâches que vous sélectionnez ici déterminent les pages que vous devrez remplir après la page **Sélectionner l'ordre des tâches de maintenance** ci-dessous.  
  
8.  Dans la page **Sélectionner l’ordre des tâches de maintenance**, sélectionnez une tâche et cliquez sur **Monter…** ou **Descendre…** pour modifier son ordre d’exécution. Lorsque vous avez terminé, ou si vous êtes satisfait de l'ordre actuel de tâches, cliquez sur **Suivant**.  
  
    > **REMARQUE :** Si vous avez sélectionné **Planification distincte pour chaque tâche** dans la page **Sélectionner les propriétés de plan** ci-dessus, vous ne pouvez pas modifier l'ordre des tâches de maintenance sur cette page.  
  
## <a name="define-database-check-integrity-checkdb"></a>Définir la vérification de l’intégrité de la base de données (CHECKDB)  
  
 Sur la page **Définir la tâche Vérifier l'intégrité de la base de données** , sélectionnez chaque base de données où l'allocation et l'intégrité de la structure des tables utilisateur/système et des index seront vérifiées. En exécutant l’instruction `DBCC CHECKDB`[!INCLUDE[tsql](../../includes/tsql-md.md)] , cette tâche permet de signaler tous les problèmes d’intégrité de la base de données, qui pourront ensuite être corrigés par l’administrateur système ou le propriétaire de la base de données. Pour plus d’informations, consultez [DBCC CHECKDB &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md)Quand vous avez terminé, cliquez sur **Suivant**.  
  
Les options supplémentaires suivantes sont disponibles sur cette page.  
  
 Liste **Bases de données**  
 Spécifie les bases de données faisant l'objet de cette tâche.  
  
 -  **Toutes les bases de données**  
  
Génère un plan de maintenance qui exécute cette tâche sur toutes les bases de données [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], à l'exception de **tempdb**.  
  
**Bases de données système**  
  
  - Génère un plan de maintenance qui exécute cette tâche sur les bases de données système de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , à l’exception de **tempdb** et des bases de données créées par l’utilisateur.  
  
 **Toutes les bases de données utilisateur (autre que master, model et msdb)**  
  
 - Génère un plan de maintenance qui exécute cette tâche sur toutes les bases de données créées par l'utilisateur. Aucune tâche de maintenance n'est exécutée sur les bases de données système de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 **Ces bases de données**  
  
  - Génère un plan de maintenance qui exécute cette tâche uniquement sur les bases de données sélectionnées. Si vous choisissez cette option, sélectionnez au moins une base de données.  
  
Case à cocher **Inclure les index**  
 - Vérifie l'intégrité de toutes les pages d'index ainsi que des pages de données des tables.  
  
**Physique uniquement**  
 - Limite la vérification à l’intégrité de la structure physique de la page, aux en-têtes d’enregistrement et à l’intégrité de la cohérence d’allocation de la base de données. L’utilisation de cette option étant susceptible de réduire considérablement la durée d’exécution de DBCC CHECKDB sur des bases de données volumineuses, elle est recommandée pour une utilisation fréquente sur des systèmes de production.  
  
**Tablock**  
 - Génère des verrouillages par DBCC CHECKDB au lieu d'utiliser un instantané de base de données interne. Cette opération comprend un verrou exclusif sur la base de données. L’utilisation de cette option contribue à accélérer l’exécution de DBCC CHECKDB sur une base de données soumise à une charge importante, tout en diminuant la concurrence disponible dans cette dernière pendant l’exécution de DBCC CHECKDB.  
  
## <a name="define-database-shrink-tasks"></a>Définir les tâches de réduction de la base de données  
  
1.  Dans la page **Définir la tâche Réduire la base de données** , créez une tâche qui tente de réduire la taille des bases de données sélectionnées à l'aide de l'instruction `DBCC SHRINKDATABASE` , avec l'option `NOTRUNCATE` ou `TRUNCATEONLY` . Pour plus d’informations, consultez [DBCC SHRINKDATABASE &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-shrinkdatabase-transact-sql.md). Lorsque vous avez terminé, cliquez sur **Suivant**.  
  
    > **Avertissement !!!** Les données déplacées pour réduire un fichier peuvent être dispersées à n’importe quel emplacement disponible dans le fichier. Cela provoque la fragmentation de l'index et peut ralentir les performances des requêtes qui recherchent une plage de l'index. Pour éliminer la fragmentation, reconstruisez les index dans le fichier après réduction.  
  
     Les options supplémentaires suivantes sont disponibles sur cette page.  
  
     Liste **Bases de données**  
     Spécifie les bases de données faisant l'objet de cette tâche. Reportez-vous à l'étape 9 ci-dessus pour plus d'informations sur les options disponibles dans cette liste.  
  
     Zone **Réduire la base de données quand elle excède**  
     Indiquez la taille de base de données (en mégaoctets) qui doit être atteinte pour que l'exécution de la tâche soit déclenchée.  
  
     Zone **Quantité d’espace disponible restant après réduction**  
     Arrête la réduction lorsque les fichiers de base de données présentent un espace libre équivalant à la taille spécifiée (en pourcentage).  
  
     **Conserver l'espace libéré dans les fichiers de base de données**  
     La base de données est condensée en pages contiguës, mais les pages ne sont pas désallouées et les fichiers de base de données ne sont pas réduits. Utilisez cette option si vous prévoyez une nouvelle expansion de la base de données et que vous ne souhaitez pas réallouer de l'espace. Avec cette option, la taille des fichiers de base de données n'est pas réduite au maximum. L'option NOTRUNCATE est utilisée.  
  
     **Retourner l'espace libéré au système d'exploitation**  
     La base de données est condensée en pages contiguës, et les pages sont remises à la disposition du système d'exploitation afin d'être utilisées par d'autres programmes. L'option TRUNCATEONLY est utilisée. Il s'agit de l'option par défaut.  
  
## <a name="define-the-index-tasks"></a>Définir les tâches d'index  
  
1.  Dans la page **Définir la tâche Réorganiser l'index** , sélectionnez le serveur ou les serveurs où vous déplacerez les pages d'index dans un ordre de recherche plus efficace. La tâche utilise l'instruction `ALTER INDEX ... REORGANIZE`. Pour plus d’informations, consultez [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md). Lorsque vous avez terminé, cliquez sur **Suivant**.  
  
     Les options supplémentaires suivantes sont disponibles sur cette page.  
  
     Liste **Bases de données**  
     Spécifie les bases de données faisant l'objet de cette tâche. Reportez-vous à l'étape 9 ci-dessus pour plus d'informations sur les options disponibles dans cette liste.  
  
     Liste **Objet**  
     Limite la liste **Sélection** pour afficher des tables, des vues, ou les deux. Cette liste est disponible uniquement si une seule base de données est sélectionnée dans la liste **Bases de données** ci-dessus.  
  
     Liste **Sélection**  
     Spécifie les tables ou les index faisant l'objet de cette tâche. Non disponible quand **Tables et vues** est sélectionné dans la zone Objet.  
  
     Case à cocher **Compacter les objets importants**  
     Annule l'allocation de l'espace pour les tables et les vues si possible. Cette option utilise `ALTER INDEX ... LOB_COMPACTION = ON`.  
  
2.  Dans la page **Définir la tâche Reconstruire l’index** , sélectionnez chaque base de données où vous allez recréer plusieurs index. La tâche utilise l'instruction `ALTER INDEX ... REBUILD PARTITION`. Pour plus d’informations, consultez [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md).) Quand vous avez terminé, cliquez sur **Suivant**.  
  
     Les options supplémentaires suivantes sont disponibles sur cette page.  
  
     Liste **Bases de données**  
     Spécifie les bases de données faisant l'objet de cette tâche. Reportez-vous à l'étape 9 ci-dessus pour plus d'informations sur les options disponibles dans cette liste.  
  
     Liste **Objet**  
     Limite la liste **Sélection** pour afficher des tables, des vues, ou les deux. Cette liste est disponible uniquement si une seule base de données est sélectionnée dans la liste **Bases de données** ci-dessus.  
  
     Liste **Sélection**  
     Spécifie les tables ou les index faisant l'objet de cette tâche. Non disponible quand **Tables et vues** est sélectionné dans la zone Objet.  
  
     Zone **Options relatives à l’espace libre**  
     Présente les options permettant d'appliquer le facteur de remplissage des index et des tables.  
  
     **Espace disponible par page par défaut**  
     Réorganise les pages avec la quantité d'espace disponible par défaut. Cela supprime les index sur les tables de la base de données et les recrée avec le facteur de remplissage spécifié lors de la création des index. Il s'agit de l'option par défaut.  
  
     Zone **Modifier l’espace disponible par page de**  
     Provoque la suppression des index des tables de la base de données et leur recréation avec un nouveau facteur de remplissage calculé automatiquement, la quantité d'espace libre spécifiée étant réservée dans les pages d'index. Plus le pourcentage est élevé, plus il y a d'espace libre réservé dans les pages d'index et plus l'index croît. Les valeurs valides sont comprises entre 0 et 100. Utilise l'option `FILLFACTOR` .  
  
     Zone **Options avancées**  
     Présente les options supplémentaires pour trier les index et pour la réindexation.  
  
     Case à cocher **Trier les résultats dans tempdb**  
     Utilise l'option `SORT_IN_TEMPDB` , qui détermine l'emplacement où les résultats de tri intermédiaires, générés lors de la création de l'index, sont temporairement stockés. Si aucune opération de tri n'est requise ou si le tri peut être effectué dans la mémoire, l'option `SORT_IN_TEMPDB` est ignorée.  
  
     Case à cocher **Index de remplissage**  
     Utilise l'option `PAD_INDEX` .  
  
     Case à cocher **Conserver l’index en ligne lors de la réindexation**  
     Utilise l'option `ONLINE` qui permet aux utilisateurs d'accéder à la table sous-jacente ou aux données d'index cluster, ainsi qu'à tous les index non cluster associés au cours des opérations d'index. La sélection de cette option active les options supplémentaires pour reconstruire les index qui n’autorisent pas les reconstructions en ligne : **Ne pas reconstruire les index** et **Reconstruire des index en mode hors connexion**.  
  
     Cette option active également la faible priorité utilisée, qui utilise l’option `WAIT_AT_LOW_PRIORITY` . Les opérations de reconstruction de l’index en ligne doivent attendre les verrous de faible priorité pendant `MAX_DURATION` minutes, pour laissant les autres opérations se poursuivre pendant la mise en attente de l’opération de construction de l’index en ligne.  
  
    > **REMARQUE :** Les opérations d'index en ligne ne sont pas disponibles dans toutes les éditions de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]. Pour plus d’informations, consultez [Fonctionnalités prises en charge par les éditions de SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).  
  
     Case à cocher **MAXDOP**  
     Remplace l’option de configuration Degré maximal de parallélisme de sp_configure pour DBCC CHECKDB. Pour plus d’informations, consultez [DBCC CHECKIDENT &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md)  
  
#### <a name="define-the-update-statistics-task"></a>Définir la tâche de mise à jour des statistiques  
  
1.  Dans la page **Définir la tâche Mettre à jour les statistiques** , sélectionnez chaque base de données pour laquelle les statistiques de table et d'index seront mises à jour. La tâche utilise l'instruction `UPDATE STATISTICS`. Pour plus d’informations, consultez [UPDATE STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/update-statistics-transact-sql.md). Quand vous avez terminé, cliquez sur **Suivant**.  
  
     Les options supplémentaires suivantes sont disponibles sur cette page.  
  
     Liste **Bases de données**  
     Spécifie les bases de données faisant l'objet de cette tâche. Reportez-vous à l'étape 9 ci-dessus pour plus d'informations sur les options disponibles dans cette liste.  
  
     Liste **Objet**  
     Limite la liste **Sélection** pour afficher des tables, des vues, ou les deux. Cette liste est disponible uniquement si une seule base de données est sélectionnée dans la liste **Bases de données** ci-dessus.  
  
     Liste **Sélection**  
     Spécifie les tables ou les index faisant l'objet de cette tâche. Non disponible quand **Tables et vues** est sélectionné dans la zone Objet.  
  
     **Toutes les statistiques existantes**  
     Met à jour les statistiques pour les colonnes et les index.  
  
     **Statistiques de colonnes uniquement**  
     Met à jour les statistiques de colonnes uniquement. Utilise l'option `WITH COLUMNS` .  
  
     **Statistiques d'index uniquement**  
     Met à jour les statistiques d'index uniquement. Utilise l'option `WITH INDEX` .  
  
     **Type d'analyse**  
     Type d'analyse destinée à la collecte des statistiques mises à jour.  
  
     **Analyse complète**  
     Lit toutes les lignes de la table ou de la vue pour rassembler les statistiques.  
  
     **Exemple par**  
     Spécifie le pourcentage de la table ou de la vue indexée, ou le nombre de lignes à échantillonner lors de la collecte des statistiques de tables ou vues volumineuses.  
  
#### <a name="define-the-history-cleanup-task"></a>Définir la tâche Nettoyage de l'historique  
  
1.  Dans la page **Définir la tâche Nettoyage de l'historique** , sélectionnez chaque base de données pour laquelle vous souhaitez ignorer l'ancien historique des tâches. Cette tâche utilise les instructions `EXEC sp_purge_jobhistory`, `EXEC sp_maintplan_delete_log`et `EXEC sp_delete_backuphistory` pour supprimer les informations d'historique des tables **msdb** . Une fois que vous avez terminé, cliquez sur **Suivant**.  
  
     Les options supplémentaires suivantes sont disponibles sur cette page.  
  
     **Sélectionner les données d'historique à supprimer**  
     Choisissez le type de données de tâche à effacer.  
  
     **Sauvegarder et restaurer l'historique**  
     La conservation des enregistrements indiquant à quel moment des sauvegardes récentes ont été créées peut aider [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à créer un plan de récupération lorsque vous souhaitez restaurer une base de données. La période de conservation doit être au moins équivalente à la fréquence des sauvegardes complètes de bases de données.  
  
     **Historique des travaux de SQL Server Agent**  
     Cet historique vous permet de résoudre les problèmes des travaux ayant échoué ou de déterminer la raison pour laquelle des actions se sont produites.  
  
     **Historique du plan de maintenance**  
     Cet historique vous permet de résoudre les problèmes des travaux de plan de maintenance ayant échoué ou de déterminer la raison pour laquelle des actions se sont produites.  
  
     **Supprimer les données d'historique antérieures à**  
     Permet de spécifier l'ancienneté des éléments à supprimer. Vous pouvez spécifier **Heure(s)** , **Jour(s)** , **Semaine(s)** (valeur par défaut), **Mois** ou **Année(s)** .  
  
#### <a name="define-the-execute-agent-job-task"></a>Définir la tâche Exécuter le travail de l'agent  
  
1.  Dans la page **Définir la tâche Exécuter le travail de l'agent** , sous **Travaux de l'Agent SQL Server disponibles**, choisissez le travail ou les travaux à exécuter. Cette option n'est pas disponible si vous ne possédez aucun travail SQL Server Agent. La tâche utilise l'instruction `EXEC sp_start_job`. Pour plus d’informations, consultez [sp_start_job &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-start-job-transact-sql.md). Quand vous avez terminé, cliquez sur **Suivant**.  
  
#### <a name="define-backup-tasks"></a>Définir les tâches de sauvegarde  
  
1.  Dans la page **Définir la tâche Sauvegarder la base de données (complète)** , sélectionnez chaque base de données sur laquelle effectuer une sauvegarde complète. La tâche utilise l'instruction `BACKUP DATABASE`. Pour plus d’informations, consultez [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md). Une fois que vous avez terminé, cliquez sur **Suivant**.  
  
     Les options supplémentaires suivantes sont disponibles sur cette page.  
  
     Liste **Type de sauvegarde**  
     Affiche le type de sauvegarde à effectuer. En lecture seule.  
  
     Liste **Bases de données**  
     Spécifie les bases de données faisant l'objet de cette tâche. Reportez-vous à l'étape 9 ci-dessus pour plus d'informations sur les options disponibles dans cette liste.  
  
     **Composant de sauvegarde**  
     Sélectionnez **Base de données** pour sauvegarder la totalité de la base de données. Sélectionnez **Fichier et groupes de fichiers** pour sauvegarder seulement une partie de la base de données. Spécifiez ensuite le nom du fichier ou du groupe de fichiers. Si vous avez sélectionné plusieurs bases de données dans la zone **Base de données** , ne spécifiez que **Bases de données** pour **Composant de sauvegarde**. Pour exécuter des sauvegardes de fichiers ou de groupes de fichiers, créez une tâche pour chaque base de données. Ces options sont disponibles uniquement si une seule base de données est sélectionnée dans la liste **Bases de données** ci-dessus.  
  
     Case à cocher **Expiration du jeu de sauvegarde**  
     Spécifie la date à laquelle le jeu de sauvegarde de cette sauvegarde peut être écrasé. Sélectionnez **Après** et entrez le nombre de jours avant l'expiration ou sélectionnez **Le** et entrez une date d'expiration. Cette option est désactivée si **URL** est sélectionné en tant que destination de sauvegarde.  
  
     **Sauvegarde sur**  
     Spécifie le support sur lequel enregistrer la base de données. Sélectionnez **Disque**, **Bande** ou **URL**. Seuls les périphériques à bande connectés à l'ordinateur sur lequel figure la base de données sont disponibles.  
  
     **Sauvegarder les bases de données sur un ou plusieurs fichiers**  
     Cliquez sur **Ajouter** pour ouvrir la boîte de dialogue **Sélectionner la destination de la sauvegarde** . Cette option est désactivée si URL est sélectionné en tant que destination de sauvegarde.  
  
     Cliquez sur **Supprimer** pour supprimer un fichier de la zone.  
  
     Cliquez sur **Contenu** pour lire l'en-tête de fichier et afficher le contenu de sauvegarde actuel du fichier.  
  
     Boîte de dialogue **Sélectionner la destination de la sauvegarde**  
     Sélectionnez le fichier, le lecteur de bande ou l'unité de sauvegarde de destination de la sauvegarde. Cette option est désactivée si URL est sélectionné en tant que destination de sauvegarde.  
  
     Liste **Si des fichiers de sauvegarde existent**  
     Spécifiez la manière dont seront traitées les sauvegardes existantes. Sélectionnez **Ajouter** pour ajouter les nouvelles sauvegardes à la suite de celles déjà présentes dans le fichier ou sur la bande. Sélectionnez **Remplacer** pour supprimer l'ancien contenu du fichier ou de la bande en le remplaçant par la nouvelle sauvegarde.  
  
     **Créer un fichier de sauvegarde pour chaque base de données**  
     Crée un fichier de sauvegarde à l'emplacement spécifié dans la zone Dossier. Un fichier unique est créé pour chaque base de données sélectionnée. Cette option est désactivée si URL est sélectionné en tant que destination de sauvegarde.  
  
     Case à cocher **Créer un sous-répertoire pour chaque base de données**  
     Crée un sous-répertoire pour chaque base de données sauvegardée dans le cadre du plan de maintenance, dans le répertoire de disque spécifié contenant la sauvegarde de la base de données.  
  
    > **IMPORTANT !** Le sous-répertoire hérite les autorisations du répertoire parent. Limitez les autorisations pour éviter les accès non autorisés.  
  
     Zone **Dossier**  
     Spécifiez le dossier dans lequel seront placés les fichiers de base de données créés automatiquement. Cette option est désactivée si URL est sélectionné en tant que destination de sauvegarde.  
  
     **Informations d'identification SQL**  
     Sélectionnez les informations d’identification SQL utilisées pour l’authentification au stockage Azure. Si vous n'avez pas d'informations d'identification SQL, cliquez sur le bouton **Créer** pour créer de nouvelles informations d'identification SQL.  
  
    > **IMPORTANT !** La boîte de dialogue qui s'ouvre lorsque vous cliquez sur **Créer** requiert un certificat de gestion ou le profil de publication de l'abonnement. Si vous n'avez pas accès au certificat de gestion ou au profil de publication, vous pouvez créer des informations d'identification SQL en spécifiant le nom du compte de stockage et les informations de clé d'accès à l'aide de Transact-SQL ou de SQL Server Management Studio. Consultez l’exemple de code dans la rubrique [Créer des informations d’identification](../../relational-databases/backup-restore/sql-server-backup-to-url.md#credential) pour créer des informations d’identification à l’aide de Transact-SQL. Vous pouvez également utiliser SQL Server Management Studio, depuis l'instance du moteur de base de données, et cliquer avec le bouton droit sur **Sécurité**, puis sélectionner **Nouveau**, puis **Informations d'identification**. Spécifiez le nom du compte de stockage pour **Identité** et la clé d'accès dans le champ **Mot de passe** .  
  
     **Conteneur de stockage Windows Azure**  
     Spécifiez le nom du conteneur de stockage Azure  
  
     **Préfixe d'URL**  
     Est généré automatiquement à partir des informations du compte de stockage contenues dans les informations d'identification SQL, et du nom du conteneur de stockage Windows Azure que vous avez spécifié. Nous vous recommandons de ne pas modifier les informations de ce champ, sauf si vous utilisez un domaine qui utilise un format autre que **\<storage account>.blob.core.windows.net**.  
  
     Zone **Extension du fichier de sauvegarde**  
     Spécifiez l'extension à utiliser pour les fichiers de sauvegarde. La valeur par défaut est .bak.  
  
     Case à cocher **Vérifier l’intégrité de la sauvegarde**  
     Vérifie si le jeu de sauvegarde est complet et que tous les volumes sont lisibles.  
  
     Case à cocher **Effectuer une somme de contrôle**  
     Vérifie dans chaque page les informations de somme de contrôle et de page endommagée, si elles sont activées et disponibles, et génère une somme de contrôle pour l’ensemble de la sauvegarde.  
  
     Case à cocher **Continuer en cas d'erreur**  
     Ordonne à BACKUP de continuer en dépit des erreurs rencontrées, telles que des sommes de contrôle de page non valides ou des pages endommagées.  
  
     **Chiffrement de sauvegarde**  
     Pour créer une sauvegarde chiffrée, activez la case à cocher **Chiffrer le fichier de sauvegarde** . Sélectionnez l'algorithme de chiffrement à utiliser pour l'étape de chiffrement et fournissez un certificat ou une clé asymétrique dans la liste des certificats ou clés numériques existants. Les algorithmes disponibles pour le chiffrement sont :  
  
    -   AES 128  
  
    -   AES 192  
  
    -   AES 256  
  
    -   Triple DES  
  
     L'option de chiffrement est désactivée si vous avez choisi d'ajouter la sauvegarde à un jeu de sauvegarde existant.  
  
     Il s'agit de la méthode conseillée pour sauvegarder votre certificat ou vos clés et les stocker dans un emplacement différent de celui de la sauvegarde chiffrée.  
  
     Seules les clés résidant dans la gestion de clés extensible (EKM) sont prises en charge.  
  
     Case à cocher, liste **Taille de bloc**  
  
     Indique, en octets, la taille physique du bloc. En règle générale, cette option affecte les performances si les données sont écrites sur des périphériques à bandes, des modules RAID ou un SAN.  
  
     Case à cocher, liste **Taille de transfert max.**  
  
     Spécifie, en octets, la plus grande unité de transfert à utiliser entre SQL Server et le support de sauvegarde.  
  
     Liste **Définir la compression de la sauvegarde**  
     Dans [!INCLUDE[ssEnterpriseEd10](../../includes/ssenterpriseed10-md.md)] (ou les versions ultérieures), sélectionnez l’une des valeurs de [compression de la sauvegarde](../../relational-databases/backup-restore/backup-compression-sql-server.md) suivantes :  
  
    |||  
    |-|-|  
    |**Utiliser le paramètre du serveur par défaut**|Cliquez sur cette option pour utiliser la valeur par défaut au niveau du serveur. Cette valeur par défaut est définie par l’option de configuration de serveur **Compression par défaut des sauvegardes** . Pour plus d’informations sur l’affichage du paramétrage actuel de cette option, consultez [Afficher ou configurer l’option de configuration du serveur valeur par défaut de compression de la sauvegarde](../../database-engine/configure-windows/view-or-configure-the-backup-compression-default-server-configuration-option.md).|  
    |**Compresser la sauvegarde**|Cliquez sur cette option pour compresser la sauvegarde, indépendamment de la valeur par défaut au niveau du serveur.<br /><br /> **\*\* Important \*\*** Par défaut, la compression augmente considérablement l’utilisation de l’UC et l’UC supplémentaire consommée par le processus de compression peut nuire aux opérations simultanées. Par conséquent, il peut être préférable, dans une session où l'utilisation de l'UC est limitée, de créer une sauvegarde compressée de priorité basse à l'aide de Resource Governor. Pour plus d'informations, consultez [Utiliser Resource Governor pour limiter l’utilisation de l’UC par compression de la sauvegarde &#40;Transact-SQL&#41;](../../relational-databases/backup-restore/use-resource-governor-to-limit-cpu-usage-by-backup-compression-transact-sql.md).|  
    |**Ne pas compresser la sauvegarde**|Cliquez sur cette option pour créer une sauvegarde non compressée, indépendamment de la valeur par défaut au niveau du serveur.|  
  
2.  Dans la page **Définir la tâche Sauvegarder la base de données (différentielle)** , sélectionnez chaque base de données sur laquelle effectuer une sauvegarde partielle. Consultez la liste des définitions à l'étape 16 ci-dessus pour plus d'informations sur les options disponibles sur cette page. La tâche utilise l'instruction `BACKUP DATABASE ... WITH DIFFERENTIAL`. Pour plus d’informations, consultez [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md).  Une fois que vous avez terminé, cliquez sur **Suivant**.  
  
3.  Dans la page **Définir la tâche Sauvegarder la base de données (journal des transactions)** , sélectionnez chaque base de données sur laquelle effectuer une sauvegarde d’un journal des transactions. Consultez la liste des définitions à l'étape 16 ci-dessus pour plus d'informations sur les options disponibles sur cette page. La tâche utilise l'instruction `BACKUP LOG`. Pour plus d’informations, consultez [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md). Une fois que vous avez terminé, cliquez sur **Suivant**.  
  
#### <a name="define-maintenance-cleanup-tasks"></a>Définir les tâches de nettoyage de maintenance  
  
1.  Dans la page **Définir la tâche de nettoyage de maintenance** , spécifiez les types de fichiers à supprimer dans le cadre du plan de maintenance, y compris les rapports de texte créés par les plans de maintenance et les fichiers de sauvegarde de la base de données. La tâche utilise l'instruction `EXEC xp_delete_file`. Une fois que vous avez terminé, cliquez sur **Suivant**.  
  
    > **IMPORTANT** Cette tâche ne supprime pas automatiquement les fichiers dans les sous-dossiers du répertoire spécifié. Cette précaution réduit la possibilité d'une attaque malveillante qui utilise la tâche de nettoyage de maintenance pour supprimer des fichiers. Pour supprimer des fichiers dans les sous-dossiers de premier niveau, vous devez sélectionner **Inclure les sous-dossiers de premier niveau**.  
  
     Les options supplémentaires suivantes sont disponibles sur cette page.  
  
     **Supprimer les fichiers du type suivant**  
     Spécifiez le type de fichiers à supprimer.  
  
     **Fichiers de sauvegarde**  
     Supprimez les fichiers de sauvegarde de base de données.  
  
     **Rapports de texte du plan de maintenance**  
     Supprimez les rapports texte des plans de maintenance exécutés précédemment.  
  
     **Emplacement du fichier**  
     Spécifiez le chemin d'accès des fichiers à supprimer.  
  
     **Supprimer un fichier spécifique**  
     Supprimez le fichier spécifié dans la zone **Nom de fichier** .  
  
     **Rechercher dans le dossier et supprimer les fichiers en fonction de l'extension**  
     Supprimez tous les fichiers contenant l'extension spécifiée dans le dossier spécifié. Utilisez cette option pour supprimer plusieurs fichiers à la fois, par exemple tous les fichiers de sauvegarde possédant l'extension .bak dans le dossier Mardi.  
  
     Zone **Dossier**  
     Chemin d'accès et nom du dossier contenant les fichiers à supprimer.  
  
     Zone **Extension de fichier**  
     Spécifiez l'extension de fichier des fichiers à supprimer. Pour supprimer plusieurs fichiers à la fois, par exemple tous les fichiers de sauvegarde possédant l'extension .bak dans le dossier Mardi, spécifiez .bak.  
  
     Case à cocher **Inclure les sous-dossiers de premier niveau**  
     Supprimez les fichiers portant l’extension spécifiée par l’option **Extension de fichier** dans les sous-dossiers de premier niveau situés dans le dossier défini par l’option **Dossier**.  
  
     Case à cocher **Supprimer les fichiers en fonction de l’ancienneté du fichier au moment de l’exécution de la tâche**  
     Spécifiez l’ancienneté minimale des fichiers à supprimer en entrant un chiffre et une unité de temps dans la zone **Supprimer les fichiers antérieurs à** .  
  
     **Supprimer les fichiers antérieurs à**  
     Spécifiez l’ancienneté minimale des fichiers à supprimer en entrant un chiffre et une unité de temps (**Heure**, **Jour**, **Semaine**, **Mois** ou **Année**). Les fichiers antérieurs au délai spécifié seront supprimés.  
  
#### <a name="select-report-options"></a>Sélectionner des options de rapport  
  
1.  Dans la page **Sélectionner des options de rapport** , sélectionnez les options d'enregistrement ou de distribution d'un rapport d'actions de plan de maintenance. La tâche utilise l'instruction `EXEC sp_notify_operator`. Pour plus d’informations, consultez [sp_notify_operator &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-notify-operator-transact-sql.md). Quand vous avez terminé, cliquez sur **Suivant**.  
  
     Les options supplémentaires suivantes sont disponibles sur cette page.  
  
     Case à cocher **Enregistrer un rapport dans un fichier texte**  
     Enregistre le rapport dans un fichier.  
  
     Zone **Emplacement du dossier**  
     Spécifiez l'emplacement du fichier qui contiendra le rapport.  
  
     Case à cocher **Envoyer le rapport par courrier électronique**  
     Envoie un courrier électronique lorsqu'une tâche échoue. Pour utiliser cette tâche, l’option Messagerie de base de données doit être activée et configurée correctement avec MSDB comme Base de données hôte de messagerie, et vous devez avoir un opérateur [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent avec une adresse e-mail valide.  
  
     **Opérateur d'agent**  
     Spécifiez le destinataire du courrier électronique.  
  
     **Profil de la messagerie**  
     Spécifiez le profil qui définit l'expéditeur du courrier électronique.  
  
#### <a name="complete-the-wizard"></a>Terminer l'Assistant  
  
1.  Dans la page **Terminer l'Assistant** , vérifiez les choix effectués dans les pages précédentes, puis cliquez sur **Terminer**.  
  
2.  Dans la page **Progression de l'Assistant Plan de maintenance** , vérifiez les informations d'état des actions de l'Assistant Plan de maintenance. Selon les options sélectionnées dans l'Assistant, la page de progression peut contenir une ou plusieurs actions. La zone supérieure affiche l'état global de l'Assistant et le nombre des messages d'état, d'erreur et d'avertissement qu'il a reçus.  
  
     Les options suivantes sont disponibles sur la page **Progression de l'Assistant Plan de maintenance** :  
  
     **Détails**  
     Indique l'action, l'état et tous les messages retournés suite à l'action entreprise par l'Assistant.  
  
     **Action**  
     Indique le type et le nom de chaque action.  
  
     **État**  
     Indique si l’action de l’Assistant dans son ensemble a retourné la valeur **Réussite** ou **Échec**.  
  
     **Message**  
     Indique les messages d'erreur ou d'avertissement retournés par le processus.  
  
     **Report**  
     Crée un rapport qui contient les résultats de l'Assistant Création de partition. Les options sont **Afficher le rapport**, **Enregistrer le rapport dans un fichier**, **Copier le rapport dans le Presse-papiers** et **Envoyer le rapport sous forme de courrier électronique**.  
  
     **Afficher le rapport**  
     Ouvre la boîte de dialogue **Afficher le rapport** , qui contient un rapport au format texte de la progression de l’Assistant Création de partition.  
  
     **Enregistrer le rapport dans un fichier**  
     Ouvre la boîte de dialogue **Enregistrer le rapport sous** .  
  
     **Copier le rapport dans le Presse-papiers**  
     Copie les résultats du rapport de progression de l’Assistant dans le Presse-papiers.  
  
     **Envoyer le rapport sous forme de courrier électronique**  
     Copie les résultats du rapport de progression de l’Assistant dans un e-mail.  
  
  

