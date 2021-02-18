---
title: Charger les assemblys SMO dans Windows PowerShell
description: Découvrez comment charger les assemblys SMO (SQL Server Management Object) dans des scripts Windows PowerShell qui n’utilisent pas le fournisseur SQL Server PowerShell.
ms.prod: sql
ms.technology: sql-server-powershell
ms.topic: conceptual
ms.assetid: 8ca42b69-da5a-47f4-9085-34e443f0e389
author: markingmyname
ms.author: maghan
ms.reviewer: matteot, drskwier
ms.custom: ''
ms.date: 10/14/2020
ms.openlocfilehash: 7bfb0d68533e116c41a6ebb2277b3bae29199e13
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340799"
---
# <a name="load-the-smo-assemblies-in-windows-powershell"></a>Charger les assemblys SMO dans Windows PowerShell

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Cet article décrit comment charger les assemblys SMO (SQL Server Management Object) dans des scripts Windows PowerShell qui n’utilisent pas le fournisseur SQL Server PowerShell.  

[!INCLUDE [sql-server-powershell-version](../includes/sql-server-powershell-version.md)]

Le mécanisme recommandé pour le chargement des assemblys SMO consiste à charger le module **SqlServer**. Le fournisseur [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] inclus dans le module charge automatiquement les assemblys SMO et implémente des fonctionnalités qui étendent l'utilité des objets SMO dans les scripts PowerShell.

Il existe deux cas dans lesquels vous pouvez être amené à charger les assemblys SMO directement :  

- Votre script fait référence à un objet SMO avant la première commande faisant référence au fournisseur ou aux applets de commande des composants logiciels enfichables [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] .  

- Vous souhaitez porter du code SMO à partir d’un autre langage, par exemple C# ou Visual Basic, qui n’utilise pas le fournisseur ou les applets de commande.  

## <a name="example-loading-the-sql-server-management-objects"></a>Exemple : chargement d’objets SMO (SQL Server Management Object)

Le code suivant charge les assemblys SMO :  

```powershell
# Loads the SQL Server Management Objects (SMO)  

$ErrorActionPreference = "Stop"
  
$sqlpsreg="HKLM:\SOFTWARE\Microsoft\PowerShell\1\ShellIds\Microsoft.SqlServer.Management.PowerShell.sqlps"  
  
if (Get-ChildItem $sqlpsreg -ErrorAction "SilentlyContinue")  
{  
    throw "SQL Server Provider for Windows PowerShell is not installed."  
}  
else  
{  
    $item = Get-ItemProperty $sqlpsreg  
    $sqlpsPath = [System.IO.Path]::GetDirectoryName($item.Path)  
}  
  
$assemblylist =
"Microsoft.SqlServer.Management.Common",  
"Microsoft.SqlServer.Smo",  
"Microsoft.SqlServer.Dmf ",  
"Microsoft.SqlServer.Instapi ",  
"Microsoft.SqlServer.SqlWmiManagement ",  
"Microsoft.SqlServer.ConnectionInfo ",  
"Microsoft.SqlServer.SmoExtended ",  
"Microsoft.SqlServer.SqlTDiagM ",  
"Microsoft.SqlServer.SString ",  
"Microsoft.SqlServer.Management.RegisteredServers ",  
"Microsoft.SqlServer.Management.Sdk.Sfc ",  
"Microsoft.SqlServer.SqlEnum ",  
"Microsoft.SqlServer.RegSvrEnum ",  
"Microsoft.SqlServer.WmiEnum ",  
"Microsoft.SqlServer.ServiceBrokerEnum ",  
"Microsoft.SqlServer.ConnectionInfoExtended ",  
"Microsoft.SqlServer.Management.Collector ",  
"Microsoft.SqlServer.Management.CollectorEnum",  
"Microsoft.SqlServer.Management.Dac",  
"Microsoft.SqlServer.Management.DacEnum",  
"Microsoft.SqlServer.Management.Utility"  
  
foreach ($asm in $assemblylist)  
{  
    $asm = [Reflection.Assembly]::LoadWithPartialName($asm)  
}  
  
Push-Location  
cd $sqlpsPath  
update-FormatData -prependpath SQLProvider.Format.ps1xml
Pop-Location  
```

## <a name="see-also"></a>Voir aussi

- [SQL Server PowerShell](sql-server-powershell.md)