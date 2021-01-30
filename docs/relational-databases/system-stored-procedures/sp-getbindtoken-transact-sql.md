---
description: sp_getbindtoken (Transact-SQL)
title: sp_getbindtoken (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_getbindtoken
- sp_getbindtoken_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_getbindtoken
ms.assetid: 5db87d77-85fa-45a3-a23a-3ea500f9a5ac
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 4613c83384aa7ff2cd36512f6c172a00fbbb047e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185757"
---
# <a name="sp_getbindtoken-transact-sql"></a>sp_getbindtoken (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Retourne un identificateur unique pour la transaction. Il s'agit en fait d'une chaîne utilisée pour lier des sessions à l'aide de sp_bindsession.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Utilisez plutôt MARS (Multiple Active Results Sets) ou des transactions distribuées. Pour plus d’informations, consultez [Utilisation de MARS &#40;Multiple Active Result Sets&#41;](../../relational-databases/native-client/features/using-multiple-active-result-sets-mars.md).  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_getbindtoken [@out_token =] 'return_value' OUTPUT   
```  
  
## <a name="arguments"></a>Arguments  
 [ @out_token =] '*return_value*'  
 Jeton à utiliser pour lier les sessions. *return_value* est de type **varchar (255)** et n’a pas de valeur par défaut.  
  
## <a name="return-code-values"></a>Codet de retour  
 Aucun  
  
## <a name="result-sets"></a>Jeux de résultats  
 None  
  
## <a name="remarks"></a>Notes  
 sp_getbindtoken retourne un jeton valide uniquement lorsque la procédure stockée est exécutée dans une transaction active. Dans le cas contraire, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] retourne un message d’erreur. Par exemple :  
  
```  
-- Declare a variable to hold the bind token.  
-- No active transaction.  
DECLARE @bind_token varchar(255);  
-- Trying to get the bind token returns an error 3921.  
EXECUTE sp_getbindtoken @bind_token OUTPUT;  
Server: Msg 3921, Level 16, State 1, Procedure sp_getbindtoken, Line 4  
Cannot get a transaction token if there is no transaction active.  
Reissue the statement after a transaction has been started.  
```  
  
 Lorsque sp_getbindtoken est utilisé pour inscrire une connexion de transaction distribuée dans une transaction ouverte, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] retourne le même jeton. Par exemple :  
  
```  
USE AdventureWorks2012;  
GO  
DECLARE @bind_token varchar(255);  
  
BEGIN TRAN;  
  
EXECUTE sp_getbindtoken @bind_token OUTPUT;  
SELECT @bind_token AS Token;  
  
BEGIN DISTRIBUTED TRAN;  
  
EXECUTE sp_getbindtoken @bind_token OUTPUT;  
SELECT @bind_token AS Token;  
```  
  
 Les deux instructions `SELECT` retournent le même jeton :  
  
```  
Token  
-----  
PKb'gN5<9aGEedk_16>8U=5---/5G=--  
(1 row(s_) affected)  
  
Token  
-----  
PKb'gN5<9aGEedk_16>8U=5---/5G=--  
(1 row(s_) affected)  
```  
  
 Le jeton de liaison peut être utilisé avec sp_bindsession afin de lier de nouvelles sessions à la même transaction. Le jeton de liaison n’est valide que localement dans chaque instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)] et ne peut pas être partagé entre plusieurs instances.  
  
 Pour obtenir et passer un jeton de liaison, vous devez exécuter sp_getbindtoken avant d'utiliser sp_bindsession afin de partager le même espace de verrouillage. Si vous disposez d'un jeton de liaison, sp_bindsession sera exécuté correctement.  
  
> [!NOTE]  
>  Nous vous conseillons d'utiliser l'API Open Data Services srv_getbindtoken pour obtenir un jeton de liaison susceptible d'être utilisé à partir d'une procédure stockée étendue.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle public.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant demande un jeton de liaison et affiche son nom.  
  
```  
DECLARE @bind_token varchar(255);  
BEGIN TRAN;  
EXECUTE sp_getbindtoken @bind_token OUTPUT;  
SELECT @bind_token AS Token;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 `Token`  
  
 `----------------------------------------------------------`  
  
 `\0]---5^PJK51bP<1F<-7U-]ANZ`  
  
## <a name="see-also"></a>Voir aussi  
 [sp_bindsession &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-bindsession-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [srv_getbindtoken &#40;API de procédure stockée étendue&#41;](../../relational-databases/extended-stored-procedures-reference/srv-getbindtoken-extended-stored-procedure-api.md)  
  
  
