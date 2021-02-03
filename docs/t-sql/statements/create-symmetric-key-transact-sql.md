---
description: CREATE SYMMETRIC KEY (Transact-SQL)
title: CREATE SYMMETRIC KEY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/11/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CREATE SYMMETRIC KEY
- SYMMETRIC KEP
- CREATE_SYMMETRIC_KEY_TSQL
- SYMMETRIC_KEY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CREATE SYMMETRIC KEY statement
- temporary symmetric keys [SQL Server]
- symmetric keys [SQL Server], creating
- symmetric keys [SQL Server]
ms.assetid: b5d23572-b79d-4cf1-9eef-d648fa3b1358
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: b7f007d4ae9ca1c742061ce5b11297ccace11ffb
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/01/2021
ms.locfileid: "99232982"
---
# <a name="create-symmetric-key-transact-sql"></a>CREATE SYMMETRIC KEY (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Permet de générer une clé symétrique et de spécifier ses propriétés dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Cette fonctionnalité est incompatible avec l'exportation de base de données à l'aide de l'infrastructure d'application de la couche Données. Vous devez supprimer toutes les clés symétriques avant l'exportation.  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
CREATE SYMMETRIC KEY key_name   
    [ AUTHORIZATION owner_name ]  
    [ FROM PROVIDER provider_name ]  
    WITH 
      [
          <key_options> [ , ... n ]  
        | ENCRYPTION BY <encrypting_mechanism> [ , ... n ] 
      ]
  
<key_options> ::=  
      KEY_SOURCE = 'pass_phrase'  
    | ALGORITHM = <algorithm>  
    | IDENTITY_VALUE = 'identity_phrase'  
    | PROVIDER_KEY_NAME = 'key_name_in_provider'   
    | CREATION_DISPOSITION = {CREATE_NEW | OPEN_EXISTING }  
  
<algorithm> ::=  
    DES | TRIPLE_DES | TRIPLE_DES_3KEY | RC2 | RC4 | RC4_128  
    | DESX | AES_128 | AES_192 | AES_256   
  
