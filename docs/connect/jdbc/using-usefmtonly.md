---
description: Récupération de ParameterMetaData via useFmtOnly
title: Récupération de ParameterMetaData via useFmtOnly | Microsoft Docs
ms.custom: ''
ms.date: 08/12/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: v-daenge
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: ''
caps.latest.revision: ''
author: rene-ye
ms.author: v-reye
manager: kenvh
ms.openlocfilehash: a88707cc4679343c2e29a86ecc3a9efb8a9719dd
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837449"
---
# <a name="retrieving-parametermetadata-via-usefmtonly"></a>Récupération de ParameterMetaData via useFmtOnly
[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

  Le pilote Microsoft JDBC pour SQL Server propose un autre moyen d’interroger les métadonnées des paramètres à partir du serveur, **useFmtOnly**. Cette fonctionnalité a été introduite pour la première fois dans la version 7.4 du pilote, et est requise comme solution de contournement pour les problèmes connus dans `sp_describe_undeclared_parameters`.
  
  Le pilote utilise principalement la procédure stockée `sp_describe_undeclared_parameters` pour interroger les métadonnées de paramètre, car il s’agit de l’approche recommandée pour la récupération des métadonnées de paramètre dans la plupart des cas. Toutefois, l’exécution de la procédure stockée échoue actuellement dans les cas d’utilisation suivants :
  
-   Par rapport à des colonnes Always Encrypted
  
-   Par rapport aux tables temporaires et aux variables de table
  
-   Par rapport aux vues 
  
  La solution proposée pour ces cas d’utilisation consiste à analyser la requête SQL de l’utilisateur pour les paramètres et les cibles de la table, puis à exécuter une requête `SELECT` avec `FMTONLY` activé. L’extrait de code suivant permet de visualiser la fonctionnalité.
  
```sql
--create a normal table 'Foo' and a temporary table 'Bar'
CREATE TABLE Foo(c1 int);
CREATE TABLE #Bar(c1 int);

EXEC sp_describe_undeclared_parameters N'SELECT * FROM Foo WHERE c1 = @p0' --works fine
EXEC sp_describe_undeclared_parameters N'SELECT * FROM #Bar WHERE c1 = @p0' --fails with "Invalid object name '#Bar'"

SET FMTONLY ON;
SELECT c1 FROM Foo; --works
SET FMTONLY OFF;
SET FMTONLY ON;
SELECT c1 FROM #Bar; --works
SET FMTONLY OFF;
```
 
## <a name="turning-the-feature-onoff"></a>Activation/désactivation de la fonctionnalité 
 La fonctionnalité **useFmtOnly** est désactivée par défaut. Les utilisateurs peuvent activer cette fonctionnalité par le biais de la chaîne de connexion en spécifiant `useFmtOnly=true`. Par exemple : `jdbc:sqlserver://<server>:<port>;databaseName=<databaseName>;user=<user>;password=<password>;useFmtOnly=true;`.
 
 La fonctionnalité est également disponible via `SQLServerDataSource`.
 ```java
SQLServerDataSource ds = new SQLServerDataSource();
ds.setServerName(<server>);
ds.setPortNumber(<port>);
ds.setDatabaseName("<databaseName>");
ds.setUser("<user>");
ds.setPassword("<password>");
ds.setUseFmtOnly(true);
try (Connection c = ds.getConnection()) {
    // do work with connection
}
 ```
 
 La fonctionnalité est aussi disponible au niveau de l’instruction. Les utilisateurs peuvent activer ou désactiver la fonctionnalité par le biais de `PreparedStatement.setUseFmtOnly(boolean)`.
> [!NOTE]  
>  Le pilote donne la priorité à la propriété de niveau instruction par rapport à celle de niveau connexion.

## <a name="using-the-feature"></a>Utilisation de la fonctionnalité
  Une fois activé, le pilote commence à utiliser la nouvelle fonctionnalité en interne au lieu de `sp_describe_undeclared_parameters` lors de l’interrogation des métadonnées de paramètre. Aucune action supplémentaire n’est nécessaire de la part de l’utilisateur final.
```java
final String sql = "INSERT INTO #Bar VALUES (?)";
try (Connection c = DriverManager.getConnection(URL, USERNAME, PASSWORD)) {
    try (Statement s = c.createStatement()) {
        s.execute("CREATE TABLE #Bar(c1 int)");
    }
    try (PreparedStatement p1 = c.prepareStatement(sql); PreparedStatement p2 = c.prepareStatement(sql)) {
        ((SQLServerPreparedStatement) p1).setUseFmtOnly(true);
        ParameterMetaData pmd1 = p1.getParameterMetaData();
        System.out.println(pmd1.getParameterTypeName(1)); // prints int
        ParameterMetaData pmd2 = p2.getParameterMetaData(); // throws exception, Invalid object name '#Bar'
    }
}
```
> [!NOTE]  
>  La fonctionnalité prend uniquement en charge les requêtes `SELECT/INSERT/UPDATE/DELETE`. Les requêtes doivent commencer par l’un des 4 mots clés pris en charge ou une [expression de table commune](../../t-sql/queries/with-common-table-expression-transact-sql.md) suivie de l’une des requêtes prises en charge. Les paramètres dans les expressions de table communes ne sont pas pris en charge.

## <a name="known-issues"></a>Problèmes connus
  Il existe actuellement des problèmes avec la fonctionnalité, qui sont dus à des lacunes dans la logique d’analyse SQL. Ces problèmes pourraient être résolus dans une prochaine mise à jour de la fonctionnalité et sont décrits ci-dessous, avec des suggestions de solutions de contournement.
  
R. Utilisation d’un alias ’forward declared'
```sql
CREATE TABLE Foo(c1 int)

DELETE fooAlias FROM Foo fooAlias WHERE c1 > ?; --Invalid object name 'fooAlias'

--Workaround #1: Specify AS keyword
DELETE fooAlias FROM Foo AS fooAlias WHERE c1 > ?;
--Workaround #2: Use the table name
DELETE Foo FROM Foo fooAlias WHERE c1 > ?;
```

B. Nom de colonne ambigu lorsque les tables ont des noms de colonnes partagés
```sql
CREATE TABLE Foo(c1 int, c2 int, c3 int)
CREATE TABLE Bar(c1 int, c2 int, c3 int)

SELECT c1,c2 FROM Foo WHERE c3 IN (SELECT c3 FROM Bar WHERE c1 > ? and c2 < ? and c3 = ?); --Ambiguous Column Name

--Workaround: Use aliases
SELECT c1,c2 FROM Foo WHERE c3 IN (SELECT c3 FROM Bar b WHERE b.c1 = ? and b.c2 = ? and b.c3 = ?);
```

C. SELECT à partir d’une sous-requête avec des paramètres
```sql

CREATE TABLE Foo(c1 int)

SELECT * FROM (SELECT * FROM Foo WHERE c1 = ?) WHERE c1 = ?; --Incorrect syntax near '?'

--Workaround: N/A
```

D. Sous-requêtes dans une clause SET
```sql
CREATE TABLE Foo(c1 int)

UPDATE Foo SET c1 = (SELECT c1 FROM Foo) WHERE c1 = ?; --Incorrect syntax near ')'

--Workaround: Add a 'delimiting' condition
UPDATE Foo SET c1 = (SELECT c1 FROM Foo HAVING (HASH JOIN)) WHERE c1 = ?;
```

## <a name="see-also"></a>Voir aussi  
 [Définition des propriétés de connexion](../../connect/jdbc/setting-the-connection-properties.md)  
  
