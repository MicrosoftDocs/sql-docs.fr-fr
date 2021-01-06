---
title: Créer une ressource DTC en cluster pour un groupe de disponibilité
description: Cette rubrique vous présente la configuration complète d’une ressource de DTC en cluster pour un groupe de disponibilité Always On SQL Server.
ms.custom: seodec18
ms.date: 08/30/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
ms.assetid: 0e332aa4-2c48-4bc4-a404-b65735a02cea
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: c976a81277f18f89486343af32c96509f8383fda
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643268"
---
# <a name="create-clustered-dtc-resource-for-an-always-on-availability-group"></a>Créer une ressource DTC en cluster pour un groupe de disponibilité Always On

[!INCLUDE[sql windows only](../../../includes/applies-to-version/sql-windows-only.md)]

Cette rubrique vous présente la configuration complète d’une ressource de DTC en cluster pour un groupe de disponibilité Always On SQL Server. La configuration complète peut prendre une heure. 

La procédure pas à pas crée une ressource DTC en cluster et les groupes de disponibilité SQL Server afin d’établir une conformité avec les exigences de la section [DTC en cluster pour les groupes de disponibilité SQL Server](../../../database-engine/availability-groups/windows/cluster-dtc-for-sql-server-2016-availability-groups.md).

La procédure pas à pas utilise des scripts PowerShell et Transact-SQL (T-SQL).  L’activation de la plupart des scripts T-SQL nécessite le **Mode SQLCMD** .  Pour plus d’informations sur le **Mode SQLCMD**, consultez [Modifier des scripts SQLCMD à l’aide de l’Éditeur de requête](../../../ssms/scripting/edit-sqlcmd-scripts-with-query-editor.md).  Le module PowerShell **FailoverClusters** doit être importé.  Pour plus d’informations sur l’importation d’un module PowerShell, consultez la section [Importing a PowerShell Module (Importation d’un module PowerShell)](/powershell/scripting/developer/module/importing-a-powershell-module).  Cette procédure pas à pas repose sur les hypothèses suivantes :
- Toutes les conditions requises de [Conditions préalables requises, restrictions et recommandations pour les groupes de disponibilité Always On (SQL Server)](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md) ont été respectées.  
- Le domaine est `contoso.lab`.
- L’utilisateur est autorisé à créer des objets Ordinateur dans l’unité d’organisation dans laquelle sera créée la ressource de nom réseau DTC.
- L’utilisateur est un utilisateur de domaine disposant des droits d’administrateur sur tous les nœuds du cluster.
- Un partage de fichiers appelé `sqlbackups` a été créé pour les sauvegardes.
- Les instances par défaut de SQL Server sont nommées `SQLNODE1` et `SQLNODE2`.
- Le même compte de service est utilisé sur toutes les instances de SQL Server.
- L’utilisateur est membre du rôle sysadmin fixe SQL Server sur l’ensemble des instances SQL Server.
- Le résultat par défaut des transactions que DTC ne pourra pas résoudre sera défini sur `presume commit`.
- Le point de terminaison de mise en miroir utilise le port `5022`.
- Aucun autre groupe de disponibilité ou ressource DTC en cluster n’existent.
- Détails du cluster (existant) :
  - Nom : `Cluster`
  - Nom du réseau : `Cluster Network 1`
  - Nœuds : `SQLNODE1, SQLNODE2`
  - Stockage partagé : `Cluster Disk 3` (détenu par `SQLNODE1`)
- Détails du cluster (à créer) :
  - Ressource de nom réseau : `DTCnet1`
  - Ressource de nom réseau DTC : `DTC1`
  - Ressource de disque physique DTC : `DTCDisk1`
  - IP DTC et ressource de sous-réseau : `192.168.2.54`, `255.255.255.0`
  - Ressource IP DTC : `DTCIP1`

