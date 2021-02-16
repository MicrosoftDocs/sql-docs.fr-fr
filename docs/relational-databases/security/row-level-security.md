---
title: Sécurité au niveau des lignes
description: Découvrez comment la sécurité au niveau des lignes vous permet d'utiliser l'appartenance à un groupe ou le contexte d'exécution pour contrôler l'accès aux lignes dans une table de base de données sur SQL Server.
ms.custom: ''
ms.date: 09/01/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- access control predicates
- row level security
- security [SQL Server], predicate based access control
- row level security described
- predicate based security
author: VanMSFT
ms.author: vanto
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: a842f6ed226fe6b298dc9063f5d6c64f287fc82e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340164"
---
# <a name="row-level-security"></a>Sécurité au niveau des lignes

[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  ![Graphique de sécurité au niveau des lignes](../../relational-databases/security/media/row-level-security-graphic.png "Graphique de sécurité au niveau des lignes")  
  
La sécurité au niveau des lignes vous permet d'utiliser l'appartenance à un groupe ou le contexte d'exécution pour contrôler l'accès aux lignes dans une table de base de données.
  
La sécurité au niveau des lignes simplifie la conception et codage de la sécurité dans votre application. La sécurité au niveau des lignes vous aide à implémenter des restrictions sur l'accès aux lignes de données. Par exemple, vous pouvez vous assurer que les collaborateurs accèdent uniquement aux lignes de données qui sont pertinentes pour leur département. Un autre exemple consiste à restreindre l’accès aux données de clients aux seules données relatives à leur entreprise.  
  
La logique de la restriction d'accès est située dans la couche de base de données plutôt que loin des données d'une autre couche Application. Le système de base de données applique les restrictions d'accès chaque fois que cet accès aux données est tenté à partir d'une couche quelconque. Cela rend votre système de sécurité plus fiable et robuste en réduisant sa surface d’exposition.  
  
Implémentez une sécurité au niveau des lignes à l’aide de l’instruction [CREATE SECURITY POLICY](../../t-sql/statements/create-security-policy-transact-sql.md)[!INCLUDE[tsql](../../includes/tsql-md.md)], et de prédicats créés en tant que [fonctions table inline](../../relational-databases/user-defined-functions/create-user-defined-functions-database-engine.md).  

[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)], [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] ([L’obtenir](/azure/azure-sql/database/features-comparison?WT.mc_id=TSQL_GetItTag)), [!INCLUDE[ssSDW](../../includes/sssdw-md.md)].
  
> [!NOTE]
> Azure Synapse prend en charge uniquement des prédicats de filtre. Les prédicats BLOCK ne sont actuellement pas pris en charge dans Azure Synapse.

## <a name="description"></a><a name="Description"></a> Description

La sécurité au niveau des lignes prend en charge deux types de prédicats de sécurité.  
  
- Les prédicats de filtre filtrent en mode silencieux les lignes disponibles pour les opérations de lecture (SELECT, UPDATE et DELETE).  
  
- Les prédicats BLOCK bloquent explicitement les opérations d’écriture (AFTER INSERT, AFTER UPDATE, BEFORE UPDATE et BEFORE DELETE) qui violent le prédicat.  
  
 L’accès aux données au niveau des lignes dans une table est restreint par un prédicat de sécurité défini en tant que fonction table incluse. La fonction est ensuite appelée et appliquée par une stratégie de sécurité. Pour les prédicats de filtre, l’application n’est pas consciente des lignes qui sont filtrées à partir du jeu de résultats. Si toutes les lignes sont filtrées, un jeu de valeurs null est retourné. Pour les prédicats BLOCK, toutes les opérations qui violent le prédicat échouent avec une erreur.  
  
 Les prédicats de filtre sont appliqués lors de la lecture des données de la table de base. Ils affectent toutes les opérations get : **SELECT**, **DELETE**  et **UPDATE**. Les utilisateurs ne peuvent pas sélectionner ou supprimer des lignes qui sont filtrées. L’utilisateur ne peut pas mettre à jour des lignes qui sont filtrées. Toutefois, il est possible de mettre à jour les lignes de sorte qu’elles seront filtrées par la suite. Les prédicats BLOCK affectent toutes les opérations d’écriture.  
  
- Les prédicats AFTER INSERT et AFTER UPDATE peuvent empêcher les utilisateurs de mettre à jour des lignes avec des valeurs qui violent le prédicat.  
  
- Les prédicats BEFORE UPDATE peuvent empêcher les utilisateurs de mettre à jour les lignes qui violent réellement le prédicat.  
  
- Les prédicats BEFORE DELETE peuvent bloquer les opérations de suppression.  
  
 Les prédicats de filtre et BLOCK, ainsi que les stratégies de sécurité, se comportent comme suit :  
  
