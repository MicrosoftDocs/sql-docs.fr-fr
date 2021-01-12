---
title: Démarrer, arrêter, suspendre, reprendre, redémarrer les services SQL Server
description: Découvrez comment démarrer, arrêter, suspendre, reprendre, redémarrer les services SQL Server. Découvrez comment utiliser Transact-SQL, PowerShell et d’autres outils pour ces actions.
ms.custom: ''
ms.date: 03/05/2020
ms.prod: sql
ms.prod_service: high-availability
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Configuration Manager, start and stop services
- stopping SQL Server Agent
- parameters [SQL Server], startup options
- SQL Server, startup options
- Database Engine [SQL Server], starting and stopping services
- pausing SQL Server
- PowerShell [SQL Server], starting and stopping services
- single-user mode [SQL Server], starting in
- SQL Server Management Studio [SQL Server], starting or stopping services
- stopping SQL Server Browser service
- starting SQL Server Agent
- default instances [SQL Server], starting and stopping
- SQL Server Agent, starting and stopping
- command prompt [SQL Server], starting and stopping SQL Server services
- continuing SQL Server
- starting SQL Server Database Engine
- net stop commands [SQL Server]
- command prompt [SQL Server], SQL Browser service
- Configuration Manager, start and stop services
- resuming SQL Server
- startup options [SQL Server]
- named instances [SQL Server], starting and stopping
- net start commands [SQL Server]
- SQL Server, starting and stopping
- stopping SQL Server
- starting SQL Server Browser service
- SQL Server Database Engine setting startup options
- administering SQL Server, starting and stopping services
- Management Studio [SQL Server], starting or stopping services
ms.assetid: 32660a02-e5a1-411a-9e57-7066ca459df6
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.openlocfilehash: 697cf043c4582796015b061c857145c214d105fb
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091752"
---
# <a name="start-stop-pause-resume-restart-sql-server-services"></a>Démarrer, arrêter, suspendre, reprendre, redémarrer les services SQL Server

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Cette rubrique explique comment démarrer, arrêter, suspendre, reprendre ou redémarrer le moteur de base de données SQL Server, SQL Server Agent ou le service SQL Server Browser à l’aide du Gestionnaire de configuration SQL Server, de SQL Server Management Studio (SSMS), des commandes net d’une invite de commandes, de Transact-SQL ou de PowerShell.

## <a name="identify-the-service"></a>Identifier le service

Les composants SQL Server sont des programmes exécutables qui s’exécutent en tant que service Windows. Les programmes qui s'exécutent en tant que service Windows peuvent continuer à fonctionner sans afficher d'activité sur l'écran de l'ordinateur.

### <a name="database-engine-service"></a>Service Moteur de base de données

Processus exécutable qui correspond au moteur de base de données SQL Server. Le moteur de base de données peut être l’instance par défaut (avec une limite d’une par ordinateur) ou l’une des nombreuses instances nommées du moteur de base de données. Utilisez le Gestionnaire de configuration SQL Server pour identifier les instances du moteur de base de données qui sont installées sur l’ordinateur. L’instance par défaut (si vous l’installez) est listée comme **SQL Server (MSSQLSERVER)** . Les instances nommées (si vous les installez) sont listées comme **SQL Server (<nom_instance>)** . Par défaut, SQL Server Express est installé comme **SQL Server (SQLEXPRESS)** .

### <a name="sql-server-agent-service"></a>service SQL Server Agent

Service Windows qui exécute des tâches administratives planifiées, appelées travaux et alertes. Pour plus d’informations, consultez [SQL Server Agent](../../ssms/agent/sql-server-agent.md). SQL Server Agent n’est pas disponible dans toutes les éditions de SQL Server. Pour obtenir la liste des fonctionnalités prises en charge par les éditions de SQL Server, consultez [Fonctionnalités prises en charge par les éditions de SQL Server 2019](../../sql-server/editions-and-components-of-sql-server-version-15.md).

### <a name="sql-server-browser-service"></a>SQL Server Browser Service

Service Windows qui écoute les demandes entrantes de ressources SQL Server et fournit aux clients des informations sur les instances SQL Server installées sur l’ordinateur. Une seule instance du service SQL Server Browser est utilisée pour toutes les instances de SQL Server installées sur l’ordinateur.

