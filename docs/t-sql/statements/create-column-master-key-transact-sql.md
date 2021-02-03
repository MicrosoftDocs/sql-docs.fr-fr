---
description: CREATE COLUMN MASTER KEY (Transact-SQL)
title: CREATE COLUMN MASTER KEY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/15/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SQL13.SWB.NEWCOLUMNMASTERKEYDEF.GENERAL.F1
- SQL13.SWB.COLUMNMASTERKEYDEF.GENERAL.F1
- CREATE COLUMN MASTER KEY
- COLUMN MASTER KEY
- CREATE_COLUMN_MASTER_KEY_TSQL
- COLUMN_MASTER_KEY_TSQL
- SQL13.SWB.NEWCOLUMNMASTERKEY.GENERAL.F1
- SQL13.SWB.COLUMNMASTERKEY.GENERAL.F1
dev_langs:
- TSQL
helpviewer_keywords:
- column master key definition
- column master key, create
- CREATE COLUMN MASTER KEY statement
- Always Encrypted, create column master key
ms.assetid: f8926b95-e146-4e3f-b56b-add0c0d0a30e
author: jaszymas
ms.author: jaszymas
ms.openlocfilehash: 6e18a1e0721428dcbfdb649be46ca83fd0d42972
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204940"
---
# <a name="create-column-master-key-transact-sql"></a>CREATE COLUMN MASTER KEY (Transact-SQL)
[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]


Crée un objet de métadonnées de clé principale de colonne dans une base de données. Une entrée de métadonnées de clé principale de colonne représente une clé stockée dans un magasin de clés externes. Cette clé protège (chiffre) les clés de chiffrement de colonne lorsque vous utilisez [Always Encrypted](../../relational-databases/security/encryption/always-encrypted-database-engine.md) ou [Always Encrypted avec enclaves sécurisées](../../relational-databases/security/encryption/always-encrypted-enclaves.md). Plusieurs clés principales de colonne autorisent la rotation périodique de clés pour améliorer la sécurité. Créez une clé principale de colonne dans un magasin de clés et son objet de métadonnées associé dans la base de données en utilisant l’Explorateur d’objets de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou PowerShell. Pour plus d’informations, consultez [Vue d’ensemble de la gestion des clés pour Always Encrypted](../../relational-databases/security/encryption/overview-of-key-management-for-always-encrypted.md).  
  
![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
 

> [!IMPORTANT]
> La création de clés prenant en charge les enclaves (avec ENCLAVE_COMPUTATIONS) nécessite [Always Encrypted avec enclaves sécurisées](../../relational-databases/security/encryption/always-encrypted-enclaves.md).

## <a name="syntax"></a>Syntaxe  

```syntaxsql
CREATE COLUMN MASTER KEY key_name   
    WITH (  
        KEY_STORE_PROVIDER_NAME = 'key_store_provider_name',  
        KEY_PATH = 'key_path'   
        [,ENCLAVE_COMPUTATIONS (SIGNATURE = signature)]
         )   
[;]  
```  

## <a name="arguments"></a>Arguments
*key_name*  
Nom de la clé principale de colonne dans la base de données.  
  
*key_store_provider_name*  
Spécifie le nom du fournisseur de magasin de clés. Un fournisseur de magasin de clés est un composant logiciel client qui encapsule un magasin de clés contenant la clé principale de colonne. 

Un pilote client activé avec Always Encrypted :

- Utilise le nom du fournisseur de magasin de clés 
- Recherche le fournisseur de magasin de clés dans le registre du pilote des fournisseurs de magasin de clés 

Ensuite, le pilote utilise le fournisseur pour déchiffrer les clés de chiffrement de colonne. Les clés de chiffrement de colonne sont protégées par une clé principale de colonne. La clé principale de colonne est stockée dans le magasin de clés sous-jacent. Une valeur en texte clair de la clé de chiffrement de colonne est ensuite utilisée pour chiffrer les paramètres de la requête qui correspondent aux colonnes de base de données chiffrées. Autre possibilité : la clé de chiffrement de colonne déchiffre les résultats de la requête à partir des colonnes chiffrées.  
  
Les bibliothèques de pilotes clients compatibles avec Always Encrypted incluent des fournisseurs de magasin de clés pour les magasins de clés les plus populaires.   
  
Un ensemble de fournisseurs disponibles dépend du type et de la version du pilote client. Pour plus d’informations sur certains pilotes, consultez la documentation d’Always Encrypted : [Développer des applications avec Always Encrypted](../../relational-databases/security/encryption/always-encrypted-client-development.md).


La table suivante affiche le nom des fournisseurs de système :  
  
|Nom du fournisseur de magasin de clés|Magasin de clés sous-jacent|  
    |-----------------------------|--------------------------|
    |'MSSQL_CERTIFICATE_STORE'|Magasin de certificats Windows| 
    |'MSSQL_CSP_PROVIDER'|Un magasin, tel qu’un module de sécurité matériel (HSM), qui prend en charge Microsoft CryptoAPI.|
    |'MSSQL_CNG_STORE'|Un magasin, tel qu’un module de sécurité matériel (HSM), qui prend en charge l’API CNG (Cryptography Next Generation).|  
    |'AZURE_KEY_VAULT'|Voir [Prise en main du coffre de clés Azure](/azure/key-vault/general/overview).|  
    |'MSSQL_JAVA_KEYSTORE'| Magasin de clés Java.}
  