- Vous pouvez définir une fonction de prédicat qui crée une jointure avec une autre table et/ou appelle une fonction. Si la stratégie de sécurité est créée avec `SCHEMABINDING = ON` (valeur par défaut), alors la jointure ou la fonction est accessible à partir de la requête et fonctionne comme prévu, sans aucun contrôle d’autorisation supplémentaire. Si la stratégie de sécurité est créée avec `SCHEMABINDING = OFF`, alors les utilisateurs ont besoin d’autorisations **SELECT** sur ces tables et fonctions supplémentaires pour interroger la table cible. Si la fonction de prédicat appelle une fonction scalaire CLR, l’autorisation **EXECUTE** est également nécessaire.
  
- Vous pouvez émettre une requête sur une table pour laquelle un prédicat de sécurité est défini, mais désactivé. Les lignes qui sont filtrées ou bloquées ne sont pas affectées.  
  
- Si un utilisateur dbo, membre du rôle **db_owner** ou le propriétaire de la table interrogent une table pour laquelle une stratégie de sécurité est définie et activée, les lignes sont filtrées ou bloquées conformément à celle-ci.  
  
- Toute tentative de modification du schéma d’une table liée par une stratégie de sécurité liée au schéma génère une erreur. En revanche, les colonnes non référencées par le prédicat peuvent être modifiées.  
  
- Toute tentative d’ajouter un prédicat sur une table pour laquelle un prédicat est déjà défini pour l’opération spécifiée génère une erreur. C’est le cas que le prédicat soit activé ou non.
  
- Toute tentative de modifier une fonction utilisée comme prédicat sur une table à l’intérieur d’une stratégie de sécurité liée à un schéma génère une erreur.  
  
- La définition de plusieurs stratégies de sécurité actives contenant des prédicats sans chevauchement réussit.  
  
 Les prédicats de filtre se comportent comme suit :  
  
- Définir une stratégie de sécurité qui filtre les lignes d'une table. L'application ignore que des lignes sont filtrées pour les opérations **SELECT**, **UPDATE** et **DELETE**. Ceci inclut les situations où toutes les lignes sont filtrées. L'application peut insérer (**INSERT**) des lignes même si elles seront filtrées lors d’une autre opération.  
  
 Les prédicats BLOCK se comportent comme sui  :  
  
- Les prédicats BLOCK pour l’opération UPDATE sont divisés en opérations distinctes pour BEFORE et AFTER. Vous ne pouvez donc pas, par exemple, bloquer des utilisateurs en les empêchant de mettre à jour une ligne pour obtenir une valeur supérieure à la valeur actuelle. Si ce type de logique est requis, vous devez utiliser des déclencheurs avec les tables intermédiaires [DELETED et INSERTED](../triggers/use-the-inserted-and-deleted-tables.md) pour référencer ensemble les valeurs anciennes et nouvelles.  
  
- L’optimiseur ne vérifie pas un prédicat BLOCK AFTER UPDATE si les colonnes utilisées par la fonction de prédicat n’ont pas été modifiées. Par exemple : Alice ne doit pas être en mesure de modifier un salaire au-delà de 100 000. Alice peut modifier l’adresse d’un employé dont le salaire est déjà supérieur à 100 000 tant que les colonnes référencées dans le prédicat n’ont pas été modifiées.  
  
- Aucune modification n’a été apportée aux API bulk, y compris à BULK INSERT. Cela signifie que les prédicats BLOCK AFTER INSERT s’appliquent aux opérations d’insertion en bloc comme s’il s’agissait d’opérations d’insertion standard.  
  
## <a name="use-cases"></a><a name="UseCases"></a> Cas d'usage

 Voici des exemples d'utilisation de la sécurité au niveau des lignes :  
  
- Un hôpital peut créer une stratégie de sécurité qui autorise les infirmières à n'afficher les lignes de données que pour leurs patients.  
  
- Une banque peut créer une stratégie pour limiter l'accès aux lignes de données financières en fonction du service de l'employé ou du rôle de l'employé au sein de la société.  
  
- Une application mutualisée peut créer une stratégie afin d'appliquer une séparation logique entre les lignes de données de chaque locataire et celles des autres locataires. L'efficacité est obtenue grâce au stockage des données de nombreux locataires dans une seule table. Chaque locataire peut afficher uniquement ses lignes de données.  
  
 Les prédicats de filtre de la sécurité au niveau des lignes sont fonctionnellement équivalents à l'ajout d'une clause **WHERE** . Le prédicat peut être aussi sophistiqué que l'exigent les pratiques professionnelles, ou la clause aussi simple que `WHERE TenantId = 42`.  
  
 En termes plus formels, la sécurité au niveau des lignes introduit le contrôle d'accès basé sur le prédicat. Il comprend une évaluation flexible, centralisée et basée sur un prédicat. Le prédicat peut reposer sur des métadonnées ou sur tout autre critère que l’administrateur définit comme il convient. Le prédicat est utilisé comme critère pour déterminer si l'utilisateur a l'accès approprié aux données en fonction de ses propres attributs. Un contrôle d’accès en fonction d’une étiquette peut être implémenté en utilisant un contrôle d’accès en fonction d’un prédicat.  
  
