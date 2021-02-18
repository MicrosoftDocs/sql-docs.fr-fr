---
title: Relire jusqu’à un curseur
titleSuffix: SQL Server Profiler
description: Découvrez comment utiliser un curseur dans SQL Server Profiler pour suspendre la relecture d’une trace à un point spécifique. Facilitez le débogage en relisant à un curseur.
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: conceptual
ms.assetid: 89eadc41-4424-4a1c-ba61-0b52c851cdb1
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.openlocfilehash: a903e7176b4715dcf54db8a55b28a280a825b3bf
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353354"
---
# <a name="replay-to-a-cursor-sql-server-profiler"></a>Relire jusqu'à un curseur (SQL Server Profiler)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Cette rubrique indique comment relire des fichiers ou des tables de trace suspendus lorsqu'un curseur est atteint à l'aide de [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]. La suspension de traces sur des curseurs simplifie le débogage car vous pouvez ainsi fractionner la relecture de longs scripts de trace en courts segments se prêtant à l'analyse incrémentielle.  
  
### <a name="to-replay-to-the-cursor"></a>Pour relire jusqu'au curseur  
  
1.  Ouvrez le fichier de trace ou la table de trace à relire. Pour plus d’informations, consultez [Ouvrir un fichier de trace &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/open-a-trace-file-sql-server-profiler.md) ou l'Assistant Paramétrage du [Ouvrir une table de trace &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/open-a-trace-table-sql-server-profiler.md).  
  
     Vérifiez que le fichier de trace ou la table de trace que vous ouvrez contient les classes d'événements nécessaires pour la relecture. Pour plus d’informations, consultez [Conditions préalables à la relecture](../../tools/sql-server-profiler/replay-requirements.md).  
  
2.  Dans la fenêtre de trace, cliquez sur un événement.  
  
3.  Dans le menu **Relire** , cliquez sur **Exécuter jusqu’au curseur**, puis connectez-vous au serveur où vous souhaitez relire la trace.  
  
4.  Dans la boîte de dialogue **Configuration de la relecture** , vérifiez les paramètres, puis cliquez sur **OK**.  
  
     La relecture commence, puis marque une pause lorsque le premier curseur est atteint.  
  
5.  Appuyez sur F5 pour reprendre la trace.  
  
6.  Répétez l'étape 5 jusqu'à la fin de la trace.  
  
## <a name="see-also"></a>Voir aussi  
 [Relire jusqu’à un point d’arrêt &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/replay-to-a-breakpoint-sql-server-profiler.md)   
 [Relire des traces](../../tools/sql-server-profiler/replay-traces.md)   
 [SQL Server Profiler](../../tools/sql-server-profiler/sql-server-profiler.md)  
  
  
