---
description: HASHBYTES (Transact-SQL)
title: HASHBYTES (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/29/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- HASHBYTES_TSQL
- HASHBYTES
dev_langs:
- TSQL
helpviewer_keywords:
- hash input
- HASHBYTES
ms.assetid: 0ea6a4d1-313e-4f70-b939-dd2cd570f6d6
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: da6f1b1bdb357a6321207720d2ea64938f4ada41
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170241"
---
# <a name="hashbytes-transact-sql"></a>HASHBYTES (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Retourne le hachage MD2, MD4, MD5, SHA, SHA1 ou SHA2 des données d'entrée dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
HASHBYTES ( '<algorithm>', { @input | 'input' } )  
  
<algorithm>::= MD2 | MD4 | MD5 | SHA | SHA1 | SHA2_256 | SHA2_512   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments

`<algorithm>`  
Identifie l'algorithme de hachage à utiliser pour les données d'entrée. Cet argument est obligatoire, sans valeur par défaut. Les guillemets simples sont obligatoires. À partir de [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)], tous les algorithmes autres que SHA2_256 et SHA2_512 sont déconseillés.  
  
`@input`  
Variable contenant les données à hacher. `@input` est de type **varchar**, **nvarchar** ou **varbinary**.  
  
'*input*'  
Spécifie une expression qui correspond à une chaîne de type caractère ou binaire à hacher.  
  
 La sortie se conforme à l'algorithme standard : 128 bits (16 octets) pour MD2, MD4 et MD5 ; 160 bits (20 octets) pour SHA et SHA1 ; 256 bits (32 octets) pour SHA2_256 et 512 bits (64 octets) pour SHA2_512.  
  
**S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et ultérieur
  
 Pour [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions antérieures, les valeurs d’entrée autorisées sont limitées à 8 000 octets.  
  
## <a name="return-value"></a>Valeur de retour  
 **varbinary** (au maximum 8 000 octets)  

## <a name="remarks"></a>Notes  
Envisagez d’utiliser `CHECKSUM` ou `BINARY_CHECKSUM` comme alternatives pour calculer une valeur de hachage.

Les algorithmes de hachage MD2, MD4, MD5, SHA et SHA1 sont dépréciés à partir de [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)]. Utilisez SHA2_256 ou SHA2_512 à la place. Des algorithmes plus anciens continueront de fonctionner, mais ils déclencheront un événement de dépréciation.

## <a name="examples"></a>Exemples  
### <a name="return-the-hash-of-a-variable"></a>Retourne le hachage d'une variable  
 L’exemple suivant renvoie le hachage `SHA2_256` des données **nvarchar** stockées dans la variable `@HashThis`.  
  
```sql  
DECLARE @HashThis NVARCHAR(32);  
SET @HashThis = CONVERT(NVARCHAR(32),'dslfdkjLK85kldhnv$n000#knf');  
SELECT HASHBYTES('SHA2_256', @HashThis);  
```  
  
### <a name="return-the-hash-of-a-table-column"></a>Retourne le hachage d'une colonne de table  
 L'exemple suivant retourne le code de hachage SHA2_256 des valeurs de la colonne `c1` dans la table `Test1`.  
  
```sql  
CREATE TABLE dbo.Test1 (c1 NVARCHAR(32));  
INSERT dbo.Test1 VALUES ('This is a test.');  
INSERT dbo.Test1 VALUES ('This is test 2.');  
SELECT HASHBYTES('SHA2_256', c1) FROM dbo.Test1;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
-------------------------------------------  
0x741238C01D9DB821CF171BF61D72260B998F7C7881D90091099945E0B9E0C2E3 
0x91DDCC41B761ACA928C62F7B0DA61DC763255E8247E0BD8DCE6B22205197154D  
(2 row(s) affected)  
```  
  
## <a name="see-also"></a>Voir aussi  
[Choisir un algorithme de chiffrement](../../relational-databases/security/encryption/choose-an-encryption-algorithm.md)
[CHECKSUM_AGG &#40;Transact-SQL&#41;](../../t-sql/functions/checksum-agg-transact-sql.md)
[CHECKSUM &#40;Transact-SQL&#41;](../../t-sql/functions/checksum-transact-sql.md)
[BINARY_CHECKSUM  &#40;Transact-SQL&#41;](../../t-sql/functions/binary-checksum-transact-sql.md)