## <a name="permissions"></a><a name="Permissions"></a> Autorisations

 La création, modification ou suppression des stratégies de sécurité nécessite l'autorisation **ALTER ANY SECURITY POLICY** . La création ou suppression d'une stratégie de sécurité nécessite l'autorisation **ALTER** sur le schéma.  
  
 En outre, les autorisations suivantes sont requises pour chaque prédicat ajouté :  
  
- Les autorisations **SELECT** et **REFERENCES** sur la fonction utilisée comme prédicat.  
  
- L'autorisation **REFERENCES** sur la table cible liée à la stratégie.  
  
- L'autorisation **REFERENCES** sur chaque colonne de la table cible utilisée comme argument.  
  
 Les stratégies de sécurité s'appliquent à tous les utilisateurs, y compris les utilisateurs dbo de la base de données. Les utilisateurs dbo peuvent modifier ou supprimer les stratégies de sécurité, mais leurs modifications des stratégies de sécurité peuvent être auditées. Si des utilisateurs à privilèges élevés, tels que sysadmin ou db_owner, doivent voir toutes les lignes afin de dépanner ou de valider des données, la stratégie de sécurité doit être écrite pour le permettre.  
  
 Si une stratégie de sécurité est créée avec `SCHEMABINDING = OFF`, alors pour interroger la table cible, les utilisateurs doivent avoir des autorisations  **SELECT** ou **EXECUTE** sur la fonction de prédicat et toutes les autres tables, vues ou fonctions utilisées au sein de la fonction de prédicat. Si une stratégie de sécurité est créée avec `SCHEMABINDING = ON` (valeur par défaut), alors ces contrôles d’autorisations sont ignorés quand les utilisateurs interrogent la table cible.  
  
## <a name="best-practices"></a><a name="Best"></a> Bonnes pratiques  
  
- Il est fortement recommandé de créer un schéma distinct pour les objets SNL : les fonctions de prédicat et les stratégies de sécurité. Cela permet de séparer les autorisations requises sur ces objets spéciaux des tables cibles. Une séparation supplémentaire pour les différentes stratégies et fonctions de prédicat peut être nécessaire dans les bases de données multi-locataires, mais pas systématiquement.
  
- L’autorisation **ALTER ANY SECURITY POLICY** est destinée aux utilisateurs disposant de privilèges élevés (par exemple, un gestionnaire de stratégie de sécurité). Le gestionnaire de stratégie de sécurité ne nécessite pas l'autorisation **SELECT** sur les tables qu'il protège.  
  
- Évitez les conversions de type dans les fonctions de prédicat pour éviter les erreurs d'exécution potentielles.  
  
- Évitez la récursivité dans les fonctions de prédicat chaque fois que possible pour éviter une dégradation des performances. L’optimiseur de requête tente de détecter les récursivités directes, mais il n’est pas garanti qu’il trouve les récursivités indirectes. Dans la récursivité indirecte, une deuxième fonction appelle la fonction de prédicat.  
  
- Évitez d'utiliser les jointures de table excessives dans les fonctions de prédicat pour optimiser les performances.  
  
 Évitez toute logique de prédicat dépendant d’[options SET](../../t-sql/statements/set-statements-transact-sql.md) propres à la session : s’il est peu probable qu’elles soient utilisées dans des applications pratiques, les fonctions de prédicat dont la logique dépend de certaines options **SET** propres à la session peuvent entraîner des fuites d’informations si des utilisateurs sont en mesure d’exécuter des requêtes arbitraires. Par exemple, une fonction de prédicat qui convertit implicitement une chaîne en **datetime** pourrait filtrer des lignes différentes, selon l’option **SET DATEFORMAT** définie pour la session active. En règle générale, les fonctions de prédicat doivent respecter les règles suivantes :  
  
