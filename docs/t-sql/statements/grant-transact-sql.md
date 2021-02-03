---
description: GRANT (Transact-SQL)
title: GRANT (Transact-SQL)
ms.custom: ''
ms.date: 06/12/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- GRANT_TSQL
- GRANT
dev_langs:
- TSQL
helpviewer_keywords:
- granting permissions [SQL Server], GRANT statement
- schema-level securables [SQL Server]
- GRANT statement
- cross-database permissions
- GRANT statement, about GRANT statement
- server-level securables [SQL Server]
- database-level securables [SQL Server]
- permissions [SQL Server], granting
ms.assetid: a760c16a-4d2d-43f2-be81-ae9315f38185
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: cb24af4f9740f5471a05e17898969a9f39624822
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99180191"
---
# <a name="grant-transact-sql"></a>GRANT (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Accorde des autorisations à un principal sur un élément sécurisable.  Le concept général consiste à ACCORDER \<some permission> SUR \<some object> À \<some user, login, or group>. Pour obtenir une présentation générale des autorisations, consultez [Autorisations &#40;moteur de base de données&#41;](../../relational-databases/security/permissions-database-engine.md).  
  
 ![Icône Lien d’article](../../database-engine/configure-windows/media/topic-link.gif "Icône Lien d’article") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
-- Simplified syntax for GRANT  
GRANT { ALL [ PRIVILEGES ] }  
      | permission [ ( column [ ,...n ] ) ] [ ,...n ]  
      [ ON [ class :: ] securable ] TO principal [ ,...n ]   
      [ WITH GRANT OPTION ] [ AS principal ]  
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
GRANT
    <permission> [ ,...n ]  
    [ ON [ <class_type> :: ] securable ]
    TO principal [ ,...n ]  
    [ WITH GRANT OPTION ]  
[;]  
  
<permission> ::=  
{ see the tables below }  
  
<class_type> ::=  
{  
      LOGIN  
    | DATABASE  
    | OBJECT  
    | ROLE  
    | SCHEMA  
    | USER  
}  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments

ALL  
Cette option est déconseillée mais maintenue uniquement pour la compatibilité descendante. Elle n'accorde pas toutes les autorisations possibles. Elle revient à accorder les autorisations suivantes :

  - Si l'élément sécurisable est une base de données, ALL représente BACKUP DATABASE, BACKUP LOG, CREATE DATABASE, CREATE DEFAULT, CREATE FUNCTION, CREATE PROCEDURE, CREATE RULE, CREATE TABLE et CREATE VIEW.  
  
  - Si l'élément sécurisable est une fonction scalaire, ALL représente EXECUTE et REFERENCES.  
  
  - Si l'élément sécurisable est une fonction table, ALL représente DELETE, INSERT, REFERENCES, SELECT et UPDATE.  
  
  - Si l'élément sécurisable est une procédure stockée, ALL représente EXECUTE.  
  
  - Si l'élément sécurisable est une table, ALL représente DELETE, INSERT, REFERENCES, SELECT et UPDATE.  
  
  - Si l'élément sécurisable est une vue, ALL représente DELETE, INSERT, REFERENCES, SELECT et UPDATE.  

PRIVILEGES  
Inclus pour la conformité aux normes ISO. Ne change pas le comportement de l'option ALL.  
  
*permission*  
Nom d'une autorisation. Les mappages valides des autorisations aux éléments sécurisables sont décrits dans les sous-rubriques qui suivent.  
  
*column*  
Spécifie le nom de la colonne d'une table à laquelle les autorisations s'appliquent. Les parenthèses () sont requises.  
  
*class*  
Indique la classe de l'élément sécurisable sur laquelle l'autorisation est accordée. Le qualificateur d’étendue **::** est obligatoire.  
  
*securable*  
Indique l'élément sécurisable sur lequel l'autorisation est accordée.  
  
TO *principal*  
Nom d’un principal. Les principaux auxquels il est possible d'accorder des autorisations sur un élément sécurisable varient en fonction de l'élément sécurisable. Consultez les sous-rubriques ci-dessous pour connaître les combinaisons acceptées.  
  
GRANT OPTION  
Indique que le détenteur de l'autorisation a également la possibilité d'accorder l'autorisation spécifiée à d'autres principaux.  
  
