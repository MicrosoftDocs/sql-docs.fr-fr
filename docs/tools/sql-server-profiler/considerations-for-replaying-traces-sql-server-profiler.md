---
title: Considérations sur la relecture des traces
titleSuffix: SQL Server Profiler
description: Découvrez les opérations, les procédures stockées, les modèles et les activités de journalisation qui empêchent SQL Server Profiler d’effectuer la relecture des traces.
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: conceptual
ms.assetid: 73fa339f-b71a-4be4-97ca-d4ae84c8b90b
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/06/2017
ms.openlocfilehash: d06b9769a76d7e6555e0dc671782fcd062b4574f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349421"
---
# <a name="considerations-for-replaying-traces-sql-server-profiler"></a>Considérations sur la relecture des traces (SQL Server Profiler)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] ne peut pas relire les types de traces suivants :  
  
-   Les traces qui contiennent une réplication transactionnelle et d'autres activités de journal de transactions. Ces événements sont ignorés. Les autres types de réplication ne sont pas affectés car ils ne font pas d'écritures dans le journal des transactions.  
  
-   Les traces qui contiennent des opérations impliquant des identificateurs globaux uniques (GUID). Ces événements sont ignorés.  
  
-   Les traces qui contiennent des opérations sur des colonnes **text**, **ntext** et **image** ayant recours à l’utilitaire **bcp** , aux instructions BULK INSERT, READTEXT, WRITETEXT et UPDATETEXT ainsi qu’aux opérations de texte intégral. Ces événements sont ignorés.  
  
-   Les traces contenant des liaisons de session : procédures stockées système **sp_getbindtoken** et **sp_bindsession** . Ces événements sont ignorés.  
  
> [!NOTE]  
>  Si vous n’utilisez pas le modèle de relecture préconfigurée (**TSQL_Replay**) et que vous ne capturez pas toutes les données requises, le [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] ne relit pas la trace. Pour plus d’informations, consultez [Conditions préalables à la relecture](../../tools/sql-server-profiler/replay-requirements.md).  
  
 Pour savoir quelles autorisations sont nécessaires pour relire une trace, consultez [Autorisations nécessaires pour exécuter SQL Server Profiler](../../tools/sql-server-profiler/permissions-required-to-run-sql-server-profiler.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Utilitaire bcp](../../tools/bcp-utility.md)   
 [Informations de référence sur la classe d’événements SQL Server](../../relational-databases/event-classes/sql-server-event-class-reference.md)   
 [sp_getbindtoken &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-getbindtoken-transact-sql.md)   
 [sp_bindsession &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-bindsession-transact-sql.md)   
 [BULK INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/bulk-insert-transact-sql.md)   
 [READTEXT &#40;Transact-SQL&#41;](../../t-sql/queries/readtext-transact-sql.md)   
 [WRITETEXT &#40;Transact-SQL&#41;](../../t-sql/queries/writetext-transact-sql.md)   
 [UPDATETEXT &#40;Transact-SQL&#41;](../../t-sql/queries/updatetext-transact-sql.md)  
  
  
