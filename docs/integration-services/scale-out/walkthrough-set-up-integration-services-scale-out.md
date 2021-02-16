---
title: 'Procédure pas à pas : Configurer SSIS Scale Out | Microsoft Docs'
description: Découvrez l’installation et la configuration de SQL Server Integration Services (SSIS) Scale Out Master.
ms.custom: performance
ms.date: 12/13/2017
ms.prod: sql
ms.prod_service: integration-services
ms.technology: integration-services
ms.topic: conceptual
author: HaoQian-MS
ms.author: haoqian
ms.reviewer: maghan
ms.openlocfilehash: d25471ef16b88ee0280ecb7dd349762b72e35312
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100337537"
---
# <a name="walkthrough-set-up-integration-services-ssis-scale-out"></a>Procédure pas à pas : Configurer Integration Services (SSIS) Scale Out

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


Configurez [!INCLUDE[ssISnoversion_md](../../includes/ssisnoversion-md.md)] (SSIS) Scale Out en effectuant les tâches suivantes. 

> [!TIP]
> Si vous installez Scale-out sur un seul ordinateur, installez les fonctionnalités Scale Out Master et Scale Out Worker en même temps. Quand vous installez les fonctionnalités en même temps, le point de terminaison est généré automatiquement pour la connexion à Scale Out Master. 

