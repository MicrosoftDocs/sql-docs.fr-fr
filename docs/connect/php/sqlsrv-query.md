---
title: sqlsrv_query
description: La fonction sqlsrv_query fournit une méthode pour exécuter une requête avec une quantité minimale de code et peut être utilisée pour exécuter des requêtes paramétrables.
ms.custom: ''
ms.date: 04/11/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- sqlsrv_query
apitype: NA
helpviewer_keywords:
- sqlsrv_query
- executing queries
- API Reference, sqlsrv_query
ms.assetid: 9fa7c4c8-4da8-4299-9893-f61815055aa3
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 751fd08e63cccfe4293785191a223728690ed13e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201074"
---
# <a name="sqlsrv_query"></a>sqlsrv_query
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Prépare et exécute une instruction.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sqlsrv_query(resource $conn, string $tsql [, array $params [, array $options]])  
```  
  
#### <a name="parameters"></a>Paramètres  
*$conn* : La ressource de connexion associée à l’instruction préparée.  
  
*$tsql* : L’expression Transact-SQL qui correspond à l’instruction préparée.  
  
*$params* [FACULTATIF] : objet **array** de valeurs qui correspondent aux paramètres d’une requête paramétrable. Chaque élément du tableau peut être l’un des éléments suivants :
  
-   Une valeur littérale  
  
-   Une variable PHP  
  
-   Un objet **array** avec la structure suivante :  
  
    ```  
    array($value [, $direction [, $phpType [, $sqlType]]])  
    ```  
  
    La description de chaque élément du tableau figure dans le tableau suivant :  
  
    |Élément|Description|  
    |-----------|---------------|  
    |*$value*|Valeur littérale, variable PHP ou variable PHP par référence.|  
    |*$direction*[FACULTATIF]|Une des constantes **SQLSRV_PARAM_\* *_ utilisées pour indiquer la direction du paramètre : _* SQLSRV_PARAM_IN**, **SQLSRV_PARAM_OUT**, **SQLSRV_PARAM_INOUT**. La valeur par défaut est **SQLSRV_PARAM_IN**.<br /><br />Pour plus d’informations sur les constantes PHP, consultez [Constantes &#40;Microsoft Drivers for PHP for SQL Server&#41;](../../connect/php/constants-microsoft-drivers-for-php-for-sql-server.md).|  
    |*$phpType*[FACULTATIF]|Constante **SQLSRV_PHPTYPE_\** _ qui spécifie le type de données PHP de la valeur retournée.<br /><br />Pour plus d’informations sur les constantes PHP, consultez [Constantes &#40;Microsoft Drivers for PHP for SQL Server&#41;](../../connect/php/constants-microsoft-drivers-for-php-for-sql-server.md).|  
    |_$sqlType*[FACULTATIF]|Constante **SQLSRV_SQLTYPE_\** _ qui spécifie le type de données SQL Server de la valeur d’entrée.<br /><br />Pour plus d’informations sur les constantes PHP, consultez [Constantes &#40;Microsoft Drivers for PHP for SQL Server&#41;](../../connect/php/constants-microsoft-drivers-for-php-for-sql-server.md).|  
  
_$options* [FACULTATIF] : tableau associatif qui définit les propriétés de la requête. Cette même liste de clés est également prise en charge par [sqlsrv_prepare](../../connect/php/sqlsrv-prepare.md#properties).
  
## <a name="return-value"></a>Valeur de retour  
Ressource d’instruction. Si l’instruction ne peut pas être créée et/ou exécutée, la valeur **false** est retournée.  
  
## <a name="remarks"></a>Notes   
La fonction **sqlsrv_query** convient bien aux requêtes à usage unique. Elle doit être le choix par défaut pour exécuter des requêtes, sauf en cas de circonstances particulières. Cette fonction fournit une méthode simplifiée pour exécuter une requête avec une quantité minimale de code. La fonction **sqlsrv_query** effectue à la fois la préparation et l’exécution des instructions. Vous pouvez l’utiliser pour exécuter des requêtes paramétrables.  
  
Pour plus d’informations, consultez [Procédure : Récupérer des paramètres de sortie à l’aide du pilote SQLSRV](../../connect/php/how-to-retrieve-output-parameters-using-the-sqlsrv-driver.md).  
  
## <a name="example-1"></a>Exemple 1  
Dans l’exemple suivant, une seule ligne est insérée dans la table *Sales.SalesOrderDetail* de la base de données AdventureWorks. L’exemple part du principe que SQL Server et la base de données [AdventureWorks](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works) sont installés sur l’ordinateur local.  Toute la sortie est écrite dans la console quand l’exemple est exécuté à partir de la ligne de commande.  
  
> [!NOTE]  
> Bien que l’exemple suivant utilise une instruction INSERT pour illustrer l’utilisation de **sqlsrv_query** pour exécuter une seule instruction, le concept s’applique à n’importe quelle instruction Transact-SQL.  
  
```  
<?php  
/* Connect to the local server using Windows Authentication and  
specify the AdventureWorks database as the database in use. */  
$serverName = "(local)";  
$connectionInfo = array("Database"=>"AdventureWorks");  
$conn = sqlsrv_connect($serverName, $connectionInfo);  
if ($conn === false) {  
    echo "Could not connect.\n";  
    die(print_r(sqlsrv_errors(), true));  
}  
  
/* Set up the parameterized query. */  
$tsql = "INSERT INTO Sales.SalesOrderDetail   
        (SalesOrderID,   
         OrderQty,   
         ProductID,   
         SpecialOfferID,   
         UnitPrice,   
         UnitPriceDiscount)  
        VALUES   
        (?, ?, ?, ?, ?, ?)";  
  