## <a name="1-check-operating-system"></a>1. Vérification du système d’exploitation
Pour les transactions distribuées prises en charge, [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] doit être exécuté sur Windows Server 2016 ou Windows Server 2012 R2.  Pour Windows Server 2012 R2, vous devez installer la mise à jour KB3090973 disponible à l’adresse [https://support.microsoft.com/kb/3090973](https://support.microsoft.com/kb/3090973).  Ce script contrôle la version du système d’exploitation et détermine s’il est nécessaire d’installer le correctif logiciel 3090973.  Exécutez le script PowerShell suivant sur `SQLNODE1`.

```powershell  
# A few OS checks

\<#
Script: 
    1) is re-runnable 
    2) will work on any node in the cluster, and 
    3) will execute against every node in the cluster
#>

$nodes = (Get-ClusterNode).Name;
foreach ($node in $nodes) {

    # At least 2012 R2
    $os = (Get-WmiObject -class Win32_OperatingSystem -ComputerName $node).caption;
    IF($os -like "*2012 R2*" -or $os -like "*2016*")
    {
        Write-Host "$os is supported on $node.";
    }
    ELSE
    {
        Write-Host "STOP!  $os is not supported on $node.";
    }
    
    # KB 3090973
    IF($os -like "*2012 R2*")
    {
        $kb = Get-Hotfix -ComputerName $node | Where {$_.HotFixID -eq 'KB3090973'};
        IF($kb)
        {
            Write-Host "KB3090973 is installed on $node."
        }
        ELSE
        {
            Write-Host "HotFixID KB3090973 must be applied on $node.  See https://support.microsoft.com/kb/3090973 for additional information and to download the hotfix.";
        }
    }
    ELSE
    {
        Write-Host "KB3090973 is not applicable to $os on $node."
    }
}
```  
## <a name="2---configure-firewall-rules"></a>2.   Configurer les règles de pare-feu
Ce script configurera le pare-feu pour la prise en charge du trafic DTC, sur chaque instance SQL Server hébergeant une instance de groupe de disponibilité, et sur chaque serveur engagé dans la transaction distribuée.  Le script configure également le pare-feu afin d’autoriser les connexions pour le point de terminaison de mise en miroir de bases de données.  Exécutez le script PowerShell suivant sur `SQLNODE1`.

```powershell  
# Configure Firewall

\<#
Script: 
    1) is re-runnable 
    2) will work on any node in the cluster, and 
    3) will execute against every node in the cluster
#>

$nodes = (Get-ClusterNode).Name;
foreach ($node in $nodes) {
    Get-NetFirewallRule -CimSession $node -DisplayGroup 'Distributed Transaction Coordinator' -Enabled False -ErrorAction SilentlyContinue | Enable-NetFirewallRule;
    New-NetFirewallRule -CimSession $node -DisplayName 'SQL Server Mirroring' -Description 'Port 5022 for SQL Server Mirroring' -Action Allow -Direction Inbound -Protocol TCP -LocalPort 5022 -RemotePort Any -LocalAddress Any -RemoteAddress Any;
    };
```  
## <a name="3--configure-in-doubt-xact-resolution"></a>3.  Configurer **in-doubt xact resolution** 
Ce script configure l’option de configuration de serveur **in-doubt xact resolution** sur « presume commit » (validation présumée) pour les transactions incertaines.  Exécutez le script T-SQL suivant dans SQL Server Management Studio (SSMS), dans `SQLNODE1`, en **mode SQLCMD**.

```sql  
/*******************************************************************
    Execute script in its entirety on SQLNODE1 in SQLCMD mode
*******************************************************************/

-- Configure in-doubt xact resolution on all SQL Server instances to presume commit
IF (SELECT CAST(value_in_use as bit) FROM sys.configurations WITH (NOLOCK) WHERE [name] = N'show advanced options') = 0
BEGIN   
    EXEC sp_configure 'show advanced options', 1;
    RECONFIGURE;
END

-- Configure the server to presume commit for in-doubt transactions.
IF (SELECT CAST(value as bit) FROM sys.configurations WITH (NOLOCK) WHERE [name] = N'in-doubt xact resolution') <> 1
BEGIN   
    EXEC sp_configure 'in-doubt xact resolution', 1;
    RECONFIGURE;
END
GO
-----------------------------------------------------------------------------

:connect SQLNODE2
IF (SELECT CAST(value_in_use as bit) FROM sys.configurations WITH (NOLOCK) WHERE [name] = N'show advanced options') = 0
BEGIN   
    EXEC sp_configure 'show advanced options', 1;
    RECONFIGURE;
END

-- Configure the server to presume commit for in-doubt transactions.
IF (SELECT CAST(value as bit) FROM sys.configurations WITH (NOLOCK) WHERE [name] = N'in-doubt xact resolution') <> 1
BEGIN   
    EXEC sp_configure 'in-doubt xact resolution', 1;
    RECONFIGURE;
END
GO
-----------------------------------------------------------------------------
```

## <a name="4-create-test-databases"></a>4. Créer des bases de données de test
Le script créera une base de données appelée `AG1` sur `SQLNODE1` et une base de données appelée `dtcDemoAG1` sur `SQLNODE2`.  Exécutez le script T-SQL suivant dans SSMS, dans `SQLNODE1` en **mode SQLCMD**.

```sql  
/*******************************************************************
    Execute script in its entirety on SQLNODE1 in SQLCMD mode
*******************************************************************/

-- On SQLNODE1 
USE master;
SET NOCOUNT ON;

IF  EXISTS (SELECT * FROM sys.databases WHERE name = N'AG1')
BEGIN
    DROP DATABASE AG1;
END
GO

CREATE DATABASE AG1;
ALTER DATABASE AG1 SET RECOVERY FULL WITH NO_WAIT;
ALTER AUTHORIZATION ON DATABASE::AG1 to sa;
GO

USE AG1;
CREATE TABLE [dbo].[Names] (
        [Name] [varchar](64) NULL,
        [EditDate] datetime
        );

INSERT Names
VALUES ('AG1', GETDATE());
GO


-- Against SQNODE2
:connect SQLNODE2
USE master;
SET NOCOUNT ON;

IF  EXISTS (SELECT * FROM sys.databases WHERE name = N'dtcDemoAG1')
BEGIN
    DROP DATABASE dtcDemoAG1;
END
GO

CREATE DATABASE dtcDemoAG1;
ALTER DATABASE dtcDemoAG1 SET RECOVERY SIMPLE WITH NO_WAIT;
ALTER AUTHORIZATION ON DATABASE::dtcDemoAG1 to sa;
GO

USE dtcDemoAG1;
CREATE TABLE [dbo].[Names] (
        [Name] [varchar](64) NULL,
        [EditDate] datetime
        );
GO    
----------------------------------------------------------------
```
## <a name="5---create-endpoints"></a>5.   Créer les points de terminaison
Ce script crée un point de terminaison nommé `AG1_endpoint` qui écoute sur le port TCP `5022`.  Exécutez le script T-SQL suivant dans SSMS, dans `SQLNODE1` en **mode SQLCMD**.

```sql  
/**********************************************
Execute on SQLNODE1 in SQLCMD mode
**********************************************/

-- Create endpoint on server instance that hosts the primary replica:
IF NOT EXISTS (SELECT * FROM sys.database_mirroring_endpoints)
BEGIN
    CREATE ENDPOINT AG1_endpoint
    AUTHORIZATION [sa]
        STATE=STARTED 
        AS TCP (LISTENER_PORT=5022) 
        FOR DATABASE_MIRRORING (ROLE=ALL);
END
GO
-----------------------------------------------------------------------------

:connect SQLNODE2
IF NOT EXISTS (SELECT * FROM sys.database_mirroring_endpoints)
BEGIN
    CREATE ENDPOINT AG1_endpoint
    AUTHORIZATION [sa]
        STATE=STARTED 
        AS TCP (LISTENER_PORT=5022) 
        FOR DATABASE_MIRRORING (ROLE=ALL);
END
GO
-----------------------------------------------------------------------------
```

## <a name="6---prepare-databases-for-availability-group"></a>6.   Préparer les bases de données pour le groupe de disponibilité
Le script sauvegarde `AG1` sur `SQLNODE1` et le restaure sur `SQLNODE2`.  Exécutez le script T-SQL suivant dans SSMS, dans `SQLNODE1` en **mode SQLCMD**.

```sql  
/*******************************************************************
    Execute script in its entirety on SQLNODE1 in SQLCMD mode
*******************************************************************/

-- Backup database
BACKUP DATABASE AG1 
TO DISK = N'\\sqlnode1\sqlbackups\AG1.bak' 
WITH FORMAT, STATS = 10;

-- Backup transaction log
BACKUP LOG AG1
TO DISK = N'\\sqlnode1\sqlbackups\AG1_Log.bak'
WITH FORMAT, STATS = 10;
GO


-- Restore database and logs on secondary WITH NORECOVERY
:connect SQLNODE2
USE [master]
RESTORE DATABASE AG1 
FROM DISK = N'\\sqlnode1\sqlbackups\AG1.bak' 
WITH NORECOVERY, STATS = 10;

RESTORE LOG AG1 
FROM DISK = N'\\sqlnode1\sqlbackups\AG1_Log.bak'
WITH NORECOVERY, STATS = 10;
GO
```

## <a name="7---create-availability-group"></a>7.   Créer un groupe de disponibilité
[!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] doit être créé avec la commande **CREATE AVAILABILITY GROUP** et la clause **WITH DTC_SUPPORT = PER_DB**.  Actuellement, vous ne pouvez pas modifier un groupe de disponibilité.  L’assistant Nouveau groupe de disponibilité ne vous autorise pas à activer la prise en charge de DTC pour un nouveau groupe de disponibilité.  Le script suivant crée le nouveau groupe de disponibilité et joint le réplica secondaire.  Exécutez le script T-SQL suivant dans SSMS, dans `SQLNODE1` en **mode SQLCMD**.

```sql  
/*******************************************************************
    Execute script in its entirety on SQLNODE1 in SQLCMD mode
*******************************************************************/

--  Create Availability Group on SQLNODE1 
USE master;
CREATE AVAILABILITY GROUP DTCAG1
WITH (DTC_SUPPORT = PER_DB) 
FOR DATABASE AG1
REPLICA ON 
  'SQLNODE1' WITH
     (
     ENDPOINT_URL = 'TCP://SQLNODE1.contoso.lab:5022', 
     AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,
     FAILOVER_MODE = MANUAL
     ),
  'SQLNODE2' WITH 
     (
     ENDPOINT_URL = 'TCP://SQLNODE2.contoso.lab:5022',
     AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,
     FAILOVER_MODE = MANUAL
     ); 
GO


-- SQLNODE2
-- Join secondary replica to the Availability Group 
:connect SQLNODE2
ALTER AVAILABILITY GROUP DTCag1 JOIN;

-- Join database to the Availability Group
ALTER DATABASE AG1 SET HADR AVAILABILITY GROUP = DTCAG1;
GO
```

> [!IMPORTANT]
> Vous ne pouvez pas activer DTC sur une instance [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] existante.  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] accepte la syntaxe suivante pour un groupe de disponibilité existant :  
> 
> USE master;    
> ALTER AVAILABILITY GROUP \<availability_group\>  
> SET (DTC_Support = Per_DB)  
> 
> Toutefois, aucune modification de configuration n’est apportée.  Vous pouvez confirmer la configuration **dtc_support** avec la requête T-SQL suivante :  
> 
> SELECT name, dtc_support FROM sys.availability_groups  
> 
> Le seul moyen d’activer la prise en charge de DTC sur un groupe de disponibilité est de le créer à l’aide de Transact-SQL.
 
