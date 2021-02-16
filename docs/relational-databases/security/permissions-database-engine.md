---
title: Autorisations (moteur de base de données) | Microsoft Docs
description: Consultez cette liste complète des autorisations de SQL Server pour savoir quelles autorisations s’appliquent aux plateformes que vous utilisez.
ms.custom: ''
ms.date: 10/30/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
f1_keywords:
- sql13.swb.databaseuser.permissions.database.f1--May use common.permissions
- sql13.swb.databaseuser.permissions.object.f1--May use common.permissions
helpviewer_keywords:
- REFERENCES permission
- permissions [SQL Server]
- security [SQL Server], permissions
- naming conventions [SQL Server]
ms.assetid: f28e3dea-24e6-4a81-877b-02ec4c7e36b9
author: AndreasWolter
ms.author: anwolter
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: b716f1daa936aa700bd28984ba71d0e9bfc1d2df
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/01/2021
ms.locfileid: "100341692"
---
# <a name="permissions-database-engine"></a>Autorisations (moteur de base de données)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Chaque élément sécurisable [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a des autorisations associées qui peuvent être accordées à un principal. Les autorisations dans le [!INCLUDE[ssDE](../../includes/ssde-md.md)] sont gérées au niveau du serveur pour les connexions et les rôles de serveur, et au niveau de la base de données pour les utilisateurs de base de données et les rôles de base de données. Le modèle pour [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] a le même système pour les autorisations de base de données, mais les autorisations de niveau serveur ne sont pas disponibles. Cette rubrique contient la liste complète des autorisations. Pour une implémentation classique des autorisations, consultez [Getting Started with Database Engine Permissions](../../relational-databases/security/authentication-access/getting-started-with-database-engine-permissions.md).  
  
Le nombre total d’autorisations pour [!INCLUDE[ssSQLv15_md](../../includes/sssql19-md.md)] est de 248. [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] expose 254 autorisations. La plupart des autorisations s’appliquent à toutes les plates-formes, mais ce n’est pas le cas pour certaines d’entre elles. Par exemple, des autorisations de niveau serveur ne peuvent pas être accordées sur [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], et seules quelques autorisations sont pertinentes sur [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].
De nouvelles autorisations sont introduites progressivement avec une nouvelle version. [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] exposait 238 autorisations. [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] exposait 230 autorisations. [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] exposait 219 autorisations. [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] exposait 214 autorisations. [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] exposait 195 autorisations. La rubrique [sys.fn_builtin_permissions](../../relational-databases/system-functions/sys-fn-builtin-permissions-transact-sql.md) spécifie les permissions qui sont nouvelles dans les versions récentes.

Une fois que vous comprenez les autorisations, appliquez des autorisations de niveau serveur aux connexions et des autorisations de niveau base de données aux utilisateurs avec les instructions [GRANT](../../t-sql/statements/grant-transact-sql.md), [REVOKE](../../t-sql/statements/revoke-transact-sql.md)et [DENY](../../t-sql/statements/deny-transact-sql.md) . Par exemple :   
```sql
GRANT SELECT ON OBJECT::HumanResources.Employee TO Larry;
REVOKE SELECT ON OBJECT::HumanResources.Employee TO Larry;
```   
Pour des conseils sur la conception d’un système d’autorisations, consultez [Bien démarrer avec les autorisations du moteur de base de données](../../relational-databases/security/authentication-access/getting-started-with-database-engine-permissions.md).
  
##  <a name="permissions-naming-conventions"></a><a name="_conventions"></a> Conventions des noms des autorisations  
 La section ci-après décrit les conventions générales qui sont suivies pour affecter des noms aux autorisations.  
  
-   CONTROL  
  
     Confère des fonctionnalités de type propriété au bénéficiaire de l'autorisation. Le bénéficiaire dispose effectivement de toutes les autorisations définies sur l'élément sécurisable. Un principal qui dispose de l'autorisation CONTROL peut aussi accorder des autorisations sur l'élément sécurisable. Le modèle de sécurité de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] étant hiérarchique, l'autorisation CONTROL sur une portée particulière étend implicitement CONTROL à tous les éléments sécurisables inclus dans cette portée. Par exemple, CONTROL sur une base de données implique toutes les autorisations sur la base de données, toutes les autorisations sur tous les assemblys de la base de données, toutes les autorisations sur tous les schémas de la base de données et toutes les autorisations sur les objets de tous les schémas de la base de données.  
  
-   ALTER  
  
     Confère la capacité de modifier les propriétés, excepté l'appartenance, d'un élément sécurisable particulier. Lorsque ALTER est accordé sur une portée, ALTER octroie également la capacité de modifier, de créer ou de supprimer tous les éléments sécurisables contenus dans cette portée. Par exemple, l'autorisation ALTER sur un schéma inclut la capacité de créer, de modifier et de supprimer les objets du schéma.  
  
-   ALTER ANY \<*Server Securable*>, où *Serveur Sécurisable*, désigne n’importe quel élément sécurisable du serveur.  
  
     Confère la capacité de créer, de modifier ou de supprimer des instances individuelles de l' *Élément sécurisable du serveur*. Par exemple, ALTER ANY LOGIN confère la capacité de créer, de modifier ou de supprimer n'importe quelle connexion dans l'instance.  
  
-   ALTER ANY \<*Database Securable*> où *Élément sécurisable de la base de données* désigne n’importe quel élément sécurisable au niveau de la base de données.  
  
     Confère la capacité de créer, de modifier ou de supprimer des instances individuelles de l' *Élément sécurisable de base de données*. Par exemple, ALTER ANY SCHEMA confère la capacité de créer, de modifier ou de supprimer n'importe quel schéma dans la base de données.  
  
