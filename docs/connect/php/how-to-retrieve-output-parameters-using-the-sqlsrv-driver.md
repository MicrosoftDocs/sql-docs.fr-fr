---
title: 'Procédure : Récupérer des paramètres de sortie à l’aide du pilote SQLSRV'
description: Découvrez comment utiliser et récupérer des paramètres de sortie dans une procédure stockée avec Microsoft SQLSRV Driver pour PHP pour SQL Server.
ms.custom: ''
ms.date: 04/11/2018
ms.prod: sql
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- stored procedure support
ms.assetid: 1157bab7-6ad1-4bdb-a81c-662eea3e7fcd
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 6f0e165c04e303a5cb65046b013df8814bfbf515
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100038363"
---
# <a name="how-to-retrieve-output-parameters-using-the-sqlsrv-driver"></a>Procédure : Récupérer des paramètres de sortie à l’aide du pilote SQLSRV
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Cette rubrique montre comment appeler une procédure stockée dans laquelle un seul paramètre a été défini en tant que paramètre de sortie. Quand vous récupérez un paramètre de sortie ou d’entrée/sortie, tous les résultats retournés par la procédure stockée doivent être consommés pour que la valeur de paramètre retournée soit accessible.  
  
> [!NOTE]  
>  Les variables initialisées ou mises à jour avec la valeur **Null**, **DateTime** ou des types de flux ne peuvent pas être utilisées comme paramètres de sortie.  
  
Une troncation de données peut se produire quand des types de flux comme SQLSRV_SQLTYPE_VARCHAR('max') sont utilisés comme paramètres de sortie. Les types de flux ne sont pas pris en charge comme paramètres de sortie. Pour les types autres que types de flux, une troncation de données peut se produire si la longueur du paramètre de sortie n’est pas spécifiée ou si la longueur spécifiée n’est pas assez élevée pour le paramètre de sortie.  
  
## <a name="example-1"></a>Exemple 1
L’exemple suivant appelle une procédure stockée qui retourne les ventes de l’année jusqu’à ce jour effectuées par un employé spécifique. La variable PHP *$lastName* est un paramètre d’entrée et *$salesYTD* est un paramètre de sortie.  
  
> [!NOTE]  
> L’initialisation de *$salesYTD* à 0.0 définit le PHPTYPE retourné sur **float**. Pour garantir l’intégrité du type de données, les paramètres de sortie doivent être initialisés avant d’appeler la procédure stockée, ou bien le PHPTYPE souhaité doit être spécifié. Pour plus d’informations sur la spécification du PHPTYPE, consultez [How to: Specify PHP Data Types](../../connect/php/how-to-specify-php-data-types.md).  
  
Étant donné qu’un seul résultat est retourné par la procédure stockée, *$salesYTD* contient la valeur retournée du paramètre de sortie immédiatement après l’exécution de la procédure stockée.  
  
> [!NOTE]  
> Appeler les procédures stockées à l’aide de la syntaxe canonique est la pratique recommandée. Pour plus d’informations sur la syntaxe canonique, consultez [Appel d’une procédure stockée](../../relational-databases/native-client-odbc-stored-procedures/calling-a-stored-procedure.md).  
  
L’exemple part du principe que SQL Server et la base de données [AdventureWorks](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works) sont installés sur l’ordinateur local. Toute la sortie est écrite dans la console quand l’exemple est exécuté à partir de la ligne de commande.  
  
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
  
/* Drop the stored procedure if it already exists. */  
$tsql_dropSP = "IF OBJECT_ID('GetEmployeeSalesYTD', 'P') IS NOT NULL  
                DROP PROCEDURE GetEmployeeSalesYTD";  
$stmt1 = sqlsrv_query( $conn, $tsql_dropSP);  
if( $stmt1 === false )  
{  
     echo "Error in executing statement 1.\n";  
     die( print_r( sqlsrv_errors(), true));  
}  
  
