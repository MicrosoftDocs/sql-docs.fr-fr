---
description: DENY (Transact-SQL)
title: DENY (Transact-SQL)
ms.custom: ''
ms.date: 05/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DENY
- DENY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- schema-level securables [SQL Server]
- security [SQL Server], denying access
- DENY statement
- permissions [SQL Server], denying
- server-level securables [SQL Server]
- inherited security settings [SQL Server]
- denying permissions [SQL Server], DENY statement
- principal security [SQL Server]
- database-level securables [SQL Server]
- denying permissions [SQL Server]
ms.assetid: c32d1e01-9ee9-4665-a516-fcfece58078e
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 0c07bd840b819a6bb25624c14584089fdcfc2ac8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159353"
---
# <a name="deny-transact-sql"></a>DENY (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Refuse une autorisation à un principal. Empêche ce principal d'hériter de l'autorisation par ses appartenances à des groupes ou à des rôles. DENY est prioritaire par rapport à toutes les autres autorisations, sauf qu’elle ne s’applique pas aux propriétaires d’objets ou aux membres du rôle serveur fixe sysadmin.
  **Note de sécurité** Les membres du rôle serveur fixe sysadmin et les propriétaires d’objets ne peuvent pas se voir refuser des autorisations.
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
-- Simplified syntax for DENY  
DENY   { ALL [ PRIVILEGES ] } 
     | <permission>  [ ( column [ ,...n ] ) ] [ ,...n ]  
    [ ON [ <class> :: ] securable ] 
    TO principal [ ,...n ]   
    [ CASCADE] [ AS principal ]  
[;]

<permission> ::=  
{ see the tables below }  
  
<class> ::=  
{ see the tables below }  
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
DENY   
    <permission> [ ,...n ]  
    [ ON [ <class_> :: ] securable ]   
    TO principal [ ,...n ]  
    [ CASCADE ]  
[;]  
  
<permission> ::=  
{ see the tables below }  
  
<class> ::=  
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
 Cette option n'interdit pas toutes les autorisations possibles. Cette option revient à interdire les autorisations suivantes.  
  
-   Si l'élément sécurisable est une base de données, ALL représente BACKUP DATABASE, BACKUP LOG, CREATE DATABASE, CREATE DEFAULT, CREATE FUNCTION, CREATE PROCEDURE, CREATE RULE, CREATE TABLE et CREATE VIEW.  
  
-   Si l'élément sécurisable est une fonction scalaire, ALL représente EXECUTE et REFERENCES.  
  
-   Si l'élément sécurisable est une fonction table, ALL représente DELETE, INSERT, REFERENCES, SELECT et UPDATE.  
  
-   Si l'élément sécurisable est une procédure stockée, ALL représente EXECUTE.  
  
-   Si l'élément sécurisable est une table, ALL représente DELETE, INSERT, REFERENCES, SELECT et UPDATE.  
  
-   Si l'élément sécurisable est une vue, ALL représente DELETE, INSERT, REFERENCES, SELECT et UPDATE.  
  
