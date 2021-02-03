---
description: 'Autorisations : GRANT, DENY, REVOKE (Azure Synapse Analytics, Parallel Data Warehouse)'
title: Autorisations GRANT-DENY-REVOKE
titleSuffix: Azure Synapse Analytics
ms.custom: seo-lt-2019
ms.date: 08/10/2017
ms.prod: sql
ms.prod_service: sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 5a3b7424-408e-4cb0-8957-667ebf4596fc
author: VanMSFT
ms.author: vanto
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: 4756d84874e207b0047127061faff55de296d01e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204625"
---
# <a name="permissions-grant-deny-revoke-azure-synapse-analytics-parallel-data-warehouse"></a>Autorisations : GRANT, DENY, REVOKE (Azure Synapse Analytics, Parallel Data Warehouse)
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  Utilisez les instructions [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] ou [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]**GRANT** et **DENY** pour accorder ou refuser une autorisation (comme **UPDATE**) sur un élément sécurisable (par exemple, une base de données, une table, une vue, etc.) à un principal de sécurité (une connexion, un utilisateur de base de données ou un rôle de base de données). Utilisez **REVOKE** pour supprimer l’accord ou le refus d’une autorisation.  
  
 Les autorisations de niveau serveur sont appliquées aux connexions. Les autorisations de niveau base de données sont appliquées aux utilisateurs de base de données et aux rôles de base de données.  
  
 Pour voir les autorisations qui ont été accordées et refusées, interrogez les vues sys.server_permissions et sys.database_permissions. Les autorisations qui ne sont pas explicitement accordées ou refusées à un principal de sécurité peuvent être héritées par le biais d'une appartenance à un rôle ayant ces autorisations. Les autorisations des rôles de base de données fixes ne peuvent pas être changées et n’apparaissent pas dans les vues sys.server_permissions et sys.database_permissions.  
  
-   **GRANT** accorde explicitement une ou plusieurs autorisations.  
  
-   **DENY** refuse explicitement une ou plusieurs autorisations au principal.  
  
-   **REVOKE** supprime les autorisations **GRANT** ou **DENY** existantes.  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique")[Conventions de la syntaxe Transact-SQL &#40;Transact-SQL&#41;](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
-- Azure Synapse Analytics and Parallel Data Warehouse  
GRANT   
    <permission> [ ,...n ]  
    [ ON [ <class_type> :: ] securable ]   
    TO principal [ ,...n ]  
    [ WITH GRANT OPTION ]  
[;]  
  
DENY   
    <permission> [ ,...n ]  
    [ ON [ <class_type> :: ] securable ]   
    TO principal [ ,...n ]  
    [ CASCADE ]  
[;]  
  
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
  
