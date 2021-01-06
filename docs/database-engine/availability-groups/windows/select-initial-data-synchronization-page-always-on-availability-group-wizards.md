---
title: Page Sélectionner la synchronisation de données initiale (Assistant Groupe de disponibilité)
description: Description de la « page Sélectionner la synchronisation de données initiale » de l’Assistant Groupe de disponibilité Always On dans SSMS (SQL Server Management Studio).
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
f1_keywords:
- sql13.swb.adddatabasewizard.selectinitialdatasync.f1
- sql13.swb.newagwizard.selectinitialdatasync.f1
- sql13.swb.addreplicawizard.selectinitialdatasync.f1
ms.assetid: 457b1140-4819-4def-8f7c-54a406e6db12
author: cawrites
ms.author: chadam
ms.openlocfilehash: fd729c01993f06c9017a898e1e804dde844e10f2
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97642153"
---
# <a name="select-initial-data-synchronization-page-always-on-availability-group-wizards"></a>Page Sélectionner la synchronisation de données initiale (Assistants de groupe de disponibilité Always On)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  Utilisez la page Always On **Sélectionner la synchronisation de données initiale** pour indiquer votre préférence pour la synchronisation des données initiale des nouvelles bases de données secondaires. Cette page est commune à trois Assistants : [!INCLUDE[ssAoNewAgWiz](../../../includes/ssaonewagwiz-md.md)], [!INCLUDE[ssAoAddRepWiz](../../../includes/ssaoaddrepwiz-md.md)] et [!INCLUDE[ssAoAddDbWiz](../../../includes/ssaoadddbwiz-md.md)].  
  
 Les choix possibles sont notamment : **Amorçage automatique**, **Sauvegarde complète de la base de données et des journaux**, **Joindre uniquement** ou **Ignorer la synchronisation de données initiale**. Avant de sélectionner **Amorçage automatique**, **Complet** ou **Joindre uniquement**, vérifiez que votre environnement est conforme aux prérequis.  
    
##  <a name="recommendations"></a><a name="Recommendations"></a> Recommandations  
  
-   Interrompez les tâches de sauvegarde de fichier journal pour les bases de données principales durant la synchronisation de données initiale.  
  
-   Pour les bases de données volumineuses, les opérations de sauvegarde complète et de restauration peuvent prendre beaucoup de temps et solliciter beaucoup de ressources. Dans ce cas, nous vous recommandons de préparer vos bases de données secondaires vous-même. Pour plus d'informations, consultez [Pour préparer manuellement une base de données secondaire](#PrepareSecondaryDbs), plus loin dans cette rubrique.  
  
-   La synchronisation de données initiale complète requiert la spécification d'un partage réseau. Avant d'utiliser un assistant pour effectuer la synchronisation initiale complète des données, nous vous recommandons d'implémenter un plan de sécurité des autorisations d'accès sur le dossier de partage réseau. Cette précaution est importante car les données potentiellement sensibles figurant dans le fichier de sauvegarde peuvent être accessibles par toute personne disposant d'une autorisation en lecture READ sur le dossier. En outre, pour protéger votre opérations de sauvegarde et de restauration, nous vous recommandons de sécuriser les canaux réseau entre chaque instance de serveur qui héberge un réplica de disponibilité et le dossier de partage réseau.  
  
     Si vos opérations de sauvegarde et de restauration nécessitent une sécurisation élevée, nous vous recommandons de sélectionner l'option **Joindre uniquement** ou **Ignorer la synchronisation de données initiale** .  
  
## <a name="automatic-seeding"></a><a name="Auto"></a> Amorçage automatique
 
 SQL Server crée automatiquement les réplicas secondaires pour chaque base de données du groupe. L’amorçage automatique nécessite que le chemin des données et des fichiers journaux soit le même sur chaque instance de SQL Server qui participe au groupe. Disponible sur [!INCLUDE[sssql15-md.md](../../../includes/sssql15-md.md)] et ultérieur. Consultez [Initialiser automatiquement le groupe de disponibilité Always On](automatically-initialize-always-on-availability-group.md).

##  <a name="full-database-and-log-backup"></a><a name="Full"></a> Sauvegarde complète de la base de données et des journaux 
 Pour chaque base de données principale, l'option **Sauvegarde complète de la base de données et des journaux** exécute plusieurs opérations dans un flux de travail : créer une sauvegarde complète et une sauvegarde des journaux de la base de données principale, créer les bases de données secondaires correspondantes en restaurant ces sauvegardes sur chaque instance de serveur qui héberge un réplica secondaire et joindre chaque base de données secondaire à un groupe de disponibilité.  
  
 Sélectionnez cette option uniquement si votre environnement satisfait aux conditions préalables requises qui suivent pour utiliser la synchronisation de données initiale complète, et si vous souhaitez que l'Assistant démarre automatiquement la synchronisation des données.  
  
 **Prérequis pour utiliser la synchronisation de données initiale avec la sauvegarde complète de la base de données et des journaux**  
  
