---
description: Fonctionnalités dépréciées du moteur de base de données dans [!INCLUDE[sssql19-md](../includes/sssql19-md.md)]
title: Fonctionnalités du moteur de base de données déconseillées dans SQL Server 2019 | Microsoft Docs
titleSuffix: SQL Server 2019
ms.custom: seo-lt-2019
ms.date: 02/12/2021
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: release-landing
ms.topic: conceptual
helpviewer_keywords:
- deprecated changes 2019 [SQL Server]
ms.assetid: ''
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15'
ms.openlocfilehash: 4e322ff478e9ccdc031882e7c3b5b18fd8506e42
ms.sourcegitcommit: ca81fc9e45fccb26934580f6d299feb0b8ec44b7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/05/2021
ms.locfileid: "102186450"
---
# <a name="deprecated-database-engine-features-in-sql-server-2019-15x"></a>Fonctionnalités du moteur de base de données dépréciées dans SQL Server 2019 (15.x)

[!INCLUDE[sqlserver2019](../includes/applies-to-version/sqlserver2019.md)]

[!INCLUDE [sssql19-md](../includes/sssql19-md.md)] ne déprécie aucune fonctionnalité en plus de celles qui ont été dépréciées dans les versions antérieures :

- [[!INCLUDE [sssql17-md](../includes/sssql17-md.md)]](deprecated-database-engine-features-in-sql-server-2017.md)
- [[!INCLUDE [sssql16-md](../includes/sssql16-md.md)]](deprecated-database-engine-features-in-sql-server-2016.md)

## <a name="deprecation-guidelines"></a>Indications à suivre pour la dépréciation

Quand une fonctionnalité est marquée comme étant dépréciée, cela signifie que :

- La fonctionnalité est en mode de maintenance uniquement. Aucune nouvelle modification ne lui sera apportée, notamment celles liées à l’interopérabilité avec de nouvelles fonctionnalités.
- Nous nous efforçons de ne pas retirer une fonctionnalité dépréciée des futures versions pour faciliter les mises à niveau. Cependant, dans de rares cas, nous pouvons décider d’arrêter (supprimer) définitivement une fonctionnalité de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] si elle limite des innovations futures.
- Pour les nouveaux travaux de développement, n’utilisez pas de fonctionnalités dépréciées. Pour les applications existantes, prévoyez de modifier dès que possible celles qui utilisent actuellement ces fonctionnalités.     

Vous pouvez surveiller l’utilisation des fonctionnalités déconseillées en utilisant le compteur de performance Objet des fonctionnalités dépréciées de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], ou les événements étendus `deprecation_announcement` et `deprecation_final_support`. Pour plus d’informations, consultez [Utiliser des objets SQL Server](../relational-databases/performance-monitor/use-sql-server-objects.md).  

## <a name="query-deprecated-features"></a>Interroger les fonctionnalités dépréciées

Vous pouvez également obtenir les valeurs de ces compteurs en exécutant l’instruction suivante :  

```sql
SELECT * FROM sys.dm_os_performance_counters
WHERE object_name = 'SQLServer:Deprecated Features';
```

### <a name="see-also"></a>Voir aussi

- [Changements cassants dans les fonctionnalités du moteur de base de données de SQL Server 2019](../database-engine/breaking-changes-to-database-engine-features-in-sql-server-version-15.md)
- [Fonctionnalités du moteur de base de données supprimées dans SQL Server](../database-engine/discontinued-database-engine-functionality-in-sql-server.md)
- [Compatibilité descendante du moteur de base de données SQL Server](./discontinued-database-engine-functionality-in-sql-server.md)
