---
title: Mettre en suspens une base de données de disponibilité
description: Découvrez comment mettre en suspens le déplacement des données d’une base de données d’un groupe de disponibilité Always On à l’aide de SQL Server Management Studio, Transact-SQL ou PowerShell.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
f1_keywords:
- sql13.swb.availabilitygroup.suspenddatamove.f1
helpviewer_keywords:
- secondary databases [SQL Server], in availability group
- primary databases [SQL Server], in availability group
- Availability Groups [SQL Server], suspending a database
- Availability Groups [SQL Server], databases
ms.assetid: 86858982-6af1-4e80-9a93-87451f0d7ee9
author: cawrites
ms.author: chadam
ms.openlocfilehash: f918daa031b25ba8f1110dc9f2aaeaebef0a9361
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97642966"
---
# <a name="suspend-an-availability-database-sql-server"></a>Interrompre une base de données de disponibilité (SQL Server)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Vous pouvez interrompre une base de données de disponibilité dans [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], de [!INCLUDE[tsql](../../../includes/tsql-md.md)]ou de PowerShell dans [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)]. Notez qu'une commande d'interruption doit être émise sur l'instance du serveur qui héberge la base de données à interrompre ou à reprendre.  
  
 L'effet d'une commande d'interruption varie selon que vous interrompez une base de données principale ou secondaire, comme suit :  
  
|Base de données interrompue|Effet de la commande d'interruption|  
|------------------------|-------------------------------|  
|Base de données secondaire|Seule la base de données secondaire locale est interrompue et son état de synchronisation devient NOT SYNCHRONIZING. Les autres bases de données secondaires ne sont pas affectées. La base de données interrompue cesse de recevoir et d'appliquer des données (enregistrements de journal) et commence à se situer en retrait par rapport à la base de données principale. Les connexions existantes sur le réplica secondaire accessible en lecture restent utilisables. Les nouvelles connexions à la base de données suspendue sur le réplica secondaire accessible en lecture ne sont pas autorisées tant que le déplacement des données n'a pas repris. Ce comportement s’applique uniquement lorsque les connexions sont ouvertes à l’aide de l’écouteur et du routage en lecture seule.<br /><br /> La base de données primaire reste disponible. Si vous interrompez chaque base de données secondaire correspondante, la base de données principale est exposée.<br /><br /> **\*\* Important \*\*** Quand une base de données secondaire est suspendue, la file d’attente d’envoi de la base de données primaire correspondante accumule des enregistrements du journal des transactions non envoyés. Les connexions au réplica secondaire retournent des données qui étaient disponibles lorsque le déplacement des données a été suspendu.|  
|Base de données principale|La base de données principale cesse le déplacement des données vers chaque base de données secondaire connectée. La base de données principale continue à s'exécuter, en mode exposé. La base de données principale reste à la disposition des clients, les connexions existantes sur un réplica secondaire accessible en lecture restent utilisables et de nouvelles connexions peuvent être établies.|  
  
> [!NOTE]  
>  La suspension d’une base de données secondaire Always On n’affectent pas directement la disponibilité de la base de données primaire. Toutefois, l'interruption d'une base de données secondaire peut avoir une incidence sur les capacités de redondance et de basculement de la base de données principale. Ce comportement diffère d'une mise en miroir de bases de données dans laquelle l'état de mise en miroir est interrompu à la fois sur la base de données miroir et la base de données principale. L’interruption d’une base de données principale Always On interrompt le déplacement des données sur toutes les bases de données secondaires correspondantes, et les capacités de redondance et de basculement cessent pour cette base de données tant que la base de données principale n’a pas repris.  
  
