---
title: Configurer une installation Server Core
description: Cet article décrit en détail la configuration de SQL Server sur une installation Server Core, y compris des outils de dépannage.
ms.custom: seo-lt-2019
ms.date: 12/13/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
helpviewer_keywords:
- IsHadrEnabled server property
- Server Core Installation [SQL Server]
ms.assetid: ed6e5e94-4b8d-422a-a17e-61b05a4df903
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 300e06b7eaab03af63685e495d8aa0de2ad83ed6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336382"
---
# <a name="configure-sql-server-on-a-server-core-installation"></a>Configurer SQL Server sur une installation Server Core

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]

Cet article fournit des détails sur la configuration de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur une installation Server Core.  

##  <a name="configure-and-manage-server-core-on-windows-server"></a><a name="BKMK_ConfigureWindows"></a> Configurer et gérer Server Core sur Windows Server  
Cette section fournit des références aux articles qui vous guident pour la configuration et la gestion d’une installation Server Core.  
  
Certaines fonctionnalités de [!INCLUDE[ssNoVersion](../../includes/ssNoVersion-md.md)] ne sont pas prises en charge en mode Server Core.  Une partie de ces fonctionnalités peuvent être installées sur un ordinateur client ou un serveur différent qui n'exécute pas Server Core, et être connectées aux services de moteur de base de données installés sur Server Core.  
  
Pour plus d’informations sur la configuration et la gestion d’une installation Server Core à distance, consultez les articles suivants :  
  
- [Installation de Server Core](/windows-server/get-started/getting-started-with-server-core)  
  
- [Configurer une installation Server Core de Windows Server 2016 avec Sconfig.cmd](/windows-server/get-started/sconfig-on-ws2016)  
  
- [Installer des rôles et fonctionnalités de serveur sur un serveur Server Core Windows Server 2012 R2](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj574158(v=ws.11))
  
- [Gestion d’une installation Server Core : Vue d’ensemble](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee441255(v=ws.10))  
  
- [Administration d’une installation Server Core](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee441258(v=ws.10))
  
##  <a name="install-ssnoversion-updates"></a><a name="BKMK_InstallSQLUpdates"></a> Installer les mises à jour de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
Cette section fournit des informations sur l'installation des mises à jour de [!INCLUDE[ssNoVersion](../../includes/ssNoVersion-md.md)] sur un ordinateur Windows Server Core. Il est recommandé que les clients évaluent et installent les dernières mises à jour de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en temps voulu pour s'assurer que les systèmes sont à jour avec des mises à jour de sécurité les plus récentes. Pour plus d’informations sur l’installation de [!INCLUDE[ssNoVersion](../../includes/ssNoVersion-md.md)] sur une machine Windows Server Core, consultez [Installer SQL Server sur Server Core](../../database-engine/install-windows/install-sql-server-on-server-core.md).  
  
Voici deux scénarios d'installation des mises à jour du produit :  
  
- [Installation des mises à jour de SQL Server pendant une nouvelle installation](../../database-engine/install-windows/configure-sql-server-on-a-server-core-installation.md#bkmk_NewInstall)  
  
- [Installation des mises à jour de SQL Server après qu’il a été installé](../../database-engine/install-windows/configure-sql-server-on-a-server-core-installation.md#bkmk_alreadyInstall)  
  
###  <a name="installing-updates-for-ssnoversion-during-a-new-installation"></a><a name="bkmk_NewInstall"></a> Installation des mises à jour de [!INCLUDE[ssNoVersion](../../includes/ssNoVersion-md.md)] pendant une nouvelle installation  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prend en charge uniquement les installations par invite de commandes sur le système d'exploitation Server Core. Pour plus d’informations, consultez [Installer SQL Server à partir de l’invite de commandes](./install-sql-server-from-the-command-prompt.md).  
  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] intègre les dernières mises à jour du produit avec l'installation principale, de sorte que le produit principal et les mises à jour applicables sont installés en même temps.  
  
Une fois que le programme d'installation a détecté les versions les plus récentes des mises à jour applicables, il les télécharge et les intègre dans le processus d'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en cours. La fonctionnalité de mise à jour du produit peut extraire une mise à jour, un Service Pack, ou un Service Pack et la mise à jour cumulative.  
  
Spécifiez les paramètres UpdateEnabled et UpdateSource pour inclure les dernières mises à jour du produit à l'installation principale. Reportez-vous à l'exemple suivant pour activer les mises à jour du produit pendant l'exécution du programme d'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] :  
  