AS *principal*  
Utilisez la clause AS principal pour indiquer que le principal enregistré comme entité accordant l’autorisation doit être un principal autre que la personne qui exécute l’instruction. Supposez par exemple que l’utilisateur Mary est le principal_id 12 et que l’utilisateur Raul est le principal 15. Mary exécute `GRANT SELECT ON OBJECT::X TO Steven WITH GRANT OPTION AS Raul;`. Maintenant, la table sys.database_permissions indique que le grantor_prinicpal_id était 15 (Raul), même si l’instruction a été exécutée par l’utilisateur 13 (Mary).  

L’utilisation de la clause AS n’est généralement pas recommandé, sauf si vous devez définir explicitement la chaîne d’autorisation. Pour plus d’informations, consultez la section **Synthèse sur l’algorithme de vérification des autorisations** de la rubrique [Autorisations (moteur de base de données)](../../relational-databases/security/permissions-database-engine.md).

L’utilisation de AS dans cette instruction n’implique pas la possibilité d’emprunter l’identité d’un autre utilisateur.

## <a name="remarks"></a>Notes

La syntaxe complète de l'instruction GRANT est complexe. Le diagramme de syntaxe ci-dessus a été simplifié pour attirer l'attention sur sa structure. La syntaxe complète d’accord des autorisations sur des éléments sécurisables particuliers est décrite dans les articles ci-dessous.  

L'instruction REVOKE peut s'utiliser pour supprimer des autorisations accordées et l'instruction DENY pour empêcher un principal d'obtenir une autorisation particulière au moyen d'une instruction GRANT.  

L'accord d'une autorisation supprime l'option DENY ou REVOKE de cette autorisation sur l'élément sécurisable spécifié. Si la même autorisation est refusée dans une étendue plus élevée qui contient l'élément sécurisable, l'option DENY est prioritaire. Cependant, la révocation de l'autorisation accordée dans une étendue plus élevée n'est pas prioritaire.  

Les autorisations au niveau base de données sont accordées sur l'étendue de la base de données spécifiée. Si un utilisateur a besoin d'autorisations sur des objets dans une autre base de données, créez le compte de l'utilisateur dans cette autre base de données ou autorisez le compte de l'utilisateur à accéder à la fois à cette autre base de données et à la base de données active.  

> [!CAUTION]  
> Une instruction DENY de niveau table n'a pas la priorité sur une instruction GRANT de niveau colonne. Cette incohérence dans la hiérarchie des autorisations a été conservée pour des raisons de compatibilité descendante. Elle sera supprimée dans une version ultérieure.  

La procédure système stockée sp_helprotect répertorie les autorisations sur un élément sécurisable au niveau base de données.  

## <a name="with-grant-option"></a>WITH GRANT OPTION

**GRANT** ... **WITH GRANT OPTION** spécifie que le principal de sécurité qui reçoit l’autorisation peut accorder l’autorisation spécifiée à d’autres comptes de sécurité. Quand le principal qui reçoit l’autorisation est un rôle ou un groupe Windows, la clause **AS** doit être utilisée si l’autorisation d’objet doit être accordée à des utilisateurs qui ne sont pas membres du groupe ou du rôle. Étant donné que seul un utilisateur, et non un groupe ou un rôle, peut exécuter une instruction **GRANT**, un membre spécifique du groupe ou du rôle doit utiliser la clause **AS** pour appeler explicitement l’appartenance au rôle ou au groupe quand il accorde l’autorisation. L’exemple suivant montre comment la clause **WITH GRANT OPTION** est utilisée quand elle est accordée à un rôle ou à un groupe Windows.  
  
```sql
-- Execute the following as a database owner  
GRANT EXECUTE ON TestProc TO TesterRole WITH GRANT OPTION;  
EXEC sp_addrolemember TesterRole, User1;  
-- Execute the following as User1  
-- The following fails because User1 does not have the permission as the User1  
GRANT EXECUTE ON TestMe TO User2;  
-- The following succeeds because User1 invokes the TesterRole membership  
GRANT EXECUTE ON TestMe TO User2 AS TesterRole;  
```

## <a name="chart-of-sql-server-permissions"></a>Graphique des autorisations SQL Server