## <a name="8--prepare-cluster-resources"></a><a name="ClusterDTC"></a>8.  Préparer des ressources de cluster

Ce script prépare les ressources dépendantes de DTC : Disk et IP.  Le stockage partagé est ajouté au cluster Windows.  Les ressources réseau sont créées, puis le DTC est généré et configuré en tant que ressource sur le groupe de disponibilité.  Exécutez le script PowerShell suivant sur `SQLNODE1`. Nous remercions [Allan Hirt](https://sqlha.com/2013/03/12/how-to-properly-configure-dtc-for-clustered-instances-of-sql-server-with-windows-server-2008-r2/) pour le script !

```powershell  
# Create a clustered Microsoft Distributed Transaction Coordinator properly in the resource group with SQL Server

\<#----------------------------------- Begin User Input -----------------------------------#>
$AGgrp = "DTCag1";                          # Name of the WSFC resource group that will contain the DTC resource

$WSFC = (Get-Cluster).Name;                 # Windows Failover Cluster name
$DTCnetwk = "Cluster Network 1"             # WSFC Network to use for the DTC IP address

$ClusterAvailableDisk = "Cluster Disk 3";   # Designated disk that can support failover clustering and is visible to all nodes, but not yet part of the set of clustered disks
$DTCdisk = "DTCDisk1";                      # Name of the disk to be used with DTC

$DTCipresnm = "DTCIP1";                     # WSFC Friendly Name of the DTC's IP resource 
$DTCipaddr = "192.168.2.54";                # IP address of the DTC resource 
$DTCsubnet = "255.255.255.0";               # Subnet for the DTC IP address 
$DTCnetnm = "DTCNet1";                      # WSFC Friendly Name of the Network Name resource
$DTCresnm = "DTC1";                         # Name of the WSFC DTC Network Name resource; Name must be unique in AD
\<#------------------------------------ End User Input ------------------------------------#>


# Make a new disk available for use in a failover cluster.
Get-ClusterAvailableDisk | Where {$_.Name -eq $ClusterAvailableDisk} | Add-ClusterDisk;

# Rename disk
$resource = Get-ClusterResource $ClusterAvailableDisk; $resource.Name = $DTCdisk;

# Create the IP resource
Add-ClusterResource -Name $DTCipresnm -ResourceType "IP Address" -Group $AGgrp;

# Set the network to use, IP address, and subnet
# All three have to be configured at the same time
$DTCIPres = Get-ClusterResource $DTCipresnm;
$ntwk = New-Object Microsoft.FailoverClusters.PowerShell.ClusterParameter $DTCipres,Network,$DTCnetwk;
$ipaddr = New-Object Microsoft.FailoverClusters.PowerShell.ClusterParameter $DTCipres,Address,$DTCipaddr;
$subnet = New-Object Microsoft.FailoverClusters.PowerShell.ClusterParameter $DTCipres,SubnetMask,$dtcsubnet;

$setdtcipparams = $ntwk,$ipaddr,$subnet;
$setdtcipparams | Set-ClusterParameter;

# Create the Network Name resource
Add-ClusterResource $DTCnetnm -ResourceType "Network Name" -Group $AGgrp;

# Set the value for the Network Name resource
Get-ClusterResource $DTCnetnm | Set-ClusterParameter DnsName $DTCresnm;

# Add the IP address as a depenency of the Network Name resource
Add-ClusterResourceDependency $DTCnetnm $DTCipresnm;

# Create the Distributed Transaction Coordinator resource
Add-ClusterResource $DTCresnm -ResourceType "Distributed Transaction Coordinator" -Group $AGgrp;

# Add the Network Name as a depenency of the DTC resource
Add-ClusterResourceDependency $DTCresnm $DTCnetnm;

# Move the disk into the resource group with SQL Server
Move-ClusterResource -Name $DTCdisk -Group $AGgrp;

# Add the disk as a depenency of the DTC resource
Add-ClusterResourceDependency $DTCresnm $DTCdisk;

# Bring the IP resource online
Start-ClusterResource $DTCipresnm;

# Bring the Network Name resource online
Start-ClusterResource $DTCnetnm;

# Bring the DTC resource online
Start-ClusterResource $DTCresnm;
```  

## <a name="9---enable-network-dtc"></a>9.   Activer le DTC réseau 

Le script suivant active l’accès DTC réseau pour le service DTC mis en cluster afin de permettre l’engagement des ordinateurs distants dans les transactions distribuées sur le réseau.  Exécutez le script PowerShell suivant sur `SQLNODE1`.

```powershell  
# Enable Network DTC access for the clustered DTC service

\<#
Script: 
    1) is re-runnable 
    2) will work on any node in the cluster, and 
    3) will execute against every node in the cluster
#>

# Enter Name of DTC resource

$DtcName = "DTC1";
\<# ------- End of User Input ------- #>

[bool]$restart = 0;
$node = (Get-ClusterResource -Name $DtcName).OwnerNode.Name;
$DtcSettings = Get-DtcNetworkSetting -DtcName $DtcName;

IF ($DtcSettings.InboundTransactionsEnabled -eq $false)   
{
    Set-DtcNetworkSetting -CimSession $node -DtcName $DtcName -AuthenticationLevel "Mutual" -InboundTransactionsEnabled $true -Confirm:$false;
    $restart = 1;
}

IF ($DtcSettings.OutboundTransactionsEnabled -eq $false)   
{
    Set-DtcNetworkSetting -CimSession $node -DtcName $DtcName -AuthenticationLevel "Mutual" -OutboundTransactionsEnabled $true -Confirm:$false;
    $restart = 1;
}

IF ($restart -eq 1)
{
    Stop-Dtc -CimSession $node -DtcName $DTCname -Confirm:$false;
    Start-Dtc -CimSession $node -DtcName $DTCname;
}
```  

## <a name="10--disable-and-stop-the-local-dtc-service-on-each-node"></a>10.  Désactiver et arrêter le service DTC local sur chaque nœud

Afin de garantir que les transactions distribuées utilisent la ressource DTC en cluster, désactivez le service DTC local sur les deux nœuds.  Le script suivant désactive et arrête le service DTC local sur chaque nœud.  Exécutez le script PowerShell suivant sur `SQLNODE1`.
```powershell  
# Disable local DTC service

\<#
Script: 
    1) is re-runnable 
    2) will work on any node in the cluster, and 
    3) will execute against every node in the cluster
#>

$DTCname = 'Local';
$nodes = (Get-ClusterNode).Name;

 foreach ($node in $nodes) {

    $service = Get-WmiObject -class Win32_Service -computername $node -Filter "Name='MSDTC'";
    IF ($service.StartMode -ne 'Disabled')
    {
        $service.ChangeStartMode('Disabled');
    }
    
    IF ($service.State -ne 'Stopped')
    {
        $service.StopService();
    }
}
```  

## <a name="11--cycle-the-ssnoversion-service-for-each-instance"></a>11.  Parcourir le service [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] pour chaque instance

Une fois le service DTC en cluster complètement configuré, vous devez arrêter et redémarrer chaque instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] dans le groupe de disponibilité afin de vous assurer que [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] est inscrit pour utiliser ce service DTC.

