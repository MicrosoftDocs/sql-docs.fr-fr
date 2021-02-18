---
title: Placer des identificateurs SQL Server dans une séquence d'échappement
description: Certains caractères qui peuvent s’afficher dans les identificateurs délimités de SQL Server ne sont pas pris en charge dans les chemins Windows PowerShell. Découvrez comment certaines d’entre elles peuvent être placées dans une séquence d’échappement avec le caractère de cycle arrière.
ms.prod: sql
ms.technology: sql-server-powershell
ms.topic: conceptual
ms.assetid: 8a73e945-daa6-4e5d-93da-10f000f1f3a2
author: markingmyname
ms.author: maghan
ms.reviewer: matteot, drskwier
ms.custom: ''
ms.date: 10/14/2020
ms.openlocfilehash: 25d6badaab16caef587afc9843ccc220f392acb4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354307"
---
# <a name="escape-sql-server-identifiers"></a>Placer des identificateurs SQL Server dans une séquence d'échappement

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Vous pouvez souvent utiliser le caractère d’échappement PowerShell « ` » (backtick) pour placer dans une séquence d’échappement les caractères qui sont autorisés dans les identificateurs délimités [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], mais pas les noms de chemins Windows PowerShell. Certains caractères ne peuvent toutefois pas être placés dans une séquence d'échappement. Par exemple, vous ne pouvez pas placer dans une séquence d’échappement le caractère deux-points (:) dans Windows PowerShell. Les identificateurs contenant ce caractère doivent être encodés. L'encodage est plus fiable que l'utilisation d'une séquence d'échappement, car l'encodage fonctionne pour tous les caractères.  

[!INCLUDE [sql-server-powershell-version](../includes/sql-server-powershell-version.md)]

Le caractère ` (backtick) se trouve généralement sur une touche située dans la partie supérieure gauche du clavier, sous la touche Échap.  

## <a name="examples"></a>Exemples

Voici un exemple de séquence d'échappement pour le caractère # :  

```powershell
cd SQLSERVER:\SQL\MyComputer\MyInstance\MyDatabase\MySchema\`#MyTempTable  
```

Voici un exemple de séquence d'échappement de la parenthèse lors de la spécification de (local) comme nom d'ordinateur :  

```powershell
Set-Location SQLSERVER:\SQL\`(local`)\DEFAULT  
```

## <a name="see-also"></a>Voir aussi

- [Identificateurs SQL Server dans PowerShell](sql-server-identifiers-in-powershell.md)
- [Fournisseur SQL Server PowerShell](sql-server-powershell-provider.md)
- [SQL Server PowerShell](sql-server-powershell.md)