---
description: Set Job Step Success or Failure Flow
title: Set Job Step Success or Failure Flow
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Agent jobs, action flow logic
- successful jobs [SQL Server]
- failed jobs [SQL Server]
- jobs [SQL Server Agent], action flow logic
ms.assetid: 23041ccf-8a07-41d3-85b9-c449a54b7e1e
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: c5556b9348d705606748e9f2b45b1eef57df2e6d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354856"
---
# <a name="set-job-step-success-or-failure-flow"></a>Set Job Step Success or Failure Flow
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> Dans [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance), la plupart, mais pas toutes les fonctionnalités SQL Server Agent sont actuellement prises en charge. Pour plus d’informations, consultez [Différences T-SQL entre Azure SQL Managed Instance et SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

Lors de la création de travaux de [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent, vous pouvez spécifier l’action [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que doit effectuer si un échec survient pendant l’exécution du travail. Déterminez l'action que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doit effectuer en cas de réussite ou d'échec de chaque étape de travail. Procédez ensuite comme suit pour définir le déroulement logique des actions de l'étape de travail en utilisant l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
-   **Avant de commencer :**  
  
    [Sécurité](#Security)  
  
-   **Pour définir un flux en cas de réussite ou d'échec de l'étape de travail, utilisez :**  
  
    [SQL Server Management Studio](#SSMS)  
  
    [Transact-SQL](#TSQL)  
  
    [SQL Server Management Objects](#SMO)  
  
## <a name="before-you-begin"></a>Avant de commencer  
  
### <a name="security"></a><a name="Security"></a>Sécurité  
Pour plus d'informations, consultez [Implémenter la sécurité de SQL Server Agent](../../ssms/agent/implement-sql-server-agent-security.md).  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMS"></a>Utilisation de SQL Server Management Studio  
  
#### <a name="to-set-job-step-success-or-failure-flow"></a>Pour définir un flux en cas de réussite ou d'échec de l'étape de travail  
  
1.  Dans l' **Explorateur d'objets**, développez **Agent SQL Server**, puis **Travaux**.  
  
2.  Cliquez avec le bouton droit sur le travail à modifier, puis cliquez sur **Propriétés**.  
  
3.  Cliquez sur la page **Étapes** , puis sur une étape et enfin sur **Modifier**.  
  
4.  Dans la boîte de dialogue **Propriétés de l'étape de travail** , cliquez sur la page **Avancé** .  
  
5.  Dans la boîte de dialogue **Action en cas de succès**, cliquez sur l’action à exécuter si l’étape de travail réussit.  
  
6.  Dans la zone **Tentatives de reprises** , saisissez le nombre de fois (entre 0 et 9999) qu'il faut répéter l'étape de travail avant de la considérer comme un échec. Si vous tapez une valeur supérieure à 0 dans la zone **Tentatives de reprises** , entrez dans la zone **Intervalle de reprise (minutes)** , le nombre de minutes d’attente, compris entre 1 et 9999, avant que le travail fasse l’objet d’un nouvel essai.  
  
7.  Dans la liste **Action en cas d'échec** , cliquez sur l'action à exécuter si l'étape de travail échoue.  
  
8.  Si le travail est un script [!INCLUDE[tsql](../../includes/tsql-md.md)] , vous pouvez choisir les options suivantes :  
  
    -   Dans la zone **Fichier de sortie** , tapez le nom du fichier de sortie dans lequel la sortie du script sera écrite. Par défaut, les données du fichier sont remplacées chaque fois que l'étape de travail s'exécute. Si vous ne voulez pas remplacer les données, activez **Ajouter la sortie au fichier existant**.  
  
    -   Activez **Enregistrer un journal dans la table** pour enregistrer l'étape de travail dans une table de base de données. Par défaut, le contenu de la table est remplacé chaque fois que l'étape de travail s'exécute. Si vous ne voulez pas remplacer les données, activez **Ajouter la sortie à l'entrée existante dans la table**. Une fois l'étape de travail exécutée, vous pouvez afficher le contenu de la table en cliquant sur **Afficher**.  
  
    -   Activez **Inclure la sortie de l'étape dans l'historique** pour inclure la sortie dans l'historique de l'étape. Le résultat ne sera affiché que s'il n'y a pas d'erreur. De même, le résultat peut être tronqué.  
  
9. Si la liste **Exécuter en tant qu'utilisateur** est disponible, sélectionnez le compte proxy ayant les informations d'identification que le travail utilisera.  
  
## <a name="using-transact-sql"></a><a name="TSQL"></a>Utilisation de Transact-SQL  
  
#### <a name="to-set-job-step-success-or-failure-flow"></a>Pour définir un flux en cas de réussite ou d'échec de l'étape de travail  
  
1.  Dans l' **Explorateur d'objets**, connectez-vous à une instance du [!INCLUDE[ssDE](../../includes/ssde_md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**.  
  
    ```  
    USE msdb;  
    GO  
    EXEC sp_add_jobstep  
        @job_name = N'Weekly Sales Data Backup',  
        @step_name = N'Set database to read only',  
        @subsystem = N'TSQL',  
        @command = N'ALTER DATABASE SALES SET READ_ONLY',   
        @on_success_action = 1;  
    GO  
    ```  
  
Pour plus d’informations, consultez [sp_add_jobstep (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-add-jobstep-transact-sql.md).  
  
## <a name="using-sql-server-management-objects"></a><a name="SMO"></a>Utilisation de SQL Server Management Objects  
**Pour définir un flux en cas de réussite ou d'échec de l'étape de travail**  
  
Utilisez la classe **JobStep** à l’aide du langage de programmation de votre choix, tel que Visual Basic, Visual C# ou PowerShell. Pour plus d’informations, consultez [SQL Server Management Objects (SMO)](../../relational-databases/server-management-objects-smo/sql-server-management-objects-smo-programming-guide.md).  