-   TAKE OWNERSHIP  
  
     Permet au bénéficiaire d'obtenir la propriété de l'élément sécurisable sur lequel cette autorisation est accordée.  
  
-   IMPERSONATE \<*Login*>  
  
     Permet au bénéficiaire d'emprunter l'identité impliquée dans la connexion.  
  
-   IMPERSONATE \<*User*>  
  
     Permet au bénéficiaire d'emprunter l'identité de l'utilisateur.  
  
-   CREATE \<*Server Securable*>  
  
     Confère au bénéficiaire la capacité de créer l' *Élément sécurisable du serveur*.  
  
-   CREATE \<*Database Securable*>  
  
     Confère au bénéficiaire la capacité de créer l' *Élément sécurisable de base de données*.  
  
-   CREATE \<*Schema-contained Securable*>  
  
     Confère la capacité de créer l'élément sécurisable contenu dans le schéma. Toutefois, l'autorisation ALTER sur le schéma est requise pour créer l'élément sécurisable dans un schéma particulier.  
  
-   VIEW DEFINITION  
  
     Permet au bénéficiaire d'accéder aux métadonnées.  
  
-   REFERENCES  
  
     L'autorisation REFERENCES sur une table est obligatoire pour pouvoir créer une contrainte FOREIGN KEY qui référence cette table.  
  
     L'autorisation REFERENCES est obligatoire sur un objet pour pouvoir créer une FONCTION ou une VUE avec la clause `WITH SCHEMABINDING` qui référence cet objet.  
  
## <a name="chart-of-sql-server-permissions"></a>Graphique des autorisations SQL Server  
[!INCLUDE[database-engine-permissions](../../includes/paragraph-content/database-engine-permissions.md)]
  
##  <a name="permissions-applicable-to-specific-securables"></a><a name="_securables"></a> Autorisations applicables à des éléments sécurisables spécifiques  
 Le tableau ci-dessous répertorie les principales classes d'autorisations et les types d'éléments sécurisables auxquels elles peuvent s'appliquer.  
  
|Autorisation|S’applique à|  
|----------------|----------------|  
|ALTER|Toutes les classes d’objets, à l’exception de TYPE.|  
|CONTROL|Toutes les classes d’objets : <br />AGGREGATE,<br />APPLICATION ROLE,<br />ASSEMBLY,<br />ASYMMETRIC KEY,<br />AVAILABILITY GROUP,<br />CERTIFICATE,<br />CONTRACT,<br />CREDENTIALS, DATABASE,<br />DATABASE SCOPED CREDENTIAL,<br /> DEFAULT,<br />ENDPOINT,<br />FULLTEXT CATALOG,<br />FULLTEXT STOPLIST,<br />FUNCTION,<br />LOGIN,<br />MESSAGE TYPE,<br />PROCEDURE,<br />QUEUE, <br />REMOTE SERVICE BINDING,<br />ROLE,<br />ROUTE,<br />RULE,<br />SCHEMA,<br />SEARCH PROPERTY LIST,<br />SERVER,<br />SERVER ROLE,<br />SERVICE,<br />SYMMETRIC KEY,<br />SYNONYM,<br />TABLE,<br />TYPE,<br /> USER,<br />VIEW et<br />XML SCHEMA COLLECTION|  
|Suppression|Toutes les classes d’objets, à l’exception de DATABASE SCOPED CONFIGURATION, SERVER et TYPE.|  
|Exécutez|Types CLR, scripts externes, procédures ([!INCLUDE[tsql](../../includes/tsql-md.md)] et CLR), fonctions scalaires et d’agrégation ([!INCLUDE[tsql](../../includes/tsql-md.md)] et CLR) et synonymes|  
|IMPERSONATE|Connexions et utilisateurs|  
|INSERT|Synonymes, tables et colonnes, vues et colonnes. l’autorisation peut être accordée au niveau de la base de données, du schéma ou de l’objet.|  
|RECEIVE|Files d’attente[!INCLUDE[ssSB](../../includes/sssb-md.md)]|  
|REFERENCES|AGGREGATE,<br />ASSEMBLY,<br />ASYMMETRIC KEY,<br />CERTIFICATE,<br />CONTRACT,<br />DATABASE,<br />DATABASE SCOPED CREDENTIAL,<br />FULLTEXT CATALOG,<br />FULLTEXT STOPLIST,<br />FUNCTION,<br />MESSAGE TYPE,<br />PROCEDURE,<br />QUEUE, <br />RULE,<br />SCHEMA,<br />SEARCH PROPERTY LIST,<br />SEQUENCE OBJECT, <br />SYMMETRIC KEY,<br />TABLE,<br />TYPE,<br />VIEW et<br />XML SCHEMA COLLECTION|  
|SELECT|Synonymes, tables et colonnes, vues et colonnes. l’autorisation peut être accordée au niveau de la base de données, du schéma ou de l’objet.|  
|TAKE OWNERSHIP|Toutes les classes d’objets, à l’exception de DATABASE SCOPED CONFIGURATION, LOGIN, SERVER et USER.|  
|UPDATE|Synonymes, tables et colonnes, vues et colonnes. l’autorisation peut être accordée au niveau de la base de données, du schéma ou de l’objet.|  
|VIEW CHANGE TRACKING|Schémas et tables|  
|VIEW DEFINITION|Toutes les classes d’objets, à l’exception de DATABASE SCOPED CONFIGURATION et SERVER.|  
  
