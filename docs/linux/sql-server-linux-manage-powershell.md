---
title: Gérer SQL Server sur Linux avec PowerShell
description: Découvrez plus en détail SQL Server PowerShell et quelques exemples de son utilisation dans Windows avec SQL Server sur Linux.
author: VanMSFT
ms.author: vanto
ms.date: 10/02/2017
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: a3492ce1-5d55-4505-983c-d6da8d1a94ad
ms.openlocfilehash: d10b498569bac6a166ded3eaf9bd03f23ea6b718
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345632"
---
# <a name="use-powershell-on-windows-to-manage-sql-server-on-linux"></a>Utiliser PowerShell sur Windows pour gérer SQL Server sur Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Cet article présente [SQL Server PowerShell](../powershell/sql-server-powershell.md) et vous guide à travers quelques exemples de son utilisation avec SQL Server sur Linux. La prise en charge de PowerShell pour SQL Server est actuellement disponible sur Windows, MacOS et Linux. Cet article vous guide tout au long de l’utilisation d’une machine Windows pour se connecter à une instance distante sur Linux.

## <a name="install-the-newest-version-of-sql-powershell-on-windows"></a>Installer la version la plus récente de SQL PowerShell sur Windows

[SQL PowerShell](../powershell/download-sql-server-ps-module.md) sur Windows est conservé dans le PowerShell Gallery. Lorsque vous utilisez SQL Server, vous devez toujours utiliser la version la plus récente du module PowerShell SqlServer.

## <a name="before-you-begin"></a>Avant de commencer

Lisez les [Problèmes connus](sql-server-linux-release-notes.md) pour SQL Server sur Linux.

## <a name="launch-powershell-and-import-the-sqlserver-module"></a>Lancer PowerShell et importer le module *sqlserver*

Commençons par lancer PowerShell sur Windows. Utilisez <kbd>Win</kbd>+<kbd>R</kbd>, sur votre ordinateur Windows et saisissez **PowerShell** pour lancer une nouvelle session PowerShell Windows.

```
PowerShell
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

Nous allons utiliser PowerShell sur Windows pour vous connecter à votre instance sur Linux et afficher plusieurs propriétés du serveur.

Copiez et collez les commandes suivantes à l’invite de PowerShell. Quand vous exécutez ces commandes, PowerShell :
- afficher une boîte de dialogue qui vous invite à entrer le nom d’hôte ou l’adresse IP de votre instance
- affichez la boîte de dialogue *Requête d’informations d’identification Windows PowerShell*, qui vous invite à entrer les informations d’identification. Vous pouvez utiliser votre *nom d'utilisateur SQL* et votre *mot de passe SQL* pour vous connecter à votre instance sur Linux
- Utilisez le cmdlet **Get-SqlInstance** pour vous connecter au **Serveur** et afficher quelques propriétés

Si vous le souhaitez, vous pouvez simplement remplacer la variable `$serverInstance` par l’adresse IP ou le nom d’hôte de votre instance.

```powershell
# Prompt for instance & credentials to login into SQL Server
$serverInstance = Read-Host "Enter the name of your instance"
$credential = Get-Credential

# Connect to the Server and get a few properties
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

Voici à quoi peut ressembler le résultat.  Vous pouvez remarquer que le résultat est similaire à ce que SSMS affiche dans le nœud Bases de données.  Il affiche les bases de données utilisateur, mais pas les bases de données système.

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

Si vous avez besoin d’afficher toutes les bases de données sur votre instance, l’une des options consiste à utiliser la cmdlet [Get-SqlDatabase](/powershell/module/sqlserver/Get-SqlDatabase).

## <a name="examine-sql-server-error-logs"></a>Examiner les journaux d'erreurs SQL Server

Les étapes suivantes utilisent PowerShell sur Windows pour examiner les journaux d’erreurs se connecter à votre instance sur Linux. Nous utiliserons également la cmdlet **Out-GridView** pour afficher les informations des journaux d’erreurs dans un affichage de grille.

Copiez et collez les commandes suivantes à l’invite de PowerShell. L’exécution peut prendre quelques minutes. Ces commandes effectuent les opérations suivantes :
- afficher une boîte de dialogue qui vous invite à entrer le nom d’hôte ou l’adresse IP de votre instance
- affichez la boîte de dialogue *Requête d’informations d’identification Windows PowerShell*, qui vous invite à entrer les informations d’identification. Vous pouvez utiliser votre *nom d'utilisateur SQL* et votre *mot de passe SQL* pour vous connecter à votre instance sur Linux
- Utilisez la cmdlet **SqlErrorLog** pour vous connecter à l’instance sur Linux et récupérer les journaux d’erreurs depuis **Hier**
- Diriger la sortie vers la cmdlet **Out-GridView**

Si vous le souhaitez, vous pouvez remplacer la variable `$serverInstance` par l’adresse IP ou le nom d’hôte de votre instance.

```powershell
# Prompt for instance & credentials to login into SQL Server
$serverInstance = Read-Host "Enter the name of your instance"
$credential = Get-Credential

# Retrieve error logs since yesterday
Get-SqlErrorLog -ServerInstance $serverInstance -Credential $credential -Since Yesterday | Out-GridView
# done
```
## <a name="see-also"></a>Voir aussi
- [SQL Server PowerShell](../powershell/sql-server-powershell.md)
- [Applets de commande SqlServer](/powershell/module/sqlserver)