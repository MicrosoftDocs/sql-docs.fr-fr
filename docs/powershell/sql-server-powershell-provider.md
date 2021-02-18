---
title: Fournisseur PowerShell SQL Server | Microsoft Docs
description: En savoir plus sur le fournisseur de SQL Server pour Windows PowerShell, qui permet d’accéder aux objets SQL Server à l’aide de chemins d’accès similaires aux chemins d’accès du système de fichiers.
ms.prod: sql
ms.technology: sql-server-powershell
ms.topic: conceptual
helpviewer_keywords:
- PowerShell [SQL Server], provider
- PowerShell [SQL Server], SQL Server PowerShell Provider
- Providers [PowerShell]
- SMO [SQL Server], PowerShell
- PowerShell [SQL Server], SMO
- SQL Server Management Objects, PowerShell
ms.assetid: b97acc43-fcd2-4ae5-b218-e183bab916f9
author: markingmyname
ms.author: maghan
ms.reviewer: matteot, drskwier
ms.custom: ''
ms.date: 07/31/2019
ms.openlocfilehash: 5e62ed5e7482ca6fc1d80eb6e7f19ca1e0da4e7f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350159"
---
# <a name="sql-server-powershell-provider"></a>SQL Server PowerShell Provider

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Le fournisseur [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] pour Windows PowerShell présente la hiérarchie des objets [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] dans des chemins d'accès semblables aux chemins d'accès de système de fichiers. Vous pouvez utiliser les chemins pour localiser un objet, puis utiliser des méthodes des modèles SMO [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] pour effectuer des actions sur les objets.  

[!INCLUDE [sql-server-powershell-version](../includes/sql-server-powershell-version.md)]

## <a name="benefits-of-the-sql-server-powershell-provider"></a>Avantages du fournisseur PowerShell SQL Server

Les chemins d'accès implémentés par le fournisseur [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] permettent de vérifier facilement et de manière interactive tous les objets dans une instance de SQL Server. Vous pouvez parcourir les chemins d'accès à l'aide d'alias Windows PowerShell semblables aux commandes que vous utilisez généralement pour parcourir les chemins d'accès du système de fichiers.  
  
## <a name="the-sql-server-powershell-hierarchy"></a>Hiérarchie PowerShell SQL Server

Les produits dont les données ou modèles objets peuvent être représentés dans une hiérarchie utilisent des fournisseurs Windows PowerShell pour exposer les hiérarchies. La hiérarchie est exposée à l'aide d'une structure de chemin d'accès semblable à celle utilisée par le système de fichiers Windows.  
  
 Chaque fournisseur Windows PowerShell implémente un ou plusieurs lecteurs. Chaque lecteur est le nœud racine d'une hiérarchie d'objets connexes. Le fournisseur [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] implémente un lecteur SQLSERVER:. Le fournisseur définit également un jeu de dossiers principaux pour le lecteur SQLSERVER:. Chaque dossier et ses sous-dossiers représentent l'ensemble d'objets auxquels il est possible d'accéder à l'aide d'un modèle SMO ( [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Management Object). Si vous vous trouvez au niveau d'un sous-dossier dans un chemin d'accès qui commence par l'un de ces dossiers principaux, vous pouvez utiliser les méthodes du modèle objet associé pour effectuer des actions sur l'objet représenté par ce nœud. Les dossiers Windows PowerShell implémentés par le fournisseur [!INCLUDE[ssnoversion](../includes/ssnoversion-md.md)] sont répertoriés dans le tableau suivant :  
  
