---
title: Activer ou désactiver un protocole réseau de serveur | Microsoft Docs
description: Découvrez comment activer ou désactiver un protocole réseau de serveur. Découvrez comment utiliser le Gestionnaire de configuration SQL Server ou PowerShell pour cette tâche.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- network protocols [SQL Server], disabling
- remote connections [SQL Server], enabling using Configuration Manager
- protocols [SQL Server], enabling using Configuration Manager
- protocols [SQL Server], disabling using Configuration Manager
- disabling network protocols, Configuration Manager
- network protocols [SQL Server], enabling
- enabling network protocols, Configuration Manager
- surface area configuration [SQL Server], connection protocols
- connections [SQL Server], enabling remote using Configuration Manager
ms.assetid: ec5ccb69-61c9-4576-8843-014b976fd46e
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 3b5f15490744ccfe8bfd47a69892f7033ddae361
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171001"
---
# <a name="enable-or-disable-a-server-network-protocol"></a>Activer ou désactiver un protocole réseau de serveur
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Tous les protocoles réseau sont installés par le programme d'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , mais ils peuvent être activés ou non. Cette rubrique décrit comment activer ou désactiver un protocole réseau de serveur dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] à l'aide du gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou de PowerShell. Le [!INCLUDE[ssDE](../../includes/ssde-md.md)] doit être arrêté et redémarré pour que la modification soit prise en compte.  
  
> [!IMPORTANT]  
>  Lors de l'installation de [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] une connexion est ajoutée pour le groupe BUILTIN\Users. Cela permet à tous les utilisateurs authentifiés sur l'ordinateur d'accéder à l'instance de [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] en tant que membres du rôle public. La connexion BUILTIN\Users peut être supprimée sans risque pour restreindre l'accès au [!INCLUDE[ssDE](../../includes/ssde-md.md)] aux utilisateurs de l'ordinateur qui disposent de connexions ou qui sont membres d'autres groupes Windows avec des connexions.  
  
> [!WARNING]  
>  Les fournisseurs de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et [!INCLUDE[msCoName](../../includes/msconame-md.md)] uniquement pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à [!INCLUDE[sssql14](../../includes/sssql14-md.md)] prennent en charge TLS 1.0 et SSL 3.0 par défaut. Si vous appliquez un autre protocole (comme TLS 1.1 ou TLS 1.2) en apportant des modifications dans la couche SChannel du système d’exploitation, vos connexions à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] risquent d’échouer, sauf si vous avez installé la mise à jour appropriée permettant de prendre en charge TLS 1.1 et 1.2 sur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], qui se trouve <a href="https://support.microsoft.com/help/3135244/tls-1-2-support-for-microsoft-sql-server">ici</a>. À partir de [!INCLUDE[sssql15](../../includes/sssql16-md.md)], toutes les versions de SQL Server incluent la prise en charge de TLS 1.2 sans aucune mise à jour supplémentaire nécessaire.
  
 **Dans cette rubrique**  
  
