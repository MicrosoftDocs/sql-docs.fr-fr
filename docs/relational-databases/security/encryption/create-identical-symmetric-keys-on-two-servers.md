---
title: Créer des clés symétriques identiques sur deux serveurs | Microsoft Docs
description: Découvrez comment créer des clés symétriques identiques sur deux serveurs dans SQL Server à l’aide de Transact-SQL. Cela prend en charge le chiffrement dans des bases de données ou des serveurs distincts.
ms.custom: ''
ms.date: 05/30/2019
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- symmetric keys [SQL Server], creating
ms.assetid: a13d0b21-a43b-43c0-9c22-7ba8f3d15e80
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 780944c299548aaa9a5664fef4688c11cdf69646
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250335"
---
# <a name="create-identical-symmetric-keys-on-two-servers"></a>Créer des clés symétriques identiques sur deux serveurs
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  Cette rubrique décrit comment créer des clés symétriques identiques sur deux serveurs différents dans [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[tsql](../../../includes/tsql-md.md)]. Pour pouvoir déchiffrer du texte chiffré, vous devez posséder la clé qui a été utilisée pour le chiffrer. Quand le chiffrement et le déchiffrement interviennent dans une seule base de données, la clé est stockée dans la base de données et demeure disponible, en fonction des autorisations, pour le chiffrement et le déchiffrement. Cependant, lorsque le chiffrement et le déchiffrement se produisent dans des bases de données distinctes ou sur des serveurs séparés, la clé stockée dans l’une des bases de données ne peut pas être utilisée dans l’autre base de données.
  
## <a name="before-you-begin"></a>Avant de commencer  
  
### <a name="limitations-and-restrictions"></a>Limitations et restrictions  
  
- Lorsqu'une clé symétrique est créée, elle doit être chiffrée à l'aide de l'un des éléments suivants au moins : certificat, mot de passe, clé symétrique, clé asymétrique ou PROVIDER. La clé peut être soumise à plusieurs chiffrements de chaque type. En d'autres termes, une clé symétrique unique peut être chiffrée à l'aide de plusieurs certificats, mots de passe, clés symétriques et clés asymétriques à la fois.  
  
- Lorsqu'une clé symétrique est chiffrée à l'aide d'un mot de passe à la place de la clé publique de la clé principale de base de données, l'algorithme de chiffrement TRIPLE_DES est utilisé. Pour cette raison, les clés créées à l'aide d'un algorithme de chiffrement renforcé, tel qu'AES, sont elles-mêmes sécurisées par un algorithme plus faible.  
  
## <a name="security"></a>Sécurité  
  
### <a name="permissions"></a>Autorisations  
 Requiert l'autorisation ALTER ANY SYMMETRIC KEY sur la base de données. Si la clause AUTHORIZATION est spécifiée, l'autorisation IMPERSONATE sur l'utilisateur de base de données ou l'autorisation ALTER sur le rôle d'application est requise. Si le chiffrement s'effectue par certificat ou clé asymétrique, l'autorisation VIEW DEFINITION est requise sur le certificat ou la clé asymétrique. Les connexions Windows, les connexions [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] et les rôles d'application sont les seuls à pouvoir posséder des clés symétriques. Les groupes et les rôles ne peuvent pas posséder de clés symétriques.  
  
## <a name="using-transact-sql"></a>Utilisation de Transact-SQL  
  
### <a name="to-create-identical-symmetric-keys-on-two-different-servers"></a>Pour créer des clés symétriques identiques sur deux serveurs différents  
  
1. Dans l' **Explorateur d'objets**, connectez-vous à une instance du [!INCLUDE[ssDE](../../../includes/ssde-md.md)].  
  
2. Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3. Créez une clé en exécutant les instructions CREATE MASTER KEY, CREATE CERTIFICATE et CREATE SYMMETRIC KEY suivantes.  
  
    ```sql
    CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'My p@55w0Rd';  
    GO  
    CREATE CERTIFICATE [cert_keyProtection] WITH SUBJECT = 'Key Protection';  
    GO  
    CREATE SYMMETRIC KEY [key_DataShare] WITH  
        KEY_SOURCE = 'My key generation bits. This is a shared secret!',  
        ALGORITHM = AES_256,   
        IDENTITY_VALUE = 'Key Identity generation bits. Also a shared secret'  
        ENCRYPTION BY CERTIFICATE [cert_keyProtection];  
    GO  
    ```  
  
