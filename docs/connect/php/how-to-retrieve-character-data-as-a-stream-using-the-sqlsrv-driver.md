---
title: Récupérer des données caractères sous la forme d’un flux avec le pilote SQLSRV
description: Cette rubrique explique comment récupérer des données caractères sous la forme d’un flux lors de l’utilisation de Microsoft SQLSRV Driver pour PHP pour SQL Server
ms.custom: ''
ms.date: 08/10/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- retrieving data, as a character stream
- streaming data
ms.assetid: 3c0dbca4-abfc-4449-b133-66c819681840
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 82a766e3f593b9f6ed49c2606703af2cdf717794
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100038409"
---
# <a name="how-to-retrieve-character-data-as-a-stream-using-the-sqlsrv-driver"></a>Procédure : Récupérer des données caractères sous la forme d’un flux à l’aide du pilote SQLSRV
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

La récupération de données sous forme de flux est disponible uniquement dans le pilote SQLSRV du [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)], et non dans le pilote PDO_SQLSRV.  
  
Le pilote SQLSRV tire parti des flux PHP pour récupérer de grandes quantités de données à partir du serveur. L’exemple de cette rubrique montre comment récupérer des données caractères sous la forme d’un flux.  
  
## <a name="example"></a> Exemple  
L’exemple suivant récupère une ligne de la table *Production.ProductReview* de la base de données AdventureWorks. Le champ *Comments* de la ligne retournée est récupéré sous la forme d’un flux et affiché à l’aide de la fonction [fpassthru](https://php.net/manual/function.fpassthru.php) PHP.  
  
La récupération de données sous la forme d’un flux s’effectue à l’aide de [sqlsrv_fetch](../../connect/php/sqlsrv-fetch.md) et [sqlsrv_get_field](../../connect/php/sqlsrv-get-field.md) avec le type de retour spécifié en tant que flux de caractères. Le type de retour est spécifié à l’aide de la constante **SQLSRV_PHPTYPE_STREAM**. Pour plus d’informations sur les constantes **sqlsrv**, consultez [Constantes &#40;Microsoft Drivers for PHP for SQL Server&#41;](../../connect/php/constants-microsoft-drivers-for-php-for-sql-server.md).  
  
L’exemple part du principe que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et la base de données [AdventureWorks](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works) sont installés sur l’ordinateur local. Toute la sortie est écrite dans la console quand l’exemple est exécuté à partir de la ligne de commande.  
  
```  
<?php  
/*Connect to the local server using Windows Authentication and  
specify the AdventureWorks database as the database in use. */  
$serverName = "(local)";  
$connectionInfo = array( "Database"=>"AdventureWorks");  
$conn = sqlsrv_connect( $serverName, $connectionInfo);  
if( $conn === false )  
{  
     echo "Could not connect.\n";  
     die( print_r( sqlsrv_errors(), true));  
}  
  
/* Set up the Transact-SQL query. */  
$tsql = "SELECT ReviewerName,   
               CONVERT(varchar(32), ReviewDate, 107) AS [ReviewDate],  
               Rating,   
               Comments   
         FROM Production.ProductReview   
         WHERE ProductReviewID = ? ";  
  
/* Set the parameter value. */  
$productReviewID = 1;  
$params = array( $productReviewID);  
  
/* Execute the query. */  
$stmt = sqlsrv_query($conn, $tsql, $params);  
if( $stmt === false )  
{  
     echo "Error in statement execution.\n";  
     die( print_r( sqlsrv_errors(), true));  
}  
  
/* Retrieve and display the data. The first three fields are retrieved  
as strings and the fourth as a stream with character encoding. */  
if(sqlsrv_fetch( $stmt ) === false )  
{  
     echo "Error in retrieving row.\n";  
     die( print_r( sqlsrv_errors(), true));  
}  
  
echo "Name: ".sqlsrv_get_field( $stmt, 0 )."\n";  
echo "Date: ".sqlsrv_get_field( $stmt, 1 )."\n";  
echo "Rating: ".sqlsrv_get_field( $stmt, 2 )."\n";  
echo "Comments: ";  
$comments = sqlsrv_get_field( $stmt, 3,   
                             SQLSRV_PHPTYPE_STREAM(SQLSRV_ENC_CHAR));  
fpassthru($comments);  
  
/* Free the statement and connection resources. */  
sqlsrv_free_stmt( $stmt);  
sqlsrv_close( $conn);  
?>  
```  
  
Étant donné qu’aucun type de retour PHP n’est spécifié pour les trois premiers champs, chaque champ est retourné selon son type PHP par défaut. Pour plus d’informations sur les types de données PHP par défaut, consultez [Default PHP Data Types](../../connect/php/default-php-data-types.md). Pour plus d’informations sur la spécification des types de retour PHP, consultez [How to: Specify PHP Data Types](../../connect/php/how-to-specify-php-data-types.md).  
  
## <a name="see-also"></a>Voir aussi  
[Récupération de données](../../connect/php/retrieving-data.md)

[Récupération des données sous la forme d’un flux à l’aide du pilote SQLSRV](../../connect/php/retrieving-data-as-a-stream-using-the-sqlsrv-driver.md)

[À propos des exemples de code dans la documentation](../../connect/php/about-code-examples-in-the-documentation.md)  
  
