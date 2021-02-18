---
title: Démarrer une trace
titleSuffix: SQL Server Profiler
description: Découvrez comment démarrer une trace et localiser ses données après avoir défini une nouvelle trace ou créé un modèle dans SQL Server Profiler.
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: conceptual
ms.assetid: aeeb38eb-229a-4c8b-ad66-57e9ce45fb6a
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: df7bc3602d03cf6467a022f060db6b5a63f1c747
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341988"
---
# <a name="start-a-trace"></a>Démarrer une trace

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Après avoir défini une nouvelle trace ou créé un modèle en utilisant le [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)], vous pouvez démarrer, suspendre ou arrêter la capture des données en utilisant la définition de la nouvelle trace ou le modèle.  
  
## <a name="starting-a-trace"></a>Démarrage d’une trace

Lorsque vous démarrez une trace et que la source définie est une instance de [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] ou des [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] crée une file d'attente qui fournit un espace de stockage temporaire pour les événements serveur capturés.  
  
Lorsque vous utilisez le [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] pour accéder à Trace SQL, une nouvelle fenêtre de trace s'ouvre (si aucune n'est ouverte) au démarrage d'une trace et les données sont capturées immédiatement.  
  
Lorsque vous utilisez des procédures stockées système [!INCLUDE[tsql](../../includes/tsql-md.md)] pour accéder à Trace SQL, vous devez démarrer une trace chaque fois qu'une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] démarre pour capturer des données. Lorsqu’une trace est démarrée, vous pouvez modifier uniquement le nom de la trace.  
  
> [!NOTE]  
>  Lorsque vous utilisez des traces existantes, vous pouvez afficher les propriétés, mais pas les modifier. Pour modifier les propriétés, arrêtez ou suspendez la trace.  
  
## <a name="see-also"></a>Voir aussi

[Démarrer automatiquement une trace après s’être connecté à un serveur &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/start-a-trace-automatically-after-connecting-to-a-server-sql-server-profiler.md)   

[Suspendre une trace &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/pause-a-trace-sql-server-profiler.md)   

[Arrêter une trace &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/stop-a-trace-sql-server-profiler.md)   

[Effacer une fenêtre de trace &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/clear-a-trace-window-sql-server-profiler.md)   

[Exécuter une trace après qu’elle a été suspendue ou arrêtée &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/run-a-trace-after-it-has-been-paused-or-stopped-sql-server-profiler.md)