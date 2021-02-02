---
description: ENCRYPTBYASYMKEY (Transact-SQL)
title: ENCRYPTBYASYMKEY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ENCRYPTBYASYMKEY
- ENCRYPTBYASYMKEY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- ENCRYPTBYASYMKEY function
- encryption [SQL Server], asymmetric keys
- asymmetric keys [SQL Server], ENCRYPTBYASYMKEY function
ms.assetid: 86bb2588-ab13-4db2-8f3c-42c9f572a67b
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 3c947213e205a823e16fd2f83256c452c6da0f19
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99158336"
---
# <a name="encryptbyasymkey-transact-sql"></a>ENCRYPTBYASYMKEY (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Cette fonction chiffre les données avec une clé asymétrique.  
  
 ![Icône Lien d’article](../../database-engine/configure-windows/media/topic-link.gif "Icône Lien d’article") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
EncryptByAsymKey ( Asym_Key_ID , { 'plaintext' | @plaintext } )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
*asym_key_ID*  
ID d’une clé asymétrique dans la base de données. *asym_key_ID* a le type de données **int**.  
  
*texte clair*  
Chaîne de données que `ENCRYPTBYASYMKEY` chiffre avec la clé asymétrique. *cleartext* peut avoir le type de données
 
+ **binary**
+ **char**
+ **nchar**
+ **nvarchar**
+ **varbinary**
  
ou
  
+ **varchar**
 
.  
  
**\@plaintext**  
Variable contenant une valeur que `ENCRYPTBYASYMKEY` chiffre avec la clé asymétrique. **\@plaintext** peut avoir un type de données
  
+ **binary**
+ **char**
+ **nchar**
+ **nvarchar**
+ **varbinary**
  
ou
  
+ **varchar**
 
.  
  
## <a name="return-types"></a>Types de retour  
**varbinary** d’une taille maximale de 8 000 octets.  
  
## <a name="remarks"></a>Notes  
Les opérations de chiffrement et de déchiffrement qui utilisent des clés asymétriques consomment une grande quantité de ressources. Elles deviennent donc coûteuses par rapport au chiffrement et au déchiffrement à clé symétrique. Nous recommandons aux développeurs d’éviter les opérations de chiffrement et de déchiffrement à clé asymétrique sur les jeux de données volumineux (par exemple, les jeux de données utilisateur stockés dans des tables de base de données). Au lieu de cela, nous recommandons donc aux développeurs de commencer par chiffrer ces données avec une clé symétrique forte, puis de chiffrer cette clé symétrique avec une clé asymétrique.  
  
En fonction de l’algorithme, `ENCRYPTBYASYMKEY` retourne **NULL** si l’entrée dépasse un certain nombre d’octets. Les limites spécifiques sont les suivantes :

+ une clé RSA de 512 bits peut chiffrer jusqu’à 53 octets
+ une clé RSA de 1 024 bits peut chiffrer jusqu’à 117 octets
+ une clé RSA de 2 048 bits peut chiffrer jusqu’à 245 octets

Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], les certificats et les clés asymétriques servent de wrappers sur les clés RSA.  
  
## <a name="examples"></a>Exemples  
L’exemple suivant chiffre le texte stocké dans `@cleartext` avec la clé asymétrique `JanainaAsymKey02`. L’instruction insère les données chiffrées dans la table `ProtectedData04`.  
  
```sql  
INSERT INTO AdventureWorks2012.Sales.ProtectedData04   
    VALUES( N'Data encrypted by asymmetric key ''JanainaAsymKey02''',  
    EncryptByAsymKey(AsymKey_ID('JanainaAsymKey02'), @cleartext) );  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [DECRYPTBYASYMKEY &#40;Transact-SQL&#41;](../../t-sql/functions/decryptbyasymkey-transact-sql.md)   
 [CREATE ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-asymmetric-key-transact-sql.md)   
 [Hiérarchie de chiffrement](../../relational-databases/security/encryption/encryption-hierarchy.md)  
  