> [!NOTE]  
>  La syntaxe DENY ALL est déconseillée. [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Refusez des autorisations spécifiques à la place.  
  
 PRIVILEGES  
 Inclus pour la conformité aux normes ISO. Ne change pas le comportement de l'option ALL.  
  
 *permission*  
 Nom d'une autorisation. Les mappages valides des autorisations des éléments sécurisables sont décrits dans les sous-rubriques qui suivent.  
  
 *column*  
 Spécifie le nom de la colonne d'une table à laquelle les interdictions s'appliquent. Les parenthèses () sont requises.  
  
 *class*  
 Indique la classe de l'élément sécurisable sur laquelle l'autorisation est refusée. Le qualificateur d’étendue **::** est obligatoire.  
  
 *securable*  
 Indique l'élément sécurisable sur lequel l'autorisation est refusée.  
  
 TO *principal*  
 Nom d’un principal. Les principaux auxquels il est possible de refuser des autorisations sur un élément sécurisable varient en fonction de l'élément sécurisable. Voir les rubriques ci-dessous relatives aux éléments sécurisables pour connaître les combinaisons acceptées.  
  
 CASCADE  
 Indique que l'autorisation est refusée au principal spécifié et à tous les autres principaux auxquels le principal a accordé cette autorisation. Nécessaire lorsque le principal a l'autorisation avec l'option GRANT OPTION.  
  
 AS *principal*  
 Spécifie le principal à partir duquel le principal qui exécute cette requête dérive son droit de refuser l'autorisation.
Utilisez la clause AS principal pour indiquer que le principal enregistré comme entité refusant l’autorisation doit être un principal autre que la personne qui exécute l’instruction. Supposez par exemple que l’utilisateur Mary est le principal_id 12 et que l’utilisateur Raul est le principal 15. Mary exécute `DENY SELECT ON OBJECT::X TO Steven WITH GRANT OPTION AS Raul;`. Maintenant, la table sys.database_permissions indique que le grantor_prinicpal_id de l’instruction deny était 15 (Raul), même si l’instruction a été exécutée par l’utilisateur 13 (Mary).
  
L’utilisation de AS dans cette instruction n’implique pas la possibilité d’emprunter l’identité d’un autre utilisateur.  
  
## <a name="remarks"></a>Notes  
 La syntaxe complète de l'instruction DENY est complexe. Le diagramme de syntaxe ci-dessus a été simplifié pour attirer l'attention sur sa structure. La syntaxe complète du refus des autorisations sur des éléments sécurisables particuliers est décrite dans les rubriques ci-dessous.  
  
 DENY échoue si CASCADE n'est pas spécifié lors du refus d'une autorisation à un principal auquel cette autorisation a été accordée avec GRANT OPTION.  
  
 La procédure système stockée sp_helprotect répertorie les autorisations sur un élément sécurisable au niveau base de données.  
  
> [!CAUTION]  
>  Une instruction DENY de niveau table n'a pas la priorité sur une instruction GRANT de niveau colonne. Cette incohérence dans la hiérarchie des autorisations a été conservée pour des raisons de compatibilité descendante. Elle sera supprimée dans une version ultérieure.  
  
> [!CAUTION]  
>  Le refus de l'autorisation CONTROL sur une base de données refuse implicitement l'autorisation CONNECT sur la base de données. Un principal dont l'autorisation CONTROL est refusée sur une base de données ne pourra pas se connecter à cette base de données.  
  
> [!CAUTION]  
>  Le refus de l'autorisation CONTROL SERVER entraîne implicitement le refus de l'autorisation CONNECT SQL sur le serveur. Un principal auquel l'autorisation CONTROL SERVER est refusée sur un serveur ne pourra pas se connecter à ce serveur.  
  
## <a name="permissions"></a>Autorisations  
 L'appelant (ou le principal spécifié avec l'option AS) doit avoir l'autorisation CONTROL sur l'élément sécurisable ou une autorisation plus élevée qui implique cette autorisation. Si vous utilisez l'option AS, le principal spécifié doit être propriétaire de l'élément sécurisable auquel l'autorisation est refusée.  
  
 Les détenteurs de l'autorisation CONTROL SERVER, tels que les membres du rôle de serveur fixe sysadmin, peuvent refuser une autorisation sur n'importe quel élément sécurisable du serveur. Les détenteurs de l'autorisation CONTROL sur la base de données, tels que les membres du rôle de base de données fixe db_owner, peuvent refuser une autorisation sur n'importe quel élément sécurisable de la base de données. Les détenteurs de l'autorisation CONTROL sur un schéma peuvent refuser une autorisation sur n'importe quel objet dans ce schéma. Si vous utilisez la clause AS, le principal spécifié doit être propriétaire de l'élément sécurisable auquel les autorisations sont refusées.  
  
## <a name="examples"></a>Exemples

 Le tableau suivant répertorie les éléments sécurisables et les rubriques qui décrivent leur syntaxe.  
  
