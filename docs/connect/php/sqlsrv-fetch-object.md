---
description: sqlsrv_fetch_object
title: sqlsrv_fetch_object | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- sqlsrv_fetch_object
apitype: NA
helpviewer_keywords:
- sqlsrv_fetch_object
- API Reference, sqlsrv_fetch_object
- retrieving data, as an object
ms.assetid: 4ce2df2c-083a-4a4d-a1e2-e866e63707d5
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 6b173c2676f25b4866e1981611c1375e4cd1bf6b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99154468"
---
# <a name="sqlsrv_fetch_object"></a>sqlsrv_fetch_object
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Récupère la ligne suivante de données sous la forme d’un objet PHP.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
sqlsrv_fetch_object( resource $stmt [, string $className [, array $ctorParams[, row[, ]offset]]])  
```  
  
#### <a name="parameters"></a>Paramètres  
*$stmt* : ressource d’instruction correspondant à une instruction exécutée.  
  
*$className* [FACULTATIF]: chaîne spécifiant le nom de la classe à instancier. Si aucune valeur n’est spécifiée pour le paramètre *$className* , une instance de **stdClass** PHP est instanciée.  
  
*$ctorParams* [FACULTATIF] : tableau qui contient les valeurs passées au constructeur de la classe spécifiée avec le paramètre *$className*. Si le constructeur de la classe spécifiée accepte des valeurs de paramètre, le paramètre *$ctorParams* doit être utilisé durant l’appel de **sqlsrv_fetch_object**.  
  
*row* [FACULTATIF] : l’une des valeurs suivantes, spécifiant la ligne à laquelle accéder dans un jeu de résultats qui utilise un curseur de défilement. (Si *row* est spécifié, *$className* et *$ctorParams* doivent être explicitement spécifiés, même si vous devez spécifier une valeur Null pour *$className* et *$ctorParams*.)  
  
-   SQLSRV_SCROLL_NEXT  
  
-   SQLSRV_SCROLL_PRIOR  
  
-   SQLSRV_SCROLL_FIRST  
  
-   SQLSRV_SCROLL_LAST  
  
-   SQLSRV_SCROLL_ABSOLUTE  
  
-   SQLSRV_SCROLL_RELATIVE  
  
Pour plus d’informations sur ces valeurs, consultez [Spécification d’un type de curseur et sélection de lignes](../../connect/php/specifying-a-cursor-type-and-selecting-rows.md).  
  
*décaler* [FACULTATIF] : Utilisé avec SQLSRV_SCROLL_ABSOLUTE et SQLSRV_SCROLL_RELATIVE pour spécifier la ligne à récupérer. Le premier enregistrement dans le jeu de résultats est 0.  
  
## <a name="return-value"></a>Valeur de retour  
Objet PHP avec des propriétés qui correspondent à des noms de champ du jeu de résultats. Les valeurs de propriété sont renseignées avec les valeurs de champ de jeu de résultats correspondantes. Si la classe spécifiée avec le paramètre *$className* facultatif n’existe pas ou s’il n’existe aucun jeu de résultats actif associé à l’instruction spécifiée, **false** est retourné. S’il n’y a plus aucune ligne à récupérer, la valeur **null** est retournée.  
  
Le type de données d’une valeur dans l’objet retourné correspond au type de données PHP par défaut. Pour plus d’informations sur les types de données PHP par défaut, consultez [Default PHP Data Types](../../connect/php/default-php-data-types.md).  
  
## <a name="remarks"></a>Notes  
Si un nom de classe est spécifié avec le paramètre *$className* facultatif, un objet de ce type de classe est instancié. Si la classe a des propriétés dont les noms correspondent aux noms de champ du jeu de résultats, les valeurs du jeu de résultats correspondantes sont appliquées aux propriétés. Si un nom de champ du jeu de résultats ne correspond pas à une propriété de classe, une propriété avec le nom de champ du jeu de résultats est ajoutée à l’objet et la valeur de jeu de résultats est appliquée à la propriété.  
  
Les règles suivantes s’appliquent lors de la spécification d’une classe avec le paramètre *$className* :  
  
-   La mise en correspondance respecte la casse. Par exemple, le nom de propriété CustomerId ne correspond pas au nom de champ CustomerID. Dans ce cas, une propriété CustomerID est ajoutée à l’objet et la valeur du champ CustomerID est donnée à la propriété CustomerID.  
  
-   La mise en correspondance se produit indépendamment des modificateurs d’accès. Par exemple, si la classe spécifiée a une propriété privée dont le nom correspond à un nom de champ du jeu de résultats, la valeur de ce champ est appliquée à la propriété.  
  
-   Les types de données des propriétés de classe sont ignorés. Si le champ CustomerID dans le jeu de résultats est une chaîne, alors que la propriété CustomerID de la classe est un entier, la valeur de la chaîne du jeu de résultats est écrite dans la propriété CustomerID.  
  
-   Si la classe spécifiée n’existe pas, la fonction retourne **false** et ajoute une erreur à la collection d’erreurs. Pour plus d’informations sur la récupération des informations d’erreur, consultez [sqlsrv_errors](../../connect/php/sqlsrv-errors.md).  
  
Si un champ sans nom est retourné, **sqlsrv_fetch_object** ignore la valeur du champ et émet un avertissement. Par exemple, considérez cette instruction Transact-SQL qui insère une valeur dans une table de base de données et récupère la clé primaire générée par le serveur :  
  
```sql
INSERT INTO Production.ProductPhoto (LargePhoto) VALUES (?);  
SELECT SCOPE_IDENTITY()
```
  
Si les résultats retournés par cette requête sont récupérés avec **sqlsrv_fetch_object**, la valeur retournée par `SELECT SCOPE_IDENTITY()` est ignorée, et un avertissement est émis. Pour éviter cela, vous pouvez spécifier un nom pour le champ retourné dans l’instruction Transact-SQL. Voici un moyen de spécifier un nom de colonne dans Transact-SQL :  
  
```sql
SELECT SCOPE_IDENTITY() AS PictureID
```
  
## <a name="object-example"></a>Exemple d’objet  
L’exemple suivant récupère chaque ligne d’un jeu de résultats sous la forme d’un objet PHP. L’exemple part du principe que SQL Server et la base de données [AdventureWorks](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works) sont installés sur l’ordinateur local. Toute la sortie est écrite dans la console quand l’exemple est exécuté à partir de la ligne de commande.  
  
``` 
<?php  
/* Connect to the local server using Windows Authentication and  
specify the AdventureWorks database as the database in use. */  
$serverName = "(local)";  
$connectionInfo = array( "Database"=>"AdventureWorks");  
$conn = sqlsrv_connect( $serverName, $connectionInfo);  
if( $conn === false )  
{  
     echo "Could not connect.\n";  
     die( print_r( sqlsrv_errors(), true));  
}  
  
