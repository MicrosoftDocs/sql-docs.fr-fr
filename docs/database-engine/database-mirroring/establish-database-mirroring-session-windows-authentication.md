---
title: Configurer la mise en miroir de bases de données (authentification Windows)
description: Découvrez comment configurer une session de mise en miroir de bases de données avec l’authentification Windows à l’aide de SQL Server Management Studio.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- database mirroring [SQL Server], sessions
ms.assetid: 7cb418d6-dce1-4a0d-830e-9c5ccfe3bd72
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: ece2afa5381b2692c3cb3b99358fc02891e75e97
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97644306"
---
# <a name="establish-database-mirroring-session---windows-authentication"></a>Établir une session de mise en miroir de base de données - authentification Windows
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
    
> [!NOTE]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Utilisez [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] à la place.  
  
 Pour établir une session de mise en miroir de bases de données et modifier les propriétés de la mise en miroir d'une base de données, utilisez la page **Mise en miroir** de la boîte de dialogue **Propriétés de la base de données** . Avant d'utiliser la page **Mise en miroir** pour configurer la mise en miroir de bases de données, assurez -vous que les conditions suivantes sont remplies :  
  
-   Les instances du principal de serveur et du serveur miroir doivent exécuter la même édition de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (Standard ou Enterprise). En outre, nous vous conseillons vivement d'exécuter les instances sur des systèmes comparables pouvant gérer des charges de travail identiques.  
  
    > [!NOTE]  
    >  Une instance de serveur témoin n'est pas disponible dans toutes les éditions de [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour obtenir la liste des fonctionnalités prises en charge par les éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez [Fonctionnalités prise en charge par les éditions de SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).  
  