Dans votre pilote client compatible avec Always Encrypted, vous pouvez implémenter un fournisseur de magasin de clés personnalisé qui stocke les clés principales de colonne pour lesquelles il n’existe aucun fournisseur de magasin de clés intégré. Les noms des fournisseurs de magasin de clés personnalisé ne peuvent pas commencer par 'MSSQL_', qui est un préfixe réservé aux fournisseurs de magasin de clés [!INCLUDE[msCoName](../../includes/msconame-md.md)]. 

  
key_path  
Chemin de la clé dans le magasin de clés principales de colonne. Le chemin de clé doit être valide pour chaque application cliente censée chiffrer ou déchiffrer des données. Les données sont stockées dans une colonne qui est protégée (indirectement) par la clé principale de colonne référencée. L’application cliente doit avoir accès à la clé. Le format du chemin de clé est propre au fournisseur de magasin de clés. La liste suivante décrit le format des chemins de clés pour des fournisseurs de magasins de clés de système Microsoft particuliers.  
  
-   **Nom du fournisseur :** MSSQL_CERTIFICATE_STORE  
  
    **Format du chemin de clé :** *nom_magasin_certificats*/*emplacement_magasin_certificats*/*empreinte_certificat*  
  
     Où :  
  
    *emplacement_magasin_certificats*  
    Emplacement du magasin de certificats, qui doit être Current User ou Local Machine. Pour plus d’informations, consultez [Local Machine and Current User Certificate Stores](/windows-hardware/drivers/install/local-machine-and-current-user-certificate-stores).  
  
    *nom_magasin_certificats*  
    Nom du magasin de certificats, par exemple 'My'.  
  
    *empreinte_certificat*  
    Empreinte du certificat.  
  
    **Exemples :**  
  
    ```  
    N'CurrentUser/My/BBF037EC4A133ADCA89FFAEC16CA5BFA8878FB94'  
  
    N'LocalMachine/My/CA5BFA8878FB94BBF037EC4A133ADCA89FFAEC16'  
    ```  
  
-   **Nom du fournisseur :** MSSQL_CSP_PROVIDER  
  
    **Format du chemin de clé :** *nom_fournisseur*/*identificateur_clé*  
  
    Où :  
  
    *ProviderName*  
    Nom d’un fournisseur de service de chiffrement (CSP), qui implémente CAPI pour le magasin de clés principales de la colonne. Si vous utilisez un module HSM comme magasin de clés, le nom du fournisseur CSP doit être celui qui vous a fourni votre module HSM. Le fournisseur doit être installé sur un ordinateur client.  
  
    *identificateur_clé*  
    Identificateur de la clé, utilisé comme clé principale de colonne, dans le magasin de clés.  
  
    **Exemples :**  
  
    ```  
    N'My HSM CSP Provider/AlwaysEncryptedKey1'  
    ```  
  
-   **Nom du fournisseur :** MSSQL_CNG_STORE  
  
    **Format du chemin de clé :** *nom_fournisseur*/*identificateur_clé*  
  
    Où :  
  
    *ProviderName*  
    Nom du fournisseur de stockage de clés (KSP), qui implémente l’API CNG (Cryptography Next Generation), pour le magasin de clés principales de colonne. Si vous utilisez un module HSM comme magasin de clés, le nom du fournisseur doit être celui du fournisseur KSP qui vous a fourni votre module HSM. Le fournisseur doit être installé sur un ordinateur client.  
  
    *identificateur_clé*  
    Identificateur de la clé, utilisé comme clé principale de colonne, dans le magasin de clés.  
  
    **Exemples :**  
  
    ```  
    N'My HSM CNG Provider/AlwaysEncryptedKey1'  
    ```  

-   **Nom du fournisseur :** AZURE_KEY_STORE  
  
    **Format du chemin de clé :** *URL_clé*  
  
    Où :  
  
    *URL_clé*  
    URL de la clé dans Azure Key Vault.

