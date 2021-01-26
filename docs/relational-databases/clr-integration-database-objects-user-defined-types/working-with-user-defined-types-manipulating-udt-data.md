---
title: Manipulation de données UDT | Microsoft Docs
description: Cet article explique comment insérer, sélectionner et mettre à jour des données dans les colonnes UDT d’une base de données SQL Server.
ms.custom: ''
ms.date: 12/05/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: clr
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- CAST function
- data deletions [CLR integration]
- data insertions [CLR integration]
- CONVERT function
- data updates [CLR integration]
- comparing data
- updating data [CLR integration]
- removing data
- IsByteOrdered property
- variables [CLR integration]
- data manipulation [CLR integration]
- user-defined types [CLR integration], data manipulation
- deleting data
- UDTs [CLR integration], data manipulation
- invoking UDT methods
- inserting data
ms.assetid: 51b1a5f2-7591-4e11-bfe2-d88e0836403f
author: rothja
ms.author: jroth
ms.openlocfilehash: d23880a7ea6a1e8f4c1beccc5ec82f40303b9b76
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783627"
---
# <a name="working-with-user-defined-types---manipulating-udt-data"></a>Utilisation de types définis par l’utilisateur - Manipulation de données UDT
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  [!INCLUDE[tsql](../../includes/tsql-md.md)] ne fournit aucune syntaxe spécialisée pour les instructions INSERT, UPDATE ou DELETE lors de la modification de données dans des colonnes de type défini par l'utilisateur (UDT). Les fonctions [!INCLUDE[tsql](../../includes/tsql-md.md)] CAST ou CONVERT sont utilisées pour convertir des types de données natives en type UDT.  
  
## <a name="inserting-data-in-a-udt-column"></a>Insertion de données dans une colonne UDT  
 Les [!INCLUDE[tsql](../../includes/tsql-md.md)] instructions suivantes insèrent trois lignes d’exemples de données dans la table **points** . Le type de données **point** est constitué de valeurs entières X et Y qui sont exposées en tant que propriétés de l’UDT. Vous devez utiliser la fonction CAST ou CONVERT pour effectuer un cast des valeurs X et Y délimitées par des virgules en type **point** . Les deux premières instructions utilisent la fonction CONVERT pour convertir une valeur de chaîne en type **point** , et la troisième instruction utilise la fonction CAST :  
  
```sql  
INSERT INTO dbo.Points (PointValue) VALUES (CONVERT(Point, '3,4'));  
INSERT INTO dbo.Points (PointValue) VALUES (CONVERT(Point, '1,5'));  
INSERT INTO dbo.Points (PointValue) VALUES (CAST ('1,99' AS Point));  
```  
  
## <a name="selecting-data"></a>Sélection de données  
 L'instruction SELECT suivante sélectionne la valeur binaire du type UDT.  
  
```sql  
SELECT ID, PointValue FROM dbo.Points  
```  
  
 Pour afficher la sortie affichée dans un format lisible, appelez la méthode **ToString** de l’UDT **point** , qui convertit la valeur en sa représentation sous forme de chaîne.  
  
```sql  
SELECT ID, PointValue.ToString() AS PointValue   
FROM dbo.Points;  
```  
  
 Ce code produit les résultats suivants.  
  
```  
ID PointValue  
-- ----------  
 1 3,4  
 2 1,5  
 3 1,99  
```  
  
 Vous pouvez également utiliser les fonctions [!INCLUDE[tsql](../../includes/tsql-md.md)] CAST et CONVERT pour obtenir les mêmes résultats.  
  
```sql  
SELECT ID, CAST(PointValue AS varchar)   
FROM dbo.Points;  
  
SELECT ID, CONVERT(varchar, PointValue)   
FROM dbo.Points;  
```  
  
 L’UDT **point** expose ses coordonnées X et Y en tant que propriétés, que vous pouvez ensuite sélectionner individuellement. L'instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] suivante sélectionne les coordonnées X et Y séparément :  
  
```sql  
SELECT ID, PointValue.X AS xVal, PointValue.Y AS yVal   
FROM dbo.Points;  
```  
  
 Les propriétés X et Y retournent une valeur entière qui est affichée dans le jeu de résultats.  
  