Pour obtenir un graphique de la taille d’une affiche de toutes les autorisations du [!INCLUDE[ssDE](../../includes/ssde-md.md)] au format PDF, consultez [https://aka.ms/sql-permissions-poster](https://aka.ms/sql-permissions-poster).  

## <a name="permissions"></a>Autorisations

Le fournisseur d'autorisations (ou le principal spécifié avec l'option AS) doit posséder l'autorisation elle-même avec l'option GRANT OPTION ou une autorisation plus élevée qui implique l'autorisation accordée. En cas d'utilisation de l'option AS, d'autres critères s'appliquent. Pour plus d’informations, consultez l’article sur les éléments sécurisables.

Les propriétaires d'objets peuvent accorder des autorisations sur les objets qu'ils possèdent. Les principaux ayant l'autorisation CONTROL sur un élément sécurisable peuvent accorder une autorisation sur cet élément.

Les détenteurs de l'autorisation CONTROL SERVER, tels que les membres du rôle serveur fixe sysadmin, peuvent accorder une autorisation sur n'importe quel élément sécurisable du serveur. Les bénéficiaires de l'autorisation CONTROL sur une base de données, tels que les membres du rôle de base de données fixe db_owner, peuvent accorder une autorisation quelconque sur tout élément sécurisable inclus dans la base de données. Les détenteurs de l'autorisation CONTROL sur un schéma peuvent accorder une autorisation sur n'importe quel objet dans ce schéma.

## <a name="examples"></a>Exemples

Le tableau suivant répertorie les éléments sécurisables et les articles qui décrivent leur syntaxe.  