-   Tous les chemins d'accès des fichiers de base de données doivent être identiques sur chaque instance de serveur qui héberge un réplica pour le groupe de disponibilité.  
  
    > [!NOTE]  
    >  Si les chemins d'accès des fichiers de sauvegarde et de restauration diffèrent entre l'instance de serveur sur laquelle vous exécutez l'Assistant et une instance de serveur qui doit héberger un réplica secondaire, les opérations de sauvegarde et de restauration doivent être exécutées manuellement à l'aide de l'option WITH MOVE. Pour plus d'informations, consultez [Pour préparer manuellement une base de données secondaire](#PrepareSecondaryDbs), plus loin dans cette rubrique.  
  
-   Aucun nom de base de données primaire ne peut exister sur une instance de serveur qui héberge un réplica secondaire. Cela signifie qu'aucune des nouvelles bases de données secondaires ne peut exister pour le moment.  
  
-   Vous devez spécifier un partage réseau pour que l'assistant crée des sauvegardes et puisse y accéder. Pour le réplica principal, le compte utilisé pour démarrer le [!INCLUDE[ssDE](../../../includes/ssde-md.md)] doit disposer d'autorisations de système de fichiers en lecture et en écriture sur un partage réseau. Pour les réplicas secondaires, le compte doit disposer d'une autorisation en lecture sur le partage réseau.  
  
    > [!IMPORTANT]  
    >  Les sauvegardes de journaux feront partie de votre chaîne de sauvegarde du journal. Stockez les fichiers de sauvegarde des journaux de manière appropriée.  
  
 **Si les conditions préalables requises ne sont pas satisfaites**  
  
 L'Assistant ne peut pas créer les bases de données secondaires pour ce groupe de disponibilité. Pour plus d'informations sur la façon de les préparer, consultez [Pour préparer manuellement une base de données secondaire](#PrepareSecondaryDbs), plus loin dans cette rubrique.  
  
 **Si les conditions préalables requises sont remplies**  
  
 Si tous les prérequis sont remplis et que vous voulez que l'Assistant effectue la synchronisation de données initiale complète, sélectionnez l'option **Sauvegarde complète de la base de données et des journaux** et spécifiez un partage réseau. L'Assitant crée alors la base de données complète et les sauvegardes des fichiers journaux de chaque base de données sélectionnée et place ces sauvegardes sur le partage réseau que vous spécifiez. Ensuite, sur chaque instance de serveur qui héberge l'un des nouveaux réplicas secondaires, l'Assistant crée les bases de données secondaires en restaurant les sauvegardes à l'aide de RESTORE WITH NORECOVERY. Après la création de chacune des bases de données secondaires, l'Assistant joint la nouvelle base de données secondaire au groupe de disponibilité. Dès qu'une base de données secondaire est jointe, les synchronisations de données démarrent sur cette base de données.  
  
 **Spécifier un emplacement réseau partagé accessible par tous les réplicas**  
 Pour créer et restaurer des sauvegardes, l'assistant vous demande de spécifier un partage réseau. Le compte utilisé pour démarrer le [!INCLUDE[ssDE](../../../includes/ssde-md.md)] sur chaque instance de serveur qui hébergera un réplica de disponibilité doit disposer d'autorisations de système de fichiers en lecture et en écriture sur le partage réseau.  
  
> [!IMPORTANT]  
>  Les sauvegardes de journaux feront partie de votre chaîne de sauvegarde du journal. Stockez les fichiers de sauvegarde de manière appropriée.  
  
##  <a name="join-only"></a><a name="Joinonly"></a> Joindre uniquement  
 Sélectionnez cette option uniquement s'il existe déjà des nouvelles bases de données secondaires sur chaque instance de serveur qui héberge un réplica secondaire pour le groupe de disponibilité. Pour plus d'informations sur la préparation des bases de données secondaires, consultez [Pour préparer manuellement une base de données secondaire](#PrepareSecondaryDbs), plus loin dans cette section.  
  
 Si vous sélectionnez **Joindre uniquement**, l'assistant essaie de joindre chaque base de données secondaire existante au groupe de disponibilité.  
  
## <a name="skip-initial-data-synchronization"></a><a name="Skip"></a> Ignorer la synchronisation de données initiale  
 Sélectionnez cette option si vous souhaitez effectuer vos propres sauvegardes de base de données et de journaux de chaque base de données primaire, puis les restaurer dans chaque instance de serveur qui héberge un réplica secondaire. Une fois que vous avez fermé l'assistant, vous devez ensuite joindre chaque base de données secondaire sur chaque réplica secondaire.  
  
> [!NOTE]  
>  Pour plus d’informations, consultez [Démarrer un mouvement de données sur une base de données secondaire Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/start-data-movement-on-an-always-on-secondary-database-sql-server.md).  
  
##  <a name="to-prepare-secondary-databases-manually"></a><a name="PrepareSecondaryDbs"></a> Pour préparer manuellement une base de données secondaire  
 Pour préparer les bases de données secondaires indépendamment de l'Assistant [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] utilisé, vous pouvez opter pour l'une des méthodes suivantes :  
  
-   Restaurez manuellement une sauvegarde récente de la base de données primaire à l'aide de RESTORE WITH NORECOVERY, puis restaurez chaque sauvegarde de journal suivante à l'aide de RESTORE WITH NORECOVERY. Si les bases de données primaire et secondaire ont des chemins d'accès différents, vous devez utiliser l'option WITH MOVE. Effectuez cette séquence de restauration sur chaque instance de serveur qui héberge un réplica secondaire pour le groupe de disponibilité.  Vous pouvez utiliser [!INCLUDE[tsql](../../../includes/tsql-md.md)] ou PowerShell pour exécuter ces opérations de sauvegarde et de restauration.  
  
     **Pour plus d'informations :**  
  
     [Préparer manuellement une base de données secondaire pour un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/manually-prepare-a-secondary-database-for-an-availability-group-sql-server.md)  
  
-   Si vous ajoutez une ou plusieurs bases de données primaires pour la copie des journaux de transaction à un groupe de disponibilité, vous pourrez peut-être effectuer une migration d'une ou de plusieurs bases de données secondaires correspondantes depuis la copie des journaux de transaction vers [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]. Pour plus d'informations, consultez [Conditions préalables requises pour la migration de la copie des journaux de transaction vers les groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-migrating-log-shipping-to-always-on-availability-groups.md).  
  
    > [!NOTE]  
    >  Après avoir créé toutes les bases de données secondaires pour le groupe de disponibilité, si vous souhaitez effectuer des sauvegardes sur des réplicas secondaires, vous devez reconfigurer la préférence de sauvegarde automatisée du groupe de disponibilité.  
  
     **Pour plus d'informations :**  
  
     [Conditions préalables requises pour la migration de la copie des journaux de transaction vers les groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-migrating-log-shipping-to-always-on-availability-groups.md)  
  
     [Configurer la sauvegarde sur des réplicas de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-backup-on-availability-replicas-sql-server.md)  
  
 Après avoir créé une base de données secondaire, appliquez toutes les sauvegardes de journal en cours à la nouvelle base de données secondaire.  
  
 Éventuellement, vous pouvez préparer toutes les bases de données secondaires avant d'exécuter l'Assistant. Puis, dans la page **Sélectionner la synchronisation de données initiale** de l'Assistant, sélectionnez **Joindre uniquement** pour joindre automatiquement vos nouvelles bases de données secondaires au groupe de disponibilité.  
  
##  <a name="related-tasks"></a><a name="LaunchWiz"></a> Tâches associées  
  
-   [Utiliser la boîte de dialogue Nouveau groupe de disponibilité &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-new-availability-group-dialog-box-sql-server-management-studio.md)  
  
-   [Utiliser l’Assistant Ajouter un réplica au groupe de disponibilité &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-add-replica-to-availability-group-wizard-sql-server-management-studio.md)  
  
-   [Utiliser l’Assistant Ajouter une base de données au groupe de disponibilité &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/availability-group-add-database-to-group-wizard.md)  
  
-   [Utiliser l’Assistant Basculer le groupe de disponibilité &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-fail-over-availability-group-wizard-sql-server-management-studio.md)  
  
-   [Démarrer un mouvement de données sur une base de données secondaire Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/start-data-movement-on-an-always-on-secondary-database-sql-server.md)  
  
-   [Joindre une base de données secondaire à un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-database-to-an-availability-group-sql-server.md)  
  
-   [Utiliser la boîte de dialogue Nouveau groupe de disponibilité &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-new-availability-group-dialog-box-sql-server-management-studio.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Vue d’ensemble des groupes de disponibilité Always On (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)  
  
  
