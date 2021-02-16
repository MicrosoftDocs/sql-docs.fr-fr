---
title: Utiliser SSMS XEvent Profiler
description: XEvent Profiler affiche une visualisation en direct des événements étendus. Découvrez pourquoi utiliser ce profileur, les fonctionnalités clés et comment commencer à afficher des événements étendus.
ms.date: 10/02/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: genemi
ms.technology: xevents
ms.topic: tutorial
helpviewer_keywords:
- extended events [SQL Server], system health session
- extended events [SQL Server], system_health session
- system_health session [SQL Server extended events]
- system health session [SQL Server extended events]
ms.assetid: 1e1fad43-d747-4775-ac0d-c50648e56d78
author: yualan
ms.author: alayu
monikerRange: '>=sql-server-2016||>=sql-server-linux-2017'
ms.openlocfilehash: 0cfc93c0e35ccfa81b6570187056dc8e2b500741
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100355276"
---
# <a name="use-the-ssms-xevent-profiler"></a>Utiliser SSMS XEvent Profiler

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
XEvent Profiler est une fonctionnalité SQL Server Management Studio (SSMS) qui affiche une fenêtre de visualisation en direct des événements étendus. Cette vue d’ensemble décrit les raisons de l’utiliser et ses principales fonctionnalités, puis fournit des instructions pour commencer à visualiser des événements étendus.

## <a name="why-would-i-use-the-xevent-profiler"></a>Pourquoi utiliser XEvent Profiler ?
Contrairement à SQL Profiler, XEvent Profiler est directement intégré à SSMS et s’appuie sur la technologie d’événements étendus scalable du moteur SQL. Cette fonctionnalité permet d’accéder rapidement à une vue de streaming en direct des événements de diagnostic sur le serveur SQL. Cette vue peut être personnalisée et ces personnalisations peuvent être partagées avec d’autres utilisateurs SSMS sous forme de fichier .viewsettings. La session créée par XE Profiler est moins intrusive dans le serveur SQL en cours d’exécution qu’une trace SQL similaire le serait si vous utilisiez SQL Profiler. Cette session peut être aussi personnalisée par l’utilisateur avec l’interface utilisateur des propriétés de session XE existante ou avec TSQL.

## <a name="prerequisites"></a>Prérequis
Cette fonctionnalité est uniquement disponible dans SQL Server Management Studio (SSMS) version 17.3 ou ultérieure. Veillez à utiliser la version la plus récente. Vous trouverez la dernière version [ici.](../../ssms/download-sql-server-management-studio-ssms.md)

## <a name="getting-started"></a><a id="getting-started"></a>Commencer
Pour accéder à XEvent Profiler, procédez comme suit :

1. Ouvrez **SQL Server Management Studio**.

2. Connectez-vous à une instance du Moteur de base de données SQL Server ou à un hôte local.

3. Dans l’Explorateur d’objets, recherchez l’élément de menu XE Profiler et développez-le en cliquant sur le signe « + ».

   ![Menu XEProfiler](media/xevents-xe-profiler-menu.png)

4. Double-cliquez sur **Standard** si vous souhaitez voir tous les événements étendus de cette session. Cliquez sur **T-SQL** si vous souhaitez voir les instructions SQL journalisées. Si une session n’est pas encore créée, une session est créée pour vous.

   ![Session XEProfiler](media/xevents-xe-profiler-start-session.png)

5. Vous pouvez maintenant voir vos événements étendus.

   ![Visionneuse XEProfiler](media/xevents-xe-profiler-start-viewer.png)

## <a name="see-also"></a>Voir aussi
[Événements étendus](../../relational-databases/extended-events/extended-events.md)  
[Outils associés aux événements étendus](../../relational-databases/extended-events/extended-events-tools.md)  
  
