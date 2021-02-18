---
description: Créer des travaux
title: Créer des travaux
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- jobs [SQL Server Agent], creating
- SQL Server Agent jobs, creating
ms.assetid: 465fb7fc-7622-4252-a178-ea51691c935b
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 4a79b08a37c6fb21f8a1e6c1b3f16466c15d135b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342726"
---
# <a name="create-jobs"></a>Créer des travaux
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> Dans [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance), la plupart, mais pas toutes les fonctionnalités SQL Server Agent sont actuellement prises en charge. Pour plus d’informations, consultez [Différences T-SQL entre Azure SQL Managed Instance et SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

Un travail est constitué d'une série d'opérations spécifiques exécutées de manière séquentielle par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent. Un travail peut effectuer diverses activités, notamment exécuter des scripts [!INCLUDE[tsql](../../includes/tsql-md.md)] , des applications d'invite de commandes, des scripts Microsoft ActiveX, des packages Integration Services, des commandes et des requêtes Analysis Services ou des tâches de réplication. Les travaux peuvent exécuter des tâches répétitives ou planifiables, et même notifier automatiquement les utilisateurs de l'état d'un travail en déclenchant des alertes, ce qui simplifie de manière significative l'administration de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
Pour créer un travail, l'utilisateur doit être membre de l'un des rôles de base de données fixes de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou du rôle de serveur fixe **sysadmin** . Un travail ne peut être modifié que par son propriétaire ou par les membres du rôle **sysadmin** . Les membres du rôle **sysadmin** peuvent attribuer la propriété du travail à d’autres utilisateurs et peuvent exécuter n’importe quel travail, quel qu’en soit le propriétaire. Pour plus d’informations sur les rôles de base de données fixe de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent, consultez [Rôles de base de données fixe de SQL Server Agent](../../ssms/agent/sql-server-agent-fixed-database-roles.md).  
  
Les travaux peuvent être écrits de manière à s'exécuter sur l'instance locale de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou sur plusieurs instances à l'échelle d'une entreprise. Pour exécuter des travaux sur plusieurs serveurs, vous devez configurer au moins un serveur maître et un ou plusieurs serveurs cibles. Pour plus d’informations sur les serveurs maîtres et cibles, consultez [Administration automatisée à l’échelle d’une entreprise](../../ssms/agent/automated-administration-across-an-enterprise.md).  
  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent enregistre dans l’historique des travaux les informations relatives aux travaux et aux étapes de travail.  
  
## <a name="related-tasks"></a>Tâches associées  
  
|Description|Rubrique|  
|-|-|  
|Explique comment créer un travail de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|[Créer un travail](../../ssms/agent/create-a-job.md)|  
|Explique comment réattribuer la propriété des travaux de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à un autre utilisateur.|[Attribuer la propriété d'un travail à d'autres utilisateurs](../../ssms/agent/give-others-ownership-of-a-job.md)|  
|Décrit la façon de définir le journal de l'historique des travaux [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|[Set Up the Job History Log](../../ssms/agent/set-up-the-job-history-log.md)|  
  
## <a name="see-also"></a>Voir aussi  
[Gérer les étapes de travail](../../ssms/agent/manage-job-steps.md)  
[Administration automatisée à l'échelle d'une entreprise](../../ssms/agent/automated-administration-across-an-enterprise.md)  
[Créer des planifications et les attacher à des travaux](../../ssms/agent/create-and-attach-schedules-to-jobs.md)  
[Exécuter des travaux](../../ssms/agent/run-jobs.md)  
[Afficher ou modifier les travaux](../../ssms/agent/view-or-modify-jobs.md)  