> [!CAUTION]  
>  Les autorisations par défaut accordées aux objets système au moment de l'installation sont évaluées avec soin par rapport aux menaces potentielles et ne doivent pas être modifiées dans le cadre du renforcement de l'installation [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Les modifications apportées aux autorisations sur les objets système peuvent limiter ou rompre le fonctionnement et pourraient potentiellement laisser votre installation [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans un état non pris en charge.  
  
##  <a name="sql-server-permissions"></a><a name="_permissions"></a> Autorisations SQL Server  
 Le tableau ci-dessous fournit la liste complète des autorisations [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Les autorisations[!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] sont disponibles seulement pour les éléments sécurisables de base pris en charge. Les autorisations de niveau serveur ne peuvent pas être accordées dans [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]; toutefois, dans certains cas, les autorisations de base de données sont disponibles à la place.  
  
|Élément sécurisable de base|Autorisations granulaires sur les éléments sécurisables de base|Code du type d'autorisation|Élément sécurisable qui contient un élément sécurisable de base|Autorisation sur l'élément sécurisable conteneur, qui implique une autorisation granulaire sur l'élément sécurisable de base|  
|--------------------|--------------------------------------------|--------------------------|--------------------------------------------|------------------------------------------------------------------------------------------|  
|APPLICATION ROLE|ALTER|AL|DATABASE|ALTER ANY APPLICATION ROLE|  
|APPLICATION ROLE|CONTROL|CL|DATABASE|CONTROL|  
|APPLICATION ROLE|VIEW DEFINITION|VW|DATABASE|VIEW DEFINITION|  
|ASSEMBLY|ALTER|AL|DATABASE|ALTER ANY ASSEMBLY|  
|ASSEMBLY|CONTROL|CL|DATABASE|CONTROL|  
|ASSEMBLY|REFERENCES|RF|DATABASE|REFERENCES|  
|ASSEMBLY|TAKE OWNERSHIP|TO|DATABASE|CONTROL|  
|ASSEMBLY|VIEW DEFINITION|VW|DATABASE|VIEW DEFINITION|  
|ASYMMETRIC KEY|ALTER|AL|DATABASE|ALTER ANY ASYMMETRIC KEY|  
|ASYMMETRIC KEY|CONTROL|CL|DATABASE|CONTROL|  
|ASYMMETRIC KEY|REFERENCES|RF|DATABASE|REFERENCES|  
|ASYMMETRIC KEY|TAKE OWNERSHIP|TO|DATABASE|CONTROL|  
|ASYMMETRIC KEY|VIEW DEFINITION|VW|DATABASE|VIEW DEFINITION|  
|AVAILABILITY GROUP|ALTER|AL|SERVER|ALTER ANY AVAILABILITY GROUP|  
|AVAILABILITY GROUP|CONTROL|CL|SERVER|CONTROL SERVER|  
|AVAILABILITY GROUP|TAKE OWNERSHIP|TO|SERVER|CONTROL SERVER|  
|AVAILABILITY GROUP|VIEW DEFINITION|VW|SERVER|VIEW ANY DEFINITION|  
|CERTIFICATE|ALTER|AL|DATABASE|ALTER ANY CERTIFICATE|  
|CERTIFICATE|CONTROL|CL|DATABASE|CONTROL|  
|CERTIFICATE|REFERENCES|RF|DATABASE|REFERENCES|  
|CERTIFICATE|TAKE OWNERSHIP|TO|DATABASE|CONTROL|  
|CERTIFICATE|VIEW DEFINITION|VW|DATABASE|VIEW DEFINITION|  
|CONTRACT|ALTER|AL|DATABASE|ALTER ANY CONTRACT|  
|CONTRACT|CONTROL|CL|DATABASE|CONTROL|  
|CONTRACT|REFERENCES|RF|DATABASE|REFERENCES|  
|CONTRACT|TAKE OWNERSHIP|TO|DATABASE|CONTROL|  
|CONTRACT|VIEW DEFINITION|VW|DATABASE|VIEW DEFINITION|  
|DATABASE|ADMINISTER DATABASE BULK OPERATIONS|DABO|SERVER|CONTROL SERVER|
|DATABASE|ALTER|AL|SERVER|ALTER ANY DATABASE|  
|DATABASE|ALTER ANY APPLICATION ROLE|ALAR|SERVER|CONTROL SERVER|  
|DATABASE|ALTER ANY ASSEMBLY|ALAS|SERVER|CONTROL SERVER|  
|DATABASE|ALTER ANY ASYMMETRIC KEY|ALAK|SERVER|CONTROL SERVER|  
|DATABASE|ALTER ANY CERTIFICATE|ALCF|SERVER|CONTROL SERVER|  
|DATABASE|ALTER ANY COLUMN ENCRYPTION KEY|ALCK<br /><br /> S’applique à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] jusqu’à la version actuelle), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|SERVER|CONTROL SERVER|  
|DATABASE|ALTER ANY COLUMN MASTER KEY|ALCM<br /><br /> S’applique à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] jusqu’à la version actuelle), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|SERVER|CONTROL SERVER|  
|DATABASE|ALTER ANY CONTRACT|ALSC|SERVER|CONTROL SERVER|  
|DATABASE|ALTER ANY DATABASE AUDIT|ALDA|SERVER|ALTER ANY SERVER AUDIT|  
|DATABASE|ALTER ANY DATABASE DDL TRIGGER|ALTG|SERVER|CONTROL SERVER|  
|DATABASE|ALTER ANY DATABASE EVENT NOTIFICATION|ALED|SERVER|ALTER ANY EVENT NOTIFICATION|  
|DATABASE|ALTER ANY DATABASE EVENT SESSION|AADS<br /><br /> S'applique à [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|SERVER|ALTER ANY EVENT SESSION|  
|DATABASE|ALTER ANY DATABASE SCOPED CONFIGURATION|ALDC<br /><br /> S’applique à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] jusqu’à la version actuelle), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|SERVER|CONTROL SERVER|  
|DATABASE|ALTER ANY DATASPACE|ALDS|SERVER|CONTROL SERVER|  
|DATABASE|ALTER ANY EXTERNAL DATA SOURCE|AEDS|SERVER|CONTROL SERVER|  
|DATABASE|ALTER ANY EXTERNAL FILE FORMAT|AEFF|SERVER|CONTROL SERVER|  
|DATABASE|ALTER ANY FULLTEXT CATALOG|ALFT|SERVER|CONTROL SERVER|  
|DATABASE|ALTER ANY MASK|AAMK<br /><br /> S’applique à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] jusqu’à la version actuelle), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|SERVER|CONTROL SERVER|  
|DATABASE|ALTER ANY MESSAGE TYPE|ALMT|SERVER|CONTROL SERVER|  
|DATABASE|ALTER ANY REMOTE SERVICE BINDING|ALSB|SERVER|CONTROL SERVER|  
|DATABASE|ALTER ANY ROLE|ALRL|SERVER|CONTROL SERVER|  
|DATABASE|ALTER ANY ROUTE|ALRT|SERVER|CONTROL SERVER|  
|DATABASE|ALTER ANY SCHEMA|ALSM|SERVER|CONTROL SERVER|  
|DATABASE|MODIFIER UNE STRATÉGIE DE SÉCURITÉ|ALSP<br /><br /> S’applique à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] jusqu’à la version actuelle), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|SERVER|CONTROL SERVER|  
|DATABASE|ALTER ANY SENSITIVITY CLASSIFICATION|ALSP<br />S’applique à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (de SQL Server 2019 (15.x) à la version actuelle), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|DATABASE|CONTROL SERVER|
|DATABASE|ALTER ANY SERVICE|ALSV|SERVER|CONTROL SERVER|  
|DATABASE|ALTER ANY SYMMETRIC KEY|ALSK|SERVER|CONTROL SERVER|  
|DATABASE|ALTER ANY USER|ALUS|SERVER|CONTROL SERVER|  
|DATABASE|AUTHENTICATE|AUTH|SERVER|AUTHENTICATE SERVER|  
|DATABASE|BACKUP DATABASE|BADB|SERVER|CONTROL SERVER|  
|DATABASE|BACKUP LOG|BALO|SERVER|CONTROL SERVER|  
|DATABASE|CHECKPOINT|CP|SERVER|CONTROL SERVER|  
|DATABASE|CONNECT|CO|SERVER|CONTROL SERVER|  
|DATABASE|CONNECT REPLICATION|CORP|SERVER|CONTROL SERVER|  
|DATABASE|CONTROL|CL|SERVER|CONTROL SERVER|  
|DATABASE|CREATE AGGREGATE|CRAG|SERVER|CONTROL SERVER|  
|DATABASE|CREATE ASSEMBLY|CRAS|SERVER|CONTROL SERVER|  
|DATABASE|CREATE ASYMMETRIC KEY|CRAK|SERVER|CONTROL SERVER|  
|DATABASE|CREATE CERTIFICATE|CRCF|SERVER|CONTROL SERVER|  
|DATABASE|CREATE CONTRACT|CRSC|SERVER|CONTROL SERVER|  
|DATABASE|CREATE DATABASE|CRDB|SERVER|CREATE ANY DATABASE|  
|DATABASE|CREATE DATABASE DDL EVENT NOTIFICATION|CRED|SERVER|CREATE DDL EVENT NOTIFICATION|  
|DATABASE|CREATE DEFAULT|CRDF|SERVER|CONTROL SERVER|  
|DATABASE|CREATE FULLTEXT CATALOG|CRFT|SERVER|CONTROL SERVER|  
|DATABASE|CREATE FUNCTION|CRFN|SERVER|CONTROL SERVER|  
|DATABASE|CREATE MESSAGE TYPE|CRMT|SERVER|CONTROL SERVER|  
|DATABASE|CREATE PROCEDURE|CRPR|SERVER|CONTROL SERVER|  
|DATABASE|CREATE QUEUE|CRQU|SERVER|CONTROL SERVER|  
|DATABASE|CREATE REMOTE SERVICE BINDING|CRSB|SERVER|CONTROL SERVER|  
|DATABASE|CREATE ROLE|CRRL|SERVER|CONTROL SERVER|  
|DATABASE|CREATE ROUTE|CRRT|SERVER|CONTROL SERVER|  
|DATABASE|CREATE RULE|CRRU|SERVER|CONTROL SERVER|  
|DATABASE|CREATE SCHEMA|CRSM|SERVER|CONTROL SERVER|  
|DATABASE|CREATE SERVICE|CRSV|SERVER|CONTROL SERVER|  
|DATABASE|CREATE SYMMETRIC KEY|CRSK|SERVER|CONTROL SERVER|  
|DATABASE|CREATE SYNONYM|CRSN|SERVER|CONTROL SERVER|  
|DATABASE|CREATE TABLE|CRTB|SERVER|CONTROL SERVER|  
|DATABASE|CREATE TYPE|CRTY|SERVER|CONTROL SERVER|  
|DATABASE|CREATE VIEW|CRVW|SERVER|CONTROL SERVER|  
|DATABASE|CREATE XML SCHEMA COLLECTION|CRXS|SERVER|CONTROL SERVER|  
|DATABASE|Suppression|DL|SERVER|CONTROL SERVER|  
|DATABASE|Exécutez|EX|SERVER|CONTROL SERVER|  
|DATABASE|EXECUTE ANY EXTERNAL SCRIPT|EAES<br /><br /> S’applique à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] jusqu’à la version actuelle).|SERVER|CONTROL SERVER|  
|DATABASE|INSERT|IN|SERVER|CONTROL SERVER|  
|DATABASE|KILL DATABASE CONNECTION|KIDC<br /><br /> S'applique uniquement à [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]. Utilisez ALTER ANY CONNECTION dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|SERVER|ALTER ANY CONNECTION|  
|DATABASE|REFERENCES|RF|SERVER|CONTROL SERVER|  
|DATABASE|SELECT|SL|SERVER|CONTROL SERVER|  
|DATABASE|SHOWPLAN|SPLN|SERVER|ALTER TRACE|  
|DATABASE|SUBSCRIBE QUERY NOTIFICATIONS|SUQN|SERVER|CONTROL SERVER|  
|DATABASE|TAKE OWNERSHIP|TO|SERVER|CONTROL SERVER|  
|DATABASE|UNMASK|UMSK<br /><br /> S’applique à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] jusqu’à la version actuelle), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|SERVER|CONTROL SERVER|  
|DATABASE|UPDATE|UP|SERVER|CONTROL SERVER|  
|DATABASE|VIEW ANY COLUMN ENCRYPTION KEY DEFINITION|VWCK<br /><br /> S’applique à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] jusqu’à la version actuelle), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|SERVER|VIEW SERVER STATE|  
|DATABASE|VIEW ANY COLUMN MASTER KEY DEFINITION|vWCM<br /><br /> S’applique à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] jusqu’à la version actuelle), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|SERVER|VIEW SERVER STATE|  
|DATABASE|VIEW DATABASE STATE|VWDS|SERVER|VIEW SERVER STATE|  
|DATABASE|VIEW DEFINITION|VW|SERVER|VIEW ANY DEFINITION|  
|DATABASE SCOPED CREDENTIAL|ALTER|AL|DATABASE|CONTROL|
|DATABASE SCOPED CREDENTIAL|CONTROL|CL|DATABASE|CONTROL|
|DATABASE SCOPED CREDENTIAL|REFERENCES|RF|DATABASE|REFERENCES|
|DATABASE SCOPED CREDENTIAL|TAKE OWNERSHIP|TO|DATABASE|CONTROL|
|DATABASE SCOPED CREDENTIAL|VIEW DEFINITION|VW|DATABASE|VIEW DEFINITION|
|ENDPOINT|ALTER|AL|SERVER|ALTER ANY ENDPOINT|  
|ENDPOINT|CONNECT|CO|SERVER|CONTROL SERVER|  
|ENDPOINT|CONTROL|CL|SERVER|CONTROL SERVER|  
|ENDPOINT|TAKE OWNERSHIP|TO|SERVER|CONTROL SERVER|  
|ENDPOINT|VIEW DEFINITION|VW|SERVER|VIEW ANY DEFINITION|  
|FULLTEXT CATALOG|ALTER|AL|DATABASE|ALTER ANY FULLTEXT CATALOG|  
|FULLTEXT CATALOG|CONTROL|CL|DATABASE|CONTROL|  
|FULLTEXT CATALOG|REFERENCES|RF|DATABASE|REFERENCES|  
|FULLTEXT CATALOG|TAKE OWNERSHIP|TO|DATABASE|CONTROL|  
|FULLTEXT CATALOG|VIEW DEFINITION|VW|DATABASE|VIEW DEFINITION|  
|FULLTEXT STOPLIST|ALTER|AL|DATABASE|ALTER ANY FULLTEXT CATALOG|  
|FULLTEXT STOPLIST|CONTROL|CL|DATABASE|CONTROL|  
|FULLTEXT STOPLIST|REFERENCES|RF|DATABASE|REFERENCES|  
|FULLTEXT STOPLIST|TAKE OWNERSHIP|TO|DATABASE|CONTROL|  
|FULLTEXT STOPLIST|VIEW DEFINITION|VW|DATABASE|VIEW DEFINITION|  
|Connexion|ALTER|AL|SERVER|ALTER ANY LOGIN|  
|Connexion|CONTROL|CL|SERVER|CONTROL SERVER|  
|Connexion|IMPERSONATE|IM|SERVER|CONTROL SERVER|  
|Connexion|VIEW DEFINITION|VW|SERVER|VIEW ANY DEFINITION|  
|MESSAGE TYPE|ALTER|AL|DATABASE|ALTER ANY MESSAGE TYPE|  
|MESSAGE TYPE|CONTROL|CL|DATABASE|CONTROL|  
|MESSAGE TYPE|REFERENCES|RF|DATABASE|REFERENCES|  
|MESSAGE TYPE|TAKE OWNERSHIP|TO|DATABASE|CONTROL|  
|MESSAGE TYPE|VIEW DEFINITION|VW|DATABASE|VIEW DEFINITION|  
|OBJECT|ALTER|AL|SCHEMA|ALTER|  
|OBJECT|CONTROL|CL|SCHEMA|CONTROL|  
|OBJECT|Suppression|DL|SCHEMA|Suppression|  
|OBJECT|Exécutez|EX|SCHEMA|Exécutez|  
|OBJECT|INSERT|IN|SCHEMA|INSERT|  
|OBJECT|RECEIVE|RC|SCHEMA|CONTROL|  
|OBJECT|REFERENCES|RF|SCHEMA|REFERENCES|  
|OBJECT|SELECT|SL|SCHEMA|SELECT|  
|OBJECT|TAKE OWNERSHIP|TO|SCHEMA|CONTROL|  
|OBJECT|UPDATE|UP|SCHEMA|UPDATE|  
|OBJECT|VIEW CHANGE TRACKING|VWCT|SCHEMA|VIEW CHANGE TRACKING|  
|OBJECT|VIEW DEFINITION|VW|SCHEMA|VIEW DEFINITION|  
|REMOTE SERVICE BINDING|ALTER|AL|DATABASE|ALTER ANY REMOTE SERVICE BINDING|  
|REMOTE SERVICE BINDING|CONTROL|CL|DATABASE|CONTROL|  
|REMOTE SERVICE BINDING|TAKE OWNERSHIP|TO|DATABASE|CONTROL|  
|REMOTE SERVICE BINDING|VIEW DEFINITION|VW|DATABASE|VIEW DEFINITION|  
|ROLE|ALTER|AL|DATABASE|ALTER ANY ROLE|  
|ROLE|CONTROL|CL|DATABASE|CONTROL|  
|ROLE|TAKE OWNERSHIP|TO|DATABASE|CONTROL|  
|ROLE|VIEW DEFINITION|VW|DATABASE|VIEW DEFINITION|  
|ROUTE|ALTER|AL|DATABASE|ALTER ANY ROUTE|  
|ROUTE|CONTROL|CL|DATABASE|CONTROL|  
|ROUTE|TAKE OWNERSHIP|TO|DATABASE|CONTROL|  
|ROUTE|VIEW DEFINITION|VW|DATABASE|VIEW DEFINITION|  
|SEARCH PROPERTY LIST|ALTER|AL|SERVER|ALTER ANY FULLTEXT CATALOG|  
|SEARCH PROPERTY LIST|CONTROL|CL|SERVER|CONTROL|  
|SEARCH PROPERTY LIST|REFERENCES|RF|SERVER|REFERENCES|  
|SEARCH PROPERTY LIST|TAKE OWNERSHIP|TO|SERVER|CONTROL|  
|SEARCH PROPERTY LIST|VIEW DEFINITION|VW|SERVER|VIEW DEFINITION|  
|SCHEMA|ALTER|AL|DATABASE|ALTER ANY SCHEMA|  
|SCHEMA|CONTROL|CL|DATABASE|CONTROL|  
|SCHEMA|CREATE SEQUENCE|CRSO|DATABASE|CONTROL|  
|SCHEMA|Suppression|DL|DATABASE|Suppression|  
|SCHEMA|Exécutez|EX|DATABASE|Exécutez|  
|SCHEMA|INSERT|IN|DATABASE|INSERT|  
|SCHEMA|REFERENCES|RF|DATABASE|REFERENCES|  
|SCHEMA|SELECT|SL|DATABASE|SELECT|  
|SCHEMA|TAKE OWNERSHIP|TO|DATABASE|CONTROL|  
|SCHEMA|UPDATE|UP|DATABASE|UPDATE|  
|SCHEMA|VIEW CHANGE TRACKING|VWCT|DATABASE|VIEW CHANGE TRACKING|  
|SCHEMA|VIEW DEFINITION|VW|DATABASE|VIEW DEFINITION|  
|SERVER|ADMINISTER BULK OPERATIONS|ADBO|Non applicable|Non applicable|  
|SERVER|ALTER ANY AVAILABILITY GROUP|ALAG|Non applicable|Non applicable|  
|SERVER|ALTER ANY CONNECTION|ALCO|Non applicable|Non applicable|  
|SERVER|ALTER ANY CREDENTIAL|ALCD|Non applicable|Non applicable|  
|SERVER|ALTER ANY DATABASE|ALDB|Non applicable|Non applicable|  
|SERVER|ALTER ANY ENDPOINT|ALHE|Non applicable|Non applicable|  
|SERVER|ALTER ANY EVENT NOTIFICATION|ALES|Non applicable|Non applicable|  
|SERVER|ALTER ANY EVENT SESSION|AAES|Non applicable|Non applicable|  
|SERVER|ALTER ANY LINKED SERVER|ALLS|Non applicable|Non applicable|  
|SERVER|ALTER ANY LOGIN|ALLG|Non applicable|Non applicable|  
|SERVER|ALTER ANY SERVER AUDIT|ALAA|Non applicable|Non applicable|  
|SERVER|ALTER ANY SERVER ROLE|ALSR|Non applicable|Non applicable|  
|SERVER|ALTER RESOURCES|ALRS|Non applicable|Non applicable|  
|SERVER|ALTER SERVER STATE|ALSS|Non applicable|Non applicable|  
|SERVER|ALTER SETTINGS|ALST|Non applicable|Non applicable|  
|SERVER|ALTER TRACE|ALTR|Non applicable|Non applicable|  
|SERVER|AUTHENTICATE SERVER|AUTH|Non applicable|Non applicable|  
|SERVER|CONNECT ANY DATABASE|CADB|Non applicable|Non applicable|  
|SERVER|CONNECT SQL|COSQ|Non applicable|Non applicable|  
|SERVER|CONTROL SERVER|CL|Non applicable|Non applicable|  
|SERVER|CREATE ANY DATABASE|CRDB|Non applicable|Non applicable|  
|SERVER|Créer un groupe de disponibilité|CRAC|Non applicable|Non applicable|  
|SERVER|CREATE DDL EVENT NOTIFICATION|CRDE|Non applicable|Non applicable|  
|SERVER|CREATE ENDPOINT|CRHE|Non applicable|Non applicable|  
|SERVER|CREATE SERVER ROLE|CRSR|Non applicable|Non applicable|  
|SERVER|CREATE TRACE EVENT NOTIFICATION|CRTE|Non applicable|Non applicable|  
|SERVER|EXTERNAL ACCESS ASSEMBLY|XA|Non applicable|Non applicable|  
|SERVER|IMPERSONATE ANY LOGIN|IAL|Non applicable|Non applicable|  
|SERVER|SELECT ALL USER SECURABLES|SUS|Non applicable|Non applicable|  
|SERVER|SHUTDOWN|SHDN|Non applicable|Non applicable|  
|SERVER|UNSAFE ASSEMBLY|XU|Non applicable|Non applicable|  
|SERVER|VIEW ANY DATABASE|VWDB|Non applicable|Non applicable|  
|SERVER|VIEW ANY DEFINITION|VWAD|Non applicable|Non applicable|  
|SERVER|VIEW SERVER STATE|VWSS|Non applicable|Non applicable|  
|SERVER ROLE|ALTER|AL|SERVER|ALTER ANY SERVER ROLE|  
|SERVER ROLE|CONTROL|CL|SERVER|CONTROL SERVER|  
|SERVER ROLE|TAKE OWNERSHIP|TO|SERVER|CONTROL SERVER|  
|SERVER ROLE|VIEW DEFINITION|VW|SERVER|VIEW ANY DEFINITION|  
|SERVICE|ALTER|AL|DATABASE|ALTER ANY SERVICE|  
|SERVICE|CONTROL|CL|DATABASE|CONTROL|  
|SERVICE|SEND|SN|DATABASE|CONTROL|  
|SERVICE|TAKE OWNERSHIP|TO|DATABASE|CONTROL|  
|SERVICE|VIEW DEFINITION|VW|DATABASE|VIEW DEFINITION|  
|SYMMETRIC KEY|ALTER|AL|DATABASE|ALTER ANY SYMMETRIC KEY|  
|SYMMETRIC KEY|CONTROL|CL|DATABASE|CONTROL|  
|SYMMETRIC KEY|REFERENCES|RF|DATABASE|REFERENCES|  
|SYMMETRIC KEY|TAKE OWNERSHIP|TO|DATABASE|CONTROL|  
|SYMMETRIC KEY|VIEW DEFINITION|VW|DATABASE|VIEW DEFINITION|  
|TYPE|CONTROL|CL|SCHEMA|CONTROL|  
|TYPE|Exécutez|EX|SCHEMA|Exécutez|  
|TYPE|REFERENCES|RF|SCHEMA|REFERENCES|  
|TYPE|TAKE OWNERSHIP|TO|SCHEMA|CONTROL|  
|TYPE|VIEW DEFINITION|VW|SCHEMA|VIEW DEFINITION|  
|Utilisateur|ALTER|AL|DATABASE|ALTER ANY USER|  
|Utilisateur|CONTROL|CL|DATABASE|CONTROL|  
|Utilisateur|IMPERSONATE|IM|DATABASE|CONTROL|  
|Utilisateur|VIEW DEFINITION|VW|DATABASE|VIEW DEFINITION|  
|XML SCHEMA COLLECTION|ALTER|AL|SCHEMA|ALTER|  
|XML SCHEMA COLLECTION|CONTROL|CL|SCHEMA|CONTROL|  
|XML SCHEMA COLLECTION|Exécutez|EX|SCHEMA|Exécutez|  
|XML SCHEMA COLLECTION|REFERENCES|RF|SCHEMA|REFERENCES|  
|XML SCHEMA COLLECTION|TAKE OWNERSHIP|TO|SCHEMA|CONTROL|  
|XML SCHEMA COLLECTION|VIEW DEFINITION|VW|SCHEMA|VIEW DEFINITION|  
  