## <a name="arguments"></a>Arguments  
 \<permission>[ **,** ...*n* ]  
 Une ou plusieurs autorisations à accorder, refuser ou révoquer.  
  
 ON [ \<class_type> :: ] *securable* La clause **ON** décrit le paramètre sécurisable pour lequel accorder, refuser ou révoquer des autorisations.  
  
 \<class_type> Type de classe de l’élément sécurisable. Il peut s’agir de **LOGIN**, **DATABASE**, **OBJECT**, **SCHEMA**, **ROLE**, or **USER**. Les autorisations peuvent également être accordées à **SERVER**_class\_type_, mais **SERVER** n’est pas spécifié pour ces autorisations. **DATABASE** n’est pas spécifié quand l’autorisation comprend le mot **DATABASE** (par exemple, **ALTER ANY DATABASE**). Quand aucun *class_type* n’est spécifié et que le type d’autorisation n’est pas limité au serveur ou à la classe de base de données, la classe est assimilée à **OBJECT**.  
  
 *securable*  
 Nom de l’objet (connexion, base de données, table, vue, schéma, procédure, rôle ou utilisateur) pour lequel accorder, refuser ou révoquer des autorisations. Le nom de l’objet peut être spécifié avec les règles de nommage en trois parties qui sont décrites dans [Conventions de la syntaxe Transact-SQL &#40;Transact-SQL&#41;](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).  
  
 TO *principal* [ **,** ...*n* ]  
 Un ou plusieurs principaux pour lesquels les autorisations sont accordées, refusées ou révoquées. Le principal est le nom d’une connexion, d’un utilisateur de base de données ou d’un rôle de base de données.  
  
 FROM *principal* [ **,** ...*n* ]  
 Un ou plusieurs principaux pour lesquels révoquer des autorisations.  Le principal est le nom d’une connexion, d’un utilisateur de base de données ou d’un rôle de base de données. **FROM** peut uniquement être utilisé avec une instruction **REVOKE**. **TO** peut être utilisé avec **GRANT**, **DENY** ou **REVOKE**.  
  
 WITH GRANT OPTION  
 Indique que le détenteur de l'autorisation a également la possibilité d'accorder l'autorisation spécifiée à d'autres principaux.  
  
 CASCADE  
 Indique que l’autorisation est refusée ou révoquée au principal spécifié et à tous les autres principaux auxquels le principal a accordé cette autorisation. Obligatoire quand le principal a l’autorisation avec **GRANT OPTION**.  
  
 GRANT OPTION FOR  
 Indique que la possibilité d'accorder l'autorisation spécifiée sera révoquée. Ce paramètre est obligatoire quand vous utilisez l’argument **CASCADE**.  
  
> [!IMPORTANT]  
>  Si le principal a l’autorisation spécifiée sans l’option **GRANT**, l’autorisation elle-même est révoquée.  
  
## <a name="permissions"></a>Autorisations  
 Pour accorder une autorisation, le fournisseur d’autorisations doit avoir l’autorisation elle-même avec **WITH GRANT OPTION** ou une autorisation plus élevée qui implique l’autorisation accordée.  Les propriétaires d'objets peuvent accorder des autorisations sur les objets qu'ils possèdent. Les principaux avec l’autorisation **CONTROL** sur un élément sécurisable peuvent accorder une autorisation sur cet élément.  Les membres des rôles de base de données fixes **db_owner** et **db_securityadmin** peuvent accorder n’importe quelle autorisation dans la base de données.  
  
## <a name="general-remarks"></a>Remarques d'ordre général  
 Le refus ou la révocation d’autorisations à un principal n’affecte pas les demandes qui ont obtenu l’autorisation et qui sont en cours d’exécution. Pour limiter immédiatement l’accès, vous devez annuler les demandes actives ou tuer les sessions en cours.  
  
> [!NOTE]  
>  La plupart des rôles serveur fixes ne sont pas disponibles dans cette version. Utilisez à la place des rôles de base de données définis par l’utilisateur. Les connexions ne peuvent pas être ajoutées au rôle serveur fixe **sysadmin**. L’accord de l’autorisation **CONTROL SERVER** s’apparente à une appartenance au rôle serveur fixe **sysadmin**.  
  
 Certaines instructions nécessitent plusieurs autorisations. Par exemple, pour créer une table, vous avez besoin des autorisations **CREATE TABLE** dans la base de données et de l’autorisation **ALTER SCHEMA** pour la table qui doit contenir la table.  
  
 PDW exécute parfois des procédures stockées pour distribuer des actions utilisateur aux nœuds de calcul. Par conséquent, l’autorisation execute pour l’ensemble d’une base de données ne peut pas être refusée. (Par exemple, `DENY EXECUTE ON DATABASE::<name> TO <user>;` échoue.) Pour contourner ce problème, refusez l’autorisation execute aux schémas d’utilisateur ou à des objets spécifiques (procédures).  
  
### <a name="implicit-and-explicit-permissions"></a>Autorisations implicites et explicites  
 Une *autorisation explicite* est une autorisation **GRANT** ou **DENY** donnée à un principal par une instruction **GRANT** ou **DENY**.  
  
 Une *autorisation implicite* est une autorisation **GRANT** ou **DENY** qu’un principal (connexion, utilisateur ou rôle de base de données) a hérité d’un autre rôle de base de données.  
  
 Une autorisation implicite peut également être héritée d’une autorisation parente ou de couverture. Par exemple, l’autorisation **UPDATE** sur une table peut être héritée si vous avez l’autorisation **UPDATE** sur le schéma qui contient la table ou l’autorisation **CONTROL** sur la table.  
  
### <a name="ownership-chaining"></a>Chaînage des propriétés  
 Quand plusieurs objets de base de données accèdent les uns aux autres de façon séquentielle, la séquence est appelée *chaîne*. Bien que de telles chaînes n'existent pas indépendamment les unes des autres, lorsque [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] parcourt les liens d'une chaîne, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] évalue les autorisations sur les objets constitutifs différemment de ce qu'il ferait s'il accédait aux objets séparément. Le chaînage des propriétés a des implication importantes sur la gestion de la sécurité. Pour plus d’informations sur les chaînes de propriétés, consultez [Chaînes de propriétés](https://msdn.microsoft.com/library/ms188676\(v=sql11\).aspx) et [Tutoriel : Chaînes de propriétés et changement de contexte](../../relational-databases/tutorial-ownership-chains-and-context-switching.md).  
  
## <a name="permission-list"></a>Liste d’autorisations  
  
### <a name="server-level-permissions"></a>Autorisations de niveau serveur  
 Les autorisations de niveau serveur peuvent être accordées, refusées et révoquées pour les connexions.  
  
 **Autorisations qui s’appliquent aux serveurs**  
  
-   CONTROL SERVER  
  
-   ADMINISTER BULK OPERATIONS  
  
-   ALTER ANY CONNECTION  
  
-   ALTER ANY DATABASE  
  
-   CREATE ANY DATABASE  
  
-   ALTER ANY EXTERNAL DATA SOURCE  
  
-   ALTER ANY EXTERNAL FILE FORMAT  
  
-   ALTER ANY LOGIN  
  
-   ALTER SERVER STATE  
  
-   CONNECT SQL  
  
-   VIEW ANY DEFINITION  
  
-   VIEW ANY DATABASE  
  
-   VIEW SERVER STATE  
  
 **Autorisations qui s’appliquent aux connexions**  
  
-   CONTROL ON LOGIN  
  
-   ALTER ON LOGIN  
  
-   IMPERSONATE ON LOGIN  
  
-   VIEW DEFINITION  
  
### <a name="database-level-permissions"></a>Autorisations de niveau base de données  
 Les autorisations de niveau base de données peuvent être accordées, refusées et révoqués pour les utilisateurs de base de données et les rôles de base de données définis par l’utilisateur.  
  
 **Autorisations qui s’appliquent à toutes les classes de base de données**  
  
-   CONTROL  
  
-   ALTER  
  
-   VIEW DEFINITION  
  
 **Autorisations qui s’appliquent à toutes les classes de base de données sauf les utilisateurs**  
  
-   TAKE OWNERSHIP  
  
 **Autorisations qui s’appliquent uniquement aux bases de données**  
  
-   ALTER ANY DATABASE  
  
-   ALTER ON DATABASE  
  
-   ALTER ANY DATASPACE  
  
-   ALTER ANY ROLE  
  
-   ALTER ANY SCHEMA  
  
-   ALTER ANY USER  
  
-   BACKUP DATABASE  
  
-   CONNECT ON DATABASE  
  
-   CREATE PROCEDURE  
  
-   CREATE ROLE  
  
-   CREATE SCHEMA  
  
-   CREATE TABLE  
  
-   CREATE VIEW  
  
-   SHOWPLAN  
  
 **Autorisations qui s’appliquent uniquement aux utilisateurs**  
  
-   IMPERSONATE  
  
 **Autorisations qui s’appliquent aux bases de données, aux schémas et aux objets**  
  
-   ALTER  
  
-   Suppression  
  
-   Exécutez  
  
-   INSERT  
  
-   SELECT  
  
-   UPDATE  
  
-   REFERENCES  
  
 Pour voir la définition de chaque type d’autorisation, consultez [Autorisations (moteur de base de données)](../../relational-databases/security/permissions-database-engine.md).  
  
### <a name="chart-of-permissions"></a>Graphique des autorisations  
 Toutes les autorisations sont représentées graphiquement dans ce schéma. C’est le moyen le plus simple de représenter la hiérarchie imbriquée des autorisations. Par exemple, l’autorisation **ALTER ON LOGIN** peut être accordée par elle-même, mais elle est également incluse si une connexion reçoit l’autorisation **CONTROL** sur cette connexion ou l’autorisation **ALTER ANY LOGIN**.  
  
 ![Schéma des autorisations de sécurité APS](../../t-sql/statements/media/aps-security-perms-poster.png "Schéma des autorisations de sécurité APS")  
  
 Pour télécharger une version en taille réelle de ce schéma, consultez [Autorisations de SQL Server PDW](https://go.microsoft.com/fwlink/?LinkId=244249) dans la section de fichiers du site APS Yammer (ou envoyez une demande par e-mail à **apsdoc\@microsoft.com**.  
  
## <a name="default-permissions"></a>Autorisations par défaut  
 La liste suivante décrit les autorisations par défaut :  
  
-   Quand une connexion est créée à l’aide de l’instruction **CREATE LOGIN**, la nouvelle connexion reçoit l’autorisation **CONNECT SQL**.  
  
-   Toutes les connexions sont membres du rôle serveur **public** et ne peuvent pas être supprimées de **public**.  
  
-   Quand un utilisateur de base de données est créé à l’aide de l’autorisation **CREATE USER**, il reçoit l’autorisation **CONNECT** dans la base de données.  
  
-   Tous les principaux, y compris le rôle **public**, n’ont pas d’autorisation explicite ou implicite par défaut.  
  
-   Quand une connexion ou un utilisateur devient propriétaire d’un objet ou d’une base de données, la connexion ou l’utilisateur a toujours toutes les autorisations sur la base de données ou l’objet. Les autorisations de propriété ne peuvent pas être changées et ne sont pas visibles comme des autorisations explicites. Les instructions **GRANT**, **DENY** et **REVOKE** sont sans effet sur les propriétaires.  
  
-   La connexion **AS** a toutes les autorisations sur l’appliance. De même que les autorisations de propriété, les autorisations **AS** ne peuvent pas être changées et ne sont pas visibles comme des autorisations explicites. Les instructions **GRANT**, **DENY** et **REVOKE** sont sans effet sur la connexion **AS**. La connexion **AS** ne peut pas être renommée.  
  
-   L’instruction **USE** n’a pas besoin d’autorisation. Tous les principaux peuvent exécuter l’instruction **USE** sur une base de données.  
  
##  <a name="examples-sssdw-and-sspdw"></a><a name="Examples"></a> Exemples : [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="a-granting-a-server-level-permission-to-a-login"></a>R. Accord d’une autorisation de niveau serveur à une connexion  
 Les deux instructions suivantes accordent une autorisation de niveau serveur à une connexion.  
  
```sql  
GRANT CONTROL SERVER TO [Ted];  
```  
  
```sql  
GRANT ALTER ANY DATABASE TO Mary;  
```  
  
### <a name="b-granting-a-server-level-permission-to-a-login"></a>B. Accord d’une autorisation de niveau serveur à une connexion  
 L’exemple suivant accorde une autorisation de niveau serveur sur une connexion à un principal de serveur (une autre connexion).  
  
```sql  
GRANT  VIEW DEFINITION ON LOGIN::Ted TO Mary;  
```  
  
### <a name="c-granting-a-database-level-permission-to-a-user"></a>C. Accord d’une autorisation de niveau base de données à un utilisateur  
 L’exemple suivant accorde une autorisation de niveau base de données sur un utilisateur à un principal de base de données (un autre utilisateur).  
  
```sql  
GRANT VIEW DEFINITION ON USER::[Ted] TO Mary;  
```  
  
### <a name="d-granting-denying-and-revoking-a-schema-permission"></a>D. Accord, refus et révocation d’une autorisation de schéma  
 L’instruction **GRANT** suivante accorde à Yuen la possibilité de sélectionner des données dans une table ou une vue dans le schéma dbo.  
  
```sql  
GRANT SELECT ON SCHEMA::dbo TO [Yuen];  
```  
  
 L’instruction **DENY** suivante empêche Yuen de sélectionner des données dans une table ou une vue dans le schéma dbo. Yuen ne peut pas lire les données, même s’il obtient l’autorisation d’une autre manière, par exemple, par le biais d’une appartenance au rôle.  
  
```sql  
DENY SELECT ON SCHEMA::dbo TO [Yuen];  
```  
  
 L’instruction **REVOKE** suivante supprime l’autorisation **DENY**. Les autorisations explicites de Yuen sont neutres désormais. Yuen peut sélectionner des données dans n’importe quelle table par le biais d’une autre autorisation implicite comme une appartenance à un rôle.  
  
```sql  
REVOKE SELECT ON SCHEMA::dbo TO [Yuen];  
```  
  
### <a name="e-demonstrating-the-optional-object-clause"></a>E. Démonstration de la clause facultative OBJECT::  
 Comme OBJECT est la classe par défaut d’une instruction d’autorisation, les deux instructions suivantes sont identiques. La clause **OBJECT::** est facultative.  
  
```sql  
GRANT UPDATE ON OBJECT::dbo.StatusTable TO [Ted];  
```  
  
```sql  
GRANT UPDATE ON dbo.StatusTable TO [Ted];  
```  
  
