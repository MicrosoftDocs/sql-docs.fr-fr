---
description: Tables Agent SQL Server (Transact-SQL)
title: Tables SQL Server Agent (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- SQL Server Agent, system tables
- system tables [SQL Server], SQL Server Agent
ms.assetid: 6cb39bfd-079e-4be4-9c42-2fa234c65ce1
author: cawrites
ms.author: chadam
ms.openlocfilehash: f606e046f09254c3beb9c933a04640eea78990ee
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191126"
---
# <a name="sql-server-agent-tables-transact-sql"></a>Tables Agent SQL Server (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Les rubriques qui suivent décrivent les tables système qui stockent des informations utilisées par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Toutes les tables correspondent au schéma dbo dans la base de données msdb.  
  
## <a name="in-this-section"></a>Dans cette section  
 [dbo.sysalerts](../../relational-databases/system-tables/dbo-sysalerts-transact-sql.md)  
 Contient une ligne pour chaque alerte.  
  
 [dbo.syscategories](../../relational-databases/system-tables/dbo-syscategories-transact-sql.md)  
 Contient les catégories utilisées par [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] pour organiser les travaux, les alertes et les opérateurs.  
  
 [dbo.sysdownloadlist](../../relational-databases/system-tables/dbo-sysdownloadlist-transact-sql.md)  
 Contient la file d'attente des instructions de téléchargement pour l'ensemble des serveurs cibles.  
  
 [dbo.sysjobactivity](../../relational-databases/system-tables/dbo-sysjobactivity-transact-sql.md)  
 Contient des informations sur l'activité et l'état du travail en cours de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 [dbo.sysjobhistory](../../relational-databases/system-tables/dbo-sysjobhistory-transact-sql.md)  
 Contient des informations sur l'exécution des travaux planifiés par l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 [dbo.sysjobs](../../relational-databases/system-tables/dbo-sysjobs-transact-sql.md)  
 Stocke les informations de chaque travail planifié qui doit être exécuté par l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 [dbo.sysjobschedules](../../relational-databases/system-tables/dbo-sysjobschedules-transact-sql.md)  
 Contient des informations de planification pour les travaux qui doivent être exécutés par l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
  
 [dbo.sysjobservers](../../relational-databases/system-tables/dbo-sysjobservers-transact-sql.md)  
 Stocke les associations ou les relations existant entre un travail particulier et un ou plusieurs serveurs cibles.  
  
 [dbo.sysjobsteps](../../relational-databases/system-tables/dbo-sysjobsteps-transact-sql.md)  
 Contient des informations sur chaque étape d'un travail devant être exécuté par l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 [dbo.sysjobstepslogs](../../relational-databases/system-tables/dbo-sysjobstepslogs-transact-sql.md)  
 Contient des informations sur les journaux d'étape de travail.  
  
 [dbo.sysnotifications](../../relational-databases/system-tables/dbo-sysnotifications-transact-sql.md)  
 Contient une ligne pour chaque notification.  
  
 [dbo.sysoperators](../../relational-databases/system-tables/dbo-sysoperators-transact-sql.md)  
 Contient une ligne pour chaque opérateur de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 [dbo.sysproxies](../../relational-databases/system-tables/dbo-sysproxies-transact-sql.md)  
 Contient des informations sur les comptes proxy de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 [dbo.sysproxylogin](../../relational-databases/system-tables/dbo-sysproxylogin-transact-sql.md)  
 Enregistre les connexions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] associées à chaque compte proxy de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 [dbo.sysproxysubsystem](../../relational-databases/system-tables/dbo-sysproxysubsystem-transact-sql.md)  
 Enregistre le sous-système de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilisé par chaque compte proxy.  
  
 [dbo.sysschedules](../../relational-databases/system-tables/dbo-sysschedules-transact-sql.md)  
 Contient des informations sur les planifications de travail de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 [dbo.syssessions](../../relational-databases/system-tables/dbo-syssessions-transact-sql.md)  
 Contient la date de début de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour chaque session de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Une session est créée à chaque démarrage du service de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 [dbo.syssubsystems](../../relational-databases/system-tables/dbo-sysproxysubsystem-transact-sql.md)  
 Contient des informations sur tous les sous-systèmes proxy de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] disponibles.  
  
 [dbo.systargetservergroupmembers](../../relational-databases/system-tables/dbo-systargetservergroupmembers-transact-sql.md)  
 Enregistre les serveurs cibles qui sont actuellement inscrits dans ce groupe multiserveur.  
  
 [dbo.systargetservergroups](../../relational-databases/system-tables/dbo-systargetservergroups-transact-sql.md)  
 Enregistre les groupes de serveurs cibles qui font actuellement partie de la liste de l'environnement multiserveur.  
  
 [dbo.systargetservers](../../relational-databases/system-tables/dbo-systargetservers-transact-sql.md)  
 Enregistrements dont les serveurs cibles font actuellement partie de la liste du domaine d'exploitation multiserveurs.  
  
 [dbo.systaskids](../../relational-databases/system-tables/dbo-systaskids-transact-sql.md)  
 Contient un mappage des tâches créées dans des versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vers des travaux [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] dans la version actuelle.  
  
  
