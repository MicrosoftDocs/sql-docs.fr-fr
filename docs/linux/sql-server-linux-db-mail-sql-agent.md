---
title: DB Mail et alertes de messagerie avec l’agent SQL sur Linux
description: Découvrez comment utiliser DB Mail et configurer les alertes par e-mail avec SQL Server Agent (mssql-server-agent) sur Linux.
author: VanMSFT
ms.author: vanto
ms.date: 02/20/2018
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: tbd
ms.openlocfilehash: 882ab5c8bff2acd7c93cc8cbeeba135152ce76ed
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348170"
---
# <a name="db-mail-and-email-alerts-with-sql-agent-on-linux"></a>DB Mail et alertes de messagerie avec l’agent SQL sur Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Les étapes suivantes vous montrent comment configurer DB Mail et l’utiliser avec SQL Server Agent (**mssql-server-agent**) sur Linux. 

## <a name="1-enable-db-mail"></a>1. Activer DB Mail

```sql
USE master 
GO 
sp_configure 'show advanced options',1 
GO 
RECONFIGURE WITH OVERRIDE 
GO 
sp_configure 'Database Mail XPs', 1 
GO 
RECONFIGURE  
GO  
```

## <a name="2-create-a-new-account"></a>2. Créer un nouveau compte
```sql
EXECUTE msdb.dbo.sysmail_add_account_sp 
@account_name = 'SQLAlerts', 
@description = 'Account for Automated DBA Notifications', 
@email_address = 'sqlagenttest@gmail.com', 
@replyto_address = 'sqlagenttest@gmail.com', 
@display_name = 'SQL Agent', 
@mailserver_name = 'smtp.gmail.com', 
@port = 587, 
@enable_ssl = 1, 
@username = 'sqlagenttest@gmail.com', 
@password = '<password>' 
GO
```

## <a name="3-create-a-default-profile"></a>3. Créer un profil par défaut

```sql
EXECUTE msdb.dbo.sysmail_add_profile_sp 
@profile_name = 'default', 
@description = 'Profile for sending Automated DBA Notifications' 
GO
```

## <a name="4-add-the-database-mail-account-to-a-database-mail-profile"></a>4. Ajouter le compte Database Mail à un profil Database Mail
```sql
EXECUTE msdb.dbo.sysmail_add_principalprofile_sp 
@profile_name = 'default', 
@principal_name = 'public', 
@is_default = 1 ; 
 ```
 
## <a name="5-add-account-to-profile"></a>5. Ajouter un compte au profil 
```sql
EXECUTE msdb.dbo.sysmail_add_profileaccount_sp   
@profile_name = 'default',   
@account_name = 'SQLAlerts',   
@sequence_number = 1;  
 ```
 
## <a name="6-send-test-email"></a>6. Envoyer un e-mail de test
> [!NOTE]
> Vous devrez peut-être accéder à votre client de messagerie et activer « Autoriser les clients moins sécurisés à envoyer des courriers ». Tous les clients ne reconnaissent pas DB Mail comme un démon de messagerie.

```
EXECUTE msdb.dbo.sp_send_dbmail 
@profile_name = 'default', 
@recipients = 'recipient-email@gmail.com', 
@Subject = 'Testing DBMail', 
@Body = 'This message is a test for DBMail' 
GO
```

## <a name="7-set-db-mail-profile-using-mssql-conf-or-environment-variable"></a>7. Définir le profil DB Mail à l’aide de la variable d’environnement mssql-conf
Vous pouvez utiliser l’utilitaire mssql-conf ou les variables d’environnement pour inscrire votre profil DB Mail. Dans ce cas, appelons notre profil par défaut.

```bash
# via mssql-conf
sudo /opt/mssql/bin/mssql-conf set sqlagent.databasemailprofile default
# via environment variable
MSSQL_AGENT_EMAIL_PROFILE=default
```

## <a name="8-set-up-an-operator-for-sqlagent-job-notifications"></a>8. Configurer un opérateur pour les notifications de travaux SQLAgent 

```sql
EXEC msdb.dbo.sp_add_operator 
@name=N'JobAdmins',  
@enabled=1, 
@email_address=N'recipient-email@gmail.com',  
@category_name=N'[Uncategorized]' 
GO 
```

## <a name="9-send-email-when-agent-test-job-succeeds"></a>9. Envoyer un e-mail lorsque le « travail de test de l’agent » a été correctement effectué 

```
EXEC msdb.dbo.sp_update_job 
@job_name='Agent Test Job', 
@notify_level_email=1, 
@notify_email_operator_name=N'JobAdmins' 
GO
```

## <a name="next-steps"></a>Étapes suivantes
Pour plus d’informations sur l’utilisation de SQL Server Agent pour créer, planifier et exécuter des tâches, consultez [Exécuter une tâche SQL Server Agent sur Linux](sql-server-linux-run-sql-server-agent-job.md).
