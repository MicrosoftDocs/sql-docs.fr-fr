---
description: sp_notify_operator (Transact-SQL)
title: sp_notify_operator (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_notify_operator_TSQL
- sp_notify_operator
dev_langs:
- TSQL
helpviewer_keywords:
- sp_notify_operator
ms.assetid: c440f5c9-9884-4196-b07c-55d87afb17c3
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 954a314760a22d9b00996dc460062fcc75af16aa
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191999"
---
# <a name="sp_notify_operator-transact-sql"></a>sp_notify_operator (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Envoie un message électronique à un opérateur utilisant la messagerie de base de données.  
  
 
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_notify_operator  
    [ @profile_name = ] 'profilename' ,  
    [ @id = ] id ,  
    [ @name = ] 'name' ,  
    [ @subject = ] 'subject' ,  
    [ @body = ] 'message' ,  
    [ @file_attachments = ] 'attachment'  
    [ @mail_database = ] 'mail_host_database'  
```  
  
## <a name="arguments"></a>Arguments  
`[ @profile_name = ] 'profilename'` Nom du profil de Database Mail à utiliser pour envoyer le message. *ProfileName* est **de type nvarchar (128)**. Si *ProfileName* n’est pas spécifié, le profil de Database mail par défaut est utilisé.  
  
`[ @id = ] id` Identificateur de l’opérateur auquel envoyer le message. *ID* est de **type int**, avec NULL comme valeur par défaut. Un *ID* ou un *nom* doit être spécifié.  
  
`[ @name = ] 'name'` Nom de l’opérateur auquel envoyer le message. *Name* est de type **nvarchar (128)**, avec NULL comme valeur par défaut. Un *ID* ou un *nom* doit être spécifié.  
  
> **Remarque :** Une adresse de messagerie doit être définie pour l’opérateur avant de pouvoir recevoir des messages.  
  
`[ @subject = ] 'subject'` Objet du message électronique. l' *objet* est de type **nvarchar (256)** et n’a pas de valeur par défaut.  
  
`[ @body = ] 'message'` Corps du message électronique. le *message* est de type **nvarchar (max)** et n’a pas de valeur par défaut.  
  
`[ @file_attachments = ] 'attachment'` Nom d’un fichier à joindre au message électronique. la *pièce jointe* est de type **nvarchar (512)**, sans valeur par défaut.  
  
`[ @mail_database = ] 'mail_host_database'` Spécifie le nom de la base de données hôte de messagerie. *mail_host_database* est **de type nvarchar (128)**. Si aucun *mail_host_database* n’est spécifié, la base de données **msdb** est utilisée par défaut.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 Envoie le message spécifié à l'adresse de messagerie de l'opérateur choisi. Si l'opérateur ne dispose pas d'une adresse de messagerie, une erreur est renvoyée.  
  
 La messagerie de base de données et une base de données hôte de courrier doivent être configurées avant qu'une notification puisse être envoyée à un opérateur.  
  
## <a name="permissions"></a>Autorisations  
 Par défaut, les membres du rôle serveur fixe **sysadmin** peuvent exécuter cette procédure stockée. Les autres utilisateurs doivent disposer de l'un des rôles de base de données fixes suivants de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent dans la base de données **msdb** :  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 Pour en savoir plus sur les autorisations de ces rôles, consultez [Rôles de base de données fixes de l'Agent SQL Server](../../ssms/agent/sql-server-agent-fixed-database-roles.md).  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant envoie une notification par courrier électronique à l'opérateur `François Ajenstat` à l'aide du profil de messagerie de la base de données `AdventureWorks Administrator`. Le message électronique a pour objet `Test Notification`. Le message électronique contient la phrase « This is a test of notification via e-mail. ».  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_notify_operator  
   @profile_name = N'AdventureWorks Administrator',  
   @name = N'François Ajenstat',  
   @subject = N'Test Notification',  
   @body = N'This is a test of notification via e-mail.' ;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [SQL Server Agent des procédures stockées &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sql-server-agent-stored-procedures-transact-sql.md)   
 [sp_add_operator &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-operator-transact-sql.md)   
 [sp_help_operator &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-operator-transact-sql.md)   
 [sp_delete_operator &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-delete-operator-transact-sql.md)  
  
  
