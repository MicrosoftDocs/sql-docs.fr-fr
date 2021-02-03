---
description: Exemples SELECT (Transact-SQL)
title: Exemples SELECT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- parentheses [SQL Server]
- GROUP BY clause, SELECT statement
- query hints [SQL Server]
- ALL keyword
- ROLLUP operator
- SELECT statement [SQL Server], examples
- correlated subqueries, SELECT statement
- SELECT INTO statement
- ORDER BY clause [Transact-SQL]
- GROUPING function
- index hints [SQL Server]
- HAVING clause, SELECT statement
- DISTINCT keyword
- CUBE operator
- UNION operator [SQL Server]
- computed sums
- WHERE clause, SELECT statement
ms.assetid: 9b9caa3d-e7d0-42e1-b60b-a5572142186c
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: e2e12a155eb1a3c710b3289912f9e9e12f3642ed
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207591"
---
# <a name="select-examples-transact-sql"></a>Exemples SELECT (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Cette rubrique fournit des exemples d’utilisation de l’instruction [SELECT](../../t-sql/queries/select-transact-sql.md).  
  
## <a name="a-using-select-to-retrieve-rows-and-columns"></a>R. Utilisation de SELECT pour extraire des lignes et des colonnes  
 L'exemple suivant présente trois exemples de code. Le premier exemple de code retourne toutes les lignes (aucune clause WHERE n'est définie) et toutes les colonnes (en utilisant `*`) de la table `Product` dans la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
 [!code-sql[Select#SelectExamples1](../../t-sql/queries/codesnippet/tsql/select-examples-transact_1.sql)]  
  
 Cet exemple retourne toutes les lignes (aucune clause WHERE n'est définie) et uniquement un sous-ensemble des colonnes (`Name`, `ProductNumber`, `ListPrice`) de la table `Product` dans la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]. En outre, un en-tête de colonne est ajouté.  
  
 [!code-sql[Select#SelectExamples2](../../t-sql/queries/codesnippet/tsql/select-examples-transact_2.sql)]  
  
 Cet exemple retourne uniquement les lignes de `Product` dont la ligne de produits a pour valeur `R` et dont le nombre de jours nécessaires à la fabrication est inférieur à `4`.  
  
 [!code-sql[Select#SelectExamples3](../../t-sql/queries/codesnippet/tsql/select-examples-transact_3.sql)]  
  
## <a name="b-using-select-with-column-headings-and-calculations"></a>B. Utilisation de SELECT avec des en-têtes de colonne et des calculs  
 Les exemples suivants retournent toutes les lignes de la table `Product`. Le premier exemple retourne le total des ventes et les remises pour chaque produit. Dans le second exemple, le gain est calculé pour chaque produit.  
  
 [!code-sql[Select#SelectExamples4](../../t-sql/queries/codesnippet/tsql/select-examples-transact_4.sql)]  
  
 Voici la requête qui calcule le gain pour chaque produit dans chaque commande.  
  
 [!code-sql[Select#SelectExamples5](../../t-sql/queries/codesnippet/tsql/select-examples-transact_5.sql)]  
  
## <a name="c-using-distinct-with-select"></a>C. Utilisation de DISTINCT avec SELECT  
 L'exemple suivant utilise `DISTINCT` afin d'éviter l'extraction de titres en double.  
  
 [!code-sql[Select#SelectExamples6](../../t-sql/queries/codesnippet/tsql/select-examples-transact_6.sql)]  
  
## <a name="d-creating-tables-with-select-into"></a>D. Création de tables avec SELECT INTO  
 Le premier exemple suivant crée une table temporaire nommée `#Bicycles` dans `tempdb`.  
  
 [!code-sql[Select#SelectExamples7](../../t-sql/queries/codesnippet/tsql/select-examples-transact_7.sql)]  
  
 Ce second exemple crée la table permanente `NewProducts`.  
  
 [!code-sql[Select#SelectExamples8](../../t-sql/queries/codesnippet/tsql/select-examples-transact_8.sql)]  
  
## <a name="e-using-correlated-subqueries"></a>E. Utilisation de sous-requêtes corrélées
Une sous-requête corrélée est une requête dont les valeurs dépendent de la requête externe. Cette requête peut être exécutée de manière répétitive, une fois pour chaque ligne que la requête externe pourrait sélectionner.

Le premier exemple montre des requêtes sémantiquement équivalentes pour illustrer la différence entre l’utilisation du mot clé `EXISTS` et celle du mot clé `IN`. Les deux exemples illustrent une sous-requête valide qui extrait une instance de chaque nom de produit dont le modèle est un pull-over à manches longues (« long sleeve logo jersey »), et dont le numéro `ProductModelID` se trouve dans les tables `Product` et `ProductModel`.  
  
 [!code-sql[Select#SelectExamples9](../../t-sql/queries/codesnippet/tsql/select-examples-transact_9.sql)]  
  
 L’exemple suivant utilise `IN` et extrait une instance du prénom et du nom de chaque employé dont la prime dans la table `SalesPerson` est égale à `5000.00` et dont le numéro d’identification d’employé se trouve dans les tables `Employee` et `SalesPerson`.  
  
 [!code-sql[Select#SelectExamples10](../../t-sql/queries/codesnippet/tsql/select-examples-transact_10.sql)]  
  
 La sous-requête précédente de cette instruction ne peut pas être évaluée indépendamment de la requête externe. Elle requiert en effet une valeur pour `Employee.EmployeeID`, mais cette valeur change à mesure que le moteur de base de données [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] examine les différentes lignes de la table `Employee`.  
  
 Une sous-requête en corrélation peut également s'employer dans la clause `HAVING` d'une requête externe. Cet exemple recherche les modèles de produit dont le tarif maximum est supérieur au double du tarif moyen du modèle concerné.  
  
 [!code-sql[Select#SelectExamples11](../../t-sql/queries/codesnippet/tsql/select-examples-transact_11.sql)]  
  
 Cet exemple utilise deux sous-requêtes corrélées pour rechercher les noms des employés ayant vendu un produit spécifique.  
  
 [!code-sql[Select#SelectExamples12](../../t-sql/queries/codesnippet/tsql/select-examples-transact_12.sql)]  
  
## <a name="f-using-group-by"></a>F. Utilisation de GROUP BY  
 L'exemple suivant calcule le total de chaque commande dans la base de données.  
  
 [!code-sql[Select#SelectExamples13](../../t-sql/queries/codesnippet/tsql/select-examples-transact_13.sql)]  
  
 Du fait de la clause `GROUP BY`, une seule ligne est retournée pour chaque commande et elle contient la somme de toutes les ventes de cette dernière.  
  
## <a name="g-using-group-by-with-multiple-groups"></a>G. Utilisation de GROUP BY avec plusieurs groupes  
 L'exemple suivant affiche le prix moyen et la somme des ventes annuelles cumulées, regroupés par ID de produit et ID d'offre spéciale :  
  
 [!code-sql[Select#SelectExamples14](../../t-sql/queries/codesnippet/tsql/select-examples-transact_14.sql)]  
  
## <a name="h-using-group-by-and-where"></a>H. Utilisation de GROUP BY et WHERE  
 L'exemple suivant présente les résultats en groupes après n'avoir extrait que les lignes avec des tarifs supérieurs à `$1000`.  
  
 [!code-sql[Select#SelectExamples15](../../t-sql/queries/codesnippet/tsql/select-examples-transact_15.sql)]  
  
## <a name="i-using-group-by-with-an-expression"></a>I. Utilisation de GROUP BY avec une expression  
 L'exemple suivant effectue un regroupement en fonction d'une expression. Vous pouvez spécifier un regroupement en fonction d'une expression à condition qu'elle ne contienne pas de fonction d'agrégation.  
  
 [!code-sql[Select#SelectExamples16](../../t-sql/queries/codesnippet/tsql/select-examples-transact_16.sql)]  
  
## <a name="j-using-group-by-with-order-by"></a>J. Utilisation de GROUP BY avec ORDER BY  
 L'exemple suivant calcule le prix moyen de chaque type de produit et trie les résultats en conséquence :  
  
 [!code-sql[Select#SelectExamples18](../../t-sql/queries/codesnippet/tsql/select-examples-transact_17.sql)]  
  
## <a name="k-using-the-having-clause"></a>K. Utilisation de la clause HAVING  
 Le premier exemple suivant illustre une clause `HAVING` utilisée avec une fonction d'agrégation. Il regroupe les lignes de la table `SalesOrderDetail` par ID de produit et élimine les produits dont la quantité moyenne d’articles commandés est inférieure ou égale à cinq. Le second exemple illustre une clause `HAVING` sans fonction d'agrégation.  
  
 [!code-sql[Select#SelectExamples19](../../t-sql/queries/codesnippet/tsql/select-examples-transact_18.sql)]  
  
 La requête suivante utilise la clause `LIKE` dans la clause `HAVING`.  
  
```sql
USE AdventureWorks2012 ;  
GO  
SELECT SalesOrderID, CarrierTrackingNumber   
FROM Sales.SalesOrderDetail  
GROUP BY SalesOrderID, CarrierTrackingNumber  
HAVING CarrierTrackingNumber LIKE '4BD%'  
ORDER BY SalesOrderID ;  
GO  
```  
  
## <a name="l-using-having-and-group-by"></a>L. Utilisation de HAVING et de GROUP BY  
 L'exemple suivant illustre l'utilisation des clauses `GROUP BY`, `HAVING`, `WHERE` et `ORDER BY` dans une instruction `SELECT`. Il génère des groupes et des valeurs résumées mais uniquement après avoir éliminé les produits dont le prix est supérieur à 25 $ et dont la quantité moyenne commandée est inférieure à 5. Il trie également les résultats par `ProductID`.  
  
 [!code-sql[Select#SelectExamples21](../../t-sql/queries/codesnippet/tsql/select-examples-transact_19.sql)]  
  
## <a name="m-using-having-with-sum-and-avg"></a>M. Utilisation de HAVING avec SUM et AVG  
 L'exemple suivant regroupe la table `SalesOrderDetail` par ID de produit et ne comprend que les groupes de produits dont les commandes s'élèvent à plus de `$1000000.00` et dont la quantité moyenne d'articles achetés est inférieure à `3`.  
  
 [!code-sql[Select#SelectExamples22](../../t-sql/queries/codesnippet/tsql/select-examples-transact_20.sql)]  
  
 Pour visualiser les produits dont le total des ventes est supérieur à `$2000000.00`, utilisez la requête suivante :  
  
 [!code-sql[Select#SelectExamples23](../../t-sql/queries/codesnippet/tsql/select-examples-transact_21.sql)]  
  
 Si vous souhaitez vous assurer qu'au moins mille cinq cents articles sont impliqués dans le calcul de chaque produit, utilisez l'instruction `HAVING COUNT(*) > 1500` pour éliminer les produits dont le total renvoyé correspond à moins de `1500` articles vendus. La requête ressemble à ceci :  
  
 [!code-sql[Select#SelectExamples24](../../t-sql/queries/codesnippet/tsql/select-examples-transact_22.sql)]  
  
## <a name="n-using-the-index-optimizer-hint"></a>N. Utilisation de l'indicateur d'optimiseur INDEX  
 L'exemple suivant illustre deux manières d'utiliser l'indicateur d'optimiseur `INDEX`. Le premier exemple illustre la façon d'obliger l'optimiseur à utiliser un index non cluster pour extraire des lignes d'une table, tandis que le second exemple génère l'analyse d'une table à l'aide d'un index 0.  
  
 [!code-sql[Select#SelectExamples45](../../t-sql/queries/codesnippet/tsql/select-examples-transact_23.sql)]  
  
## <a name="m-using-option-and-the-group-hints"></a>M. Utilisation de la clause OPTION avec les indicateurs GROUP  
 L'exemple suivant montre comment la clause `OPTION (GROUP)` est utilisée avec une clause `GROUP BY`.  
  
 [!code-sql[Select#SelectExamples46](../../t-sql/queries/codesnippet/tsql/select-examples-transact_24.sql)]  
  
## <a name="o-using-the-union-query-hint"></a>O. Utilisation de l'indicateur de requête UNION  
 L'exemple suivant utilise l'indicateur de requête `MERGE UNION`.  
  
 [!code-sql[Select#SelectExamples47](../../t-sql/queries/codesnippet/tsql/select-examples-transact_25.sql)]  
  
## <a name="p-using-a-simple-union"></a>P. Utilisation de l'opérateur UNION simple  
 Dans l'exemple suivant, le jeu de résultats comprend le contenu des colonnes `ProductModelID` et `Name` des deux tables `ProductModel` et `Gloves`.  
  
 [!code-sql[Select#SelectExamples48](../../t-sql/queries/codesnippet/tsql/select-examples-transact_26.sql)]  
  
## <a name="q-using-select-into-with-union"></a>Q. Utilisation de SELECT INTO avec UNION  
 Dans l'exemple suivant, la clause `INTO` de la seconde instruction `SELECT` indique que la table nommée `ProductResults` contient le jeu de résultats final de l'union des colonnes désignées des tables `ProductModel` et `Gloves`. Notez que la table `Gloves` est créée dans la première instruction `SELECT`.  
  
 [!code-sql[Select#SelectExamples49](../../t-sql/queries/codesnippet/tsql/select-examples-transact_27.sql)]  
  
## <a name="r-using-union-of-two-select-statements-with-order-by"></a>R. Utilisation de l'opérateur UNION dans deux instructions SELECT avec ORDER BY  
 L'ordre de certains paramètres utilisés avec la clause UNION est important. L'exemple suivant illustre l'utilisation incorrecte et correcte de `UNION` dans deux instructions `SELECT` où une colonne doit être renommée dans le résultat.  
  
 [!code-sql[Select#SelectExamples50](../../t-sql/queries/codesnippet/tsql/select-examples-transact_28.sql)]  
  
## <a name="s-using-union-of-three-select-statements-to-show-the-effects-of-all-and-parentheses"></a>S. Utilisation de l'opérateur UNION dans trois instructions SELECT pour illustrer les effets de ALL et des parenthèses  
 Les exemples suivants utilisent `UNION` pour combiner les résultats de trois tables, ayant chacune les 5 lignes de données identiques. Le premier exemple utilise `UNION ALL` pour montrer les doublons d'enregistrement et retourne l'ensemble des 15 lignes. Le deuxième exemple utilise `UNION` sans `ALL` pour éliminer les doublons de ligne des résultats combinés des trois instructions `SELECT` et retourne 5 lignes.  
  
 Le troisième exemple utilise `ALL` avec la première clause `UNION` et met entre parenthèses la seconde clause `UNION` qui n'utilise pas `ALL`. La seconde clause `UNION` est traitée en premier, car elle est entre parenthèses. Elle retourne 5 lignes car l'option `ALL` n'est pas utilisée et les doublons sont supprimés. Ces 5 lignes sont combinées avec les résultats de la première instruction `SELECT` à l'aide des mots clés `UNION ALL`. Cela ne supprime pas les doublons entre les deux ensembles de 5 lignes. Le résultat final contient 10 lignes.  
  
 [!code-sql[Select#SelectExamples51](../../t-sql/queries/codesnippet/tsql/select-examples-transact_29.sql)]  
  
## <a name="see-also"></a>Voir aussi  
 [CREATE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/create-trigger-transact-sql.md)   
 [CREATE VIEW &#40;Transact-SQL&#41;](../../t-sql/statements/create-view-transact-sql.md)   
 [DELETE &#40;Transact-SQL&#41;](../../t-sql/statements/delete-transact-sql.md)   
 [EXECUTE &#40;Transact-SQL&#41;](../../t-sql/language-elements/execute-transact-sql.md)   
 [Expressions &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)   
 [INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md)   
 [LIKE &#40;Transact-SQL&#41;](../../t-sql/language-elements/like-transact-sql.md)   
 [UNION &#40;Transact-SQL&#41;](../../t-sql/language-elements/set-operators-union-transact-sql.md)   
 [EXCEPT et INTERSECT &#40;Transact-SQL&#41;](../../t-sql/language-elements/set-operators-except-and-intersect-transact-sql.md)   
 [UPDATE &#40;Transact-SQL&#41;](../../t-sql/queries/update-transact-sql.md)   
 [WHERE &#40;Transact-SQL&#41;](../../t-sql/queries/where-transact-sql.md)   
 [PathName &#40;Transact-SQL&#41;](../../relational-databases/system-functions/pathname-transact-sql.md)   
 [INTO, clause &#40;Transact-SQL&#41;](../../t-sql/queries/select-into-clause-transact-sql.md)  
  
  
