---
title: Chiffrement Always Encrypted | Microsoft Docs
description: Découvrez les algorithmes et mécanismes de chiffrement dérivant le matériel de chiffrement dans la fonctionnalité Always Encrypted dans SQL Server et Azure SQL Database.
ms.custom: ''
ms.date: 10/30/2019
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Always Encrypted, cryptography system
ms.assetid: ae8226ff-0853-4716-be7b-673ce77dd370
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 04a4515467265456797fbff8f5dc0446748229ba
ms.sourcegitcommit: 6f4fb9cfd0cad06127a6328adc745e2ba7c191d1
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/05/2021
ms.locfileid: "99570466"
---
# <a name="always-encrypted-cryptography"></a>Chiffrement Always Encrypted
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]

  Ce document décrit les algorithmes et mécanismes de chiffrement permettant de dériver le matériel de chiffrement utilisé dans la fonctionnalité [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md) dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] et [!INCLUDE[ssSDSFull](../../../includes/sssdsfull-md.md)].  
  
## <a name="keys-key-stores-and-key-encryption-algorithms"></a>Clés, magasins de clés et algorithmes de chiffrement de clé
 Always Encrypted utilise deux types de clés : des clés principales de colonne et des clés de chiffrement de colonne.  
  
 Une clé principale de colonne (CMK) est une clé de chiffrement de clé (par exemple, une clé utilisée pour chiffrer d’autres clés) qui est toujours contrôlée par un client et stockée dans un magasin de clés externes. Un pilote client avec Always Encrypted interagit avec le magasin de clés via un fournisseur de magasins CMK, qui peut faire partie de la bibliothèque de pilotes (un fournisseur [!INCLUDE[msCoName](../../../includes/msconame-md.md)]/système) ou de l’application cliente (un fournisseur personnalisé). Les bibliothèques de pilotes clients incluent actuellement des fournisseurs de magasins de clés [!INCLUDE[msCoName](../../../includes/msconame-md.md)] pour le [magasin de certificats Windows](/windows/desktop/SecCrypto/using-certificate-stores) et les modules de sécurité matériels (HSM). Pour obtenir la liste actuelle des fournisseurs, consultez [CREATE COLUMN MASTER KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/create-column-master-key-transact-sql.md). Un développeur d'applications peut proposer un fournisseur personnalisé pour un magasin arbitraire.  
  
 Une clé de chiffrement de colonne (CEK) est une clé de chiffrement de contenu (par exemple, une clé utilisée pour protéger des données) protégée par une clé CMK.  
  
 Tous les fournisseurs du magasin CMK [!INCLUDE[msCoName](../../../includes/msconame-md.md)] chiffrent les clés CEK à l’aide de la méthode RSA-OAEP (RSA avec Optimal Asymmetric Encryption Padding). Le fournisseur du magasin de clés qui prend en charge l’API de chiffrement Microsoft : La prochaine génération (CNG) dans .NET Framework ([classe SqlColumnEncryptionCngProvider](/dotnet/api/system.data.sqlclient.sqlcolumnencryptioncngprovider)) utilise les paramètres par défaut spécifiés par RFC 8017 dans la Section A.2.1. Ces paramètres par défaut utilisent une fonction de hachage de SHA-1 et une fonction de génération de masque de MGF1 avec SHA-1. Tous les autres fournisseurs du magasin de clés utilisent SHA-256. 

Always Encrypted utilise en interne les modules de chiffrement validés FIPS 140-2. 

