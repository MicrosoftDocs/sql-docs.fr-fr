---
title: Établir un référentiel de performances | Microsoft Docs
description: Mesurez les performances à intervalles réguliers, même en l’absence de problèmes, pour établir une base de référence des performances du serveur dans SQL Server.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- database performance [SQL Server], baselines
- monitoring performance [SQL Server], baselines
- tuning databases [SQL Server], baselines
- server performance [SQL Server], baselines
- performance [SQL Server], baselines
- baseline performance [SQL Server]
- measurements for baseline statistics [SQL Server]
- monitoring server performance [SQL Server], establishing baseline
- database monitoring [SQL Server], baselines
ms.assetid: dc5aa8d6-2507-448f-ad86-4196443915fc
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: ebe94b0a4d678b888d99e51b7bda4c0addd0f17b
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/14/2020
ms.locfileid: "97461920"
---
# <a name="establish-a-performance-baseline"></a>Établir un niveau de référence des performances
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Pour déterminer si votre système [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] assure des performances optimales, mesurez les performances à intervalles réguliers, même en l'absence de problèmes, pour établir un niveau de référence des performances du serveur. Comparez chaque nouvel ensemble de mesures à ceux réalisés précédemment.  
  
 Les éléments suivants ont une influence sur les performances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]:  
  
-   ressources système (matériel) ;  
  
-   Architecture réseau  
  
-   système d'exploitation ;  
  
-   applications de base de données ;  
  
-   Applications clientes  
  
 Au minimum, vous devez effectuer des mesures de niveau de référence pour déterminer :  
  
-   les heures de pointe et les heures creuses d'exploitation ;  
  
-   les temps de réponse des requêtes de production et des commandes de traitement par lots ;  
  
-   les temps de sauvegarde et de restauration de la base de données.  
  
 Après avoir établi le niveau de référence des performances, comparez vos statistiques aux performances actuelles du serveur. Toute valeur très supérieure ou très inférieure à votre niveau de référence doit donner lieu à un examen approfondi. Elle peut indiquer la nécessité d'optimiser ou de reconfigurer certains points. Par exemple, si le temps nécessaire à l'exécution d'un ensemble de requêtes augmente, examinez les requêtes en question pour déterminer si vous pouvez les réécrire ou si vous devez ajouter des colonnes de statistiques ou de nouveaux index.  
  
## <a name="see-also"></a>Voir aussi  
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)  
  
  