```  
ID xVal yVal  
-- ---- ----  
 1    3    4  
 2    1    5  
 3    1   99  
```  
  
## <a name="working-with-variables"></a>Utilisation de variables  
 Vous pouvez utiliser des variables à l'aide de l'instruction DECLARE pour attribuer une variable à un type UDT. Les instructions suivantes affectent une valeur à l’aide de l' [!INCLUDE[tsql](../../includes/tsql-md.md)] instruction SET et affichent les résultats en appelant la méthode **ToString** de l’UDT sur la variable :  
  
```sql  
DECLARE @PointValue Point;  
SET @PointValue = (SELECT PointValue FROM dbo.Points  
    WHERE ID = 2);  
SELECT @PointValue.ToString() AS PointValue;  
```  
  
 Le jeu de résultats affiche la valeur de la variable :  
  
```  
PointValue  
----------  
-1,5  
```  
  
 Les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] suivantes génèrent le même résultat en remplaçant SET par SELECT pour l'attribution de la variable :  
  
```sql  
DECLARE @PointValue Point;  
SELECT @PointValue = PointValue FROM dbo.Points  
    WHERE ID = 2;  
SELECT @PointValue.ToString() AS PointValue;  
```  
  
 La différence entre SELECT et SET pour l'attribution de la variable réside dans le fait que SELECT vous permet d'attribuer plusieurs variables dans une instruction SELECT, tandis que la syntaxe SET nécessite que chaque attribution de variable possède sa propre instruction SET.  
  
## <a name="comparing-data"></a>Comparaison de données  
 Vous pouvez utiliser des opérateurs de comparaison pour comparer des valeurs dans votre UDT si vous avez affecté la **valeur true** à la propriété **IsByteOrdered** lors de la définition de la classe. Pour plus d’informations, consultez [création d’un type de User-Defined](../../relational-databases/clr-integration-database-objects-user-defined-types/creating-user-defined-types.md).  
  
```sql  
SELECT ID, PointValue.ToString() AS Points   
FROM dbo.Points  
WHERE PointValue > CONVERT(Point, '2,2');  
```  
  
 Vous pouvez comparer les valeurs internes de l’UDT indépendamment du paramètre **IsByteOrdered** si les valeurs elles-mêmes sont comparables. L'instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] suivante sélectionne des lignes où X est supérieur à Y :  
  
```sql  
SELECT ID, PointValue.ToString() AS PointValue   
FROM dbo.Points  
WHERE PointValue.X < PointValue.Y;  
```  
  
 Vous pouvez également utiliser des opérateurs de comparaison avec des variables, comme indiqué dans cette requête que recherche un PointValue correspondant.  
  
```sql  
DECLARE @ComparePoint Point;  
SET @ComparePoint = CONVERT(Point, '3,4');  
SELECT ID, PointValue.ToString() AS MatchingPoint   
FROM dbo.Points  
WHERE PointValue = @ComparePoint;  
```  
  
## <a name="invoking-udt-methods"></a>Appel de méthodes UDT  
 Vous pouvez également appeler des méthodes qui sont définies dans votre type UDT dans [!INCLUDE[tsql](../../includes/tsql-md.md)]. La classe **point** contient trois méthodes : **distance**, **DistanceFrom** et **DistanceFromXY**. Pour obtenir les listes de code qui définissent ces trois méthodes, consultez [codage des types de User-Defined](../../relational-databases/clr-integration-database-objects-user-defined-types/creating-user-defined-types-coding.md).  
  
 L' [!INCLUDE[tsql](../../includes/tsql-md.md)] instruction suivante appelle la méthode **PointValue. distance** :  
  
```sql  
SELECT ID, PointValue.X AS [Point.X],   
    PointValue.Y AS [Point.Y],  
    PointValue.Distance() AS DistanceFromZero   
FROM dbo.Points;  
```  
  
 Les résultats s’affichent dans la colonne **distance** :  
  
```  
ID X  Y  Distance  
-- -- -- ----------------  
 1  3  4                5  
 2  1  5 5.09901951359278  
 3  1 99 99.0050503762308  
```  
  
 La méthode **DistanceFrom** accepte un argument de type **point** et affiche la distance entre le point spécifié et le PointValue :  
  
