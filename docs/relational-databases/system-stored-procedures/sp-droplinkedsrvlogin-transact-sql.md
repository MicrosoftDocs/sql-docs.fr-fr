---
description: sp_droplinkedsrvlogin (Transact-SQL)
title: sp_droplinkedsrvlogin (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_droplinkedsrvlogin_TSQL
- sp_droplinkedsrvlogin
dev_langs:
- TSQL
helpviewer_keywords:
- sp_droplinkedsrvlogin
ms.assetid: 75a4a040-72d5-4d29-8304-de0aa481ad4b
author: markingmyname
ms.author: maghan
ms.openlocfilehash: c6e67dc1b33fb5c5655992b11367ab60639c8654
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209318"
---
# <a name="sp_droplinkedsrvlogin-transact-sql"></a>sp_droplinkedsrvlogin (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Supprime un mappage existant entre une connexion du serveur local exécutant [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et une connexion sur le serveur lié.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_droplinkedsrvlogin [ @rmtsrvname= ] 'rmtsrvname' ,   
   [ @locallogin= ] 'locallogin'  
```  
  
## <a name="arguments"></a>Arguments  
`[ @rmtsrvname = ] 'rmtsrvname'` Nom d’un serveur lié auquel [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] s’applique le mappage de connexion. *rmtsrvname* est de **type sysname**, sans valeur par défaut. *rmtsrvname* doit déjà exister.  
  
`[ @locallogin = ] 'locallogin'` Nom de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] connexion sur le serveur local qui a un mappage au serveur lié *rmtsrvname*. *LocalLogin* est de **type sysname**, sans valeur par défaut. Un mappage de *LocalLogin* à *rmtsrvname* doit déjà exister. Si la valeur est NULL, le mappage par défaut créé par **sp_addlinkedserver**, qui mappe toutes les connexions sur le serveur local aux connexions sur le serveur lié, est supprimé.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="remarks"></a>Notes  
 Lorsque le mappage existant pour une connexion est supprimé, le serveur local utilise le mappage par défaut créé par **sp_addlinkedserver** lorsqu’il se connecte au serveur lié pour le compte de cette connexion. Pour modifier le mappage par défaut, utilisez **sp_addlinkedsrvlogin**.  
  
 Si le mappage par défaut est également supprimé, seules les connexions qui ont été explicitement affectées à un mappage de connexion au serveur lié, à l’aide de **sp_addlinkedsrvlogin**, peuvent accéder au serveur lié.  
  
 les **sp_droplinkedsrvlogin** ne peuvent pas être exécutées à partir d’une transaction définie par l’utilisateur.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation ALTER ANY LOGIN sur le serveur.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-removing-the-login-mapping-for-an-existing-user"></a>R. Suppression du mappage de connexion d'un utilisateur existant  
 L'exemple suivant supprime le mappage pour la connexion `Mary` du serveur local vers le serveur lié `Accounts`. Par conséquent, la connexion `Mary` utilise le mappage de connexion par défaut.  
  
```  
EXEC sp_droplinkedsrvlogin 'Accounts', 'Mary';  
```  
  
### <a name="b-removing-the-default-login-mapping"></a>B. Suppression du mappage de connexion par défaut  
 L'exemple suivant supprime le mappage de connexion par défaut créé à l'origine par l'exécution de `sp_addlinkedserver` sur le serveur lié `Accounts`.  
  
```  
EXEC sp_droplinkedsrvlogin 'Accounts', NULL;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sp_addlinkedserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md)   
 [sp_addlinkedsrvlogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlinkedsrvlogin-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