-   **Avant de commencer :**  
  
     [Limitations et restrictions](#Restrictions)  
  
     [Composants requis](#Prerequisites)  
  
     [Recommandations](#Recommendations)  
  
     [Sécurité](#Security)  
  
-   **Pour interrompre une base de données à l'aide de :**  
  
-   [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
     [PowerShell](#PowerShellProcedure)  
  
-   **Suivi :** [Comment éviter un journal des transactions plein](#FollowUp)  
  
-   [Tâches associées](#RelatedTasks)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitations et restrictions  
 Une commande SUSPEND retourne dès qu'elle est acceptée par le réplica qui héberge la base de données cible, mais en réalité, l'interruption de la base de données se produit de façon asynchrone.  
  
###  <a name="prerequisites"></a><a name="Prerequisites"></a> Conditions préalables  
 Vous devez être connecté à l'instance de serveur qui héberge la base de données que vous souhaitez interrompre. Pour interrompre une base de données primaire et les bases de données secondaires correspondantes, connectez-vous à l'instance de serveur qui héberge le réplica principal. Pour interrompre une base de données secondaire tout en laissant la base de données primaire disponible, connectez-vous au réplica secondaire.  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Recommandations  
 Lors de goulots d'étranglement, l'interruption d'une ou plusieurs bases de données secondaires, même brièvement, peut être utile pour améliorer les performances temporairement sur le réplica principal. Tant qu'une base de données secondaire reste suspendue, le journal des transactions de la base de données primaire correspondante ne peut pas être tronqué. Cela provoque l'accumulation des enregistrements du journal sur la base de données primaire. Par conséquent, nous vous recommandons de reprendre, ou de supprimer, une base de données secondaire suspendue rapidement. Pour plus d’informations, consultez [Suivi : Comment éviter un journal des transactions plein](#FollowUp) plus loin dans cette rubrique.  
  
###  <a name="security"></a><a name="Security"></a> Sécurité  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Nécessite l'autorisation ALTER sur la base de données.  
  
 Requiert l'autorisation ALTER AVAILABILITY GROUP sur le groupe de disponibilité, l'autorisation CONTROL AVAILABILITY GROUP, l'autorisation ALTER ANY AVAILABILITY GROUP ou l'autorisation CONTROL SERVER.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
 **Pour interrompre une base de données**  
  
1.  Dans l'Explorateur d'objets, connectez-vous à l'instance de serveur qui héberge le réplica de disponibilité sur lequel vous souhaitez interrompre une base de données et développez l'arborescence du serveur. Pour plus d'informations, consultez [Conditions préalables requises](#Prerequisites), plus haut dans cette rubrique.  
  
2.  Développez le nœud **Haute disponibilité AlwaysOn** et le nœud **Groupes de disponibilité** .  
  
3.  Développez le groupe de disponibilité.  
  
4.  Développez le nœud **Bases de données de disponibilité** , cliquez avec le bouton droit sur la base de données, puis sélectionnez **Suspendre le déplacement des données**.  
  
5.  Dans la boîte de dialogue **Suspendre le déplacement des données** , cliquez sur **OK**.  
  
     L'Explorateur d'objets indique que la base de données est interrompue en modifiant l'icône de base de données de sorte qu'elle affiche un indicateur de pause.  
  
> [!NOTE]  
>  Pour interrompre des bases de données supplémentaires sur cet emplacement de réplica, répétez les étapes 4 et 5 pour chaque base de données.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
 **Pour interrompre une base de données**  
  
1.  Connectez-vous à l'instance de serveur qui héberge le réplica dont vous souhaitez interrompre la base de données. Pour plus d'informations, consultez [Conditions préalables requises](#Prerequisites), plus haut dans cette rubrique.  
  
2.  Suspendez la base de données à l’aide de l’instruction [ALTER DATABASE](../../../t-sql/statements/alter-database-transact-sql-set-hadr.md) suivante :  
  
     ALTER DATABASE *nom_base_de_données* SET HADR SUSPEND;
  
##  <a name="using-powershell"></a><a name="PowerShellProcedure"></a> Utilisation de PowerShell  
 **Pour interrompre une base de données**  
  
1.  Remplacez le répertoire (**cd**) par l’instance de serveur qui héberge le réplica dont vous souhaitez interrompre la base de données. Pour plus d'informations, consultez [Conditions préalables requises](#Prerequisites), plus haut dans cette rubrique.  
  
2.  Utilisez l’applet de commande **Suspend-SqlAvailabilityDatabase** pour interrompre le groupe de disponibilité.  
  
     Par exemple, la commande suivante interrompt la synchronisation des données pour la base de données de disponibilité `MyDb3` dans le groupe de disponibilité `MyAg` sur l’instance de serveur nommée `Computer\Instance`.  
  
    ```  
    Suspend-SqlAvailabilityDatabase `   
    -Path SQLSERVER:\Sql\Computer\Instance\AvailabilityGroups\MyAg\Databases\MyDb3  
    ```  
  
    > [!NOTE]  
    >  Pour voir la syntaxe d’une applet de commande, utilisez l’applet de commande **Get-Help** dans l’environnement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] PowerShell. Pour en savoir plus, voir [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md).  
  
 **Pour configurer et utiliser le fournisseur SQL Server PowerShell**  
  
-   [Fournisseur SQL Server PowerShell](../../../powershell/sql-server-powershell-provider.md)  
  
##  <a name="follow-up-avoiding-a-full-transaction-log"></a><a name="FollowUp"></a> Suivi : Comment éviter un journal des transactions plein  
 En général, lorsqu'un point de contrôle automatique est effectué sur une base de données, son journal des transactions est tronqué jusqu'à ce point de contrôle après la sauvegarde du journal suivante. Toutefois, pendant qu'une base de données secondaire est interrompue, tous les enregistrements de journal en cours restent actifs sur la base de données primaire. Si le journal des transactions est saturé (soit parce qu'il a atteint sa taille maximale, soit parce que l'instance de serveur manque de place), la base de données ne peut plus effectuer de mises à jour.  
  
 Pour éviter ce problème, vous devez procédez de l'une des manières suivantes :  
  
-   Ajoutez davantage d'espace de journal pour la base de données primaire.  
  
-   Reprenez la base de données secondaire avant que le journal ne soit saturé. Pour plus d'informations, consultez [Reprendre une base de données de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/resume-an-availability-database-sql-server.md).  
  
-   Supprimez la base de données secondaire. Pour plus d’informations, consultez [Supprimer une base de données secondaire d’un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/remove-a-secondary-database-from-an-availability-group-sql-server.md).  
  
 **Pour résoudre les problèmes liés à la saturation du journal des transactions**  
  
-   [Résoudre les problèmes liés à un journal des transactions saturé &#40;erreur SQL Server 9002&#41;](../../../relational-databases/logs/troubleshoot-a-full-transaction-log-sql-server-error-9002.md)  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Tâches associées  
  
-   [Reprendre une base de données de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/resume-an-availability-database-sql-server.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Vue d’ensemble des groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Reprendre une base de données de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/resume-an-availability-database-sql-server.md)  
  
