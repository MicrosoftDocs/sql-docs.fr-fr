---
title: Relire un script Transact-SQL
titleSuffix: SQL Server Profiler
description: Découvrez comment utiliser SQL Server Profiler pour relire des scripts Transact-SQL afin de pouvoir comparer différentes solutions possibles à un problème de performances.
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: conceptual
ms.assetid: 9c0eb222-e6e3-4bc1-a25f-a41e962d361b
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: 20252994f90032293730335dda1f6dd689133497
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353434"
---
# <a name="replay-a-transact-sql-script-sql-server-profiler"></a>Relire un script Transact-SQL (SQL Server Profiler)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Lorsque vous testez d'éventuelles solutions à un problème de performances, utilisez le [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] pour lire des scripts [!INCLUDE[tsql](../../includes/tsql-md.md)] , et comparer les performances avant et après les modifications.  
  
### <a name="to-replay-a-transact-sql-script"></a>Relecture d'un script Transact-SQL  
  
1.  Dans le menu **Fichier** , pointez sur **Ouvrir**, puis cliquez sur **Fichier de script**.  
  
2.  Sélectionnez le fichier de script [!INCLUDE[tsql](../../includes/tsql-md.md)] que vous souhaitez ouvrir. Vérifiez que le script [!INCLUDE[tsql](../../includes/tsql-md.md)] contient les événements nécessaires pour la relecture. Pour plus d’informations, consultez [Conditions préalables à la relecture](../../tools/sql-server-profiler/replay-requirements.md).  
  
3.  Dans le menu **Relire** , cliquez sur **Démarrer**, puis connectez-vous au serveur où vous souhaitez relire le script.  
  
4.  Dans la boîte de dialogue **Configuration de la relecture** , vérifiez les paramètres, puis cliquez sur **OK**.  
  
## <a name="see-also"></a>Voir aussi  
 [Relire des traces](../../tools/sql-server-profiler/replay-traces.md)   
 [SQL Server Profiler](../../tools/sql-server-profiler/sql-server-profiler.md)  
  
  
