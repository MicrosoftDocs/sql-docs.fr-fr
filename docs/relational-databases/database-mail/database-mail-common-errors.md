---
description: Erreurs courantes avec la messagerie de base de données
title: Erreurs courantes avec la messagerie de base de données | Microsoft Docs
ms.custom: ''
ms.date: 04/22/2019
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- architecture [SQL Server], Database Mail
- Database Mail [SQL Server], architecture
- Database Mail [SQL Server], components
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 0df7b437ff897966502291975a99d56a64003f08
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272360"
---
# <a name="common-errors-with-database-mail"></a>Erreurs courantes avec la messagerie de base de données 
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

Cet article décrit certaines erreurs courantes rencontrées avec la messagerie de base de données, et leurs solutions.

## <a name="could-not-find-stored-procedure-sp_send_dbmail"></a>la procédure stockée « sp_send_dbmail » est introuvable
La procédure stockée [sp_send_dbmail](../system-stored-procedures/sp-send-dbmail-transact-sql.md) est installée dans la base de données msdb. Vous devez exécuter **sp_send_dbmail** à partir de la base de données msdb ou spécifier un nom en trois parties pour la procédure stockée.

Exemple :
```sql
EXEC msdb.dbo.sp_send_dbmail ...
```

Ou :

```sql
USE msdb;
GO
EXEC dbo.sp_send_dbmail ...
```

Utilisez l’[Assistant Configuration de Database Mail](configure-database-mail.md) pour activer et configurer la messagerie de base de données.

## <a name="profile-not-valid"></a>profil incorrect
Ce message peut s'afficher pour deux raisons. Le profil spécifié n’existe pas ou l’utilisateur qui exécute [sp_send_dbmail (Transact-SQL)](../system-stored-procedures/sp-send-dbmail-transact-sql.md) n’a pas l’autorisation d’accéder au profil.

Pour vérifier les autorisations pour un profil, exécutez la procédure stockée [sysmail_help_principalprofile_sp (Transact-SQL)](../system-stored-procedures/sysmail-help-principalprofile-sp-transact-sql.md) avec le nom du profil. Utilisez la procédure stockée [sysmail_add_principalprofile_sp (Transact-SQL)](../system-stored-procedures/sysmail-help-principalprofile-sp-transact-sql.md) ou l’[Assistant Configuration de Database Mail](configure-database-mail.md) pour accorder l’autorisation à un groupe ou un utilisateur de msdb d’accéder à un profil.

## <a name="permission-denied-on-sp_send_dbmail"></a>autorisation refusée sur la procédure sp_send_dbmail

Cette rubrique décrit le dépannage suite à un message d’erreur indiquant que l’utilisateur qui essaie d’envoyer des messages à l’aide de Database Mail n’est pas autorisé à exécuter la procédure sp_send_dbmail.

Le texte du message d'erreur est le suivant :

```
EXECUTE permission denied on object 'sp_send_dbmail', 
database 'msdb', schema 'dbo'.
```

Pour envoyer des messages à partir de Database Mail, l’utilisateur doit être un utilisateur de la base de données msdb et un membre du rôle de base de données DatabaseMailUserRole dans la base de données msdb. Pour ajouter des utilisateurs ou groupes msdb à ce rôle, utilisez SQL Server Management Studio ou exécutez l’instruction suivante pour l’utilisateur ou le rôle que vous voulez autoriser à envoyer des messages à partir de Database Mail.

```sql
EXEC msdb.dbo.sp_addrolemember @rolename = 'DatabaseMailUserRole'
    ,@membername = '<user or role name>';
GO
```
Pour plus d’informations, consultez [sp_addrolemember](../system-stored-procedures/sp-addrolemember-transact-sql.md) et [sp_droprolemember](../system-stored-procedures/sp-droprolemember-transact-sql.md).

## <a name="database-mail-queued-no-entries-in-sysmail_event_log-or-windows-application-event-log"></a>Courrier en file d’attente, aucune entrée dans sysmail_event_log ou dans le journal des événements des applications Windows 

Database Mail s’appuie sur Service Broker pour mettre les e-mails en file d’attente. Si Database Mail est arrêté ou si la remise de message Service Broker n’est pas activée dans la base de données **msdb**, Database Mail met en file d’attente les messages dans la base de données mais ne peut pas remettre les messages. Dans ce cas, les messages Service Broker restent dans la file d’attente de messagerie Service Broker. Service Broker n’active pas le programme externe ; il n’existe donc aucune entrée du journal dans **sysmail_event_log** et l’état de l’élément n’est pas mis à jour dans **sysmail_allitems** ni dans les vues associées.

Exécutez l’instruction suivante pour vérifier si Service Broker est activé dans la base de données **msdb** :

```sql
SELECT is_broker_enabled FROM sys.databases WHERE name = 'msdb';
```

Une valeur 0 indique que la remise de message Service Broker n’est pas activée dans la base de données msdb. Pour résoudre le problème, activez Service Broker dans la base de données avec la commande Transact-SQL suivante :

```sql
USE master ;
GO

ALTER DATABASE msdb SET ENABLE_BROKER ;
GO
``` 

La messagerie de base de données est basée sur un certain nombre de procédures stockées internes. Pour réduire la surface d’exposition, ces procédures stockées sont désactivées lors d’une nouvelle installation de SQL Server. Pour activer ces procédures stockées, utilisez l’[option Database Mail XPs](../../database-engine/configure-windows/database-mail-xps-server-configuration-option.md) de la procédure stockée système **sp_configure**, comme dans l’exemple suivant :

```sql
EXEC sp_configure 'show advanced options', 1;  
RECONFIGURE;
EXEC sp_configure 'Database Mail XPs', 1;  
RECONFIGURE  
GO  

Database Mail may be stopped in the **msdb** database. To check status of Database Mail, execute the following statement:

```sql
EXECUTE dbo.sysmail_help_status_sp;
```

Pour démarrer Database Mail dans une base de données hôte de messagerie, exécutez la commande suivante dans la base de données msdb :

```sql
EXECUTE dbo.sysmail_start_sp;
```

Service Broker examine la durée de vie de la boîte de dialogue pour les messages quand elle est activée ; par conséquent, les messages ayant figuré dans la file d’attente de transmission Service Broker pour une durée supérieure à la durée de vie de la boîte de dialogue configurée échouent immédiatement. Database Mail met à jour l’état des messages qui ont échoué dans [sysmail_allitems](../system-catalog-views/sysmail-allitems-transact-sql.md) et les vues associées. Vous devez déterminer si vous voulez envoyer de nouveau les messages électroniques. Pour plus d’informations sur la configuration de la durée de vie de la boîte de dialogue utilisée par Database Mail, consultez [sysmail_configure_sp (Transact-SQL)](../system-stored-procedures/sysmail-configure-sp-transact-sql.md).



##  <a name="see-also"></a><a name="RelatedContent"></a> Voir aussi
  
-  [Vue d’ensemble de Database Mail](database-mail.md)
-  [Créer un profil de messagerie de base de données](create-a-database-mail-profile.md)
  
  
