---
title: Convertir les types de données R et SQL
description: Passez en revue les conversions de types de données implicites et explicites entre R et SQL Server dans les solutions de science des données et d’apprentissage automatique.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 10/06/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: e024d65ed8a2dd9b55b7940fa19ed400a01af175
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348118"
---
# <a name="data-type-mappings-between-r-and-sql-server"></a>Mappages de types de données entre R et SQL Server
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

Cet article liste les types de données pris en charge, ainsi que les conversions de types de données exécutées, lors de l’utilisation de la fonctionnalité d’intégration R dans SQL Server Machine Learning Services.

## <a name="base-r-version"></a>Version R de base

SQL Server 2016 R services et SQL Server Machine Learning Services avec R sont alignés sur des versions spécifiques de Microsoft R Open. Par exemple, la dernière version, SQL Server 2019 Machine Learning Services, repose sur Microsoft R Open 3.5.2.

Pour afficher la version R associée à une instance particulière de SQL Server, ouvrez **RGui** dans l’instance SQL. Par exemple, le chemin de l’instance par défaut dans SQL Server 2019 serait : `C:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\R_SERVICES\bin\x64\Rgui.exe`.

L’outil charge les bibliothèques R de base et d’autres bibliothèques. Les informations de version du package sont fournies dans une notification pour chaque package chargé au démarrage de la session.

## <a name="r-and-sql-data-types"></a>Types de données R et SQL

Alors que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prend en charge plusieurs dizaines de types de données, R est compatible avec un nombre limité de types de données scalaires (numériques, entières, complexes, logiques, caractère, date/heure et brutes). Par conséquent, chaque fois que vous utilisez des données de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans des scripts R, les données peuvent être implicitement converties en un type de données compatible. Toutefois, la plupart du temps, une conversion exacte ne peut pas être effectuée automatiquement et une erreur est renvoyée, telle que « Unhandled SQL data type ».

Cette section répertorie les conversions implicites fournies et les types de données non pris en charge. Des conseils sont fournis pour le mappage des types de données entre R et SQL Server.

## <a name="implicit-data-type-conversions"></a>Conversions de types de données implicites

Le tableau suivant présente les modifications de types de données et de valeurs lorsque des données de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont utilisées dans un script R et retournées à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

| Type SQL                         | Classe R     | Type RESULT SET    | Commentaires            |
|----------------------------------|-------------|--------------------|---------------------|
| **bigint**                       | `numeric`   | **float**          | L’exécution d’un script R avec `sp_execute_external_script` autorise les données d’entrée de type bigint. Toutefois, étant donné qu’elles sont converties dans le type numérique de R, elles subissent une perte de précision pour les valeurs très élevées ou comportant des décimales. R prend en charge les entiers dans la limite de 53 bits, au-delà de laquelle il commence à afficher une perte de précision.                                                                                         |
| **binary(n)**<br /> n <= 8000    | `raw`       | **varbinary(max)** | Uniquement autorisé en tant que paramètre d’entrée et sortie |
| **bit**                          | `logical`   | **bit**            |                     |
| **char(n)**<br /> n <= 8000      | `character` | **varchar(max)**   | La trame de données d’entrée (input_data_1) est créée sans définir explicitement un paramètre *stringsAsFactors*. Ainsi, le type de la colonne dépend de *default.stringsAsFactors()* en R.                                                                                                                                                                                                                                           |
| **datetime**                     | `POSIXct`   | **datetime**       | Représenté au format GMT  |
| **date**                         | `POSIXct`   | **datetime**       | Représenté au format GMT  |
| **decimal(p,s)**                 | `numeric`   | **float**          | L’exécution d’un script R avec `sp_execute_external_script` autorise les données d’entrée de type décimal. Toutefois, étant donné qu’elles sont converties dans le type numérique de R, elles subissent une perte de précision pour les valeurs très élevées ou comportant des décimales. `sp_execute_external_script` avec un script R ne prend pas en charge la totalité du type de données et modifie les derniers chiffres décimaux, en particulier ceux qui comportent une fraction. |
| **float**                        | `numeric`   | **float**          |                     |
| **int**                          | `integer`   | **int**            |                     |
| **money**                        | `numeric`   | **float**          | L’exécution d’un script R avec `sp_execute_external_script` autorise les données d’entrée de type monnaie. Toutefois, étant donné qu’elles sont converties dans le type numérique de R, elles subissent une perte de précision pour les valeurs très élevées ou comportant des décimales. Les valeurs de centimes sont parfois imprécises, auquel cas un avertissement est émis : *Avertissement : Impossible de représenter avec précision les valeurs de centimes*.                                               |
| **numeric(p,s)**                 | `numeric`   | **float**          | L’exécution d’un script R avec `sp_execute_external_script` autorise les données d’entrée de type numérique. Toutefois, étant donné qu’elles sont converties dans le type numérique de R, elles subissent une perte de précision pour les valeurs très élevées ou comportant des décimales. `sp_execute_external_script` avec un script R ne prend pas en charge la totalité du type de données et modifie les derniers chiffres décimaux, en particulier ceux qui comportent une fraction. |
| **real**                         | `numeric`   | **float**          |                     |
| **smalldatetime**                | `POSIXct`   | **datetime**       | Représenté au format GMT  |
| **smallint**                     | `integer`   | **int**            |                     |
| **smallmoney**                   | `numeric`   | **float**          |                     |
| **tinyint**                      | `integer`   | **int**            |                     |
| **uniqueidentifier**             | `character` | **varchar(max)**   |                     |
| **varbinary(n)**<br /> n <= 8000 | `raw`       | **varbinary(max)** | Uniquement autorisé en tant que paramètre d’entrée et sortie |
| **varbinary(max)**               | `raw`       | **varbinary(max)** | Uniquement autorisé en tant que paramètre d’entrée et sortie |
| **varchar(n)**<br /> n <= 8000   | `character` | **varchar(max)**   | La trame de données d’entrée (input_data_1) est créée sans définir explicitement un paramètre *stringsAsFactors*. Ainsi, le type de la colonne dépend de *default.stringsAsFactors()* en R.                                                                                                                                                                                                                                           |