## <a name="data-encryption-algorithm"></a>Algorithme de chiffrement des données  
 Always Encrypted utilise l’algorithme **AEAD_AES_256_CBC_HMAC_SHA_256** pour chiffrer les données de la base de données.  
  
 **AEAD_AES_256_CBC_HMAC_SHA_256** est dérivé du projet de spécification disponible ici : [https://tools.ietf.org/html/draft-mcgrew-aead-aes-cbc-hmac-sha2-05](https://tools.ietf.org/html/draft-mcgrew-aead-aes-cbc-hmac-sha2-05). Il utilise un schéma de chiffrement authentifié avec données associées, en suivant une approche Encrypt-then-MAC. Autrement dit, le texte en clair est d'abord chiffré puis les informations MAC sont générées selon le texte chiffré qui en résulte.  
  
 Pour masquer les modèles, **AEAD_AES_256_CBC_HMAC_SHA_256** utilise le mode CBC (Cipher Block Chaining), dans lequel une valeur initiale est chargée dans le système nommé vecteur d’initialisation (IV). Vous trouverez la description complète du mode CBC ici : [https://csrc.nist.gov/publications/nistpubs/800-38a/sp800-38a.pdf](https://csrc.nist.gov/publications/nistpubs/800-38a/sp800-38a.pdf).  
  
 **AEAD_AES_256_CBC_HMAC_SHA_256** calcule une valeur de texte chiffré pour une valeur en texte clair donnée en procédant comme suit.  
  
### <a name="step-1-generating-the-initialization-vector-iv"></a>Étape 1 : Génération du vecteur d’initialisation (IV)  
 Always Encrypted prend en charge deux variantes de **AEAD_AES_256_CBC_HMAC_SHA_256**:  
  
-   Aléatoire  
  
-   Déterministe  
  
 Pour le chiffrement aléatoire, le vecteur d'initialisation est généré de façon aléatoire. Par conséquent, chaque fois que le même texte en clair est chiffré, un texte chiffré différent est généré, ce qui empêche toute divulgation d'informations.  
  
```  
When using randomized encryption: IV = Generate cryptographicaly random 128bits  
```  
  
 Dans le cas du chiffrement déterministe, le vecteur d’initialisation n’est pas généré de façon aléatoire, mais plutôt dérivé de la valeur en texte clair à l’aide de l’algorithme suivant :  
  
```  
When using deterministic encryption: IV = HMAC-SHA-256( iv_key, cell_data ) truncated to 128 bits.  
```  
  
 Où la valeur iv_key est dérivée de la clé CEK de la façon suivante :  
  
```  
iv_key = HMAC-SHA-256(CEK, "Microsoft SQL Server cell IV key" + algorithm + CEK_length)  
```  
  
 La troncation de la valeur HMAC est effectuée afin d’ajuster un bloc de données si nécessaire pour le vecteur d’initialisation.
Par conséquent, le chiffrement déterministe génère toujours le même texte chiffré pour une valeur en texte clair donnée, ce qui permet de déduire si deux valeurs de texte en clair sont égales en comparant leurs valeurs de texte chiffré correspondantes. Cette divulgation limitée des informations permet au système de base de données de prendre en charge une comparaison d'égalité au niveau des valeurs de colonne chiffrées.  
  
 Le chiffrement déterministe est plus efficace pour dissimuler les modèles par rapport aux autres méthodes comme l'utilisation d'une valeur IV prédéfinie.  
  
### <a name="step-2-computing-aes_256_cbc-ciphertext"></a>Étape 2 : Calcul du texte chiffré AES_256_CBC  
 Après le calcul du vecteur d’initialisation, le texte chiffré **AES_256_CBC** est généré :  
  
```  
aes_256_cbc_ciphertext = AES-CBC-256(enc_key, IV, cell_data) with PKCS7 padding.  
```  
  
 Où la clé de chiffrement (enc_key) est dérivée de la clé CEK comme suit.  
  
```  
enc_key = HMAC-SHA-256(CEK, "Microsoft SQL Server cell encryption key" + algorithm + CEK_length )  
```  
  
### <a name="step-3-computing-mac"></a>Étape 3 : Calcul de la valeur MAC  
 Par la suite, la valeur MAC est calculée à l'aide de l'algorithme suivant :  
  
```  
MAC = HMAC-SHA-256(mac_key, versionbyte + IV + Ciphertext + versionbyte_length)  
```  
  
 Où :  
  
```  
versionbyte = 0x01 and versionbyte_length = 1
mac_key = HMAC-SHA-256(CEK, "Microsoft SQL Server cell MAC key" + algorithm + CEK_length)  
```  
  
### <a name="step-4-concatenation"></a>Étape 4 : Concaténation  
 Enfin, la valeur chiffrée est générée en concaténant simplement l'octet de version de l’algorithme, la valeur MAC, le vecteur d'initialisation et le texte chiffré AES_256_CBC :  
  
```  
aead_aes_256_cbc_hmac_sha_256 = versionbyte + MAC + IV + aes_256_cbc_ciphertext  
```  
  
## <a name="ciphertext-length"></a>Longueur du texte chiffré  
 Les longueurs (en octets) des composants particuliers du texte chiffré **AEAD_AES_256_CBC_HMAC_SHA_256** sont les suivantes :  
  
-   versionbyte : 1  
  
-   MAC : 32  
  
-   Vecteur d’initialisation : 16  
  
-   aes_256_cbc_ciphertext: `(FLOOR (DATALENGTH(cell_data)/ block_size) + 1)* block_size`, où :  
  
    -   block_size est de 16 octets  
  
    -   cell_data est une valeur de texte en clair  
  
     Par conséquent, la taille minimale de aes_256_cbc_ciphertext est de 1 bloc, soit 16 octets.  
  
 Ainsi, la longueur du texte chiffré, qui résulte du chiffrement de valeurs en texte clair données (cell_data) peut être calculée à l'aide de la formule suivante :  
  
```  
1 + 32 + 16 + (FLOOR(DATALENGTH(cell_data)/16) + 1) * 16  
```  
  
 Par exemple :  
  
-   Une valeur en texte clair **int** d’une longueur de 4 octets devient une valeur binaire d’une longueur de 65 octets après chiffrement.  
  
-   Une valeur en texte clair **nchar(1000)** d’une longueur de 2 000 octets devient une valeur binaire d’une longueur de 2 065 octets après chiffrement.  
  
 Le tableau suivant contient une liste complète des types de données et la longueur du texte chiffré pour chaque type.  
  
|Type de données|Longueur du texte chiffré [octets]|  
|---------------|---------------------------------|  
|**bigint**|65|  
|**binary**|Varie. Utilisez la formule ci-dessus.|  
|**bit**|65|  
|**char**|Varie. Utilisez la formule ci-dessus.|  
|**date**|65|  
|**datetime**|65|  
|**datetime2**|65|  
|**datetimeoffset**|65|  
|**decimal**|81|  
|**float**|65|  
|**Geography**|N/A (non pris en charge)|  
|**geometry**|N/A (non pris en charge)|  
|**hierarchyid**|N/A (non pris en charge)|  
|**image**|N/A (non pris en charge)|  
|**int**|65|  
|**money**|65|  
|**nchar**|Varie. Utilisez la formule ci-dessus.|  
|**ntext**|N/A (non pris en charge)|  
|**numeric**|81|  
|**nvarchar**|Varie. Utilisez la formule ci-dessus.|  
|**real**|65|  
|**smalldatetime**|65|  
|**smallint**|65|  
|**smallmoney**|65|  
|**sql_variant**|N/A (non pris en charge)|  
|**sysname**|N/A (non pris en charge)|  
|**text**|N/A (non pris en charge)|  
|**time**|65|  
|**timestamp**<br /><br /> (**rowversion**)|N/A (non pris en charge)|  
|**tinyint**|65|  
|**uniqueidentifier**|81|  
|**varbinary**|Varie. Utilisez la formule ci-dessus.|  
|**varchar**|Varie. Utilisez la formule ci-dessus.|  
|**xml**|N/A (non pris en charge)|  
  
## <a name="net-reference"></a>Référence .NET  
 Pour plus d’informations sur les algorithmes présentés dans ce document, consultez les fichiers **SqlAeadAes256CbcHmac256Algorithm.cs**, **SqlColumnEncryptionCertificateStoreProvider.cs** et **SqlColumnEncryptionCertificateStoreProvider.cs** dans la [référence .NET](https://referencesource.microsoft.com/).  
  
## <a name="see-also"></a>Voir aussi  
 - [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md)   
 - [Développer des applications avec Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-client-development.md)  
  
