---
title: Joindre une base de données secondaire à un groupe de disponibilité
description: Découvrez les étapes à suivre pour joindre une base de données secondaire à un groupe de disponibilité Always On à l’aide de Transact-SQL (T-SQL), PowerShell ou SQL Server Management Studio.
ms.custom: seodec18
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
f1_keywords:
- sql13.swb.availabilitygroup.joindbs.f1
helpviewer_keywords:
- secondary databases [SQL Server], in availability group
- secondary databases [SQL Server]
- Availability Groups [SQL Server], joining
- Availability Groups [SQL Server], configuring
- Availability Groups [SQL Server], databases
ms.assetid: fd7efe79-c1f9-497d-bfe7-b2a2b2321cf5
author: cawrites
ms.author: chadam
ms.openlocfilehash: 0ac5ab1195dcc34ebb7dcf3b30ef41a190f9a3eb
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97644256"
---
# <a name="join-a-secondary-database-to-an-always-on-availability-group"></a>Joindre une base de données secondaire à un groupe de disponibilité Always On
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Cette rubrique explique comment joindre une base de données secondaire à un groupe de disponibilité Always On à l’aide de [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], de [!INCLUDE[tsql](../../../includes/tsql-md.md)]ou de PowerShell dans [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)]. Après avoir préparé une base de données secondaire pour un réplica secondaire, vous devez joindre la base de données au groupe de disponibilité dès que possible. Cette opération lance le déplacement des données entre la base de données primaire correspondante et la base de données secondaire.  
   
> [!NOTE]  
>  Pour plus d’informations sur ce qui se produit après avoir joint une base de données secondaire au groupe, consultez [Vue d’ensemble des groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md).  
   
##  <a name="prerequisites"></a><a name="Prerequisites"></a> Conditions préalables  
  
-   Vous devez être connecté à l'instance de serveur qui héberge le réplica secondaire.  
  
-   Le réplica secondaire doit déjà être joint au groupe de disponibilité. Pour plus d’informations, consultez [Joindre un réplica secondaire à un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-replica-to-an-availability-group-sql-server.md).  
  
-   La base de données secondaire doit avoir été préparée récemment. Pour plus d’informations, consultez [Préparer manuellement une base de données secondaire pour un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/manually-prepare-a-secondary-database-for-an-availability-group-sql-server.md).  
  
###  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Requiert l'autorisation ALTER AVAILABILITY GROUP sur le groupe de disponibilité, l'autorisation CONTROL AVAILABILITY GROUP, l'autorisation ALTER ANY AVAILABILITY GROUP ou l'autorisation CONTROL SERVER.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
 **Pour joindre une base de données secondaire à un groupe de disponibilité**  
  
1.  Dans l'Explorateur d'objets, connectez-vous à l'instance de serveur qui héberge le réplica secondaire et développez l'arborescence du serveur.  
  
2.  Développez le nœud **Haute disponibilité AlwaysOn** et le nœud **Groupes de disponibilité** .  
  
3.  Développez le groupe de disponibilité que vous souhaitez modifier, puis développez le nœud **Bases de données de disponibilité** .  
  
4.  Cliquez avec le bouton droit sur la base de données, puis cliquez sur **Joindre au groupe de disponibilité**.  
  
5.  Cette opération ouvre la boîte de dialogue **Joindre les bases de données au groupe de disponibilité** . Vérifiez le nom du groupe de disponibilité affiché dans la barre de titre, ainsi que le nom de la ou des bases de données affichées dans la grille, puis cliquez sur **OK**, ou cliquez sur **Annuler**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
 **Pour joindre une base de données secondaire à un groupe de disponibilité**  
  
1.  Connectez-vous à l'instance de serveur qui héberge le réplica secondaire.  
  
2.  Utilisez la [clause SET HADR de l'instruction ALTER DATABASE](../../../t-sql/statements/alter-database-transact-sql-set-hadr.md) comme suit :  
  
     ALTER DATABASE *nom_base_de_données* SET HADR AVAILABILITY GROUP = *nom_groupe*  
  
     où *nom_base_de_données* est le nom d’une base de données à joindre et *nom_groupe* est le nom du groupe de disponibilité .  
  
     L'exemple suivant joint la base de données secondaire, `Db1`, au réplica secondaire local du groupe de disponibilité `MyAG`.  
  
    ```  
    ALTER DATABASE Db1 SET HADR AVAILABILITY GROUP = MyAG;  
    ```  
  
    > [!NOTE]  
    >  Pour consulter cette instruction [!INCLUDE[tsql](../../../includes/tsql-md.md)] utilisée en contexte, consultez [Créer un groupe de disponibilité &#40;Transact-SQL&#41;](../../../database-engine/availability-groups/windows/create-an-availability-group-transact-sql.md).  
  
##  <a name="using-powershell"></a><a name="PowerShellProcedure"></a> Utilisation de PowerShell  
 **Pour joindre une base de données secondaire à un groupe de disponibilité**  
  
1.  Remplacez le répertoire (**cd**) par l’instance de serveur qui héberge le réplica secondaire.  
  
2.  Utilisez l’applet de commande **Add-SqlAvailabilityDatabase** pour joindre une ou plusieurs bases de données secondaires au groupe de disponibilité.  
  
     Par exemple, la commande suivante joint une base de données secondaire, `Db1`, au groupe de disponibilité `MyAG` sur l'une des instances de serveur qui héberge un réplica secondaire.  
  
    ```  
    Add-SqlAvailabilityDatabase `   
    -Path SQLSERVER:\SQL\SecondaryServer\InstanceName\AvailabilityGroups\MyAG `   
    -Database "Db1"  
    ```  
  
    > [!NOTE]  
    >  Pour voir la syntaxe d’une applet de commande, utilisez l’applet de commande **Get-Help** dans l’environnement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] PowerShell. Pour en savoir plus, voir [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md).  
  
 **Pour configurer et utiliser le fournisseur SQL Server PowerShell**  
  
-   [Fournisseur SQL Server PowerShell](../../../powershell/sql-server-powershell-provider.md)  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Tâches associées  
  
-   [Joindre un réplica secondaire à un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-replica-to-an-availability-group-sql-server.md)  
  
-   [Préparer manuellement une base de données secondaire pour un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/manually-prepare-a-secondary-database-for-an-availability-group-sql-server.md)  
  
## <a name="see-also"></a>Voir aussi  
 [ALTER AVAILABILITY GROUP &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-availability-group-transact-sql.md)   
 [Vue d’ensemble des groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Résoudre des problèmes de configuration des groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/troubleshoot-always-on-availability-groups-configuration-sql-server.md)  
  
