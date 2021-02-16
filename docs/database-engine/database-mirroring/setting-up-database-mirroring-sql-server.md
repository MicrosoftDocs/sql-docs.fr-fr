---
title: Configuration de la mise en miroir de bases de données (SQL Server) | Microsoft Docs
description: Découvrez les conditions préalables, les suggestions et les étapes à suivre pour configurer la mise en miroir de bases de données dans SQL Server, y compris une vue d’ensemble d’une session de mise en miroir de bases de données.
ms.custom: ''
ms.date: 05/17/2016
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- database mirroring [SQL Server], deployment
ms.assetid: da45efed-55eb-4c71-be34-ac2589dfce8d
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: c085a7e1571e85e4fc3f3f9267e033e17302e09e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352286"
---
# <a name="setting-up-database-mirroring-sql-server"></a>Configuration de la mise en miroir de bases de données (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Cette section décrit les conditions préalables, les recommandations et les étapes relatives à la configuration de la mise en miroir d'une base de données. Pour une présentation de la mise en miroir de bases de données, consultez [Mise en miroir de bases de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-sql-server.md).  
  
> [!IMPORTANT]  
>  Nous vous recommandons de configurer la mise en miroir de bases de données durant les heures creuses, car cela peut affecter les performances.  
  
  
##  <a name="preparing-a-server-instance-to-host-a-mirror-server"></a><a name="PrepareInstances"></a> Préparation d'une instance de serveur pour héberger un serveur miroir  
 Pour chaque session de mise en miroir de bases de données :  
  
1.  Le serveur principal, le serveur miroir et le serveur témoin, le cas échéant, doivent être hébergés par des instances de serveur distinctes, chacune devant se trouver sur des systèmes hôtes distincts. Chacune des instances de serveur nécessite un point de terminaison de mise en miroir de bases de données. Si vous devez créer un point de terminaison de mise en miroir de bases de données, vérifiez qu'il est accessible aux autres instances de serveur.  
  
     Le type d'authentification utilisé pour la mise en miroir de la base de données par une instance de serveur est une propriété de son point de terminaison de mise en miroir de bases de données. Deux types de sécurité de transport sont disponibles pour la mise en miroir de bases de données : l’authentification Windows ou l’authentification basée sur les certificats. Pour plus d’informations, consultez [Sécurité du transport de la mise en miroir de bases de données et des groupes de disponibilité Always On &#40;SQL Server&#41;](../../database-engine/database-mirroring/transport-security-database-mirroring-always-on-availability.md).  
  
     Les spécifications liées à l'accès réseau sont spécifiques à la forme d'authentification, comme suit :  
  
    -   **Avec une authentification Windows**  
  
         Si des instances de serveurs s'exécutent sous différents comptes d'utilisateurs de domaine, chaque instance requiert une connexion à la base de données **master** de toutes les autres. Si la connexion n'existe pas, vous devez la créer. Pour plus d’informations, consultez [Autoriser l’accès sur le réseau à un point de terminaison de mise en miroir de bases de données au moyen de l’authentification Windows &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-allow-network-access-windows-authentication.md).  
  
    -   **Avec des certificats**  
  
         Pour activer l'authentification des certificats en vue de la mise en miroir de bases de données sur une instance déterminée du serveur, l'administrateur système doit configurer chaque instance du serveur afin d'utiliser les certificats à la fois sur les connexions sortantes et entrantes. Les connexions sortantes doivent être configurées en premier. Pour plus d'informations, consultez [Utiliser des certificats pour un point de terminaison de mise en miroir de bases de données &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql.md).  
  