| Élément sécurisable | Syntaxe GRANT|
| ---------| ------ |
|Rôle d'application|[GRANT - Octroyer des autorisations à un principal de base de données &#40;Transact-SQL&#41;](../../t-sql/statements/grant-database-principal-permissions-transact-sql.md)|  
|Assembly|[GRANT - Accorder des autorisations sur un assembly &#40;Transact-SQL&#41;](../../t-sql/statements/grant-assembly-permissions-transact-sql.md)|  
|Clé asymétrique|[GRANT - Octroyer des autorisations sur une clé asymétrique &#40;Transact-SQL&#41;](../../t-sql/statements/grant-asymmetric-key-permissions-transact-sql.md)|  
|Groupe de disponibilité|[GRANT - Octroyer des autorisations sur un groupe de disponibilité &#40;Transact-SQL&#41;](../../t-sql/statements/grant-availability-group-permissions-transact-sql.md)|  
|Certificat|[GRANT - Octroyer des autorisations sur un certificat &#40;Transact-SQL&#41;](../../t-sql/statements/grant-certificate-permissions-transact-sql.md)|  
|Contrat|[GRANT - Octroyer des autorisations dans Service Broker &#40;Transact-SQL&#41;](../../t-sql/statements/grant-service-broker-permissions-transact-sql.md)|  
|Base de données|[GRANT - Octroyer des autorisations sur une base de données &#40;Transact-SQL&#41;](../../t-sql/statements/grant-database-permissions-transact-sql.md)|
|Informations d’identification délimitées à la base de données|[GRANT - Octroyer des autorisations sur les informations d’identification délimitées à la base de données (Transact-SQL)](../../t-sql/statements/grant-database-scoped-credential-transact-sql.md)|  
|Point de terminaison|[GRANT - Octroyer des autorisations sur un point de terminaison &#40;Transact-SQL&#41;](../../t-sql/statements/grant-endpoint-permissions-transact-sql.md)|  
|Catalogue de texte intégral|[GRANT - Octroyer des autorisations sur un catalogue de texte intégral &#40;Transact-SQL&#41;](../../t-sql/statements/grant-full-text-permissions-transact-sql.md)|  
|Liste de mots vides de texte intégral|[GRANT - Octroyer des autorisations sur un catalogue de texte intégral &#40;Transact-SQL&#41;](../../t-sql/statements/grant-full-text-permissions-transact-sql.md)|  
|Fonction|[GRANT – Octroyer des autorisations sur un objet &#40;Transact-SQL&#41;](../../t-sql/statements/grant-object-permissions-transact-sql.md)|  
|Connexion|[GRANT – Octroyer des autorisations à un principal de serveur &#40;Transact-SQL&#41;](../../t-sql/statements/grant-server-principal-permissions-transact-sql.md)|  
|Type de message|[GRANT - Octroyer des autorisations dans Service Broker &#40;Transact-SQL&#41;](../../t-sql/statements/grant-service-broker-permissions-transact-sql.md)|  
|Object|[GRANT – Octroyer des autorisations sur un objet &#40;Transact-SQL&#41;](../../t-sql/statements/grant-object-permissions-transact-sql.md)|  
|File d'attente|[GRANT – Octroyer des autorisations sur un objet &#40;Transact-SQL&#41;](../../t-sql/statements/grant-object-permissions-transact-sql.md)|  
|Liaisons de service distant|[GRANT - Octroyer des autorisations dans Service Broker &#40;Transact-SQL&#41;](../../t-sql/statements/grant-service-broker-permissions-transact-sql.md)|  
|Role|[GRANT - Octroyer des autorisations à un principal de base de données &#40;Transact-SQL&#41;](../../t-sql/statements/grant-database-principal-permissions-transact-sql.md)|  
|Routage|[GRANT - Octroyer des autorisations dans Service Broker &#40;Transact-SQL&#41;](../../t-sql/statements/grant-service-broker-permissions-transact-sql.md)|  
|schéma|[GRANT - Octroyer des autorisations sur un schéma &#40;Transact-SQL&#41;](../../t-sql/statements/grant-schema-permissions-transact-sql.md)|  
|Liste de propriétés de recherche|[GRANT - Accorder des autorisations sur une liste de propriétés de recherche &#40;Transact-SQL&#41;](../../t-sql/statements/grant-search-property-list-permissions-transact-sql.md)|  
|Serveur|[Autorisations de serveur GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-server-permissions-transact-sql.md)|  
|Service|[GRANT - Octroyer des autorisations dans Service Broker &#40;Transact-SQL&#41;](../../t-sql/statements/grant-service-broker-permissions-transact-sql.md)|  
|Procédure stockée|[GRANT – Octroyer des autorisations sur un objet &#40;Transact-SQL&#41;](../../t-sql/statements/grant-object-permissions-transact-sql.md)|  
|Clé symétrique|[GRANT - Octroyer des autorisations sur une clé symétrique &#40;Transact-SQL&#41;](../../t-sql/statements/grant-symmetric-key-permissions-transact-sql.md)|  
|Synonyme|[GRANT – Octroyer des autorisations sur un objet &#40;Transact-SQL&#41;](../../t-sql/statements/grant-object-permissions-transact-sql.md)|  
|Objets système|[GRANT – octroi d’autorisations d’objet système &#40;Transact-SQL&#41;](../../t-sql/statements/grant-system-object-permissions-transact-sql.md)|  
|Table de charge de travail|[GRANT – Octroyer des autorisations sur un objet &#40;Transact-SQL&#41;](../../t-sql/statements/grant-object-permissions-transact-sql.md)|  
|Type|[GRANT – Octroyer des autorisations sur un type &#40;Transact-SQL&#41;](../../t-sql/statements/grant-type-permissions-transact-sql.md)|  
|Utilisateur|[GRANT - Octroyer des autorisations à un principal de base de données &#40;Transact-SQL&#41;](../../t-sql/statements/grant-database-principal-permissions-transact-sql.md)|  
|Affichage|[GRANT – Octroyer des autorisations sur un objet &#40;Transact-SQL&#41;](../../t-sql/statements/grant-object-permissions-transact-sql.md)|  
|Collection de schémas XML|[GRANT – Octroyer des autorisations sur une collection de schémas XML &#40;Transact-SQL&#41;](../../t-sql/statements/grant-xml-schema-collection-permissions-transact-sql.md)|  

## <a name="see-also"></a>Voir aussi

- [DENY &#40;Transact-SQL&#41;](../../t-sql/statements/deny-transact-sql.md)
- [REVOKE &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-transact-sql.md)
- [sp_addlogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlogin-transact-sql.md)
- [sp_adduser &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-adduser-transact-sql.md)
- [sp_changedbowner &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changedbowner-transact-sql.md)
- [sp_dropuser &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropuser-transact-sql.md)
- [sp_helprotect &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helprotect-transact-sql.md)
- [sp_helpuser &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpuser-transact-sql.md)
