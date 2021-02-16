---
description: Tâche Exécuter le travail de l'Agent SQL Server
title: Tâche Exécuter le travail de l'Agent SQL Server
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.executesqlserveragentjobtask.f1
helpviewer_keywords:
- Execute SQL Server Agent Job task [Integration Services]
- jobs [Integration Services]
- SQL Server Agent [Integration Services]
ms.assetid: 3aa3bc0e-1a1c-452e-81b8-b4e3422ea053
author: chugugrace
ms.author: chugu
ms.custom: seo-lt-2019
ms.openlocfilehash: f11a70a1118fcfc2b6393d8b204ac7b1b8250d0b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352068"
---
# <a name="execute-sql-server-agent-job-task"></a>Tâche Exécuter le travail de l'Agent SQL Server

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  La tâche Exécuter le travail de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] exécute des travaux de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent est un service Windows [!INCLUDE[msCoName](../../includes/msconame-md.md)] qui exécute des travaux définis dans une instance de SQL Server. Vous pouvez créer des travaux qui exécutent des instructions Transact-SQL et des scripts ActiveX, réalisent des tâches de maintenance des services [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] et de réplication ou exécutent des packages. Vous pouvez également configurer une tâche pour surveiller [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et déclencher des alertes. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Les travaux de l’Agent sont généralement utilisés pour automatiser des tâches à caractère répétitif. Pour plus d’informations, consultez [Implémenter des travaux](../../ssms/agent/implement-jobs.md).  
  
 Grâce à la tâche Exécuter le travail de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , un package peut effectuer des tâches d'administration liées aux composants [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Par exemple, un travail d’Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut exécuter une procédure stockée système telle que **sp_enum_dtspackages** pour obtenir la liste des packages stockés dans un dossier.  
  
> [!NOTE]  
>  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent doit être en cours d’exécution pour que les travaux d’administration locaux ou multiserveurs puissent être exécutés automatiquement.  
  
 Cette tâche encapsule la procédure système **sp_start_job** et transmet le nom du travail d’Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à la procédure en guise d’argument. Pour plus d’informations, consultez [sp_start_job &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-start-job-transact-sql.md).  
  
## <a name="configuring-the-execute-sql-server-agent-job-task"></a>Configuration de la tâche d'exécution de travail d'Agent SQL Server  
 Vous pouvez définir les propriétés par le biais du concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] . Cette tâche se trouve dans la section **Tâches du plan de maintenance** de la **boîte à outils** du concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] .  
  
 Pour plus d'informations sur les propriétés définissables dans le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] , cliquez sur la rubrique suivante :  
  
-   [Tâche Exécuter le travail de l’Agent SQL Server &#40;Plan de maintenance&#41;](../../relational-databases/maintenance-plans/execute-sql-server-agent-job-task-maintenance-plan.md)  
  
 Pour plus d'informations sur la définition de ces propriétés dans le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] , cliquez sur la rubrique suivante :  
  
-   [Définir les propriétés d'une tâche ou d'un conteneur](./add-or-delete-a-task-or-a-container-in-a-control-flow.md)  
  