* [Installer Scale Out Master](#InstallMaster)

* [Installer Scale Out Worker](#InstallWorker)

* [Installer le certificat client Scale Out Worker](#InstallCert)

* [Ouvrir le port de pare-feu](#Firewall)

* [Démarrer les services SQL Server Scale Out Master et Worker](#Start)

* [Activer Scale Out Master](#EnableMaster)

* [Activer le mode d’authentification SQL Server](#EnableAuth)

* [Activer Scale Out Worker](#EnableWorker)

## <a name="install-scale-out-master"></a><a name="InstallMaster"></a> Installer Scale Out Master

Pour configurer Scale Out Master, vous devez installer les services du moteur de base de données, [!INCLUDE[ssISnoversion_md](../../includes/ssisnoversion-md.md)] et la fonctionnalité Scale Out Master de SSIS quand vous configurez [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)]. 

Pour plus d’informations sur la configuration du moteur de base de données et d’[!INCLUDE[ssISnoversion_md](../../includes/ssisnoversion-md.md)], consultez [Installer le moteur de base de données SQL Server](../../database-engine/install-windows/install-sql-server-database-engine.md) et [Installer Integration Services](../install-windows/install-integration-services.md).

> [!NOTE]
> Pour utiliser le compte d’authentification SQL Server par défaut pour la journalisation Scale-out, sélectionnez le mode mixte comme mode d’authentification dans la page **Configuration du moteur de base de données** pendant l’installation du moteur de base de données. Consultez [Changer le compte pour la journalisation Scale-out](change-logdb-account.md) pour plus d’informations.

Pour installer la fonctionnalité Scale Out Master, utilisez l’Assistant Installation de [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ou l’invite de commandes.

### <a name="install-scale-out-master-with-the-sql-server-installation-wizard"></a>Installer Scale Out Master avec l’Assistant Installation de SQL Server
1.  Dans la page **Sélection de fonctionnalités**, choisissez **Scale Out Master** sous [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].   
  
    ![Sélection de la fonctionnalité Master](media/feature-select-master.PNG)
  
2.  Dans la page **Configuration du serveur** , sélectionnez le compte pour exécuter le **service SQL Server Integration Services Scale Out Master** et sélectionnez le **Type de démarrage**.  
    ![Configuration de Serveur](media/server-config.PNG)

3.  Dans la page **Configuration d’Integration Services Scale Out Master** , spécifiez le numéro de port utilisé par Scale Out Master pour communiquer avec Scale Out Worker. Le numéro de port par défaut est 8391.  

    ![Configuration de Master](media/master-config.PNG "Configuration de Master")

4.  Spécifiez le certificat TLS/SSL utilisé pour protéger les communications entre Scale Out Master et Scale Out Worker en effectuant l’une des opérations suivantes.
    * Laissez le processus d’installation créer un certificat TLS/SSL auto-signé par défaut en cliquant sur **Créer un certificat SSL**.  Le certificat par défaut est installé sous Autorités de certification racines de confiance, Ordinateur local. Vous pouvez spécifier les noms communs dans ce certificat. Le nom d’hôte du point de terminaison principal doit être inclus dans les noms communs. Par défaut, le nom de machine et l’adresse IP du nœud Master sont inclus.
    * Sélectionnez un certificat TLS/SSL existant sur l’ordinateur local en cliquant sur **Utiliser un certificat TLS/SSL existant**, puis sur **Parcourir** pour sélectionner un certificat. L’empreinte numérique du certificat s’affiche dans la zone de texte. Cliquez sur **Parcourir** pour afficher les certificats stockés dans Autorités de certification racines de confiance, Ordinateur local. Le certificat que vous sélectionnez doit être stocké ici.       

    ![Configuration de Master 2](media/master-config-2.PNG "Configuration de Master 2")
  
5.  Terminez l’Assistant Installation de [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] .

### <a name="install-scale-out-master-from-the-command-prompt"></a>Installer Scale Out Master à partir de l’invite de commandes

Suivez les instructions fournies dans [Installer SQL Server à partir de l’invite de commandes](../../database-engine/install-windows/install-sql-server-from-the-command-prompt.md). Définissez les paramètres pour Scale Out Master en effectuant les opérations suivantes :
 
1.  Ajoutez `IS_Master` au paramètre `/FEATURES`

2.  Configurez Scale Out Master en spécifiant les paramètres suivants et leurs valeurs :
    -   `/ISMASTERSVCACCOUNT`
    -   `/ISMASTERSVCPASSWORD`
    -   `/ISMASTERSVCSTARTUPTYPE`
    -   `/ISMASTERSVCPORT`
    -   `/ISMasterSVCSSLCertCN` (facultatif)
    -   `/ISMASTERSVCTHUMBPRINT` (facultatif)

    > [!NOTE]
    > Si Scale Out Master n’est pas installé avec le moteur de base de données et que l’instance de celui-ci est une instance nommée, vous devez configurer `SqlServerName` dans le fichier de configuration du service Scale Out Master après l’installation. Pour plus d’informations, consultez [Scale Out Master](integration-services-ssis-scale-out-master.md).

## <a name="install-scale-out-worker"></a><a name="InstallWorker"></a> Installer Scale Out Worker
 
Pour configurer Scale Out Worker, vous devez installer [!INCLUDE[ssISnoversion_md](../../includes/ssisnoversion-md.md)] et sa fonctionnalité Scale Out Worker dans le programme d’installation de [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)].

Pour installer la fonctionnalité Scale Out Worker, utilisez l’Assistant Installation de [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ou l’invite de commandes.

### <a name="install-scale-out-worker-with-the-sql-server-installation-wizard"></a>Installer Scale Out Worker avec l’Assistant Installation de SQL Server

1.  Dans la page **Sélection de fonctionnalités**, choisissez **Scale Out Worker**, sous [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].

    ![Sélection de la fonctionnalité Worker](media/feature-select-worker.PNG)

2.  Dans la page **Configuration du serveur** , sélectionnez le compte pour exécuter le **service SQL Server Integration Services Scale Out Worker** et sélectionnez le **Type de démarrage**.

    ![Configuration de Serveur 2](media/server-config-2.PNG "Configuration de Serveur 2")

3.  Dans la page **Configuration d’Integration Services Scale Out Worker** , spécifiez le point de terminaison pour la connexion à Scale Out Master. 

    - Pour un environnement à **un seul ordinateur**, le point de terminaison est généré automatiquement quand Scale Out Master et Scale Out Worker sont installés en même temps. 

    - Pour un environnement à **plusieurs ordinateurs**, le point de terminaison se compose du nom ou de l’adresse IP de l’ordinateur où Scale Out Master est installé et du numéro de port spécifié pendant l’installation de Scale Out Master.
   
    ![Configuration de Worker 1](media/worker-config.PNG "Configuration de Worker 1")    

    > [!NOTE]
    > Vous pouvez également ignorer la configuration de Worker à ce stade et associer Scale Out Worker à Scale Out Master en utilisant [Scale Out Manager](integration-services-ssis-scale-out-manager.md) après l’installation.

4. Pour un environnement à **plusieurs ordinateurs**, spécifiez le certificat TLS/SSL client utilisé pour valider Scale Out Master. Pour un environnement à **un seul ordinateur**, il est inutile de spécifier un certificat TLS/SSL client. 
  
    Cliquez sur **Parcourir** pour rechercher le fichier de certificat (*.cer). Pour utiliser le certificat TLS/SSL par défaut, sélectionnez le fichier `SSISScaleOutMaster.cer` situé sous `\<drive\>:\Program Files\Microsoft SQL Server\140\DTS\Binn` sur l’ordinateur où Scale Out Master est installé.   

    ![Configuration de Worker 2](media/worker-config-2.PNG "Configuration de Worker 2")

    > [!NOTE]
    > Quand le certificat TLS/SSL utilisé par Scale Out Master est auto-signé, un certificat TLS/SSL client correspondant doit être installé sur l’ordinateur avec Scale Out Worker. Si vous fournissez le chemin de fichier du certificat client TLS/SSL dans la page **Configuration d’Integration Services Scale Out Worker**, le certificat est installé automatiquement. Sinon, vous devez installer le certificat manuellement plus tard. 
     
5. Terminez l’Assistant Installation de [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] .

### <a name="install-scale-out-worker-from-the-command-prompt"></a>Installer Scale Out Worker à partir de l’invite de commandes

Suivez les instructions fournies dans [Installer SQL Server à partir de l’invite de commandes](../../database-engine/install-windows/install-sql-server-from-the-command-prompt.md). Définissez les paramètres pour Scale Out Worker en effectuant les opérations suivantes :

1.  Ajoutez IS_Worker au paramètre `/FEATURES`.

2. Configurez Scale Out Worker en spécifiant les paramètres suivants et leurs valeurs :
    -   `/ISWORKERSVCACCOUNT`
    -   `/ISWORKERSVCPASSWORD`
    -   `/ISWORKERSVCSTARTUPTYPE`
    -   `/ISWORKERSVCMASTER` (facultatif)
    -   `/ISWORKERSVCCERT` (facultatif)
 
## <a name="install-scale-out-worker-client-certificate"></a><a name="InstallCert"></a> Installer le certificat client Scale Out Worker

Pendant l’installation de Scale Out Worker, un certificat Worker est créé et installé automatiquement sur l’ordinateur. Un certificat client correspondant, SSISScaleOutWorker.cer, est aussi installé sous `\<drive\>:\Program Files\Microsoft SQL Server\140\DTS\Binn`. Pour que Scale Out Master authentifie Scale Out Worker, vous devez ajouter ce certificat client au magasin racine de l’ordinateur local où Scale Out Master est installé.
  
Pour ajouter le certificat client au magasin racine, double-cliquez sur le fichier .cer et cliquez sur **Installer le certificat** dans la boîte de dialogue Certificat. **L’Assistant Importation de certificat** s’ouvre.  

## <a name="open-firewall-port"></a><a name="Firewall"></a> Ouvrir le port de pare-feu

Sur l’ordinateur Scale Out Master, ouvrez le port spécifié pendant l’installation de Scale Out Master et le port de SQL Server (1433, par défaut) dans le Pare-feu Windows.

> [!Note]
> Après avoir ouvert le port du pare-feu, vous devez également redémarrer le service Scale Out Worker.
    
## <a name="start-sql-server-scale-out-master-and-worker-services"></a><a name="Start"></a> Démarrer les services SQL Server Scale Out Master et Worker

Si vous n’avez pas défini le type de démarrage des services comme étant **automatique** pendant l’installation, démarrez les services suivants :

-   SQL Server Integration Services Scale Out Master 14.0 (SSISScaleOutMaster140)

-   SQL Server Integration Services Scale Out Worker 14.0 (SSISScaleOutWorker140)

## <a name="enable-scale-out-master"></a><a name="EnableMaster"></a> Activer Scale Out Master

Quand vous créez le catalogue SSISDB dans [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] [!INCLUDE[ssManStudio_md](../../includes/ssmanstudio-md.md)], sélectionnez **Activer ce serveur comme SSIS Scale Out Master** dans la boîte de dialogue **Créer un catalogue**.

Une fois le catalogue créé, vous pouvez activer Scale Out Master avec [Scale Out Manager](integration-services-ssis-scale-out-manager.md).

## <a name="enable-sql-server-authentication-mode"></a><a name="EnableAuth"></a> Activer le mode d’authentification SQL Server
Si vous n’avez pas activé l’authentification [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] pendant l’installation du moteur de base de données, activez le mode d’authentification SQL Server sur l’instance de [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] qui héberge le catalogue SSISDB. 

L’exécution des packages n’est pas bloquée quand l’authentification SQL Server est désactivée. Toutefois, le journal d’exécution ne peut pas écrire dans la base de données SSISDB.

## <a name="enable-scale-out-worker"></a><a name="EnableWorker"></a> Activer Scale Out Worker

Vous pouvez activer Scale Out Worker avec [Scale Out Manager](integration-services-ssis-scale-out-manager.md), qui fournit une interface graphique utilisateur, ou avec une procédure stockée.

Pour activer Scale Out Worker avec une procédure stockée, exécutez la procédure stockée `[catalog].[enable_worker_agent]` avec **WorkerAgentId** comme paramètre. 

Obtenez la valeur de **WorkerAgentId** à partir de la vue `[catalog].[worker_agents]` dans SSISDB, une fois que Scale Out Worker est inscrit auprès de Scale Out Master. L’inscription prend plusieurs minutes une fois que les services Scale Out Master et Worker sont démarrés.

#### <a name="example"></a>Exemple
L’exemple suivant active Scale Out Worker sur `computerA`.

```sql
SELECT WorkerAgentId, MachineName FROM [catalog].[worker_agents]
GO
-- Result: --
-- WorkerAgentId                           MachineName  --
-- 6583054A-E915-4C2A-80E4-C765E79EF61D    computerA    --

EXEC [catalog].[enable_worker_agent] '6583054A-E915-4C2A-80E4-C765E79EF61D'
GO 
```

## <a name="next-steps"></a>Étapes suivantes
-   [Exécuter des packages dans SSIS (SQL Server Integration Services) Scale Out](run-packages-in-integration-services-ssis-scale-out.md).