```  
Setup.exe /qs /ACTION=Install /FEATURES=SQLEngine /INSTANCENAME=MSSQLSERVER /SQLSVCACCOUNT="<DomainName\UserName>" /SQLSVCPASSWORD="<StrongPassword>" /SQLSYSADMINACCOUNTS="<DomainName\UserName>" /AGTSVCACCOUNT="NT AUTHORITY\Network Service" /UpdateEnabled=True /UpdateSource="<SourcePath>" /IACCEPTSQLSERVERLICENSETERMS  
```  
  
###  <a name="installing-updates-for-ssnoversion-after-it-has-been-installed"></a><a name="bkmk_alreadyInstall"></a> Installation des mises à jour de [!INCLUDE[ssNoVersion](../../includes/ssNoVersion-md.md)] après qu'il a déjà été installé  
Sur une instance installée de [!INCLUDE[ssNoVersion](../../includes/ssNoVersion-md.md)], nous vous recommandons d'appliquer les dernières mises à jour de sécurité et mises à jour critique comprenant les versions générales de distribution (GDRs) et les Services Pack (SP). Différentes mises à jour cumulatives et mises à jour de sécurité doivent être adoptées au cas par cas, « si nécessaires ». Évaluez si la mise à jour est nécessaire, puis appliquez-la.  
  
Appliquez une mise à jour à partir de l'invite de commandes en remplaçant <nom_package> par le nom de votre package de mise à jour :  
  
- Mettez à jour une seule instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et tous les composants partagés. Vous pouvez spécifier l'instance à l'aide du paramètre InstanceName ou du paramètre InstanceID.  
  
    ```  
    <package_name>.exe /qs /IAcceptSQLServerLicenseTerms /Action=Patch /InstanceName=MyInstance  
    ```  
  
- Mettez à jour les composants partagés de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] uniquement :  
  
    ```  
    <package_name>.exe /qs /IAcceptSQLServerLicenseTerms /Action=Patch  
    ```  
  
- Mettez à jour toutes les instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur l'ordinateur et tous les composants partagés :  
  
    ```  
    <package_name>.exe /qs /IAcceptSQLServerLicenseTerms /Action=Patch /AllInstances  
    ```  
  
## <a name="startstop-ssnoversion-service"></a><a name="BKMK_StartStopServices"></a> Démarrer/arrêter un service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
L’application [Application sqlservr](../../tools/sqlservr-application.md) démarre, arrête, suspend et poursuit une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à partir d’une invite de commandes.  
  