##  <a name="summary-of-the-permission-check-algorithm"></a><a name="_algorithm"></a> Résumé de l’algorithme de vérification des autorisations  
 La vérification des autorisations peut être complexe. L'algorithme de vérification des autorisations englobe les membres de groupes qui se chevauchent et le chaînage des propriétés, l'autorisation explicite et implicite, et peut être affecté par les autorisations sur les classes sécurisables qui contiennent l'entité sécurisable. Le processus général de l'algorithme consiste à collecter toutes les autorisations pertinentes. Si aucun blocage DENY n'est rencontré, l'algorithme recherche une instruction GRANT fournissant un accès suffisant. L'algorithme contient trois éléments essentiels : le **contexte de sécurité**, l' **espace d'autorisation** et l' **autorisation requise**.  
  
> [!NOTE]  
>  Vous ne pouvez pas accorder, refuser ou révoquer des autorisations à sa, dbo, le propriétaire de l'entité (entity owner), information_schema, sys ou à vous-même.  
  
-   **Contexte de sécurité**  
  
     Il s'agit du groupe de principaux qui apporte les autorisations à la vérification d'accès. Ces autorisations sont liées à la connexion ou à l'utilisateur actif, à moins que le contexte de sécurité n'ait été modifié au profit d'une autre connexion ou d'un autre utilisateur par le biais de l'instruction EXECUTE AS. Le contexte de sécurité se compose des principaux suivants :  
  
    -   la connexion ;  
  
    -   L’utilisateur  
  
    -   les appartenances aux rôles ;  
  
    -   les appartenances aux groupes Windows ;  
  
    -   si la signature de module est utilisée, toute connexion ou compte d'utilisateur du certificat utilisé pour signer le module actuellement exécuté par l'utilisateur, ainsi que les appartenances aux rôles associées de ce principal.  
  
