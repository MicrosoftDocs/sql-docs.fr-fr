---
description: sp_releaseapplock (Transact-SQL)
title: sp_releaseapplock (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_releaseapplock_TSQL
- sp_releaseapplock
dev_langs:
- TSQL
helpviewer_keywords:
- sp_releaseapplock
ms.assetid: 51b03c2f-0d54-40f5-9172-e747942d4a46
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 020a2964a4f73872669a4f9623a8c24aa2a88fd5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185654"
---
# <a name="sp_releaseapplock-transact-sql"></a>sp_releaseapplock (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Libère un verrou appliqué à une ressource d'application.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_releaseapplock [ @Resource = ] 'resource_name'   
     [ , [ @LockOwner = ] 'lock_owner' ]  
     [ , [ @DbPrincipal = ] 'database_principal' ]  
[ ; ]  
```  
  
## <a name="arguments"></a>Arguments  
 [ @Resource =] '*resource_name*'  
 Nom de ressource de verrou spécifié par l'application cliente. L'application doit veiller à ce que la ressource soit unique. Le nom spécifié est haché en interne en une valeur qui peut être stockée dans le gestionnaire de verrous [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. *resource_name* est de type **nvarchar (255)** et n’a pas de valeur par défaut. *resource_name* est comparé en binaire, respecte donc la casse, quels que soient les paramètres de classement de la base de données actuelle.  
  
 [ @LockOwner =] '*lock_owner*'  
 Propriétaire du verrou, qui est la valeur de *lock_owner* lorsque le verrou a été demandé. *lock_owner* est de type **nvarchar(32)**. La valeur peut être **Transaction** (valeur par défaut) ou **Session**. Lorsque la valeur de *lock_owner* est **transaction**, par défaut ou explicitement spécifiée, sp_getapplock doit être exécutée à partir d’une transaction.  
  
 [ @DbPrincipal =] '*database_principal*'  
 Utilisateur, rôle ou rôle d'application qui dispose d'autorisations sur un objet d'une base de données. L’appelant de la fonction doit être membre du rôle de base de données fixe *database_principal*, dbo ou db_owner pour pouvoir appeler la fonction avec succès. La valeur par défaut est public.  
  
## <a name="return-code-values"></a>Codet de retour  
 \>= 0 (succès), ou < 0 (échec)  
  
|Valeur|Résultats|  
|-----------|------------|  
|0|Le verrou a été libéré avec succès.|  
|-999|Indique la validation de paramètre ou une autre erreur d'appel.|  
  
## <a name="remarks"></a>Notes  
 Si une application utilise plusieurs fois sp_getapplock pour appeler la même ressource de verrou, sp_releaseapplock doit être appelée autant de fois pour libérer le verrou.  
  
 Lorsque le serveur s'interrompt pour une raison quelconque, les verrous sont libérés.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle public.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant libère le verrou associé à la transaction active sur la ressource `Form1` de la base de données `AdventureWorks2012`.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_getapplock @DbPrincipal = 'dbo', @Resource = 'Form1',   
     @LockMode = 'Shared';  
EXEC sp_releaseapplock @DbPrincipal = 'dbo', @Resource = 'Form1';  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [APPLOCK_MODE &#40;Transact-SQL&#41;](../../t-sql/functions/applock-mode-transact-sql.md)   
 [APPLOCK_TEST &#40;Transact-SQL&#41;](../../t-sql/functions/applock-test-transact-sql.md)   
 [sp_getapplock &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-getapplock-transact-sql.md)  
  
  
