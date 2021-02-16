---
title: 'Installer : PowerShell Desired State Configuration'
description: Installez SQL Server à l’aide de PowerShell DSC et découvrez l’installation initiale d’une instance autonome de SQL Server 2017 sur Windows Server 2016.
ms.custom: seo-lt-2019
ms.date: 12/13/2019
ms.devlang: PowerShell
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
author: randomnote1
ms.author: dareist
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: a1105db9984a8ff52485247e6699c578ec350986
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100061444"
---
# <a name="install-sql-server-with-powershell-desired-state-configuration"></a>Installer SQL Server avec la configuration d’état souhaité PowerShell

Ne vous est-il jamais arrivé de parcourir l’interface d’installation SQL Server juste en cliquant sur les mêmes boutons et en y entrant les informations habituelles sans réfléchir ? L’installation terminée, vous vous rendez compte que vous avez oublié de spécifier le groupe DBA dans le rôle **sysadmin**. Vous devez alors effectuer les opérations suivantes :
* Passer en mode mono-utilisateur.
* Ajouter les utilisateurs ou groupes appropriés.
* Revenir en mode multi-utilisateur.
* Effectuer des tests. 

Le pire, c’est que vous n’avez même plus confiance en l’installation que vous venez d’effectuer. « Ai-je oublié autre chose ? » vous pouvez vous demander.

Découvrez plus en détail la [configuration DSC (Desired State Configuration) PowerShell](/powershell/scripting/dsc/overview/overview). À l’aide de DSC, vous créez un modèle de configuration qui peut être réutilisé sur plusieurs centaines voire milliers de serveurs. En fonction de la build, vous aurez peut-être quelques paramètres d’installation à ajuster, mais rien de très important, car vous pouvez conserver tous les paramètres standard. Ainsi, vous ne pouvez plus oublier d’entrer un paramètre important.

Cet article décrit la configuration initiale d’une instance autonome de SQL Server 2017 sur Windows Server 2016 à l’aide de la ressource DSC **SqlServerDsc**. Il est utile de connaître déjà DSC, car nous n’allons pas aborder son fonctionnement.

Pour cette procédure pas à pas, les éléments suivants sont nécessaires :

- Un ordinateur qui exécute Windows Server 2016.
- Un support d’installation de SQL Server 2017.
- La ressource DSC **SqlServerDsc**.

## <a name="prerequisites"></a>Prérequis

Dans la plupart des cas, DSC sert à gérer les prérequis. Toutefois, pour cette démonstration, nous allons gérer les prérequis manuellement.

## <a name="install-the-sqlserverdsc-dsc-resource"></a>Installer la ressource SqlServerDsc de DSC