<encrypting_mechanism> ::=  
      CERTIFICATE certificate_name   
    | PASSWORD = 'password'   
    | SYMMETRIC KEY symmetric_key_name   
    | ASYMMETRIC KEY asym_key_name  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *KEY_NAME*  
 Spécifie le nom unique sous lequel la clé symétrique est connue dans la base de données. Des clés temporaires sont désignées lorsque _key_name_ commence par un croisillon (#). Par exemple, **#temporaryKey900007**. Vous ne pouvez pas créer une clé symétrique dont le nom commence par plusieurs #. Vous ne pouvez pas créer de clé symétrique temporaire à l'aide d'un fournisseur EKM.  
  
 AUTHORIZATION *owner_name*  
 Spécifie le nom de l'utilisateur de base de données ou du rôle d'application auquel appartiendra la clé.  
  
 FROM PROVIDER *provider_name*  
 Spécifie un fournisseur EKM (Extensible Key Management) et un nom. La clé n'est pas exportée à partir du périphérique EKM. Le fournisseur doit d'abord être défini à l'aide de l'instruction CREATE PROVIDER. Pour plus d’informations sur la création de fournisseurs de clés externes, consultez [Gestion de clés extensible &#40;EKM&#41;](../../relational-databases/security/encryption/extensible-key-management-ekm.md).  
  
> [!NOTE]  
>  Cette option n'est pas disponible dans une base de données autonome.  
  
 KEY_SOURCE **='** _pass\_phrase_ **'**  
 Spécifie une phrase secrète à partir de laquelle la clé doit être dérivée.  
  
 IDENTITY_VALUE **='** _identity\_phrase_ **'**  
 Spécifie une expression relative à l'identité à partir de laquelle un GUID doit être généré pour baliser les données qui sont chiffrées à l'aide d'une clé temporaire.  
  
 PROVIDER_KEY_NAME **='** _key\_name\_in\_provider_ **'**  
 Spécifie le nom référencé dans le fournisseur EKM (Extensible Key Management).  
  
> [!NOTE]  
>  Cette option n'est pas disponible dans une base de données autonome.  
  
 CREATION_DISPOSITION **=** CREATE_NEW  
 Crée une clé sur le périphérique EKM (Extensible Key Management).  S'il existe déjà une clé sur le périphérique, l'instruction échoue et génère une erreur.  
  
 CREATION_DISPOSITION **=** OPEN_EXISTING  
 Mappe une clé symétrique [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à une clé EKM (Extensible Key Management) existante. Si CREATION_DISPOSITION = OPEN_EXISTING n'est pas spécifié, la valeur par défaut est CREATE_NEW.  
  
 *certificate_name*  
 Spécifie le nom du certificat qui sera utilisé pour chiffrer la clé symétrique. Le certificat doit déjà exister dans la base de données.  
  
 **'** *password* **'**  
 Spécifie un mot de passe à partir duquel dériver une clé TRIPLE_DES avec laquelle sécuriser la clé symétrique. *password* doit satisfaire aux critères de la stratégie de mot de passe Windows de l’ordinateur qui exécute l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Utilisez toujours des mots de passe forts.  
  
 *symmetric_key_name*  
 Spécifie une clé symétrique utilisée pour chiffrer la clé en cours de création. La clé spécifiée doit déjà exister dans la base de données et elle doit être ouverte.  
  
 *asym_key_name*  
 Spécifie une clé asymétrique utilisée pour chiffrer la clé en cours de création. Cette clé asymétrique doit déjà exister dans la base de données.  
  
 \<algorithm>  
Spécifiez l’algorithme de chiffrement.   
> [!WARNING]  
> À partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], tous les algorithmes autres que AES_128, AES_192 et AES_256 sont déconseillés. Pour utiliser des algorithmes plus anciens (ce qui n’est pas recommandé), vous devez affecter le niveau de compatibilité 120 ou un niveau inférieur à la base de données.  
  
## <a name="remarks"></a>Notes  
 Lorsqu'une clé symétrique est créée, elle doit être chiffrée à l'aide de l'un des éléments suivants au moins : certificat, mot de passe, clé symétrique, clé asymétrique ou PROVIDER. La clé peut être soumise à plusieurs chiffrements de chaque type. En d'autres termes, une clé symétrique unique peut être chiffrée à l'aide de plusieurs certificats, mots de passe, clés symétriques et clés asymétriques à la fois.  
  
> [!CAUTION]  
>  Quand une clé symétrique est chiffrée avec un mot de passe au lieu d’un certificat (ou une autre clé), l’algorithme de chiffrement TRIPLE DES est utilisé pour chiffrer le mot de passe. Pour cette raison, les clés créées à l'aide d'un algorithme de chiffrement renforcé, tel qu'AES, sont elles-mêmes sécurisées par un algorithme plus faible.  
  
 Un mot de passe facultatif peut être utilisé pour chiffrer la clé symétrique avant de la distribuer à plusieurs utilisateurs.  
  
 Les clés temporaires appartiennent à l'utilisateur qui les crée. Les clés temporaires sont valides uniquement pour la session en cours.  
  
 IDENTITY_VALUE génère un GUID qui permet de baliser les données chiffrées à l'aide de la nouvelle clé symétrique. Ce balisage peut être utilisé pour faire correspondre les clés aux données chiffrées. Le GUID généré par une expression spécifique est toujours le même. Lorsqu'une expression est utilisée pour générer un GUID, elle ne peut être réutilisée que si au moins une session l'utilise activement. IDENTITY_VALUE est une clause facultative ; toutefois, nous vous conseillons de l'utiliser pour stocker des données chiffrées à l'aide d'une clé temporaire.  
  
 Il n'y a pas d'algorithme de chiffrement par défaut.  
  
> [!IMPORTANT]  
>  Nous vous déconseillons d'utiliser les chiffrements de flux RC4 et RC4_128 pour protéger les données sensibles. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n'ajoute pas d'encodage supplémentaire au chiffrement effectué à l'aide de ces clés.  
  
 Des informations sur les clés symétriques sont consultables dans la vue de catalogue [sys.symmetric_keys](../../relational-databases/system-catalog-views/sys-symmetric-keys-transact-sql.md).  
  
 Les clés symétriques ne peuvent pas être chiffrées par les clés symétriques créées à partir du fournisseur de chiffrement.  
  
 **Éclaircissement concernant les algorithmes DES :**  
  
-   DESX a été nommé incorrectement. Les clés symétriques créées avec ALGORITHM = DESX utilisent en fait le chiffrement TRIPLE DES avec une clé de 192 bits. L'algorithme DESX n'est pas fourni. [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  
-   Les clés symétriques créées avec ALGORITHM = TRIPLE_DES_3KEY utilisent TRIPLE DES avec une clé de 192 bits.  
-   Les clés symétriques créées avec ALGORITHM = TRIPLE_DES utilisent TRIPLE DES avec une clé de 128 bits.  
  
 **Dépréciation de l’algorithme RC4 :**  
  
 L’utilisation répétée du même KEY_GUID RC4 ou RC4_128 sur différents blocs de données génère la même clé RC4 car [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne fournit pas automatiquement de salt. L'utilisation répétée de la même clé RC4 est une erreur connue qui entraîne un chiffrement très faible. Par conséquent, les mots clés RC4 et RC4_128 sont déconseillés. [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)]  
  
> [!WARNING]  
>  L'algorithme RC4 est uniquement pris en charge pour des raisons de compatibilité descendante. Le nouveau matériel ne peut être chiffré à l'aide de RC4 ou de RC4_128 que lorsque la base de données se trouve dans le niveau de compatibilité 90 ou 100. (Non recommandé.) Utilisez à la place un algorithme plus récent, tel qu'un des algorithmes AES. Dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)], le matériel chiffré à l'aide de RC4 ou de RC4_128 peut être déchiffré dans n'importe quel niveau de compatibilité.  
  
## <a name="permissions"></a>Autorisations  
 Requiert l'autorisation ALTER ANY SYMMETRIC KEY sur la base de données. Si la clause AUTHORIZATION est spécifiée, l'autorisation IMPERSONATE sur l'utilisateur de base de données ou l'autorisation ALTER sur le rôle d'application est requise. Si le chiffrement s'effectue par certificat ou clé asymétrique, l'autorisation VIEW DEFINITION est requise sur le certificat ou la clé asymétrique. Les connexions Windows, les connexions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et les rôles d'application sont les seuls à pouvoir posséder des clés symétriques. Les groupes et les rôles ne peuvent pas posséder de clés symétriques.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-creating-a-symmetric-key"></a>R. Création d'une clé symétrique  
 Dans l'exemple ci-dessous, une clé symétrique nommée `JanainaKey09` est créée à l'aide de l'algorithme `AES 256`, puis la nouvelle clé est chiffrée au moyen du certificat `Shipping04`.  
  
```sql  
CREATE SYMMETRIC KEY JanainaKey09   
WITH ALGORITHM = AES_256  
ENCRYPTION BY CERTIFICATE Shipping04;  
GO  
```  
  
### <a name="b-creating-a-temporary-symmetric-key"></a>B. Création d'une clé symétrique temporaire  
 Dans l'exemple ci-dessous, une clé symétrique temporaire nommée `#MarketingXXV` est créée à partir de la phrase secrète : `The square of the hypotenuse is equal to the sum of the squares of the sides`. La clé obtient un GUID généré à partir de la chaîne `Pythagoras` et est chiffrée à l'aide du certificat `Marketing25`.  
  
```sql 
CREATE SYMMETRIC KEY #MarketingXXV   
WITH ALGORITHM = AES_128,  
KEY_SOURCE   
     = 'The square of the hypotenuse is equal to the sum of the squares of the sides',  
IDENTITY_VALUE = 'Pythagoras'  
ENCRYPTION BY CERTIFICATE Marketing25;  
GO  
```  
  
### <a name="c-creating-a-symmetric-key-using-an-extensible-key-management-ekm-device"></a>C. Création d'une clé symétrique à l'aide d'un périphérique EKM (Extensible Key Management)  
 L'exemple suivant crée une clé symétrique appelée `MySymKey` à l'aide d'un fournisseur appelé `MyEKMProvider` et du nom de clé `KeyForSensitiveData`. Il assigne l'autorisation à `User1` et suppose que l'administrateur système a déjà inscrit le fournisseur appelé `MyEKMProvider` dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
```sql  
CREATE SYMMETRIC KEY MySymKey  
AUTHORIZATION User1  
FROM PROVIDER EKMProvider  
WITH  
PROVIDER_KEY_NAME='KeyForSensitiveData',  
CREATION_DISPOSITION=OPEN_EXISTING;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Choisir un algorithme de chiffrement](../../relational-databases/security/encryption/choose-an-encryption-algorithm.md)   
 [ALTER SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/alter-symmetric-key-transact-sql.md)   
 [DROP SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/drop-symmetric-key-transact-sql.md)   
 [Hiérarchie de chiffrement](../../relational-databases/security/encryption/encryption-hierarchy.md)   
 [sys.symmetric_keys &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-symmetric-keys-transact-sql.md)   
 [Gestion de clés extensible &#40;EKM&#41;](../../relational-databases/security/encryption/extensible-key-management-ekm.md)   
 [Gestion de clés extensible à l’aide d’Azure Key Vault &#40;SQL Server&#41;](../../relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server.md)  
  
  