ENCLAVE_COMPUTATIONS  
Spécifie que la clé principale de colonne prend en charge les enclaves. Vous pouvez partager toutes les clés de chiffrement de colonne, chiffrées avec la clé principale de colonne avec une enclave sécurisée côté serveur, et les utiliser pour effectuer des calculs à l’intérieur de l’enclave. Pour plus d’informations, consultez [Always Encrypted avec enclaves sécurisées](../../relational-databases/security/encryption/always-encrypted-enclaves.md).

*signature*  
Un littéral binaire qui est un résultat du *chemin de clé* fournissant la signature numérique et du paramètre ENCLAVE_COMPUTATIONS avec la clé principale de colonne. La signature indique si ENCLAVE_COMPUTATIONS est spécifié ou non. La signature empêche les valeurs signées d’être altérées par des utilisateurs non autorisés. Un pilote client prenant en charge Always Encrypted vérifie la signature et renvoie une erreur à l’application si la signature n’est pas valide. La signature doit être générée à l’aide d’outils côté client. Pour plus d’informations, consultez [Always Encrypted avec enclaves sécurisées](../../relational-databases/security/encryption/always-encrypted-enclaves.md).

## <a name="remarks"></a>Notes

Créez une entrée de métadonnées de clé principale de colonne avant de créer une entrée de métadonnées de clé de chiffrement de colonne dans la base de données et avant que toute colonne dans la base de données puisse être chiffrée à l’aide d’Always Encrypted. Une entrée de clé principale de colonne dans les métadonnées ne contient pas la clé principale de colonne réelle. La clé principale de colonne doit être stockée dans un magasin de clés de colonne externe (en dehors de SQL Server). Le nom du fournisseur de magasin de clés et le chemin de clé principale de colonne dans les métadonnées doivent être valides pour une application cliente. L’application cliente doit utiliser la clé principale de colonne pour déchiffrer une clé de chiffrement de colonne. La clé de chiffrement de colonne est chiffrée avec la clé principale de colonne. L’application cliente doit également interroger les colonnes chiffrées.

Nous vous recommandons d’utiliser des outils, tels que SQL Server Management Studio (SSMS) ou PowerShell, pour gérer les clés principales de colonne. Ces outils génèrent des signatures (si vous utilisez Always Encrypted avec des enclaves sécurisées) et émettent automatiquement des instructions `CREATE COLUMN MASTER KEY` pour créer des objets de métadonnées de clé de chiffrement de colonne. Consultez [Provisionner des clés Always Encrypted avec SQL Server Management Studio](../../relational-databases/security/encryption/configure-always-encrypted-keys-using-ssms.md) et [Provisionner des clés Always Encrypted avec PowerShell](../../relational-databases/security/encryption/configure-always-encrypted-keys-using-powershell.md). 

  
## <a name="permissions"></a>Autorisations  
Nécessite l’autorisation **ALTER ANY COLUMN MASTER KEY**.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-creating-a-column-master-key"></a>R. Création d’une clé principale de colonne  
L’exemple suivant crée une entrée de métadonnées de clé principale de colonne avec pour une clé principale de colonne. La clé principale de colonne est stockée dans le magasin de certificats des applications clientes qui utilisent le fournisseur MSSQL_CERTIFICATE_STORE pour accéder à la clé principale de colonne :  
  
```sql  
CREATE COLUMN MASTER KEY MyCMK  
WITH (  
     KEY_STORE_PROVIDER_NAME = N'MSSQL_CERTIFICATE_STORE',   
     KEY_PATH = 'Current User/Personal/f2260f28d909d21c642a3d8e0b45a830e79a1420'  
   );  
```  
  
Créez une entrée de métadonnées de clé principale de colonne pour une clé principale de colonne. Les applications clientes qui utilisent le fournisseur MSSQL_CNG_STORE ont accès à la clé principale de colonne :  
  
```sql  
CREATE COLUMN MASTER KEY MyCMK  
WITH (  
    KEY_STORE_PROVIDER_NAME = N'MSSQL_CNG_STORE',    
    KEY_PATH = N'My HSM CNG Provider/AlwaysEncryptedKey'  
);  
```  
  
Créez une entrée de métadonnées de clé principale de colonne pour une clé principale de colonne. La clé principale de colonne est stockée dans Azure Key Vault, pour les applications clientes qui utilisent le fournisseur AZURE_KEY_VAULT, afin d’accéder à la clé principale de colonne.  
  
```sql  
CREATE COLUMN MASTER KEY MyCMK  
WITH (  
    KEY_STORE_PROVIDER_NAME = N'AZURE_KEY_VAULT',  
    KEY_PATH = N'https://myvault.vault.azure.net:443/keys/  
        MyCMK/4c05f1a41b12488f9cba2ea964b6a700');  
```  
  
