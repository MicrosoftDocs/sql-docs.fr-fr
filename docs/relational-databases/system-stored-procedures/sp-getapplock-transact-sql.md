---
description: sp_getapplock (Transact-SQL)
title: sp_getapplock (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_getapplock_TSQL
- sp_getapplock
dev_langs:
- TSQL
helpviewer_keywords:
- application locks
- sp_getapplock
ms.assetid: e1e85908-9f31-47cf-8af6-88c77e6f24c9
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 82c0c343d76c3fb8fe6275c2450b62d50fd11b34
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185766"
---
# <a name="sp_getapplock-transact-sql"></a>sp_getapplock (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Place un verrou sur une ressource d'application.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_getapplock [ @Resource = ] 'resource_name' ,  
     [ @LockMode = ] 'lock_mode'   
     [ , [ @LockOwner = ] 'lock_owner' ]   
     [ , [ @LockTimeout = ] 'value' ]  
     [ , [ @DbPrincipal = ] 'database_principal' ]  
[ ; ]  
```  
  
## <a name="arguments"></a>Arguments  
 [ @Resource =] '*resource_name*'  
 Chaîne de caractères qui indique le nom qui identifie la ressource de verrou. L'application doit vérifier que le nom de ressource est unique. Le nom spécifié est haché en interne en une valeur qui peut être stockée dans le gestionnaire de verrous [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. *resource_name* est de type **nvarchar (255)** et n’a pas de valeur par défaut. Si une chaîne de ressource est plus longue que **nvarchar (255)**, elle sera tronquée à **nvarchar (255)**.  
  
 *resource_name* est comparé en binaire et respecte donc la casse, quels que soient les paramètres de classement de la base de données actuelle.  
  
> [!NOTE]  
>  Une fois qu'un verrou d'application a été acquis, seuls les 32 premiers caractères peuvent être récupérés sous forme de texte brut ; les autres caractères sont hachés.  
  
 [ @LockMode =] '*lock_mode*'  
 Mode de verrouillage à obtenir pour une ressource spécifique. *lock_mode* est de type **varchar (32)** et n’a pas de valeur par défaut. La valeur peut être l’une des suivantes : **Shared**, **Update**, **IntentShared**, **IntentExclusive** ou **exclusive**. Pour plus d’informations, consultez [modes de verrouillage](../sql-server-transaction-locking-and-row-versioning-guide.md#lock_modes).
  
 [ @LockOwner =] '*lock_owner*'  
 Propriétaire du verrou, qui est la valeur de *lock_owner* lorsque le verrou a été demandé. *lock_owner* est **de type varchar (32)**. La valeur peut être **Transaction** (valeur par défaut) ou **Session**. Lorsque la valeur de *lock_owner* est **transaction**, par défaut ou explicitement spécifiée, sp_getapplock doit être exécutée à partir d’une transaction.  
  
 [ @LockTimeout =] '*valeur*'  
 Valeur de délai d'attente de verrou, en millisecondes. La valeur par défaut est la même que la valeur retournée par @ @LOCK_TIMEOUT . Pour indiquer qu’une demande de verrou doit retourner un code de retour de-1 au lieu d’attendre le verrou quand la demande ne peut pas être accordée immédiatement, spécifiez 0.  
  
 [ @DbPrincipal =] '*database_principal*'  
 Utilisateur, rôle ou rôle d'application qui dispose d'autorisations sur un objet d'une base de données. L’appelant de la fonction doit être membre de *database_principal*, dbo ou du rôle de base de données fixe db_owner pour appeler la fonction avec succès. La valeur par défaut est public.  
  
## <a name="return-code-values"></a>Codet de retour  
 \>= 0 (succès), ou < 0 (échec)  
  
|Valeur|Résultats|  
|-----------|------------|  
|0|Le verrou a été accordé de manière synchrone.|  
|1|Le verrou a été accordé après attente de la libération des autres verrous incompatibles.|  
|-1|La demande de verrou a expiré.|  
|-2|La demande de verrou a été annulée.|  
|-3|La demande de verrou a été choisie comme victime du blocage.|  
|-999|Erreur de validation de paramètre ou autre erreur d'appel.|  
  
## <a name="remarks"></a>Notes  
 Les verrous placés sur une ressource sont associés à la transaction en cours ou à la session en cours. Les verrous associés à la transaction en cours sont libérés lorsque la transaction est validée ou annulée. Les verrous associés à la session sont libérés lorsque la session est déconnectée. Lorsque le serveur s’arrête pour une raison quelconque, tous les verrous sont libérés.  
  
 La ressource de verrou créée par sp_getapplock est créée dans la base de données active de la session. Chaque ressource de verrou est identifiée par les valeurs combinées :  
  
-   de l'ID de la base de données contenant la ressource de verrou ;  
  
-   Principal de base de données spécifié dans le paramètre @DbPrincipal.  
  
-   Nom de verrou spécifié dans le paramètre @Resource.  
  
 Seul un membre du principal de la base de données spécifié dans le paramètre @DbPrincipal peut acquérir les verrous d'application qui spécifient ce principal. Les membres des rôles dbo et db_owner sont implicitement considérés comme membres de tous les rôles.  
  
 sp_releaseapplock permet de libérer explicitement les verrous. Si une application utilise plusieurs fois sp_getapplock pour appeler la même ressource de verrou, sp_releaseapplock doit être appelée autant de fois pour libérer le verrou.  Lorsqu’un verrou est ouvert avec le `Transaction` propriétaire du verrou, ce verrou est libéré lorsque la transaction est validée ou annulée.
  
 Si sp_getapplock est appelée plusieurs fois pour la même ressource de verrou, mais que le mode de verrouillage spécifié dans une requête est différent du mode existant, l'effet sur la ressource est l'union des deux modes de verrouillage. Dans la plupart des cas, cela signifie que le mode de verrouillage est converti dans le mode de verrouillage le plus fort entre le mode existant et celui qui vient d'être demandé. Ce mode de verrouillage plus fort est conservé jusqu'à ce que le verrou soit libéré, même si des appels de libération du verrou ont eu lieu auparavant. Par exemple, dans la série d'appels suivante, la ressource est maintenue en mode `Exclusive` au lieu d'utiliser le mode `Shared`.  
  
```  
USE AdventureWorks2012;  
GO  
BEGIN TRANSACTION;  
DECLARE @result int;  
EXEC @result = sp_getapplock @Resource = 'Form1',   
               @LockMode = 'Shared';  
EXEC @result = sp_getapplock @Resource = 'Form1',   
               @LockMode = 'Exclusive';  
EXEC @result = sp_releaseapplock @Resource = 'Form1';  
COMMIT TRANSACTION;  
GO  
```  
  
 Un blocage avec un verrou d'application n'annule pas la transaction qui a demandé le verrou. Toute annulation qui peut être requise en conséquence de la valeur de retour doit être effectuée manuellement. Par conséquent, nous vous recommandons d'inclure un contrôle d'erreur dans le code de façon à ce qu'une instruction ROLLBACK TRANSACTION ou une action équivalente soit exécutée si certaines valeurs sont retournées (-3 par exemple).  
  
 Voici un exemple :  
  
```  
USE AdventureWorks2012;  
GO  
BEGIN TRANSACTION;  
DECLARE @result int;  
EXEC @result = sp_getapplock @Resource = 'Form1',   
               @LockMode = 'Exclusive';  
IF @result = -3  
BEGIN  
    ROLLBACK TRANSACTION;  
END  
ELSE  
BEGIN  
    EXEC @result = sp_releaseapplock @Resource = 'Form1';  
    COMMIT TRANSACTION;  
END;  
GO  
```  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilise l'ID de la base de données active pour qualifier la ressource. Par conséquent, si sp_getapplock est exécutée, le résultat sera des verrous distincts sur des ressources distinctes, même si les valeurs des paramètres sont identiques.  
  
 Utilisez la vue de gestion dynamique sys.dm_tran_locks ou la procédure stockée système sp_lock pour examiner les informations de verrou. Vous pouvez également utiliser [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] pour surveiller les verrous.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle public.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant place un verrou partagé, associé à la transaction en cours, sur la ressource `Form1` de la base de données `AdventureWorks2012`.  
  
```  
USE AdventureWorks2012;  
GO  
BEGIN TRAN;  
DECLARE @result int;  
EXEC @result = sp_getapplock @Resource = 'Form1',   
               @LockMode = 'Shared';  
COMMIT TRAN;  
GO  
```  
  
 L'exemple suivant spécifie `dbo` en tant que principal de base de données.  
  
```  
BEGIN TRAN;  
EXEC sp_getapplock @DbPrincipal = 'dbo', @Resource = 'AdventureWorks2012',   
     @LockMode = 'Shared';  
COMMIT TRAN;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [APPLOCK_MODE &#40;Transact-SQL&#41;](../../t-sql/functions/applock-mode-transact-sql.md)   
 [APPLOCK_TEST &#40;Transact-SQL&#41;](../../t-sql/functions/applock-test-transact-sql.md)   
 [sp_releaseapplock &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-releaseapplock-transact-sql.md)  
  
  
