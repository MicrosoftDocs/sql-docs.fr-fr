---
description: Définir l'arrêt de l'exécution d'un travail
title: Définir l'arrêt de l'exécution d'un travail
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- jobs [SQL Server Agent], stopping
- SQL Server Agent jobs, stopping
- stopping jobs
- SQL Server Agent jobs, execution shutdowns
ms.assetid: ac23e88f-53fc-41de-bb16-0c27c002d5a5
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 01/19/2017
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: feb58f3e88b36dfd24c0a09182df822b20c11d55
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/02/2021
ms.locfileid: "99251014"
---
# <a name="set-job-execution-shutdown"></a>Définir l'arrêt de l'exécution d'un travail

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> Dans [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance), la plupart, mais pas toutes les fonctionnalités SQL Server Agent sont actuellement prises en charge. Pour plus d’informations, consultez [Différences T-SQL entre Azure SQL Managed Instance et SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

Cette rubrique explique comment définir le délai pendant lequel [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent doit attendre la fin de l’exécution des travaux avant la fin de l’exécution de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent lui-même dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l’aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>Avant de commencer  
  
### <a name="security"></a><a name="Security"></a>Sécurité  
  
#### <a name="permissions"></a><a name="Permissions"></a>Autorisations  
Par défaut, les membres du rôle serveur fixe **sysadmin** peuvent définir la durée pendant laquelle [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent doit attendre la fin de l’exécution des travaux avant la fin de l’exécution de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent lui-même. Les autres utilisateurs doivent disposer de l'un des rôles de base de données fixes suivants de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent dans la base de données **msdb** :  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a>Utilisation de SQL Server Management Studio  
  
#### <a name="to-set-job-execution-shutdown"></a>Pour définir l'arrêt de l'exécution d'un travail  
  
1.  Dans **l’Explorateur d'objets,** , cliquez sur le signe plus pour développer le serveur sur lequel vous souhaitez définir un intervalle d'arrêt d'exécution d'un travail.  
  
2.  Cliquez avec le bouton droit sur **SQL Server Agent** , puis sélectionnez **Propriétés**.  
  
3.  Sous **Sélectionner une page**, sélectionnez **Système de travaux**.  
  
4.  Définissez **l’intervalle du délai d’arrêt** en secondes pour augmenter ou réduire l’intervalle d’arrêt. Cette valeur détermine le délai d'attente pendant lequel [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent attend la fin de l'exécution des travaux avant la fin de l'exécution de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent lui-même.  