-   **Espace d’autorisation**  
  
     Correspond à l'entité sécurisable et aux classes sécurisables qui contiennent l'élément sécurisable. Par exemple, une table (entité sécurisable) est contenue par la classe sécurisable de schéma et par la classe sécurisable de base de données. L'accès peut être affecté par des autorisations de niveau table, schéma, base de données et serveur. Pour plus d’informations, consultez [Hiérarchie des autorisations &#40;moteur de base de données&#41;](../../relational-databases/security/permissions-hierarchy-database-engine.md).  
  
-   **Autorisation obligatoire**  
  
     Correspond au type d'autorisation requise. Il peut s'agir, par exemple, d'une autorisation INSERT, UPDATE, DELETE, SELECT, EXECUTE, ALTER, CONTROL, etc.  
  
     L'accès peut nécessiter plusieurs autorisations, comme l'illustrent les exemples suivants :  
  
    -   Une procédure stockée peut nécessiter une autorisation EXECUTE sur la procédure stockée et une autorisation INSERT sur plusieurs tables référencées par la procédure stockée.  
  
    -   Une vue de gestion dynamique peut nécessiter à la fois une autorisation VIEW SERVER STATE et une autorisation SELECT sur la vue.  
  
### <a name="general-steps-of-the-algorithm"></a>Étapes générales de l'algorithme  
 Au moment de déterminer si l'accès à un élément sécurisable doit être autorisé, l'algorithme peut suivre différentes étapes en fonction des principaux et des éléments sécurisables concernés. Cependant, l'algorithme exécute les étapes générales suivantes :  
  
