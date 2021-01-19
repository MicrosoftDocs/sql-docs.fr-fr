---
title: Choisir un algorithme de chiffrement | Microsoft Docs
description: Utilisez ce guide pour choisir un algorithme de chiffrement afin de sécuriser une instance de SQL Server, qui prend en charge plusieurs algorithmes courants.
ms.custom: ''
ms.date: 08/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- cryptography [SQL Server], algorithms
- encryption [SQL Server], algorithms
- security [SQL Server], encryption
- algorithms [SQL Server encryption]
ms.assetid: 8227028c-a9c9-489d-bd27-fbf8242634ae
author: jaszymas
ms.author: jaszymas
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 8fb31365f95cddc23739db02df9ba4ad9db0345e
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171811"
---
# <a name="choose-an-encryption-algorithm"></a>Choisir un algorithme de chiffrement
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  Le chiffrement est l'une des mesures préventives à la disposition des administrateurs qui souhaitent sécuriser une instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
 Les algorithmes de chiffrement définissent les transformations de données qui ne peuvent pas être facilement inversées par les utilisateurs non autorisés. [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] permet aux administrateurs et aux développeurs de choisir entre plusieurs algorithmes, notamment DES, Triple DES, TRIPLE_DES_3KEY, RC2, RC4, RC4 128 bits, DESX, AES 128 bits, AES 192 bits et AES 256 bits.  
  
> [!NOTE]  
>  À partir de [!INCLUDE[ssSQL15](../../../includes/sssql16-md.md)], tous les algorithmes autres que AES_128, AES_192 et AES_256 sont déconseillés. Pour utiliser des algorithmes plus anciens (ce qui n’est pas recommandé), vous devez affecter le niveau de compatibilité 120 ou un niveau inférieur à la base de données.  
  
 Aucun algorithme unique ne convient pour toutes les situations et l'appréciation des avantages de chaque algorithme dépasse le cadre de la documentation en ligne de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Cependant, les principes suivants s'appliquent :  
  
-   Un chiffrement renforcé consomme en général davantage de ressources processeur qu'un chiffrement plus faible.  
  
-   Les clés longues produisent en général un chiffrement plus fort que les clés courtes.  
  
-   Le chiffrement asymétrique est plus lent que le chiffrement symétrique.  
  
-   Les mots de passe longs et complexes sont plus forts que les mots de passe courts.  

-   Le chiffrement symétrique est généralement recommandé quand la clé est uniquement stockée localement, tandis que le chiffrement asymétrique est recommandé quand les clés doivent être partagées sur le réseau.
  
-   Si vous chiffrez de grandes quantités de données, vous devez utiliser pour cela une clé symétrique, puis chiffrer cette clé avec une clé asymétrique.  
  
-   Les données chiffrées ne peuvent pas être compressées, mais les données compressées peuvent être chiffrées. Si vous utilisez la compression, vous devez compresser les données avant de les chiffrer.  
  
> [!IMPORTANT]  
>  L'algorithme RC4 est uniquement pris en charge pour des raisons de compatibilité descendante. Le nouveau matériel ne peut être chiffré à l'aide de RC4 ou de RC4_128 que lorsque la base de données se trouve dans le niveau de compatibilité 90 ou 100. (Non recommandé.) Utilisez à la place un algorithme plus récent, tel qu'un des algorithmes AES. Dans [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)] et les versions ultérieures, le matériel chiffré à l'aide de RC4 ou de RC4_128 peut être déchiffré dans n'importe quel niveau de compatibilité.  
>   
>  L'utilisation répétée du même RC4 ou RC4_128 KEY_GUID sur différents blocs de données entraîne la même clé RC4 car [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ne fournit pas automatiquement de salt. L'utilisation répétée de la même clé RC4 est une erreur connue qui entraîne un chiffrement très faible. Par conséquent, les mots clés RC4 et RC4_128 sont déconseillés. [!INCLUDE[ssNoteDepFutureDontUse](../../../includes/ssnotedepfuturedontuse-md.md)]  
  
 Pour plus d'informations sur les algorithmes et la technologie de chiffrement, consultez la rubrique [Concepts fondamentaux sur la sécurité](/previous-versions/aa720225(v=vs.71)) dans le Guide du développeur .NET Framework sur le site MSDN.  
  
 **Éclaircissement concernant les algorithmes DES :**  
  
-   DESX a été nommé incorrectement. Les clés symétriques créées avec ALGORITHM = DESX utilisent en fait le chiffrement TRIPLE DES avec une clé de 192 bits. L'algorithme DESX n'est pas fourni. [!INCLUDE[ssNoteDepFutureAvoid](../../../includes/ssnotedepfutureavoid-md.md)]  
  
-   Les clés symétriques créées avec ALGORITHM = TRIPLE_DES_3KEY utilisent TRIPLE DES avec une clé de 192 bits.  
  
-   Les clés symétriques créées avec ALGORITHM = TRIPLE_DES utilisent TRIPLE DES avec une clé de 128 bits.  
  
## <a name="related-tasks"></a>Tâches associées  
  
| Tâche | Type |
| ---- | ---- |
|Chiffrement à l'aide d'une clé symétrique.|[CREATE SYMMETRIC KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/create-symmetric-key-transact-sql.md)|  
|Chiffrement à l'aide d'une clé asymétrique.|[CREATE ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/create-asymmetric-key-transact-sql.md)|  
|Chiffrement à l'aide d'un certificat.|[CREATE CERTIFICATE &#40;Transact-SQL&#41;](../../../t-sql/statements/create-certificate-transact-sql.md)|  
|Chiffrement de fichiers de base de données à l'aide du chiffrement transparent des données.|[Chiffrement transparent des données &#40;TDE&#41;](../../../relational-databases/security/encryption/transparent-data-encryption.md)|  
|Comment chiffrer une colonne d'une table.|[Chiffrer une colonne de données](../../../relational-databases/security/encryption/encrypt-a-column-of-data.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Chiffrement SQL Server](../../../relational-databases/security/encryption/sql-server-encryption.md)   
 [Hiérarchie de chiffrement](../../../relational-databases/security/encryption/encryption-hierarchy.md)  
  
