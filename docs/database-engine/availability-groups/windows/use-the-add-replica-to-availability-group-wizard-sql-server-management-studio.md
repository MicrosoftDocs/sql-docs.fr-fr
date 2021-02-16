---
title: Ajouter un réplica à un groupe de disponibilité (SSMS)
description: Ajoutez un réplica à un groupe de disponibilité Always On à l’aide de l’Assistant figurant dans SQL Server Management Studio.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
f1_keywords:
- sql13.swb.addreplicawizard.f1
helpviewer_keywords:
- Availability Groups [SQL Server], availability replicas
- Availability Groups [SQL Server], wizards
ms.assetid: 60d962b6-2af4-4394-9190-61939a102bc0
author: cawrites
ms.author: chadam
ms.openlocfilehash: e9180f809dd1300a0c5c6a4c5b5d429c6054adc4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340453"
---
# <a name="add-a-replica-to-your-always-on-availability-group-using-the-availability-group-wizard-in-sql-server-management"></a>Ajouter un réplica à un groupe de disponibilité Always On à l’aide de l’Assistant Groupe de disponibilité dans SQL Server Management
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Utilisez l’**Assistant Ajouter un réplica au groupe de disponibilité** pour ajouter un nouveau réplica secondaire à un groupe de disponibilité AlwaysOn existant.  
  
> [!NOTE]  
>  Pour plus d’informations sur l’utilisation de [!INCLUDE[tsql](../../../includes/tsql-md.md)] ou de PowerShell pour ajouter un réplica secondaire à un groupe de disponibilité, consultez [Ajouter un réplica secondaire à un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/add-a-secondary-replica-to-an-availability-group-sql-server.md).  
    
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
 Si vous n’avez jamais ajouté de réplica de disponibilité à un groupe de disponibilité, consultez les sections « Instances de serveur » et « Groupes de disponibilité et réplicas » dans [Conditions préalables requises, restrictions et recommandations pour les groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md).  
  
##  <a name="prerequisites"></a><a name="Prerequisites"></a> Conditions préalables  
  
-   Vous devez être connecté à l'instance de serveur qui héberge le réplica principal actuel.  
  
-   Avant d’ajouter un réplica secondaire, vérifiez que l’instance hôte de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] se trouve dans le même cluster de basculement Windows Server que les réplicas existants, mais qu’il est sur un nœud de cluster différent. Vérifiez également que cette instance de serveur répond à toutes les autres conditions requises relatives à [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] . Pour plus d’informations, consultez [Conditions préalables requises, restrictions et recommandations pour les groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md).  
  
