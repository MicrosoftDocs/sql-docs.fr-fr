---
description: PERMISSIONS (Transact-SQL)
title: PERMISSIONS (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- PERMISSIONS_TSQL
- PERMISSIONS
dev_langs:
- TSQL
helpviewer_keywords:
- permissions [SQL Server], verifying
- current permission status
- users [SQL Server], permissions status
- checking permission status
- security [SQL Server], permissions
- verifying permission status
- testing permissions
- PERMISSIONS function
ms.assetid: 81625a56-b160-4424-91c5-1ce8b259a8e6
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 2fa0effd0cd559ae6f52e422ce96684d6b9b9e0f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211445"
---
# <a name="permissions-transact-sql"></a>PERMISSIONS (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Retourne une valeur contenant un fichier bitmap qui indique les autorisations d'instruction, d'objet ou de colonne de l'utilisateur actuel.  
  
 **Important** [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Utilisez plutôt [fn_my_permissions](../../relational-databases/system-functions/sys-fn-my-permissions-transact-sql.md) et [Has_Perms_By_Name](../../t-sql/functions/has-perms-by-name-transact-sql.md). L'utilisation continue de la fonction PERMISSIONS peut entraîner une baisse des performances.  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
PERMISSIONS ( [ objectid [ , 'column' ] ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *objectid*  
 Identificateur d'un élément sécurisable. Si l’argument *objectid* n’est pas spécifié, la valeur de fichier bitmap contient les autorisations d’instruction de l’utilisateur actuel ; dans le cas contraire, la bitmap contient les autorisations sur l’élément sécurisable pour l’utilisateur actuel. L'élément sécurisable spécifié doit se trouver dans la base de données active. Utilisez la fonction [OBJECT_ID](../../t-sql/functions/object-id-transact-sql.md) pour déterminer la valeur *objectid*.  
  
 **'** *column* **'**  
 Nom facultatif de la colonne dont les informations sur les autorisations sont renvoyées. La colonne doit être un nom de colonne valide dans la table spécifiée par *objectid*.  
  
## <a name="return-types"></a>Types de retour  
 **int**  
  
## <a name="remarks"></a>Notes  
 La fonction PERMISSIONS peut servir à déterminer si l'utilisateur actuel possède les autorisations nécessaires pour exécuter une instruction ou pour accorder une autorisation (GRANT) à un autre utilisateur.  
  
 Les informations sur les autorisations sont renvoyées sous forme d'un fichier bitmap 32 bits.  
  
 Les 16 bits de poids faible correspondent aux autorisations accordées à l'utilisateur, mais aussi aux autorisations qui sont appliquées aux groupes Windows ou aux rôles serveur fixes auxquels l'utilisateur appartient. Par exemple, la valeur renvoyée 66 (valeur hexadécimale 0x42), lorsqu’aucun argument *objectid* n’est spécifié, indique que l’utilisateur a le droit d’exécuter les instructions CREATE TABLE (valeur décimale 2) et BACKUP DATABASE (valeur décimale 64).  
  
 Les 16 bits de poids fort correspondent aux autorisations que l'utilisateur peut accorder (GRANT) à d'autres utilisateurs. Ils sont interprétés exactement comme les 16 bits de poids faible décrits dans les tables ci-dessous, à ceci près qu'ils sont décalés de 16 bits vers la gauche (il faut multiplier par 65 536). Par exemple, 0x8 (valeur décimale 8) est le bit qui indique l’autorisation INSERT lorsqu’un argument *objectid* est spécifié. En revanche, 0x80000 (valeur décimale 524288) indique que l'autorisation INSERT peut être accordée (GRANT), parce que 524288 = 8 x 65536.  
  
 Du fait de l'appartenance aux rôles, un utilisateur qui n'a pas l'autorisation d'exécuter une instruction peut toutefois être en mesure d'accorder cette autorisation à un autre utilisateur.  
  
 Le tableau suivant indique les bits utilisés pour les autorisations d’instruction (*objectid* n’est pas spécifié).  
  
|Bit (dec)|Bit (hex)|Autorisation d'instruction|  
|-----------------|-----------------|--------------------------|  
|1|0x1|CREATE DATABASE (base de données master uniquement)|  
|2|0x2|CREATE TABLE|  
|4|0x4|CREATE PROCEDURE|  
|8|0x8|CREATE VIEW|  
|16|0x10|CREATE RULE|  
|32|0x20|CREATE DEFAULT|  
|64|0x40|BACKUP DATABASE|  
|128|0x80|BACKUP LOG|  
|256|0x100|Réservé|  
  
 Le tableau suivant indique les bits utilisés pour les autorisations d’objet renvoyées lorsque seul l’argument *objectid* est spécifié.  
  
|Bit (dec)|Bit (hex)|Autorisation d'instruction|  
|-----------------|-----------------|--------------------------|  
|1|0x1|SELECT ALL|  
|2|0x2|UPDATE ALL|  
|4|0x4|REFERENCES ALL|  
|8|0x8|INSERT|  
|16|0x10|Suppression|  
|32|0x20|EXECUTE (procédures uniquement)|  
|4096|0x1000|SELECT ANY (une colonne minimum)|  
|8 192|0x2000|UPDATE ANY|  
|16384|0x4000|REFERENCES ANY|  
  
 Le tableau ci-dessous indique les bits utilisés pour les autorisations d’objet renvoyées (au niveau des colonnes) lorsque l’argument *objectid* et la colonne sont tous deux spécifiés.  
  
|Bit (dec)|Bit (hex)|Autorisation d'instruction|  
|-----------------|-----------------|--------------------------|  
|1|0x1|SELECT|  
|2|0x2|UPDATE|  
|4|0x4|REFERENCES|  
  
 Une valeur NULL est renvoyée si l’un des paramètres spécifiés est NULL ou non valide (par exemple, un argument *objectid* ou une colonne n’existe pas). Les valeurs des bits des autorisations qui ne s'appliquent pas (par exemple l'autorisation EXECUTE sur une table, bit 0x20) ne sont pas définies.  
  
 Pour déterminer chaque bit défini dans le fichier bitmap retourné par la fonction PERMISSIONS, utilisez l’opérateur de bits AND (&).  
  
 Vous pouvez également utiliser la procédure stockée système sp_helprotect pour retourner la liste des autorisations pour un utilisateur de la base de données active.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-using-the-permissions-function-with-statement-permissions"></a>R. Utilisation de la fonction PERMISSIONS avec des autorisations d'instruction  
 Cet exemple détermine si l'utilisateur actuel peut exécuter l'instruction `CREATE TABLE`.  
  
```sql  
IF PERMISSIONS()&2=2  
   CREATE TABLE test_table (col1 INT)  
ELSE  
   PRINT 'ERROR: The current user cannot create a table.';  
```  
  
### <a name="b-using-the-permissions-function-with-object-permissions"></a>B. Utilisation de la fonction PERMISSIONS avec des autorisations d'objet  
 Cet exemple détermine si l'utilisateur actuel peut insérer une ligne de données dans la table `Address` de la base de données `AdventureWorks2012`.  
  
```sql  
IF PERMISSIONS(OBJECT_ID('AdventureWorks2012.Person.Address','U'))&8=8   
   PRINT 'The current user can insert data into Person.Address.'  
ELSE  
   PRINT 'ERROR: The current user cannot insert data into Person.Address.';  
```  
  
### <a name="c-using-the-permissions-function-with-grantable-permissions"></a>C. Utilisation de la fonction PERMISSIONS avec des autorisations pouvant être accordées  
 Cet exemple détermine si l'utilisateur actuel peut accorder l'autorisation INSERT sur la table `Address` de la base de données `AdventureWorks2012` à un autre utilisateur.  
  
```sql  
IF PERMISSIONS(OBJECT_ID('AdventureWorks2012.Person.Address','U'))&0x80000=0x80000  
   PRINT 'INSERT on Person.Address is grantable.'  
ELSE  
   PRINT 'You may not GRANT INSERT permissions on Person.Address.';  
```  
  
## <a name="see-also"></a>Voir aussi  
 [DENY &#40;Transact-SQL&#41;](../../t-sql/statements/deny-transact-sql.md)   
 [GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-transact-sql.md)   
 [OBJECT_ID &#40;Transact-SQL&#41;](../../t-sql/functions/object-id-transact-sql.md)   
 [REVOKE &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-transact-sql.md)   
 [sp_helprotect &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helprotect-transact-sql.md)   
 [Fonctions système &#40;Transact-SQL&#41;](../../relational-databases/system-functions/system-functions-category-transact-sql.md)  
  
  
