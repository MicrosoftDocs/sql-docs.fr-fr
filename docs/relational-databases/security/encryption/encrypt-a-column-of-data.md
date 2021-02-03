---
title: Chiffrer une colonne de données | Microsoft Docs
description: Découvrez comment chiffrer une colonne de données en utilisant le chiffrement symétrique dans SQL Server à l’aide de Transact-SQL, parfois connu comme chiffrement au niveau de la colonne ou de la cellule.
ms.custom: ''
titleSuffix: SQL Server & Azure Synapse Analytics & Azure SQL Database & SQL Managed Instance
ms.date: 12/15/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- encryption [SQL Server], columns
- cryptography [SQL Server], columns
- column level encryption
- cell level encryption
ms.assetid: 38e9bf58-10c6-46ed-83cb-e2d76cda0adc
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current||=azure-sqldw-latest
ms.openlocfilehash: ae34b381a6f1d46329855662d0a796ea6ab3d265
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250189"
---
# <a name="encrypt-a-column-of-data"></a>Chiffrer une colonne de données

[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]  

Cet article explique comment chiffrer une colonne de données à l’aide du chiffrement symétrique dans [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] avec [!INCLUDE[tsql](../../../includes/tsql-md.md)]. On parle parfois de chiffrement au niveau colonne ou au niveau cellule. Cette fonctionnalité est en préversion pour Azure Synapse Analytics.

Les exemples de cet article ont été validés sur AdventureWorks2017. Pour obtenir des exemples de bases de données, consultez [Exemples de bases de données AdventureWorks](../../../samples/adventureworks-install-configure.md).

## <a name="security"></a>Sécurité  
  
### <a name="permissions"></a>Autorisations  

Les autorisations suivantes sont nécessaires pour effectuer les étapes ci-dessous :  
  
- Une autorisation `CONTROL` au niveau de la base de données.  
- Une autorisation `CREATE CERTIFICATE` au niveau de la base de données. Les connexions Windows, les connexions SQL Server et les rôles d'application sont les seuls à pouvoir posséder des certificats. Les groupes et les rôles ne peuvent pas posséder de certificats.  
- Autorisation `ALTER` sur la table.  
- Autorisation sur la clé, et l’autorisation `VIEW DEFINITION` ne doit pas lui avoir été refusée.  
  
## <a name="create-database-master-key"></a>Créer une clé principale de base de données  

Pour pouvoir utiliser les exemples suivants, vous devez disposer d’une clé principale de base de données. Si votre base de données n’a pas encore de clé principale de base de données, créez-en une. Pour en créer une, connectez-vous à votre base de données et exécutez le script suivant. Veillez à utiliser un mot de passe complexe.

Copiez et collez l’exemple suivant dans la fenêtre de requête qui est connectée à l’exemple de base de données AdventureWorks. Cliquez sur **Exécuter**.  

```sql  
CREATE MASTER KEY ENCRYPTION BY   
PASSWORD = '<complex password>';  
```  

Sauvegardez toujours votre clé principale de base de données. Pour plus d’informations sur les clés principales de base de données, consultez [CREATE MASTER KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/create-master-key-transact-sql.md).

## <a name="example-encrypt-with-symmetric-encryption-and-authenticator"></a>Exemple : Chiffrer avec le chiffrement symétrique et l’authentificateur
  
1. Dans l' **Explorateur d'objets**, connectez-vous à une instance du [!INCLUDE[ssDE](../../../includes/ssde-md.md)].  
  
2. Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3. Copiez et collez l’exemple suivant dans la fenêtre de requête qui est connectée à l’exemple de base de données AdventureWorks. Cliquez sur **Exécuter**.

    ```sql
    CREATE CERTIFICATE Sales09  
       WITH SUBJECT = 'Customer Credit Card Numbers';  
    GO  
  
    CREATE SYMMETRIC KEY CreditCards_Key11  
        WITH ALGORITHM = AES_256  
        ENCRYPTION BY CERTIFICATE Sales09;  
    GO  
  
    -- Create a column in which to store the encrypted data.  
    ALTER TABLE Sales.CreditCard   
        ADD CardNumber_Encrypted varbinary(160);   
    GO  
  
    -- Open the symmetric key with which to encrypt the data.  
    OPEN SYMMETRIC KEY CreditCards_Key11  
       DECRYPTION BY CERTIFICATE Sales09;  
  
    -- Encrypt the value in column CardNumber using the  
    -- symmetric key CreditCards_Key11.  
    -- Save the result in column CardNumber_Encrypted.    
    UPDATE Sales.CreditCard  
    SET CardNumber_Encrypted = EncryptByKey(Key_GUID('CreditCards_Key11')  
        , CardNumber, 1, HASHBYTES('SHA2_256', CONVERT( varbinary  
        , CreditCardID)));  
    GO  
  
    -- Verify the encryption.  
    -- First, open the symmetric key with which to decrypt the data.  
  
    OPEN SYMMETRIC KEY CreditCards_Key11  
       DECRYPTION BY CERTIFICATE Sales09;  
    GO  
  
    -- Now list the original card number, the encrypted card number,  
    -- and the decrypted ciphertext. If the decryption worked,  
    -- the original number will match the decrypted number.  
  
    SELECT CardNumber, CardNumber_Encrypted   
        AS 'Encrypted card number', CONVERT(nvarchar,  
        DecryptByKey(CardNumber_Encrypted, 1 ,   
        HASHBYTES('SHA2_256', CONVERT(varbinary, CreditCardID))))  
        AS 'Decrypted card number' FROM Sales.CreditCard;  
    GO  
    ```  
  
## <a name="encrypt-with-simple-symmetric-encryption"></a>Chiffrer avec le chiffrement symétrique simple  

1. Dans l' **Explorateur d'objets**, connectez-vous à une instance du [!INCLUDE[ssDE](../../../includes/ssde-md.md)].  
  
2. Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3. Copiez et collez l’exemple suivant dans la fenêtre de requête qui est connectée à l’exemple de base de données AdventureWorks. Cliquez sur **Exécuter**.  
  
    ```sql
     CREATE CERTIFICATE HumanResources037  
       WITH SUBJECT = 'Employee Social Security Numbers';  
    GO  
  
    CREATE SYMMETRIC KEY SSN_Key_01  
        WITH ALGORITHM = AES_256  
        ENCRYPTION BY CERTIFICATE HumanResources037;  
    GO  
  
    USE [AdventureWorks2012];  
    GO  
  
    -- Create a column in which to store the encrypted data.  
    ALTER TABLE HumanResources.Employee  
        ADD EncryptedNationalIDNumber varbinary(128);   
    GO  
  
    -- Open the symmetric key with which to encrypt the data.  
    OPEN SYMMETRIC KEY SSN_Key_01  
       DECRYPTION BY CERTIFICATE HumanResources037;  
  
    -- Encrypt the value in column NationalIDNumber with symmetric   
    -- key SSN_Key_01. Save the result in column EncryptedNationalIDNumber.  
    UPDATE HumanResources.Employee  
    SET EncryptedNationalIDNumber = EncryptByKey(Key_GUID('SSN_Key_01'), NationalIDNumber);  
    GO  
  
    -- Verify the encryption.  
    -- First, open the symmetric key with which to decrypt the data.  
    OPEN SYMMETRIC KEY SSN_Key_01  
       DECRYPTION BY CERTIFICATE HumanResources037;  
    GO  
  
    -- Now list the original ID, the encrypted ID, and the   
    -- decrypted ciphertext. If the decryption worked, the original  
    -- and the decrypted ID will match.  
    SELECT NationalIDNumber, EncryptedNationalIDNumber   
        AS 'Encrypted ID Number',  
        CONVERT(nvarchar, DecryptByKey(EncryptedNationalIDNumber))   
        AS 'Decrypted ID Number'  
        FROM HumanResources.Employee;  
    GO  
    ```  
  
 Pour plus d’informations, consultez les rubriques suivantes :  
  
-   [CREATE CERTIFICATE &#40;Transact-SQL&#41;](../../../t-sql/statements/create-certificate-transact-sql.md)  
  
-   [CREATE SYMMETRIC KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/create-symmetric-key-transact-sql.md)  
  
-   [ALTER TABLE &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-table-transact-sql.md)  
  
-   [OPEN SYMMETRIC KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/open-symmetric-key-transact-sql.md)  
