---
description: sysmail_update_account_sp (Transact-SQL)
title: sysmail_update_account_sp (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/17/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysmail_update_account_sp
- sysmail_update_account_sp_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_update_account_sp
ms.assetid: ba2fdccc-5ed4-40ef-a479-79497b4d61aa
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: b441dfe7aa92bd78bb87ef05b4b7f9cdd6108919
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181889"
---
# <a name="sysmail_update_account_sp-transact-sql"></a>sysmail_update_account_sp (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Modifie les informations dans un compte de messagerie de base de données existant.  
 
 
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sysmail_update_account_sp [ [ @account_id = ] account_id ] [ , ] [ [ @account_name = ] 'account_name' ] ,  
    [ @email_address = ] 'email_address' ,   
    [ @display_name = ] 'display_name' ,   
    [ @replyto_address = ] 'replyto_address' ,  
    [ @description = ] 'description' ,   
    [ @mailserver_name = ] 'server_name' ,   
    [ @mailserver_type = ] 'server_type' ,   
    [ @port = ] port_number ,   
    [ @timeout = ] 'timeout' ,  
    [ @username = ] 'username' ,  
    [ @password = ] 'password' ,  
    [ @use_default_credentials = ] use_default_credentials ,  
    [ @enable_ssl = ] enable_ssl   
```  
  
## <a name="arguments"></a>Arguments  
`[ @account_id = ] account_id` ID de compte à mettre à jour. *account_id* est de **type int**, avec NULL comme valeur par défaut. Au moins un des *account_id* ou *account_name* doit être spécifié. Si les deux arguments sont précisés, la procédure modifie le nom du compte.  
  
`[ @account_name = ] 'account_name'` Nom du compte à mettre à jour. *account_name* est de **type sysname**, avec NULL comme valeur par défaut. Au moins un des *account_id* ou *account_name* doit être spécifié. Si les deux arguments sont précisés, la procédure modifie le nom du compte.  
  
`[ @email_address = ] 'email_address'` Nouvelle adresse de messagerie à partir de laquelle envoyer le message. Cette adresse doit être une adresse de messagerie Internet. Le nom de serveur figurant dans l'adresse identifie le serveur qu'utilise la messagerie de base de données pour envoyer le courrier à partir de ce compte. *email_address* est de type **nvarchar (128)**, avec NULL comme valeur par défaut.  
  
`[ @display_name = ] 'display_name'` Nouveau nom complet à utiliser pour les messages électroniques à partir de ce compte. *display_name* est de type **nvarchar (128)**, sans valeur par défaut.  
  
`[ @replyto_address = ] 'replyto_address'` Nouvelle adresse à utiliser dans l’en-tête Reply-To des messages électroniques à partir de ce compte. *replyto_address* est de type **nvarchar (128)**, sans valeur par défaut.  
  
`[ @description = ] 'description'` Nouvelle description du compte. *Description* est de type **nvarchar (256)**, avec NULL comme valeur par défaut.  
  
`[ @mailserver_name = ] 'server_name'` Nouveau nom du serveur de messagerie SMTP à utiliser pour ce compte. L’ordinateur qui exécute [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doit être en mesure de résoudre le *SERVER_NAME* en adresse IP. *SERVER_NAME* est de **type sysname**, sans valeur par défaut.  
  
`[ @mailserver_type = ] 'server_type'` Nouveau type du serveur de messagerie. *server_type* est de **type sysname**, sans valeur par défaut. Seule une valeur **« SMTP »** est prise en charge.  
  
`[ @port = ] port_number` Nouveau numéro de port du serveur de messagerie. *port_number* est de **type int**, sans valeur par défaut.  
  
`[ @timeout = ] 'timeout'` Paramètre timeout pour SmtpClient. Send d’un seul message électronique. *Timeout* est de **type int** en secondes, sans valeur par défaut.  
  
`[ @username = ] 'username'` Nouveau nom d’utilisateur à utiliser pour se connecter au serveur de messagerie. Le *nom d’utilisateur* est de **type sysname**, sans valeur par défaut.  
  
`[ @password = ] 'password'` Nouveau mot de passe à utiliser pour se connecter au serveur de messagerie. *Password* est de **type sysname**, sans valeur par défaut.  
  
`[ @use_default_credentials = ] use_default_credentials` Spécifie si le courrier électronique doit être envoyé au serveur SMTP à l’aide des informations d’identification du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] service. **use_default_credentials** est de bits, sans valeur par défaut. Lorsque la valeur de ce paramètre est définie sur 1, la messagerie de base de données utilise les informations d'identification du moteur de base de données [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Lorsque ce paramètre a la valeur 0, Database Mail utilise le **\@ nom d’utilisateur** et le **\@ mot de passe** pour l’authentification sur le serveur SMTP. Si le **\@ nom d’utilisateur** et le **\@ mot de passe** sont NULL, l’authentification anonyme est utilisée. Contactez votre administrateur SMTP avant de définir ce paramètre.  
  
`[ @enable_ssl = ] enable_ssl` Spécifie si Database Mail chiffre les communications à l’aide du protocole TLS (Transport Layer Security), précédemment connu sous le nom de SSL (Secure Sockets Layer) (SSL). Utilisez cette option si TLS est requis sur votre serveur SMTP. **enable_ssl** est de bits, sans valeur par défaut.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 Lorsque le nom et l'ID du compte sont tous deux spécifiés, la procédure stockée change le nom du compte en plus de mettre à jour les autres informations du compte. Il peut s'avérer nécessaire de modifier le nom du compte pour corriger des erreurs qui s'y seraient glissées.  
  
 La procédure stockée **sysmail_update_account_sp** se trouve dans la base de données **msdb** et appartient au schéma **dbo** . La procédure doit être exécutée avec un nom en trois parties si la base de données actuelle n’est pas **msdb**.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle serveur fixe **sysadmin** .  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-changing-the-information-for-an-account"></a>R. Modification des informations d'un compte  
 L’exemple suivant met à jour le compte `AdventureWorks Administrator` dans la base de données **msdb** . Les informations du compte prennent les valeurs fournies.  
  
```  
EXECUTE msdb.dbo.sysmail_update_account_sp  
     @account_name = 'AdventureWorks Administrator'  
    ,@description = 'Mail account for administrative e-mail.'  
    ,@email_address = 'dba@Adventure-Works.com'  
    ,@display_name = 'AdventureWorks Automated Mailer'  
    ,@replyto_address = NULL  
    ,@mailserver_name = 'smtp.Adventure-Works.com'  
    ,@mailserver_type = 'SMTP'  
    ,@port = 25  
    ,@timeout = 60  
    ,@username = NULL  
    ,@password = NULL  
    ,@use_default_credentials = 0  
    ,@enable_ssl = 0;  
```  
  
### <a name="b-changing-the-name-of-an-account-and-the-information-for-an-account"></a>B. Modification du nom d'un compte et des informations qui s'y rapportent  
 L'exemple suivant modifie le nom du compte et le met à jour avec l'ID de compte `125`. Le nouveau nom du compte est `Backup Mail Server`.  
  
```  
EXECUTE msdb.dbo.sysmail_update_account_sp  
     @account_id = 125  
    ,@account_name = 'Backup Mail Server'  
    ,@description = 'Mail account for administrative e-mail.'  
    ,@email_address = 'dba@Adventure-Works.com'  
    ,@display_name = 'AdventureWorks Automated Mailer'  
    ,@replyto_address = NULL  
    ,@mailserver_name = 'smtp-backup.Adventure-Works.com'  
    ,@mailserver_type = 'SMTP'  
    ,@port = 25  
    ,@timeout = 60  
    ,@username = NULL  
    ,@password = NULL  
    ,@use_default_credentials = 0  
    ,@enable_ssl = 0;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Messagerie de base de données](../../relational-databases/database-mail/database-mail.md)   
 [Créer un compte Database Mail](../../relational-databases/database-mail/create-a-database-mail-account.md)   
 [Database Mail des procédures stockées &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/database-mail-stored-procedures-transact-sql.md)  
  
  
