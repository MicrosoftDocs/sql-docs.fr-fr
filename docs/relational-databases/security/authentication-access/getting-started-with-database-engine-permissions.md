---
title: Bien démarrer avec les autorisations du moteur de base de données | Microsoft Docs
description: Passez en revue certains concepts de sécurité de base dans SQL Server et découvrez une implémentation classique des autorisations Moteur de base de données.
ms.custom: ''
ms.date: 01/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: security
ms.topic: quickstart
helpviewer_keywords:
- permissions [SQL Server], getting started
ms.assetid: 051af34e-bb5b-403e-bd33-007dc02eef7b
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: cc483082fe25fbed8a8e989544e189ccdc81bb73
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596244"
---
# <a name="getting-started-with-database-engine-permissions"></a>Prise en main des autorisations du moteur de base de données
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Les autorisations dans le [!INCLUDE[ssDE](../../../includes/ssde-md.md)] sont gérées au niveau du serveur, par le biais des connexions et des rôles serveur, et au niveau de la base de données, par le biais des utilisateurs de base de données et des rôles de base de données. Le modèle du [!INCLUDE[ssSDS](../../../includes/sssds-md.md)] expose le même système dans chaque base de données, mais les autorisations de niveau serveur ne sont pas disponibles. Cette rubrique passe en revue les concepts de sécurité de base, puis décrit une implémentation classique des autorisations.  
  
## <a name="security-principals"></a>Principaux de sécurité  
 Le principal de sécurité est le nom officiel des identités qui utilisent [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] et qui peuvent être autorisées à prendre des mesures. Ce sont généralement des personnes ou des groupes de personnes, mais il peut également s’agir d’entités qui se font passer pour des personnes. Les principaux de sécurité peuvent être créés et gérés à l’aide des instructions [!INCLUDE[tsql](../../../includes/tsql-md.md)] répertoriées ou de [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)].  
  
##### <a name="logins"></a>Connexions  
 Les connexions sont des comptes d’utilisateur qui permettent d’ouvrir une session sur le [!INCLUDE[ssDEnoversion](../../../includes/ssdenoversion-md.md)]. [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] et [!INCLUDE[ssSDS](../../../includes/sssds-md.md)] prennent en charge les connexions basées sur l’authentification Windows et les connexions basées sur l’authentification [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Pour plus d’informations sur les deux types de connexions, consultez [Choose an Authentication Mode](../../../relational-databases/security/choose-an-authentication-mode.md).  
  
##### <a name="fixed-server-roles"></a>Rôles serveur fixes  
 Dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], les rôles serveur fixes sont un ensemble de rôles préconfigurés qui fournissent un groupe pratique d’autorisations de niveau serveur. Les connexions peuvent être ajoutées aux rôles à l’aide de l’instruction `ALTER SERVER ROLE ... ADD MEMBER` . Pour plus d’informations, consultez [ALTER SERVER ROLE &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-server-role-transact-sql.md). [!INCLUDE[ssSDS](../../../includes/sssds-md.md)] ne prend pas en charge les rôles serveur fixes, mais a deux rôles dans la base de données MASTER (`dbmanager` et `loginmanager`) qui agissent comme des rôles serveur.  
  
##### <a name="user-defined-server-roles"></a>Rôles de serveur définis par l’utilisateur  
 Dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], vous pouvez créer vos propres rôles serveur et leur attribuer des autorisations de niveau serveur. Les connexions peuvent être ajoutées aux rôles de serveur à l’aide de l’instruction `ALTER SERVER ROLE ... ADD MEMBER` . Pour plus d’informations, consultez [ALTER SERVER ROLE &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-server-role-transact-sql.md). [!INCLUDE[ssSDS](../../../includes/sssds-md.md)] ne prend pas en charge les rôles serveur définis par l’utilisateur.  
  
