---
title: Gérer SQL Server sur Linux avec PowerShell Core
description: Découvrez en détail SQL Server PowerShell grâce à quelques exemples de son utilisation avec PowerShell Core (PS Core) sur macOS et Linux.
ms.date: 04/22/2019
ms.prod: sql
ms.technology: linux
ms.topic: conceptual
author: SQLvariant
ms.author: aanelson
ms.reviewer: vanto
ms.openlocfilehash: 8018a90b3483f9baf4b58a4b057fa93685c6d828
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100016324"
---
# <a name="manage-sql-server-on-linux-with-powershell-core"></a>Gérer SQL Server sur Linux avec PowerShell Core

Cet article présente [SQL Server PowerShell](../powershell/sql-server-powershell.md) et vous guide à travers quelques exemples de son utilisation avec PowerShell Core (PS Core) sur macOS et Linux. PowerShell Core est maintenant un projet Open Source sur [GitHub](https://github.com/powershell/powershell).

## <a name="cross-platform-editor-options"></a>Options de l'éditeur multiplateforme

Toutes les étapes PowerShell Core ci-dessous fonctionneront dans un terminal standard, mais vous pouvez les exécuter depuis un terminal dans VS Code ou Azure Data Studio.  VS Code et Azure Data Studio sont disponibles sur macOS et Linux.  Pour plus d'informations sur Azure Data Studio, voir [ce démarrage rapide](../azure-data-studio/quickstart-sql-server.md).  Vous pouvez également utiliser l'extension [PowerShell](../azure-data-studio/extensions/powershell-extension.md) pour cela.

## <a name="installing-powershell-core"></a>Installation de PowerShell Core

Pour plus d'informations sur l'installation de PowerShell Core sur diverses plateformes prises en charge et expérimentales, consultez les articles suivants :

- [Installation de PowerShell Core sur Windows](/powershell/scripting/install/installing-powershell-core-on-windows)
- [Installation de PowerShell Core sur Linux](/powershell/scripting/install/installing-powershell-core-on-linux)
- [Installation de PowerShell Core sur macOS](/powershell/scripting/install/installing-powershell-core-on-macos)
- [Installation de PowerShell Core sur ARM](/powershell/scripting/install/powershell-core-on-arm)

## <a name="install-the-sqlserver-module"></a>Installer le module SqlServer

Le module `SqlServer` est géré dans la galerie [PowerShell](https://www.powershellgallery.com/packages/SqlServer/). Lorsque vous utilisez SQL Server, vous devez toujours utiliser la version la plus récente du module PowerShell SqlServer.

Pour installer le module SqlServer, ouvrez une session PowerShell Core et exécutez le code suivant :

```powerhsell
Install-Module -Name SqlServer
```

Pour plus d'informations sur l'installation du module SqlServer à partir de la galerie PowerShell, voir cette [page](../powershell/download-sql-server-ps-module.md).

## <a name="using-the-sqlserver-module"></a>Utilisation du module SqlServer

Commençons par lancer PowerShell Core.  Si vous utilisez macOS ou Linux, ouvrez une *session du terminal* sur votre ordinateur, puis tapez **pwsh** pour lancer une nouvelle session PowerShell Core.  Sur Windows, utilise <kbd>Win</kbd>+<kbd>R</kbd> et tapez `pwsh` pour lancer une nouvelle session PowerShell Core.

```
pwsh
```

SQL Server fournit un module PowerShell nommé **SqlServer**. Vous pouvez utiliser le module **SqlServer** pour importer les composants de SQL Server (fournisseur et cmdlets SQL Server) dans un environnement ou un script PowerShell.

Copiez et collez la commande suivante à l’invite PowerShell pour importer le module **SqlServer** dans votre session PowerShell actuelle :

```powershell
Import-Module SqlServer
```

À l’invite de PowerShell, tapez la commande suivante pour vérifier que le module **SqlServer** a été correctement importé :

```powershell
Get-Module -Name SqlServer
```

PowerShell doit afficher des informations similaires à la sortie suivante :

```
ModuleType Version    Name          ExportedCommands
---------- -------    ----          ----------------
Script     21.1.18102 SqlServer     {Add-SqlAvailabilityDatabase, Add-SqlAvailabilityGroupList...
```

## <a name="connect-to-sql-server-and-get-server-information"></a>Se connecter à SQL Server et obtenir des informations sur le serveur

Les étapes suivantes utilisent PowerShell pour vous connecter à votre instance sur Linux et afficher plusieurs propriétés du serveur.

Copiez et collez les commandes suivantes à l’invite de PowerShell. Quand vous exécutez ces commandes, PowerShell :
- afficher une boîte de dialogue qui vous invite à entrer le nom d’hôte ou l’adresse IP de votre instance
- afficher la boîte de dialogue *requête d’informations d’identification PowerShell*, qui vous invite à entrer les informations d’identification. Vous pouvez utiliser votre *nom d'utilisateur SQL* et votre *mot de passe SQL* pour vous connecter à votre instance sur Linux
- Utilisez le cmdlet **Get-SqlInstance** pour vous connecter au **Serveur** et afficher quelques propriétés

Si vous le souhaitez, vous pouvez simplement remplacer la variable `$serverInstance` par l’adresse IP ou le nom d’hôte de votre instance.

```powershell
# Prompt for instance & credentials to login into SQL Server
$serverInstance = Read-Host "Enter the name of your instance"
$credential = Get-Credential

# Connect to the Server and return a few properties
Get-SqlInstance -ServerInstance $serverInstance -Credential $credential
# done
```

PowerShell doit afficher des informations similaires à la sortie suivante :

```
Instance Name                   Version    ProductLevel UpdateLevel  HostPlatform HostDistribution
-------------                   -------    ------------ -----------  ------------ ----------------
your_server_instance            14.0.3048  RTM          CU13         Linux        Ubuntu
```

> [!NOTE]
> Si rien ne s’affiche pour ces valeurs, la connexion à l’instance cible a probablement échoué. Assurez-vous que vous pouvez utiliser les mêmes informations de connexion pour vous connecter à partir de SQL Server Management Studio. Examinez ensuite les [recommandations en matière de résolution des problèmes de connexion](sql-server-linux-troubleshooting-guide.md#connection).

## <a name="using-the-sql-server-powershell-provider"></a>Utilisation du fournisseur SQL Server PowerShell

Une autre option pour se connecter à votre instance consiste à utiliser le [Fournisseur SQL Server PowerShell](../powershell/sql-server-powershell-provider.md).  Ce fournisseur vous permet de naviguer dans l’instance comme si vous naviguiez dans l’arborescence dans l’Explorateur d’objets, mais a niveau de la cmdline.  Par défaut, ce fournisseur est présenté sous la forme d’un PSDrive nommé `SQLSERVER:\` que vous pouvez utiliser pour vous connecter et naviguer dans les instances auxquelles votre compte de domaine a accès.  Consultez [Étapes de configuration](./sql-server-linux-active-directory-auth-overview.md#configuration-steps) pour plus d’informations sur le mode de configuration de l’authentification Active Directory pour SQL Server sur Linux.

Vous pouvez également utiliser l’authentification SQL avec le fournisseur SQL Server PowerShell. Pour ce faire, utilisez la cmdlet `New-PSDrive` pour créer un nouveau PSDrive et fournir les informations d’identification appropriées pour la connexion.

Dans l’exemple ci-dessous, vous verrez un exemple de création d’un nouveau PSDrive à l’aide de l’authentification SQL.

```powershell
# NOTE: We are reusing the values saved in the $credential variable from the above example.

New-PSDrive -Name SQLonDocker -PSProvider SqlServer -Root 'SQLSERVER:\SQL\localhost,10002\Default\' -Credential $credential
```

Vous pouvez vérifier que le lecteur a été créé en exécutant la cmdlet `Get-PSDrive`.

```powershell
Get-PSDrive
```

Une fois que vous avez créé votre nouveau PSDrive, vous pouvez commencer à le parcourir.

```powershell
dir SQLonDocker:\Databases
```

Voici à quoi peut ressembler le résultat.  Vous pouvez remarquer que ce résultat est similaire à ce que SSMS affiche dans le nœud Bases de données.  Il affiche les bases de données utilisateur, mais pas les bases de données système.

```powershell
Name                 Status           Size     Space  Recovery Compat. Owner
                                            Available  Model     Level
----                 ------           ---- ---------- -------- ------- -----
AdventureWorks2016   Normal      209.63 MB    1.31 MB Simple       130 sa
AdventureWorksDW2012 Normal      167.00 MB   32.47 MB Simple       110 sa
AdventureWorksDW2014 Normal      188.00 MB   78.10 MB Simple       120 sa
AdventureWorksDW2016 Normal      172.00 MB   74.76 MB Simple       130 sa
AdventureWorksDW2017 Normal      208.00 MB   40.57 MB Simple       140 sa
```

Si vous avez besoin d’afficher toutes les bases de données sur votre instance, l’une des options consiste à utiliser la cmdlet `Get-SqlDatabase`.

## <a name="get-databases"></a>Obtenir des bases de données

Une cmdlet importante à connaître est `Get-SqlDatabase`.  Vous pouvez utiliser la cmdlet `Get-SqlDatabase` dans de nombreuses opérations impliquant une base de données ou des objets d’une base de données.  Si vous fournissez des valeurs pour les paramètres `-ServerInstance` et `-Database`, un seul objet de base de données sera récupéré.  Cependant, si vous spécifiez uniquement le paramètre `-ServerInstance`, une liste complète de toutes les bases de données de cette instance sera retournée.

```powershell
# NOTE: We are reusing the values saved in the $credential variable from the above example.

# Connect to the Instance and retrieve all databases
Get-SqlDatabase -ServerInstance ServerB -Credential $credential
```

Voici un exemple de résultat obtenu avec la commande Get-SqlDatabase ci-dessus :

```powershell
Name                 Status           Size     Space  Recovery Compat. Owner
                                            Available  Model     Level
----                 ------           ---- ---------- -------- ------- -----
AdventureWorks2016   Normal      209.63 MB    1.31 MB Simple       130 sa
AdventureWorksDW2012 Normal      167.00 MB   32.47 MB Simple       110 sa
AdventureWorksDW2014 Normal      188.00 MB   78.10 MB Simple       120 sa
AdventureWorksDW2016 Normal      172.00 MB   74.88 MB Simple       130 sa
AdventureWorksDW2017 Normal      208.00 MB   40.63 MB Simple       140 sa
master               Normal        6.00 MB  600.00 KB Simple       140 sa
model                Normal       16.00 MB    5.70 MB Full         140 sa
msdb                 Normal       15.50 MB    1.14 MB Simple       140 sa
tempdb               Normal       16.00 MB    5.49 MB Simple       140 sa

```

## <a name="examine-sql-server-error-logs"></a>Examiner les journaux d'erreurs SQL Server

Les étapes suivantes utilisent PowerShell Core pour examiner les journaux d’erreurs se connecter à votre instance sur Linux.

Copiez et collez les commandes suivantes à l’invite de PowerShell. L’exécution peut prendre quelques minutes. Ces commandes effectuent les opérations suivantes :
- afficher une boîte de dialogue qui vous invite à entrer le nom d’hôte ou l’adresse IP de votre instance
- afficher la boîte de dialogue *requête d’informations d’identification PowerShell*, qui vous invite à entrer les informations d’identification. Vous pouvez utiliser votre *nom d'utilisateur SQL* et votre *mot de passe SQL* pour vous connecter à votre instance sur Linux
- Utilisez la cmdlet **SqlErrorLog** pour vous connecter à l’instance sur Linux et récupérer les journaux d’erreurs depuis **Hier**

Si vous le souhaitez, vous pouvez remplacer la variable `$serverInstance` par l’adresse IP ou le nom d’hôte de votre instance.

```powershell
# Prompt for instance & credentials to login into SQL Server
$serverInstance = Read-Host "Enter the name of your instance"
$credential = Get-Credential

# Retrieve error logs since yesterday
Get-SqlErrorLog -ServerInstance $serverInstance -Credential $credential -Since Yesterday
# done
```

## <a name="explore-cmdlets-currently-available-in-ps-core"></a>Explorer les cmdlets actuellement disponibles dans PS Core
Même si le module SqlServer propose actuellement 109 applets de commande sur Windows PowerShell, seules 62 d’entre elles sont disponibles sur PSCore. Vous trouverez ci-dessous une liste complète des 62 applets de commande actuellement disponibles.  Pour une documentation détaillée de tous les cmdlets du module SqlServer, voir la [référence cmdlet](/powershell/module/sqlserver/) SqlServer.

La commande suivante vous montrera tous les cmdlets disponibles sur la version PowerShell que vous utilisez.

```powershell
Get-Command -Module SqlServer -CommandType Cmdlet |
Sort-Object -Property Noun |
SELECT Name
```

- ConvertFrom-EncodedSqlName
- ConvertTo-EncodedSqlName
- Get-SqlAgent
- Get-SqlAgentJob
- Get-SqlAgentJobHistory
- Get-SqlAgentJobSchedule
- Get-SqlAgentJobStep
- Get-SqlAgentSchedule
- Invoke-SqlAssessment
- Get-SqlAssessmentItem
- Remove-SqlAvailabilityDatabase
- Resume-SqlAvailabilityDatabase
- Add-SqlAvailabilityDatabase
- Suspend-SqlAvailabilityDatabase
- New-SqlAvailabilityGroup
- Set-SqlAvailabilityGroup
- Remove-SqlAvailabilityGroup
- Switch-SqlAvailabilityGroup
- Join-SqlAvailabilityGroup
- Revoke-SqlAvailabilityGroupCreateAnyDatabase
- Grant-SqlAvailabilityGroupCreateAnyDatabase
- New-SqlAvailabilityGroupListener
- Set-SqlAvailabilityGroupListener
- Add-SqlAvailabilityGroupListenerStaticIp
- Set-SqlAvailabilityReplica
- Remove-SqlAvailabilityReplica
- New-SqlAvailabilityReplica
- Set-SqlAvailabilityReplicaRoleToSecondary
- New-SqlBackupEncryptionOption
- Get-SqlBackupHistory
- Invoke-Sqlcmd
- New-SqlCngColumnMasterKeySettings
- Remove-SqlColumnEncryptionKey
- Get-SqlColumnEncryptionKey
- Remove-SqlColumnEncryptionKeyValue
- Add-SqlColumnEncryptionKeyValue
- Get-SqlColumnMasterKey
- Remove-SqlColumnMasterKey
- New-SqlColumnMasterKey
- Get-SqlCredential
- Set-SqlCredential
- New-SqlCredential
- Remove-SqlCredential
- New-SqlCspColumnMasterKeySettings
- Get-SqlDatabase
- Restore-SqlDatabase
- Backup-SqlDatabase
- Set-SqlErrorLog
- Get-SqlErrorLog
- New-SqlHADREndpoint
- Set-SqlHADREndpoint
- Get-SqlInstance
- Add-SqlLogin
- Remove-SqlLogin
- Get-SqlLogin
- Set-SqlSmartAdmin
- Get-SqlSmartAdmin
- Read-SqlTableData
- Write-SqlTableData
- Read-SqlViewData
- Read-SqlXEvent
- Convert-UrnToPath

## <a name="see-also"></a>Voir aussi
- [SQL Server PowerShell](../powershell/sql-server-powershell.md)