## <a name="data-types-not-supported-by-r"></a>Types de données non pris en charge par R

Parmi les catégories de types de données pris en charge par le [système SQL Server](../../t-sql/data-types/data-types-transact-sql.md), les types suivants sont susceptibles de poser des problèmes lorsqu’ils sont passés au code R :

+ Types de données répertoriés dans la section **Autres** de l’article système de type SQL : **cursor**, **timestamp**, **hierarchyid**, **uniqueidentifier**, **sql_variant**, **xml**, **table**
+ Tous les types spatiaux
+ **image**

## <a name="data-types-that-might-convert-poorly"></a>Types de données qui peuvent être mal convertis

+ La plupart des types datetime doivent fonctionner, à l’exception de **datetimeoffset**.
+ La plupart des types de données numériques sont pris en charge, mais les conversions risquent d’échouer pour **money** et **smallmoney**.
+ **varchar** est pris en charge, mais étant donné que SQL Server utilise en règle générale Unicode, l’utilisation de **nvarchar** et d’autres types de données texte Unicode est recommandée lorsque cela est possible.
+ Les fonctions de la bibliothèque RevoScaleR précédées de rx peuvent traiter les types de données binaires SQL (**binary** et **varbinary**), mais dans la plupart des scénarios, un traitement spécial sera nécessaire pour ces types. La plupart du code R ne fonctionne pas avec des colonnes binaires.

  
 Pour plus d’informations sur les types de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez [Types de données &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md).

## <a name="changes-in-data-types-between-sql-server-versions"></a>Modifications des types de données entre les versions de SQL Server.

Microsoft SQL Server 2016 et les versions ultérieures apportent des améliorations aux conversions des types de données et à plusieurs autres opérations. Ils offrent une plus grande précision lors de l’utilisation de types à virgule flottante, ainsi que des modifications mineures des opérations sur les types **datetime** classiques.

Ces améliorations sont toutes disponibles par défaut lorsque vous utilisez un niveau de compatibilité de base de données de 130 ou ultérieur. Toutefois, si vous utilisez un niveau de compatibilité différent ou si vous vous connectez à une base de données à l’aide d’une version antérieure, vous pouvez constater des différences dans la précision des nombres ou d’autres résultats. 

Pour plus d’informations, consultez [Améliorations de SQL Server 2016 dans le traitement de certains types de données et des opérations peu courantes](https://support.microsoft.com/help/4010261/sql-server-2016-improvements-in-handling-some-data-types-and-uncommon-).
 

## <a name="verify-r-and-sql-data-schemas-in-advance"></a>Vérifier à l’avance les schémas de données R et SQL

En général, en cas de doute sur la manière dont un type ou une structure de données spécifique est utilisé dans R, vous pouvez faire appel à la fonction  `str()` pour obtenir la structure interne et le type de l’objet R. Le résultat de la fonction s’affiche dans la console R et est également disponible dans les résultats de requête, dans l’onglet **Messages** de [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]. 

Lorsque vous récupérez des données d’une base de données pour les utiliser dans le code R, vous devez toujours supprimer les colonnes qui ne peuvent pas être utilisés dans R, de même que les colonnes qui ne sont pas utiles pour l’analyse, comme les GUID (uniqueidentifier), les horodateurs et les autres colonnes utilisées pour l’audit, ou les informations de lignage créées par le processus ETL. 

Notez que la conservation des colonnes inutiles peut réduire considérablement les performances du code R, surtout si des colonnes de cardinalité élevée sont utilisées comme facteurs. Par conséquent, nous vous recommandons d’utiliser les procédures stockées système de SQL Server et les vues informations pour obtenir les types de données d’une table spécifique à l’avance et de supprimer ou de convertir les colonnes incompatibles. Pour plus d’informations, consultez [Vues de schémas d’informations système dans Transact-SQL](../../relational-databases/system-information-schema-views/system-information-schema-views-transact-sql.md)

Si un type de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] particulier n’est pas pris en charge par R, mais que vous devez utiliser les colonnes de données dans le script R, nous vous recommandons d’utiliser les fonctions [CAST et CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md) pour vous assurer que les conversions de types de données sont effectuées comme attendu avant d’utiliser les données dans votre script R.  

