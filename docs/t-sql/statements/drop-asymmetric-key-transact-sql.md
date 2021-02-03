---
description: DROP ASYMMETRIC KEY (Transact-SQL)
title: DROP ASYMMETRIC KEY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DROP ASYMMETRIC KEY
- DROP_ASYMMETRIC_KEY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- asymmetric keys [SQL Server], removing
- removing asymmetric keys
- encryption [SQL Server], asymmetric keys
- DROP ASYMMETRIC KEY statement
- dropping asymmetric keys
- deleting asymmetric keys
- cryptography [SQL Server], asymmetric keys
ms.assetid: bf94ac07-9b62-4318-b55b-1eed8f3a1ac6
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 75c47b56394c8bd2f9ea7e8fffa3de72584ebe34
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200373"
---
# <a name="drop-asymmetric-key-transact-sql"></a>DROP ASYMMETRIC KEY (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Supprime une clé asymétrique de la base de données.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
DROP ASYMMETRIC KEY key_name [ REMOVE PROVIDER KEY ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *key_name*  
 Nom de la clé asymétrique à supprimer de la base de données.  
  
 REMOVE PROVIDER KEY  
 Supprime une clé EKM (Gestion de clés extensible) d'un périphérique EKM. Pour plus d’informations sur la gestion de clés extensible, consultez [Gestion de clés extensible &#40;EKM&#41;](../../relational-databases/security/encryption/extensible-key-management-ekm.md).  
  
## <a name="remarks"></a>Notes  
 Une clé asymétrique avec laquelle une clé symétrique de la base de données a été chiffrée, ou sur laquelle un utilisateur ou une connexion est mappée, ne peut pas être supprimée. Avant de supprimer une telle clé, vous devez supprimer tout utilisateur ou connexion mappée sur cette clé. Vous devez également supprimer ou modifier toute clé symétrique chiffrée avec la clé asymétrique. Vous pouvez utiliser l’option DROP ENCRYPTION de l’instruction [ALTER SYMMETRIC KEY](../../t-sql/statements/alter-symmetric-key-transact-sql.md) pour supprimer le chiffrement par une clé asymétrique.  
  
 Les métadonnées des clés asymétriques sont accessibles par le biais de la vue de catalogue [sys.asymmetric_keys](../../relational-databases/system-catalog-views/sys-asymmetric-keys-transact-sql.md). Il n'est pas possible d'afficher les clés elles-mêmes directement à partir de la base de données.  
  
 Si la clé asymétrique est mappée à une clé EKM (Gestion de clés extensible) sur un périphérique EKM et que l'option REMOVE PROVIDER KEY n'est pas spécifiée, la clé sera supprimée de la base de données, mais pas du périphérique. Un avertissement sera émis.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation CONTROL sur la clé asymétrique.  
  
## <a name="examples"></a>Exemples  
 Le code exemple suivant supprime la clé asymétrique `MirandaXAsymKey6` de la base de données `AdventureWorks2012`.  
  
```sql  
USE AdventureWorks2012;  
DROP ASYMMETRIC KEY MirandaXAsymKey6;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [CREATE ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-asymmetric-key-transact-sql.md)   
 [ALTER ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/alter-asymmetric-key-transact-sql.md)   
 [Hiérarchie de chiffrement](../../relational-databases/security/encryption/encryption-hierarchy.md)   
 [ALTER SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/alter-symmetric-key-transact-sql.md)  
  
  
