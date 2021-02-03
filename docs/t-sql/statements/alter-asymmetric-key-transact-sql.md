---
description: ALTER ASYMMETRIC KEY (Transact-SQL)
title: ALTER ASYMMETRIC KEY (Transact-SQL) | Microsoft Docs
ms.date: 04/12/2017
ms.prod: sql
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER_ASYMMETRIC_KEY_TSQL
- ALTER ASYMMETRIC KEY
dev_langs:
- TSQL
helpviewer_keywords:
- ENCRYPTION BY PASSWORD option
- passwords [SQL Server], asymmetric keys
- encryption [SQL Server], asymmetric keys
- DECRYPTION BY PASSWORD option
- ALTER ASYMMETRIC KEY statement
- asymmetric keys [SQL Server], modifying
ms.assetid: 958e95d6-fbe6-43e8-abbd-ccedbac2dbac
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 5d9604d366417688c8c04697e77852c3a27e189a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182998"
---
# <a name="alter-asymmetric-key-transact-sql"></a>ALTER ASYMMETRIC KEY (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Permet de modifier les propriétés d'une clé asymétrique.  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
ALTER ASYMMETRIC KEY Asym_Key_Name <alter_option>  
  
<alter_option> ::=  
      <password_change_option>   
    | REMOVE PRIVATE KEY   

<password_change_option> ::=  
    WITH PRIVATE KEY ( <password_option> [ , <password_option> ] )  

<password_option> ::=  
      ENCRYPTION BY PASSWORD = 'strongPassword'  
    | DECRYPTION BY PASSWORD = 'oldPassword'  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *Asym_Key_Name*  
 Nom sous lequel la clé asymétrique est connue dans la base de données.  
  
 REMOVE PRIVATE KEY  
 Permet de supprimer la clé privée de la clé asymétrique, sans supprimer la clé publique.  
  
 WITH PRIVATE KEY  
 Modifie la protection de la clé privée.  
  
 ENCRYPTION BY PASSWORD **='** _strongPassword_*_'_*  
 Spécifie un nouveau mot de passe pour protéger la clé privée. *password* doit satisfaire aux critères de la stratégie de mot de passe Windows de l’ordinateur qui exécute l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si cette option est omise, la clé privée sera chiffrée à l'aide de la clé principale de base de données.  
  
 DECRYPTION BY PASSWORD **='** _oldPassword_*_'_*  
 Spécifie l'ancien mot de passe au moyen duquel la clé privée est actuellement protégée. Cette option n'est pas requise si la clé privée est chiffrée à l'aide de la clé principale de base de données.  
  
## <a name="remarks"></a>Remarques  
 Si aucune clé principale de base de données n'existe, l'option ENCRYPTION BY PASSWORD est requise et l'opération échouera si aucun mot de passe n'est fourni. Pour plus d’informations sur la création d’une clé principale de base de données, consultez [CREATE MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-master-key-transact-sql.md).  
  
 Vous pouvez utiliser ALTER ASYMMETRIC KEY pour modifier la protection de la clé privée en spécifiant des options PRIVATE KEY, comme l'illustre le tableau ci-dessous.  
  
|Changer la protection|ENCRYPTION BY PASSWORD|DECRYPTION BY PASSWORD|  
|----------------------------|----------------------------|----------------------------|  
|De l'ancien mot de passe au nouveau mot de passe|Obligatoire|Obligatoire|  
|Du mot de passe à la clé principale|Omettre|Obligatoire|  
|De la clé principale au mot de passe|Obligatoire|Omettre|  
  
 La clé principale de base de données doit être ouverte pour pouvoir être utilisée pour protéger une clé privée. Pour plus d’informations, consultez [OPEN MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/open-master-key-transact-sql.md).  
  
 Pour modifier la propriété d’une clé asymétrique, utilisez [ALTER AUTHORIZATION](../../t-sql/statements/alter-authorization-transact-sql.md).  
  
## <a name="permissions"></a>Autorisations  
 Requiert l'autorisation CONTROL sur la clé asymétrique si la clé privée doit être supprimée.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-changing-the-password-of-the-private-key"></a>R. Modification du mot de passe de la clé privée  
 Dans l'exemple ci-dessous, le mot de passe utilisé pour protéger la clé privée de la clé asymétrique `PacificSales09` est modifié. Le nouveau mot de passe est `<enterStrongPasswordHere>`.  
  
```sql  
ALTER ASYMMETRIC KEY PacificSales09   
    WITH PRIVATE KEY (  
    DECRYPTION BY PASSWORD = '<oldPassword>',  
    ENCRYPTION BY PASSWORD = '<enterStrongPasswordHere>');  
GO  
```  
  
### <a name="b-removing-the-private-key-from-an-asymmetric-key"></a>B. Suppression de la clé privée d'une clé asymétrique  
 Dans l'exemple ci-dessous, la clé privée est supprimée de `PacificSales19` et seule la clé publique demeure.  
  
```sql  
ALTER ASYMMETRIC KEY PacificSales19 REMOVE PRIVATE KEY;  
GO  
```  
  
### <a name="c-removing-password-protection-from-a-private-key"></a>C. Suppression de la protection par mot de passe d'une clé privée  
 Dans l'exemple ci-dessous, la protection par mot de passe d'une clé privée est supprimée et la clé est protégée au moyen de la clé principale de base de données.  
  
```sql  
OPEN MASTER KEY DECRYPTION BY PASSWORD = '<database master key password>';  
ALTER ASYMMETRIC KEY PacificSales09 WITH PRIVATE KEY (  
    DECRYPTION BY PASSWORD = '<enterStrongPasswordHere>' );  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [CREATE ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-asymmetric-key-transact-sql.md)   
 [DROP ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/drop-asymmetric-key-transact-sql.md)   
 [SQL Server et clés de chiffrement de base de données &#40;moteur de base de données&#41;](../../relational-databases/security/encryption/sql-server-and-database-encryption-keys-database-engine.md)   
 [Hiérarchie de chiffrement](../../relational-databases/security/encryption/encryption-hierarchy.md)   
 [CREATE MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-master-key-transact-sql.md)   
 [OPEN MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/open-master-key-transact-sql.md)   
 [Gestion de clés extensible &#40;EKM&#41;](../../relational-databases/security/encryption/extensible-key-management-ekm.md)  
  
  