Vous pouvez également utiliser les services .Net pour démarrer et arrêter les services [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="enable-alwayson-availability-groups"></a><a name="BKMK_EnableAlwaysON"></a> Activer les groupes de disponibilité AlwaysOn  
Les groupes de disponibilité AlwaysOn doivent être activés pour qu'une instance de serveur utilise les groupes de disponibilité comme solution de haute disponibilité et de récupération d'urgence. Pour plus d’informations sur la gestion des groupes de disponibilité AlwaysOn, consultez [Activer et désactiver les groupes de disponibilité AlwaysOn (SQL Server)](../../database-engine/availability-groups/windows/enable-and-disable-always-on-availability-groups-sql-server.md).  
  
### <a name="using-ssnoversion-configuration-manager-remotely"></a>Utilisation du Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à distance  
Ces étapes doivent être effectuées sur un PC exécutant l’édition cliente de Windows, ou sur un serveur Windows Server où l’interpréteur de commandes graphique de serveur est installé.  
  
1. Ouvrez **Gestion de l’ordinateur**. Pour ouvrir **Gestion de l’ordinateur**, cliquez sur **Démarrer**, tapez `compmgmt.msc` puis cliquez sur **OK**.    
  
2. Dans l’arborescence de la console, cliquez avec le bouton droit sur **Gestion de l’ordinateur** puis cliquez sur **Se connecter à un autre ordinateur...** .  
  
3. Dans la boîte de dialogue **Sélectionner un ordinateur**, tapez le nom de la machine Server Core que vous voulez gérer, ou cliquez sur **Parcourir** pour la localiser, puis cliquez sur **OK**.  
  
4. Dans l’arborescence de la console, sous **Gestion de l’ordinateur** de la machine Server Core, cliquez sur **Services et applications**.  
  
5. Double-cliquez sur **Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** .  
  
6. Dans **Configuration Manager [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** , cliquez sur **Services [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** , cliquez avec le bouton droit sur **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** (\<instance name>), où \<instance name> est le nom d’une instance de serveur local pour laquelle vous voulez activer les groupes de disponibilité AlwaysOn, puis cliquez sur Propriétés.  
  
7. Sélectionnez l'onglet **Haute disponibilité AlwaysOn** .  
  
8. Vérifiez que champ Nom du cluster de basculement Windows contient le nom du nœud de cluster de basculement local. Si ce champ est vide, cette instance de serveur ne prend pas actuellement en charge les groupes de disponibilité AlwaysOn. L’ordinateur local n’est pas un nœud de cluster, le cluster WSFC a été arrêté ou bien cette édition de [!INCLUDE[ssNoVersion](../../includes/ssNoVersion-md.md)] ne prend pas en charge les groupes de disponibilité AlwaysOn.  
  
9. Activez la case à cocher Activer les groupes de disponibilité AlwaysOn, puis cliquez sur OK.  
  
10. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] enregistre votre modification. Ensuite, vous devez redémarrer manuellement le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Cela vous permet de choisir l'heure de redémarrage la plus adaptée aux besoins de l'entreprise. Lorsque le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] redémarre, AlwaysOn est activé, et la propriété de serveur IsHadrEnabled a la valeur 1.  
  
> [!NOTE]
>  -   Vous devez posséder les droits d'utilisateur appropriés, ou vous devez avoir l'autorisation appropriée sur l'ordinateur cible pour vous connecter à cet ordinateur.  
> -   Le nom de l'ordinateur que vous gérez apparaît entre parenthèses en regard de Gestion d'ordinateur dans l'arborescence de la console.  
  
### <a name="using-powershell-cmdlets-to-enable-alwayson-availability-groups"></a>Utilisation des applets de commande PowerShell pour activer les groupes de disponibilité AlwaysOn  
L'applet de commande PowerShell Enable-SqlAlwaysOn sert à activer la fonctionnalité Groupe de disponibilité AlwaysOn sur une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si Groupes de disponibilité AlwaysOn est activé pendant que le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est en cours d'exécution, le service Moteur de base de données doit être redémarré pour terminer les modifications. Sauf si vous spécifiez le paramètre -Force, l'applet de commande vous demande si vous voulez redémarrer le service ; en cas d'annulation, aucune opération ne se produit.  
  
Vous devez disposer des autorisations d'administrateur pour exécuter cette applet de commande.  
  
Vous pouvez utiliser l'une des syntaxes suivantes pour activer les groupes de disponibilité AlwaysOn pour une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]:  
  
```  
Enable-SqlAlwaysOn [-Path <string>] [-Credential <PSCredential>] [-Force] [-NoServiceRestart] [-Confirm] [-WhatIf] [<Commom Parameters>]  
```  
  
```  
Enable-SqlAlwaysOn -InputObject <Server> [-Credential <PSCredential>] [-Force] [-NoServiceRestart] [-Confirm] [-WhatIf] [<Commom Parameters>]  
```  
  
```  
Enable-SqlAlwaysOn [-ServerInstance <string>] [-Credential <PSCredential>] [-Force] [-NoServiceRestart] [-Confirm] [-WhatIf] [<Commom Parameters>]  
```  
  
La commande PowerShell suivante active les groupes de disponibilité AlwaysOn sur une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (Ordinateur\instance) :  
  
```  
Enable-SqlAlwaysOn -Path SQLSERVER:\SQL\Machine\Instance  
```  
  
##  <a name="configuring-remote-access-of-ssnoversion-running-on-server-core"></a><a name="BKMK_ConfigureRemoteAccess"></a> Configuration de l'accès à distance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] s'exécutant sur Server Core  
 Effectuez les actions décrites ci-dessous pour configurer l’accès à distance d’une instance de [!INCLUDE[ssNoVersion](../../includes/ssNoVersion-md.md)] qui s’exécute sur Windows Server Core.  
  
### <a name="enable-remote-connections-on-the-instance-of-ssnoversion"></a>Activer les connexions distantes sur l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
 Pour activer les connexions distantes, utilisez SQLCMD.exe localement et exécutez les instructions suivantes sur l'instance de Server Core :  
  
-   `EXEC sys.sp_configure N'remote access', N'1'`  
  
     `GO`  
  
-   `RECONFIGURE WITH OVERRIDE`  
  
     `GO`  
  
### <a name="enable-and-start-the-ssnoversion-browser-service"></a>Activer et démarrer le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser  
 Par défaut, le service Browser est désactivé.  Si elle est désactivée sur une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] s'exécutant sur Server Core, exécutez la commande suivante à partir de l'invite de commandes pour l'activer :  
  
 `sc config SQLBROWSER start= auto`  
  
 Après activation, exécutez la commande suivante à partir de l'invite de commandes pour démarrer le service :  
  
 `net start SQLBROWSER`  
  
