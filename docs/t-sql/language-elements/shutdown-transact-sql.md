---
description: SHUTDOWN (Transact-SQL)
title: SHUTDOWN (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- SHUTDOWN_TSQL
- SHUTDOWN
dev_langs:
- TSQL
helpviewer_keywords:
- SQL Server, stopping
- shutting down SQL Server
- SHUTDOWN statement
- stopping SQL Server
- immediately stopping SQL Server
ms.assetid: c8b03ff9-688c-4fe8-86e8-bd6bd401c9a4
author: cawrites
ms.author: chadam
ms.openlocfilehash: 680652ceacfe82ca4f2372d082b790e066f2692f
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097029"
---
# <a name="shutdown-transact-sql"></a>SHUTDOWN (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Arrête immédiatement SQL Server.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
SHUTDOWN [ WITH NOWAIT ]   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 WITH NOWAIT  
 facultatif. Arrête [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sans générer de points de contrôle dans chaque base de données. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] s'arrête après avoir essayé de mettre un terme à tous les processus utilisateurs. Lorsque le serveur redémarre, il annule et restaure à l'état initial toutes les transactions incomplètes.  
  
## <a name="remarks"></a>Notes  
 À moins que l’option WITHNOWAIT ne soit utilisée, SHUTDOWN arrête [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en procédant ainsi :  
  
1.  Désactive les connexions (sauf pour les membres des rôles serveur fixes **sysadmin** et **serveradmin**).  
  
    > [!NOTE]  
    >  Pour obtenir la liste de tous les utilisateurs actuels, exécutez **sp_who**.  
  
2.  Attend que les instructions Transact-SQL ou les procédures stockées en cours d'exécution s'achèvent. Pour obtenir la liste de tous les processus et verrous actifs, exécutez respectivement **sp_who** et **sp_lock**.  
  
3.  Insert un point de contrôle dans chaque base de données.  
  
 L’utilisation de l’instruction SHUTDOWN minimise le travail de récupération automatique nécessaire lorsque des membres du rôle serveur fixe **sysadmin** redémarrent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Les outils et méthodes suivants peuvent également être utilisés pour arrêter [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Chacun d'entre eux génère un point de contrôle dans toutes les bases de données. Vous pouvez vider les données validées du cache de données et arrêter le serveur :  
  
-   à l'aide du Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ;  
  
-   En exécutant **net stop mssqlserver** à partir d’une invite de commandes pour une instance par défaut, ou en exécutant **net stop mssql$** _nom_instance_ à partir d’une invite de commandes pour une instance nommée.  
  
-   à l'aide des Services du Panneau de configuration ;  
  
 Si **sqlservr.exe** a été lancé à partir d’une invite de commandes, appuyez sur Ctrl+C pour arrêter [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Sachez toutefois que Ctrl+C ne génère pas de point de contrôle.  
  
> [!NOTE]  
>  Quelle que soit la méthode utilisée pour arrêter [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], le message `SERVICE_CONTROL_STOP` est envoyé à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="permissions"></a>Autorisations  
 Les autorisations SHUTDOWN sont attribuées aux membres des rôles serveur fixes **sysadmin** et **serveradmin**, et ne peuvent pas être transférées.  
  
## <a name="see-also"></a>Voir aussi  
 [CHECKPOINT &#40;Transact-SQL&#41;](../../t-sql/language-elements/checkpoint-transact-sql.md)   
 [sp_lock &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-lock-transact-sql.md)   
 [sp_who &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-who-transact-sql.md)   
 [Application sqlservr](../../tools/sqlservr-application.md)   
 [Démarrer, arrêter, suspendre, reprendre, redémarrer le moteur de base de données, SQL Server Agent ou le service SQL Server Browser](../../database-engine/configure-windows/start-stop-pause-resume-restart-sql-server-services.md)  
  
  
