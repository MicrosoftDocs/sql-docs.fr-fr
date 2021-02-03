---
description: ADD SIGNATURE (Transact-SQL)
title: ADD SIGNATURE (Transact-SQL)
ms.prod: sql
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ADD SIGNATURE
- ADD_SIGNATURE_TSQL
helpviewer_keywords:
- ADD SIGNATURE statement
- adding digital signatures
- signatures [SQL Server]
- digital signatures [SQL Server]
ms.assetid: 64d8b682-6ec1-4e5b-8aee-3ba11e72d21f
author: VanMSFT
ms.author: vanto
ms.reviewer: ''
ms.custom: ''
ms.date: 06/10/2020
ms.openlocfilehash: 95bfeb321f43fb860bbbeecb32ac18ec221e5067
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194683"
---
# <a name="add-signature-transact-sql"></a>ADD SIGNATURE (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Ajoute une signature numérique à une procédure stockée, à une fonction, à un assembly ou à un déclencheur DML. Ajoute également une contre-signature à une procédure stockée, à une fonction, à un assembly ou à un déclencheur DML.

![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  

## <a name="syntax"></a>Syntaxe

```syntaxsql
ADD [ COUNTER ] SIGNATURE TO module_class::module_name
    BY <crypto_list> [ ,...n ]
  
<crypto_list> ::=  
    CERTIFICATE cert_name  
    | CERTIFICATE cert_name [ WITH PASSWORD = 'password' ]
    | CERTIFICATE cert_name WITH SIGNATURE = signed_blob
    | ASYMMETRIC KEY Asym_Key_Name  
    | ASYMMETRIC KEY Asym_Key_Name [ WITH PASSWORD = 'password'.]
    | ASYMMETRIC KEY Asym_Key_Name WITH SIGNATURE = signed_blob
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments

*module_class*  
Classe du module auquel la signature est ajoutée. Valeur par défaut pour les modules dont l'étendue concerne le schéma OBJECT.  
  
 *module_name*  
 Nom d'une procédure stockée, d'une fonction, d'un assembly ou d'un déclencheur à signer ou à contre-signer.  
  
 CERTIFICATE *cert_name*  
 Nom d'un certificat avec lequel signer ou contre-signer la procédure stockée, la fonction, l'assembly ou le déclencheur.  
  
 WITH PASSWORD ='*password*'  
 Mot de passe requis pour déchiffrer la clé privée du certificat ou de la clé asymétrique. Cette clause n'est requise que si la clé privée n'est pas protégée par la clé principale de la base de données.  
  
 SIGNATURE =*signed_blob*  
 Spécifie le BLOB (Binary Large Object) signé du module. Cette clause est utile si vous souhaitez livrer un module sans fournir la clé privée. Dans ce cas, seuls le module, la signature et la clé publique sont nécessaires pour ajouter l'objet BLOB signé à une base de données. *signed_blob* est l’objet BLOB proprement dit au format hexadécimal.  
  
 ASYMMETRIC KEY *Asym_Key_Name*  
 Nom d'une clé asymétrique avec laquelle signer ou contresigner la procédure stockée, la fonction, l'assembly ou le déclencheur.  
  
## <a name="remarks"></a>Notes

Le module signé ou contresigné et le certificat ou la clé asymétrique utilisés pour la signature doivent déjà exister. Chaque caractère du module est inclus dans le calcul de la signature, y compris les retours chariot et les sauts de ligne.  
  
 Un module peut être signé et contresigné par n'importe quel nombre de certificats et de clés asymétriques.  
  
 La signature d'un module est supprimée lorsque ce dernier est modifié.  
  
 Si un module contient une clause EXECUTE AS, l'ID de sécurité (SID) du principal est aussi inclus dans le cadre du processus de signature.  
  
> [!CAUTION]
> La signature de module ne doit être utilisée que pour accorder des autorisations, jamais pour en refuser ou en révoquer.  
  
 Ni les déclencheurs de langage de définition de données (DDL) ni les fonctions table incluses ne peuvent être signés.  
  
 Des informations sur les signatures sont visibles dans l'affichage catalogue sys.crypt_properties.  
  
> [!WARNING]
>  Lorsqu'on recrée une procédure pour la signature, toutes les instructions du traitement d'origine doivent correspondre au traitement de recréation. Si une partie du traitement est différée, même au niveau des espaces ou des commentaires, la signature obtenue sera différente.  
  
## <a name="countersignatures"></a>Contre-signatures  
 Lors de l’exécution d’un module signé, les signatures sont ajoutées temporairement au jeton SQL, mais elles sont perdues si le module exécute un autre module ou si l’exécution du module se termine. Une contre-signature est une forme spéciale de signature. Une contre-signature seule n'accorde pas d'autorisations, mais elle permet de conserver les signatures effectuées par le même certificat ou la même clé asymétrique pendant la durée de l'appel passé à l'objet contresigné.  
  
 Par exemple, supposons que l'utilisateur Alice appelle la procédure ProcSelectT1ForAlice, qui appelle la procédure procSelectT1, qui effectue une sélection dans la table T1. Alice a l'autorisation EXECUTE sur ProcSelectT1ForAlice et procSelectT1, mais elle n'a pas d'autorisation SELECT sur T1, et aucun chaînage de propriétés n'est impliqué dans cette chaîne entière. Alice ne peut pas accéder à la table T1, que ce soit directement ou via l'utilisation de ProcSelectT1ForAlice et procSelectT1. Dans la mesure où nous voulons qu’Alice utilise toujours ProcSelectT1ForAlice pour l’accès, nous ne souhaitons pas lui accorder l’autorisation d’exécuter procSelectT1. Comment pouvons-nous effectuer cette opération ?  
  
-   Si nous signons procSelectT1, afin que procSelectT1 puisse accéder à T1, Alice peut appeler procSelectT1 directement et elle n'a pas à appeler ProcSelectT1ForAlice.  
  
-   Nous pouvons refuser l'autorisation EXECUTE sur procSelectT1 à Alice, mais elle ne serait alors pas en mesure d'appeler procSelectT1 via ProcSelectT1ForAlice.
  
-   La signature de ProcSelectT1ForAlice ne fonctionne pas seule, parce qu'elle est perdue dans l'appel à procSelectT1.  
  
Toutefois, en contresignant procSelectT1 avec le même certificat que celui utilisé pour signer ProcSelectT1ForAlice, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] conserve la signature sur la chaîne d’appel et autorise l’accès à T1. Si Alice essaie d'appeler procSelectT1 directement, elle ne peut pas accéder à T1, parce que la contre-signature n'accorde pas de droits. L'exemple C ci-dessous affiche le code [!INCLUDE[tsql](../../includes/tsql-md.md)] pour cet exemple.  
  
## <a name="permissions"></a>Autorisations  

Nécessite l'autorisation ALTER sur l'objet et l'autorisation CONTROL sur le certificat ou la clé asymétrique. Si une clé privée associée est protégée par un mot de passe, l'utilisateur doit également disposer de ce mot de passe.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-signing-a-stored-procedure-by-using-a-certificate"></a>R. Signature d'une procédure stockée à l'aide d'un certificat

 L'exemple suivant signe la procédure stockée `HumanResources.uspUpdateEmployeeLogin` avec le certificat `HumanResourcesDP`.  
  
```sql  
USE AdventureWorks2012;  
ADD SIGNATURE TO HumanResources.uspUpdateEmployeeLogin   
    BY CERTIFICATE HumanResourcesDP;  
GO  
```  
  
### <a name="b-signing-a-stored-procedure-by-using-a-signed-blob"></a>B. Signature d'une procédure stockée à l'aide d'un BLOB signé

L'exemple suivant crée une base de données et un certificat à utiliser dans cet exemple. L'exemple crée et signe une procédure stockée simple et récupère le BLOB de signature de `sys.crypt_properties`. La signature est ensuite supprimée, puis ajoutée de nouveau. L'exemple signe la procédure à l'aide de la syntaxe WITH SIGNATURE.  
  
```sql  
CREATE DATABASE TestSignature ;  
GO  
USE TestSignature ;  
GO  
-- Create a CERTIFICATE to sign the procedure.  
CREATE CERTIFICATE cert_signature_demo   
    ENCRYPTION BY PASSWORD = 'pGFD4bb925DGvbd2439587y'  
    WITH SUBJECT = 'ADD SIGNATURE demo';  
GO  

-- Create a simple procedure.  
CREATE PROC [sp_signature_demo]  
AS  
    PRINT 'This is the content of the procedure.' ;  
GO  
-- Sign the procedure.  
ADD SIGNATURE TO [sp_signature_demo]   
    BY CERTIFICATE [cert_signature_demo]   
    WITH PASSWORD = 'pGFD4bb925DGvbd2439587y' ;  
GO  

-- Get the signature binary BLOB for the sp_signature_demo procedure.  
SELECT cp.crypt_property  
    FROM sys.crypt_properties AS cp  
    JOIN sys.certificates AS cer  
        ON cp.thumbprint = cer.thumbprint  
    WHERE cer.name = 'cert_signature_demo' ;  
GO  
```  
  
 La signature `crypt_property` retournée par cette instruction différera à chaque création de procédure. Notez le résultat afin de le réutiliser plus tard dans cet exemple. Pour cet exemple, le résultat illustré est le suivant : `0x831F5530C86CC8ED606E5BC2720DA835351E46219A6D5DE9CE546297B88AEF3B6A7051891AF3EE7A68EAB37CD8380988B4C3F7469C8EABDD9579A2A5C507A4482905C2F24024FFB2F9BD7A953DD5E98470C4AA90CE83237739BB5FAE7BAC796E7710BDE291B03C43582F6F2D3B381F2102EEF8407731E01A51E24D808D54B373`.  
  
```sql  
-- Drop the signature so that it can be signed again.  
DROP SIGNATURE FROM [sp_signature_demo]   
    BY CERTIFICATE [cert_signature_demo];  
GO  

-- Add the signature. Use the signature BLOB obtained earlier.  
ADD SIGNATURE TO [sp_signature_demo]   
    BY CERTIFICATE [cert_signature_demo]  
    WITH SIGNATURE = 0x831F5530C86CC8ED606E5BC2720DA835351E46219A6D5DE9CE546297B88AEF3B6A7051891AF3EE7A68EAB37CD8380988B4C3F7469C8EABDD9579A2A5C507A4482905C2F24024FFB2F9BD7A953DD5E98470C4AA90CE83237739BB5FAE7BAC796E7710BDE291B03C43582F6F2D3B381F2102EEF8407731E01A51E24D808D54B373 ;  
GO  
```  
  
### <a name="c-accessing-a-procedure-using-a-countersignature"></a>C. Accès à une procédure à l'aide d'une contre-signature

L'exemple suivant montre comment une contre-signature peut aider à contrôler l'accès à un objet.  
  
```sql  
-- Create tesT1 database  
CREATE DATABASE testDB;  
GO  
USE testDB;  
GO  
-- Create table T1  
CREATE TABLE T1 (c VARCHAR(11));  
INSERT INTO T1 VALUES ('This is T1.');  
  
-- Create a TestUser user to own table T1  
CREATE USER TestUser WITHOUT LOGIN;  
ALTER AUTHORIZATION ON T1 TO TestUser;  
  
-- Create a certificate for signing  
CREATE CERTIFICATE csSelectT  
  ENCRYPTION BY PASSWORD = 'SimplePwd01'  
  WITH SUBJECT = 'Certificate used to grant SELECT on T1';  
CREATE USER ucsSelectT1 FROM CERTIFICATE csSelectT;  
GRANT SELECT ON T1 TO ucsSelectT1;  
  
-- Create a principal with low privileges  
CREATE LOGIN Alice WITH PASSWORD = 'SimplePwd01';  
CREATE USER Alice;  
  
-- Verify Alice cannoT1 access T1;  
EXECUTE AS LOGIN = 'Alice';  
    SELECT * FROM T1;  
REVERT;  
  
-- Create a procedure that directly accesses T1  
CREATE PROCEDURE procSelectT1 AS  
BEGIN  
    PRINT 'Now selecting from T1...';  
    SELECT * FROM T1;  
END;  
GO  
GRANT EXECUTE ON procSelectT1 to public;  
  
-- Create special procedure for accessing T1  
CREATE PROCEDURE  procSelectT1ForAlice AS  
BEGIN  
   IF USER_ID() <> USER_ID('Alice')  
    BEGIN  
        PRINT 'Only Alice can use this.';  
        RETURN  
    END  
   EXEC procSelectT1;  
END;  
GO;  
GRANT EXECUTE ON procSelectT1ForAlice TO PUBLIC;  
  
-- Verify procedure works for a sysadmin user  
EXEC procSelectT1ForAlice;  
  
-- Alice still can't use the procedure yet  
EXECUTE AS LOGIN = 'Alice';  
    EXEC procSelectT1ForAlice;  
REVERT;  
  
-- Sign procedure to grant it SELECT permission  
ADD SIGNATURE TO procSelectT1ForAlice BY CERTIFICATE csSelectT   
WITH PASSWORD = 'SimplePwd01';  
  
-- Counter sign proc_select_t, to make this work  
ADD COUNTER SIGNATURE TO procSelectT1 BY CERTIFICATE csSelectT   
WITH PASSWORD = 'SimplePwd01';  
  
-- Now the proc works.   
-- Note that calling procSelectT1 directly still doesn't work  
EXECUTE AS LOGIN = 'Alice';  
    EXEC procSelectT1ForAlice;  
    EXEC procSelectT1;  
REVERT;  
  
-- Cleanup  
USE master;  
GO  
DROP DATABASE testDB;  
DROP LOGIN Alice;  
```  
  
## <a name="see-also"></a>Voir aussi

- [sys.crypt_properties &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-crypt-properties-transact-sql.md)
- [DROP SIGNATURE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-signature-transact-sql.md)
