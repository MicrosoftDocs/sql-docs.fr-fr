---
title: Créer et exécuter des tâches pour SQL Server sur Linux
description: Découvrez comment créer une tâche SQL Server Agent sur Linux à l’aide de Transact-SQL et SQL Server Management Studio (SSMS).
author: VanMSFT
ms.author: vanto
ms.date: 02/20/2018
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 1d93d95e-9c89-4274-9b3f-fa2608ec2792
ms.openlocfilehash: bc52f0a2052d0b9abd1700e92af3a30c0e02bad2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346442"
---
# <a name="create-and-run-sql-server-agent-jobs-on-linux"></a>Créer et exécuter des tâches SQL Server Agent sur Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Les tâches SQL Server sont utilisées pour effectuer régulièrement la même séquence de commandes dans votre base de données SQL Server. Ce didacticiel fournit un exemple de création d’une tâche SQL Server Agent sur Linux à l’aide de Transact-SQL et SQL Server Management Studio (SSMS).

> [!div class="checklist"]
> * Installer SQL Server Agent sur Linux
> * Créer une nouvelle tâche pour effectuer des sauvegardes de données quotidiennes
> * Planifier et exécuter la tâche
> * Effectuer les mêmes étapes dans SSMS (facultatif)

Pour les problèmes connus avec SQL Server Agent sur Linux, consultez les [Notes de publication](sql-server-linux-release-notes.md).

## <a name="prerequisites"></a>Conditions préalables requises

Pour exécuter ce didacticiel, vous devez réunir les conditions préalables suivantes :

* Machine Linux avec les conditions préalables suivantes :
  * SQL Server ([RHEL](quickstart-install-connect-red-hat.md), [SLES](quickstart-install-connect-suse.md) ou [Ubuntu](quickstart-install-connect-ubuntu.md)) avec les outils de ligne de commande.

Les conditions préalables suivantes sont facultatives :

* Machine Windows avec SSMS :
  * [SQL Server Management Studio](../ssms/download-sql-server-management-studio-ssms.md) pour les étapes SSMS facultatives.

## <a name="enable-sql-server-agent"></a>Activer SQL Server Agent

Pour utiliser SQL Server Agent sur Linux, vous devez avant tout activer SQL Server Agent sur une machine avec SQL Server déjà installé.

1. Pour activer SQL Server Agent, procédez comme suit.
  ```bash
  sudo /opt/mssql/bin/mssql-conf set sqlagent.enabled true 
  ```

1. Redémarrez SQL Server avec la commande suivante :
  ```bash
  sudo systemctl restart mssql-server
  ```

> [!NOTE]
> En commençant par SQL Server 2017 CU4, SQL Server Agent est inclus avec le package **mssql-server** et est désactivé par défaut. Pour la configuration de l’Agent avant CU4, visitez [Installer SQL Server Agent sur Linux](sql-server-linux-setup-sql-agent.md).

## <a name="create-a-sample-database"></a>Créer un exemple de base de données

Suivez les étapes suivantes pour créer un exemple de base de données nommé **SampleDB**. Cette base de données est utilisée pour la tâche de sauvegarde quotidienne.

1. Sur votre machine Linux, ouvrez une session du terminal Bash.

1. Utilisez **sqlcmd** pour exécuter une commande Transact-SQL **CREATE DATABASE**.

   ```bash
   /opt/mssql-tools/bin/sqlcmd -S localhost -U SA -Q 'CREATE DATABASE SampleDB'
   ```

1. Vérifiez la création de la base de données en répertoriant les bases de données sur votre serveur.

   ```bash
   /opt/mssql-tools/bin/sqlcmd -S localhost -U SA -Q 'SELECT Name FROM sys.Databases'
   ```

## <a name="create-a-job-with-transact-sql"></a>Créer une tâche avec Transact-SQL

Les tâches suivantes permettent de créer une tâche SQL Server Agent sur Linux avec les commandes Transact-SQL. La tâche exécute une sauvegarde quotidienne de l’exemple de base de données **SampleDB**.

> [!TIP]
> Vous pouvez utiliser tout client T-SQL pour exécuter ces commandes. Par exemple, sur Linux, vous pouvez utiliser [sqlcmd](sql-server-linux-setup-tools.md) ou [Visual Studio Code](../tools/visual-studio-code/sql-server-develop-use-vscode.md). À partir de Windows Server distant, vous pouvez également exécuter des requêtes dans SQL Server Management Studio (SSMS) ou utiliser l’interface IU pour la gestion des tâches, décrite dans la section suivante.

1. Utilisez [sp_add_job](../relational-databases/system-stored-procedures/sp-add-job-transact-sql.md) pour créer une tâche nommée `Daily SampleDB Backup`.

   ```sql
   -- Adds a new job executed by the SQLServerAgent service
   -- called 'Daily SampleDB Backup'
   USE msdb ;
   GO
   EXEC dbo.sp_add_job
      @job_name = N'Daily SampleDB Backup' ;
   GO
   ```