### <a name="additional-information"></a>Informations supplémentaires

- La suspension du service Moteur de base de données empêche les nouveaux utilisateurs de se connecter au moteur de base de données, mais les utilisateurs qui sont déjà connectés peuvent continuer à travailler jusqu’à ce que leurs connexions soient interrompues. Utilisez suspendre lorsque vous souhaitez attendre que les utilisateurs aient terminé leur travail avant d'arrêter le service. Cela leur permet d'effectuer les transactions en cours. *Reprendre* permet au moteur de base de données d’accepter à nouveau de nouvelles connexions. Le service SQL Server Agent ne peut pas être suspendu ni repris.  

- Le Gestionnaire de configuration SQL Server et SSMS indiquent l’état actuel des services avec les icônes suivantes.  

    **Gestionnaire de configuration SQL Server**

  - Une flèche verte sur l'icône située à côté du nom du service indique que le service a démarré.

  - Un carré rouge sur l'icône située à côté du nom du service indique que le service s'est arrêté.

  - Deux lignes bleues verticales sur l’icône située à côté du nom du service indiquent que le service est suspendu.

  - Lors du redémarrage du moteur de base de données, un carré rouge indique que le service s’est arrêté, puis une flèche verte signale que le service a démarré.

    **SQL Server Management Studio (SSMS)**

  - Une flèche blanche sur l'icône de cercle vert située à côté du nom du service indique que le service a démarré.  

  - Un carré blanc sur l'icône de cercle rouge située à côté du nom du service indique que le service s'est arrêté.  

  - Deux lignes blanches verticales sur une icône de cercle bleue située à côté du nom du service indiquent que le service est suspendu.  

- Lorsque vous utilisez le Gestionnaire de configuration SQL Server ou SSMS, seules les options possibles sont disponibles. Par exemple, si le service est déjà démarré, **Démarrer** n’est pas disponible.

- Lors de l’exécution sur un cluster, le service Moteur de base de données SQL Server est mieux géré à l’aide de l’Administrateur de cluster.  

### <a name="security"></a>Sécurité

#### <a name="permissions"></a>Autorisations