- Les fonctions de prédicat ne doivent pas convertir implicitement des chaînes de caractères en **date**, **smalldatetime**, **datetime**, **datetime2** ou **datetimeoffset**, ou inversement, car ces conversions sont affectées par les options [SET DATEFORMAT &#40;Transact-SQL&#41;](../../t-sql/statements/set-dateformat-transact-sql.md) et [SET LANGUAGE &#40;Transact-SQL&#41;](../../t-sql/statements/set-language-transact-sql.md). Utilisez plutôt la fonction **CONVERT** , et spécifiez explicitement le paramètre de style.  
  
- Les fonctions de prédicat ne doivent pas dépendre de la valeur du premier jour de la semaine, car celle-ci est affectée par l’option [SET DATEFIRST &#40;Transact-SQL&#41;](../../t-sql/statements/set-datefirst-transact-sql.md).  
  
- Les fonctions de prédicat ne doivent pas dépendre d’expressions arithmétiques ou d’agrégation retournant **NULL** en cas d’erreur (telle qu’un dépassement ou une division par zéro), car ce comportement est affecté par les options [SET ANSI_WARNINGS &#40;Transact-SQL&#41;](../../t-sql/statements/set-ansi-warnings-transact-sql.md), [SET NUMERIC_ROUNDABORT &#40;Transact-SQL&#41;](../../t-sql/statements/set-numeric-roundabort-transact-sql.md) et [SET ARITHABORT &#40;Transact-SQL&#41;](../../t-sql/statements/set-arithabort-transact-sql.md).  
  
- Les fonctions de prédicat ne doivent pas comparer des chaînes concaténées à **NULL**, car ce comportement est affecté par l’option [SET CONCAT_NULL_YIELDS_NULL &#40;Transact-SQL&#41;](../../t-sql/statements/set-concat-null-yields-null-transact-sql.md).  

## <a name="security-note-side-channel-attacks"></a><a name="SecNote"></a> Remarque relative à la sécurité : Attaques par canal auxiliaire

### <a name="malicious-security-policy-manager"></a>Gestionnaire de stratégie de sécurité malveillant

il est important d’observer qu’un gestionnaire de stratégie de sécurité malveillant, avec les autorisations suffisantes pour créer une stratégie de sécurité sur une colonne sensible, et qui dispose de l’autorisation de créer ou modifier des fonctions table inline, peut s’entendre avec un autre utilisateur ayant les autorisations select sur une table pour effectuer une exfiltration des données en créant à des fins malveillantes des fonctions table inline destinées à déclencher des attaques côté canal pour en déduire des données. Ces attaques nécessitent une collusion (ou des autorisations excessives accordées à un utilisateur malveillant) et probablement plusieurs itérations de modification de la stratégie (nécessitant l'autorisation de supprimer le prédicat pour rompre la liaison de schéma), de modification des fonctions table inline et d'exécution répétée d'instructions select sur la table cible. Nous vous recommandons de limiter les autorisations au strict nécessaire et de surveiller toute activité suspecte. Une activité telles qu’une modification constante des stratégies et des fonctions table incluses liées à la sécurité au niveau des lignes doit être analysée.  
  
### <a name="carefully-crafted-queries"></a>Requêtes élaborées avec soin

Il est possible de provoquer des fuites d'informations via l'utilisation de requêtes élaborées avec soin. Par exemple, `SELECT 1/(SALARY-100000) FROM PAYROLL WHERE NAME='John Doe'` permettrait à un utilisateur malveillant de savoir que le salaire de John Doe est 100 000 $. Même s'il existe un prédicat de sécurité en vigueur pour empêcher un utilisateur malveillant d'interroger directement le salaire d'autres personnes, l'utilisateur peut déterminer si la requête retourne une exception de division par zéro.  

## <a name="cross-feature-compatibility"></a><a name="Limitations"></a> Compatibilité entre fonctionnalités

 En général, la sécurité au niveau des lignes fonctionne comme prévu pour les fonctionnalités. Il existe cependant quelques exceptions. Cette section contient plusieurs remarques et avertissements concernant l’utilisation de la sécurité au niveau des lignes avec certaines autres fonctionnalités de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
- **DBCC SHOW_STATISTICS** fournit des statistiques sur des données non filtrées et peut entraîner des fuites d’informations qui sont autrement protégées par une stratégie de sécurité. Pour cette raison, l’accès pour afficher un objet de statistiques d’une table avec une stratégie de sécurité au niveau des lignes est limité. L'utilisateur doit être propriétaire de la table ou membre du rôle serveur fixe sysadmin , du rôle de base de données fixe db_owner ou du rôle de base de données fixe db_ddladmin.  
  
- **Flux de fichier :** la sécurité au niveau des lignes est incompatible avec le flux de fichier.  
  
- **PolyBase :** La sécurité au niveau des lignes est prise en charge avec des tables externes PolyBase pour Azure Synapse uniquement.

- **Tables à mémoire optimisée :** la fonction table incluse utilisée comme prédicat de sécurité sur une table optimisée en mémoire doit être définie avec l’option `WITH NATIVE_COMPILATION`. Avec cette option, les fonctionnalités de langue non prises en charge par les tables optimisées en mémoire sont interdites, et l’erreur appropriée est au moment de la création. Pour plus d’informations, consultez la section **Sécurité au niveau des lignes dans les tables optimisées en mémoire** dans [Introduction aux tables optimisées en mémoire](../../relational-databases/in-memory-oltp/introduction-to-memory-optimized-tables.md).  
  
- **Vues indexées :** en général, des stratégies de sécurité peuvent être créées sur des vues, et des vues créées sur des tables liées par des stratégies de sécurité. En revanche, des vues indexées ne peuvent pas être créées sur des tables ayant une stratégie de sécurité, car les recherches de ligne via l’index contourneraient la stratégie.  
  
- **Capture des changements de données :** la capture des changements de données peut entraîner une fuite de lignes entières qui devraient être filtrées pour les membres de **db_owner** ou les utilisateurs membres du rôle de régulation spécifié, lorsque la capture des changements de données est activée pour une table (remarque : vous pouvez définir explicitement cette fonction sur **NULL** pour permettre à tous les utilisateurs d’accéder aux données modifiées). En effet, **db_owner** et les membres de ce rôle de régulation peuvent voir toutes les modifications de données sur une table, même si une stratégie de sécurité s’applique à celle-ci.  
  
- **Change tracking :** Change Tracking peut entraîner une fuite de la clé primaire de lignes qui devraient être filtrées concernant les utilisateurs disposant d’autorisations **SELECT** et **VIEW CHANGE TRACKING**. La fuite n’a pas trait aux valeurs de données réelles, mais uniquement à la survenance d’une mise à jour, d’une insertion ou d’une suppression dans une colonne A pour la ligne contenant une clé primaire B. Cela peut être problématique si la clé primaire contient un élément confidentiel tel un numéro de sécurité sociale. Toutefois, dans la pratique, cette fonction **CHANGETABLE** est presque toujours jointe à la table d’origine afin d’obtenir les données les plus récentes.  
  
- **Recherche en texte intégral :** un gain de performances est attendu pour des requêtes utilisant les fonctions de recherche en texte intégral et de recherche sémantique ci-après, en raison d’une jointure supplémentaire introduite pour appliquer la sécurité au niveau des lignes et éviter la fuite des clés primaires de lignes qui devraient être filtrées : **CONTAINSTABLE**, **FREETEXTTABLE**, semantickeyphrasetable, semanticsimilaritydetailstable, semanticsimilaritytable.  
  
- **Index columnstore :** la sécurité au niveau des lignes est compatible aussi bien avec les index columnstore cluster que non-cluster. Toutefois, étant donné que la sécurité au niveau des lignes s’applique à une fonction, l’optimiseur peut modifier le plan de requête de façon à ne pas utiliser le mode batch.  
  
- **Vues partitionnées :** les prédicats BLOCK ne peuvent pas être définis sur des vues partitionnées, et celles-ci ne peuvent pas être créées sur des tables qui utilisent des prédicats BLOCK. Les prédicats de filtre sont compatibles avec les vues partitionnées.  
  
- **Tables temporelles :** les tables temporelles sont compatibles avec la fonction de sécurité au niveau des lignes. Toutefois, les prédicats de sécurité sur la table actuelle ne sont pas automatiquement répliqués dans la table de l’historique. Pour appliquer une stratégie de sécurité aux tables actuelle et de l’historique, vous devez ajouter un prédicat de sécurité à chaque table.  
  
## <a name="examples"></a><a name="CodeExamples"></a> Exemples  
  
### <a name="a-scenario-for-users-who-authenticate-to-the-database"></a><a name="Typical"></a> A. Scénario pour les utilisateurs qui s’authentifient auprès de la base de données

 Cet exemple crée trois utilisateurs et crée et remplit une table de six lignes. Il crée ensuite une fonction table incluse et une stratégie de sécurité pour la table. L'exemple montre ensuite comment les instructions select sont filtrées pour les différents utilisateurs.  
  
 Créez trois comptes d'utilisateur qui illustrent différentes fonctionnalités d'accès.  


```sql  
CREATE USER Manager WITHOUT LOGIN;  
CREATE USER Sales1 WITHOUT LOGIN;  
CREATE USER Sales2 WITHOUT LOGIN;  
```

Créez une table pour stocker des données.  

```sql
CREATE TABLE Sales  
    (  
    OrderID int,  
    SalesRep sysname,  
    Product varchar(10),  
    Qty int  
    );  
```

 Remplissez la table avec six lignes de données, en affichant trois commandes pour chaque représentant commercial.  

```sql
INSERT INTO Sales VALUES (1, 'Sales1', 'Valve', 5);
INSERT INTO Sales VALUES (2, 'Sales1', 'Wheel', 2);
INSERT INTO Sales VALUES (3, 'Sales1', 'Valve', 4);
INSERT INTO Sales VALUES (4, 'Sales2', 'Bracket', 2);
INSERT INTO Sales VALUES (5, 'Sales2', 'Wheel', 5);
INSERT INTO Sales VALUES (6, 'Sales2', 'Seat', 5);
-- View the 6 rows in the table  
SELECT * FROM Sales;
```

Accordez l'accès en lecture sur la table à chaque utilisateur.  

```sql
GRANT SELECT ON Sales TO Manager;  
GRANT SELECT ON Sales TO Sales1;  
GRANT SELECT ON Sales TO Sales2;  
```

Créez un schéma et une fonction table inline. La fonction renvoie 1 lorsqu'une ligne de la colonne SalesRep est identique à l'utilisateur exécutant la requête (`@SalesRep = USER_NAME()`) ou si l'utilisateur exécutant la requête est l'utilisateur Manager (`USER_NAME() = 'Manager'`).

```sql
CREATE SCHEMA Security;  
GO  
  
CREATE FUNCTION Security.fn_securitypredicate(@SalesRep AS sysname)  
    RETURNS TABLE  
WITH SCHEMABINDING  
AS  
    RETURN SELECT 1 AS fn_securitypredicate_result
WHERE @SalesRep = USER_NAME() OR USER_NAME() = 'Manager';  
```

Créez une stratégie de sécurité en ajoutant la fonction comme prédicat de filtre. L'état doit être défini sur ON pour activer la stratégie.

```sql
CREATE SECURITY POLICY SalesFilter  
ADD FILTER PREDICATE Security.fn_securitypredicate(SalesRep)
ON dbo.Sales  
WITH (STATE = ON);  
```

Autoriser des autorisations SELECT à la fonction fn_securitypredicate 
```sql
GRANT SELECT ON security.fn_securitypredicate TO Manager;  
GRANT SELECT ON security.fn_securitypredicate TO Sales1;  
GRANT SELECT ON security.fn_securitypredicate TO Sales2;  
```

Maintenant testez le prédicat de filtrage, tel que sélectionné à partir de la table Sales pour chaque utilisateur.

```sql
EXECUTE AS USER = 'Sales1';  
SELECT * FROM Sales;
REVERT;  
  
EXECUTE AS USER = 'Sales2';  
SELECT * FROM Sales;
REVERT;  
  
EXECUTE AS USER = 'Manager';  
SELECT * FROM Sales;
REVERT;  
```

L'utilisateur Manager doit visualiser l'ensemble des six lignes. Les utilisateurs Sales1 et Sales2 doivent voir uniquement leurs propres ventes.

Modifiez la stratégie de sécurité pour désactiver la stratégie.

```sql
ALTER SECURITY POLICY SalesFilter  
WITH (STATE = OFF);  
```

Les utilisateurs Sales1 et Sales2 peuvent maintenant visualiser l'ensemble des six lignes.

Connectez-vous à la base de données SQL pour nettoyer les ressources

```sql
DROP USER Sales1;
DROP USER Sales2;
DROP USER Manager;

DROP SECURITY POLICY SalesFilter;
DROP TABLE Sales;
DROP FUNCTION Security.fn_securitypredicate;
DROP SCHEMA Security;
```

### <a name="b-scenarios-for-using-row-level-security-on-an-azure-synapse-external-table"></a><a name="external"></a> B. Scénarios pour l’utilisation de la sécurité au niveau des lignes sur une table externe Azure Synapse

Ce petit exemple crée trois utilisateurs et une table externe de six lignes. Il crée ensuite une fonction table incluse et une stratégie de sécurité pour la table externe. L'exemple montre comment les instructions select sont filtrées pour les différents utilisateurs. 

### <a name="prerequisites"></a>Prérequis

1. Vous devez disposer d’un pool SQL dédié. Consultez [Création d’un pool SQL dédié](/azure/synapse-analytics/sql-data-warehouse/create-data-warehouse-portal).
1. Le serveur qui héberge votre pool SQL dédié doit être inscrit auprès d’AAD. Par ailleurs, vous devez disposer d’un compte Stockage Azure doté d’autorisations de contributeur aux données du blog de Stockage. Suivez cette [procédure](/azure/azure-sql/database/vnet-service-endpoint-rule-overview#steps).
1. Créez un système de fichiers pour votre compte de stockage Azure. Utilisez l’Explorateur Stockage pour afficher votre compte de stockage. Cliquez avec le bouton droit sur des conteneurs, puis sélectionnez *Créer un système de fichiers*.  

Une fois les prérequis en place, créez trois comptes d’utilisateur qui illustrent différentes fonctionnalités d’accès.

```sql
--run in master
CREATE LOGIN Manager WITH PASSWORD = '<user_password>'
GO
CREATE LOGIN Sales1 WITH PASSWORD = '<user_password>'
GO
CREATE LOGIN Sales2 WITH PASSWORD = '<user_password>'
GO

--run in master and your dedicated SQL pool database
CREATE USER Manager FOR LOGIN Manager;  
CREATE USER Sales1  FOR LOGIN Sales1;  
CREATE USER Sales2  FOR LOGIN Sales2 ;
```

Créez une table pour stocker des données.  

```sql
CREATE TABLE Sales  
    (  
    OrderID int,  
    SalesRep sysname,  
    Product varchar(10),  
    Qty int  
    );  
```

Remplissez la table avec six lignes de données, en affichant trois commandes pour chaque représentant commercial.  

```sql
INSERT INTO Sales VALUES (1, 'Sales1', 'Valve', 5);
INSERT INTO Sales VALUES (2, 'Sales1', 'Wheel', 2);
INSERT INTO Sales VALUES (3, 'Sales1', 'Valve', 4);
INSERT INTO Sales VALUES (4, 'Sales2', 'Bracket', 2);
INSERT INTO Sales VALUES (5, 'Sales2', 'Wheel', 5);
INSERT INTO Sales VALUES (6, 'Sales2', 'Seat', 5);
-- View the 6 rows in the table  
SELECT * FROM Sales;
```

Créez une table externe Azure Synapse à partir de la table Sales que vous venez de créer.

```sql
CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<user_password>';

CREATE DATABASE SCOPED CREDENTIAL msi_cred WITH IDENTITY = 'Managed Service Identity';

CREATE EXTERNAL DATA SOURCE ext_datasource_with_abfss WITH (TYPE = hadoop, LOCATION = 'abfss://<file_system_name@storage_account>.dfs.core.windows.net', CREDENTIAL = msi_cred);

CREATE EXTERNAL FILE FORMAT MSIFormat  WITH (FORMAT_TYPE=DELIMITEDTEXT);
  
CREATE EXTERNAL TABLE Sales_ext WITH (LOCATION='<your_table_name>', DATA_SOURCE=ext_datasource_with_abfss, FILE_FORMAT=MSIFormat, REJECT_TYPE=Percentage, REJECT_SAMPLE_VALUE=100, REJECT_VALUE=100)
AS SELECT * FROM sales;
```

Accordez SELECT pour les trois utilisateurs sur la table externe Sales_ext que vous avez créée.

```sql
GRANT SELECT ON Sales_ext TO Sales1;  
GRANT SELECT ON Sales_ext TO Sales2;  
GRANT SELECT ON Sales_ext TO Manager;
```

Créez un schéma et une fonction table inlined. Vous avez peut-être déjà effectué cette opération dans l’exemple A. La fonction retourne 1 quand une ligne de la colonne SalesRep est identique à l’utilisateur qui exécute la requête (`@SalesRep = USER_NAME()`) ou si l’utilisateur qui exécute la requête correspond à l’utilisateur Manager (`USER_NAME() = 'Manager'`).

```sql
CREATE SCHEMA Security;  
GO  
  
CREATE FUNCTION Security.fn_securitypredicate(@SalesRep AS sysname)  
    RETURNS TABLE  
WITH SCHEMABINDING  
AS  
    RETURN SELECT 1 AS fn_securitypredicate_result
WHERE @SalesRep = USER_NAME() OR USER_NAME() = 'Manager';  
```

Créez une stratégie de sécurité sur votre table externe en utilisant la fonction table inlined comme prédicat de filtre. L'état doit être défini sur ON pour activer la stratégie.

```sql
CREATE SECURITY POLICY SalesFilter_ext
ADD FILTER PREDICATE Security.fn_securitypredicate(SalesRep)
ON dbo.Sales_ext  
WITH (STATE = ON);
```

Testez maintenant le prédicat de filtrage en effectuant une sélection dans la table externe Sales_ex. Connectez-vous chacun en tant qu’utilisateur, Sales1, Sales2 et manager. Exécutez la commande suivante en tant qu’utilisateur.

```sql
SELECT * FROM Sales_ext;
```

L'utilisateur Manager doit visualiser l'ensemble des six lignes. Les utilisateurs Sales1 et Sales2 ne doivent voir que leurs propres ventes.

Modifiez la stratégie de sécurité pour désactiver la stratégie.

```sql
ALTER SECURITY POLICY SalesFilter_ext  
WITH (STATE = OFF);  
```

Les utilisateurs Sales1 et Sales2 peuvent maintenant visualiser l'ensemble des six lignes.

Connectez-vous à la base de données Azure Synapse pour nettoyer les ressources

```sql
DROP USER Sales1;
DROP USER Sales2;
DROP USER Manager;

DROP SECURITY POLICY SalesFilter_ext;
DROP TABLE Sales;
DROP EXTERNAL TABLE Sales_ext;
DROP EXTERNAL DATA SOURCE ext_datasource_with_abfss ;
DROP EXTERNAL FILE FORMAT MSIFormat;
DROP DATABASE SCOPED CREDENTIAL msi_cred; 
DROP MASTER KEY;
```

Connectez-vous au maître logique pour nettoyer les ressources.

```sql
DROP LOGIN Sales1;
DROP LOGIN Sales2;
DROP LOGIN Manager;
```

### <a name="c-scenario-for-users-who-connect-to-the-database-through-a-middle-tier-application"></a><a name="MidTier"></a> C. Scénario pour les utilisateurs qui se connectent à la base de données via une application intermédiaire

> [!NOTE]
> Dans cet exemple la fonctionnalité Prédicats BLOCK n’est pas actuellement prise en charge pour Azure Synapse, par conséquent, l’insertion de lignes pour le mauvais identificateur d'utilisateur n’est pas bloquée avec Azure Synapse.

Cet exemple montre comment une application de couche intermédiaire peut implémenter le filtrage des connexions, lorsque les utilisateurs (ou locataires) d'application partagent le même utilisateur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (l'application). L’application définit l’ID d’utilisateur d’application actuel dans [SESSION_CONTEXT &#40;Transact-SQL&#41;](../../t-sql/functions/session-context-transact-sql.md) après la connexion à la base de données, puis les stratégies de sécurité filtrent en toute transparence les lignes qui ne devraient pas être visibles par cet ID, et empêchent l’utilisateur d’insérer des lignes pour l’ID d’utilisateur incorrect. Aucune autre modification de l'application n'est nécessaire.  
  
 Créez une table pour stocker des données.

```sql
CREATE TABLE Sales (  
    OrderId int,  
    AppUserId int,  
    Product varchar(10),  
    Qty int  
);  
```

Remplissez la table avec six lignes de données, en affichant trois commandes pour chaque utilisateur d’application.

```sql
INSERT Sales VALUES
    (1, 1, 'Valve', 5),
    (2, 1, 'Wheel', 2),
    (3, 1, 'Valve', 4),  
    (4, 2, 'Bracket', 2),
    (5, 2, 'Wheel', 5),
    (6, 2, 'Seat', 5);  
```

Créez un utilisateur à faibles privilèges que l’application utilisera pour se connecter.

```sql
-- Without login only for demo  
CREATE USER AppUser WITHOUT LOGIN;
GRANT SELECT, INSERT, UPDATE, DELETE ON Sales TO AppUser;  
  
-- Never allow updates on this column  
DENY UPDATE ON Sales(AppUserId) TO AppUser;  
```

Créez un schéma et une fonction de prédicat qui utilisent l’ID d’utilisateur de l’application stocké dans **SESSION_CONTEXT** pour filtrer les lignes.

```sql
CREATE SCHEMA Security;  
GO  
  
CREATE FUNCTION Security.fn_securitypredicate(@AppUserId int)  
    RETURNS TABLE  
    WITH SCHEMABINDING  
AS  
    RETURN SELECT 1 AS fn_securitypredicate_result  
    WHERE  
        DATABASE_PRINCIPAL_ID() = DATABASE_PRINCIPAL_ID('AppUser')
        AND CAST(SESSION_CONTEXT(N'UserId') AS int) = @AppUserId;
GO  
```

Créez une stratégie de sécurité qui ajoute cette fonction en tant que prédicat de filtre et prédicat BLOCK sur la table `Sales`. Le prédicat BLOCK a uniquement besoin de **AFTER INSERT**, car **BEFORE UPDATE** et **BEFORE DELETE** sont déjà filtrés, et **AFTER UPDATE** n’est pas nécessaire puisque la colonne `AppUserId` ne peut pas être mise à jour avec d’autres valeurs, en raison de l’autorisation définie précédemment.

```sql
CREATE SECURITY POLICY Security.SalesFilter  
    ADD FILTER PREDICATE Security.fn_securitypredicate(AppUserId)
        ON dbo.Sales,  
    ADD BLOCK PREDICATE Security.fn_securitypredicate(AppUserId)
        ON dbo.Sales AFTER INSERT
    WITH (STATE = ON);  
```

À présent, nous pouvons simuler le filtrage des connexions en opérant une sélection dans la table `Sales` , après avoir défini différents ID d’utilisateur dans **SESSION_CONTEXT**. Dans la pratique, l’application est chargée de définir l’ID d’utilisateur actuel dans **SESSION_CONTEXT** après l’ouverture d’une connexion.

```sql
EXECUTE AS USER = 'AppUser';  
EXEC sp_set_session_context @key=N'UserId', @value=1;  
SELECT * FROM Sales;  
GO  
  
/* Note: @read_only prevents the value from changing again until the connection is closed (returned to the connection pool)*/
EXEC sp_set_session_context @key=N'UserId', @value=2, @read_only=1;
  
SELECT * FROM Sales;  
GO  
  
INSERT INTO Sales VALUES (7, 1, 'Seat', 12); -- error: blocked from inserting row for the wrong user ID  
GO  
  
REVERT;  
GO  
```

Nettoyez les ressources de base de données.

```sql
DROP USER AppUser;

DROP SECURITY POLICY Security.SalesFilter;
DROP TABLE Sales;
DROP FUNCTION Security.fn_securitypredicate;
DROP SCHEMA Security;
```

## <a name="see-also"></a>Voir aussi

 [CREATE SECURITY POLICY &#40;Transact-SQL&#41;](../../t-sql/statements/create-security-policy-transact-sql.md)</br>
 [ALTER SECURITY POLICY &#40;Transact-SQL&#41;](../../t-sql/statements/alter-security-policy-transact-sql.md)</br>
 [DROP SECURITY POLICY &#40;Transact-SQL&#41;](../../t-sql/statements/drop-security-policy-transact-sql.md)</br>
 [CREATE FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/create-function-transact-sql.md)</br>
 [SESSION_CONTEXT &#40;Transact-SQL&#41;](../../t-sql/functions/session-context-transact-sql.md)</br>
 [sp_set_session_context &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-set-session-context-transact-sql.md)</br>
 [sys.security_policies &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-security-policies-transact-sql.md)</br>
 [sys.security_predicates &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-security-predicates-transact-sql.md)</br>
 [Créer des fonctions définies par l’utilisateur &#40;moteur de base de données&#41;](../../relational-databases/user-defined-functions/create-user-defined-functions-database-engine.md)
