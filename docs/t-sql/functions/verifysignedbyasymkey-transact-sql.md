---
description: VERIFYSIGNEDBYASYMKEY (Transact-SQL)
title: VERIFYSIGNEDBYASYMKEY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- VERIFYSIGNEDBYASYMKEY_TSQL
- VERIFYSIGNEDBYASYMKEY
dev_langs:
- TSQL
helpviewer_keywords:
- verifying digitally signed data for changes
- VERIFYSIGNEDBYASYMKEY
- testing digitally signed data for changes
- checking digitally signed data for changes
- signatures [SQL Server]
- digital signatures [SQL Server]
ms.assetid: 9f7c6e0b-5ba4-4dbb-994d-5bd59f4908de
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 417f8c177c250c6e93257da7a063b21b58241ed8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181428"
---
# <a name="verifysignedbyasymkey-transact-sql"></a>VERIFYSIGNEDBYASYMKEY (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Teste si les données signées numériquement ont été modifiées depuis leur dernière signature.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
VerifySignedByAsymKey( Asym_Key_ID , clear_text , signature )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *Asym_Key_ID*  
 ID d'un certificat de clé asymétrique de la base de données.  
  
 *clear_text*  
 Texte en clair en cours de vérification.  
  
 *signature*  
 Signature attachée aux données signées. *signature* est de type **varbinary**.  
  
## <a name="return-types"></a>Types de retour  
 **int**  
  
 Retourne 1 lorsque les signatures correspondent, sinon 0.  
  
## <a name="remarks"></a>Remarques  
 **VerifySignedByAsymKey** déchiffre la signature des données à l’aide de la clé publique de la clé asymétrique spécifiée, puis compare la valeur déchiffrée à un hachage MD5 des données récemment calculé. Si les valeurs correspondent, la validité de la signature est confirmée.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation VIEW DEFINITION sur la clé asymétrique.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-testing-for-data-with-a-valid-signature"></a>R. Test de données ayant une signature valide  
 Le code exemple suivant retourne 1 si les données sélectionnées n'ont pas été modifiées depuis leur dernière signature avec la clé asymétrique `WillisKey74`. Le code exemple retourne 0 si les données ont été modifiées.  
  
```sql
SELECT Data,  
     VerifySignedByAsymKey( AsymKey_Id( 'WillisKey74' ), SignedData,  
     DataSignature ) as IsSignatureValid  
FROM [AdventureWorks2012].[SignedData04]   
WHERE Description = N'data encrypted by asymmetric key ''WillisKey74''';  
GO  
RETURN;  
```  
  
### <a name="b-returning-a-result-set-that-contains-data-with-a-valid-signature"></a>B. Retour d'un jeu de résultats qui contient des données avec une signature valide  
 L'exemple suivant retourne les lignes dans `SignedData04` qui contiennent des données qui n'ont pas été modifiées depuis leur dernière signature avec la clé asymétrique `WillisKey74`. L'exemple de code appelle la fonction `AsymKey_ID` pour obtenir l'ID de la clé asymétrique à partir de la base de données.  
  
```sql
SELECT Data   
FROM [AdventureWorks2012].[SignedData04]   
WHERE VerifySignedByAsymKey( AsymKey_Id( 'WillisKey74' ), Data,  
     DataSignature ) = 1  
AND Description = N'data encrypted by asymmetric key ''WillisKey74''';  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [ASYMKEY_ID &#40;Transact-SQL&#41;](../../t-sql/functions/asymkey-id-transact-sql.md)   
 [SIGNBYASYMKEY &#40;Transact-SQL&#41;](../../t-sql/functions/signbyasymkey-transact-sql.md)   
 [CREATE ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-asymmetric-key-transact-sql.md)   
 [ALTER ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/alter-asymmetric-key-transact-sql.md)   
 [DROP ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/drop-asymmetric-key-transact-sql.md)   
 [Hiérarchie de chiffrement](../../relational-databases/security/encryption/encryption-hierarchy.md)  
  
  