##### <a name="database-users"></a>Utilisateurs de base de données  
 Pour qu’une connexion puisse accéder à une base de données, un utilisateur de base de données doit être créé dans une base de données, puis mappé à la connexion. En général, le nom d’utilisateur de base de données est le même que le nom de connexion, mais ce n’est pas obligatoire. Chaque utilisateur de base de données est mappé à une seule connexion. Une connexion ne peut être mappée qu’à un seul utilisateur dans une base de données, mais peut être mappée comme utilisateur de base de données dans plusieurs bases de données.  
  
 En outre, les utilisateurs de base de données peuvent être créés sans avoir de connexion correspondante. Ils sont appelés *utilisateurs de base de données autonome*. [!INCLUDE[msCoName](../../../includes/msconame-md.md)] encourage l’utilisation de ces utilisateurs, car elle facilite le déplacement de votre base de données vers un autre serveur. Comme une connexion, un utilisateur de base de données autonome peut utiliser l’authentification Windows ou l’authentification [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Pour plus d’informations, voir [Utilisateurs de base de données autonome - Rendre votre base de données portable](../../../relational-databases/security/contained-database-users-making-your-database-portable.md).  
  
 Il existe 12 types d’utilisateurs, qui varient légèrement selon la façon dont ils s’authentifient et qui ils représentent. Pour voir une liste d’utilisateurs, consultez [CREATE USER &#40;Transact-SQL&#41;](../../../t-sql/statements/create-user-transact-sql.md).  
  
##### <a name="fixed-database-roles"></a>Rôles de base de données fixes  
 Les rôles de base de données fixes sont un ensemble de rôles préconfigurés qui fournissent un groupe pratique d’autorisations de niveau base de données. Les utilisateurs de base de données et les rôles de base de données définis par l’utilisateur peuvent être ajoutés aux rôles de base de données fixes à l’aide de l’instruction `ALTER ROLE ... ADD MEMBER`. Pour plus d’informations, consultez [ALTER ROLE &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-role-transact-sql.md).  
  
##### <a name="user-defined-database-roles"></a>Rôles de base de données définis par l’utilisateur  
 Les utilisateurs ayant l’autorisation `CREATE ROLE` peuvent créer des rôles de base de données définis par l’utilisateur pour représenter des groupes d’utilisateurs disposant d’autorisations courantes. En général, les autorisations sont accordées ou refusées à l’ensemble du rôle, ce qui simplifie la gestion et la surveillance des autorisations. Les utilisateurs de base de données peuvent être ajoutés aux rôles de base de données à l’aide de l’instruction `ALTER ROLE ... ADD MEMBER` . Pour plus d’informations, consultez [ALTER ROLE &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-role-transact-sql.md).  
  
##### <a name="other-principals"></a>Autres principaux  
 Des principaux de sécurité supplémentaires non présentés ici incluent les rôles d’application, ainsi que les connexions et les utilisateurs basés sur des certificats ou des clés asymétriques.  
  
 Pour obtenir un graphique montrant les relations entre les utilisateurs Windows, les groupes Windows, les connexions et les utilisateurs de base de données, consultez [Create a Database User](../../../relational-databases/security/authentication-access/create-a-database-user.md).  
  
## <a name="typical-scenario"></a>Scénario typique  
 L’exemple suivant illustre une méthode courante et recommandée pour configurer des autorisations.  
  
#### <a name="in-active-directory-or-azure-active-directory"></a>Dans Active Directory ou Azure Active Directory :  
  
1.  Créez un utilisateur Windows pour chaque personne.  
  
2.  Créez des groupes Windows qui représentent les unités de travail et les fonctions de travail.  
  
3.  Ajoutez les utilisateurs Windows aux groupes Windows.  

#### <a name="if-the-person-connecting-will-be-connecting-to-many-databases"></a>Si la personne qui se connecte doit se connecter à plusieurs bases de données  
  
1.  Créez une connexion pour les groupes Windows. (Si vous utilisez l’authentification [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , ignorez les étapes d’Active Directory et créez ici des connexions d’authentification [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .)  
  
2.  Dans la base de données utilisateur, créez un utilisateur de base de données pour la connexion représentant les groupes Windows.  
  
3.  Dans la base de données utilisateur, créez un ou plusieurs rôles de base de données définis par l’utilisateur, chacun représentant une fonction similaire. Par exemple, analyste financier et analyste des ventes.  
  
4.  Ajoutez les utilisateurs de base de données à un ou plusieurs rôles de base de données définis par l’utilisateur.  
  
5.  Accordez des autorisations aux rôles de base de données définis par l’utilisateur.  
  
#### <a name="if-the-person-connecting-will-be-connecting-to-only-one-database"></a>Si la personne qui se connecte doit se connecter à une seule base de données  
  
1.  Dans la base de données utilisateur, créez un utilisateur de base de données autonome pour le groupe Windows. (Si vous utilisez l’authentification [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], ignorez les étapes d’Active Directory et créez ici une authentification [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] d’utilisateur de base de données autonome.)  
  
1.  Dans la base de données utilisateur, créez un ou plusieurs rôles de base de données définis par l’utilisateur, chacun représentant une fonction similaire. Par exemple, analyste financier et analyste des ventes.  
  
1.  Ajoutez les utilisateurs de base de données à un ou plusieurs rôles de base de données définis par l’utilisateur.  
  
1.  Accordez des autorisations aux rôles de base de données définis par l’utilisateur.  
  
 En général, à ce stade, un utilisateur Windows est devenu membre d’un groupe Windows. Le groupe Windows dispose d’une connexion dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ou [!INCLUDE[ssSDS](../../../includes/sssds-md.md)]. La connexion est mappée à une identité d’utilisateur dans la base de données utilisateur. L’utilisateur est membre d’un rôle de base de données. Vous devez maintenant ajouter des autorisations au rôle.  
  
## <a name="assigning-permissions"></a>Affectation d’autorisations  
 La plupart des instructions d’autorisation ont le format suivant :  
  
```  
AUTHORIZATION  PERMISSION  ON  SECURABLE::NAME  TO  PRINCIPAL;  
```  
  
-   `AUTHORIZATION` doit être `GRANT`, `REVOKE` ou `DENY`.  
  
-   `PERMISSION` établit l’action autorisée ou interdite. [!INCLUDE[ssSQL15](../../../includes/sssql16-md.md)] peut spécifier 230 autorisations. [!INCLUDE[ssSDS](../../../includes/sssds-md.md)] a moins d’autorisations, car certaines actions ne sont pas pertinentes dans Azure. Les autorisations sont répertoriées dans la rubrique [Autorisations &#40;moteur de base de données&#41;](../../../relational-databases/security/permissions-database-engine.md) et dans le graphique ci-dessous.  
  
-   `ON SECURABLE::NAME` est le type d’élément sécurisable (serveur, objet serveur, base de données ou objet de base de données) et son nom. Certaines autorisations n’exigent pas `ON SECURABLE::NAME` , car le contexte ne le justifie pas. Par exemple, l’autorisation `CREATE TABLE` n’exige pas la clause `ON SECURABLE::NAME`. (Par exemple, `GRANT CREATE TABLE TO Mary;` permet à Mary de créer des tables.)  
  
-   `PRINCIPAL` est le principal de sécurité (connexion, utilisateur ou rôle) qui reçoit ou perd l’autorisation. Accordez des autorisations aux rôles chaque fois que possible.  
  
 Dans l’exemple d’instruction grant suivante, l’autorisation `UPDATE` est accordée sur la table ou vue `Parts` contenue dans le schéma `Production` au rôle nommé `PartsTeam`:  
  
```  
GRANT UPDATE ON OBJECT::Production.Parts TO PartsTeam;  
```  
  
 Les autorisations sont accordées aux principaux de sécurité (connexions, utilisateurs et rôles) à l’aide de l’instruction `GRANT` . Les autorisations sont refusées explicitement à l’aide de la commande  `DENY` . Une autorisation précédemment accordée ou refusée est supprimée à l’aide de l’instruction `REVOKE` . Les autorisations sont cumulatives : l’utilisateur bénéficie de toutes les autorisations accordées à lui-même, à la connexion et à toute appartenance à un groupe. Toutefois, tout refus d’autorisation remplace toutes les attributions.  
  
> [!TIP]  
>  Une erreur courante consiste à tenter de supprimer un `GRANT` à l’aide de `DENY` au lieu de `REVOKE`. Cela peut engendrer des problèmes quand un utilisateur reçoit des autorisations de plusieurs sources, ce qui est assez courant. L’exemple suivant illustre le principal.  
  
 Le groupe Sales reçoit des autorisations `SELECT` sur la table OrderStatus par le biais de l’instruction `GRANT SELECT ON OBJECT::OrderStatus TO Sales;`. L’utilisateur Ted est membre du rôle Sales. Ted se voit également accorder l’autorisation `SELECT` sur la table OrderStatus sous son propre nom d’utilisateur par le biais de l’instruction  `GRANT SELECT ON OBJECT::OrderStatus TO Ted;`. Supposons que l’administrateur souhaite supprimer le `GRANT` associé au rôle Sales.  
  
-   Si l’administrateur exécute correctement `REVOKE SELECT ON OBJECT::OrderStatus TO Sales;`, Ted conserve l’accès `SELECT` à la table OrderStatus par le biais de son instruction `GRANT` individuelle.  
  
-   Par contre, si l’administrateur exécute `DENY SELECT ON OBJECT::OrderStatus TO Sales;` incorrectement, Ted, en tant que membre du rôle Sales, se voit refuser l’autorisation `SELECT` , car l’instruction `DENY` associée à Sales remplace son instruction  `GRANT`individuelle.  
  
> [!NOTE]  
>  Les autorisations peuvent être configurées à l’aide de [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)]. Recherchez l’élément sécurisable dans l’Explorateur d’objets, cliquez dessus avec le bouton droit, puis sélectionnez **Propriétés**. Sélectionnez la page **Autorisations** . Pour plus d’aide sur l’utilisation de la page des autorisations, consultez [Permissions or Securables Page](../../../relational-databases/security/permissions-or-securables-page.md).  
  
## <a name="permission-hierarchy"></a>Hiérarchie d’autorisations  
 Les autorisations ont une hiérarchie parent/enfant. Autrement dit, si vous accordez l’autorisation `SELECT` sur une base de données, cette autorisation inclut l’autorisation `SELECT` sur tous les schémas (enfants) dans la base de données. Si vous accordez l’autorisation `SELECT` sur un schéma, elle inclut l’autorisation `SELECT` sur toutes les tables et vues (enfants) dans le schéma. Les autorisations sont transitives : si vous accordez l’autorisation `SELECT` sur une base de données, elle inclut l’autorisation `SELECT` sur tous les schémas (enfant) et toutes les tables et vues (petits-enfants).  
  
 En outre, les autorisations ont des autorisations couvrantes. L’autorisation `CONTROL` sur un objet vous donne normalement toutes les autres autorisations sur l’objet.  
  
 Étant donné que la hiérarchie parent/enfant et la hiérarchie de couverture peuvent agir sur la même autorisation, le système d’autorisation peut se compliquer. Par exemple, prenons une table (Region), dans un schéma (Customers), dans une base de données (SalesDB).  
  
-   `CONTROL` sur la table Region inclut toutes les autres autorisations sur la table Region, y compris `ALTER`, `SELECT`, `INSERT`,  `UPDATE`, `DELETE`, et certaines autres autorisations.  
  
-   `SELECT` sur le schéma Customers qui possède la table Region inclut l’autorisation `SELECT` sur la table Region.  
  
 Ainsi, l’autorisation `SELECT` sur la table Region peut être obtenue par le biais de ces six instructions :  
  
```  
GRANT SELECT ON OBJECT::Region TO Ted;   
  
GRANT CONTROL ON OBJECT::Region TO Ted;   
  
GRANT SELECT ON SCHEMA::Customers TO Ted;   
  
GRANT CONTROL ON SCHEMA::Customers TO Ted;   
  
GRANT SELECT ON DATABASE::SalesDB TO Ted;   
  
GRANT CONTROL ON DATABASE::SalesDB TO Ted;  
```  
  
## <a name="grant-the-least-permission"></a>Accorder l’autorisation minimale  
 La première autorisation répertoriée ci-dessus (`GRANT SELECT ON OBJECT::Region TO Ted;`) est la plus granulaire ; autrement dit, cette instruction est l’autorisation minimale la plus stricte pour accorder l’autorisation `SELECT`. Aucune autorisation sur des objets subordonnés ne l’accompagne. Par principe, accordez toujours l’autorisation minimale la plus stricte possible. Toutefois, si les circonstances imposent une simplification du système d’octroi, accordez une autorisation plus générale. Ainsi, si Ted a besoin d’autorisations pour l’ensemble du schéma, accordez `SELECT` une fois au niveau du schéma, au lieu d’accorder `SELECT` au niveau table ou vue plusieurs fois. La conception de la base de données détermine en grande partie l’efficacité de cette stratégie. Cette dernière fonctionne de façon optimale si votre base de données permet d’inclure dans un seul schéma les objets nécessitant des autorisations identiques.  
  
## <a name="list-of-permissions"></a>Liste d’autorisations  
 [!INCLUDE[ssSQL15](../../../includes/sssql16-md.md)] a 230 autorisations. [!INCLUDE[ssSQL14](../../../includes/sssql14-md.md)] a 219 autorisations. [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)] a 214 autorisations. [!INCLUDE[ssKilimanjaro](../../../includes/sskilimanjaro-md.md)] a 195 autorisations. [!INCLUDE[ssSDS](../../../includes/sssds-md.md)], [!INCLUDE[ssDW](../../../includes/ssdw-md.md)]et [!INCLUDE[ssAPS](../../../includes/ssaps-md.md)] ont moins d’autorisations, car ils exposent uniquement une partie du moteur de base de données, bien que chacun ait des autorisations qui ne s’appliquent pas à [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. 
 
 [!INCLUDE[database-engine-permissions](../../../includes/paragraph-content/database-engine-permissions.md)]
 
 Pour obtenir un graphique montrant les relations entre les principaux [!INCLUDE[ssDE](../../../includes/ssde-md.md)] et les objets serveur et de base de données, consultez [Hiérarchie des autorisations &#40;moteur de base de données&#41;](../../../relational-databases/security/permissions-hierarchy-database-engine.md).  
  
## <a name="permissions-vs-fixed-server-and-fixed-database-roles"></a>Autorisations associées aux rôles serveur fixes et rôles de base de données fixes  
 Les autorisations des rôles serveur fixes et des rôles de base de données fixes sont similaires, mais ne sont pas exactement les mêmes que les autorisations granulaires. Par exemple, les membres du rôle serveur fixe `sysadmin` disposent de toutes les autorisations sur l’instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], de même que les connexions avec l’autorisation `CONTROL SERVER` . Toutefois, l’octroi de l’autorisation `CONTROL SERVER` ne fait pas d’une connexion un membre du rôle serveur fixe sysadmin et l’ajout d’une connexion au rôle serveur fixe `sysadmin` n’octroie pas explicitement à celle-ci l’autorisation `CONTROL SERVER`. Parfois, une procédure stockée vérifie les autorisations en examinant le rôle fixe et pas l’autorisation granulaire. Par exemple, le détachement d’une base de données requiert l’appartenance au rôle de base de données fixe `db_owner` . L’autorisation `CONTROL DATABASE` équivalente n’est pas suffisante. Ces deux systèmes fonctionnent en parallèle, mais interagissent rarement. Microsoft recommande d’utiliser le système d’autorisation granulaire plus récent au lieu des rôles fixes chaque fois que possible.
  
## <a name="monitoring-permissions"></a>Surveillance des autorisations  
 Les vues suivantes retournent des informations de sécurité.  
  
-   Vous pouvez examiner les connexions et les rôles serveur définis par l’utilisateur sur un serveur à l’aide de la vue `sys.server_principals` . Cette vue n’est pas disponible dans [!INCLUDE[ssSDS](../../../includes/sssds-md.md)].  
  
-   Vous pouvez examiner les utilisateurs et les rôles définis par l’utilisateur dans une base de données à l’aide de la vue `sys.database_principals` .  
  
-   Vous pouvez examiner les autorisations accordées aux connexions et aux rôles serveur fixes définis par l’utilisateur à l’aide de la vue `sys.server_permissions` . Cette vue n’est pas disponible dans [!INCLUDE[ssSDS](../../../includes/sssds-md.md)].  
  
-   Vous pouvez examiner les autorisations accordées aux utilisateurs et aux rôles de base de données fixes définis par l’utilisateur à l’aide de la vue `sys.database_permissions` .  
  
-   L’appartenance au rôle de base de données peut être examinée à l’aide de la vue `sys. sys.database_role_members` .  
  
-   L’appartenance au rôle serveur peut être examinée à l’aide de la vue `sys.server_role_members` . Cette vue n’est pas disponible dans [!INCLUDE[ssSDS](../../../includes/sssds-md.md)].  
  
-   Pour obtenir des vues supplémentaires associées à la sécurité, consultez [Affichages catalogue de sécurité &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md) .  
  
### <a name="useful-transact-sql-statements"></a>Instructions Transact-SQL utiles  
 Les instructions suivantes retournent des informations utiles sur les autorisations.  
  
 Pour retourner les autorisations explicites accordées ou refusées dans une base de données ([!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] et [!INCLUDE[ssSDS](../../../includes/sssds-md.md)]), exécutez l’instruction suivante dans la base de données.  
  
```sql  
SELECT   
    perms.state_desc AS State,   
    permission_name AS [Permission],   
    obj.name AS [on Object],   
    dPrinc.name AS [to User Name]  
FROM sys.database_permissions AS perms  
JOIN sys.database_principals AS dPrinc  
    ON perms.grantee_principal_id = dPrinc.principal_id  
JOIN sys.objects AS obj  
    ON perms.major_id = obj.object_id;  
```  
  
 Pour retourner les membres des rôles serveur ([!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] uniquement), exécutez l’instruction suivante.  
  
```sql  
SELECT sRole.name AS [Server Role Name] , sPrinc.name AS [Members]  
FROM sys.server_role_members AS sRo  
JOIN sys.server_principals AS sPrinc  
    ON sRo.member_principal_id = sPrinc.principal_id  
JOIN sys.server_principals AS sRole  
    ON sRo.role_principal_id = sRole.principal_id;  
```  
  
 
 Pour retourner les membres des rôles de base de données ([!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] et [!INCLUDE[ssSDS](../../../includes/sssds-md.md)]), exécutez l’instruction suivante.  
  
```sql  
SELECT dRole.name AS [Database Role Name], dPrinc.name AS [Members]  
FROM sys.database_role_members AS dRo  
JOIN sys.database_principals AS dPrinc  
    ON dRo.member_principal_id = dPrinc.principal_id  
JOIN sys.database_principals AS dRole  
    ON dRo.role_principal_id = dRole.principal_id;  
```  
  
## <a name="next-steps"></a>Étapes suivantes  
 Les rubriques suivantes vous aideront à démarrer :  
  
-   [Tutoriel : Bien démarrer avec le moteur de base de données](../../../relational-databases/tutorial-getting-started-with-the-database-engine.md) 

-   [Création d’une base de données &#40;Didacticiel&#41;](../../../t-sql/lesson-1-creating-database-objects.md)  
  
-   [Tutoriel : SQL Server Management Studio](../../../ssms/quickstarts/ssms-connect-query-sql-server.md)  
  
-   [Tutoriel : Écriture d’instructions Transact-SQL](../../../t-sql/tutorial-writing-transact-sql-statements.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Centre de sécurité pour le moteur de base de données SQL Server et Azure SQL Database](../../../relational-databases/security/security-center-for-sql-server-database-engine-and-azure-sql-database.md)   
 [Fonctions de sécurité &#40;Transact-SQL&#41;](../../../t-sql/functions/security-functions-transact-sql.md)   
 [Fonctions et vues de gestion dynamique relatives à la sécurité &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/security-related-dynamic-management-views-and-functions-transact-sql.md)   
 [Affichages catalogue de sécurité &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)   
 [sys.fn_builtin_permissions &#40;Transact-SQL&#41;](../../../relational-databases/system-functions/sys-fn-builtin-permissions-transact-sql.md)   
 [Détermination des autorisations de moteur de base de données effectives](../../../relational-databases/security/authentication-access/determining-effective-database-engine-permissions.md)
