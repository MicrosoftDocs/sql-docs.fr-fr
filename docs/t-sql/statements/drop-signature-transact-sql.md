---
description: DROP SIGNATURE (Transact-SQL)
title: DROP SIGNATURE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DROP SIGNATURE
- DROP_SIGNATURE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- deleting signatures
- dropping signatures
- DROP SIGNATURE statement
- removing signatures
- signatures [SQL Server]
- digital signatures [SQL Server]
ms.assetid: 8a1fd8c5-0e75-4b2f-9d3c-c296bed56cc7
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: fe872b1860091178dadf181fb47c1b887693079c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210960"
---
# <a name="drop-signature-transact-sql"></a>DROP SIGNATURE (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Supprime une signature numérique d'une procédure stockée, d'une fonction, d'un déclencheur ou d'un assembly.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
DROP [ COUNTER ] SIGNATURE FROM module_name   
    BY <crypto_list> [ ,...n ]  
  
<crypto_list> ::=  
    CERTIFICATE cert_name  
    | ASYMMETRIC KEY Asym_key_name  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *module_name*  
 Nom d'une procédure stockée, d'une fonction, d'un assembly ou d'un déclencheur.  
  
 CERTIFICATE *cert_name*  
 Nom d'un certificat dont la procédure stockée, la fonction, l'assembly ou le déclencheur porte la signature.  
  
 ASYMMETRIC KEY *Asym_key_name*  
 Nom d'une clé asymétrique dont la procédure stockée, la fonction, l'assembly ou le déclencheur porte la signature.  
  
## <a name="remarks"></a>Remarques  
 Des informations sur les signatures sont visibles dans l'affichage catalogue sys.crypt_properties.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation ALTER sur l'objet et l'autorisation CONTROL sur le certificat ou la clé asymétrique. Si une clé privée associée est protégée par un mot de passe, l'utilisateur doit également disposer de ce mot de passe.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant supprime la signature du certificat `HumanResourcesDP` de la procédure stockée `HumanResources.uspUpdateEmployeeLogin`.  
  
```sql  
USE AdventureWorks2012;  
DROP SIGNATURE FROM HumanResources.uspUpdateEmployeeLogin   
    BY CERTIFICATE HumanResourcesDP;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sys.crypt_properties &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-crypt-properties-transact-sql.md)   
 [ADD SIGNATURE &#40;Transact-SQL&#41;](../../t-sql/statements/add-signature-transact-sql.md)  
  
  