La première fois que le service [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] nécessite une transaction distribuée, il s’inscrit auprès d’un service DTC. Le service SQL Server continue d’utiliser ce service DTC jusqu’à ce qu’il soit redémarré. Si un service DTC en cluster est disponible, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] s’inscrit auprès du service DTC en cluster. Si un service DTC en cluster est disponible, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] s’inscrit auprès du service DTC en local. Afin de vérifier que [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] s’inscrit auprès du service DTC en cluster, arrêtez et redémarrez chaque instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. 

Suivez les étapes contenues dans le script T-SQL ci-dessous :
```sql  
/*
Gracefully cycle the SQL Server service and failover the Availability Group
    a.  On SQLNODE2, cycle the SQL Server service from SQL Server Configuration Manger

    b.  On SQLNODE2 failover the Availability Group to SQLNODE2
        Execute T-SQL script, below, on SQLNODE2 (Use Results to Text)

    c.  On SQLNODE1, cycle the SQL Server service from SQL Server Configuration Manger

    d.  On SQLNODE1 failover the Availability Group to SQLNODE1 once the databases are back in sync.
        Execute T-SQL script, below, on SQLNODE1 (Use Results to Text)
*/

SET NOCOUNT ON;

-- Ensure replica is secondary
IF (
SELECT rs.is_primary_replica 
    FROM sys.availability_groups ag
    JOIN sys.dm_hadr_database_replica_states rs
    ON  ag.group_id = rs.group_id
    WHERE ag.name = N'DTCag1'
    AND rs.is_local = 1) = 0
BEGIN
    -- Wait for SYNCHRONIZED state
    DECLARE @ctr tinyint = 0;
    declare @msg varchar(128);
    WHILE (SELECT synchronization_state 
        FROM sys.availability_groups ag
        JOIN sys.dm_hadr_database_replica_states rs
        ON  ag.group_id = rs.group_id
        WHERE ag.name = N'DTCag1'
        AND rs.is_primary_replica = 0
        AND rs.is_local = 1) <> 2
    BEGIN
        WAITFOR DELAY '00:00:01'
        SET @ctr += 1
        SET @msg = 'Waiting for databases to become synchronized. Duration in seconds: ' + cast(@ctr AS varchar(3))
        RAISERROR (@msg, 0, 1) WITH NOWAIT
    END

    ALTER AVAILABILITY GROUP DTCAG1 FAILOVER;
    SELECT 'Failover complete' AS [Sucess]
END
ELSE BEGIN
    SELECT 'This instance is the primary replica.  Connect to the secondary replica and try again.' AS [Error]
END

```