### <a name="create-exceptions-in-windows-firewall"></a>Créer des exceptions dans le pare-feu Windows  
 Pour créer des exceptions pour l’accès à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans le Pare-feu Windows, suivez les étapes spécifiées dans [Configurer le Pare-feu Windows pour autoriser l’accès à SQL Server](../../sql-server/install/configure-the-windows-firewall-to-allow-sql-server-access.md).  
  
### <a name="enable-tcpip-on-the-instance-of-ssnoversion"></a>Activer TCP/IP sur l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
 Le protocole TCP/IP peut être activé via Windows PowerShell pour une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur Server Core. Procédez comme suit :  
  
1.  Sur un ordinateur qui exécute Windows Server Core, lancez le **Gestionnaire des tâches**.  
  
2.  Dans l'onglet **Applications** , cliquez sur **Nouvelle tâche**.  
  
3.  Dans la boîte de dialogue **Créer une nouvelle tâche** , tapez **sqlps.exe** dans le champ **Ouvrir** , puis cliquez sur **OK**. Cela ouvre la fenêtre **Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Powershell**.  
  
4.  Dans la fenêtre **Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Powershell**, exécutez le script suivant pour activer le protocole TCP/IP :  
  
```powershell
$smo = 'Microsoft.SqlServer.Management.Smo.'  
$wmi = new-object ($smo + 'Wmi.ManagedComputer')  
# Enable the TCP protocol on the default instance.  If the instance is named, replace MSSQLSERVER with the instance name in the following line.  
$uri = "ManagedComputer[@Name='" + (get-item env:\computername).Value + "']/ServerInstance[@Name='MSSQLSERVER']/ServerProtocol[@Name='Tcp']"  
$Tcp = $wmi.GetSmoObject($uri)  
$Tcp.IsEnabled = $true  
$Tcp.Alter()  
$Tcp  
```  
  
##  <a name="ssnoversion-profiler"></a><a name="BKMK_Profiler"></a> [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Profiler  
 Sur un ordinateur distant, démarrez [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] et sélectionnez Nouvelle trace dans le menu Fichier, l'application affiche la boîte de dialogue Se connecter au serveur, dans laquelle vous pouvez spécifier l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , résidant sur l'ordinateur Server Core, à laquelle vous souhaitez vous connecter. Pour plus d'informations, consultez [Start SQL Server Profiler](../../tools/sql-server-profiler/start-sql-server-profiler.md).  
  
 Pour plus d’informations sur les autorisations nécessaires pour exécuter [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)], consultez [Autorisations nécessaires pour exécuter SQL Server Profiler](../../tools/sql-server-profiler/permissions-required-to-run-sql-server-profiler.md).  
  
 Pour obtenir des détails supplémentaires sur [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)], consultez [SQL Server Profiler](../../tools/sql-server-profiler/sql-server-profiler.md).  
  
