---
title: sqlsrv_prepare
description: Référence API pour la fonction sqlsrv_prepare dans Microsoft SQLSRV Driver pour PHP pour SQL Server.
ms.custom: ''
ms.date: 04/11/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- sqlsrv_prepare
apitype: NA
helpviewer_keywords:
- executing queries
- API Reference, sqlsrv_prepare
- sqlsrv_prepare
ms.assetid: 8c74c697-3296-4f5d-8fb9-e361f53f19a6
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 566b5804a042e96bb18c37c5b4405178bd8899f0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201085"
---
# <a name="sqlsrv_prepare"></a>sqlsrv_prepare
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Crée une ressource d’instruction associée à la connexion spécifiée. Cette fonction est utile pour exécuter plusieurs requêtes.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sqlsrv_prepare(resource $conn, string $tsql [, array $params [, array $options]])  
```  
  
#### <a name="parameters"></a>Paramètres  
*$conn*: ressource de connexion associée à l’instruction créée.  
  
*$tsql* : expression Transact-SQL qui correspond à l’instruction créée.  
  
*$params* [FACULTATIF] : objet **array** de valeurs qui correspondent aux paramètres d’une requête paramétrable. Chaque élément du tableau peut être l’un des éléments suivants :
  
-   Une valeur littérale  
  
-   Référence à une variable PHP  
  
-   Un objet **array** avec la structure suivante :  
  
    ```  
    array(&$value [, $direction [, $phpType [, $sqlType]]])  
    ```  
  
    > [!NOTE]  
    > Les variables passées en tant que paramètres de requête doivent être passées par référence plutôt que par valeur. Par exemple, passez `&$myVariable` plutôt que `$myVariable`. Un avertissement PHP est déclenché quand une requête avec des paramètres par valeur est exécutée.  
  
    Le tableau suivant décrit ces éléments de tableau :  
  
    |Élément|Description|  
    |-----------|---------------|  
    |*&$value*|Valeur littérale ou référence à une variable PHP.|  
    |*$direction*[FACULTATIF]|Une des constantes **SQLSRV_PARAM_\* *_ utilisées pour indiquer la direction du paramètre : _* SQLSRV_PARAM_IN**, **SQLSRV_PARAM_OUT**, **SQLSRV_PARAM_INOUT**. La valeur par défaut est **SQLSRV_PARAM_IN**.<br /><br />Pour plus d’informations sur les constantes PHP, consultez [Constantes &#40;Microsoft Drivers for PHP for SQL Server&#41;](../../connect/php/constants-microsoft-drivers-for-php-for-sql-server.md).|  
    |*$phpType*[FACULTATIF]|Constante **SQLSRV_PHPTYPE_\** _ qui spécifie le type de données PHP de la valeur retournée.|  
    |_$sqlType*[FACULTATIF]|Constante **SQLSRV_SQLTYPE_\** _ qui spécifie le type de données SQL Server de la valeur d’entrée.|  
  
_$options* [FACULTATIF] : tableau associatif qui définit les <a name="properties">propriétés de la requête</a>. Le tableau suivant liste les clés prises en charge et les valeurs correspondantes :

|Clé|Valeurs prises en charge|Description|  
|-------|--------------------|---------------|  
|ClientBufferMaxKBSize|Entier positif|Configure la taille de la mémoire tampon qui contient le jeu de résultats pour un curseur côté client.<br /><br />La valeur par défaut est 10 240 Ko. Pour plus d’informations, voir [Spécifier un type de curseur et sélectionner des lignes](../../connect/php/specifying-a-cursor-type-and-selecting-rows.md).|
|DecimalPlaces|Entier compris entre 0 et 4 (inclus)|Spécifie le nombre de décimales pour le formatage des valeurs monétaires extraites.<br /><br />Les entiers négatifs et les valeurs supérieures à 4 sont ignorés.<br /><br />Cette option fonctionne seulement si FormatDecimals est **true**.|
|FormatDecimals|**true** ou **false**<br /><br />La valeur par défaut est **false**.|Spécifie s’il faut ajouter des zéros au début des chaînes décimales si nécessaire et active l’option `DecimalPlaces` pour mettre en forme les types monétaires.<br /><br />Pour plus d’informations, voir [Mettre en forme des chaînes décimales et des valeurs monétaires (pilote SQLSRV)](../../connect/php/formatting-decimals-sqlsrv-driver.md).|
|QueryTimeout|Entier positif|Définit le délai d’expiration de la requête, en secondes. Par défaut, le pilote attend les résultats indéfiniment.|  
|ReturnDatesAsStrings|**true** ou **false**<br /><br />La valeur par défaut est **false**.|Configure l’instruction de façon à récupérer les types date et heure sous forme de chaînes (**true**). Pour plus d’informations, lisez [Procédure : récupérer des types de date et heure sous forme de chaînes à l’aide du pilote SQLSRV](../../connect/php/how-to-retrieve-date-and-time-type-as-strings-using-the-sqlsrv-driver.md).
|Défilement|SQLSRV_CURSOR_FORWARD<br /><br />SQLSRV_CURSOR_STATIC<br /><br />SQLSRV_CURSOR_DYNAMIC<br /><br />SQLSRV_CURSOR_KEYSET<br /><br />SQLSRV_CURSOR_CLIENT_BUFFERED|Pour plus d’informations sur ces valeurs, consultez [Spécification d’un type de curseur et sélection de lignes](../../connect/php/specifying-a-cursor-type-and-selecting-rows.md).|  
|SendStreamParamsAtExec|**true** ou **false**<br /><br />La valeur par défaut est **true**.|Configure le pilote pour envoyer toutes les données de flux au moment de l’exécution (**true**), ou pour les envoyer par blocs (**false**). La valeur par défaut est **true**. Pour plus d’informations, consultez [sqlsrv_send_stream_data](../../connect/php/sqlsrv-send-stream-data.md).|  
  
## <a name="return-value"></a>Valeur de retour  
Ressource d’instruction. Si la ressource d’instruction ne peut pas être créée, la valeur **false** est retournée.  
  
## <a name="remarks"></a>Notes  
Quand vous préparez une instruction qui utilise des variables comme paramètres, les variables sont liées à l’instruction. Cela signifie que si vous mettez à jour les valeurs des variables, la prochaine fois que vous exécuterez l’instruction elle s’exécutera avec les valeurs de paramètres mises à jour.  
  
La combinaison de **sqlsrv_prepare** et **sqlsrv_execute** sépare la préparation et l’exécution de l’instruction en deux appels de fonction. De plus, elle peut être utilisée pour exécuter des requêtes paramétrables. Cette fonction est idéale pour exécuter une instruction plusieurs fois avec différentes valeurs de paramètres pour chaque exécution.  
  
Pour découvrir d’autres stratégies d’écriture et de lecture de grandes quantités d’informations, consultez [Batches of SQL Statements](../../odbc/reference/develop-app/batches-of-sql-statements.md) et [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md).  
  
Pour plus d’informations, consultez [Procédure : Récupérer des paramètres de sortie à l’aide du pilote SQLSRV](../../connect/php/how-to-retrieve-output-parameters-using-the-sqlsrv-driver.md).  
  
## <a name="example-1"></a>Exemple 1  
L’exemple suivant prépare et exécute une instruction. L’instruction, quand elle est exécutée (voir [sqlsrv_execute](../../connect/php/sqlsrv-execute.md)), met à jour un champ dans la table *Sales.SalesOrderDetail* de la base de données AdventureWorks. L’exemple part du principe que SQL Server et la base de données [AdventureWorks](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works) sont installés sur l’ordinateur local. Toute la sortie est écrite dans la console quand l’exemple est exécuté à partir de la ligne de commande.  
  
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
  
/* Set up Transact-SQL query. */  
$tsql = "UPDATE Sales.SalesOrderDetail   
         SET OrderQty = ?   
         WHERE SalesOrderDetailID = ?";  
  
/* Assign parameter values. */  
$param1 = 5;  
$param2 = 10;  
$params = array(&$param1, &$param2);  
  
/* Prepare the statement. */  
if ($stmt = sqlsrv_prepare($conn, $tsql, $params)) {
    echo "Statement prepared.\n";  
} else {  
    echo "Statement could not be prepared.\n";  
    die(print_r(sqlsrv_errors(), true));  
}  
  
/* Execute the statement. */  
if (sqlsrv_execute($stmt)) {  
    echo "Statement executed.\n";  
} else {  
    echo "Statement could not be executed.\n";  
    die(print_r(sqlsrv_errors(), true));  
}  
  
/* Free the statement and connection resources. */  
sqlsrv_free_stmt($stmt);  
sqlsrv_close($conn);  
?>  
```  
  
