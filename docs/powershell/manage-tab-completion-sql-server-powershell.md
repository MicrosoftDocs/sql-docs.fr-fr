---
title: Gérer la saisie semi-automatique par tabulation (SQL Server PowerShell) | Microsoft Docs
description: Découvrez comment contrôler la saisie semi-automatique via la touche tab Windows PowerShell en utilisant correctement trois variables dans les modules SQL Server PowerShell.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: sql-server-powershell
ms.topic: conceptual
ms.assetid: 6296848a-890f-4ad3-8d9f-92ed6a79aa00
author: markingmyname
ms.author: maghan
ms.openlocfilehash: b60741f9e35b9910f9650d1ce7dcb920ba22c08b
ms.sourcegitcommit: a9f16d7819ed0e2b7ad8f4a7d4d2397437b2bbb2
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/21/2020
ms.locfileid: "88714127"
---
# <a name="manage-tab-completion-sql-server-powershell"></a>Gérer la saisie semi-automatique par tabulation (SQL Server PowerShell)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

 Les composants logiciels enfichables [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] PowerShell introduisent trois variables (**$SqlServerMaximumTabCompletion**, **$SqlServerMaximumChildItems** et **$SqlServerIncludeSystemObjects**) pour contrôler la complétion par tabulation de Windows PowerShell. La saisie semi-automatique par tabulation réduit la quantité de caractères que vous devez taper en renvoyant des tableaux d'éléments dont le nom commence par la chaîne que vous tapez.  

> [!NOTE]
> Il existe deux modules SQL Server PowerShell : **SqlServer** et **SQLPS**. Le module **SQLPS** fait partie de l’installation de SQL Server (à des fins de compatibilité descendante), mais il n’est plus mis à jour. Le module PowerShell le plus récent est **SqlServer**. Le module **SqlServer** contient les versions mises à jour des applets de commande disponibles dans **SQLPS**, ainsi que de nouvelles applets de commande pour prendre en charge les dernières fonctionnalités SQL.  
> Des versions précédentes du module **SqlServer** *étaient* fournies avec SQL Server Management Studio (SSMS), mais uniquement avec les versions 16.x de SSMS. Pour utiliser PowerShell avec SSMS 17.0 et ultérieur, vous devez installer le module **SqlServer** à partir de PowerShell Gallery.
> Pour installer le module **SqlServer**, consultez [Installer SQL Server PowerShell](download-sql-server-ps-module.md).
  
Avec la saisie semi-automatique par tabulation de Windows PowerShell, une fois que vous avez tapé une partie d'un chemin d'accès ou d'un nom d'applet de commande, vous pouvez appuyer sur la touche Tab pour obtenir la liste des éléments dont le nom correspond à ce que vous avez déjà tapé. Vous pouvez alors sélectionner l'élément souhaité dans la liste sans avoir à taper le reste du nom.  
  
Si vous travaillez dans une base de données qui contient beaucoup d’objets, les listes de saisie semi-automatique par tabulation peuvent devenir longues. Certains types d'objets [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] , tels que les vues, contiennent également de nombreux objets système.  
  
Les composants logiciels enfichables [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] introduisent trois variables système que vous pouvez utiliser pour contrôler la quantité d’informations présentées par le biais de la saisie semi-automatique par tabulation et de **Get-ChildItem**.  
  
 **$SqlServerMaximumTabCompletion =** *n*  
 Spécifie le nombre maximal d'objets à inclure dans une liste de saisie semi-automatique par tabulation. Si vous appuyez sur la touche Tab au niveau d’un nœud de chemin contenant plus de *n* objets, la liste de saisie semi-automatique par tabulation est tronquée au niveau *n*. *n* est un entier. Le paramètre par défaut 0 signifie que le nombre d'objets répertoriés est illimité.  
  
 **$SqlServerMaximumChildItems =** *n*  
 Spécifie le nombre maximal d’objets affichés par **Get-ChildItem**. Si **Get-ChildItem** est exécuté sur un nœud de chemin contenant plus de *n* objets, la liste est tronquée au niveau *n*. *n* est un entier. Le paramètre par défaut 0 signifie que le nombre d'objets répertoriés est illimité.  
  
 **$SqlServerIncludeSystemObjects =** { **$True** |  **$False** }  
 Si cette variable est définie sur **$True**, les objets système sont affichés par le biais de la saisie semi-automatique par tabulation et de **Get-ChildItem**. Si cette variable est définie sur **$False**, aucun objet système n’est affiché. La valeur par défaut est **$False**.  
  
## <a name="set-the-sql-server-tab-completion-variables"></a>Définir les variables de la saisie semi-automatique par tabulation de SQL Server  
 Pour chacune des variables pour lesquelles vous souhaitez utiliser une valeur autre que la valeur par défaut, définissez la nouvelle valeur de la variable.  
  
### <a name="example-powershell"></a>Exemple (PowerShell)  
 L'exemple suivant définit les trois variables et répertorie leurs paramètres :  
  
```  
$SqlServerMaximumTabCompletion = 20  
$SqlServerMaximumChildItems = 10  
$SqlServerIncludeSystemObjects = $False  
dir variable:sqlserver*  
```  
  
## <a name="see-also"></a>Voir aussi  
 [SQL Server PowerShell](sql-server-powershell.md)  
  
  