##  <a name="ssnoversion-auditing"></a><a name="BKMK_Auditing"></a> [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Audit  
 Vous pouvez utiliser [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou [!INCLUDE[tsql](../../includes/tsql-md.md)] à distance pour définir un audit. Une fois l'audit créé et activé, la cible reçoit des entrées. Pour plus d’informations sur la création et la gestion d’audits [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez [SQL Server Audit &#40;Moteur de base de données &#41;](../../relational-databases/security/auditing/sql-server-audit-database-engine.md).  
  
##  <a name="command-prompt-utilities"></a><a name="BKMK_CMD"></a> Utilitaires de ligne de commande  
 Vous pouvez utiliser les utilitaires d'invite de commandes suivants pour écrire des opérations [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur un ordinateur Server Core. Le tableau suivant contient la liste des utilitaires d'invite de commandes fournis avec [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour Server Core :  
  
|**Utilitaire**|**Description**|**Installé dans**|  
|-----------------|---------------------|----------------------|  
|[Utilitaire bcp](../../tools/bcp-utility.md)|Utilisé pour copier des données entre une instance de [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et un fichier de données dans un format spécifié par l’utilisateur.|[!INCLUDE[ssInstallPathVar](../../includes/ssinstallpathvar-md.md)]Tools\Binn|  
|[Utilitaire dtexec](../../integration-services/packages/dtexec-utility.md)|Sert à configurer et à exécuter un package [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .|[!INCLUDE[ssInstallPathVar](../../includes/ssinstallpathvar-md.md)]DTS\Binn|  
|[Utilitaire dtutil](../../integration-services/dtutil-utility.md)|Utilisé pour gérer les packages SSIS.|[!INCLUDE[ssInstallPathVar](../../includes/ssinstallpathvar-md.md)]DTS\Binn|  
|[Utilitaire osql](../../tools/osql-utility.md)|Vous permet d'entrer des instructions, des procédures système et des fichiers de script [!INCLUDE[tsql](../../includes/tsql-md.md)] à l'invite de commandes.|[!INCLUDE[ssInstallPathVar](../../includes/ssinstallpathvar-md.md)]Tools\Binn|  
|[Application sqlagent90](../../tools/sqlagent90-application.md)|Utilisé pour démarrer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent à partir d'une invite de commandes.|\<drive>:\Program Files\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\\<*nom_instance*>\MSSQL\Binn|  
|[Utilitaire sqlcmd](../../tools/sqlcmd-utility.md)|Vous permet d'entrer des instructions, des procédures système et des fichiers de script [!INCLUDE[tsql](../../includes/tsql-md.md)] à l'invite de commandes.|[!INCLUDE[ssInstallPathVar](../../includes/ssinstallpathvar-md.md)]Tools\Binn|  
|[Utilitaire SQLdiag](../../tools/sqldiag-utility.md)|Sert à recueillir des informations de diagnostic pour le service de support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)] .|[!INCLUDE[ssInstallPathVar](../../includes/ssinstallpathvar-md.md)]Tools\Binn|  
|[Utilitaire sqlmaint](../../tools/sqlmaint-utility.md)|Sert à exécuter des plans de maintenance de bases de données créés dans des versions précédentes de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|\<drive>:\Program Files\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\MSSQL14.MSSQLSERVER\MSSQL\Binn|  
|[Utilitaire sqlps](../../tools/sqlps-utility.md)|Sert à exécuter des commandes et des scripts PowerShell. Charge et inscrit le fournisseur PowerShell [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et les cmdlets.|[!INCLUDE[ssInstallPathVar](../../includes/ssinstallpathvar-md.md)]Tools\Binn|  
|[Application sqlservr](../../tools/sqlservr-application.md)|Sert à démarrer et arrêter une instance de [!INCLUDE[ssDE](../../includes/ssde-md.md)] à partir de l'invite de commandes pour le dépannage.|\<drive>:\Program Files\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\MSSQL14.MSSQLSERVER\MSSQL\Binn|  
  
##  <a name="use-troubleshooting-tools"></a><a name="BKMK_troubleshoot"></a> Utiliser les outils de dépannage  
 Vous pouvez utiliser l’ [utilitaire SQLdiag](../../tools/sqldiag-utility.md) pour collecter des fichiers journaux et des fichiers de données à partir de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et depuis d’autres types de serveurs, mais aussi analyser vos serveurs au fil des jours ou trouver des solutions à des problèmes spécifiques les concernant. SQLdiag a été conçu pour accélérer et simplifier la collecte d'informations de diagnostic pour les services d'assistance Microsoft.  
  
 Vous pouvez lancer l’utilitaire dans l’invite de commandes d’administrateur sur Server Core, à l’aide de la syntaxe spécifiée dans l’article : [Utilitaire SQLdiag](../../tools/sqldiag-utility.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Installer SQL Server sur Server Core](../../database-engine/install-windows/install-sql-server-on-server-core.md)   
 [Articles de procédures relatives à l’installation](/previous-versions/sql/)  
  
