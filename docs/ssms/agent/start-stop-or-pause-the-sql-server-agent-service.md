---
description: Start, Stop, or Pause the SQL Server Agent Service
title: Démarrer, arrêter ou suspendre le service
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Agent, starting
- SQL Server Agent, pausing
- SQL Server Agent, stopping
ms.assetid: c95a9759-dd30-4ab6-9ab0-087bb3bfb97c
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 01/19/2017
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 7e8c1c619c6513ff2863489720d065eec427dc93
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/25/2021
ms.locfileid: "98765764"
---
# <a name="start-stop-or-pause-the-sql-server-agent-service"></a>Start, Stop, or Pause the SQL Server Agent Service

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> Dans [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance), la plupart, mais pas toutes les fonctionnalités SQL Server Agent sont actuellement prises en charge. Pour plus d’informations, consultez [Différences T-SQL entre Azure SQL Managed Instance et SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

Cette rubrique explique comment démarrer, arrêter ou redémarrer le service de l'Agent SQL Server dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
Vous pouvez configurer le service l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour qu'il démarre automatiquement lorsque le système d'exploitation démarre, ou vous pouvez le démarrer manuellement lorsque vous avez besoin d'exécuter des travaux. Vous pouvez arrêter ou interrompre le service de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour interrompre les travaux, les notifications de l'opérateur et les alertes.  
  
## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>Avant de commencer  
  
### <a name="limitations-and-restrictions"></a><a name="Restrictions"></a>Limitations et restrictions  
  
-   [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent doit être exécuté en tant que service pour automatiser les tâches administratives. Pour plus d’informations, consultez [Configure SQL Server Agent](../../ssms/agent/configure-sql-server-agent.md).  
  
-   Cependant, l'Explorateur d'objets affiche le nœud de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent uniquement si vous avez l'autorisation de l'utiliser.  
  
### <a name="security"></a><a name="Security"></a>Sécurité  
  
#### <a name="permissions"></a><a name="Permissions"></a>Autorisations  
Pour exécuter ses fonctions, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent doit être configuré pour utiliser les informations d'identification d'un compte qui est membre du rôle serveur fixe **sysadmin** dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Le compte doit avoir les autorisations Windows suivantes :  
  
-   Ouvrir une session en tant que service (SeServiceLogonRight)  
  
-   Remplacer un jeton de niveau processus (SeAssignPrimaryTokenPrivilege)  
  
-   Outrepasser le contrôle de parcours (SeChangeNotifyPrivilege)  
  
-   Changer les quotas de mémoire d'un processus (SeIncreaseQuotaPrivilege)  
  
Pour plus d’informations sur les autorisations Windows nécessaires pour le compte de service de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent, consultez [Sélectionner un compte pour le service SQL Server Agent](../../ssms/agent/select-an-account-for-the-sql-server-agent-service.md) et [Setting Up Windows Service Accounts](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md)(Configuration des comptes de service Windows).  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a>Utilisation de SQL Server Management Studio  
  
#### <a name="to-start-stop-or-restart-the-sql-server-agent-service"></a>Pour démarrer, arrêter ou redémarrer le service de l'Agent SQL Server  
  
1.  Dans l' **Explorateur d'objets**, cliquez sur le signe plus (+) pour développer le serveur sur lequel vous souhaitez gérer le service de l'Agent SQL Server.  
  
2.  Cliquez avec le bouton droit sur **SQL Server Agent**, puis sélectionnez **Démarrer**, **Arrêter** ou **Redémarrer**.  
  
3.  Dans la boîte de dialogue **Contrôle de compte d’utilisateur**, cliquez sur **Oui**.  
  
4.  Lorsque vous y êtes invité, et si vous souhaitez exécuter l'action, cliquez sur **Oui**.  
  
Pour plus d'informations, consultez les pages suivantes :  
  
-   [Démarrer, arrêter, suspendre, reprendre, redémarrer le moteur de base de données, SQL Server Agent ou le service SQL Server Browser](../../database-engine/configure-windows/start-stop-pause-resume-restart-sql-server-services.md)  
  
-   [Démarrer automatiquement SQL Server Agent &#40;SQL Server Management Studio&#41;](../../ssms/agent/autostart-sql-server-agent-sql-server-management-studio.md)  