> [!WARNING]
> Si vous utilisez **rxDataStep** pour supprimer les colonnes incompatibles lors du déplacement de données, n’oubliez pas que les arguments _varsToKeep_ et _varsToDrop_ ne sont pas pris en charge pour le type de source de données **RxSqlServerData**.


## <a name="examples"></a>Exemples

### <a name="example-1-implicit-conversion"></a>Exemple 1 : Conversion implicite

L’exemple suivant montre comment les données sont transformées lors de la boucle entre SQL Server et R.

La requête extrait une série de valeurs d’une table [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et utilise la procédure stockée [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) pour générer les valeurs à l’aide du runtime R.

```sql
CREATE TABLE MyTable (    
 c1 int,    
 c2 varchar(10),    
 c3 uniqueidentifier    
);    
go    
INSERT MyTable VALUES(1, 'Hello', newid());    
INSERT MyTable VALUES(-11, 'world', newid());    
SELECT * FROM MyTable;    
  
EXECUTE sp_execute_external_script    
 @language = N'R'    
 , @script = N'    
inputDataSet["cR"] <- c(4, 2)    
str(inputDataSet)    
outputDataSet <- inputDataSet'    
 , @input_data_1 = N'SELECT c1, c2, c3 FROM MyTable'    
 , @input_data_1_name = N'inputDataSet'    
 , @output_data_1_name = N'outputDataSet'    
 WITH RESULT SETS((C1 int, C2 varchar(max), C3 varchar(max), C4 float));  
```

**Résultats**

|Ligne \#|C1|C2|C3|C4|
|-|-|-|-|-|
|1|1|Hello|6e225611-4b58-4995-a0a5-554d19012ef1|4|
|2|-11|world|6732ea46-2d5d-430b-8ao1-86e7f3351c3e|2|

Notez l’utilisation de la fonction `str` dans R pour obtenir le schéma des données de sortie. Cette fonction retourne les informations suivantes :

```output
'data.frame':2 obs. of  4 variables:
 $ c1: int  1 -11
 $ c2: Factor w/ 2 levels "Hello","world": 1 2
 $ c3: Factor w/ 2 levels "6732EA46-2D5D-430B-8A01-86E7F3351C3E",..: 2 1
 $ cR: num  4 2
```

Vous pouvez voir que les conversions de types de données suivantes ont été effectuées implicitement dans le cadre de cette requête :

+ **Colonne C1**. La colonne est représentée sous la forme **int** dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], `integer` dans R et **int** dans le jeu de résultats de sortie.  
  
  Aucune conversion de type n’a été effectuée.  
  
+ **Colonne C2**. La colonne est représentée sous la forme **varchar(10)** dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], `factor` dans R et **varchar(max)** dans la sortie.  
  
  Notez le changement de la sortie : toute chaîne de R (un facteur ou une chaîne normale) est représentée sous la forme **varchar(max)** , quelle que soit la longueur de la chaîne.  
  
+ **Colonne C3**.  La colonne est représentée sous la forme **uniqueidentifier** dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], `character` dans R et **varchar(max)** dans la sortie.
  
  Remarque la conversion de type de données qui se produit. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prend en charge le type **uniqueidentifier** , mais pas R ; par conséquent, les identificateurs sont représentés sous forme de chaînes.
  
+ **Colonne C4**. La colonne contient des valeurs générées par le script R et non présentes dans les données d’origine.


## <a name="example-2-dynamic-column-selection-using-r"></a>Exemple 2 : sélection de colonnes dynamiques à l’aide de R

L’exemple suivant montre comment vous pouvez utiliser le code R pour vérifier les types de colonnes non valides. La requête extrait le schéma d’une table spécifiée à l’aide des vues système SQL Server et supprime toutes les colonnes qui ont un type non valide spécifié.

```R
connStr <- "Server=.;Database=TestDB;Trusted_Connection=Yes"
data <- RxSqlServerData(connectionString = connStr, sqlQuery = "SELECT COLUMN_NAME FROM TestDB.INFORMATION_SCHEMA.COLUMNS WHERE TABLE_NAME = N'testdata' AND DATA_TYPE <> 'image';")
columns <- rxImport(data)
columnList <- do.call(paste, c(as.list(columns$COLUMN_NAME), sep = ","))
sqlQuery <- paste("SELECT", columnList, "FROM testdata")
```

## <a name="see-also"></a>Voir aussi

+ [Mappages de types de données entre Python et SQL Server](../python/python-libraries-and-data-types.md)
