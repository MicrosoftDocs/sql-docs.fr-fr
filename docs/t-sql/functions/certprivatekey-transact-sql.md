---
description: CERTPRIVATEKEY (Transact-SQL)
title: CERTPRIVATEKEY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CERTPRIVATEKEY
- CERTPRIVATEKEY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CERTPRIVATEKEY
ms.assetid: 33e0f01e-39ac-46da-94ff-fe53b1116df4
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: f32b206053cdfbf3f8fce4f8ecc5da69417fc571
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202179"
---
# <a name="certprivatekey-transact-sql"></a>CERTPRIVATEKEY (Transact-SQL)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

Cette fonction retourne la clé privée d’un certificat au format binaire. Cette fonction accepte trois arguments.
-   Un ID de certificat.  
-   Un mot de passe de chiffrement utilisé pour chiffrer les bits de clé privée retournés par la fonction. Cette approche n’expose pas les clés en texte clair aux utilisateurs.  
-   Un mot de passe de déchiffrement facultatif. Un mot de passe de déchiffrement spécifié est utilisé pour déchiffrer la clé privée du certificat. Sinon, la clé principale de la base de données est utilisée.  
  
Seuls les utilisateurs qui ont accès à la clé privée du certificat peuvent utiliser cette fonction. Cette fonction retourne la clé privée au format PVK.
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
CERTPRIVATEKEY   
    (  
          cert_ID   
        , ' encryption_password '   
      [ , ' decryption_password ' ]  
    )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
*certificate_ID*  
**certificate_id** du certificat. Obtenez cette valeur à partir de sys.certificates ou à l’aide de la fonction [CERT_ID &#40;Transact-SQL&#41;](../../t-sql/functions/cert-id-transact-sql.md). *cert_id* a le type de données **int**.
  
*encryption_password*  
Mot de passe utilisé pour chiffrer la valeur binaire retournée.
  
*decryption_password*  
Mot de passe utilisé pour déchiffrer la valeur binaire retournée.
  
## <a name="return-types"></a>Types de retour
**varbinary**
  
## <a name="remarks"></a>Notes  
Les fonctions **CERTENCODED** et **CERTPRIVATEKEY** sont utilisées ensemble pour retourner les différentes parties d’un certificat sous forme binaire.
  
## <a name="permissions"></a>Autorisations  
**CERTPRIVATEKEY** est disponible publiquement.
  
## <a name="examples"></a>Exemples  
  
```sql
CREATE DATABASE TEST1;  
GO  
USE TEST1  
CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'Use 5tr0ng P^55Words'  
GO  
CREATE CERTIFICATE Shipping04   
WITH SUBJECT = 'Sammamish Shipping Records',   
EXPIRY_DATE = '20401031';  
GO  
SELECT CERTPRIVATEKEY(CERT_ID('Shipping04'), 'jklalkaa/; uia3dd');  
```  
  
Pour obtenir un exemple plus complexe qui utilise **CERTPRIVATEKEY** et **CERTENCODED** afin de copier un certificat dans une autre base de données, consultez l’exemple B de la rubrique [CERTENCODED &#40;Transact-SQL&#41;](../../t-sql/functions/certencoded-transact-sql.md).
  
## <a name="see-also"></a>Voir aussi
[Fonctions de sécurité &#40;Transact-SQL&#41;](../../t-sql/functions/security-functions-transact-sql.md)  
[CREATE CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/create-certificate-transact-sql.md)
[Fonctions de sécurité &#40;Transact-SQL&#41;](../../t-sql/functions/security-functions-transact-sql.md)
[sys.certificates &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-certificates-transact-sql.md)
  
  
