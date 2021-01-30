---
description: sysmail_help_account_sp (Transact-SQL)
title: sysmail_help_account_sp (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysmail_help_account_sp_TSQL
- sysmail_help_account_sp
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_help_account_sp
ms.assetid: 87c7c39c-8e05-4e68-9272-45f908809c3b
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 1931cbc58177686c1b7f09b069bc9ba061447e76
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181980"
---
# <a name="sysmail_help_account_sp-transact-sql"></a>sysmail_help_account_sp (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Répertorie les informations (à l'exception des mots de passe) relatifs aux comptes de messagerie de base de données.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sysmail_help_account_sp [ [ @account_id = ] account_id | [ @account_name = ] 'account_name' ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @account_id = ] account_id` ID du compte pour lequel répertorier les informations. *account_id* est de **type int**, avec NULL comme valeur par défaut.  
  
`[ @account_name = ] 'account_name'` Nom du compte pour lequel répertorier les informations. *account_name* est de **type sysname**, avec NULL comme valeur par défaut.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 Retourne un jeu de résultats contenant les colonnes répertoriées ci-après.  
  
| Nom de la colonne | Type de données | Description |
| ----------- | --------- | ----------- |
|**account_id**|**int**|Identificateur du compte.|  
|**name**|**sysname**|Nom du compte.|  
|**description**|**nvarchar (256)**|Description du compte.|  
|**email_address**|**nvarchar(128)**|Adresse électronique à partir de laquelle les messages sont envoyés.|  
|**display_name**|**nvarchar(128)**|Nom d'affichage du compte|  
|**replyto_address**|**nvarchar(128)**|Adresse à laquelle les réponses aux messages de ce compte sont envoyées.|  
|**ServerType**|**sysname**|Type de serveur de messagerie pour le compte.|  
|**nom du serveur**|**sysname**|Nom du serveur de messagerie pour le compte.|  
|**port**|**int**|Numéro de port utilisé par le serveur de messagerie.|  
|**username**|**nvarchar(128)**|Nom d'utilisateur à utiliser pour se connecter au serveur de messagerie, si ce serveur utilise l'authentification. Lorsque **username** a la valeur NULL, Database mail n’utilise pas l’authentification pour ce compte.|  
|**use_default_credentials**|**bit**|Spécifie si le courrier électronique doit être envoyé au serveur SMTP en utilisant les informations d'identification du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]. **use_default_credentials** est de bits, sans valeur par défaut. Lorsque ce paramètre est 1, Database Mail utilise les informations d’identification du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] service. Lorsque ce paramètre a la valeur 0, Database Mail utilise le **\@ nom d’utilisateur** et le **\@ mot de passe** pour l’authentification sur le serveur SMTP. Si le **\@ nom d’utilisateur** et le **\@ mot de passe** sont NULL, Database mail utilise l’authentification anonyme. Consultez l’administrateur SMTP avant de spécifier ce paramètre.|  
|**enable_ssl**|**bit**|Spécifie si Database Mail chiffre les communications à l’aide du protocole TLS (Transport Layer Security), précédemment connu sous le nom de SSL (Secure Sockets Layer) (SSL). Utilisez cette option si TLS est requis sur votre serveur SMTP. **enable_ssl** est de bits, sans valeur par défaut. 1 indique Database Mail chiffre les communications à l’aide de TLS. 0 indique Database Mail envoie le courrier sans chiffrement TLS.|  
  
## <a name="remarks"></a>Notes  
 Quand aucun *account_id* ou *account_name* n’est fourni, **sysmail_help_account** répertorie des informations sur tous les comptes de Database mail de l’instance Microsoft SQL Server.  
  
 La procédure stockée **sysmail_help_account_sp** se trouve dans la base de données **msdb** et appartient au schéma **dbo** . La procédure doit être exécutée avec un nom en trois parties si la base de données actuelle n’est pas **msdb**.  
  
## <a name="permissions"></a>Autorisations  
 Les autorisations d’exécution pour cette procédure sont octroyées par défaut aux membres du rôle serveur fixe **sysadmin** .  
  
## <a name="examples"></a>Exemples  
 **A. Affichage d'informations relatives à tous les comptes**  
  
 L'exemple suivant illustre comment répertorier les informations relatives à tous les comptes de l'instance.  
  
```  
EXECUTE msdb.dbo.sysmail_help_account_sp ;  
```  
  
 Voici un exemple d'ensemble de résultats, modifié pour la longueur de ligne :  
  
```  
account_id  name                         description                             email_address             display_name                     replyto_address servertype servername                port        username use_default_credentials enable_ssl  
----------- ---------------------------- --------------------------------------- ------------------------- -------------------------------- --------------- ---------- ------------------------- ----------- -------- ----------------------- ----------  
148         AdventureWorks Administrator Mail account for administrative e-mail. dba@Adventure-Works.com   AdventureWorks Automated Mailer  NULL            SMTP       smtp.Adventure-Works.com  25          NULL 0                          0        
149         Audit Account                Account for audit e-mail.               audit@Adventure-Works.com Automated Mailer (Audit)         NULL            SMTP       smtp.Adventure-Works.com  25          NULL 0                          0        
```  
  
 **B. Affichage d’informations sur un compte spécifique**  
  
 L'exemple suivant illustre comment répertorier les informations relatives au compte nommé `AdventureWorks Administrator`.  
  
```  
EXECUTE msdb.dbo.sysmail_help_account_sp  
    @account_name = 'AdventureWorks Administrator' ;  
```  
  
 Voici un exemple d'ensemble de résultats, modifié pour la longueur de ligne :  
  
```  
account_id  name                         description                             email_address             display_name                     replyto_address servertype servername                port        username use_default_credentials enable_ssl  
----------- ---------------------------- ------------------------------------------------------ ------------------------- ---------------- ---------- ------------------------- ----------- -------- ----------------------- ----------  
148         AdventureWorks Administrator Mail account for administrative e-mail. dba@Adventure-Works.com   AdventureWorks Automated Mailer  NULL            SMTP       smtp.Adventure-Works.com  25          NULL     0                       0       
```  
  
## <a name="see-also"></a>Voir aussi  
 [Messagerie de base de données](../../relational-databases/database-mail/database-mail.md)   
 [Créer un compte Database Mail](../../relational-databases/database-mail/create-a-database-mail-account.md)   
 [Database Mail des procédures stockées &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/database-mail-stored-procedures-transact-sql.md)  
  
  