-   **Pour activer ou désactiver un protocole réseau de serveur à l'aide de :**  
  
     [Gestionnaire de configuration SQL Server](#SSMSProcedure)  
  
     [PowerShell](#PowerShellProcedure)  
  
##  <a name="using-sql-server-configuration-manager"></a><a name="SSMSProcedure"></a> Utilisation du Gestionnaire de configuration SQL Server  
  
#### <a name="to-enable-a-server-network-protocol"></a>Pour activer un protocole réseau de serveur  
  
1.  Dans le Gestionnaire de configuration du [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , développez **Configuration du réseau SQL Server** dans le volet de la console.  
  
2.  Dans le volet de console, cliquez sur **Protocoles pour** _\<instance name>_ .  
  
3.  Dans le volet d’informations, cliquez avec le bouton droit sur le protocole à modifier, puis cliquez sur **Activer** ou **Désactiver**.  
  
4.  Dans le volet de la console, cliquez sur **Services SQL Server**.  
  
5.  Dans le volet d'informations, cliquez avec le bouton droit sur **SQL Server (** _\<instance name>_ **)** , puis cliquez sur **Redémarrer** pour arrêter et redémarrer le service de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
##  <a name="using-sql-server-powershell"></a><a name="PowerShellProcedure"></a> Utilisation de PowerShell SQL Server  
  
#### <a name="to-enable-a-server-network-protocol-using-powershell"></a>Pour activer un protocole réseau de serveur à l'aide de PowerShell  
  
1.  En utilisant des autorisations d'administrateur, ouvrez une invite de commandes.  
  
2.  Démarrez Windows PowerShell à partir de la barre des tâches, ou cliquez successivement sur Démarrer, Tous les Programmes, Accessoires, Windows PowerShell, puis Windows PowerShell.  
  
3.  Importez le module **sqlps** en entrant **Import-Module "sqlps"**  
  
4.  Exécutez les instructions suivantes pour activer les protocoles TCP et de canaux nommés. Remplacez `<computer_name>` par le nom de l'ordinateur qui exécute [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si vous configurez une instance nommée, remplacez `MSSQLSERVER` par le nom de cette instance.  
  
     Pour désactiver des protocoles, affectez aux propriétés `IsEnabled` la valeur `$false`.  
  
    ```  
    $smo = 'Microsoft.SqlServer.Management.Smo.'  
    $wmi = new-object ($smo + 'Wmi.ManagedComputer').  
  
    # List the object properties, including the instance names.  
    $Wmi  
  
    # Enable the TCP protocol on the default instance.  
    $uri = "ManagedComputer[@Name='<computer_name>']/ ServerInstance[@Name='MSSQLSERVER']/ServerProtocol[@Name='Tcp']"  
    $Tcp = $wmi.GetSmoObject($uri)  
    $Tcp.IsEnabled = $true  
    $Tcp.Alter()  
    $Tcp  
  
    # Enable the named pipes protocol for the default instance.  
    $uri = "ManagedComputer[@Name='<computer_name>']/ ServerInstance[@Name='MSSQLSERVER']/ServerProtocol[@Name='Np']"  
    $Np = $wmi.GetSmoObject($uri)  
    $Np.IsEnabled = $true  
    $Np.Alter()  
    $Np  
    ```  
  
#### <a name="to-configure-the-protocols-for-the-local-computer"></a>Pour configurer les protocoles pour l'ordinateur local  
  
-   Lorsque le script est exécuté localement et configure l'ordinateur local, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] PowerShell peut rendre le script plus souple en déterminant de façon dynamique le nom de l'ordinateur local. Pour récupérer le nom de l'ordinateur local, remplacez la ligne qui définit la variable `$uri` par la ligne suivante.  
  
    ```  
    $uri = "ManagedComputer[@Name='" + (get-item env:\computername).Value + "']/ServerInstance[@Name='MSSQLSERVER']/ServerProtocol[@Name='Tcp']"  
    ```  
  
#### <a name="to-restart-the-database-engine-by-using-sql-server-powershell"></a>Pour redémarrer le Moteur de base de données à l'aide de SQL Server PowerShell  
  
-   Après avoir activé ou désactivé des protocoles, vous devez arrêter et redémarrer le [!INCLUDE[ssDE](../../includes/ssde-md.md)] pour que la modification entre en vigueur. Exécutez les instructions suivantes pour arrêter et démarrer l'instance par défaut à l'aide de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] PowerShell. Pour arrêter et démarrer une instance nommée, remplacez `'MSSQLSERVER'` par `'MSSQL$<instance_name>'`.  
  
    ```  
    # Get a reference to the ManagedComputer class.  
    CD SQLSERVER:\SQL\<computer_name>  
    $Wmi = (get-item .).ManagedComputer  
    # Get a reference to the default instance of the Database Engine.  
    $DfltInstance = $Wmi.Services['MSSQLSERVER']  
    # Display the state of the service.  
    $DfltInstance  
    # Stop the service.  
    $DfltInstance.Stop();  
    # Wait until the service has time to stop.  
    # Refresh the cache.  
    $DfltInstance.Refresh();   
    # Display the state of the service.  
    $DfltInstance  
    # Start the service again.  
    $DfltInstance.Start();  
    # Wait until the service has time to start.  
    # Refresh the cache and display the state of the service.  
    $DfltInstance.Refresh(); $DfltInstance  
    ```  
  
  