Créez une entrée de métadonnées de clé principale de colonne pour une clé principale de colonne. La clé principale de colonne est stockée dans un magasin de clés principales de colonne personnalisé :  
  
```sql  
CREATE COLUMN MASTER KEY MyCMK  
WITH (  
    KEY_STORE_PROVIDER_NAME = 'CUSTOM_KEY_STORE',    
    KEY_PATH = 'https://contoso.vault/sales_db_tce_key'  
);  
```  
### <a name="b-creating-an-enclave-enabled-column-master-key"></a>B. Création d’une clé principale de colonne prenant en charge les enclaves  
L’exemple suivant crée une entrée de métadonnées de clé principale de colonne pour une clé principale de colonne prenant en charge les enclaves. La clé principale de colonne prenant en charge les enclaves est stockée dans le magasin de certificats, pour les applications clientes qui utilisent le fournisseur MSSQL_CERTIFICATE_STORE afin d’accéder à la clé principale de colonne :  
  
```sql  
CREATE COLUMN MASTER KEY MyCMK  
WITH (  
     KEY_STORE_PROVIDER_NAME = N'MSSQL_CERTIFICATE_STORE',   
     KEY_PATH = 'Current User/Personal/f2260f28d909d21c642a3d8e0b45a830e79a1420'  
     ENCLAVE_COMPUTATIONS (SIGNATURE = 0xA80F5B123F5E092FFBD6014FC2226D792746468C901D9404938E9F5A0972F38DADBC9FCBA94D9E740F3339754991B6CE26543DEB0D094D8A2FFE8B43F0C7061A1FFF65E30FDDF39A1B954F5BA206AAC3260B0657232020542419990261D878318CC38EF4E853970ED69A8D4A306693B8659AAC1C4E4109DE5EB148FD0E1FDBBC32F002C1D8199D313227AD689279D8DEEF91064DF122C19C3767C463723AB663A6F8412AE17E745922C0E3A257EAEF215532588ACCBD440A03C7BC100A38BD0609A119E1EF7C5C6F1B086C68AB8873DBC6487B270340E868F9203661AFF0492CEC436ABF7C4713CE64E38CF66C794B55636BFA55E5B6554AF570CF73F1BE1DBD)
  );  
```  
  
Créez une entrée de métadonnées de clé principale de colonne pour une clé principale de colonne prenant en charge les enclaves. La clé principale de colonne prenant en charge les enclaves est stockée dans Azure Key Vault, pour les applications clientes qui utilisent le fournisseur AZURE_KEY_VAULT, afin d’accéder à la clé principale de colonne.  
  
```sql  
CREATE COLUMN MASTER KEY MyCMK  
WITH (  
    KEY_STORE_PROVIDER_NAME = N'AZURE_KEY_VAULT',  
    KEY_PATH = N'https://myvault.vault.azure.net:443/keys/MyCMK/4c05f1a41b12488f9cba2ea964b6a700');
    ENCLAVE_COMPUTATIONS (SIGNATURE = 0xA80F5B123F5E092FFBD6014FC2226D792746468C901D9404938E9F5A0972F38DADBC9FCBA94D9E740F3339754991B6CE26543DEB0D094D8A2FFE8B43F0C7061A1FFF65E30FDDF39A1B954F5BA206AAC3260B0657232020582413990261D878318CC38EF4E853970ED69A8D4A306693B8659AAC1C4E4109DE5EB148FD0E1FDBBC32F002C1D8199D313227AD689279D8DEEF91064DF122C19C3767C463723AB663A6F8412AE17E745922C0E3A257EAEF215532588ACCBD440A03C7BC100A38BD0609A119E1EF7C5C6F1B086C68AB8873DBC6487B270340E868F9203661AFF0492CEC436ABF7C4713CE64E38CF66C794B55636BFA55E5B6554AF570CF73F1BE1DBD)
  );
```  
  
## <a name="see-also"></a>Voir aussi
 
* [DROP COLUMN MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/drop-column-master-key-transact-sql.md)   
* [CREATE COLUMN ENCRYPTION KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-column-encryption-key-transact-sql.md)
* [sys.column_master_keys (Transact-SQL)](../../relational-databases/system-catalog-views/sys-column-master-keys-transact-sql.md)
* [Always Encrypted](../../relational-databases/security/encryption/always-encrypted-database-engine.md)   
* [Always Encrypted avec enclaves sécurisées](../../relational-databases/security/encryption/always-encrypted-enclaves.md)   
* [Vue d’ensemble de la gestion des clés pour Always Encrypted](../../relational-databases/security/encryption/overview-of-key-management-for-always-encrypted.md)   
* [Gérer des clés pour Always Encrypted avec enclaves sécurisées](../../relational-databases/security/encryption/always-encrypted-enclaves-manage-keys.md)   
