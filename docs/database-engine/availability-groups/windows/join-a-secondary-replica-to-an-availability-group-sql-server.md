---
title: Joindre un réplica secondaire à un groupe de disponibilité
description: Découvrez les étapes à suivre pour joindre un réplica secondaire à un groupe de disponibilité Always On à l’aide de Transact-SQL (T-SQL), PowerShell ou SQL Server Management Studio.
ms.custom: seodec18
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
f1_keywords:
- sql13.swb.availabilitygroup.joinreplica.f1
helpviewer_keywords:
- Availability Groups [SQL Server], availability replicas
- Availability Groups [SQL Server], joining
- Availability Groups [SQL Server], configuring
ms.assetid: e5bd2489-097a-490e-8ea1-34fe48378ad1
author: cawrites
ms.author: chadam
ms.openlocfilehash: e20561c81c732982d5787c05546453b9c7340406
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348463"
---
# <a name="join-a-secondary-replica-to-an-always-on-availability-group"></a>Joindre un réplica secondaire à un groupe de disponibilité Always On
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Cette rubrique explique comment joindre un réplica secondaire à un groupe de disponibilité AlwaysOn à l’aide de [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], de [!INCLUDE[tsql](../../../includes/tsql-md.md)]ou de PowerShell dans [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)]. Une fois qu’un réplica secondaire a été ajouté à un groupe de disponibilité AlwaysOn, le réplica secondaire doit être joint au groupe de disponibilité. L'opération de jointure du réplica doit être effectuée sur l'instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] qui héberge le réplica secondaire.  

  
##  <a name="prerequisites"></a><a name="Prerequisites"></a> Conditions préalables  
  
-   Le réplica principal du groupe de disponibilité doit être actuellement en ligne.    
-   Vous devez être connecté à l'instance de serveur qui héberge un réplica secondaire qui n'a pas encore été joint au groupe de disponibilité.    
-   L'instance de serveur local doit être en mesure de se connecter au point de terminaison de mise en miroir de bases de données de l'instance de serveur qui héberge le réplica principal.  
  
> [!IMPORTANT]  
>  Si aucune condition préalable n'est satisfaite, l'opération de jointure échoue. Après l'échec d'une tentative de jointure, vous devrez peut-être vous connecter à l'instance de serveur qui héberge le réplica principal afin de supprimer et de rajouter le réplica secondaire avant de pouvoir le joindre au groupe de disponibilité. Pour plus d’informations, consultez [Supprimer un réplica secondaire d’un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/remove-a-secondary-replica-from-an-availability-group-sql-server.md) et [Ajouter un réplica secondaire à un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/add-a-secondary-replica-to-an-availability-group-sql-server.md).  
  
##  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Requiert l'autorisation ALTER AVAILABILITY GROUP sur le groupe de disponibilité, l'autorisation CONTROL AVAILABILITY GROUP, l'autorisation ALTER ANY AVAILABILITY GROUP ou l'autorisation CONTROL SERVER.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
 **Pour joindre un réplica de disponibilité à un groupe de disponibilité**  
  
1.  Dans l'Explorateur d'objets, connectez-vous à l'instance de serveur qui héberge le réplica secondaire, puis cliquez sur le nom du serveur pour développer son arborescence.  
  
2.  Développez le nœud **Haute disponibilité AlwaysOn** et le nœud **Groupes de disponibilité** .  
  
3.  Sélectionnez le groupe de disponibilité du réplica secondaire auquel vous êtes connecté.  
  
4.  Cliquez avec le bouton droit sur le réplica secondaire et cliquez sur **Joindre au groupe de disponibilité**.  
  
5.  Cette opération ouvre la boîte de dialogue **Joindre le réplica au groupe de disponibilité** .  
  
6.  Pour joindre le réplica secondaire au groupe de disponibilité, cliquez sur **OK**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
 **Pour joindre un réplica de disponibilité à un groupe de disponibilité**  
  
1.  Connectez-vous à l'instance de serveur qui héberge le réplica secondaire.  
  
2.  Utilisez l'instruction [ALTER AVAILABILITY GROUP](../../../t-sql/statements/alter-availability-group-transact-sql.md) , comme suit :  
  
     ALTER AVAILABILITY GROUP *nom_groupe* JOIN  
  
     où *nom_groupe* correspond au nom du groupe de disponibilité.  
  
     L'exemple suivant joint le réplica secondaire au groupe de disponibilité `MyAG`.  
  
    ```  
    ALTER AVAILABILITY GROUP MyAG JOIN;  
    ```  
  
    > [!NOTE]  
    >  Pour consulter cette instruction [!INCLUDE[tsql](../../../includes/tsql-md.md)] utilisée en contexte, consultez [Créer un groupe de disponibilité &#40;Transact-SQL&#41;](../../../database-engine/availability-groups/windows/create-an-availability-group-transact-sql.md).  
  
##  <a name="using-powershell"></a><a name="PowerShellProcedure"></a> Utilisation de PowerShell  
 **Pour joindre un réplica de disponibilité à un groupe de disponibilité**  
  
 Dans le fournisseur PowerShell [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] :  
  
1.  Remplacez le répertoire (**cd**) par l’instance de serveur qui héberge le réplica secondaire.  
  
2.  Joignez le réplica secondaire au groupe de disponibilité en exécutant l’applet de commande **Join-SqlAvailabilityGroup** avec le nom du groupe de disponibilité.  
  
     Par exemple, la commande suivante joint un réplica secondaire hébergé par l'instance de serveur situé dans le chemin d'accès spécifié au groupe de disponibilité nommé `MyAg`.  Cette instance de serveur doit héberger un réplica secondaire dans ce groupe de disponibilité.  
  
    ```  
    Join-SqlAvailabilityGroup -Path SQLSERVER:\SQL\SecondaryServer\InstanceName -Name 'MyAg'  
    ```  
  
    > [!NOTE]  
    >  Pour voir la syntaxe d’une applet de commande, utilisez l’applet de commande **Get-Help** dans l’environnement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] PowerShell. Pour en savoir plus, voir [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md).  
  
 **Pour configurer et utiliser le fournisseur SQL Server PowerShell**  
  
-   [Fournisseur SQL Server PowerShell](../../../powershell/sql-server-powershell-provider.md)  
  
##  <a name="follow-up-configure-secondary-databases"></a><a name="FollowUp"></a> Suivi : configurer des bases de données secondaires  
 Pour chaque base de données dans le groupe de disponibilité, vous avez besoin d'une base de données secondaire sur l'instance de serveur qui héberge le réplica secondaire. Vous pouvez configurer des bases de données secondaires avant ou après avoir joint un réplica secondaire à un groupe de disponibilité, comme suit :  
  
1.  Restaurez une base de données récente et les sauvegardes de fichier journal de chaque base de données primaire sur l'instance de serveur qui héberge le réplica secondaire, à l'aide de RESTORE WITH NORECOVERY pour chaque opération de restauration. Pour plus d’informations, consultez [Préparer manuellement une base de données secondaire pour un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/manually-prepare-a-secondary-database-for-an-availability-group-sql-server.md).  
  
2.  Joignez chaque base de données secondaire au groupe de disponibilité. Pour plus d’informations, consultez [Joindre une base de données secondaire à un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-database-to-an-availability-group-sql-server.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Création et configuration des groupes de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/creation-and-configuration-of-availability-groups-sql-server.md)   
 [Vue d’ensemble des groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Résoudre des problèmes de configuration des groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/troubleshoot-always-on-availability-groups-configuration-sql-server.md)  
  
