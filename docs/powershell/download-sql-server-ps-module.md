---
title: Télécharger le module PowerShell SQL Server
description: Découvrez comment installer le module SqlServer PowerShell, qui fournit des cmdlets qui prennent en charge les fonctionnalités SQL les plus récentes et contient également des versions mises à jour des cmdlets dans le module SQLPS.
ms.prod: sql
ms.technology: sql-server-powershell
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: matteot, drskwier
ms.custom: ''
ms.date: 10/14/2020
ms.openlocfilehash: 960f146d7cd6943d27a0ecce2c1edaedc41de472
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102897"
---
# <a name="install-the-sql-server-powershell-module"></a>Installer le module SQL Server PowerShell

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Cet article fournit des instructions pour installer le module PowerShell **SqlServer**.

## <a name="powershell-modules-for-sql-server"></a>Modules PowerShell pour SQL Server

Il existe deux modules SQL Server PowerShell :

- **SqlServer** : Le module SqlServer inclut de nouvelles applets de commande pour prendre en charge les dernières fonctionnalités SQL. Le module contient également des versions mises à jour des applets de commande dans **SQLPS**. Pour télécharger le module SqlServer, accédez à [Module SqlServer dans PowerShell Gallery](https://www.powershellgallery.com/packages/Sqlserver).

- **SQLPS** : Le SQLPS est le module utilisé par [SQL Agent](sql-server-powershell.md#sql-server-agent) pour exécuter les travaux de l’agent dans les étapes de travail de l’agent à l’aide du sous-système PowerShell.

> [!NOTE]
> Les versions du module **SqlServer** dans PowerShell Gallery prennent en charge la gestion des versions et nécessitent PowerShell version 5.0 ou ultérieure.

Pour consulter les rubriques d’aide, accédez à :

- Applets de commande [SqlServer](/powershell/module/sqlserver).
- Applets de commande [SQLPS](/powershell/module/sqlps).

## <a name="sql-server-management-studio"></a>SQL Server Management Studio

[SQL Server Management Studio (SSMS)](../ssms/download-sql-server-management-studio-ssms.md) n’installe aucun module PowerShell. Pour utiliser PowerShell avec SSMS, installez le module **SqlServer** à partir de [PowerShell Gallery](https://www.powershellgallery.com/packages/Sqlserver).

> [!NOTE]
> Avec SSMS 16.x, une version antérieure du module **SqlServer** est incluse avec SQL Server Management Studio (SSMS)

## <a name="azure-data-studio"></a>Azure Data Studio

[Azure Data Studio](../azure-data-studio/download-azure-data-studio.md) n’installe aucun module PowerShell. Pour utiliser PowerShell avec Azure Data Studio, installez le module **SqlServer** à partir de [PowerShell Gallery](https://www.powershellgallery.com/packages/Sqlserver).

Vous pouvez utiliser l’[extension PowerShell](../azure-data-studio/extensions/powershell-extension.md), qui fournit une prise en charge complète de l’éditeur PowerShell dans Azure Data Studio.

## <a name="installing-or-updating-the-sqlserver-module"></a>Installation ou mise à jour du module SqlServer

Pour installer le module **SqlServer** à partir de PowerShell Gallery, démarrez une session [PowerShell](/powershell/scripting/overview) en tant qu’administrateur. Vous pouvez également démarrer Azure Data Studio en tant qu’administrateur et exécutez ces commandes dans une session PowerShell du terminal intégré.

Vous pouvez également utiliser *Installer-Module SQLServer-Étendue CurrentUser* pour exécuter des autorisations élevées. Ce cmdlet est utile pour les utilisateurs qui ne sont pas administrateurs dans leur environnement. Toutefois, étant donné que l’étendue est limitée à l’utilisateur actuel, les autres utilisateurs sur le même ordinateur ne peuvent pas utiliser le module.

### <a name="install-the-sqlserver-module"></a>Installer le module SqlServer

Exécutez la commande suivante dans votre session PowerShell afin d’installer le module SqlServer pour tous les utilisateurs :

```powershell
Install-Module -Name SqlServer
```

### <a name="to-view-the-versions-of-the-sqlserver-module-installed"></a>Pour afficher les versions du module SqlServer installées :

Exécutez la commande suivante pour afficher les versions du module SqlServer qui ont été installées

```powershell
Get-Module SqlServer -ListAvailable
```

### <a name="install-for-the-current-user-rather-than-as-an-administrator"></a>Effectuez l’installation pour l’utilisateur actuel plutôt que comme administrateur

Si vous ne pouvez pas exécuter la session PowerShell en tant qu’administrateur, effectuez l’installation pour l’utilisateur actuel à l’aide de la commande suivante :

```powershell
Install-Module -Name SqlServer -Scope CurrentUser
```

### <a name="to-overwrite-a-previous-version-of-the-sqlserver-module"></a>Pour remplacer une version précédente du module SqlServer

Vous pouvez également utiliser la commande `Install-Module` pour remplacer une version précédente.

```powershell
Install-Module -Name SqlServer -AllowClobber
```

> [!Note]
> PowerShell utilise toujours le dernier module installé.

### <a name="update-the-installed-version-of-the-sqlserver-module"></a>Mettre à jour la version installée du module SqlServer

Quand des versions mises à jour du module **SqlServer** sont disponibles, vous pouvez installer la dernière version à l’aide de la commande suivante :

```powershell
Install-Module -Name SqlServer -AllowClobber
```

Vous pouvez utiliser la commande `Update-Module` pour installer la version la plus récente du module SQL Server PowerShell, mais cela ne supprime pas les versions antérieures. Cette commande installe la version la plus récente côte à côte pour vous permettre de tester la version la plus récente en conservant les anciens modules installés.

Toutefois, si vous ne souhaitez pas conserver les anciennes versions du module, vous pouvez utiliser la commande `Uninstall-Module` pour supprimer les versions précédentes.

Vous pouvez utiliser la commande suivante pour répertorier les différentes versions installées :

```powershell
Get-Module SqlServer -ListAvailable
```

Vous pouvez utiliser la commande suivante pour supprimer les anciennes versions :

```powershell
Uninstall-module -Name SQLServer -RequiredVersion "<version number>" -AllowClobber
```

### <a name="troubleshooting"></a>Dépannage

Si vous rencontrez des problèmes d’installation, consultez la [documentation sur Install-Module](https://www.powershellgallery.com/packages/PowerShellGet/2.2.1) et les [informations de référence sur Install-Module](/powershell/module/powershellget/Install-Module).

## <a name="using-a-specific-version-of-the-sqlserver-module"></a>Utilisation d’une version spécifique du module SqlServer

Pour utiliser une version spécifique du module, importez-la avec un numéro de version spécifique comme dans la commande suivante :

```powershell
Import-Module SqlServer -Version 21.1.18218
```

## <a name="pre-release-versions-of-the-sqlserver-module"></a>Versions préliminaires du module SqlServer

Les versions préliminaires (ou « préversions ») du module SqlServer peuvent être disponibles dans PowerShell Gallery.

> [!IMPORTANT]
> Vous pouvez découvrir et installer ces versions à l’aide des applets de commande *Find-Module* et *Install-Module* mises à jour (qui font partie du module [PowerShellGet](https://www.powershellgallery.com/packages/PowerShellGet)) en passant le commutateur *- AllowPrerelease*. Pour utiliser ces applets de commande, installez le module PowerShellGet, puis ouvrez une nouvelle session.

### <a name="to-discover-pre-release-versions-of-the-sqlserver-module"></a>Pour découvrir les versions préliminaires du module SqlServer

Pour découvrir les versions préliminaires (préversions) du module SqlServer, exécutez la commande suivante :

```powershell
Find-Module SqlServer -AllowPrerelease
```

### <a name="to-install-a-specific-pre-release-version-of-the-sqlserver-module"></a>Pour installer une version préliminaire spécifique du module SqlServer

Pour installer une version préliminaire spécifique du module, installez-la avec un numéro de version spécifique.

Vous pouvez essayer d’utiliser la commande suivante :

```powershell
Install-Module SqlServer -RequiredVersion 21.1.18218-preview -AllowPrerelease
```

## <a name="sql-server-powershell-on-linux"></a>SQL Server PowerShell sur Linux

Visitez [Gérer SQL Server sur Linux avec PowerShell Core](../linux/sql-server-linux-manage-powershell-core.md) pour savoir comment installer SQL Server PowerShell sur Linux.

## <a name="other-modules"></a>Autres modules

- [Az.Sql](https://www.powershellgallery.com/packages/Az.Sql/) : cmdlets de service SQL pour Azure Resource Manager dans Windows PowerShell et PowerShell Core.

- [SqlServerDsc](https://www.powershellgallery.com/packages/SqlServerDsc/) : module avec des ressources DSC pour le déploiement et la configuration de Microsoft SQL Server.

## <a name="cmdlet-reference"></a>Référence des applets de commande

- [Applets de commande SqlServer](/powershell/module/sqlserver)
- [Applets de commande SQLPS](/powershell/module/sqlps)

## <a name="next-steps"></a>Étapes suivantes

- [SQL Server PowerShell](sql-server-powershell.md)
- [Applets de commande SQL Server PowerShell](/powershell/module/sqlserver)
- [Utiliser PowerShell avec Azure Data Studio](../azure-data-studio/extensions/powershell-extension.md)