/* Set parameter values. */  
$params = array(75123, 5, 741, 1, 818.70, 0.00);  
  
/* Prepare and execute the query. */  
$stmt = sqlsrv_query($conn, $tsql, $params);  
if ($stmt) {  
    echo "Row successfully inserted.\n";  
} else {  
    echo "Row insertion failed.\n";  
    die(print_r(sqlsrv_errors(), true));  
}  
  
/* Free statement and connection resources. */  
sqlsrv_free_stmt($stmt);  
sqlsrv_close($conn);  
?>  
```  
  
## <a name="example-2"></a>Exemple 2  
L’exemple suivant met à jour un champ dans la table *Sales.SalesOrderDetail* de la base de données AdventureWorks. L’exemple part du principe que SQL Server et la base de données [AdventureWorks](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works) sont installés sur l’ordinateur local.  Toute la sortie est écrite dans la console quand l’exemple est exécuté à partir de la ligne de commande.  
  
```  
<?php  
/* Connect to the local server using Windows Authentication and  
specify the AdventureWorks database as the database in use. */  
$serverName = "(local)";  
$connectionInfo = array("Database"=>"AdventureWorks");  
$conn = sqlsrv_connect($serverName, $connectionInfo);  
if ($conn === false) {  
    echo "Could not connect.\n";  
    die(print_r(sqlsrv_errors(), true));  
}  
  
/* Set up the parameterized query. */  
$tsql = "UPDATE Sales.SalesOrderDetail   
         SET OrderQty = (?)   
         WHERE SalesOrderDetailID = (?)";  
  
/* Assign literal parameter values. */  
$params = array(5, 10);  
  
/* Execute the query. */  
if (sqlsrv_query($conn, $tsql, $params)) {  
    echo "Statement executed.\n";  
} else {  
    echo "Error in statement execution.\n";  
    die(print_r(sqlsrv_errors(), true));  
}  
  
/* Free connection resources. */  
sqlsrv_close($conn);  
?>  
```  
  
> [!NOTE]
> Il est recommandé d’utiliser des chaînes en entrée pour lier des valeurs à une [colonne décimale ou numérique](../../t-sql/data-types/decimal-and-numeric-transact-sql.md) pour des raisons de précision et d’exactitude, car PHP n’offre qu’une précision limitée pour les [nombres à virgule flottante](https://php.net/manual/en/language.types.float.php). Il en va de même pour les colonnes bigint, en particulier si les valeurs sont en dehors de la plage des [entiers](../../t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql.md).

## <a name="example-3"></a>Exemple 3  
Cet exemple de code montre comment lier une valeur décimale comme paramètre d’entrée.  

```
<?php
$serverName = "(local)";
$connectionInfo = array("Database"=>"YourTestDB");  
$conn = sqlsrv_connect($serverName, $connectionInfo);  
if ($conn === false) {  
     echo "Could not connect.\n";  
     die(print_r(sqlsrv_errors(), true));  
}  

// Assume TestTable exists with a decimal field 
$input = "9223372036854.80000";
$params = array($input);
$stmt = sqlsrv_query($conn, "INSERT INTO TestTable (DecimalCol) VALUES (?)", $params);

sqlsrv_free_stmt($stmt);  
sqlsrv_close($conn);  

?>
```

## <a name="example-4"></a>Exemple 4
Cet exemple de code montre comment créer une table de types [sql_variant](../../t-sql/data-types/sql-variant-transact-sql.md) et extraire les données insérées.

```
<?php
$server = 'serverName';
$dbName = 'databaseName';
$uid = 'yourUserName';
$pwd = 'yourPassword';

$options = array("Database"=>$dbName, "UID"=>$uid, "PWD"=>$pwd);
$conn = sqlsrv_connect($server, $options);
if($conn === false) {
    die(print_r(sqlsrv_errors(), true));
}

$tableName = 'testTable';
$query = "CREATE TABLE $tableName ([c1_int] sql_variant, [c2_varchar] sql_variant)";

$stmt = sqlsrv_query($conn, $query);
if($stmt === false) {
    die(print_r(sqlsrv_errors(), true));
}
sqlsrv_free_stmt($stmt);

$query = "INSERT INTO [$tableName] (c1_int, c2_varchar) VALUES (1, 'test_data')";
$stmt = sqlsrv_query($conn, $query);
if($stmt === false) {
    die(print_r(sqlsrv_errors(), true));
}
sqlsrv_free_stmt($stmt);

$query = "SELECT * FROM $tableName";
$stmt = sqlsrv_query($conn, $query);

if(sqlsrv_fetch($stmt) === false) {
    die(print_r(sqlsrv_errors(), true));
}

$col1 = sqlsrv_get_field($stmt, 0);
echo "First field:  $col1 \n";

$col2 = sqlsrv_get_field($stmt, 1);
echo "Second field:  $col2 \n";

sqlsrv_free_stmt($stmt);
sqlsrv_close($conn);

?>
```

La sortie attendue est :

```
First field:  1
Second field:  test_data
```

## <a name="see-also"></a> Voir aussi  
[Informations de référence sur l’API du pilote SQLSRV](../../connect/php/sqlsrv-driver-api-reference.md)  

[Procédure : Exécuter des requêtes paramétrables](../../connect/php/how-to-perform-parameterized-queries.md)  

[À propos des exemples de code dans la documentation](../../connect/php/about-code-examples-in-the-documentation.md)  

[Procédure : Envoyer des données sous forme de flux](../../connect/php/how-to-send-data-as-a-stream.md)  

[Utilisation de paramètres directionnels](../../connect/php/using-directional-parameters.md)  