|Éléments sécurisables|Syntaxe|
|----------|------|
|Rôle d'application|[DENY - Refuser des autorisations à un principal de base de données &#40;Transact-SQL&#41;](../../t-sql/statements/deny-database-principal-permissions-transact-sql.md)|  
|Assembly|[REVOKE - Révoquer des autorisations sur un assembly &#40;Transact-SQL&#41;](../../t-sql/statements/deny-assembly-permissions-transact-sql.md)|  
|Clé asymétrique|[DENY - Refuser des autorisations sur une clé asymétrique &#40;Transact-SQL&#41;](../../t-sql/statements/deny-asymmetric-key-permissions-transact-sql.md)|  
|Groupe de disponibilité|[DENY - Refuser des autorisations sur un groupe de disponibilité &#40;Transact-SQL&#41;](../../t-sql/statements/deny-availability-group-permissions-transact-sql.md)|  
|Certificat|[DENY - Refuser des autorisations sur un certificat &#40;Transact-SQL&#41;](../../t-sql/statements/deny-certificate-permissions-transact-sql.md)|  
|Contrat|[DENY - Refuser des autorisations dans Service Broker &#40;Transact-SQL&#41;](../../t-sql/statements/deny-service-broker-permissions-transact-sql.md)|  
|Base de données|[DENY - Refuser des autorisations sur une base de données &#40;Transact-SQL&#41;](../../t-sql/statements/deny-database-permissions-transact-sql.md)|  
|Informations d’identification délimitées à la base de données|[DENY - Refuser des autorisations sur des informations d’identification délimitées à la base de données (Transact-SQL)](../../t-sql/statements/deny-database-scoped-credential-transact-sql.md)|  
|Point de terminaison|[DENY - Refuser des autorisations sur un point de terminaison &#40;Transact-SQL&#41;](../../t-sql/statements/deny-endpoint-permissions-transact-sql.md)|  
|Catalogue de texte intégral|[DENY - Refuser des autorisations sur un texte intégral &#40;Transact-SQL&#41;](../../t-sql/statements/deny-full-text-permissions-transact-sql.md)|  
|Liste de mots vides de texte intégral|[DENY - Refuser des autorisations sur un texte intégral &#40;Transact-SQL&#41;](../../t-sql/statements/deny-full-text-permissions-transact-sql.md)|  
|Fonction|[DENY - Refuser des autorisations sur un objet &#40;Transact-SQL&#41;](../../t-sql/statements/deny-object-permissions-transact-sql.md)|  
|Connexion|[DENY - Refuser des autorisations à un principal du serveur &#40;Transact-SQL&#41;](../../t-sql/statements/deny-server-principal-permissions-transact-sql.md)|  
|Type de message|[DENY - Refuser des autorisations dans Service Broker &#40;Transact-SQL&#41;](../../t-sql/statements/deny-service-broker-permissions-transact-sql.md)|  
|Object|[DENY - Refuser des autorisations sur un objet &#40;Transact-SQL&#41;](../../t-sql/statements/deny-object-permissions-transact-sql.md)|  
|File d'attente|[DENY - Refuser des autorisations sur un objet &#40;Transact-SQL&#41;](../../t-sql/statements/deny-object-permissions-transact-sql.md)|  
|Liaisons de service distant|[DENY - Refuser des autorisations dans Service Broker &#40;Transact-SQL&#41;](../../t-sql/statements/deny-service-broker-permissions-transact-sql.md)|  
|Role|[DENY - Refuser des autorisations à un principal de base de données &#40;Transact-SQL&#41;](../../t-sql/statements/deny-database-principal-permissions-transact-sql.md)|  
|Routage|[DENY - Refuser des autorisations dans Service Broker &#40;Transact-SQL&#41;](../../t-sql/statements/deny-service-broker-permissions-transact-sql.md)|  
|schéma|[DENY - Refuser des autorisations sur un schéma &#40;Transact-SQL&#41;](../../t-sql/statements/deny-schema-permissions-transact-sql.md)|  
|Liste de propriétés de recherche|[DENY - Refuser des autorisations sur une liste de propriétés de recherche &#40;Transact-SQL&#41;](../../t-sql/statements/deny-search-property-list-permissions-transact-sql.md)|  
|Serveur|[DENY - Refuser des autorisations sur un serveur &#40;Transact-SQL&#41;](../../t-sql/statements/deny-server-permissions-transact-sql.md)|  
|Service|[DENY - Refuser des autorisations dans Service Broker &#40;Transact-SQL&#41;](../../t-sql/statements/deny-service-broker-permissions-transact-sql.md)|  
|Procédure stockée|[DENY - Refuser des autorisations sur un objet &#40;Transact-SQL&#41;](../../t-sql/statements/deny-object-permissions-transact-sql.md)|  
|Clé symétrique|[DENY - Refuser des autorisations sur une clé symétrique &#40;Transact-SQL&#41;](../../t-sql/statements/deny-symmetric-key-permissions-transact-sql.md)|  
|Synonyme|[DENY - Refuser des autorisations sur un objet &#40;Transact-SQL&#41;](../../t-sql/statements/deny-object-permissions-transact-sql.md)|  
|Objets système|[DENY - Refuser des autorisations sur un objet système &#40;Transact-SQL&#41;](../../t-sql/statements/deny-system-object-permissions-transact-sql.md)|  
|Table de charge de travail|[DENY - Refuser des autorisations sur un objet &#40;Transact-SQL&#41;](../../t-sql/statements/deny-object-permissions-transact-sql.md)|  
|Type|[DENY - Refuser des autorisations sur un type &#40;Transact-SQL&#41;](../../t-sql/statements/deny-type-permissions-transact-sql.md)|  
|Utilisateur|[DENY - Refuser des autorisations à un principal de base de données &#40;Transact-SQL&#41;](../../t-sql/statements/deny-database-principal-permissions-transact-sql.md)|  
|Affichage|[DENY - Refuser des autorisations sur un objet &#40;Transact-SQL&#41;](../../t-sql/statements/deny-object-permissions-transact-sql.md)|  
|Collection de schémas XML|[DENY – Refuser des autorisations sur une collection de schémas &#40;Transact-SQL&#41;](../../t-sql/statements/deny-xml-schema-collection-permissions-transact-sql.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [REVOKE &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-transact-sql.md)   
 [sp_addlogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlogin-transact-sql.md)   
 [sp_adduser &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-adduser-transact-sql.md)   
 [sp_changedbowner &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changedbowner-transact-sql.md)   
 [sp_dropuser &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropuser-transact-sql.md)   
 [sp_helprotect &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helprotect-transact-sql.md)   
 [sp_helpuser &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpuser-transact-sql.md)  
  
  
