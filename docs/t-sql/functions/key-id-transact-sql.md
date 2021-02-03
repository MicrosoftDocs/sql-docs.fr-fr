---
description: KEY_ID (Transact-SQL)
title: KEY_ID (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- Key_ID
- Key_ID_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- identification numbers [SQL Server], symmetric keys
- KEY_ID function
- symmetric keys [SQL Server], IDs
- IDs [SQL Server], symmetric keys
ms.assetid: d7309542-dbbe-41dc-b42e-5d9a1c8b4838
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: c1481c85e999ebccdc47e18da315ddd302c77679
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183955"
---
# <a name="key_id-transact-sql"></a>KEY_ID (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Renvoie l'ID d'une clé symétrique dans la base de données actuelle.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
Key_ID ( 'Key_Name' )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 **'** *Key_Name* **'**  
 Nom de la clé symétrique dans la base de données.  
  
## <a name="return-types"></a>Types de retour  
 **int**  
  
## <a name="remarks"></a>Notes  
 Le nom d'une clé temporaire doit commencer par le signe #.  
  
## <a name="permissions"></a>Autorisations  
 Les clés temporaires étant disponibles seulement dans la session où elles sont créées, aucune autorisation n'est nécessaire pour y accéder. Pour accéder à une clé non temporaire, l'appelant doit disposer d'une autorisation sur la clé et l'autorisation VIEW sur la clé ne doit pas lui avoir été refusée.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-returning-the-id-of-a-symmetric-key"></a>R. Renvoi de l'ID d'une clé symétrique  
 L'exemple suivant renvoie l'ID de la clé `ABerglundKey1`.  
  
```sql  
SELECT KEY_ID('ABerglundKey1');  
```  
  
### <a name="b-returning-the-id-of-a-temporary-symmetric-key"></a>B. Renvoi de l'ID d'une clé symétrique temporaire  
 L'exemple suivant renvoie l'ID d'une clé symétrique temporaire. Notez que `#` figure au début du nom de la clé.  
  
```sql  
SELECT KEY_ID('#ABerglundKey2');  
```  
  
## <a name="see-also"></a>Voir aussi  
 [KEY_GUID &#40;Transact-SQL&#41;](../../t-sql/functions/key-guid-transact-sql.md)   
 [CREATE SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-symmetric-key-transact-sql.md)   
 [sys.symmetric_keys &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-symmetric-keys-transact-sql.md)   
 [sys.key_encryptions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-key-encryptions-transact-sql.md)   
 [Hiérarchie de chiffrement](../../relational-databases/security/encryption/encryption-hierarchy.md)  
  
  