1. Appelez [sp_add_jobstep](../relational-databases/system-stored-procedures/sp-add-jobstep-transact-sql.md) pour créer une étape de tâche qui crée une sauvegarde de la base de données`SampleDB`.

   ```sql
   -- Adds a step (operation) to the job
   EXEC sp_add_jobstep
      @job_name = N'Daily SampleDB Backup',
      @step_name = N'Backup database',
      @subsystem = N'TSQL',
      @command = N'BACKUP DATABASE SampleDB TO DISK = \
         N''/var/opt/mssql/data/SampleDB.bak'' WITH NOFORMAT, NOINIT, \
         NAME = ''SampleDB-full'', SKIP, NOREWIND, NOUNLOAD, STATS = 10',
      @retry_attempts = 5,
      @retry_interval = 5 ;
   GO
   ```

1. Créez ensuite une planification quotidienne pour votre tâche avec [sp_add_schedule](../relational-databases/system-stored-procedures/sp-add-jobschedule-transact-sql.md).

   ```sql
   -- Creates a schedule called 'Daily'
   EXEC dbo.sp_add_schedule
      @schedule_name = N'Daily SampleDB',
      @freq_type = 4,
      @freq_interval = 1,
      @active_start_time = 233000 ;
   USE msdb ;
   GO
   ```

1. Joignez la planification des tâches à la tâche avec [sp_attach_schedule](../relational-databases/system-stored-procedures/sp-attach-schedule-transact-sql.md).

   ```sql
   -- Sets the 'Daily' schedule to the 'Daily SampleDB Backup' Job
   EXEC sp_attach_schedule
      @job_name = N'Daily SampleDB Backup',
      @schedule_name = N'Daily SampleDB';
   GO
   ```

1. Utilisez [sp_add_jobserver](../relational-databases/system-stored-procedures/sp-add-jobserver-transact-sql.md) pour attribuer la tâche à un serveur cible. Dans cet exemple, la cible est le serveur local.

   ```sql
   EXEC dbo.sp_add_jobserver
      @job_name = N'Daily SampleDB Backup',
      @server_name = N'(LOCAL)';
   GO
   ```
1. Démarrez la tâche par [sp_start_job](../relational-databases/system-stored-procedures/sp-start-job-transact-sql.md).

   ```sql
   EXEC dbo.sp_start_job N' Daily SampleDB Backup' ;
   GO
   ```

## <a name="create-a-job-with-ssms"></a>Créer une tâche avec SSMS

Vous pouvez également créer et gérer des tâches à distance à l’aide de SQL Server Management Studio (SSMS) sur Windows.

1. Démarrez SSMS sur Windows et connectez-vous à votre instance Linux. Pour plus d’informations, consultez [Gérer SQL Server sur Linux avec SSMS](sql-server-linux-manage-ssms.md).

1. Vérifiez que vous avez bien créé un exemple de base de données nommé **SampleDB**.

   <img src="./media/sql-server-linux-run-sql-server-agent-job/ssms-agent-0.png" alt="Create a SampleDB database" style="width: 550px;"/>

1. Vérifiez que l'Agent SQL est [installé](sql-server-linux-setup-sql-agent.md) et configuré correctement. Recherchez le signe plus à côté de SQL Server Agent dans l’Explorateur d'objets. Si SQL Server Agent n’est pas activé, essayez de redémarrer le service **mssql-server** sur Linux.

   ![Vérifier que SQL Server Agent est installé](./media/sql-server-linux-run-sql-server-agent-job/ssms-agent-1.png)

1. Créez une nouvelle tâche.

   ![Créer une nouvelle tâche](./media/sql-server-linux-run-sql-server-agent-job/ssms-agent-2.png)

1. Donnez un nom à votre tâche et créez l’étape de votre tâche.

   ![Créer une étape de la tâche](./media/sql-server-linux-run-sql-server-agent-job/ssms-agent-3.png)

1. Spécifiez quel sous-système et quelle étape de la tâche utiliser.

   ![Sous-système de la tâche](./media/sql-server-linux-run-sql-server-agent-job/ssms-agent-4.png)

   ![Action de l’étape de la tâche](./media/sql-server-linux-run-sql-server-agent-job/ssms-agent-5.png)

1. Créez une nouvelle planification de tâches.

   ![Capture d’écran de la boîte de dialogue Nouvelle tâche, avec l’option Planifications et l’option Nouveau mises en évidence.](./media/sql-server-linux-run-sql-server-agent-job/ssms-agent-6.png)

   ![Capture d’écran de la boîte de dialogue Nouvelle tâche, avec l’option OK mise en évidence.](./media/sql-server-linux-run-sql-server-agent-job/ssms-agent-8.png)

1. Démarrez votre tâche.

   <img src="./media/sql-server-linux-run-sql-server-agent-job/ssms-agent-9.png" alt="Start the SQL Server Agent job" style="width: 550px;"/>

## <a name="next-steps"></a>Étapes suivantes

Dans ce didacticiel, vous avez appris à :

> [!div class="checklist"]
> * Installer SQL Server Agent sur Linux
> * utiliser Transact-SQL et les procédures stockées système pour créer des tâches
> * créer une tâche qui exécute des sauvegardes de données quotidiennes
> * utiliser l’IU SSMS pour créer et gérer des tâches

Par la suite, explorez d’autres fonctionnalités pour créer et gérer des tâches :

> [!div class="nextstepaction"]
>[Documentation SQL Server Agent](../ssms/agent/sql-server-agent.md)