-   La base de données miroir doit exister et être active.  
  
     La création d'une base de données miroir nécessite la restauration d'une sauvegarde récente de la base de données principale (au moyen de WITH NORECOVERY) sur l'instance de serveur miroir. Elle requiert également de prendre une ou plusieurs sauvegardes de journaux après la sauvegarde complète et de les restaurer dans l'ordre dans la base de données miroir (à l'aide de WITH NORECOVERY). Pour plus d’informations, consultez [Préparer une base de données miroir pour la mise en miroir &#40;SQL Server&#41;](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md).  
  
-   Si les instances de serveurs s'exécutent sous différents comptes d'utilisateurs de domaine, chacune requiert une connexion dans la base de données **master** des autres. Si la connexion n'existe pas, vous devez la créer avant de configurer la mise en miroir. Pour plus d’informations, consultez [Autoriser l’accès sur le réseau à un point de terminaison de mise en miroir de bases de données au moyen de l’authentification Windows &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-allow-network-access-windows-authentication.md).  
  
### <a name="to-configure-database-mirroring"></a>Pour configurer la mise en miroir de bases de données  
  
1.  Après vous être connecté à l'instance du serveur principal, dans l'Explorateur d'objets, cliquez sur le nom du serveur pour développer son arborescence.  
  
2.  Développez **Bases de données** et sélectionnez la base de données à mettre en miroir.  
  
3.  Cliquez avec le bouton droit sur la base de données, sélectionnez **Tâches**, puis cliquez sur **Miroir**. La page **Mise en miroir** de la boîte de dialogue **Propriétés de la base de données** s'affiche.  
  
4.  Pour commencer à configurer la mise en miroir, cliquez sur le bouton **Configurer la sécurité** afin de lancer l'Assistant Configurer la sécurité de mise en miroir de bases de données.  
  
    > [!NOTE]  
    >  Durant une session de mise en miroir de bases de données, vous pouvez utiliser cet Assistant uniquement pour ajouter ou modifier l'instance de serveur témoin.  
  
5.  L’Assistant Configurer la sécurité de mise en miroir de bases de données crée automatiquement le point de terminaison de mise en miroir de bases de données (s’il n’en existe aucun) sur chaque instance de serveur et il entre ses adresses réseau de serveur dans le champ correspondant au rôle de l’instance de serveur (**Principal**, **Miroir** ou **Témoin**).  
  
    > [!IMPORTANT]  
    >  Lors de la création d'un point de terminaison, l'Assistant Configurer la sécurité de mise en miroir de bases de données utilise toujours l'authentification Windows. Pour que vous puissiez utiliser l'Assistant avec l'authentification basée sur certificat, le point de terminaison de mise en miroir doit déjà avoir été configuré de façon à utiliser les certificats sur chacune des instances de serveur. En outre, tous les champs de la boîte de dialogue **Comptes de service** de l'Assistant doivent rester vides. Pour plus d’informations sur la création d’un point de terminaison de mise en miroir de bases de données, consultez [CREATE ENDPOINT &#40;Transact-SQL&#41;](../../t-sql/statements/create-endpoint-transact-sql.md).  
  
6.  Si vous le souhaitez, vous pouvez modifier le mode d'opération. La définition d'une adresse TCP pour un témoin détermine la disponibilité de tel ou tel mode d'opération. Les options disponibles sont les suivantes :  
  
    |Option|Témoin ?|Explication|  
    |------------|--------------|-----------------|  
    |**Haute performance (asynchrone)**|Nul (s'il existe, non utilisé mais la session requiert un quorum)|Pour optimiser les performances, la base de données miroir reste toujours en léger décalage par rapport à la base de données principale, sans jamais complètement le rattraper. Toutefois, l'écart entre les bases de données est généralement faible. La perte d'un partenaire a les effets suivants :<br /><br /> Si l'instance de serveur miroir devient non disponible, le principal continue.<br /><br /> Si l'instance de serveur principal devient indisponible, le miroir s'arrête ; mais si la session n'a aucun témoin (comme recommandé) ou si le témoin est connecté au serveur miroir, celui-ci est accessible en tant que secours semi-automatique ; le propriétaire de la base de données peut forcer le service sur l'instance de serveur miroir (avec une perte de données possible).<br /><br /> <br /><br /> Pour plus d’informations, consultez [Basculement de rôle durant une session de mise en miroir de bases de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/role-switching-during-a-database-mirroring-session-sql-server.md).|  
    |**Haute sécurité sans basculement automatique (synchrone)**|Non|Ce mode garantit que toutes les transactions validées sont écrites sur disque sur le serveur miroir.<br /><br /> Le basculement manuel est possible lorsque les partenaires sont connectés entre eux et que la base de données est synchronisée.<br /><br /> La perte d'un partenaire a les effets suivants :<br /><br /> Si l'instance de serveur miroir devient non disponible, le principal continue.<br /><br /> Si l'instance de serveur principal devient indisponible, l'instance miroir s'arrête mais reste accessible en tant que secours semi-automatique ; le propriétaire de la base de données peut forcer le service sur l'instance de serveur miroir (avec une perte de données possible).<br /><br /> <br /><br /> Pour plus d’informations, consultez [Basculement de rôle durant une session de mise en miroir de bases de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/role-switching-during-a-database-mirroring-session-sql-server.md).|  
    |**Haute sécurité avec basculement automatique (synchrone)**|Oui (requis)|Ce mode garantit que toutes les transactions validées sont écrites sur disque sur le serveur miroir.<br /><br /> La disponibilité est optimisée par l'inclusion d'une instance de serveur témoin pour prendre en charge le basculement automatique. Notez que vous pouvez sélectionner l’option **Haute sécurité avec basculement automatique (synchrone)** seulement si vous avez spécifié au préalable une adresse de serveur témoin.<br /><br /> Le basculement manuel est possible lorsque les partenaires sont connectés entre eux et que la base de données est synchronisée.<br /><br /> En présence d'un témoin, la conséquence de la perte d'un partenaire est la suivante :<br /><br /> Si l'instance de serveur principal devient non disponible, un basculement automatique se produit. L'instance de serveur miroir prend le rôle de principal et propose sa base de données comme base de données principale.<br /><br /> Si l'instance de serveur miroir devient non disponible, le principal continue.<br /><br /> <br /><br /> Pour plus d’informations, consultez [Basculement de rôle durant une session de mise en miroir de bases de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/role-switching-during-a-database-mirroring-session-sql-server.md).<br /><br /> **&#42;&#42; Important &#42;&#42;** Si le témoin est déconnecté, les partenaires doivent être connectés entre eux pour que la base de données soit disponible. Pour plus d’informations, consultez [Quorum : effets d’un témoin sur la disponibilité de la base de données &#40;mise en miroir de bases de données&#41;](../../database-engine/database-mirroring/quorum-how-a-witness-affects-database-availability-database-mirroring.md).|  
  
7.  Lorsque toutes les conditions suivantes sont réunies, cliquez sur **Démarrer la mise en miroir** pour commencer la mise en miroir :  
  
    -   Vous êtes actuellement connecté à l'instance de serveur principal.  
  
    -   La sécurité a été correctement configurée.  
  
    -   Les adresses TCP complètes des instances du principal et du serveur miroir sont spécifiées (dans la section **Adresses réseau du serveur** ).  
  
    -   Si le mode d’opération est **Haute sécurité avec basculement automatique (synchrone)** , l’adresse TCP complète de l’instance de serveur témoin est également spécifiée.  
  
8.  Une fois que la mise en miroir a démarré, vous pouvez changer le mode d'opération et enregistrer la modification en cliquant sur **OK**. Vous pouvez passer en mode haute sécurité avec basculement automatique si vous avez préalablement spécifié une adresse de serveur témoin.  
  
    > [!NOTE]  
    >  Pour supprimer le témoin, supprimez son adresse réseau de serveur du champ **Témoin** . Si vous passez du mode de sécurité élevée avec basculement automatique au mode haute performance, le champ **Témoin** est automatiquement supprimé.  
  
## <a name="see-also"></a>Voir aussi  
 [Basculement de rôle durant une session de mise en miroir de bases de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/role-switching-during-a-database-mirroring-session-sql-server.md)   
 [Préparer une base de données miroir pour la mise en miroir &#40;SQL Server&#41;](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md)   
 [Propriétés de la base de données &#40;page Mise en miroir&#41;](../../relational-databases/databases/database-properties-mirroring-page.md)   
 [Suspendre ou reprendre une session de mise en miroir de bases de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/pause-or-resume-a-database-mirroring-session-sql-server.md)   
 [Configurer une base de données miroir pour utiliser la propriété Trustworthy &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/set-up-a-mirror-database-to-use-the-trustworthy-property-transact-sql.md)   
 [Supprimer une mise en miroir de bases de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/remove-database-mirroring-sql-server.md)   
 [Gestion des connexions et des travaux après un basculement de rôle &#40;SQL Server&#41;](../../sql-server/failover-clusters/management-of-logins-and-jobs-after-role-switching-sql-server.md)   
 [Configuration de la mise en miroir d’une base de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/setting-up-database-mirroring-sql-server.md)   
 [Gérer les métadonnées durant la mise à disposition d’une base de données sur une autre instance de serveur &#40;SQL Server&#41;](../../relational-databases/databases/manage-metadata-when-making-a-database-available-on-another-server.md)   
 [Ajouter ou remplacer un témoin de mise en miroir de bases de données &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/add-or-replace-a-database-mirroring-witness-sql-server-management-studio.md)  
  
  
