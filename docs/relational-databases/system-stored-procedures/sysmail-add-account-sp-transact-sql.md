---
description: sysmail_add_account_sp (Transact-SQL)
title: sysmail_add_account_sp (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysmail_add_account_sp
- sysmail_add_account_sp_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_add_account_sp
ms.assetid: 65e15e2e-107c-49c3-b12c-f4edf0eb1617
author: markingmyname
ms.author: maghan
ms.openlocfilehash: e5458a79b5cb44aef8f8a931051cc8346bf4e88f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99178073"
---
# <a name="sysmail_add_account_sp-transact-sql"></a>sysmail_add_account_sp (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Crée un nouveau compte de messagerie de base de données contenant des informations sur un compte SMTP.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sysmail_add_account_sp  [ @account_name = ] 'account_name',  
    [ @email_address = ] 'email_address' ,  
    [ [ @display_name = ] 'display_name' , ]  
    [ [ @replyto_address = ] 'replyto_address' , ]  
    [ [ @description = ] 'description' , ]  
    [ @mailserver_name = ] 'server_name'   
    [ , [ @mailserver_type = ] 'server_type' ]  
    [ , [ @port = ] port_number ]  
    [ , [ @username = ] 'username' ]  
    [ , [ @password = ] 'password' ]  
    [ , [ @use_default_credentials = ] use_default_credentials ]  
    [ , [ @enable_ssl = ] enable_ssl ]  
    [ , [ @account_id = ] account_id OUTPUT ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @account_name = ] 'account_name'` Nom du compte à ajouter. *account_name* est de **type sysname**, sans valeur par défaut.  
  
`[ @email_address = ] 'email_address'` Adresse de messagerie à partir de laquelle envoyer le message. Cette adresse doit être une adresse de messagerie Internet. *email_address* est de type **nvarchar (128)**, sans valeur par défaut. Par exemple, un compte pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] agent peut envoyer des messages électroniques à partir de l’adresse **SQLAgent \@ Adventure-Works.com**.  
  
`[ @display_name = ] 'display_name'` Nom complet à utiliser pour les messages électroniques à partir de ce compte. *display_name* est de type **nvarchar (128)**, avec NULL comme valeur par défaut. Par exemple, un compte pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] agent peut afficher le nom **SQL Server agent le courrier électronique automatisé** sur les messages électroniques.  
  
`[ @replyto_address = ] 'replyto_address'` Adresse à laquelle les réponses aux messages de ce compte sont envoyées. *replyto_address* est de type **nvarchar (128)**, avec NULL comme valeur par défaut. Par exemple, les réponses à un compte pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] agent peuvent être envoyées à l’administrateur de base de données, **danw \@ Adventure-Works.com**.  
  
`[ @description = ] 'description'` Description du compte. *Description* est de type **nvarchar (256)**, avec NULL comme valeur par défaut.  
  
`[ @mailserver_name = ] 'server_name'` Nom ou adresse IP du serveur de messagerie SMTP à utiliser pour ce compte. L’ordinateur qui exécute [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doit être en mesure de résoudre le *SERVER_NAME* en adresse IP. *SERVER_NAME* est de **type sysname**, sans valeur par défaut.  
  
`[ @mailserver_type = ] 'server_type'` Type de serveur de messagerie. *server_type* est de **type sysname**, avec **'SMTP'** comme valeur par défaut.  
  
`[ @port = ] port_number` Numéro de port du serveur de messagerie. *port_number* est de **type int**, avec 25 comme valeur par défaut.  
  
`[ @username = ] 'username'` Nom d’utilisateur à utiliser pour se connecter au serveur de messagerie. *username* est de type **nvarchar (128)**, avec NULL comme valeur par défaut. Lorsque ce paramètre possède la valeur NULL, la messagerie de base de données n'utilise pas d'authentification pour ce compte. Si le serveur de messagerie ne nécessite pas d'authentification, utilisez NULL comme nom d'utilisateur.  
  
`[ @password = ] 'password'` Mot de passe à utiliser pour se connecter au serveur de messagerie. *Password* est de type **nvarchar (128)**, avec NULL comme valeur par défaut. Il est inutile de fournir un mot de passe, sauf si un nom d'utilisateur est spécifié.  
  
`[ @use_default_credentials = ] use_default_credentials` Spécifie si le courrier électronique doit être envoyé au serveur SMTP à l’aide des informations d’identification du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] . **use_default_credentials** est de bit, avec 0 comme valeur par défaut. Lorsque la valeur de ce paramètre est définie sur 1, la messagerie de base de données utilise les informations d'identification du moteur de base de données [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Lorsque ce paramètre a la valeur 0, Database Mail envoie les paramètres de **\@ nom d’utilisateur** et **\@ de mot de passe** s’ils sont présents, sinon envoie un message sans **\@ nom d’utilisateur** ni paramètre **\@ de mot de passe** .  
  
`[ @enable_ssl = ] enable_ssl` Spécifie si Database Mail chiffre les communications à l’aide de SSL (Secure Sockets Layer). **Enable_ssl** est de bit, avec 0 comme valeur par défaut.  
  
`[ @account_id = ] account_id OUTPUT` Retourne l’ID de compte pour le nouveau compte. *account_id* est de **type int**, avec NULL comme valeur par défaut.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 Database mail fournit des paramètres distincts pour **\@ email_address**, **\@ display_name** et **\@ replyto_address**. Le paramètre **\@ email_address** est l’adresse à partir de laquelle le message est envoyé. Le paramètre **\@ display_name** est le nom affiché dans le champ **de :** du message électronique. Le paramètre **\@ replyto_address** est l’adresse à laquelle les réponses au message électronique sont envoyées. Par exemple, un compte utilisé pour l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut envoyer des messages électroniques à partir d'une adresse de messagerie utilisée uniquement pour l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Les messages provenant de cette adresse doivent afficher un nom convivial, afin que les destinataires puissent aisément déterminer que le message a été envoyé par l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si un destinataire répond au message, la réponse doit arriver à l'administrateur de bases de données et non à l'adresse utilisée par l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour ce scénario, le compte utilise **SqlAgent@Adventure-Works.com** comme adresse de messagerie. Le nom d’affichage est défini sur **SQL Server Agent Mailer automatisé**. Le compte utilise **danw@Adventure-Works.com** comme adresse de réponse, donc les réponses aux messages envoyés à partir de ce compte sont envoyées à l’administrateur de la base de données plutôt qu’à l’adresse de messagerie de l' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] agent. En fournissant des paramètres indépendants pour ces trois paramètres, la messagerie de base de données vous permet de configurer des messages afin qu'ils répondent à vos besoins.  
  
 Le paramètre **\@ mailserver_type** prend en charge la valeur **'SMTP'**.  
  
 Lorsque **\@ use_default_credentials** a la valeur 1, le courrier est envoyé au serveur SMTP à l’aide des informations d’identification du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] . Lorsque **\@ use_default_credentials** a la valeur 0 et qu’un **\@ nom d’utilisateur** et un **\@ mot de passe** sont spécifiés pour un compte, le compte utilise l’authentification SMTP. Le **\@ nom d’utilisateur** et le **\@ mot de passe** sont les informations d’identification utilisées par le compte pour le serveur SMTP, et non les informations d’identification pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou le réseau sur lequel se trouve l’ordinateur.  
  
 La procédure stockée **sysmail_add_account_sp** se trouve dans la base de données **msdb** et appartient au schéma **dbo** . La procédure doit être exécutée avec un nom en trois parties si la base de données actuelle n’est pas **msdb**.  
  
## <a name="permissions"></a>Autorisations  
 Les autorisations d’exécution pour cette procédure sont octroyées par défaut aux membres du rôle serveur fixe **sysadmin** .  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant crée un compte nommé `AdventureWorks Administrator`. Ce compte utilise l'adresse de messagerie électronique `dba@Adventure-Works.com` en envoie du courrier électronique au serveur de messagerie SMTP `smtp.Adventure-Works.com`. Les messages électroniques envoyés à partir de ce compte s’affichent `AdventureWorks Automated Mailer` sur la ligne de **:** du message. Les réponses aux messages sont envoyés vers `danw@Adventure-Works.com`.  
  
```  
EXECUTE msdb.dbo.sysmail_add_account_sp  
    @account_name = 'AdventureWorks Administrator',  
    @description = 'Mail account for administrative e-mail.',  
    @email_address = 'dba@Adventure-Works.com',  
    @display_name = 'AdventureWorks Automated Mailer',  
    @mailserver_name = 'smtp.Adventure-Works.com' ;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Messagerie de base de données](../../relational-databases/database-mail/database-mail.md)   
 [Créer un compte Database Mail](../../relational-databases/database-mail/create-a-database-mail-account.md)   
 [Database Mail des procédures stockées &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/database-mail-stored-procedures-transact-sql.md)  
  
  