## <a name="example-2"></a>Exemple 2  
L’exemple suivant montre comment préparer une instruction puis la réexécuter avec des valeurs de paramètres différentes. L’exemple met à jour la colonne *OrderQty* de la table *Sales.SalesOrderDetail* dans la base de données AdventureWorks. Une fois les mises à jour effectuées, la base de données est interrogée pour vérifier que les mises à jour ont réussi. L’exemple part du principe que SQL Server et la base de données [AdventureWorks](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works) sont installés sur l’ordinateur local.  Toute la sortie est écrite dans la console quand l’exemple est exécuté à partir de la ligne de commande.  
  
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
  
/* Define the parameterized query. */  
$tsql = "UPDATE Sales.SalesOrderDetail  
         SET OrderQty = ?  
         WHERE SalesOrderDetailID = ?";  
  
/* Initialize parameters and prepare the statement. Variables $qty  
and $id are bound to the statement, $stmt1. */  
$qty = 0; $id = 0;  
$stmt1 = sqlsrv_prepare($conn, $tsql, array(&$qty, &$id));  
if ($stmt1) {  
    echo "Statement 1 prepared.\n";  
} else {  
    echo "Error in statement preparation.\n";  
    die(print_r(sqlsrv_errors(), true));  
}  
  
/* Set up the SalesOrderDetailID and OrderQty information. This array  
maps the order ID to order quantity in key=>value pairs. */  
$orders = array(1=>10, 2=>20, 3=>30);  
  
