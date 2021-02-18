---
title: SQL Server PowerShell
description: En savoir plus sur les deux modules SQL Server PowerShell, SqlServer et SQLPS, qui incluent des fournisseurs et des cmdlets PowerShell.
ms.prod: sql
ms.technology: sql-server-powershell
ms.topic: conceptual
ms.assetid: 89b70725-bbe7-4ffe-a27d-2a40005a97e7
author: markingmyname
ms.author: maghan
ms.reviewer: matteot
ms.custom: ''
ms.date: 06/11/2020
ms.openlocfilehash: d8b03ae1e20acfefd673717858d57c894c619437
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350156"
---
# <a name="sql-server-powershell"></a>SQL Server PowerShell

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

**[Installer SQL Server PowerShell](download-sql-server-ps-module.md)**

Il existe deux modules SQL Server PowerShell  **[SqlServer](/powershell/module/sqlserver)** et **[SQLPS](/powershell/module/sqlps)** .

Le module **SqlServer** est le module PowerShell actuel à utiliser.

Le module **SQLPS** fait partie de l’installation de SQL Server (à des fins de compatibilité descendante), mais il n’est plus mis à jour.

Le module **SqlServer** contient les versions mises à jour des applets de commande disponibles dans **SQLPS** ainsi que de nouvelles applets de commande pour prendre en charge les dernières fonctionnalités SQL.

Des versions précédentes du module **SqlServer** *étaient* fournies avec [SQL Server Management Studio (SSMS)](../ssms/download-sql-server-management-studio-ssms.md), mais uniquement avec les versions 16.x de SSMS.

