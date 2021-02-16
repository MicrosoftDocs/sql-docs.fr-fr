---
title: Créer un groupe de disponibilité à l’aide de PowerShell
description: Découvrez comment utiliser les applets de commande PowerShell pour créer et configurer un groupe de disponibilité Always On à l’aide de PowerShell dans SQL Server 2019 (15.x).
ms.custom: seodec18
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
helpviewer_keywords:
- Availability Groups [SQL Server], creating
ms.assetid: bc69a7df-20fa-41e1-9301-11317c5270d2
author: cawrites
ms.author: chadam
ms.openlocfilehash: 4120b9f72b77056753b842317e2271ffdd18b370
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343344"
---
# <a name="create-an-always-on-availability-group-using-powershell"></a>Créer un groupe de disponibilité Always On à l’aide de PowerShell
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Cette rubrique explique comment utiliser des applets de commande PowerShell pour créer et configurer un groupe de disponibilité Always On à l’aide de PowerShell dans [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)]. Un *groupe de disponibilité* définit un jeu de bases de données utilisateur qui basculent en tant qu'unité unique et un jeu de partenaires de basculement, appelés *réplicas de disponibilité*, qui prennent en charge le basculement.  
  
> [!NOTE]  
> Pour obtenir une présentation des groupes de disponibilité, consultez [Vue d’ensemble des groupes de disponibilité Always On &#40;SQL Server&#41;](~/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md).  
  
> [!NOTE]  
> Comme alternative à l'utilisation des applets de commande PowerShell, vous pouvez utiliser l'Assistant Création d'un groupe de disponibilité ou [!INCLUDE[tsql](../../../includes/tsql-md.md)]. Pour plus d’informations, consultez [Utiliser la boîte de dialogue Nouveau groupe de disponibilité &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-new-availability-group-dialog-box-sql-server-management-studio.md) ou [Créer un groupe de disponibilité &#40;Transact-SQL&#41;](../../../database-engine/availability-groups/windows/create-an-availability-group-transact-sql.md).  

## <a name="before-you-begin"></a>Avant de commencer
### <a name="prerequisites-restrictions-and-recommendations"></a><a name="PrerequisitesRestrictions"></a> Conditions préalables requises, restrictions et recommandations  

- Avant de créer un groupe de disponibilité, vérifiez que les instances hôtes de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] résident chacune sur un nœud WSFC (Clustering de basculement Windows Server) différent au sein du même clustering de basculement WSFC. En outre, vérifiez que vos instances de serveur respectent les autres conditions préalables applicables, que toutes les autres spécifications [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] sont respectées et que vous avez pris note des recommandations. Pour plus d’informations, nous vous conseillons vivement de lire la rubrique [Conditions préalables requises, restrictions et recommandations pour les groupes de disponibilité Always On &#40;SQL Server&#41;](~/database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md).  

### <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Requiert l’appartenance au rôle serveur fixe **sysadmin** et l’autorisation de serveur CREATE AVAILABILITY GROUP, l’autorisation ALTER ANY AVAILABILITY GROUP ou l’autorisation CONTROL SERVER.  

## <a name="using-powershell-to-create-and-configure-an-availability-group"></a><a name="PowerShellProcedure"></a> Utilisation de PowerShell pour créer et configurer un groupe de disponibilité  
 
Le tableau suivant répertorie les tâches de base impliquées dans la configuration d'un groupe de disponibilité et indique celles qui sont prises en charge par les applets de commande PowerShell. Les tâches [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] doivent être effectuées dans la séquence dans laquelle elles sont présentées dans le tableau.  
  
|Tâche|Applets de commande PowerShell (le cas échéant) ou instruction Transact-SQL|Où effectuer la tâche|  
|----------|--------------------------------------------------------------------|---------------------------------|  
|Créer le point de terminaison de mise en miroir de bases de données (une fois par instance [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] )|**New-SqlHadrEndPoint**|Exécutez sur chaque instance de serveur dans laquelle le point de terminaison de mise en miroir de bases de données est manquant.<br /><br />Pour modifier un point de terminaison de mise en miroir de bases de données existant, utilisez **Set-SqlHadrEndpoint**.|  
|Créer un groupe de disponibilité|En premier lieu, utilisez l’applet de commande **New-SqlAvailabilityReplica** avec le paramètre **-AsTemplate** pour créer un objet en mémoire du réplica de disponibilité pour chacun des deux réplicas de disponibilité que vous envisagez d’inclure dans le groupe de disponibilité.<br /><br /> Créez ensuite le groupe de disponibilité en utilisant l’applet de commande **New-SqlAvailabilityGroup** et en référençant vos objets de réplica de disponibilité.|Exécutez sur l'instance de serveur qui hébergera le réplica principal initial.|  
|Joindre le réplica secondaire au groupe de disponibilité|**Join-SqlAvailabilityGroup**|Exécutez sur chaque instance de serveur qui héberge un réplica secondaire.|  
|Préparer la base de données secondaire|**Backup-SqlDatabase** et **Restore-SqlDatabase**|Créez des sauvegardes sur l'instance de serveur qui héberge le réplica principal.<br /><br /> Restaurez les sauvegardes sur chaque instance de serveur qui héberge un réplica secondaire, à l’aide du paramètre de restauration **NoRecovery** . Si les chemins de fichier diffèrent entre les ordinateurs qui hébergent le réplica principal et le réplica secondaire cible, utilisez également le paramètre de restauration **RelocateFile** .|  
|Démarrer la synchronisation des données en joignant chaque base de données secondaire au groupe de disponibilité|**Add-SqlAvailabilityDatabase**|Exécutez sur chaque instance de serveur qui héberge un réplica secondaire.|  
  
> [!NOTE]
> Pour effectuer les tâches données, remplacez le répertoire (**cd**) par l’instance ou les instances de serveur indiquées.  

## <a name="using-powershell"></a>Utilisation de PowerShell

Configurez et utilisez le [fournisseur SQL Server PowerShell](../../../powershell/sql-server-powershell-provider.md). 

> [!NOTE]  
> Pour afficher la syntaxe et un exemple d’une applet de commande, utilisez l’applet de commande **Get-Help** dans l’environnement PowerShell [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Pour en savoir plus, voir [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md).  

1. Remplacez le répertoire (**cd**) par l’instance de serveur qui doit héberger le réplica principal.  
  
1. Créez un objet réplica de disponibilité en mémoire pour le réplica principal.  
  
1. Créez un objet réplica de disponibilité en mémoire pour chacun des réplicas secondaires.  
  
1. Créez le groupe de disponibilité.  
  
    > [!NOTE]  
    > La longueur maximale d'un nom de groupe de disponibilité est de 128 caractères.  

1. Joignez le nouveau réplica secondaire au groupe de disponibilité. Consultez [Joindre un réplica secondaire à un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-replica-to-an-availability-group-sql-server.md).  
  
1. Pour chaque base de données dans le groupe de disponibilité, créez une base de données secondaire en restaurant des sauvegardes récentes de la base de données primaire, à l'aide de RESTORE WITH NORECOVERY.  
  
1. Joignez chaque nouvelle base de données secondaire au groupe de disponibilité. Consultez [Joindre un réplica secondaire à un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-replica-to-an-availability-group-sql-server.md).  
  
1. (Facultatif) Utilisez la commande **dir** Windows pour vérifier le contenu du nouveau groupe de disponibilité.  
  
> [!NOTE]  
> Si les comptes de service [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] des instances serveur s'exécutent sous des comptes utilisateur de domaine différents, sur chaque instance serveur, créez une connexion pour l'autre instance serveur et accordez l'autorisation CONNECT sur le point de terminaison de mise en miroir de bases de données local.  

### <a name="example"></a><a name="ExampleConfigureGroup"></a> Exemple
L'exemple PowerShell suivant crée et configure un groupe de disponibilité simple nommé `<myAvailabilityGroup>` avec deux réplicas de disponibilité et une base de données de disponibilité. L'exemple :  

1. Sauvegarde `<myDatabase>` et son journal des transactions.  

1. Restaure `<myDatabase>` et son journal des transactions, à l’aide de l’option **-NoRecovery** .  

1. Crée une représentation en mémoire du réplica principal, qui sera hébergée par l'instance locale de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] (nommée `PrimaryComputer\Instance`).  

1. Crée une représentation en mémoire du réplica secondaire, qui sera hébergée par une instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] (nommée `SecondaryComputer\Instance`).  

1. Crée un groupe de disponibilité nommé `<myAvailabilityGroup>`.  

1. Joint le réplica secondaire au groupe de disponibilité.  

1. Joint la base de données secondaire au groupe de disponibilité.  

```powershell
# Backup my database and its log on the primary  
Backup-SqlDatabase `  
    -Database "<myDatabase>" `  
    -BackupFile "\\share\backups\<myDatabase>.bak" `  
    -ServerInstance "PrimaryComputer\Instance"  
  
Backup-SqlDatabase `  
    -Database "<myDatabase>" `  
    -BackupFile "\\share\backups\<myDatabase>.log" `  
    -ServerInstance "PrimaryComputer\Instance" `  
    -BackupAction Log   
  
# Restore the database and log on the secondary (using NO RECOVERY)  
Restore-SqlDatabase `  
    -Database "<myDatabase>" `  
    -BackupFile "\\share\backups\<myDatabase>.bak" `  
    -ServerInstance "SecondaryComputer\Instance" `  
    -NoRecovery  
  
Restore-SqlDatabase `  
    -Database "<myDatabase>" `  
    -BackupFile "\\share\backups\<myDatabase>.log" `  
    -ServerInstance "SecondaryComputer\Instance" `  
    -RestoreAction Log `  
    -NoRecovery  
  
# Create an in-memory representation of the primary replica.  
$primaryReplica = New-SqlAvailabilityReplica `  
    -Name "PrimaryComputer\Instance" `  
    -EndpointURL "TCP://PrimaryComputer.domain.com:5022" `  
    -AvailabilityMode "SynchronousCommit" `  
    -FailoverMode "Automatic" `  
    -Version 12 `  
    -AsTemplate  
  
# Create an in-memory representation of the secondary replica.  
$secondaryReplica = New-SqlAvailabilityReplica `  
    -Name "SecondaryComputer\Instance" `  
    -EndpointURL "TCP://SecondaryComputer.domain.com:5022" `  
    -AvailabilityMode "SynchronousCommit" `  
    -FailoverMode "Automatic" `  
    -Version 12 `  
    -AsTemplate  
  
# Create the availability group  
New-SqlAvailabilityGroup `  
    -Name "<myAvailabilityGroup>" `  
    -Path "SQLSERVER:\SQL\PrimaryComputer\Instance" `  
    -AvailabilityReplica @($primaryReplica,$secondaryReplica) `  
    -Database "<myDatabase>"  
  
# Join the secondary replica to the availability group.  
Join-SqlAvailabilityGroup -Path "SQLSERVER:\SQL\SecondaryComputer\Instance" -Name "<myAvailabilityGroup>"  
  
# Join the secondary database to the availability group.  
Add-SqlAvailabilityDatabase -Path "SQLSERVER:\SQL\SecondaryComputer\Instance\AvailabilityGroups\<myAvailabilityGroup>" -Database "<myDatabase>"  
```  
  
## <a name="related-tasks"></a><a name="RelatedTasks"></a> Tâches associées  
 **Pour configurer une instance de serveur pour les groupes de disponibilité Always On**  
  
- [Activer et désactiver les groupes de disponibilité Always On &#40;SQL Server&#41;](~/database-engine/availability-groups/windows/enable-and-disable-always-on-availability-groups-sql-server.md)  
  
- [Créer un point de terminaison de mise en miroir de bases de données pour les groupes de disponibilité Always On &#40;SQL Server PowerShell&#41;](~/database-engine/availability-groups/windows/database-mirroring-always-on-availability-groups-powershell.md)  
  
 **Pour configurer les propriétés du groupe de disponibilité et du réplica**  
  
- [Modifier le mode de disponibilité d’un réplica de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/change-the-availability-mode-of-an-availability-replica-sql-server.md)  
  
- [Modifier le mode de basculement d’un réplica de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/change-the-failover-mode-of-an-availability-replica-sql-server.md)  
  
- [Créer ou configurer un écouteur de groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md)  
  
- [Configurer la stratégie de basculement flexible pour contrôler les conditions du basculement automatique &#40;groupes de disponibilité Always On&#41;](~/database-engine/availability-groups/windows/configure-flexible-automatic-failover-policy.md)  
  
- [Spécifier l’URL de point de terminaison lors de l’ajout ou lors de la modification d’un réplica de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/specify-endpoint-url-adding-or-modifying-availability-replica.md)  
  
- [Configurer la sauvegarde sur des réplicas de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-backup-on-availability-replicas-sql-server.md)  
  
- [Configurer l’accès en lecture seule sur un réplica de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-read-only-access-on-an-availability-replica-sql-server.md)  
  
- [Configurer le routage en lecture seule pour un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-read-only-routing-for-an-availability-group-sql-server.md)  
  
- [Modifier le délai d’expiration de session pour un réplica de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/change-the-session-timeout-period-for-an-availability-replica-sql-server.md)  
  
 **Pour terminer la configuration du groupe de disponibilité**  
  
- [Joindre un réplica secondaire à un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-replica-to-an-availability-group-sql-server.md)  
  
- [Préparer manuellement une base de données secondaire pour un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/manually-prepare-a-secondary-database-for-an-availability-group-sql-server.md)  
  
- [Joindre une base de données secondaire à un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-database-to-an-availability-group-sql-server.md)  
  
- [Créer ou configurer un écouteur de groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md)  
  
 **Autres méthodes de création d'un groupe de disponibilité**  
  
- [Utiliser l’Assistant Groupe de disponibilité &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-availability-group-wizard-sql-server-management-studio.md)  
  
- [Utiliser la boîte de dialogue Nouveau groupe de disponibilité &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-new-availability-group-dialog-box-sql-server-management-studio.md)  
  
- [Créer un groupe de disponibilité &#40;Transact-SQL&#41;](../../../database-engine/availability-groups/windows/create-an-availability-group-transact-sql.md)  
  
 **Pour résoudre des problèmes de configuration des groupes de disponibilité Always On**  
  
- [Résoudre des problèmes de configuration des groupes de disponibilité Always On &#40;SQL Server&#41;](~/database-engine/availability-groups/windows/troubleshoot-always-on-availability-groups-configuration-sql-server.md)  
  
- [Résoudre une opération d’ajout de fichier ayant échoué &#40;groupes de disponibilité Always On&#41;](~/database-engine/availability-groups/windows/troubleshoot-a-failed-add-file-operation-always-on-availability-groups.md)  
  
## <a name="related-content"></a><a name="RelatedContent"></a> Contenu associé  
  
- **Blogs :**  
  
     [Always On - HADRON Learning Series: Worker Pool Usage for HADRON Enabled Databases](/archive/blogs/psssql/alwayson-hadron-learning-series-worker-pool-usage-for-hadron-enabled-databases)  
  
     [Configuration d’Always On avec SQL Server PowerShell](/archive/blogs/sqlalwayson/configuring-alwayson-with-sql-server-powershell)  
  
     [Blogs de l’équipe de SQL Server Always On : Blog officiel de l’équipe de SQL Server Always On](/archive/blogs/sqlalwayson/)  
  
     [Blogs des ingénieurs du Service clientèle et du Support technique de SQL Server](/archive/blogs/psssql/)  
  
- **Vidéos :**  
  
     [Microsoft SQL Server Code-Named "Denali" Always On Series,Part 1: Introducing the Next Generation High Availability Solution (Présentation de la solution haute disponibilité de nouvelle génération)](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2011/DBI302)  
  
     [Microsoft SQL Server Code-Named "Denali" Always On Series,Part 2: Building a Mission-Critical High Availability Solution Using Always On (Génération d’une solution haute disponibilité critique à l’aide d’Always On)](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2011/DBI404)  
  
- **Livres blancs :**  
  
     [Guide de solutions Microsoft SQL Server Always On pour la haute disponibilité et la récupération d’urgence](/previous-versions/sql/sql-server-2012/hh781257(v=msdn.10))  
  
     [Livres blancs de Microsoft pour SQL Server 2012](https://social.technet.microsoft.com/wiki/contents/articles/13146.white-paper-gallery-for-sql-server.aspx#[Category]SQLServer2012)  
  
     [Livres blancs de l'équipe de consultants clients de SQL Server](https://techcommunity.microsoft.com/t5/DataCAT/bg-p/DataCAT/)  
  
## <a name="see-also"></a>Voir aussi  
 [Point de terminaison de mise en miroir de bases de données &#40;SQL Server&#41;](../../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md)   
 [Vue d’ensemble des groupes de disponibilité Always On (SQL Server)](~/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)