```sql  
SELECT ID, PointValue.ToString() AS Pnt,  
   PointValue.DistanceFrom(CONVERT(Point, '1,99')) AS DistanceFromPoint  
FROM dbo.Points;  
```  
  
 Les résultats affichent les résultats de la méthode **DistanceFrom** pour chaque ligne de la table :  
  
```  
ID Pnt DistanceFromPoint  
-- --- -----------------  
 1 3,4  95.0210502993942  
 2 1,5                94  
 3 1,9                90  
```  
  
 La méthode **DistanceFromXY** prend les points individuellement comme arguments :  
  
```sql  
SELECT ID, PointValue.X as X, PointValue.Y as Y,   
PointValue.DistanceFromXY(1, 99) AS DistanceFromXY   
FROM dbo.Points  
```  
  
 Le jeu de résultats est le même que la méthode **DistanceFrom** .  
  
## <a name="updating-data-in-a-udt-column"></a>Mise à jour de données dans une colonne UDT  
 Pour mettre à jour des données dans une colonne UDT, utilisez l'instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] UPDATE. Vous pouvez également utiliser une méthode du type UDT pour mettre à jour l'état de l'objet. L'instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] suivante met à jour une ligne unique dans la table :  
  
```sql  
UPDATE dbo.Points  
SET PointValue = CAST('1,88' AS Point)  
WHERE ID = 3  
```  
  
 Vous pouvez également mettre à jour des éléments UDT séparément. L'instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] suivante met à jour uniquement la coordonnée Y :  
  
```sql  
UPDATE dbo.Points  
SET PointValue.Y = 99  
WHERE ID = 3  
```  
  
 Si le type défini par l’utilisateur a été défini avec la valeur **true** pour l’ordonnancement des octets, [!INCLUDE[tsql](../../includes/tsql-md.md)] peut évaluer la colonne UDT dans une clause WHERE.  
  
```sql  
UPDATE dbo.Points  
SET PointValue = '4,5'  
WHERE PointValue = '3,4';  
```  
  
### <a name="updating-limitations"></a>Mise à jour de limitations  
 Vous ne pouvez pas mettre à jour plusieurs propriétés à la fois à l'aide de [!INCLUDE[tsql](../../includes/tsql-md.md)]. Par exemple, l’instruction UPDATE suivante échoue avec une erreur, car vous ne pouvez pas utiliser le même nom de colonne à deux reprises dans une instruction UPDATE.  
  
```sql  
UPDATE dbo.Points  
SET PointValue.X = 5, PointValue.Y = 99  
WHERE ID = 3  
```  
  
 Pour mettre à jour chaque point individuellement, vous devez créer une méthode mutateur dans l'assembly UDT Point. Vous pouvez ensuite appeler la méthode mutateur pour mettre à jour l'objet dans une instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] UPDATE, comme suit :  
  
```sql  
UPDATE dbo.Points  
SET PointValue.SetXY(5, 99)  
WHERE ID = 3  
```  
  
## <a name="deleting-data-in-a-udt-column"></a>Suppression de données dans une colonne UDT  
 Pour supprimer des données dans un type UDT, utilisez l'instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] DELETE. L'instruction suivante supprime toutes les lignes dans la table qui correspondent aux critères spécifiés dans la clause WHERE. Si vous omettez la clause WHERE dans une instruction DELETE, toutes les lignes dans la table seront supprimées.  
  
```sql  
DELETE FROM dbo.Points  
WHERE PointValue = CAST('1,99' AS Point)  
```  
  
 Utilisez l'instruction UPDATE si vous souhaitez supprimer les valeurs dans une colonne UDT tout en laissant d'autres valeurs de lignes intactes. Cet exemple attribut la valeur null à PointValue.  
  
```sql  
UPDATE dbo.Points  
SET PointValue = null  
WHERE ID = 2  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Utilisation de types définis par l’utilisateur dans SQL Server](../../relational-databases/clr-integration-database-objects-user-defined-types/working-with-user-defined-types-in-sql-server.md)  
  
  