1.  Contournement de la procédure de vérification des autorisations si la connexion est membre du rôle serveur fixe sysadmin ou si l'utilisateur est l'utilisateur dbo dans la base de données active.  
  
2.  Autorisation de l'accès si le chaînage des propriétés est applicable et si la vérification de l'accès sur l'objet plus tôt dans la chaîne a passé avec succès le contrôle de sécurité.  
  
3.  Agrégation des identités de niveau serveur, base de données et du module signé qui sont associées à l'appelant pour créer le **contexte de sécurité**.  
  
4.  Pour ce **contexte de sécurité**, collecte de toutes les autorisations accordées ou refusées pour l' **espace d'autorisation**. L'autorisation peut être explicitement déclarée comme une autorisation GRANT, GRANT WITH GRANT ou DENY ; les autorisations peuvent également correspondre à une autorisation implicite ou couvrante GRANT ou DENY. Par exemple, l'autorisation CONTROL sur un schéma implique une autorisation CONTROL sur une table. Et une autorisation CONTROL sur une table implique une autorisation SELECT. Par conséquent, si l'autorisation CONTROL a été accordée sur le schéma, l'autorisation SELECT l'est également sur la table. Si l'autorisation CONTROL a été refusée sur la table, l'autorisation SELECT l'est tout autant sur la table.  
  
    > [!NOTE]  
    >  Une instruction GRANT associée à une autorisation au niveau colonne se substitue à une instruction DENY au niveau objet.  
  