-   Si une instance de serveur que vous sélectionnez pour héberger un réplica de disponibilité s'exécute sous un compte d'utilisateur de domaine et n'a pas encore de point de terminaison de mise en miroir de bases de données, l'Assistant peut créer le point de terminaison et accorder l'autorisation CONNECT au compte de service de l'instance de serveur. Toutefois, si le service [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] s'exécute en tant que compte intégré, tel que Système local, Service local ou Service réseau, ou comme compte qui n'appartient pas au domaine, vous devez utiliser des certificats pour l'authentification du point de terminaison, et l'Assistant ne peut pas créer un point de terminaison de mise en miroir de bases de données sur l'instance de serveur. Dans ce cas, nous recommandons de créer les points de terminaison de mise en miroir de bases de données manuellement avant de lancer l'Assistant Ajouter un réplica au groupe de disponibilité.  
  
     **Pour utiliser des certificats pour un point de terminaison de mise en miroir de bases de données :**  
  
     [CREATE ENDPOINT &#40;Transact-SQL&#41;](../../../t-sql/statements/create-endpoint-transact-sql.md)  
  
     [Utiliser des certificats pour un point de terminaison de mise en miroir de bases de données &#40;Transact-SQL&#41;](../../../database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql.md)  
  
-   **Conditions préalables requises pour utiliser la synchronisation de données initiale complète**  
  
    -   Tous les chemins d'accès des fichiers de base de données doivent être identiques sur chaque instance de serveur qui héberge un réplica pour le groupe de disponibilité.  
  
    -   Aucun nom de base de données primaire ne peut exister sur une instance de serveur qui héberge un réplica secondaire. Cela signifie qu'aucune des nouvelles bases de données secondaires ne peut exister pour le moment.  
  
    -   Vous devez spécifier un partage réseau pour que l'assistant crée des sauvegardes et puisse y accéder. Pour le réplica principal, le compte utilisé pour démarrer le [!INCLUDE[ssDE](../../../includes/ssde-md.md)] doit disposer d'autorisations de système de fichiers en lecture et en écriture sur un partage réseau. Pour les réplicas secondaires, le compte doit disposer d'une autorisation en lecture sur le partage réseau.  
  
     Si vous ne pouvez pas utiliser l'Assistant pour effectuer la synchronisation des données initiale complète, vous devez préparer vos bases de données secondaires manuellement. Vous pouvez le faire avant ou après l'exécution de l'Assistant. Pour plus d’informations, consultez [Préparer manuellement une base de données secondaire pour un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/manually-prepare-a-secondary-database-for-an-availability-group-sql-server.md).  
   
## <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Requiert l'autorisation ALTER AVAILABILITY GROUP sur le groupe de disponibilité, l'autorisation CONTROL AVAILABILITY GROUP, l'autorisation ALTER ANY AVAILABILITY GROUP ou l'autorisation CONTROL SERVER.  
  
 Nécessite également l'autorisation CONTROL ON ENDPOINT si vous souhaitez autoriser l'Assistant Ajouter un réplica au groupe de disponibilité à gérer le point de terminaison de mise en miroir de bases de données.  
  
##  <a name="using-the-add-replica-to-availability-group-wizard-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de l'Assistant Ajouter un réplica au groupe de disponibilité (SQL Server Management Studio)  
 **Pour utiliser l'Assistant Ajouter un réplica au groupe de disponibilité**  
  
1.  Dans l'Explorateur d'objets, connectez-vous à l'instance de serveur qui héberge le réplica principal du groupe de disponibilité et développez l'arborescence du serveur.  
  
2.  Développez le nœud **Haute disponibilité AlwaysOn** et le nœud **Groupes de disponibilité** .  
  
3.  Cliquez avec le bouton droit sur le groupe de disponibilité auquel vous ajoutez un réplica secondaire, puis sélectionnez la commande **Ajouter un réplica** . Cela permet de lancer l'Assistant Ajouter un réplica au groupe de disponibilité.  
  
4.  Sur la page **Se connecter à des réplicas secondaires existants** , connectez-vous à chaque réplica secondaire du groupe de disponibilité. Pour plus d’informations, consultez la [page Se connecter à des réplicas secondaires existants &#40;Assistant Ajout de réplica : Assistant Ajout de bases de données&#41;](../../../database-engine/availability-groups/windows/connect-to-existing-secondary-replicas-page.md).  
  
5.  Sur la page **Spécifier les réplicas** , spécifiez et configurez un ou plusieurs réplicas secondaires pour le groupe de disponibilité. Cette page contient trois onglets. Le tableau suivant présente ces onglets. Pour plus d’informations, consultez la [page Spécifier les réplicas &#40;Assistant Nouveau groupe de disponibilité : Assistant Ajout de réplica&#41;](../../../database-engine/availability-groups/windows/specify-replicas-page-new-availability-group-wizard-add-replica-wizard.md).  
  
    |Onglet|Brève description|  
    |---------|-----------------------|  
    |**Réplicas**|Cet onglet vous permet de spécifier chaque instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] qui hébergera un nouveau réplica secondaire.|  
    |**Points de terminaison**|Utilisez cet onglet pour vérifier le point de terminaison de mise en miroir de bases de données existant, le cas échéant, pour chaque nouveau réplica secondaire. Si ce point de terminaison manque sur une instance de serveur dont les comptes de service utilisent l'authentification Windows, l'Assistant tente de créer le point de terminaison automatiquement.<br /><br /> <br /><br /> Remarque : Si une instance de serveur s’exécute sous un compte d’utilisateur qui n’appartient pas au domaine, vous devez apporter une modification manuelle à votre instance de serveur avant de pouvoir poursuivre l’Assistant. Pour plus d'informations, consultez [Conditions préalables requises](#Prerequisites), plus haut dans cette rubrique.|  
    |**Préférences de sauvegarde**|Utilisez cet onglet pour spécifier vos préférences de sauvegarde pour le groupe de disponibilité dans son ensemble, si vous souhaitez modifier le paramètre actuel, et pour indiquer vos priorités de sauvegarde pour les différents réplicas de disponibilité.|  
  
6.  Si les réplicas sélectionnés contiennent des bases de données comportant une clé principale de base de données, entrez les mots de passe pour les clés principales de base de données dans la colonne **Mot de passe**. La colonne **État** indique **Mot de passe requis** pour les bases de données qui comportent une clé principale de base de données. L’option **Suivant** est grisée jusqu’à ce que le mot de passe correct soit entré dans la colonne **Mot de passe**. Après avoir entré les mots de passe, cliquez sur **Actualiser**. Si les mots de passe entrés sont corrects, la colonne État indique **Mot de passe entré** et **Suivant** devient disponible.  
  
7.  Sur la page **Sélectionner la synchronisation de données initiale** , choisissez comment vous souhaitez que vos nouvelles bases de données secondaires soient créées et jointes au groupe de disponibilité. Choisissez l’une des options suivantes :  
  
    -   **Complète**  
  
         Sélectionnez cette option si votre environnement répond aux conditions requises pour démarrer automatiquement la synchronisation initiale des données (pour plus d’informations, consultez [Conditions préalables requises, restrictions et recommandations](#Prerequisites), plus haut dans cette rubrique).  
  
         Si vous sélectionnez **Complet**, après avoir créé le groupe de disponibilité, l'assistant sauvegarde chaque base de données primaire et son journal des transactions sur un partage réseau et restaure les sauvegardes sur chaque instance de serveur qui héberge un nouveau réplica secondaire. L'assistant joint ensuite chaque nouvelle base de données secondaire au groupe de disponibilité.  
  
         Dans le champ **Spécifier un emplacement réseau partagé accessible par tous les réplicas** , spécifiez un partage de sauvegarde dans lequel l’intégralité de l’instance de serveur qui héberge les réplicas dispose d’un accès en lecture-écriture. Les sauvegardes de journaux feront partie de votre chaîne de sauvegarde du journal. Stockez les fichiers de sauvegarde des journaux de manière appropriée.  
  
        > [!IMPORTANT]  
        >  Pour plus d’informations sur les autorisations de système de fichiers requises, consultez [Configuration préalable requise](#Prerequisites)plus haut dans cette rubrique.  
  
    -   **Joindre uniquement**  
  
         Si vous avez préparé manuellement les bases de données secondaires sur les instances de serveur qui hébergeront les nouveaux réplicas secondaires, vous pouvez sélectionner cette option. L'assistant joindra ces nouvelles bases de données secondaires au groupe de disponibilité.  
  
    -   **Ignorer la synchronisation de données initiale**  
  
         Sélectionnez cette option si vous souhaitez utiliser votre propre base de données et sauvegardes de journaux de vos bases de données primaires. Pour plus d’informations, consultez [Démarrer un mouvement de données sur une base de données secondaire Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/start-data-movement-on-an-always-on-secondary-database-sql-server.md).  
  
8.  La page **Validation** vérifie si les valeurs que vous avez spécifiées dans cet Assistant répondent aux exigences de l'Assistant Ajouter un réplica au groupe de disponibilité. Pour effectuer un changement, cliquez sur **Précédent** pour revenir à une page antérieure de l'assistant pour modifier une ou plusieurs valeurs. Cliquez sur **Suivant** pour revenir à la page **Validation** , puis cliquez sur **Réexécuter la validation**.  
  
9. Sur la page **Résumé** , examinez vos choix pour le nouveau groupe de disponibilité. Pour apporter une modification, cliquez sur **Précédent** pour revenir à la page appropriée. Après avoir apporté la modification, cliquez sur **Suivant** pour revenir à la page **Résumé** .  
  
     Si vous êtes satisfait de vos sélections, cliquez éventuellement sur Script pour créer un script des étapes que l'assistant devra exécuter. Ensuite, pour créer et configurer le nouveau groupe de disponibilité, cliquez sur **Terminer**.  
  
10. La page **État d’avancement** affiche l’état d’avancement des étapes de création du groupe de disponibilité (configuration de points de terminaison, création du groupe de disponibilité et jointure du réplica secondaire au groupe).  
  
11. Lorsque ces étapes sont terminées, la page **Résultats** affiche le résultat de chaque étape. Si toutes ces étapes aboutissent, le nouveau groupe de disponibilité est entièrement configuré. Si l'une des étapes se solde par une erreur, vous devrez peut-être effectuer la configuration manuellement. Pour plus d'informations sur la cause d'une erreur donnée, cliquez sur le lien « Erreur » associé dans la colonne **Résultat** .  
  
     À la fin de l'Assistant, cliquez sur **Fermer** pour le quitter.  
  
> [!IMPORTANT]  
>  Après avoir ajouté un réplica, consultez la section « Suivi : Après avoir ajouté un réplica » dans [Ajouter un réplica secondaire à un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/add-a-secondary-replica-to-an-availability-group-sql-server.md).  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Tâches associées  
  
-   [Ajouter un réplica secondaire à un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/add-a-secondary-replica-to-an-availability-group-sql-server.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Vue d’ensemble des groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Conditions préalables requises, restrictions et recommandations pour les groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)   
 [Ajouter un réplica secondaire à un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/add-a-secondary-replica-to-an-availability-group-sql-server.md)  
  
  
