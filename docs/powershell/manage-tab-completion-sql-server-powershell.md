---
title: Gérer la saisie semi-automatique par tabulation (SQL Server PowerShell)
description: Découvrez comment contrôler la saisie semi-automatique via la touche tab Windows PowerShell en utilisant correctement trois variables dans les modules SQL Server PowerShell.
ms.prod: sql
ms.technology: sql-server-powershell
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: matteot, drskwier
ms.custom: ''
ms.date: 10/14/2020
ms.openlocfilehash: 4fd55fc24889e84757fb1d78f5e9cd5c8dbb7212
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/04/2021
ms.locfileid: "101838010"
---
# <a name="manage-tab-completion-with-sql-server-powershell"></a>Gérer la saisie semi-automatique via la touche Tab avec SQL Server PowerShell

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

 Les composants logiciels enfichables [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] PowerShell introduisent trois variables (**$SqlServerMaximumTabCompletion**, **$SqlServerMaximumChildItems** et **$SqlServerIncludeSystemObjects**) pour contrôler la complétion par tabulation de Windows PowerShell. La saisie semi-automatique par tabulation réduit la quantité de caractères que vous devez taper en renvoyant des tableaux d'éléments dont le nom commence par la chaîne que vous tapez.  

[!INCLUDE [sql-server-powershell-version](../includes/sql-server-powershell-version.md)]

Avec la saisie semi-automatique par tabulation de Windows PowerShell, une fois que vous avez tapé une partie d'un chemin d'accès ou d'un nom d'applet de commande, vous pouvez appuyer sur la touche Tab pour obtenir la liste des éléments dont le nom correspond à ce que vous avez déjà tapé. Vous pouvez alors sélectionner l'élément souhaité dans la liste sans avoir à taper le reste du nom.  

Si vous travaillez dans une base de données qui contient beaucoup d’objets, les listes de saisie semi-automatique par tabulation peuvent devenir longues. Certains types d'objets [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] , tels que les vues, contiennent également de nombreux objets système.  

Les composants logiciels enfichables [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] introduisent trois variables système que vous pouvez utiliser pour contrôler la quantité d’informations présentées par le biais de la saisie semi-automatique par tabulation et de **Get-ChildItem**.

## <a name="sqlservermaximumtabcompletion--n"></a>$SqlServerMaximumTabCompletion =** *n*

Spécifie le nombre maximal d'objets à inclure dans une liste de saisie semi-automatique par tabulation. Si vous appuyez sur la touche Tab au niveau d’un nœud de chemin contenant plus de *n* objets, la liste de saisie semi-automatique par tabulation est tronquée au niveau *n*. *n* est un entier. Le paramètre par défaut 0 signifie que le nombre d'objets répertoriés est illimité.  

## <a name="sqlservermaximumchilditems--n"></a>$SqlServerMaximumChildItems =** *n*

Spécifie le nombre maximal d’objets affichés par **Get-ChildItem**. Si **Get-ChildItem** est exécuté sur un nœud de chemin contenant plus de *n* objets, la liste est tronquée au niveau *n*. *n* est un entier. Le paramètre par défaut 0 signifie que le nombre d'objets répertoriés est illimité.  

## <a name="sqlserverincludesystemobjects---true--false-"></a>$SqlServerIncludeSystemObjects =** { **$True** | **$False** }

Si cette variable est définie sur **$True**, les objets système sont affichés par le biais de la saisie semi-automatique par tabulation et de **Get-ChildItem**. Si cette variable est définie sur **$False**, aucun objet système n’est affiché. La valeur par défaut est **$False**.  

## <a name="set-the-sql-server-tab-completion-variables"></a>Définir les variables de la saisie semi-automatique par tabulation de SQL Server

Pour chacune des variables pour lesquelles vous souhaitez utiliser une valeur autre que la valeur par défaut, définissez la nouvelle valeur de la variable.  

### <a name="example-powershell"></a>Exemple (PowerShell)

L'exemple suivant définit les trois variables et répertorie leurs paramètres :  

```powershell
$SqlServerMaximumTabCompletion = 20  
$SqlServerMaximumChildItems = 10  
$SqlServerIncludeSystemObjects = $False  
dir variable:sqlserver*  
```

## <a name="see-also"></a>Voir aussi

- [Installer SQL Server PowerShell](download-sql-server-ps-module.md)
- [SQL Server PowerShell](sql-server-powershell.md)