---
description: DROP SECURITY POLICY (Transact-SQL)
title: DROP SECURITY POLICY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/11/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DROP_SECURITY_POLICY_TSQL
- DROP SECURITY POLICY
- DROP SECURITY
- DROP_SECURITY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- DROP SECURITY POLICY statement
ms.assetid: 5bd3393d-2fa5-4db0-a69a-a1a72d638e9d
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: a9a0566b1601696dd63579f90b75cb81b6f8709c
ms.sourcegitcommit: 331b8495e4ab37266945c81ff5b93d250bdaa6da
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/20/2020
ms.locfileid: "88646299"
---
# <a name="drop-security-policy-transact-sql"></a>DROP SECURITY POLICY (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

  Supprime une stratégie de sécurité.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
DROP SECURITY POLICY [ IF EXISTS ] [schema_name. ] security_policy_name    
[;]  
```  

## <a name="arguments"></a>Arguments
 *IF EXISTS*  
 **S’applique à**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (de [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] à la [version actuelle](https://go.microsoft.com/fwlink/p/?LinkId=299658)).  
  
 Supprime, de manière conditionnelle, la stratégie de sécurité uniquement si elle existe déjà.  
  
 *schema_name*  
 Nom du schéma auquel appartient la stratégie de sécurité.  
  
 *security_policy_name*  
 Nom de la stratégie de sécurité. Les noms de stratégie de sécurité doivent respecter les règles applicables aux identificateurs et doivent être uniques dans la base de données et pour son schéma.  
  
## <a name="remarks"></a>Notes
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation ALTER ANY SECURITY POLICY et l'autorisation ALTER sur le schéma.  
  
## <a name="example"></a> Exemple  
  
```  
DROP SECURITY POLICY secPolicy;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Sécurité au niveau des lignes](../../relational-databases/security/row-level-security.md)   
 [CREATE SECURITY POLICY &#40;Transact-SQL&#41;](../../t-sql/statements/create-security-policy-transact-sql.md)   
 [ALTER SECURITY POLICY &#40;Transact-SQL&#41;](../../t-sql/statements/alter-security-policy-transact-sql.md)   
 [sys.security_policies &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-security-policies-transact-sql.md)   
 [sys.security_predicates &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-security-predicates-transact-sql.md)  
  
  
