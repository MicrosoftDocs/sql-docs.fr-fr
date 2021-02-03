---
description: APPLOCK_TEST (Transact-SQL)
title: APPLOCK_TEST (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- APPLOCK_TEST_TSQL
- APPLOCK_TEST
dev_langs:
- TSQL
helpviewer_keywords:
- locking [SQL Server], applications
- APPLOCK_TEST function
- applications [SQL Server], locks
- sessions [SQL Server], application locks
- testing application locks
ms.assetid: 4ea33d04-f8e9-46ff-ae61-985bd3eaca2c
author: cawrites
ms.author: chadam
ms.openlocfilehash: 742673470ba1298a77ed8f67a7097bf835db15d9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202283"
---
# <a name="applock_test-transact-sql"></a>APPLOCK_TEST (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Cette fonction retourne des informations indiquant si un verrou peut ou non être octroyé sur une ressource d'application à un propriétaire de verrou donné, sans acquisition du verrou. En tant que fonction de verrouillage d'application, APPLOCK_TEST agit sur la base de données active. La base de données constitue l'étendue des verrous d'application.
  
![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
APPLOCK_TEST ( 'database_principal' , 'resource_name' , 'lock_mode' , 'lock_owner' )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
**'** *database_principal* **'**  
Utilisateur, rôle ou rôle d’application qui peuvent se voir octroyer des autorisations sur des objets dans la base de données. Pour pouvoir appeler la fonction, l’appelant de la fonction doit être membre du rôle de base de données fixe *database_principal*, dbo ou db_owner.
  
**'** *resource_name* **'**  
Nom de ressource de verrouillage spécifié par l'application cliente. L'application doit garantir un nom de ressource unique. Le nom spécifié est haché en interne en une valeur que le gestionnaire de verrous [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut stocker en interne.  *resource_name* est de type **nvarchar(255)**, sans valeur par défaut. L’argument *resource_name* est évalué en binaire et respecte la casse, quels que soient les paramètres de classement de la base de données active.
  
**'** *lock_mode* **'**  
Mode de verrouillage à obtenir pour une ressource spécifique. L’argument *lock_mode* est de type **nvarchar(32)** et n’a pas de valeur par défaut. La valeur de l’argument *lock_mode* peut être l’une des suivantes : **Shared**, **Update**, **IntentShared**, **IntentExclusive**, **Exclusive**.
  
**'** *lock_owner* **'**  
Propriétaire du verrou, correspondant à la valeur *lock_owner* lorsque le verrou a été demandé. *lock_owner* est de type **nvarchar(32)** et sa valeur peut être **Transaction** (valeur par défaut) ou **Session**. Si la valeur par défaut ou **Transaction** est explicitement spécifiée, la fonction APPLOCK_TEST doit être exécutée à partir d’une transaction.
  
## <a name="return-types"></a>Types de retour
**smallint**
  
## <a name="return-value"></a>Valeur retournée
0 si le verrou ne peut pas être octroyé au propriétaire spécifié ou 1 si le verrou peut être octroyé.
  
## <a name="function-properties"></a>Propriétés de la fonction
**Non déterministe**
  
**Non indexable**
  
**Non parallélisable**
  
## <a name="examples"></a>Exemples  
Deux utilisateurs (**Utilisateur A** et **Utilisateur B**) exécutent la séquence d'instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] suivante dans des sessions différentes.
  
**Utilisateur A** exécute :
  
```sql
USE AdventureWorks2012;  
GO  
BEGIN TRAN;  
DECLARE @result INT;  
EXEC @result=sp_getapplock  
    @DbPrincipal='public',  
    @Resource='Form1',  
    @LockMode='Shared',  
    @LockOwner='Transaction';  
SELECT APPLOCK_MODE('public', 'Form1', 'Transaction');  
GO  
```  
  
**Utilisateur B** exécute ensuite :
  
```sql
Use AdventureWorks2012;  
GO  
BEGIN TRAN;  
SELECT APPLOCK_MODE('public', 'Form1', 'Transaction');  
--Result set: NoLock  
  
SELECT APPLOCK_TEST('public', 'Form1', 'Shared', 'Transaction');  
--Result set: 1 (Lock is grantable.)  
  
SELECT APPLOCK_TEST('public', 'Form1', 'Exclusive', 'Transaction');  
--Result set: 0 (Lock is not grantable.)  
GO  
```  
  
**Utilisateur A** exécute ensuite :
  
```sql
EXEC sp_releaseapplock @Resource='Form1', @DbPrincipal='public';  
GO  
```  
  
**Utilisateur B** exécute ensuite :
  
```sql
SELECT APPLOCK_TEST('public', 'Form1', 'Exclusive', 'Transaction');  
--Result set: '1' (The lock is grantable.)  
GO  
```  
  
Les deux utilisateurs, **Utilisateur A** et **Utilisateur B**, exécutent ensuite :
  
```sql
COMMIT TRAN;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi
[APPLOCK_MODE &#40;Transact-SQL&#41;](../../t-sql/functions/applock-mode-transact-sql.md)  
[sp_getapplock &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-getapplock-transact-sql.md)  
[sp_releaseapplock &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-releaseapplock-transact-sql.md)
