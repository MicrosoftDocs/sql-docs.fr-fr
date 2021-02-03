---
description: REVOKE (Transact-SQL)
title: REVOKE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/26/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- REVOKE_TSQL
- REVOKE
dev_langs:
- TSQL
helpviewer_keywords:
- schema-level securables [SQL Server]
- REVOKE statement, Transact-SQL syntax
- removing permissions
- server-level securables [SQL Server]
- deleting permissions
- revoking permissions [SQL Server]
- REVOKE statement
- denying permissions [SQL Server], removing
- database-level securables [SQL Server]
- granting permissions [SQL Server], removing
- permissions [SQL Server], revoking
- dropping permissions
ms.assetid: 9d31d3e7-0883-45cd-bf0e-f0361bbb0956
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: db5c82465b97d1425fb68ba42fffb799355d32c8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206544"
---
# <a name="revoke-transact-sql"></a>REVOKE (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Supprime une autorisation accordée ou refusée antérieurement.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
-- Simplified syntax for REVOKE  
REVOKE [ GRANT OPTION FOR ]  
      {   
        [ ALL [ PRIVILEGES ] ]  
        |  
                permission [ ( column [ ,...n ] ) ] [ ,...n ]  
      }  
      [ ON [ class :: ] securable ]   
      { TO | FROM } principal [ ,...n ]   
      [ CASCADE] [ AS principal ]  
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
REVOKE   
    <permission> [ ,...n ]  
    [ ON [ <class_type> :: ] securable ]   
    [ FROM | TO ] principal [ ,...n ]  
    [ CASCADE ]  
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
 GRANT OPTION FOR  
 Indique que la possibilité d'accorder l'autorisation spécifiée sera révoquée. C'est obligatoire si vous utilisez l'argument CASCADE.  
  
> [!IMPORTANT]  
>  Si le principal possède l'autorisation spécifiée sans l'option GRANT, l'autorisation elle-même sera révoquée.  
  
 ALL  
**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et ultérieur
  
 Cette option ne révoque pas toutes les autorisations possibles. La révocation complète, ALL, équivaut à révoquer les autorisations suivantes.  
  
-   Si l'élément sécurisable est une base de données, ALL représente BACKUP DATABASE, BACKUP LOG, CREATE DATABASE, CREATE DEFAULT, CREATE FUNCTION, CREATE PROCEDURE, CREATE RULE, CREATE TABLE et CREATE VIEW.  
  
-   Si l'élément sécurisable est une fonction scalaire, ALL représente EXECUTE et REFERENCES.  
  
-   Si l'élément sécurisable est une fonction table, ALL représente DELETE, INSERT, REFERENCES, SELECT et UPDATE.  
  
-   Si l'élément sécurisable est une procédure stockée, ALL représente EXECUTE.  
  
-   Si l'élément sécurisable est une table, ALL représente DELETE, INSERT, REFERENCES, SELECT et UPDATE.  
  
-   Si l'élément sécurisable est une vue, ALL représente DELETE, INSERT, REFERENCES, SELECT et UPDATE.  
  