/* Set up and execute the query. */  
$tsql = "SELECT FirstName, LastName  
         FROM Person.Contact  
         WHERE LastName='Alan'";  
$stmt = sqlsrv_query( $conn, $tsql);  
if( $stmt === false )  
{  
     echo "Error in query preparation/execution.\n";  
     die( print_r( sqlsrv_errors(), true));  
}  
  
/* Retrieve each row as a PHP object and display the results.*/  
while( $obj = sqlsrv_fetch_object( $stmt))  
{  
      echo $obj->LastName.", ".$obj->FirstName."\n";  
}  
  
/* Free statement and connection resources. */  
sqlsrv_free_stmt( $stmt);  
sqlsrv_close( $conn);  
?>  
```  
  
## <a name="class-example"></a>Exemple de classe  
L’exemple suivant récupère chaque ligne d’un jeu de résultats sous la forme d’une instance de la classe *Product* définie dans le script. L’exemple récupère, à partir des tables *Purchasing.PurchaseOrderDetail* et *Production.Product* de la base de données AdventureWorks, les informations sur les produits qui ont une date d’échéance spécifique (*DueDate*), et dont la quantité en stock (*StockQty*) est inférieure à la valeur spécifiée. L’exemple met en évidence certaines des règles qui s’appliquent durant la spécification d’une classe dans un appel à **sqlsrv_fetch_object**:  
  
-   La variable *$product* est une instance de la classe *Product* , car Product a été spécifié avec le paramètre *$className* et la classe *Product* existe.  
  
-   La propriété *Name* est ajoutée à l’instance *$product* , car la propriété *name* existante ne correspond pas.  
  
-   La propriété *Color* est ajoutée à l’instance *$product* , car il n’existe pas de propriété correspondante.  
  
-   La propriété privée *UnitPrice* est renseignée avec la valeur du champ *UnitPrice* .  
  
L’exemple part du principe que SQL Server et la base de données [AdventureWorks](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works) sont installés sur l’ordinateur local. Toute la sortie est écrite dans la console quand l’exemple est exécuté à partir de la ligne de commande.  
  
```  
<?php  
/* Define the Product class. */  
class Product  
{  
     /* Constructor */  
     public function Product($ID)  
     {  
          $this->objID = $ID;  
     }  
     public $objID;  
     public $name;  
     public $StockedQty;  
     public $SafetyStockLevel;  
     private $UnitPrice;  
     function getPrice()  
     {  
          return $this->UnitPrice;  
     }  
}  
  
