---
description: Clause SELECT (Transact-SQL)
title: SELECT, clause (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SELECT Clause
- SELECT_Clause_TSQL
- DISTINCT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- parentheses [SQL Server]
- identity columns [SQL Server], SELECT clause
- SELECT clause
- $IDENTITY keyword
- user-defined types [SQL Server], invoking methods and properties
- SELECT statement [SQL Server], processing orders
- clauses [SQL Server], SELECT
- $ROWGUID keyword
- queries [SQL Server], results
ms.assetid: 2616d800-4853-4cf1-af77-d32d68d8c2ef
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 2a633f4cfb8a0c9e5158b24c89035c5e97588a46
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207593"
---
# <a name="select-clause-transact-sql"></a>Clause SELECT (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Spécifie les colonnes à retourner par la requête.  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
SELECT [ ALL | DISTINCT ]  
[ TOP ( expression ) [ PERCENT ] [ WITH TIES ] ]   
<select_list>   
<select_list> ::=   
    {   
      *   
      | { table_name | view_name | table_alias }.*   
      | {  
          [ { table_name | view_name | table_alias }. ]  
               { column_name | $IDENTITY | $ROWGUID }   
          | udt_column_name [ { . | :: } { { property_name | field_name }   
            | method_name ( argument [ ,...n] ) } ]  
          | expression  
          [ [ AS ] column_alias ]   
         }  
      | column_alias = expression   
    } [ ,...n ]   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 **ALL**  
 Indique que les doublons de lignes peuvent apparaître dans le jeu de résultats. ALL est l'argument par défaut.  
  
 DISTINCT  
 Indique que seules des lignes uniques peuvent apparaître dans le jeu de résultats. Les valeurs nulles sont considérées comme étant égales pour le mot clé DISTINCT.  
  
 TOP (*expression* ) [ PERCENT ] [ WITH TIES ]  
 Indique que seul un premier ensemble ou pourcentage de lignes spécifié sera retourné à partir du jeu de résultats de la requête. L'argument *expression* peut être un nombre ou un pourcentage de lignes.  
  
 Pour la compatibilité descendante, l’utilisation de *l’expression* TOP sans parenthèses dans les instructions SELECT est prise en charge, mais déconseillée. Pour plus d’informations, consultez [TOP &#40;Transact-SQL&#41;](../../t-sql/queries/top-transact-sql.md).  
  
\< select_list > Colonnes à sélectionner pour le jeu de résultats. La liste de sélection est une série d'expressions séparées par des virgules. Le nombre maximum d'expressions pouvant être spécifiées dans la liste de sélection est de 4096.  
  
 \*  
 Indique que toutes les colonnes de toutes les tables et vues de la clause FROM doivent être renvoyées. Les colonnes sont renvoyées par table ou vue, comme indiqué dans la clause FROM, et par ordre d'apparition dans la table ou la vue.  
  
 *table_name* | *view_name* | *table* _ *alias*.*  
 Limite l’étendue du \* à la table ou vue spécifiée.  
  
 *column_name*  
 Nom d'une colonne à renvoyer. Spécifiez *column_name* pour éviter une référence ambiguë, notamment quand deux tables dans la clause FROM ont des noms de colonnes en double. Par exemple, les tables SalesOrderHeader et SalesOrderDetail de la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] possèdent toutes les deux une colonne nommée ModifiedDate. Si les deux tables sont associées dans une requête, la date de modification des entrées SalesOrderDetail peut être spécifiée dans la liste de sélection en tant que SalesOrderDetail.ModifiedDate.  
  
 *expression*  
 Constante, fonction ou toute combinaison de noms de colonne, de constantes et de fonctions reliées par un ou plusieurs opérateurs, ou par une sous-requête.  
  
 $IDENTITY  
 Renvoie la colonne d'identité. Pour plus d’informations, consultez [IDENTITY &#40;propriété&#41; &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql-identity-property.md), [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md) et [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md).  
  
 Si plusieurs tables de la clause FROM possèdent une colonne dotée de la propriété IDENTITY, $IDENTITY doit être qualifié avec le nom de table approprié, tel que T1.$IDENTITY.  
  
 $ROWGUID  
 Retourne la colonne GUID de la ligne.  
  
 Si plusieurs tables de la clause FROM sont dotées de la propriété ROWGUIDCOL, $ROWGUID doit être qualifié avec le nom de table approprié, tel que T1.$ROWGUID.  
  
 *udt_column_name*  
 Nom d'une colonne de type CLR (Common Language Runtime) définie par l'utilisateur à retourner.  
  
> [!NOTE]  
>  [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] renvoie des valeurs d'un type défini par l'utilisateur en représentation binaire. Pour retourner des valeurs d’un type défini par l’utilisateur au format chaîne ou XML, utilisez [CAST](../../t-sql/functions/cast-and-convert-transact-sql.md) ou [CONVERT](../../t-sql/functions/cast-and-convert-transact-sql.md).  
  
 { . | :: }  
 Spécifie une méthode, une propriété ou un champ d'un type CLR défini par l'utilisateur. Utilisez . pour une méthode, une propriété ou un champ d'instance (non statique). Utilisez :: pour une méthode, une propriété ou un champ statique. Pour appeler une méthode, une propriété ou un champ de type CLR défini par l'utilisateur, vous devez avoir l'autorisation EXECUTE sur le type.  
  
 *property_name*  
 Propriété publique de *udt_column_name*.  
  
 *field_name*  
 Membre de données public de *udt_column_name*.  
  
 *method_name*  
 Méthode publique de *udt_column_name* qui prend un ou plusieurs arguments. *method_name* ne peut pas être une méthode mutateur.  
  
 L'exemple ci-dessous sélectionne les valeurs pour la colonne `Location`, défini comme étant de type `point`, à partir de la table `Cities`, en invoquant une méthode du type appelé `Distance` :  
  
```sql
CREATE TABLE dbo.Cities (  
     Name VARCHAR(20),  
     State VARCHAR(20),  
     Location POINT);  
GO  
DECLARE @p POINT (32, 23), @distance FLOAT;  
GO  
SELECT Location.Distance (@p)  
FROM Cities;  
```  
  
 *column_alias*  
 Nom utilisé pour remplacer le nom de colonne dans le jeu de résultats de la requête. Par exemple, vous pouvez spécifier un alias tel que Quantité, Quantité à la date du jour ou Qté pour une colonne appelée quantité.  
  
 Les alias sont également utilisés pour spécifier les noms des résultats d'expressions, par exemple :  
  
 ```sql
 USE AdventureWorks2012;  
 GO  
 SELECT AVG(UnitPrice) AS [Average Price]  
 FROM Sales.SalesOrderDetail;
 ```  
  
 *column_alias* peut être utilisé dans une clause ORDER BY. Il ne peut toutefois pas être utilisé dans une clause WHERE, GROUP BY ou HAVING. Si l’expression de requête fait partie d’une instruction DECLARE CURSOR, *column_alias* ne peut pas être utilisé dans la clause FOR UPDATE.  
  
## <a name="remarks"></a>Notes  
 La longueur des données retournées pour les colonnes **text** ou **ntext** incluses dans la liste de sélection est égale à la plus petite des valeurs suivantes : la taille réelle de la colonne **text**, le paramètre de session TEXTSIZE par défaut ou la limite codée en dur dans l’application. Pour modifier la longueur du texte renvoyé dans une session, utilisez l'instruction SET. Par défaut, la limite de longueur pour des données textuelles renvoyées avec une instruction SELECT est de 4 Ko.  
  
 Le [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] déclenche l'exception 511 et annule l'instruction en cours d'exécution dans les cas suivants :  
  
-   L'instruction SELECT produit une ligne de résultats ou une ligne de table de travail intermédiaire excédant les 8060 octets.  
  
-   L'instruction DELETE, INSERT ou UPDATE tente d'opérer une action sur une ligne excédant les 8060 octets.  
  
 Une erreur se produit si aucun nom de colonne n'est attribué à une colonne créée par une instruction SELECT INTO ou CREATE VIEW.  
  
## <a name="see-also"></a>Voir aussi  
 [Exemples SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-examples-transact-sql.md)   
 [Expressions &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)  
  
  