4. Connectez-vous à une instance de serveur distincte, ouvrez une fenêtre de requête différente et exécutez les instructions SQL ci-dessus pour créer la même clé sur le deuxième serveur.  
  
5. Testez les clés en exécutant d'abord les instructions OPEN SYMMETRIC KEY et SELECT ci-dessous sur le premier serveur.  
  
    ```sql
    OPEN SYMMETRIC KEY [key_DataShare]   
        DECRYPTION BY CERTIFICATE cert_keyProtection;  
    GO  
    SELECT encryptbykey(key_guid('key_DataShare'), 'MyData' )  
    GO  
    -- For example, the output might look like this: 0x2152F8DA8A500A9EDC2FAE26D15C302DA70D25563DAE7D5D1102E3056CE9EF95CA3E7289F7F4D0523ED0376B155FE9C3  
    ```  
  
6. Sur le second serveur, collez le résultat de l'instruction SELECT précédente dans le code suivant comme valeur de `@blob` et exécutez le code suivant pour vérifier que la clé dupliquée peut déchiffrer le texte chiffré.  
  
    ```sql
    OPEN SYMMETRIC KEY [key_DataShare]   
        DECRYPTION BY CERTIFICATE cert_keyProtection;  
    GO  
    DECLARE @blob varbinary(8000);  
    SET @blob = SELECT CONVERT(varchar(8000), decryptbykey(@blob));  
    GO  
    ```  
  
7. Fermez les clés symétriques sur les deux serveurs.  
  
    ```sql
    CLOSE SYMMETRIC KEY [key_DataShare];  
    GO  
    ```  

### <a name="encryption-changes-in-sql-server-2017-cu2"></a>Modification du chiffrement dans SQL Server 2017 CU2

SQL Server 2016 utilise l’algorithme de hachage SHA1 pour son travail de chiffrement. À compter de SQL Server 2017, SHA2 est utilisé à la place. Cela signifie que des étapes supplémentaires peuvent être nécessaires pour que votre installation de SQL Server 2017 puisse déchiffrer les éléments qui ont été chiffrées par SQL Server 2016. Voici les étapes supplémentaires :

- Vérifiez que votre ordinateur SQL Server 2017 dispose au minimum de la Mise à jour cumulative 2 (CU2).
  - Pour obtenir des informations importantes, consultez [Mise à jour cumulative 2 (CU2) pour SQL Server 2017](https://support.microsoft.com/help/4052574).
- Après avoir installé CU2, activez l’indicateur de trace 4631 dans SQL Server 2017 : `DBCC TRACEON(4631, -1);`
  - L’indicateur de trace 4631 est une nouveauté dans SQL Server 2017. L’indicateur de trace 4631 doit être `ON` globalement avant que vous créiez la clé principale, le certificat ou la clé symétrique dans SQL Server 2017. Ainsi, ces éléments créés peuvent interopérer avec SQL Server 2016 et versions antérieures.

Pour plus d’informations, consultez :

- [CORRECTIF : SQL Server 2017 ne peut pas déchiffrer les données chiffrées par les versions antérieures de SQL Server à l’aide de la même clé symétrique](https://support.microsoft.com/help/4053407/sql-server-2017-cannot-decrypt-data-encrypted-by-earlier-versions)
- [Les clés symétriques identiques ne fonctionnent pas entre SQL Server 2017 et une autre version de SQL Server](https://feedback.azure.com/forums/908035-sql-server/suggestions/33116269-identical-symmetric-keys-do-not-work-between-sql-s) <!-- Issue 2225. Thank you Stephen W and Sam Rueby. -->

## <a name="for-more-information"></a>Informations supplémentaires

-   [CREATE MASTER KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/create-master-key-transact-sql.md)  
  
-   [CREATE CERTIFICATE &#40;Transact-SQL&#41;](../../../t-sql/statements/create-certificate-transact-sql.md)  
  
-   [CREATE SYMMETRIC KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/create-symmetric-key-transact-sql.md)  
  
-   [ENCRYPTBYKEY &#40;Transact-SQL&#41;](../../../t-sql/functions/encryptbykey-transact-sql.md)  
  
-   [DECRYPTBYKEY &#40;Transact-SQL&#41;](../../../t-sql/functions/decryptbykey-transact-sql.md)  
  
-   [OPEN SYMMETRIC KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/open-symmetric-key-transact-sql.md)  
