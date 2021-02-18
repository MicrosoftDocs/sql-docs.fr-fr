---
description: Journal des erreurs de SQL Server Agent
title: Journal des erreurs de SQL Server Agent
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- logs [SQL Server], SQL Server Agent
- messages [SQL Server], SQL Server Agent
- errors [SQL Server], logs
- SQL Server Agent, errors
ms.assetid: 0b2d6e6e-cd2d-4b8b-9fa2-2bbd2fc0da41
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 01/19/2017
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 00924a495f67313824bdd9982975814c1bd460f7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344243"
---
# <a name="sql-server-agent-error-log"></a>Journal des erreurs de SQL Server Agent

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> Dans [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance), la plupart, mais pas toutes les fonctionnalités SQL Server Agent sont actuellement prises en charge. Pour plus d’informations, consultez [Différences T-SQL entre Azure SQL Managed Instance et SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent crée un journal des erreurs qui, par défaut, enregistre les avertissements et les erreurs. Le journal contient :  
  
-   des messages d'avertissement qui contiennent des informations relatives à des problèmes potentiels, tels que « Le travail \<*job_name*> a été supprimé en cours d'exécution ».  
  
-   Des messages d'erreur dont la solution nécessite habituellement l'intervention d'un administrateur système, tels que « Impossible de démarrer la session de messagerie ». Les messages d’erreur peuvent être envoyés à un utilisateur ou un ordinateur spécifique via **net send**.  
  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] conserve jusqu’à neuf journaux des erreurs [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent. Chaque journal des erreurs archivé possède un suffixe indiquant son ancienneté relative. Par exemple, le suffixe .1 signale le journal des erreurs le plus récemment archivé alors que le suffixe .9 indique qu'il s'agit du plus ancien journal des erreurs existant.  
  
Par défaut, les messages de trace d'exécution ne sont pas écrits dans le journal d'erreurs [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent, parce qu'ils risquent de le remplir. Lorsque le fichier d'erreurs est plein, votre possibilité de sélectionner et analyser les erreurs plus difficiles se trouve réduite. Le journal étant une charge supplémentaire de travail pour le serveur, il est important de bien réfléchir à ce que la capture de messages de trace d’exécution dans le journal des erreurs vous apporte. Il est en général préférable de capturer tous les messages uniquement au moment de corriger un problème donné.  
  
Lorsque [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent n'est pas activé, vous pouvez modifier l'emplacement du journal des erreurs [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent. Lorsque le journal est vide, il ne peut pas être ouvert. Vous pouvez parcourir le journal de l’Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à tout moment sans devoir l’arrêter [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] avec [dbo.sp_cycle_agent_errorlog](../../relational-databases/system-stored-procedures/sp-cycle-agent-errorlog-transact-sql.md).  
  
**Pour afficher le journal des erreurs SQL Server Agent**  
  
-   [Afficher le journal des erreurs SQL Server Agent &#40;SQL Server Management Studio&#41;](../../ssms/agent/view-sql-server-agent-error-log-sql-server-management-studio.md)  
  
**Pour renommer le journal des erreurs SQL Server Agent**  
  
-   [Renommer un journal d’erreurs SQL Server Agent &#40;SQL Server Management Studio&#41;](../../ssms/agent/rename-a-sql-server-agent-error-log-sql-server-management-studio.md)  
  
**Pour envoyer des messages d'erreur SQL Server Agent**  
  
-   [Envoyer des messages d'erreur SQL Server Agent](../../ssms/agent/send-sql-server-agent-error-messages.md)  
  
**Pour écrire des messages de trace d'exécution dans le journal des erreurs SQL Server Agent**  
  
-   [Écrire des messages de trace d’exécution dans le journal des erreurs SQL Server Agent &#40;SQL Server Management Studio&#41;](../../ssms/agent/write-execution-trace-messages-to-sql-server-agent-log-ssms.md)  
