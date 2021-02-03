---
description: VERIFYSIGNEDBYCERT (Transact-SQL)
title: VERIFYSIGNEDBYCERT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- VERIFYSIGNEDBYCERT
- VERIFYSIGNEDBYCERT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- digitally signed data for changes [SQL Server]
- verifying digitally signed data for changes
- testing digitally signed data for changes
- checking digitally signed data for changes
- VERIFYSIGNEDBYCERT
- signatures [SQL Server]
- digital signatures [SQL Server]
ms.assetid: 4e041f33-60c4-4190-91c7-220d51dd6c8f
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 6a0d08e5d4c85e34617685614b4305e3cd7b3529
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210621"
---
# <a name="verifysignedbycert-transact-sql"></a>VERIFYSIGNEDBYCERT (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Teste si les données signées numériquement ont été modifiées depuis leur dernière signature.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
VerifySignedByCert( Cert_ID , signed_data , signature )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *Cert_ID*  
 Identificateur d'un certificat dans la base de données. *Cert_ID* est de type **int**.  
  
 *signed_data*  
 Variable de type **nvarchar**, **char**, **varchar** ou **nchar** qui contient des données qui ont été signées avec un certificat.  
  
 *signature*  
 Signature attachée aux données signées. *signature* est de type **varbinary**.  
  
## <a name="return-types"></a>Types de retour  
 **int**  
  
 Retourne 1 lorsque les données signées n'ont pas changé, sinon 0.  
  
## <a name="remarks"></a>Remarques  
 **VerifySignedBycert** déchiffre la signature des données à l’aide de la clé publique du certificat spécifié, puis compare la valeur déchiffrée à un hachage MD5 des données récemment calculé. Si les valeurs correspondent, la validité de la signature est confirmée.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation VIEW DEFINITION sur le certificat.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-verifying-that-signed-data-has-not-been-tampered-with"></a>R. Vérification de la non-falsification des données signées  
 L'exemple suivant vérifie si les informations contenues dans `Signed_Data` ont été modifiées depuis leur signature à l'aide du certificat `Shipping04`. La signature est stockée dans `DataSignature`. Le certificat, `Shipping04`, est transmis à `Cert_ID` qui retourne l'ID du certificat dans la base de données. Si `VerifySignedByCert` retourne la valeur 1, la signature est correcte. Si `VerifySignedByCert` retourne la valeur 0, les données de `Signed_Data` sont différentes de celles utilisées pour générer `DataSignature`. Dans ce cas, soit les données de `Signed_Data` ont été modifiées depuis leur signature, soit les données de `Signed_Data` ont été signées avec un certificat différent.  
  
```sql
SELECT Data, VerifySignedByCert( Cert_Id( 'Shipping04' ),  
    Signed_Data, DataSignature ) AS IsSignatureValid  
FROM [AdventureWorks2012].[SignedData04]   
WHERE Description = N'data signed by certificate ''Shipping04''';  
GO  
```  
  
### <a name="b-returning-only-records-that-have-a-valid-signature"></a>B. Obtention des seuls enregistrements dont la signature est valide  
 Cette requête retourne uniquement les enregistrements qui n'ont pas été modifiés depuis leur signature à l'aide du certificat `Shipping04`.  
  
```sql
SELECT Data FROM [AdventureWorks2012].[SignedData04]   
WHERE VerifySignedByCert( Cert_Id( 'Shipping04' ), Data,   
    DataSignature ) = 1   
AND Description = N'data signed by certificate ''Shipping04''';  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [CERT_ID &#40;Transact-SQL&#41;](../../t-sql/functions/cert-id-transact-sql.md)   
 [SIGNBYCERT &#40;Transact-SQL&#41;](../../t-sql/functions/signbycert-transact-sql.md)   
 [CREATE CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/create-certificate-transact-sql.md)   
 [ALTER CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-certificate-transact-sql.md)   
 [DROP CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-certificate-transact-sql.md)   
 [BACKUP CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/backup-certificate-transact-sql.md)   
 [Hiérarchie de chiffrement](../../relational-databases/security/encryption/encryption-hierarchy.md)  
  
  
