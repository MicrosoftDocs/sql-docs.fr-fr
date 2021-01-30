---
description: sp_helplinkedsrvlogin (Transact-SQL)
title: sp_helplinkedsrvlogin (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_helplinkedsrvlogin_TSQL
- sp_helplinkedsrvlogin
dev_langs:
- TSQL
helpviewer_keywords:
- sp_helplinkedsrvlogin
ms.assetid: a2b1eba0-bf71-47e7-a4c7-9f55feec82a3
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 274d1d9a39feac333d5b7d572ec475bb89444c2b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99190015"
---
# <a name="sp_helplinkedsrvlogin-transact-sql"></a>sp_helplinkedsrvlogin (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Affiche des informations sur le mappage des connexions défini sur un serveur lié spécifique utilisé pour les requêtes distribuées et les procédures stockées distantes.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_helplinkedsrvlogin [ [ @rmtsrvname = ] 'rmtsrvname' ]   
     [ , [ @locallogin = ] 'locallogin' ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @rmtsrvname = ] 'rmtsrvname'` Nom du serveur lié auquel s’applique le mappage de connexion. *rmtsrvname* est de **type sysname**, avec NULL comme valeur par défaut. Si cet argument a la valeur NULL, tous les mappages de connexions précisés sur les serveurs liés définis sur l'ordinateur local exécutant [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont renvoyés.  
  
`[ @locallogin = ] 'locallogin'` Nom de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] connexion sur le serveur local qui a un mappage au serveur lié *rmtsrvname*. *LocalLogin* est de **type sysname**, avec NULL comme valeur par défaut. NULL spécifie que tous les mappages de connexion définis sur *rmtsrvname* sont retournés. Si la valeur n’est pas NULL, un mappage de *LocalLogin* à *rmtsrvname* doit déjà exister. la connexion *locale* peut être une [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] connexion ou un utilisateur Windows. L'utilisateur Windows doit avoir une autorisation d'accès à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] directe ou par son appartenance à un groupe Windows qui possède une autorisation d'accès.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**Serveur lié**|**sysname**|Nom du serveur lié.|  
|**Connexion locale**|**sysname**|Connexion locale à laquelle s'applique le mappage.|  
|**Is Self Mapping**|**smallint**|0 = la **connexion locale** est mappée à **session à distance** lors de la connexion au **serveur lié**.<br /><br /> 1 = la **connexion locale** est mappée à la même connexion et au même mot de passe lors de la connexion au **serveur lié**.|  
|**Remote Login**|**sysname**|Nom de connexion sur le **LinkedServer** mappé à **LocalLogin** quand **IsSelfMapping** est égal à 0. Si **IsSelfMapping** a la valeur 1, **RemoteLogin** a la valeur null.|  
  
## <a name="remarks"></a>Notes  
 Avant de supprimer les mappages de connexion, utilisez **sp_helplinkedsrvlogin** pour déterminer les serveurs liés qui sont impliqués.  
  
## <a name="permissions"></a>Autorisations  
 Aucune autorisation n’est vérifiée.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-displaying-all-login-mappings-for-all-linked-servers"></a>R. Affichage de tous les mappages de connexion des serveurs liés  
 Le code exemple suivant affiche tous les mappages de connexion de tous les serveurs liés définis sur l'ordinateur local qui exécute [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
```  
EXEC sp_helplinkedsrvlogin;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Linked Server    Local Login   Is Self Mapping Remote Login   
---------------- ------------- --------------- --------------   
Accounts         NULL          1               NULL  
Sales            NULL          1               NULL  
Sales            Mary          0               sa  
Marketing        NULL          1               NULL  
  
(4 row(s) affected)  
```  
  
### <a name="b-displaying-all-login-mappings-for-a-linked-server"></a>B. Affichage de tous les mappages de connexion d'un serveur lié  
 Le code exemple suivant affiche tous les mappages de connexion définis localement pour le serveur lié `Sales`.  
  
```  
EXEC sp_helplinkedsrvlogin 'Sales';  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Linked Server    Local Login   Is Self Mapping Remote Login   
---------------- ------------- --------------- --------------   
Sales            NULL          1               NULL  
Sales            Mary          0               sa  
  
(2 row(s) affected)  
```  
  
### <a name="c-displaying-all-login-mappings-for-a-local-login"></a>C. Affichage de tous les mappages de connexion d'une connexion locale  
 Le code exemple suivant affiche tous les mappages de connexion définis localement pour la connexion `Mary`.  
  
```  
EXEC sp_helplinkedsrvlogin NULL, 'Mary';  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Linked Server    Local Login   Is Self Mapping Remote Login   
---------------- ------------- --------------- --------------   
Sales            NULL          1               NULL  
Sales            Mary          0               sa  
  
(2 row(s) affected)  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées de sécurité &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [sp_addlinkedserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md)   
 [sp_droplinkedsrvlogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-droplinkedsrvlogin-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
