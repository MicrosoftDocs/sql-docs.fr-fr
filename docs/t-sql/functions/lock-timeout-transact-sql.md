---
title: '@@LOCK_TIMEOUT (Transact-SQL) | Microsoft Docs'
ms.custom: ''
ms.date: 09/19/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- '@@LOCK_TIMEOUT'
- '@@LOCK_TIMEOUT_TSQL'
dev_langs:
- TSQL
helpviewer_keywords:
- timeout options [SQL Server], locks
- '@@LOCK_TIMEOUT function'
- current lock time-out setting
- locking [SQL Server], time-outs
ms.assetid: 6bf8bf97-60b8-40c1-b89d-8f5a00bcae2e
author: markingmyname
ms.author: maghan
ms.openlocfilehash: aa8f92f7820954a6bc59e3843f2ba62f80ffe9b0
ms.sourcegitcommit: 768f046107642f72693514f51bf2cbd00f58f58a
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87110881"
---
# <a name="x40x40lock_timeout-transact-sql"></a>&#x40;&#x40;LOCK_TIMEOUT (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Renvoie le paramètre de délai d'attente de verrouillage en cours, en millisecondes, pour la session actuelle.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
@@LOCK_TIMEOUT  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Types de retour
 **entier**  
  
## <a name="remarks"></a>Notes  
 SET LOCK_TIMEOUT permet à une application de définir le délai maximal pendant lequel une instruction doit attendre une ressource bloquée. Si l'attente d'une instruction dépasse la valeur du paramètre LOCK_TIMEOUT, l'instruction bloquée est automatiquement annulée, et un message d'erreur est renvoyé à l'application.  
  
 @@LOCK_TIMEOUT renvoie la valeur -1 si la fonction SET LOCK_TIMEOUT n’a pas encore été exécutée au cours de la session actuelle.  
  
## <a name="examples"></a>Exemples  
 Cet exemple affiche l'ensemble de résultats lorsqu'une valeur LOCK_TIMEOUT n'a pas été définie.  
  
```  
SELECT @@LOCK_TIMEOUT AS [Lock Timeout];  
GO  
```  
  
 Voici l'ensemble de résultats obtenu :  
  
```  
Lock Timeout  
------------  
-1  
```  
  
 Cet exemple définit LOCK_TIMEOUT sur 1 800 millisecondes, puis appelle @@LOCK_TIMEOUT.  
  
```  
SET LOCK_TIMEOUT 1800;  
SELECT @@LOCK_TIMEOUT AS [Lock Timeout];  
GO  
```  
  
 Voici l'ensemble de résultats obtenu :  
  
```  
Lock Timeout  
------------  
1800          
```  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions de configuration &#40;Transact-SQL&#41;](../../t-sql/functions/configuration-functions-transact-sql.md)   
 [SET LOCK_TIMEOUT &#40;Transact-SQL&#41;](../../t-sql/statements/set-lock-timeout-transact-sql.md)  
  
  