2.  Vérifiez que les connexions existent sur le serveur miroir pour tous les utilisateurs de base de données. Pour plus d’informations, consultez [Configurer des comptes de connexion pour la mise en miroir de bases de données ou les groupes de disponibilité Always On &#40;SQL Server&#41;](../../database-engine/database-mirroring/set-up-login-accounts-database-mirroring-always-on-availability.md).  
  
3.  Sur l'instance de serveur qui hébergera la base de données miroir, configurez le reste de l'environnement requis pour la base de données mise en miroir. Pour plus d’informations, consultez [Gérer les métadonnées durant la mise à disposition d’une base de données sur une autre instance de serveur &#40;SQL Server&#41;](../../relational-databases/databases/manage-metadata-when-making-a-database-available-on-another-server.md).  
  
##  <a name="overview-establishing-a-database-mirroring-session"></a><a name="EstablishUsingWinAuthentication"></a> Vue d’ensemble : établissement d’une session de mise en miroir de bases de données  
 Les étapes de base pour établir une session de mise en miroir sont les suivantes :  
  
1.  Créez la base de données miroir en restaurant les sauvegardes suivantes, à l'aide de RESTORE WITH NORECOVERY sur chaque opération de restauration :  
  
    1.  Restaurez une sauvegarde complète récente de la base de données principale, après avoir vérifié que la base de données principale utilisait déjà le mode de récupération complète lorsque la sauvegarde a été effectuée. La base de données miroir doit porter le même nom que la base de données principale.  
  
    2.  Si vous avez effectué des sauvegardes différentielles de la base de données depuis la sauvegarde complète restaurée, restaurez votre sauvegarde différentielle la plus récente.  
  
    3.  Restaurez toutes les sauvegardes de fichiers journaux effectuées depuis la sauvegarde complète ou différentielle de base de données.  
  
     Pour plus d’informations, consultez [Préparer une base de données miroir pour la mise en miroir &#40;SQL Server&#41;](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md).  
  
    > [!IMPORTANT]  
    >  Exécutez les étapes de configuration restantes dès que possible après avoir effectué la sauvegarde de la base de données principale. Pour pouvoir procéder à la mise en miroir sur les partenaires, vous devez tout d'abord créer une sauvegarde du journal actif sur la base de données d'origine et le restaurer sur la future base de données miroir.  
  
2.  Vous pouvez configurer la mise en miroir en utilisant [!INCLUDE[tsql](../../includes/tsql-md.md)] ou l'Assistant Mise en miroir de bases de données. Pour plus d'informations, consultez l'une des rubriques suivantes :  
  
    -   [Établir une session de mise en miroir de bases de données au moyen de l’authentification Windows &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/database-mirroring-establish-session-windows-authentication.md)  
  
    -   [Établir une session de mise en miroir de bases de données au moyen de l’authentification Windows &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/establish-database-mirroring-session-windows-authentication.md)  
  
3.  Par défaut, les sessions sont définies sur une sécurité des transactions totale (valeur de SAFETY définie sur FULL), la session est donc démarrée en mode haute sécurité sans basculement automatique. Pour reconfigurer la session pour qu'elle s'exécute en mode haute sécurité avec basculement automatique ou en mode haute performance asynchrone, procédez comme suit :  
  
    -   **Mode haute sécurité avec basculement automatique**  
  
         Si vous souhaitez qu'une session en mode haute sécurité prenne en charge le basculement automatique, ajoutez une instance de serveur témoin.  
  
         **Pour ajouter un témoin**  
  
        -   [Ajouter un témoin de mise en miroir de bases de données à l’aide de l’authentification Windows &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/add-a-database-mirroring-witness-using-windows-authentication-transact-sql.md)  
  
        -   [Établir une session de mise en miroir de bases de données au moyen de l’authentification Windows &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/establish-database-mirroring-session-windows-authentication.md)  
  
        > [!NOTE]  
        >  Le propriétaire de la base de données peut désactiver le serveur témoin d'une base de données à tout moment. La désactivation du serveur témoin équivaut à l'absence de témoin ; le basculement automatique ne peut donc pas se produire.  
  
    -   **Mode hautes performances**  
  
         Si vous ne voulez pas utiliser le basculement automatique et si vous préférez privilégier les performances par rapport à la disponibilité, désactivez la sécurité des transactions. Pour plus d’informations, consultez [Modifier la sécurité des transactions dans une session de mise en miroir de bases de données &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/change-transaction-safety-in-a-database-mirroring-session-transact-sql.md).  
  
        > [!NOTE]  
        >  En mode haute performance, la valeur de WITNESS doit être définie sur OFF. Pour plus d’informations, consultez [Quorum : effets d’un témoin sur la disponibilité de la base de données &#40;Mise en miroir de bases de données&#41;](../../database-engine/database-mirroring/quorum-how-a-witness-affects-database-availability-database-mirroring.md).  
  
> [!NOTE]  
>  Pour obtenir un exemple de l’utilisation de [!INCLUDE[tsql](../../includes/tsql-md.md)] pour configurer la mise en miroir de bases de données à l’aide de l’authentification Microsoft Windows, consultez [Exemple : Configuration de la mise en miroir de bases de données à l’aide de l’authentification Windows &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/example-setting-up-database-mirroring-using-windows-authentication-transact-sql.md).  
>   
>  Pour obtenir un exemple de l’utilisation de [!INCLUDE[tsql](../../includes/tsql-md.md)] pour configurer la mise en miroir de bases de données à l’aide de la sécurité basée sur les certificats, consultez [Exemple : Configuration de la mise en miroir de bases de données à l’aide de certificats &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/example-setting-up-database-mirroring-using-certificates-transact-sql.md).  
  
  
##  <a name="in-this-section"></a><a name="InThisSection"></a> Dans cette section  
 [Préparer une base de données miroir pour la mise en miroir &#40;SQL Server&#41;](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md)  
 Résume les étapes de la création d'une base de données miroir ou de la préparation d'une base de données miroir avant la reprise d'une session suspendue. Fournit également des liens vers des articles de procédure.  
  
 [Spécifier une adresse réseau de serveur &#40;mise en miroir de bases de données&#41;](../../database-engine/database-mirroring/specify-a-server-network-address-database-mirroring.md)  
 Décrit la syntaxe d'une adresse réseau de serveur et explique comment l'adresse identifie le point de terminaison de mise en miroir de bases de données de l'instance de serveur et comment rechercher le nom de domaine complet d'un système.  
  
 [Établir une session de mise en miroir de bases de données au moyen de l’authentification Windows &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/establish-database-mirroring-session-windows-authentication.md)  
 Indique comment utiliser l'Assistant Configurer la sécurité de la mise en miroir de bases de données pour démarrer la mise en miroir de bases de données sur une base de données.  
  
 [Établir une session de mise en miroir de bases de données au moyen de l’authentification Windows &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/database-mirroring-establish-session-windows-authentication.md)  
 Décrit les étapes [!INCLUDE[tsql](../../includes/tsql-md.md)] de la configuration de la mise en miroir de bases de données.  
  
 [Exemple : Configuration de la mise en miroir de bases de données à l’aide de l’authentification Windows &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/example-setting-up-database-mirroring-using-windows-authentication-transact-sql.md)  
 Contient un exemple de toutes les étapes nécessaires à la création d'une session de mise en miroir de bases de données avec un serveur témoin, en utilisant l'authentification Windows.  
  
 [Exemple : Configuration de la mise en miroir de bases de données à l’aide de certificats &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/example-setting-up-database-mirroring-using-certificates-transact-sql.md)  
 Contient un exemple de toutes les étapes nécessaires à la création d'une session de mise en miroir de bases de données avec un serveur témoin, en utilisant l'authentification basée sur les certificats.  
  
 [Configurer des comptes de connexion pour la mise en miroir de bases de données ou les groupes de disponibilité Always On &#40;SQL Server&#41;](../../database-engine/database-mirroring/set-up-login-accounts-database-mirroring-always-on-availability.md)  
 Décrit la création d'une connexion pour une instance de serveur distant qui n'utilise pas le même compte que l'instance de serveur local.  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Tâches associées  
 **SQL Server Management Studio**  
  
-   [Démarrer l’Assistant Configuration de la sécurité de mise en miroir de bases de données &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/start-the-configuring-database-mirroring-security-wizard.md)  
  
-   [Établir une session de mise en miroir de bases de données au moyen de l’authentification Windows &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/establish-database-mirroring-session-windows-authentication.md)  
  
 **Transact-SQL**  
  
-   [Autoriser l’accès sur le réseau à un point de terminaison de mise en miroir de bases de données au moyen de l’authentification Windows &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-allow-network-access-windows-authentication.md)  
  
-   [Autoriser un point de terminaison de mise en miroir de bases de données à utiliser des certificats pour les connexions sortantes &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-outbound-connections.md)  
  
-   [Autoriser un point de terminaison de mise en miroir de bases de données à utiliser des certificats pour les connexions entrantes &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-inbound-connections.md)  
  
-   [Créer un point de terminaison de mise en miroir de bases de données pour l’authentification Windows &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md)  
  
-   [Établir une session de mise en miroir de bases de données au moyen de l’authentification Windows &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/database-mirroring-establish-session-windows-authentication.md)  
  
-   [Ajouter un témoin de mise en miroir de bases de données à l’aide de l’authentification Windows &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/add-a-database-mirroring-witness-using-windows-authentication-transact-sql.md)  
  
-   [Configurer une base de données miroir pour utiliser la propriété Trustworthy &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/set-up-a-mirror-database-to-use-the-trustworthy-property-transact-sql.md)  
  
 **Transact-SQL/SQL Server Management Studio**  
  
-   [Mise à niveau des instances en miroir](../../database-engine/database-mirroring/upgrading-mirrored-instances.md)  
  
-   [Préparer une base de données miroir pour la mise en miroir &#40;SQL Server&#41;](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md)  
  
-   [Résoudre des problèmes de configuration de mise en miroir de bases de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/troubleshoot-database-mirroring-configuration-sql-server.md)  
  
  
## <a name="see-also"></a>Voir aussi  
 [Mise en miroir de bases de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-sql-server.md)   
 [Mise en miroir de bases de données : interopérabilité et coexistence &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-interoperability-and-coexistence-sql-server.md)   
 [Sécurité du transport de la mise en miroir de bases de données et des groupes de disponibilité Always On &#40;SQL Server&#41;](../../database-engine/database-mirroring/transport-security-database-mirroring-always-on-availability.md)   
 [Spécifier une adresse réseau de serveur &#40;mise en miroir de bases de données&#41;](../../database-engine/database-mirroring/specify-a-server-network-address-database-mirroring.md)  
  
  
