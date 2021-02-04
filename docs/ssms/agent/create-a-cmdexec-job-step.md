---
description: Créer une étape de travail CmdExec
title: Créer une étape de travail CmdExec
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- CmdExec jobs
ms.assetid: b48da5b4-6fe7-4eb7-bade-dc7d697c6d5c
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: e3b29f00dadcc29d8a790b36c7f0da24058a67f9
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237165"
---
# <a name="create-a-cmdexec-job-step"></a>Créer une étape de travail CmdExec
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> Dans [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance), la plupart, mais pas toutes les fonctionnalités SQL Server Agent sont actuellement prises en charge. Pour plus d’informations, consultez [Différences T-SQL entre Azure SQL Managed Instance et SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

Cette rubrique explique comment créer et définir une étape de travail de [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] qui utilise un programme exécutable ou une commande de système d’exploitation, à l’aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../includes/tsql-md.md)] ou SQL Server Management Objects.  
  
## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>Avant de commencer  
  
### <a name="security"></a><a name="Security"></a>Sécurité  
Par défaut, seuls les membres du rôle de serveur fixe **sysadmin** peuvent créer des étapes de travail CmdExec. Ces étapes de travail s'exécutent sous le contexte du compte de service SQL Server Agent à moins que l'utilisateur **sysadmin** crée un compte proxy. Les utilisateurs qui ne sont pas membres du rôle **sysadmin** peuvent créer des étapes de travail CmdExec s'ils peuvent accéder à un compte proxy CmdExec.  
  
#### <a name="permissions"></a><a name="Permissions"></a>Autorisations  
Pour plus d'informations, consultez [Implémenter la sécurité de SQL Server Agent](../../ssms/agent/implement-sql-server-agent-security.md).  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMS"></a>Utilisation de SQL Server Management Studio  
  
#### <a name="to-create-a-cmdexec-job-step"></a>Pour créer une étape de travail CmdExec  
  
1.  Dans **l’Explorateur d'objets** , connectez-vous à une instance du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion_md.md)]et développez-la.  
  
2.  Développez **SQL Server Agent**, créez un travail ou cliquez avec le bouton droit de la souris sur un travail existant, puis cliquez sur **Propriétés**.  
  
3.  Dans la boîte de dialogue **Propriétés du travail** , cliquez sur la page **Étapes** , puis sur **Nouveau**.  
  
4.  Dans la boîte de dialogue **Nouvelle étape du travail** , tapez un **nom d'étape** de travail.  
  
5.  Dans la liste **Type** , choisissez **Système d’exploitation (CmdExec)** .  
  
6.  Dans la liste **Exécuter en tant que** , sélectionnez le compte proxy avec les informations d'identification que doit utiliser le travail. Par défaut, les étapes de travail CmdExec s'exécutent dans le contexte du compte de service SQL Server Agent.  
  
7.  Dans la zone **Traiter le code de sortie d'une commande réussie** , entrez une valeur comprise entre 0 et 999999.  
  
8.  Dans la zone **Commande** , saisissez la commande du système d'exploitation ou le programme exécutable. Consultez « Utilisation de Transact T-SQL » pour obtenir un exemple.  
  
9. Cliquez sur la page **Avancé** pour définir les options d'étape de travail, telles que l'action à exécuter lorsque l'étape de travail aboutit ou échoue, le nombre de tentatives d'exécution de l'étape de travail que doit effectuer l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et le fichier où [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut écrire la sortie de l'étape de travail. Seuls les membres du rôle de serveur fixe **sysadmin** peuvent écrire une sortie d'étape de travail dans un fichier du système d'exploitation.  
  
## <a name="using-transact-sql"></a><a name="TSQL"></a>Utilisation de Transact-SQL  
  
#### <a name="to-create-a-cmdexec-job-step"></a>Pour créer une étape de travail CmdExec  
  
1.  Dans l' **Explorateur d'objets**, connectez-vous à une instance du [!INCLUDE[ssDE](../../includes/ssde_md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**.  
  
    ```  
    -- creates a job step that uses CmdExec  
    USE msdb;  
    GO  
    EXEC sp_add_jobstep  
        @job_name = N'Weekly Sales Data Backup',  
        @step_name = N'Set database to read only',  
        @subsystem = N'CMDEXEC',  
        @command = 'C:\clickme_scripts\SQL11\PostBOLReorg GetHsX.exe',   
        @retry_attempts = 5,  
        @retry_interval = 5 ;  
    GO  
    ```  
  
Pour plus d’informations, consultez [sp_add_jobstep (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-add-jobstep-transact-sql.md).  
  
## <a name="using-sql-server-management-objects"></a><a name="SMO"></a>Utilisation de SQL Server Management Objects  
**Pour créer une étape de travail CmdExec**  
  
Utilisez la classe **JobStep** à l’aide du langage de programmation de votre choix, tel que Visual Basic, Visual C# ou PowerShell.  
