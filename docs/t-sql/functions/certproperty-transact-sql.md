---
description: CERTPROPERTY (Transact-SQL)
title: CERTPROPERTY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CERTPROPERTY
- CERTPROPERTY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- certificates [SQL Server], schema names
- schemas [SQL Server], names
- CERTPROPERTY function
ms.assetid: 966c09aa-bc4e-45b0-ba53-c8381871f638
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 3b345594914d4fcb4439def47251cb63c22d07b1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193438"
---
# <a name="certproperty-transact-sql"></a>CERTPROPERTY (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Retourne la valeur d'une propriété de certificat spécifiée.
  
![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
CertProperty ( Cert_ID , '<PropertyName>' )  
  
<PropertyName> ::=  
   Expiry_Date | Start_Date | Issuer_Name   
   | Cert_Serial_Number | Subject | SID | String_SID   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
*Cert_ID*  
Valeur d’ID de certificat, du type de données int.
  
*Expiry_Date*  
Date d'expiration du certificat.
  
*Start_Date*  
Date à laquelle le certificat devient valide.
  
*Issuer_Name*  
Nom de l’émetteur du certificat.
  
*Cert_Serial_Number*  
Numéro de série du certificat.
  
*Subject*  
Objet du certificat
  
 *SID*  
SID du certificat C'est également le SID de n'importe quelle connexion ou utilisateur mappés à ce certificat.
  
*String_SID*  
SID du certificat, sous forme de chaîne de caractères. C'est également le SID de n'importe quelle connexion ou utilisateur mappés à ce certificat.
  
## <a name="return-types"></a>Types de retour
La spécification de la propriété doit être placée dans des guillemets simples.
  
Le type de valeur retournée dépend de la propriété qui est spécifiée dans l’appel de fonction. Le type de retour **sql_variant** wrappe toutes les valeurs de retour.
-   *Expiry_Date* et *Start_Date* renvoient **datetime**.  
-   *Cert_Serial_Number*, *Issuer_Name*, *String_SID*, et *Subject* retournent **nvarchar**.  
-   *SID* renvoie **varbinary**.  
  
## <a name="remarks"></a>Remarques  
Ouvrez la vue de catalogue [sys.certificates](../../relational-databases/system-catalog-views/sys-certificates-transact-sql.md) pour consulter les informations relatives aux certificats.
  
## <a name="permissions"></a>Autorisations  
Nécessite des autorisations sur le certificat, et nécessite que l’appelant ne se soit pas vu refuser l’autorisation VIEW pour le certificat. Pour plus d’informations sur les autorisations de certificat, consultez [CREATE CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/create-certificate-transact-sql.md) et [GRANT CERTIFICATE PERMISSIONS &#40;Transact-SQL&#41;](../../t-sql/statements/grant-certificate-permissions-transact-sql.md).
  
## <a name="examples"></a>Exemples  
L'exemple suivant retourne l'objet du certificat.
  
```sql
-- First create a certificate.  
CREATE CERTIFICATE Marketing19 WITH   
    START_DATE = '04/04/2004' ,  
    EXPIRY_DATE = '07/07/2040' ,  
    SUBJECT = 'Marketing Print Division';  
GO  
  
-- Now use CertProperty to examine certificate  
-- Marketing19's properties.  
DECLARE @CertSubject sql_variant;  
set @CertSubject = CertProperty( Cert_ID('Marketing19'), 'Subject');  
PRINT CONVERT(nvarchar, @CertSubject);  
GO  
```  
  
## <a name="see-also"></a>Voir aussi
[CREATE CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/create-certificate-transact-sql.md)  
[ALTER CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-certificate-transact-sql.md)  
[CERT_ID &#40;Transact-SQL&#41;](../../t-sql/functions/cert-id-transact-sql.md)
[Hiérarchie de chiffrement](../../relational-databases/security/encryption/encryption-hierarchy.md)
[sys.certificates &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-certificates-transact-sql.md)
[Vues de catalogue de sécurité &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)
  
  