5.  Identification de l' **autorisation requise**.  
  
6.  Échec de la vérification des autorisations si l' **autorisation requise** est directement ou implicitement refusée à l'une des identités dans le **contexte de sécurité** pour les objets contenus dans l' **espace d'autorisation**.  
  
7.  Succès de la vérification des autorisations si l' **autorisation requise** n'a pas été refusée et si l' **autorisation requise** contient une autorisation GRANT ou GRANT WITH GRANT directement ou implicitement accordée à l'une des identités dans le **contexte de sécurité** pour un objet contenu dans l' **espace d'autorisation**.  

## <a name="special-considerations-for-column-level-permissions"></a>Considérations spéciales relatives aux autorisations au niveau des colonnes

Les autorisations au niveau des colonnes sont accordées avec la syntaxe *<nom_table>(\<column _name>)* . Par exemple :
```sql
GRANT SELECT ON OBJECT::Customer(CustomerName) TO UserJoe;
```
Une instruction DENY sur la table est remplacée par une instruction GRANT sur une colonne. Toutefois, une instruction DENY ultérieure sur la table supprime la colonne GRANT. 
  
##  <a name="examples"></a><a name="_examples"></a> Exemples  
 Les exemples de cette section montrent comment récupérer des informations relatives aux autorisations.  
  