Téléchargez la ressource DSC [SqlServerDsc](https://www.powershellgallery.com/packages/SqlServerDsc) à partir de [PowerShell Gallery](https://www.powershellgallery.com/) à l’aide de l’applet de commande [Install-Module](/powershell/module/powershellget/Install-Module). 

> [!NOTE]
> Pour installer le module, vérifiez que PowerShell est exécuté **en tant qu’administrateur**.

```PowerShell
Install-Module -Name SqlServerDsc
```

### <a name="get-the-sql-server-2017-installation-media"></a>Obtenir le support d’installation de SQL Server 2017
Téléchargez le support d’installation de SQL Server 2017 sur le serveur. Nous avons téléchargé SQL Server 2017 Enterprise à partir d’un abonnement Visual Studio et copié l’image ISO à l’emplacement `C:\en_sql_server_2017_enterprise_x64_dvd_11293666.iso`.

Maintenant, l’image ISO doit être extraite dans un répertoire :

```PowerShell
New-Item -Path C:\SQL2017 -ItemType Directory
$mountResult = Mount-DiskImage -ImagePath 'C:\en_sql_server_2017_enterprise_x64_dvd_11293666.iso' -PassThru
$volumeInfo = $mountResult | Get-Volume
$driveInfo = Get-PSDrive -Name $volumeInfo.DriveLetter
Copy-Item -Path ( Join-Path -Path $driveInfo.Root -ChildPath '*' ) -Destination C:\SQL2017\ -Recurse
Dismount-DiskImage -ImagePath 'C:\en_sql_server_2017_enterprise_x64_dvd_11293666.iso'
```

## <a name="create-the-configuration"></a>Créer la configuration

### <a name="configuration"></a>Configuration

Créez la fonction de configuration qui sera appelée pour générer les documents [MOF (Managed Object Format)](/windows/desktop/WmiSdk/managed-object-format--mof-) :

```PowerShell
Configuration SQLInstall
{...}
```

### <a name="modules"></a>Modules

Importez les modules dans la session active. Ces modules indiquent au document de configuration comment générer des documents MOF. Ils indiquent également au moteur DSC comment appliquer les documents MOF au serveur :

```PowerShell
Import-DscResource -ModuleName SqlServerDsc
```

### <a name="resources"></a>Ressources

#### <a name="net-framework"></a>.NET Framework

SQL Server s’appuie sur le .NET Framework. Nous devons donc l’installer avant d’installer SQL Server. La ressource **WindowsFeature** est utilisée pour installer la fonctionnalité Windows **Net-Framework-45-Core** :

```PowerShell
WindowsFeature 'NetFramework45'
{
     Name = 'Net-Framework-45-Core'
     Ensure = 'Present'
}
```

#### <a name="sqlsetup"></a>SqlSetup

La ressource **SqlSetup** est utilisée pour indiquer à DSC comment installer SQL Server. Les paramètres nécessaires à une installation de base sont les suivants :

- **InstanceName**. Nom de l'instance. Utilisez **MSSQLSERVER** pour une instance par défaut.
- **Features**. Fonctionnalités à installer. Dans cet exemple, nous installons uniquement la fonctionnalité **SQLEngine**.
- **SourcePath**. Chemin du support d’installation de SQL. Dans cet exemple, nous avons stocké le support d’installation de SQL dans `C:\SQL2017`. Un partage réseau peut réduire l’espace utilisé sur le serveur.
- **SQLSysAdminAccounts**. Utilisateurs ou groupes qui doivent être membres du rôle **sysadmin**. Dans cet exemple, nous allons accorder un accès **sysadmin** au groupe Administrateurs local. 

> [!NOTE]
> Cette configuration n’est pas recommandée dans un environnement hautement sécurisé.

La liste complète et la description des paramètres **SqlSetup** sont disponibles dans le [dépôt GitHub SqlServerDsc](https://github.com/PowerShell/SqlServerDsc/tree/master#sqlsetup).

La ressource **SqlSetup** installe uniquement SQL Server **sans** conserver les paramètres appliqués. C’est le cas par exemple si les **SQLSysAdminAccounts** sont spécifiés au moment de l’installation. Un administrateur peut ajouter ou supprimer des connexions dans le rôle **sysadmin**. La ressource **SqlSetup** n’est pas affectée. Si vous voulez que DSC applique l’appartenance au rôle **sysadmin**, utilisez la ressource [SqlServerRole](https://github.com/PowerShell/SqlServerDsc/tree/master#sqlserverrole).

#### <a name="finish-configuration"></a>Terminer la configuration

```PowerShell
Configuration SQLInstall
{
     Import-DscResource -ModuleName SqlServerDsc

     node localhost
     {
          WindowsFeature 'NetFramework45'
          {
               Name   = 'NET-Framework-45-Core'
               Ensure = 'Present'
          }

          SqlSetup 'InstallDefaultInstance'
          {
               InstanceName        = 'MSSQLSERVER'
               Features            = 'SQLENGINE'
               SourcePath          = 'C:\SQL2017'
               SQLSysAdminAccounts = @('Administrators')
               DependsOn           = '[WindowsFeature]NetFramework45'
          }
     }
}
```

## <a name="build-and-deploy"></a>Générer et déployer

### <a name="compile-the-configuration"></a>Compiler la configuration

« Dot sourcez » le script de configuration :

```PowerShell
. .\SQLInstallConfiguration.ps1
```

Exécutez la fonction de configuration :

```PowerShell
SQLInstall
```

Un répertoire appelé **SQLInstall** est créé dans le répertoire de travail. Il contient un fichier appelé **localhost.mof**. Examinez le contenu du fichier MOF, qui affiche la configuration DSC compilée.

### <a name="deploy-the-configuration"></a>Déployer la configuration

Pour démarrer le déploiement DSC de SQL Server, appelez l’applet de commande **Start-DscConfiguration**. Les paramètres suivants sont fournis à l’applet de commande :

- **Path**. Chemin du dossier contenant les documents MOF à déployer, par exemple `C:\SQLInstall`.
- **Wait**. Attend la fin de la tâche de configuration.
- **Force**. Remplace toutes les configurations DSC existantes.
- **Verbose**. Affiche la sortie détaillée. Utile lors du premier envoi (push) d’une configuration pour faciliter le dépannage.

```PowerShell
Start-DscConfiguration -Path C:\SQLInstall -Wait -Force -Verbose
```

Quand la configuration est appliquée, la sortie détaillée affiche des informations sur les événements actuels. Tant qu’aucune erreur n’est signalée (en rouge), si le message **Operation 'Invoke CimMethod' complete** s’affiche à l’écran, SQL Server doit être installé.

## <a name="validate-installation"></a>Valider l’installation

### <a name="dsc"></a>DSC

Les applets de commande [Test-DscConfiguration](/powershell/module/psdesiredstateconfiguration/test-dscconfiguration) peuvent déterminer si l’état actuel du serveur correspond à l’état souhaité. Dans ce cas, il s’agit de l’installation SQL Server. Le résultat de **Test-DscConfiguration** doit être **True** :

```PowerShell
PS C:\> Test-DscConfiguration
True
```

### <a name="services"></a>Services

Les services proposés retournent à présent les services SQL Server :

```PowerShell
PS C:\> Get-Service -Name *SQL*
Status  Name           DisplayName
------  ----           -----------
Running MSSQLSERVER    SQL Server (MSSQLSERVER)
Stopped SQLBrowser     SQL Server Browser
Running SQLSERVERAGENT SQL Server Agent (MSSQLSERVER)
Running SQLTELEMETRY   SQL Server CEIP service (MSSQLSERVER)
Running SQLWriter      SQL Server VSS Writer
```

### <a name="sql-server"></a>SQL Server

```PowerShell
PS C:\> & sqlcmd -S $env:COMPUTERNAME
1> SELECT @@SERVERNAME
2> GO
1> quit
```

## <a name="see-also"></a>Voir aussi

[Vue d’ensemble de la configuration d’état souhaité Windows PowerShell](/powershell/scripting/dsc/overview/overview)

[Installer SQL Server à partir de l’invite de commandes](../../database-engine/install-windows/install-sql-server-from-the-command-prompt.md)

[Installer SQL Server à l’aide d’un fichier de configuration](../../database-engine/install-windows/install-sql-server-using-a-configuration-file.md)