|Dossier|Espace de noms du modèle objet SQL Server|Objets|  
|------------|---------------------------------------|-------------|  
|`SQLSERVER:\SQL`|<xref:Microsoft.SqlServer.Management.Smo><br /><br /> <xref:Microsoft.SqlServer.Management.Smo.Agent><br /><br /> <xref:Microsoft.SqlServer.Management.Smo.Broker><br /><br /> <xref:Microsoft.SqlServer.Management.Smo.Mail>|Objets de base de données, tels que les tables, les vues et les procédures stockées.|  
|`SQLSERVER:\SQLPolicy`|<xref:Microsoft.SqlServer.Management.Dmf><br /><br /> <xref:Microsoft.SqlServer.Management.Facets>|Objets de la Gestion basée sur des stratégies, tels que les stratégies et les facettes.|  
|`SQLSERVER:\SQLRegistration`|<xref:Microsoft.SqlServer.Management.RegisteredServers><br /><br /> <xref:Microsoft.SqlServer.Management.Smo.RegSvrEnum>|Objets de serveurs inscrits, tels que des groupes de serveurs et des serveurs inscrits.|  
|`SQLSERVER:\Utility`|<xref:Microsoft.SqlServer.Management.Utility>|Objets utilitaires, tels que les instances gérées du [!INCLUDE[ssDE](../includes/ssde-md.md)].|  
|`SQLSERVER:\DAC`|[Microsoft.SqlServer.Management.Dac](/previous-versions/sql/sql-server-2012/ee212127(v=sql.110))|Objets d'application de couche Données tels que les packages de DAC, et opérations telles que le déploiement d'une DAC.|  
|`SQLSERVER:\DataCollection`|<xref:Microsoft.SqlServer.Management.Collector>|Objets du collecteur de données tels que les jeux d'éléments de collecte et magasins de configuration.|  
|`SQLSERVER:\SSIS`|<xref:Microsoft.SqlServer.Management.IntegrationServices>|[!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] tels que les projets, les packages et les environnements.|  
|`SQLSERVER:\XEvent`|<xref:Microsoft.SqlServer.Management.XEvent>|Événements étendus SQL Server|
|`SQLSERVER:\DatabaseXEvent`|[Microsoft.SqlServer.Management.XEventDbScoped](/dotnet/api/microsoft.sqlserver.management.xeventdbscoped)|Événements étendus SQL Server|
|`SQLSERVER:\SQLAS`|<xref:Microsoft.AnalysisServices>|[!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] tels que des cubes, des agrégations et des dimensions.|  
  
 Par exemple, vous pouvez utiliser le dossier SQLSERVER:\SQL pour commencer des chemins d'accès pouvant représenter tout objet pris en charge par le modèle objet SMO. Le début d’un chemin SQLSERVER:\SQL est SQLSERVER:\SQL\\*Nom_Ordinateur*\\*Nom_instance*. Les nœuds après le nom de l’instance alternent entre des collections d’objets (telles que *Bases de données* ou *Vues*) et des noms d’objets (comme AdventureWorks2012). Les schémas ne sont pas représentés en tant que classes d'objets. Lorsque vous spécifiez le nœud pour un objet de niveau supérieur dans un schéma, tel qu’une table ou une vue, vous devez spécifier le nom d’objet au format *SchemaName.ObjectName*.  
  
 L’exemple suivant montre le chemin de la table Vendor dans le schéma Purchasing de la base de données AdventureWorks2012 dans une instance par défaut du [!INCLUDE[ssDE](../includes/ssde-md.md)] sur l’ordinateur local :  
  
```powershell
SQLSERVER:\SQL\localhost\DEFAULT\Databases\AdventureWorks2012\Tables\Purchasing.Vendor  
```
  
 Pour plus d'informations sur la hiérarchie du modèle objet SMO, consultez [SMO Object Model Diagram](../relational-databases/server-management-objects-smo/smo-object-model-diagram.md).  
  
 Les nœuds de collection dans un chemin d'accès sont associés à une classe de collection dans le modèle objet associé. Les nœuds de noms d’objets sont associés à une classe d’objet dans le modèle objet associé, comme indiqué dans le tableau suivant :  
  
|Path|Classe SMO|  
|----------|---------------|  
|`SQLSERVER:\SQL\MyComputer\DEFAULT\Databases`|<xref:Microsoft.SqlServer.Management.Smo.DatabaseCollection>|  
|`SQLSERVER:\SQL\MyComputer\DEFAULT\Databases\AdventureWorks2012`|<xref:Microsoft.SqlServer.Management.Smo.Database>|  
  
## <a name="sql-server-provider-tasks"></a>Tâches de fournisseur SQL Server  
  
|Description de la tâche|Article|  
|----------------------|-----------|  
|Explique comment utiliser des applets de commande Windows PowerShell pour parcourir les nœuds dans un chemin d'accès et obtenir la liste des objets au niveau de chaque nœud.|[Parcourir les chemins SQL Server PowerShell](navigate-sql-server-powershell-paths.md)|  
|Explique comment utiliser les méthodes et les propriétés SMO pour signaler et effectuer un travail sur l'objet représenté par un nœud dans un chemin d'accès. Explique également comment obtenir la liste des méthodes et des propriétés SMO pour ce nœud.|[Utiliser des chemins SQL Server PowerShell](work-with-sql-server-powershell-paths.md)|  
|Explique comment convertir une valeur URN (Uniform Resource Name) SMO en chemin d'accès de fournisseur SQL Server.|[Convertir des URN en chemins de fournisseur SQL Server](/powershell/module/sqlserver/Convert-UrnToPath)|  
|Explique comment ouvrir des connexions d'authentification SQL Server à l'aide du fournisseur [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . Par défaut, le fournisseur utilise des connexions via l'authentification Windows établies à l'aide des informations d'identification du compte Windows qui exécute la session Windows PowerShell.|[Gérer l’authentification dans le moteur de base de données PowerShell](manage-authentication-in-database-engine-powershell.md)|  
  
## <a name="next-steps"></a>Étapes suivantes

[SQL Server PowerShell](sql-server-powershell.md)