## <a name="12--test-configuration"></a>12.  Configuration test

Ce test utilise un serveur lié de `SQLNODE1` à `SQLNODE2` pour créer une transaction distribuée.  Assurez-vous que le réplica principal du groupe de disponibilité se trouve sur `SQLNODE1`. Pour tester la configuration, il vous faut :

- Créer des serveurs liés
- Exécuter une transaction distribuée

### <a name="create-linked-servers"></a>Créer des serveurs liés  
Le script suivant crée deux serveurs liés sur `SQLNODE1`.  Exécutez le script T-SQL suivant dans SSMS, dans `SQLNODE1`.

```sql  
-- SQLNODE1
IF NOT EXISTS (SELECT * FROM sys.servers where name = N'SQLNODE1')
BEGIN
    EXEC master.dbo.sp_addlinkedserver @server = N'SQLNODE1';   
END

IF NOT EXISTS (SELECT * FROM sys.servers where name = N'SQLNODE2')
BEGIN
    EXEC master.dbo.sp_addlinkedserver @server = N'SQLNODE2';   
END
 ```

### <a name="execute-a-distributed-transaction"></a>Exécuter une transaction distribuée
Ce script renvoie dans un premier temps les statistiques en cours de la transaction DTC.  Ensuite, le script exécute une transaction distribuée utilisant les bases de données sur `SQLNODE1` et `SQLNODE2`.  Ultérieurement, le script renvoie de nouveau les statistiques de la transaction DTC, qui doivent désormais correspondre à une quantité accrue.  Connectez-vous physiquement à `SQLNODE1` , puis exécutez le script T-SQL suivant dans SSSMS, dans `SQLNODE1` en **mode SQLCMD**.

```sql  
/*******************************************************************
    Execute script in its entirety on SQLNODE1 in SQLCMD mode
    Must be physically connected to SQLNODE1
*******************************************************************/

USE AG1;
SET NOCOUNT ON;

-- Get Baseline
!! Powershell; $DtcNameC = Get-DtcClusterDefault; Get-DtcTransactionsStatistics -DtcName $DtcNameC;

SET XACT_ABORT ON
BEGIN DISTRIBUTED TRANSACTION
    INSERT INTO SQLNODE1.[AG1].[dbo].[Names] VALUES ('TestValue1', GETDATE());
    INSERT INTO SQLNODE2.[dtcDemoAG1].[dbo].[Names] VALUES ('TestValue2', GETDATE());
COMMIT TRAN
GO

-- Review DTC Transaction Statistics
!! Powershell; $DtcNameC = Get-DtcClusterDefault; Get-DtcTransactionsStatistics -DtcName $DtcNameC;
```

> [!IMPORTANT]
> L’instruction `USE AG1` doit être exécutée afin de garantir que le contexte de base de données est défini sur `AG1`.  Sinon, vous recevez le message d’erreur suivant : « Le contexte de transaction est utilisé par une autre session. »