/* Execute the statement for each order. */  
foreach ($orders as $id => $qty) {  
    // Because $id and $qty are bound to $stmt1, their updated  
    // values are used with each execution of the statement.   
    if (sqlsrv_execute($stmt1) === false) {  
        echo "Error in statement execution.\n";  
        die(print_r(sqlsrv_errors(), true));  
    }  
}  
echo "Orders updated.\n";  
  
/* Free $stmt1 resources.  This allows $id and $qty to be bound to a different statement.*/  
sqlsrv_free_stmt($stmt1);  
  
/* Now verify that the results were successfully written by selecting   
the newly inserted rows. */  
$tsql = "SELECT OrderQty   
         FROM Sales.SalesOrderDetail   
         WHERE SalesOrderDetailID = ?";  
  
/* Prepare the statement. Variable $id is bound to $stmt2. */  
$stmt2 = sqlsrv_prepare($conn, $tsql, array(&$id));  
if ($stmt2) {  
    echo "Statement 2 prepared.\n";  
} else {  
    echo "Error in statement preparation.\n";  
    die(print_r(sqlsrv_errors(), true));  
}  
  
/* Execute the statement for each order. */  
foreach (array_keys($orders) as $id)  
{  
    /* Because $id is bound to $stmt2, its updated value   
    is used with each execution of the statement. */  
    if (sqlsrv_execute($stmt2)) {  
        sqlsrv_fetch($stmt2);  
        $quantity = sqlsrv_get_field($stmt2, 0);  
        echo "Order $id is for $quantity units.\n";  
    } else {  
        echo "Error in statement execution.\n";  
        die(print_r(sqlsrv_errors(), true));  
    }  
}  
  
/* Free $stmt2 and connection resources. */  
sqlsrv_free_stmt($stmt2);  
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
$stmt = sqlsrv_prepare($conn, "INSERT INTO TestTable (DecimalCol) VALUES (?)", $params);
sqlsrv_execute($stmt);

sqlsrv_free_stmt($stmt);  
sqlsrv_close($conn);  

?>
```

## <a name="see-also"></a>Voir aussi  
[Informations de référence sur l’API du pilote SQLSRV](../../connect/php/sqlsrv-driver-api-reference.md)

[Procédure : Exécuter des requêtes paramétrables](../../connect/php/how-to-perform-parameterized-queries.md)

[À propos des exemples de code dans la documentation](../../connect/php/about-code-examples-in-the-documentation.md)

[Procédure : Envoyer des données sous forme de flux](../../connect/php/how-to-send-data-as-a-stream.md)

[Utilisation de paramètres directionnels](../../connect/php/using-directional-parameters.md)

[Récupération de données](../../connect/php/retrieving-data.md)

[Mise à jour des données &#40;pilotes Microsoft SQL Server pour PHP&#41;](../../connect/php/updating-data-microsoft-drivers-for-php-for-sql-server.md)