### <a name="a-returning-the-complete-list-of-grantable-permissions"></a>R. Retour de la liste complète des autorisations accordables  
 L'instruction suivante retourne toutes les autorisations du [!INCLUDE[ssDE](../../includes/ssde-md.md)] à l'aide de la fonction `fn_builtin_permissions` . Pour plus d’informations, consultez [sys.fn_builtin_permissions &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-builtin-permissions-transact-sql.md).  
  
```sql  
SELECT * FROM fn_builtin_permissions(default);  
GO  
```  
  
### <a name="b-returning-the-permissions-on-a-particular-class-of-objects"></a>B. Retour des autorisations sur une classe d'objets particulière  
 L'exemple suivant utilise la fonction `fn_builtin_permissions` pour consulter toutes les autorisations disponibles pour une catégorie d'élément sécurisable donnée. L'exemple retourne les autorisations sur les assemblys.  
  
```sql  
SELECT * FROM fn_builtin_permissions('assembly');  
GO    
```  
  
### <a name="c-returning-the-permissions-granted-to-the-executing-principal-on-an-object"></a>C. Retour des autorisations accordées au principal en cours d'exécution sur un objet  
 L'exemple suivant utilise la fonction `fn_my_permissions` pour retourner la liste des autorisations effectives détenues par le principal appelant sur un élément sécurisable spécifié. L'exemple retourne les autorisations sur un objet nommé `Orders55`. Pour plus d’informations, consultez [sys.fn_my_permissions &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-my-permissions-transact-sql.md).  
  
```sql  
SELECT * FROM fn_my_permissions('Orders55', 'object');  
GO  
```  
  
### <a name="d-returning-the-permissions-applicable-to-a-specified-object"></a>D. Retour des autorisations applicables à un objet spécifié  
 L'exemple suivant retourne les autorisations applicables à un objet nommé `Yttrium`. Notez que la fonction intégrée `OBJECT_ID` est utilisée pour récupérer l'identificateur de l'objet `Yttrium`.  
  
```sql  
SELECT * FROM sys.database_permissions   
    WHERE major_id = OBJECT_ID('Yttrium');  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Hiérarchie des autorisations &#40;Moteur de base de données&#41;](../../relational-databases/security/permissions-hierarchy-database-engine.md)   
 [sys.database_permissions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-permissions-transact-sql.md)  
  
  
