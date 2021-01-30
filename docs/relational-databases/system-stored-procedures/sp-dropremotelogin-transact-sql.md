---
description: sp_dropremotelogin (Transact-SQL)
title: sp_dropremotelogin (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_dropremotelogin
- sp_dropremotelogin_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_dropremotelogin
ms.assetid: 9f097652-a286-40b2-be73-568d77ada698
ms.author: vanto
author: VanMSFT
ms.openlocfilehash: 7b468ff83bffdd581695b9553c636b06fbe6c784
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187034"
---
# <a name="sp_dropremotelogin-transact-sql"></a>sp_dropremotelogin (Transact-SQL)

[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Supprime une connexion distante mappée sur une connexion locale utilisée pour exécuter des procédures stockées distantes sur le serveur local exécutant [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepNextDontUse](../../includes/ssnotedepnextdontuse-md.md)] Utilisez plutôt des serveurs liés et des procédures stockées de serveurs liés.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
sp_dropremotelogin [ @remoteserver = ] 'remoteserver'   
     [ , [ @loginame = ] 'login' ]   
     [ , [ @remotename = ] 'remote_name' ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @remoteserver = ] 'remoteserver'` Nom du serveur distant mappé sur la connexion distante à supprimer. *serveur_distant* est de **type sysname**, sans valeur par défaut. *serveur_distant* doit déjà exister.  
  
`[ @loginame = ] 'login'` Nom de connexion facultatif sur le serveur local qui est associé au serveur distant. *login* est de type **sysname**, avec NULL comme valeur par défaut. la *connexion* doit déjà exister si elle est spécifiée.  
  
`[ @remotename = ] 'remote_name'` Nom facultatif de la connexion distante mappée à *login* lors de la connexion à partir du serveur distant. *remote_name* est de **type sysname**, avec NULL comme valeur par défaut.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="remarks"></a>Notes  
 Si seul *serveur_distant* est spécifié, toutes les connexions distantes de ce serveur distant sont supprimées du serveur local. Si la *connexion* est également spécifiée, toutes les connexions distantes à partir de *serveur_distant* mappées à cette connexion locale spécifique sont supprimées du serveur local. Si *remote_name* est également spécifié, seule la connexion distante pour cet utilisateur distant à partir de *serveur_distant* est supprimée du serveur local.  
  
 Pour ajouter des utilisateurs de serveur local, utilisez **sp_addlogin**. Pour supprimer des utilisateurs du serveur local, utilisez **sp_droplogin**.  
  
 Les connexions distantes sont obligatoires uniquement lorsque vous utilisez des versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 7.0 et les versions ultérieures utilisent à la place des connexions de serveurs liés. Utilisez **sp_addlinkedsrvlogin** et **sp_droplinkedsrvlogin** pour ajouter et supprimer des connexions de serveurs liés.  
  
 **sp_dropremotelogin** ne peut pas être exécutée dans une transaction définie par l’utilisateur.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l’appartenance aux rôles serveur fixes **sysadmin** ou **securityadmin** .  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-dropping-all-remote-logins-for-a-remote-server"></a>R. Suppression de toutes les connexions distantes d'un serveur distant  
 Le code exemple suivant supprime l'entrée du serveur distant `ACCOUNTS`, supprimant ainsi tous les mappages entre les connexions sur le serveur local et les connexions distantes sur le serveur distant.  
  
```sql
EXEC sp_dropremotelogin 'ACCOUNTS';  
```  
  
### <a name="b-dropping-a-login-mapping"></a>B. Suppression d'un mappage de connexion  
 Le code exemple suivant supprime l'entrée qui mappe des connexions distantes du serveur distant `ACCOUNTS` sur la connexion locale `Albert`.  
  
```sql
EXEC sp_dropremotelogin 'ACCOUNTS', 'Albert';  
```  
  
### <a name="c-dropping-a-remote-user"></a>C. Suppression d'un utilisateur distant  
 Le code exemple suivant supprime la connexion de l'utilisateur distant `Chris` sur le serveur distant `ACCOUNTS` qui a été mappée sur l'utilisateur local `salesmgr`.  
  
```sql
EXEC sp_dropremotelogin 'ACCOUNTS', 'salesmgr', 'Chris';  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées de sécurité &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [sp_addlinkedsrvlogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlinkedsrvlogin-transact-sql.md)   
 [sp_addlogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlogin-transact-sql.md)   
 [sp_addremotelogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addremotelogin-transact-sql.md)   
 [sp_addserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addserver-transact-sql.md)   
 [sp_droplinkedsrvlogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-droplinkedsrvlogin-transact-sql.md)   
 [sp_droplogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-droplogin-transact-sql.md)   
 [sp_helpremotelogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpremotelogin-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
