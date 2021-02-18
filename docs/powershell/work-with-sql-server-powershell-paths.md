---
title: Utiliser des chemins PowerShell SQL Server | Microsoft Docs
description: Découvrez comment manipuler et récupérer des informations à l’aide des cmdlets ou des méthodes et propriétés de l’objet identifié par le chemin d’accès du fournisseur.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.technology: sql-server-powershell
ms.topic: conceptual
ms.assetid: f31d8e2c-8d59-4fee-ac2a-324668e54262
author: markingmyname
ms.author: maghan
ms.reviewer: matteot, drskwier
ms.openlocfilehash: 19e50c5c389e5fb29316358940501bf7791bd457
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350127"
---
# <a name="work-with-sql-server-powershell-paths"></a>Utiliser des chemins d'accès PowerShell SQL Server

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Après avoir accédé à un nœud dans un chemin d'accès de fournisseur du [!INCLUDE[ssDE](../includes/ssde-md.md)] , vous pouvez effectuer des opérations ou récupérer des informations à l'aide des méthodes et propriétés de l'objet de gestion du [!INCLUDE[ssDE](../includes/ssde-md.md)] associé au nœud.  

[!INCLUDE [sql-server-powershell-version](../includes/sql-server-powershell-version.md)]

Après avoir accédé à un nœud dans un chemin de fournisseur du [!INCLUDE[ssDE](../includes/ssde-md.md)], vous pouvez effectuer deux types d’actions :  
  
-   Vous pouvez exécuter des applets de commande Windows PowerShell qui s’appliquent à des nœuds, telles que **Rename-Item**.  
  
-   Vous pouvez appeler les méthodes du modèle objet SMO ( [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Management Objects) associé, tel que SMO. Par exemple, si vous accédez au nœud Databases dans un chemin d’accès, vous pouvez utiliser les méthodes et propriétés de la classe <xref:Microsoft.SqlServer.Management.Smo.Database> .  
  
 Le fournisseur [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] est utilisé pour gérer les objets dans une instance du [!INCLUDE[ssDE](../includes/ssde-md.md)]. Il n'est pas utilisé pour travailler avec les données de bases de données. Si vous avez accédé à une table ou une vue, vous ne pouvez pas utiliser le fournisseur pour sélectionner, insérer, mettre à jour ou supprimer des données. Utilisez l’applet de commande **Invoke-Sqlcmd** pour interroger ou modifier des données dans des tables et des vues à partir de l’environnement Windows PowerShell. Pour plus d’informations, consultez [Invoke-Sqlcmd (applet de commande)](/powershell/module/sqlserver/invoke-sqlcmd).  
  
##  <a name="listing-methods-and-properties"></a><a name="ListPropMeth"></a> Affichage de la liste des méthodes et des propriétés  
 **Affichage de la liste des méthodes et des propriétés**  
  
 Pour afficher les méthodes et propriétés disponibles pour des objets ou classes d’objets spécifiques, utilisez l’applet de commande **Get-Member** .  
  
### <a name="examples-listing-methods-and-properties"></a>Exemples : Affichage de la liste des méthodes et des propriétés  
 Cet exemple affecte à une variable Windows PowerShell la classe <xref:Microsoft.SqlServer.Management.Smo.Database> SMO et répertorie les méthodes et les propriétés :  
  
```  
$MyDBVar = New-Object Microsoft.SqlServer.Management.SMO.Database  
$MyDBVar | Get-Member -Type Methods  
$MyDBVar | Get-Member -Type Properties  
```  
  
 Vous pouvez également utiliser **Get-Member** pour répertorier les méthodes et les propriétés associées au nœud de fin d’un chemin d’accès Windows PowerShell.  
  
 L'exemple suivant accède au nœud Databases d'un chemin d'accès SQLSERVER: et répertorie les propriétés de collection :  
  
```  
Set-Location SQLSERVER:\SQL\localhost\DEFAULT\Databases  
Get-Item . | Get-Member -Type Properties  
```  
  
 Cet exemple accède au nœud AdventureWorks2012 d'un chemin d'accès SQLSERVER: et répertorie les propriétés d'objet :  
  
```  
Set-Location SQLSERVER:\SQL\localhost\DEFAULT\Databases\AdventureWorks2012  
Get-Item . | Get-Member -Type Properties  
```  
  
##  <a name="using-methods-and-properties"></a><a name="UsePropMeth"></a> Utilisation des méthodes et des propriétés  
 **Utilisation des méthodes et des propriétés SMO**  
  
 Pour effectuer un travail sur les objets d'un chemin d'accès de fournisseur du [!INCLUDE[ssDE](../includes/ssde-md.md)] , vous pouvez utiliser les méthodes et les propriétés SMO.  
  
### <a name="examples-using-methods-and-properties"></a>Exemples : Utilisation des méthodes et des propriétés  
 L’exemple suivant utilise la propriété SMO **Schema** pour obtenir la liste des tables du schéma Sales dans AdventureWorks2012 :  
  
```  
Set-Location SQLSERVER:\SQL\localhost\DEFAULT\Databases\AdventureWorks2012\Tables  
Get-ChildItem | where {$_.Schema -eq "Sales"}  
```  
  
 L’exemple suivant utilise la méthode SMO **Script** pour générer un script qui contient les instructions **CREATE VIEW** que vous devez avoir pour recréer les affichages dans AdventureWorks2012 :  
  
```  
Remove-Item C:\PowerShell\CreateViews.sql  
Set-Location SQLSERVER:\SQL\localhost\DEFAULT\Databases\AdventureWorks2012\Views  
foreach ($Item in Get-ChildItem) { $Item.Script() | Out-File -Filepath C:\PowerShell\CreateViews.sql -append }  
```  
  
 L'exemple suivant utilise la méthode SMO **Create** pour créer une base de données, puis la propriété **State** pour indiquer si la base de données existe :  
  
```  
Set-Location SQLSERVER:\SQL\localhost\DEFAULT\Databases  
$MyDBVar = New-Object Microsoft.SqlServer.Management.SMO.Database  
$MyDBVar.Parent = (Get-Item ..)  
$MyDBVar.Name = "NewDB"  
$MyDBVar.Create()  
$MyDBVar.State  
```  
  
## <a name="see-also"></a>Voir aussi  
 [fournisseur PowerShell SQL Server](sql-server-powershell-provider.md)   
 [Parcourir les chemins d'accès PowerShell SQL Server](navigate-sql-server-powershell-paths.md)   
 [Convertir des URN en chemins d'accès de fournisseur SQL Server](/powershell/module/sqlserver/Convert-UrnToPath)   
 [SQL Server PowerShell](sql-server-powershell.md)  
  