/* Connect to the local server using Windows Authentication, and  
specify the AdventureWorks database as the database in use. */  
$serverName = "(local)";  
$connectionInfo = array( "Database"=>"AdventureWorks");  
$conn = sqlsrv_connect( $serverName, $connectionInfo);  
if( $conn === false )  
{  
     echo "Could not connect.\n";  
     die( print_r( sqlsrv_errors(), true));  
}  
  
/* Define the query. */  
$tsql = "SELECT Name,  
                SafetyStockLevel,  
                StockedQty,  
                UnitPrice,  
                Color  
         FROM Purchasing.PurchaseOrderDetail AS pdo  
         JOIN Production.Product AS p  
         ON pdo.ProductID = p.ProductID  
         WHERE pdo.StockedQty < ?  
         AND pdo.DueDate= ?";  
  
/* Set the parameter values. */  
$params = array(3, '2002-01-29');  
  
/* Execute the query. */  
$stmt = sqlsrv_query( $conn, $tsql, $params);  
if ( $stmt )  
{  
     echo "Statement executed.\n";  
}   
else   
{  
     echo "Error in statement execution.\n";  
     die( print_r( sqlsrv_errors(), true));  
}  
  
/* Iterate through the result set, printing a row of data upon each  
 iteration. Note the following:  
     1) $product is an instance of the Product class.  
     2) The $ctorParams parameter is required in the call to  
        sqlsrv_fetch_object, because the Product class constructor is  
        explicity defined and requires parameter values.  
     3) The "Name" property is added to the $product instance because  
        the existing "name" property does not match.  
     4) The "Color" property is added to the $product instance  
        because there is no matching property.  
     5) The private property "UnitPrice" is populated with the value  
        of the "UnitPrice" field.*/  
$i=0; //Used as the $objID in the Product class constructor.  
while( $product = sqlsrv_fetch_object( $stmt, "Product", array($i)))  
{  
     echo "Object ID: ".$product->objID."\n";  
     echo "Product Name: ".$product->Name."\n";  
     echo "Stocked Qty: ".$product->StockedQty."\n";  
     echo "Safety Stock Level: ".$product->SafetyStockLevel."\n";  
     echo "Product Color: ".$product->Color."\n";  
     echo "Unit Price: ".$product->getPrice()."\n";  
     echo "-----------------\n";  
     $i++;  
}  
  
/* Free statement and connection resources. */  
sqlsrv_free_stmt( $stmt);  
sqlsrv_close( $conn);  
?>  
```  
  
La variable **sqlsrv_fetch_object** retourne toujours des données en fonction des [Default PHP Data Types](../../connect/php/default-php-data-types.md). Pour plus d’informations sur la façon de spécifier le type de données PHP, consultez [Procédure : spécifier des types de données PHP](../../connect/php/how-to-specify-php-data-types.md).  
  
Si un champ sans nom est retourné, **sqlsrv_fetch_object** ignore la valeur du champ et émet un avertissement. Par exemple, considérez cette instruction Transact-SQL qui insère une valeur dans une table de base de données et récupère la clé primaire générée par le serveur :  
  
```sql
INSERT INTO Production.ProductPhoto (LargePhoto) VALUES (?);  
SELECT SCOPE_IDENTITY()
```
  
Si les résultats retournés par cette requête sont récupérés avec **sqlsrv_fetch_object**, la valeur retournée par `SELECT SCOPE_IDENTITY()` est ignorée, et un avertissement est émis. Pour éviter cela, vous pouvez spécifier un nom pour le champ retourné dans l’instruction Transact-SQL. Voici un moyen de spécifier un nom de colonne dans Transact-SQL :  
  
```sql
SELECT SCOPE_IDENTITY() AS PictureID
```
  
## <a name="see-also"></a>Voir aussi  
[Récupération de données](../../connect/php/retrieving-data.md)  

[À propos des exemples de code dans la documentation](../../connect/php/about-code-examples-in-the-documentation.md)  

[Informations de référence sur l’API du pilote SQLSRV](../../connect/php/sqlsrv-driver-api-reference.md)  
  
