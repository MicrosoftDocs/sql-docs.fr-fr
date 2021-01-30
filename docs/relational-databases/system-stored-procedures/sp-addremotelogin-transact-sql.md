---
description: sp_addremotelogin (Transact-SQL)
title: sp_addremotelogin (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_addremotelogin_TSQL
- sp_addremotelogin
dev_langs:
- TSQL
helpviewer_keywords:
- sp_addremotelogin
ms.assetid: 71b7cd36-a17d-4b12-b102-10aeb0f9268b
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 13b935d91772d70d58b38f9f995341d81e04388f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99165183"
---
# <a name="sp_addremotelogin-transact-sql"></a>sp_addremotelogin (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Ajoute un nouvel ID de connexion à distance sur le serveur local. Cela permet aux serveurs distants de se connecter et d'exécuter des appels de procédure distante.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepNextDontUse](../../includes/ssnotedepnextdontuse-md.md)] Utilisez des serveurs liés et des procédures stockées de serveur lié à la place.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_addremotelogin [ @remoteserver = ] 'remoteserver'   
     [ , [ @loginame = ] 'login' ]   
     [ , [ @remotename = ] 'remote_name' ]  
```  
  
## <a name="arguments"></a>Arguments  
 [ @remoteserver **=** ] **'**_serveur_distant_**'**  
 Nom du serveur distant auquel s'applique la connexion d'accès à distance. *serveur_distant* est de **type sysname**, sans valeur par défaut. Si seul *serveur_distant* est spécifié, tous les utilisateurs sur *serveur_distant* sont mappés à des connexions existantes du même nom sur le serveur local. Le serveur doit être connu du serveur local. Il est ajouté avec sp_addserver. Lorsque les utilisateurs du *serveur_distant* se connectent au serveur local qui exécute [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour exécuter une procédure stockée distante, ils se connectent en tant que connexion locale qui correspond à leur propre connexion sur le *serveur_distant*. *serveur_distant* est le serveur qui lance l’appel de procédure distante.  
  
 [ @loginame **=** ] **«**_connexion_**»**  
 ID de connexion de l'utilisateur sur l'instance locale de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. *login* est de type **sysname**, avec NULL comme valeur par défaut. la *connexion* doit déjà exister sur l’instance locale de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Si la *connexion* est spécifiée, tous les utilisateurs sur *serveur_distant* sont mappés à cette connexion locale spécifique. Lorsque les utilisateurs du *serveur_distant* se connectent à l’instance locale de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour exécuter une procédure stockée distante, ils se connectent en tant que *connexion*.  
  
 [ @remotename **=** ] **'**_remote_name_**'**  
 ID de connexion de l'utilisateur sur le serveur distant. *remote_name* est de **type sysname**, avec NULL comme valeur par défaut. *remote_name* doit exister sur *serveur_distant*. Si *remote_name* est spécifié, l’utilisateur *remote_name* spécifique est mappé à la *connexion* sur le serveur local. Lorsque *remote_name* sur *serveur_distant* se connecte à l’instance locale de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour exécuter une procédure stockée distante, il se connecte en tant que *connexion*. L’ID de connexion de *remote_name* peut être différent de l’ID de connexion sur le serveur distant, *login*.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="remarks"></a>Notes  
 Pour exécuter des requêtes distribuées, utilisez sp_addlinkedsrvlogin.  
  
 La procédure stockée sp_addremotelogin ne peut pas être utilisée au sein d'une transaction définie par l'utilisateur.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres des rôles serveur fixes sysadmin et securityadmin sont habilités à exécuter sp_addremotelogin.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-mapping-one-to-one"></a>R. Mappage de un-à-un  
 L'exemple suivant mappe des noms distants à des noms locaux lorsque le serveur distant `ACCOUNTS` et le serveur local ont les mêmes connexions utilisateur.  
  
```  
EXEC sp_addremotelogin 'ACCOUNTS';  
```  
  
### <a name="b-mapping-many-to-one"></a>B. Mappage de plusieurs-à-un  
 L'exemple suivant crée une entrée qui mappe tous les utilisateurs du serveur distant `ACCOUNTS` à la connexion locale `Albert`.  
  
```  
EXEC sp_addremotelogin 'ACCOUNTS', 'Albert';  
```  
  
### <a name="c-using-explicit-one-to-one-mapping"></a>C. Utilisation d'un mappage explicite de un-à-un  
 L'exemple suivant mappe une connexion distante de l'utilisateur distant `Chris` situé sur le serveur distant `ACCOUNTS` à l'utilisateur local `salesmgr`.  
  
```  
EXEC sp_addremotelogin 'ACCOUNTS', 'salesmgr', 'Chris';  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sp_addlinkedsrvlogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlinkedsrvlogin-transact-sql.md)   
 [sp_addlogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlogin-transact-sql.md)   
 [sp_addserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addserver-transact-sql.md)   
 [sp_dropremotelogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropremotelogin-transact-sql.md)   
 [sp_grantlogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-grantlogin-transact-sql.md)   
 [sp_helpremotelogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpremotelogin-transact-sql.md)   
 [sp_helpserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpserver-transact-sql.md)   
 [sp_remoteoption &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-remoteoption-transact-sql.md)   
 [sp_revokelogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-revokelogin-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