Par défaut, seuls les membres du groupe Administrateurs local peuvent démarrer, arrêter, suspendre, reprendre ou redémarrer un service. Pour accorder aux non-administrateurs la capacité de gérer des services, consultez [Comment accorder aux utilisateurs des droits de gestion des services dans Windows Server 2003](https://support.microsoft.com/kb/325349). (Le processus est semblable sur d’autres versions de Windows Server.)

L’arrêt du moteur de base de données à l’aide de la commande Transact-SQL **SHUTDOWN** requiert l’appartenance aux rôles serveur fixes **sysadmin** ou **serveradmin**, et n’est pas transférable.

## <a name="sql-server-configuration-manager"></a>Gestionnaire de configuration SQL Server

### <a name="starting-sql-server-configuration-manager"></a>Démarrage du Gestionnaire de configuration SQL Server

Dans le menu **Démarrer** , pointez sur **Tous les programmes**, sur **Microsoft SQL Server**, sur **Outils de configuration**, puis cliquez sur **Gestionnaire de configuration SQL Server**.

Étant donné que le Gestionnaire de configuration SQL Server est un composant logiciel enfichable pour le programme Microsoft Management Console et non un programme autonome, le Gestionnaire de configuration SQL Server n’apparaît pas en tant qu’application dans les versions plus récentes de Windows. Voici les chemins d’accès aux quatre dernières versions lorsque Windows est installé sur le lecteur C.  

|Version|Path|
|-|-|
|SQL Server 2019|C:\Windows\SysWOW64\SQLServerManager15.msc|
|SQL Server 2017|C:\Windows\SysWOW64\SQLServerManager14.msc|
|SQL Server 2016|C:\Windows\SysWOW64\SQLServerManager13.msc|
|SQL Server 2014|C:\Windows\SysWOW64\SQLServerManager12.msc|
|SQL Server 2012|C:\Windows\SysWOW64\SQLServerManager11.msc|

#### <a name="to-start-stop-pause-resume-or-restart-an-instance-of-the-sql-server-database-engine"></a><a name="configmande"></a> Pour démarrer, arrêter, suspendre, reprendre ou redémarrer une instance du moteur de base de données SQL Server

1. Démarrez le Gestionnaire de configuration SQL Server en suivant les instructions ci-dessus.

2. Si la boîte de dialogue **Contrôle de compte d'utilisateur** s'affiche, cliquez sur **Oui**.

3. Dans le Gestionnaire de configuration SQL Server, dans le volet gauche, cliquez sur **Services SQL Server**.

4. Dans le volet de résultats, cliquez avec le bouton droit sur **SQL Server (MSSQLServer)** ou sur une instance nommée, puis cliquez sur **Démarrer**, **Arrêter**, **Suspendre**, **Reprendre** ou **Redémarrer**.

5. Cliquez sur **OK** pour fermer le Gestionnaire de configuration SQL Server.

> [!NOTE]
> Pour démarrer une instance du moteur de base de données SQL Server avec les options de démarrage, consultez [Configurer les options de démarrage de serveur &#40;Gestionnaire de configuration SQL Server&#41;](../../database-engine/configure-windows/scm-services-configure-server-startup-options.md).  

#### <a name="to-start-stop-pause-resume-or-restart-the-sql-server-browser-or-an-instance-of-the-sql-server-agent"></a>Pour démarrer, arrêter, suspendre, reprendre, ou redémarrer SQL Server Browser ou une instance de SQL Server Agent

1. Démarrez le Gestionnaire de configuration SQL Server en suivant les instructions ci-dessus.

2. Si la boîte de dialogue **Contrôle de compte d'utilisateur** s'affiche, cliquez sur **Oui**.

3. Dans le Gestionnaire de configuration SQL Server, dans le volet gauche, cliquez sur **Services SQL Server**.

4. Dans le volet de résultats, cliquez avec le bouton droit sur **SQL Server Browser**, **SQL Server Agent (MSSQLServer)** ou **SQL Server Agent (<nom_instance>)** pour une instance nommée, puis cliquez sur **Démarrer**, **Arrêter**, **Suspendre**, **Reprendre** ou **Redémarrer**.  

5. Cliquez sur **OK** pour fermer le Gestionnaire de configuration SQL Server.  

> [!NOTE]
> SQL Server Agent ne peut pas être suspendu.

## <a name="sql-server-management-studio"></a>SQL Server Management Studio

### <a name="to-start-stop-pause-resume-or-restart-an-instance-of-the-sql-server-database-engine"></a><a name="ssmsde"></a> Pour démarrer, arrêter, suspendre, reprendre ou redémarrer une instance du moteur de base de données SQL Server

1. Dans l’Explorateur d’objets, connectez-vous à l’instance du moteur de base de données, cliquez avec le bouton droit sur l’instance du moteur de base de données à démarrer, puis cliquez sur **Démarrer**, **Arrêter**, **Suspendre**, **Reprendre** ou **Redémarrer**.

    Ou, dans Serveurs inscrits, cliquez avec le bouton droit sur l’instance du moteur de base de données à démarrer, pointez sur **Contrôle du service**, puis cliquez sur **Démarrer**, **Arrêter**, **Suspendre**, **Reprendre** ou **Redémarrer**.

2. Si la boîte de dialogue **Contrôle de compte d'utilisateur** s'affiche, cliquez sur **Oui**.

3. Quand le système vous demande si vous voulez agir, cliquez sur **Oui**.  

#### <a name="to-start-stop-or-restart-an-instance-of-the-sql-server-agent"></a>Pour démarrer, arrêter ou redémarrer une instance de SQL Server Agent

1. Dans l’Explorateur d’objets, connectez-vous à l’instance du moteur de base de données, cliquez avec le bouton droit sur **SQL Server Agent**, puis cliquez sur **Démarrer**, **Arrêter** ou **Redémarrer**.

2. Si la boîte de dialogue **Contrôle de compte d'utilisateur** s'affiche, cliquez sur **Oui**.

3. Quand le système vous demande si vous voulez agir, cliquez sur **Oui**.

## <a name="command-prompt-window-using-net-commands"></a><a name="CommandPrompt"></a> Fenêtre d’invite de commandes avec les commandes net

Les services Microsoft SQL Server peuvent être démarrés, arrêtés ou suspendus à l’aide des commandes **net** Microsoft Windows.

### <a name="to-start-the-default-instance-of-the-database-engine"></a><a name="dbDefault"></a> Pour démarrer l’instance par défaut du moteur de base de données

- À partir d'une invite de commandes, entrez l'une des commandes suivantes :  
  
    **net start "SQL Server (MSSQLSERVER)"**

   -ou-  

    **net start MSSQLSERVER**

### <a name="to-start-a-named-instance-of-the-database-engine"></a><a name="dbNamed"></a> Pour démarrer une instance nommée du moteur de base de données

- À partir d'une invite de commandes, entrez l'une des commandes suivantes. Remplacez le *\<instancename>* par le nom de l'instance à gérer.  
  
    **net start "SQL Server (** *nom_instance* **)"**
  
   -ou-  
  
    **net start MSSQL$** *nom_instance*  
  
### <a name="to-start-the-database-engine-with-startup-options"></a><a name="dbStartup"></a> Pour démarrer le moteur de base de données avec les options de démarrage  

- Ajoutez les options de démarrage à la fin de l’instruction **"SQL Server (MSSQLSERVER)"** , en les séparant par un espace. Lors d’un démarrage avec l’instruction **net start**, les options de démarrage utilisent une barre oblique (/) au lieu d’un tiret (-).  
  
    **net start "SQL Server (MSSQLSERVER)" /f /m**
  
   -ou-  
  
    **net start MSSQLSERVER /f /m**
  
  > [!NOTE]
  >  Pour plus d’informations sur les options de démarrage, consultez [Options de démarrage du service moteur de base de données](../../database-engine/configure-windows/database-engine-service-startup-options.md).  
  
###  <a name="to-start-the-sql-server-agent-on-the-default-instance-of-sql-server"></a><a name="agDefault"></a> Pour démarrer SQL Server Agent sur l’instance par défaut de SQL Server
  
- À partir d'une invite de commandes, entrez l'une des commandes suivantes :  
  
    **net start "SQL Server Agent (MSSQLSERVER)"**
  
   -ou-  
  
    **net start SQLSERVERAGENT**
  
###  <a name="to-start-the-sql-server-agent-on-a-named-instance-of-sql-server"></a><a name="agNamed"></a> Pour démarrer SQL Server Agent sur une instance nommée de SQL Server  
  
- À partir d'une invite de commandes, entrez l'une des commandes suivantes. Remplacez *nom_instance* par le nom de l’instance à gérer.  
  
    **net start "SQL Server Agent (** *nom_instance* **)"**
  
   -ou-  
  
    **net start SQLAgent$** *nom_instance*  
  
 Pour plus d’informations sur la façon d’exécuter SQL Server Agent en mode détaillé à des fins de résolution des problèmes, consultez [Application sqlagent90](../../tools/sqlagent90-application.md).  

### <a name="to-start-the-sql-server-browser"></a><a name="Browser"></a> Pour démarrer SQL Server Browser  

- À partir d'une invite de commandes, entrez l'une des commandes suivantes :  
  
    **net start "SQL Server Browser"**
  
   -ou-  
  
    **net start SQLBrowser**
  
### <a name="to-pause-or-stop-services-from-the-command-prompt-window"></a><a name="pauseStop"></a> Pour suspendre ou arrêter des services à partir de la fenêtre d'invite de commandes  

- Pour suspendre ou arrêter des services, modifiez les commandes des façons suivantes.  

- Pour suspendre un service, remplacez **net start** par **net pause**.  

- Pour arrêter un service, remplacez **net start** par **net stop**.  

## <a name="transact-sql"></a><a name="TsqlProcedure"></a> Transact-SQL

Le moteur de base de données peut être arrêté à l’aide de l’instruction **SHUTDOWN**.  

### <a name="to-stop-the-database-engine-using-transact-sql"></a>Pour arrêter le moteur de base de données avec Transact-SQL

- Pour attendre la fin des instructions Transact-SQL et des procédures stockées en cours d’exécution, puis arrêter le moteur de base de données, exécutez l’instruction suivante.  
  
    ```sql
    SHUTDOWN;
    ```
  
- Pour arrêter le moteur de base de données immédiatement, exécutez l’instruction suivante.  
  
    ```sql
    SHUTDOWN WITH NOWAIT;
    ```

Pour plus d’informations sur l’instruction **SHUTDOWN**, consultez [SHUTDOWN &#40;Transact-SQL&#41;](../../t-sql/language-elements/shutdown-transact-sql.md).  

## <a name="powershell"></a><a name="PowerShellProcedure"></a> PowerShell

### <a name="to-start-and-stop-database-engine-services"></a>Pour démarrer et arrêter des services Moteur de base de données

1. Dans une fenêtre d’invite de commandes, démarrez SQL Server PowerShell en exécutant la commande suivante.  

    ```cmd
    sqlps
    ```

2. À l’invite de commandes SQL Server PowerShell, exécutez la commande suivante. Remplacez `computername` par le nom de votre ordinateur.  

    ```powershell
    # Get a reference to the ManagedComputer class.
    CD SQLSERVER:\SQL\computername
    $Wmi = (get-item .).ManagedComputer
    ```

3. Identifiez le service que vous souhaitez arrêter ou démarrer. Choisissez l'une des lignes suivantes. Remplacez `instancename` par le nom de l'instance nommée.

    - Pour obtenir une référence à l’instance par défaut du moteur de base de données.

        ```powershell
        $DfltInstance = $Wmi.Services['MSSQLSERVER']
        ```

    - Pour obtenir une référence à une instance nommée du moteur de base de données.

        ```powershell
        $DfltInstance = $Wmi.Services['MSSQL$instancename']
        ```

    - Pour obtenir une référence au service SQL Server Agent sur l’instance par défaut du moteur de base de données.  

        ```powershell
        $DfltInstance = $Wmi.Services['SQLSERVERAGENT']
        ```

    - Pour obtenir une référence au service SQL Server Agent sur une instance nommée du moteur de base de données.  

        ```powershell
        $DfltInstance = $Wmi.Services['SQLAGENT$instancename']
        ```

    - Pour obtenir une référence au service SQL Server Browser.

        ```powershell
        $DfltInstance = $Wmi.Services['SQLBROWSER']
        ```

4. Terminez l'exemple pour démarrer, puis arrêter le service sélectionné.
  
    ```powershell
    # Display the state of the service.
    $DfltInstance
    # Start the service.
    $DfltInstance.Start();
    # Wait until the service has time to start.
    # Refresh the cache.  
    $DfltInstance.Refresh();
    # Display the state of the service.
    $DfltInstance
    # Stop the service.
    $DfltInstance.Stop();
    # Wait until the service has time to stop.
    # Refresh the cache.
    $DfltInstance.Refresh();
    # Display the state of the service.
    $DfltInstance
    ```  
  
##  <a name="using-service-controller-class"></a><a name="ServiceController"></a> Utilisation de la classe ServiceController

Vous pouvez utiliser la classe ServiceController pour contrôler le service SQL Server ou tout autre service Windows. Pour obtenir un exemple sur la façon de procéder, consultez [ServiceController, classe](/dotnet/api/system.serviceprocess.servicecontroller).

## <a name="manage-the-sql-server-service-on-linux"></a>Gérer le service SQL Server sur Linux

### <a name="to-start-stop-or-restart-an-instance-of-the-sql-server-database-engine"></a>Pour démarrer, arrêter ou redémarrer une instance du moteur de base de données SQL Server

Le code suivant indique comment démarrer, arrêter, redémarrer et vérifier l’état du [service SQL Server sur Linux](../../linux/sql-server-linux-troubleshooting-guide.md#manage-the-sql-server-service).

Vérifiez l’état du service SQL Server à l’aide de cette commande :

   ```bash
   sudo systemctl status mssql-server
   ```

Vous pouvez arrêter, démarrer ou redémarrer le service SQL Server en fonction des besoins à l’aide des commandes suivantes :

   ```bash
   sudo systemctl stop mssql-server
   sudo systemctl start mssql-server
   sudo systemctl restart mssql-server
   ```

## <a name="next-steps"></a>Étapes suivantes

- [Vue d’ensemble de la documentation d’installation de SQL Server](../install-windows/install-sql-server.md)
- [Afficher et lire les fichiers journaux d'installation de SQL Server](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md)
- [Gestionnaire de configuration SQL Server](../../relational-databases/sql-server-configuration-manager.md)
- [Démarrage de SQL Server avec une configuration minimale](../../database-engine/configure-windows/start-sql-server-with-minimal-configuration.md)
- [Fonctionnalités prises en charge par les éditions de SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md)