---
title: Exécuter Windows PowerShell à partir de SQL Server Management Studio | Microsoft Docs
description: Découvrez comment démarrer une session Windows PowerShell à partir de l’Explorateur d’objets dans SQL Server Management Studio, avec la présélection de chemin d’accès à l’emplacement de votre choix d’objets.
ms.prod: sql
ms.technology: sql-server-powershell
ms.topic: conceptual
ms.assetid: 1f841825-da1f-4062-9a81-3cdbab03845b
author: markingmyname
ms.author: maghan
ms.reviewer: matteot, drskwier
ms.custom: ''
ms.date: 03/14/2017
ms.openlocfilehash: 3e74949152adb8ecadc720ced14ff3b29b196fd6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338170"
---
# <a name="run-windows-powershell-from-sql-server-management-studio"></a>Exécuter Windows PowerShell à partir de SQL Server Management Studio

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Vous pouvez démarrer des sessions Windows PowerShell à partir de l' **Explorateur d'objets** dans [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)]. [!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)] lance Windows PowerShell, charge le module **SqlServer** et affecte au contexte de chemin le nœud associé dans l’arborescence de **l’Explorateur d’objets**.  

[!INCLUDE [sql-server-powershell-version](../includes/sql-server-powershell-version.md)]

Quand vous spécifiez l’exécution de PowerShell pour un objet dans l’ **Explorateur d’objets**, [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] démarre une session Windows PowerShell dans laquelle les composants logiciels enfichables [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] PowerShell ont été chargés et inscrits. Le chemin de la session est prédéfini avec l’emplacement de l’objet sur lequel vous avez cliqué avec le bouton droit dans l’Explorateur d’objets. Par exemple, si vous cliquez avec le bouton droit sur l’objet de base de données [!INCLUDE[ssSampleDBobject](../includes/sssampledbobject-md.md)] dans l’Explorateur d’objets et que vous sélectionnez **Démarrer PowerShell**, le chemin Windows PowerShell est défini comme suit :  

```powershell
SQLSERVER:\SQL\MyComputer\MyInstance\Databases\AdventureWorks2012>  
```

## <a name="run-powershell"></a>Exécuter PowerShell

### <a name="to-run-powershell-from-sql-server-management-studio"></a>Pour exécuter PowerShell à partir de SQL Server Management Studio

1. Ouvrez l' **Explorateur d'objets**.

2. Accédez au nœud de l'objet à utiliser.

3. Cliquez avec le bouton droit sur l’objet et sélectionnez **Démarrer PowerShell**.

## <a name="permissions"></a>Autorisations

S’il a été ouvert à partir de [!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)], PowerShell ne fonctionne pas avec les privilèges d’administrateur, ce qui peut bloquer certaines activités comme les appels à WMI.  
  
## <a name="see-also"></a>Voir aussi

- [SQL Server PowerShell](sql-server-powershell.md)