---
title: Gérer l’authentification pour SQL Server dans PowerShell
description: Découvrez comment utiliser l’authentification SQL Server plutôt que l’authentification Windows (valeur par défaut) lors de la connexion à une instance du Moteur de base de données.
titleSuffix: SQL Server on Linux
ms.prod: sql
ms.technology: sql-server-powershell
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: matteot, drskwier
ms.custom: seo-lt-2019
ms.date: 10/14/2020
ms.openlocfilehash: 233c456a2e4c7f41efd6038efccc6b00b68eb72d
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/04/2021
ms.locfileid: "101838057"
---
# <a name="manage-authentication-to-sql-server-in-powershell"></a>Gérer l’authentification pour SQL Server dans PowerShell

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Par défaut, les composants de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] PowerShell utilisent l'authentification Windows lors de la connexion à une instance du [!INCLUDE[ssDE](../includes/ssde-md.md)]. Vous pouvez utiliser l’authentification SQL Server en définissant une unité virtuelle PowerShell ou en spécifiant les paramètres **-Username** et **-Password** pour **Invoke-Sqlcmd**.

[!INCLUDE [sql-server-powershell-version](../includes/sql-server-powershell-version.md)]

## <a name="permissions"></a>Autorisations

Toutes les actions que vous pouvez effectuer dans une instance du [!INCLUDE[ssDE](../includes/ssde-md.md)] sont contrôlées par les autorisations accordées aux informations d'identification utilisées pour la connexion à l'instance. Par défaut, le fournisseur et les applets de commande [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] utilisent le compte Windows sous lequel ils s'exécutent pour établir une connexion via l'authentification Windows au [!INCLUDE[ssDE](../includes/ssde-md.md)].  

Pour établir une connexion via l'authentification [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] , vous devez fournir un ID de connexion et un mot de passe d'authentification SQL Server. Quand vous utilisez le fournisseur [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] , vous devez associer les informations d’identification de connexion [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] à un lecteur virtuel, puis exécuter la commande de changement de répertoire (**cd**) pour passer à ce lecteur. Dans Windows PowerShell, les informations d'identification de sécurité peuvent être associées uniquement à des lecteurs virtuels.  

## <a name="sql-server-authentication-using-a-virtual-drive"></a>Authentification SQL Server avec un lecteur virtuel

### <a name="to-create-a-virtual-drive-associated-with-a-sql-server-authentication-login"></a>Pour créer un lecteur virtuel associé à une connexion via l'authentification SQL Server

1. Créez une fonction qui :

    1. Possède des paramètres pour le nom indiquant le lecteur virtuel, l'ID de connexion et le chemin d'accès du fournisseur à associer au lecteur virtuel.

    2. Utilise **read-host** pour inviter l’utilisateur à fournir le mot de passe.  

    3. Utilise **new-object** pour créer un objet d’informations d’identification.  

    4. Utilise **new-psdrive** pour créer un lecteur virtuel avec les informations d’identification fournies.  

2. Appelez la fonction pour créer un lecteur virtuel avec les informations d'identification fournies.  

#### <a name="example-virtual-drive"></a>Exemple (lecteur virtuel)

Cet exemple crée une fonction nommée **sqldrive** que vous pouvez utiliser pour créer un lecteur virtuel associé à la connexion via l'authentification [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] et à l'instance spécifiées.  
  
 La fonction **sqldrive** vous invite à entrer le mot de passe de votre connexion, en masquant celui-ci à mesure que vous le tapez. Ensuite, chaque fois que vous exécutez la commande de changement de répertoire (**cd**) pour vous connecter à un chemin via le nom de lecteur virtuel, toutes les opérations sont effectuées en utilisant les informations d’identification de connexion de l’authentification [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] que vous avez fournies lors de la création du lecteur.  
  
```powershell
## Create a function that specifies the login and prompts for the password.  
  
function sqldrive  
{  
    param( [string]$name, [string]$login = "MyLogin", [string]$root = "SQLSERVER:\SQL\MyComputer\MyInstance" )  
    $pwd = read-host -AsSecureString -Prompt "Password"  
    $cred = new-object System.Management.Automation.PSCredential -argumentlist $login,$pwd  
    New-PSDrive $name -PSProvider SqlServer -Root $root -Credential $cred -Scope 1  
}  
  
## Use the sqldrive function to create a SQLAuth virtual drive.  
sqldrive SQLAuth
  
## Set-Location to the virtual drive, which invokes the supplied authentication credentials.  
sl SQLAuth:
```

## <a name="sql-server-authentication-using-invoke-sqlcmd"></a>Authentification SQL Server avec Invoke-Sqlcmd

### <a name="to-use-invoke-sqlcmd-with-sql-server-authentication"></a>Pour utiliser Invoke-Sqlcmd avec l'authentification SQL Server

1. Utilisez le paramètre **-Username** pour spécifier un ID de connexion, et le paramètre **-Password** pour spécifier le mot de passe associé.  

#### <a name="example-invoke-sqlcmd"></a>Exemple (Invoke-Sqlcmd)

Cet exemple utilise l'applet de commande read-host pour inviter l'utilisateur à entrer un mot de passe, puis se connecte via l'authentification SQL Server.  

```powershell
## Prompt the user for their password.  
$pwd = read-host -AsSecureString -Prompt "Password"  
  
Invoke-Sqlcmd -Query "SELECT GETDATE() AS TimeOfQuery;" -ServerInstance "MyComputer\MyInstance" -Username "MyLogin" -Password $pwd  
```

## <a name="see-also"></a>Voir aussi

- [SQL Server PowerShell](sql-server-powershell.md)
- [Fournisseur SQL Server PowerShell](sql-server-powershell-provider.md)
- [Invoke-Sqlcmd, applet de commande](/powershell/module/sqlserver/invoke-sqlcmd)
- [Utiliser PowerShell avec Azure Data Studio](../azure-data-studio/extensions/powershell-extension.md)