/* Create the stored procedure. */  
$tsql_createSP = " CREATE PROCEDURE GetEmployeeSalesYTD  
                   @SalesPerson nvarchar(50),  
                   @SalesYTD money OUTPUT  
                   AS  
                   SELECT @SalesYTD = SalesYTD  
                   FROM Sales.SalesPerson AS sp  
                   JOIN HumanResources.vEmployee AS e   
                   ON e.EmployeeID = sp.SalesPersonID  
                   WHERE LastName = @SalesPerson";  
$stmt2 = sqlsrv_query( $conn, $tsql_createSP);  
if( $stmt2 === false )  
{  
     echo "Error in executing statement 2.\n";  
     die( print_r( sqlsrv_errors(), true));  
}  
  
/*--------- The next few steps call the stored procedure. ---------*/  
  
/* Define the Transact-SQL query. Use question marks (?) in place of  
 the parameters to be passed to the stored procedure */  
$tsql_callSP = "{call GetEmployeeSalesYTD( ?, ? )}";  
  
/* Define the parameter array. By default, the first parameter is an  
INPUT parameter. The second parameter is specified as an OUTPUT  
parameter. Initializing $salesYTD to 0.0 sets the returned PHPTYPE to  
float. To ensure data type integrity, output parameters should be  
initialized before calling the stored procedure, or the desired  
PHPTYPE should be specified in the $params array.*/  
$lastName = "Blythe";  
$salesYTD = 0.0;  
$params = array(   
                 array($lastName, SQLSRV_PARAM_IN),  
                 array(&$salesYTD, SQLSRV_PARAM_OUT)  
               );  
  
/* Execute the query. */  
$stmt3 = sqlsrv_query( $conn, $tsql_callSP, $params);  
if( $stmt3 === false )  
{  
     echo "Error in executing statement 3.\n";  
     die( print_r( sqlsrv_errors(), true));  
}  
  
/* Display the value of the output parameter $salesYTD. */  
echo "YTD sales for ".$lastName." are ". $salesYTD. ".";  
  
/*Free the statement and connection resources. */  
sqlsrv_free_stmt( $stmt1);  
sqlsrv_free_stmt( $stmt2);  
sqlsrv_free_stmt( $stmt3);  
sqlsrv_close( $conn);  
?>  
```  

> [!NOTE]
> Si vous liez un paramètre de sortie à un type bigint et que la valeur se retrouve en dehors de la plage d’un [entier](../../t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql.md), vous devez spécifier le type de champ SQL SQLSRV_SQLTYPE_BIGINT. Sinon, une exception « valeur hors limites » risque de se produire.

## <a name="example-2"></a>Exemple 2
Cet exemple de code montre comment lier une grosse valeur bigint comme paramètre de sortie.  

```
<?php
$serverName = "(local)";
$connectionInfo = array("Database"=>"testDB");  
$conn = sqlsrv_connect($serverName, $connectionInfo);  
if ($conn === false) {  
    echo "Could not connect.\n";  
    die(print_r(sqlsrv_errors(), true));  
}  

// Assume the stored procedure spTestProcedure exists, which retrieves a bigint value of some large number
// e.g. 9223372036854
$bigintOut = 0;
$outSql = "{CALL spTestProcedure (?)}";
$stmt = sqlsrv_prepare($conn, $outSql, array(array(&$bigintOut, SQLSRV_PARAM_OUT, null, SQLSRV_SQLTYPE_BIGINT)));
sqlsrv_execute($stmt);
echo "$bigintOut\n";   // Expect 9223372036854

sqlsrv_free_stmt($stmt);  
sqlsrv_close($conn);  

?>
```

## <a name="see-also"></a>Voir aussi  
[Procédure : Spécifier la direction du paramètre à l’aide du pilote SQLSRV](../../connect/php/how-to-specify-parameter-direction-using-the-sqlsrv-driver.md)

[Procédure : Récupérer des paramètres d’entrée et de sortie à l’aide du pilote SQLSRV](../../connect/php/how-to-retrieve-input-and-output-parameters-using-the-sqlsrv-driver.md)

[Récupération de données](../../connect/php/retrieving-data.md)  
  