Pour utiliser PowerShell avec SSMS 17.0 et versions ultérieures, installez le module **SqlServer** à partir de [PowerShell Gallery](https://www.powershellgallery.com/packages/SqlServer).

Vous pouvez également utiliser [PowerShell avec Azure Data Studio](../azure-data-studio/extensions/powershell-extension.md).

**Pourquoi le module SQLPS s’appelle-t-il désormais SqlServer ?**

Pour expédier les mises à jour SQL PowerShell, nous avons dû modifier l’identité du module SQL PowerShell et le wrapper appelé *SQLPS.exe*. En raison de ce changement, il existe désormais deux modules SQL PowerShell : **SqlServer** et **SQLPS**.  

**Mettez à jour vos scripts PowerShell s’ils importent le module SQLPS.**

Si vous avez des scripts PowerShell qui exécutent `Import-Module -Name SQLPS` et que vous souhaitez tirer parti des fonctionnalités du nouveau fournisseur et des nouvelles applets de commande, vous devez utiliser à la place `Import-Module -Name SqlServer`. Le nouveau module est installé dans le dossier `%ProgramFiles%\WindowsPowerShell\Modules\SqlServer`. Il est donc inutile de mettre à jour la variable $env:PSModulePath. Si vous avez des scripts qui utilisent une version Community ou tierce d’un module nommé **SqlServer**, utilisez le paramètre Prefix pour éviter toute collision de noms.

Il est recommandé de démarrer votre script avec *import-module SQLServer* afin d’éviter les problèmes côte à côte si le module SQLPS est installé sur le même ordinateur.

Cette section s’applique aux scripts exécutés à partir de PowerShell et non à l’agent SQL. Le nouveau module peut être utilisé avec les étapes de travail de SQL Agent à l’aide de [#NOSQLPS](#sql-server-agent).

## <a name="sql-server-powershell-components"></a>Composants de SQL Server PowerShell

Le module **SqlServer** est fourni avec :

- Des [Fournisseurs PowerShell](/powershell/module/microsoft.powershell.core/about/about_providers) qui activent un mécanisme de navigation simple semblable aux chemins d’accès des systèmes de fichiers. Vous pouvez générer des chemins d'accès semblables aux chemins d'accès des systèmes de fichiers, où le lecteur est associé à un modèle objet de gestion SQL Server et les nœuds sont basés sur les classes du modèle objet. Vous pouvez ensuite utiliser des commandes familières telles que **cd** et **dir** pour naviguer parmi les chemins d’accès de la même façon que vous naviguez parmi des dossiers dans une fenêtre d’invite de commandes. Vous pouvez utiliser d’autres commandes, telles que **ren** ou **del**, pour exécuter des actions sur les nœuds du chemin d’accès.

- Un jeu de cmdlets qui prennent en charge des actions telles que l’exécution d’un script **sqlcmd** contenant des instructions Transact-SQL ou XQuery.  

- Le fournisseur AS et les cmdlets, qui étaient installés précédemment séparément.

## <a name="sql-server-versions"></a>Versions de SQL Server

Les applets de commande SQL PowerShell peuvent servir à gérer des instances d’Azure SQL Database, Azure Synapse Analytics et tous les [produits SQL Server pris en charge](https://support.microsoft.com/lifecycle/search/1044).

## <a name="sql-server-identifiers-that-contain-characters-not-supported-in-powershell-paths"></a>Identificateurs SQL Server contenant des caractères non pris en charge dans les chemins PowerShell

Les applets de commande **Encode-Sqlname** et **Decode-Sqlname** vous aident à spécifier les identificateurs SQL Server qui contiennent des caractères non pris en charge dans les chemins PowerShell. Pour plus d’informations, consultez [Identificateurs SQL Server dans PowerShell](sql-server-identifiers-in-powershell.md).

Utilisez la cmdlet **Convert-UrnToPath** pour convertir un nom de ressource unique pour un objet du Moteur de base de données en un chemin d’accès pour le fournisseur PowerShell SQL Server. Pour plus d’informations, consultez [Convertir des URN en chemins d’accès de fournisseur SQL Server](/powershell/module/sqlserver/Convert-UrnToPath).
  
## <a name="query-expressions-and-unique-resource-names"></a>Expressions de requête et noms de ressource uniques  

Les expressions de requête sont des chaînes qui utilisent une syntaxe semblable à XPath pour spécifier un jeu de critères permettant d'énumérer un ou plusieurs objets dans une hiérarchie de modèle objet. Un nom de ressource unique (URN) est un type spécifique de chaîne d'expression de requête qui identifie de façon unique un objet particulier. Pour plus d’informations, consultez [Expressions de requête et noms URN](query-expressions-and-uniform-resource-names.md).

## <a name="sql-server-agent"></a>SQL Server Agent

Le module utilisé par SQL Server Agent est le même. Par conséquent, les tâches SQL Server Agent, qui ont des étapes de travail de type PowerShell utilisent le module SQLPS. Pour plus d’informations, consultez [Guide pratique pour exécuter PowerShell avec SQL Server Agent](run-windows-powershell-steps-in-sql-server-agent.md). Cependant, depuis SQL Server 2019, vous pouvez désactiver SQLPS. Pour ce faire, sur la première ligne d’une étape de travail de type PowerShell, vous pouvez ajouter `#NOSQLPS`, ce qui empêche SQL Agent de charger automatiquement le module SQLPS. Dans ce cas, la tâche de votre SQL Agent exécute la version de PowerShell installée sur l’ordinateur, puis vous pouvez utiliser n’importe quel autre module PowerShell de votre choix.

Si vous souhaitez utiliser le module **SqlServer** à l’étape de la tâche de votre SQL Agent, vous pouvez placer ce code sur les deux premières lignes de votre script.

```powershell
#NOSQLPS
Import-Module -Name SqlServer
```

## <a name="cmdlet-reference"></a>Référence des applets de commande

- [Applets de commande SqlServer](/powershell/module/sqlserver)
- [Applets de commande SQLPS](/powershell/module/sqlps)

## <a name="next-steps"></a>Étapes suivantes

- [Télécharger le module SQL Server PowerShell](download-sql-server-ps-module.md)
- [Applets de commande SQL Server PowerShell](/powershell/module/sqlserver)
- [Utiliser PowerShell avec Azure Data Studio](../azure-data-studio/extensions/powershell-extension.md)