> [!NOTE]  
>  La syntaxe REVOKE ALL est déconseillée. [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Révoquez des autorisations spécifiques à la place.  
  
 PRIVILEGES  
 Inclus pour la conformité aux normes ISO. Ne change pas le comportement de l'option ALL.  
  
 *permission*  
 Nom d'une autorisation. Les mappages valides d’autorisations à des éléments sécurisables sont décrits dans les rubriques répertoriées dans la section [Syntaxe spécifique aux éléments sécurisables](#securable), plus loin dans cette rubrique.  
  
 *column*  
 Spécifie le nom d'une colonne d'une table sur laquelle les autorisations sont révoquées. Les parenthèses sont obligatoires.  
  
 *class*  
 Spécifie la classe de l'élément sécurisable sur lequel l'autorisation est révoquée. Le qualificateur d’étendue **::** est obligatoire.  
  
 *securable*  
 Spécifie l'élément sécurisable sur lequel l'autorisation est révoquée.  
  
 TO | FROM *principal*  
 Nom d’un principal. Les principaux d'où les autorisations portant sur un élément sécurisable peuvent être révoquées varient, selon cet élément sécurisable. Pour plus d’informations sur les combinaisons valides, consultez les rubriques répertoriées dans la section [Syntaxe spécifique aux éléments sécurisables](#securable), plus loin dans cette rubrique.  
  
 CASCADE  
 Indique que l'autorisation qui est révoquée l'est aussi à partir d'autres principaux auxquels elle a été accordée par ce principal. Lorsque vous utilisez l'argument CASCADE, vous devez également inclure l'argument GRANT OPTION FOR.  
  
> [!CAUTION]  
>  Une révocation en cascade d'une autorisation accordée avec l'option WITH GRANT OPTION entraîne la révocation des deux options GRANT et DENY de cette autorisation.  
  
 AS *principal*  
 Utilisez la clause AS principal pour indiquer que vous révoquez une autorisation accordée par une entité de sécurité autre que vous. Supposez par exemple que l’utilisateur Mary est le principal_id 12 et que l’utilisateur Raul est le principal_id 15. Mary et Raul accordent tous les deux à un utilisateur nommé Steven la même autorisation. La table sys.database_permissions indique deux fois les autorisations, mais elles auront chacune une valeur grantor_principal_id différente. Mary peut révoquer l’autorisation à l’aide de la clause `AS RAUL` pour supprimer l’octroi d’autorisation de Raul.
 
L’utilisation de AS dans cette instruction n’implique pas la possibilité d’emprunter l’identité d’un autre utilisateur.  
  
## <a name="remarks"></a>Notes  
 La syntaxe complète de l'instruction REVOKE est complexe. Le diagramme de syntaxe ci-dessus a été simplifié pour attirer l'attention sur sa structure. La syntaxe complète de la révocation d’autorisations sur des éléments sécurisables spécifiques est décrite dans les rubriques répertoriées dans la section [Syntaxe spécifique aux éléments sécurisables](#securable), plus loin dans cette rubrique.  
  
 L'instruction REVOKE peut s'utiliser pour supprimer des autorisations accordées et l'instruction DENY pour empêcher un principal d'obtenir une autorisation particulière au moyen d'une instruction GRANT.  
  
 L'accord d'une autorisation supprime l'option DENY ou REVOKE de cette autorisation sur l'élément sécurisable spécifié. Si la même autorisation est refusée dans une étendue plus élevée qui contient l'élément sécurisable, l'option DENY est prioritaire. Toutefois, la révocation de l'autorisation accordée à une étendue supérieure ne devient pas prioritaire.  
  
> [!CAUTION]  
>  Une instruction DENY de niveau table n'a pas la priorité sur une instruction GRANT de niveau colonne. Cette incohérence dans la hiérarchie des autorisations a été conservée pour des raisons de compatibilité descendante. Elle sera supprimée dans une version ultérieure.  
  
 La procédure stockée système sp_helprotect répertorie les autorisations sur un élément sécurisable au niveau de la base de données.  
  
 L'instruction REVOKE échoue si l'argument CASCADE n'est pas spécifié lorsque vous révoquez une autorisation d'un principal auquel a été accordée cette autorisation avec GRANT OPTION spécifié.  
  
## <a name="permissions"></a>Autorisations  
 Les principaux avec l'autorisation CONTROL sur un élément sécurisable peuvent révoquer l'autorisation sur cet élément sécurisable. Les propriétaires d'objets peuvent révoquer des autorisations sur les objets qu'ils possèdent.  
  
 Les bénéficiaires de l'autorisation CONTROL SERVER, par exemple les membres du rôle serveur fixe sysadmin, peuvent révoquer n'importe quelle autorisation sur n'importe quel élément sécurisable du serveur. Les bénéficiaires de l'autorisation CONTROL sur une base de données, par exemple les membres du rôle de base de données fixe db_owner, peuvent révoquer n'importe quelle autorisation sur n'importe quel élément sécurisable de la base de données. Les bénéficiaires de l'autorisation CONTROL sur un schéma peuvent révoquer n'importe quelle autorisation sur n'importe quel objet du schéma.  
  
##  <a name="securable-specific-syntax"></a><a name="securable"></a> Syntaxe spécifique aux éléments sécurisables  
 Le tableau suivant répertorie les éléments sécurisables et les rubriques qui décrivent leur syntaxe.  
  
|Élément sécurisable|Rubrique|  
|---------------|-----------|  
|Rôle d'application|[REVOKE - Révoquer des autorisations sur un principal de base de données &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-database-principal-permissions-transact-sql.md)|  
|Assembly|[REVOKE - Révoquer des autorisations sur un assembly &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-assembly-permissions-transact-sql.md)|  
|Clé asymétrique|[REVOKE - Révoquer des autorisations sur une clé asymétrique &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-asymmetric-key-permissions-transact-sql.md)|  
|Groupe de disponibilité|[REVOKE - Révoquer des autorisations sur un groupe de disponibilité &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-availability-group-permissions-transact-sql.md)|  
|Certificat|[REVOKE - Révoquer des autorisations sur un certificat &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-certificate-permissions-transact-sql.md)|  
|Contrat|[Autorisations REVOKE dans Service Broker &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-service-broker-permissions-transact-sql.md)|  
|Base de données|[REVOKE - Révoquer des autorisations sur une base de données &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-database-permissions-transact-sql.md)|  
|Point de terminaison|[REVOKE - Révoquer des autorisations sur un point de terminaison &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-endpoint-permissions-transact-sql.md)|  
|Informations d’identification délimitées à la base de données|[REVOKE - Révoquer des autorisations sur les informations d’identification délimitées à la base de données (Transact-SQL)](../../t-sql/statements/revoke-database-scoped-credential-transact-sql.md)|  
|Catalogue de texte intégral|[REVOKE - Révoquer des autorisations sur un texte intégral &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-full-text-permissions-transact-sql.md)|  
|Liste de mots vides de texte intégral|[REVOKE - Révoquer des autorisations sur un texte intégral &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-full-text-permissions-transact-sql.md)|  
|Fonction|[REVOKE - Révoquer des autorisations sur un objet &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-object-permissions-transact-sql.md)|  
|Connexion|[Autorisations du principal du serveur REVOKE &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-server-principal-permissions-transact-sql.md)|  
|Type de message|[Autorisations REVOKE dans Service Broker &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-service-broker-permissions-transact-sql.md)|  
|Object|[REVOKE - Révoquer des autorisations sur un objet &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-object-permissions-transact-sql.md)|  
|File d'attente|[REVOKE - Révoquer des autorisations sur un objet &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-object-permissions-transact-sql.md)|  
|Liaisons de service distant|[Autorisations REVOKE dans Service Broker &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-service-broker-permissions-transact-sql.md)|  
|Role|[REVOKE - Révoquer des autorisations sur un principal de base de données &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-database-principal-permissions-transact-sql.md)|  
|Routage|[Autorisations REVOKE dans Service Broker &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-service-broker-permissions-transact-sql.md)|  
|schéma|[REVOKE - Révoquer des autorisations sur un schéma &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-schema-permissions-transact-sql.md)|  
|Liste de propriétés de recherche|[REVOKE - Révoquer des autorisations sur une liste de propriétés de recherche &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-search-property-list-permissions-transact-sql.md)|  
|Serveur|[REVOKE - Révoquer des autorisations sur un serveur &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-server-permissions-transact-sql.md)|  
|Service|[Autorisations REVOKE dans Service Broker &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-service-broker-permissions-transact-sql.md)|  
|Procédure stockée|[REVOKE - Révoquer des autorisations sur un objet &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-object-permissions-transact-sql.md)|  
|Clé symétrique|[REVOKE - Révoquer des autorisations sur une clé symétrique &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-symmetric-key-permissions-transact-sql.md)|  
|Synonyme|[REVOKE - Révoquer des autorisations sur un objet &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-object-permissions-transact-sql.md)|  
|Objets système|[REVOKE - Révoquer des autorisations sur un objet système &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-system-object-permissions-transact-sql.md)|  
|Table de charge de travail|[REVOKE - Révoquer des autorisations sur un objet &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-object-permissions-transact-sql.md)|  
|Type|[REVOKE – Révoquer des autorisations sur un type &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-type-permissions-transact-sql.md)|  
|Utilisateur|[REVOKE - Révoquer des autorisations sur un principal de base de données &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-database-principal-permissions-transact-sql.md)|  
|Vue|[REVOKE - Révoquer des autorisations sur un objet &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-object-permissions-transact-sql.md)|  
|Collection de schémas XML|[REVOKE – Révoquer des autorisations sur une collection de schémas XML &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-xml-schema-collection-permissions-transact-sql.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Hiérarchie des autorisations &#40;Moteur de base de données&#41;](../../relational-databases/security/permissions-hierarchy-database-engine.md)   
 [DENY &#40;Transact-SQL&#41;](../../t-sql/statements/deny-transact-sql.md)   
 [GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-transact-sql.md)   
 [sp_addlogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlogin-transact-sql.md)   
 [sp_adduser &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-adduser-transact-sql.md)   
 [sp_changedbowner &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changedbowner-transact-sql.md)   
 [sp_dropuser &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropuser-transact-sql.md)   
 [sp_helprotect &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helprotect-transact-sql.md)   
 [sp_helpuser &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpuser-transact-sql.md)  
  
  
