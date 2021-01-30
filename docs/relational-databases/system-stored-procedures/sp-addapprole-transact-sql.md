---
description: sp_addapprole (Transact-SQL)
title: sp_addapprole (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_addapprole_TSQL
- sp_addapprole
dev_langs:
- TSQL
helpviewer_keywords:
- sp_addapprole
ms.assetid: 24200295-9a54-4cab-9922-fb2e88632721
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 5a48cfb80bab8e9a754cbcb8186de8cb2955f096
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182855"
---
# <a name="sp_addapprole-transact-sql"></a>sp_addapprole (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Ajoute un rôle d'application à la base de données active.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Utilisez à la place [Create application Role](../../t-sql/statements/create-application-role-transact-sql.md) .  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_addapprole [ @rolename = ] 'role' , [ @password = ] 'password'  
```  
  
## <a name="arguments"></a>Arguments  
`[ @rolename = ] 'role'` Nom du nouveau rôle d’application. *role* est de **type sysname**, sans valeur par défaut. le *rôle* doit être un identificateur valide et ne peut pas déjà exister dans la base de données actuelle.  
  
 Les rôles d'application peuvent contenir de 1 à 128 caractères, y compris les lettres, les symboles et les nombres. Les noms de rôles ne peuvent pas contenir de barre oblique inverse ( \\ ) ni de valeur null ou de chaîne vide (' ').  
  
`[ @password = ] 'password'` Mot de passe requis pour activer le rôle d’application. *Password* est de **type sysname**, sans valeur par défaut. le *mot de passe* ne peut pas être null.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="remarks"></a>Notes  
 Dans les versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], les utilisateurs (et leurs rôles) ne sont pas complètement distincts des schémas. À compter de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)], les schémas sont complètement distincts des rôles. Cette nouvelle architecture se manifeste dans le comportement de CREATE APPLICATION ROLE. Cette instruction remplace **sp_addapprole**.  
  
 Pour assurer la compatibilité descendante avec les versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , **sp_addapprole** effectuera les opérations suivantes :  
  
-   S'il n'existe pas encore de schéma portant le même nom que le rôle d'application, ce schéma est créé. Le nouveau schéma est la propriété du rôle d'application et devient le schéma par défaut de ce rôle.  
  
-   S'il existe déjà un schéma portant le même nom que le rôle d'application, la procédure échoue.  
  
-   La complexité du mot de passe n’est pas vérifiée par **sp_addapprole**. contrairement à CREATE APPLICATION ROLE.  
  
 Le *mot de passe* du paramètre est stocké sous la forme d’un hachage unidirectionnel.  
  
 La procédure stockée **sp_addapprole** ne peut pas être exécutée à partir d’une transaction définie par l’utilisateur.  
  
> [!IMPORTANT]  
>  L’option Microsoft ODBC **Encrypt** n’est pas prise en charge par **SqlClient**. Invitez si possible les utilisateurs à entrer les informations d'identification des rôles d'application au moment de l'exécution. Évitez de stocker ces informations dans un fichier. Si vous devez les conserver, chiffrez-les à l'aide des fonctions CryptoAPI.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation ALTER ANY APPLICATION ROLE sur la base de données. S'il n'existe pas encore de schéma présentant le même nom et le même propriétaire que le nouveau rôle, l'autorisation CREATE SCHEMA sur la base de données est également requise.  
  
## <a name="examples"></a>Exemples  
 L’exemple suivant ajoute le nouveau rôle d’application `SalesApp` avec le mot de passe `x97898jLJfcooFUYLKm387gf3` à la base de données active.  
  
```  
EXEC sp_addapprole 'SalesApp', 'x97898jLJfcooFUYLKm387gf3' ;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [CREATE APPLICATION ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-application-role-transact-sql.md)  
  
  
