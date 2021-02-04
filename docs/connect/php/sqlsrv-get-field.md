---
description: sqlsrv_get_field
title: sqlsrv_get_field | Microsoft Docs
ms.custom: ''
ms.date: 06/26/2018
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- sqlsrv_get_field
apitype: NA
helpviewer_keywords:
- sqlsrv_get_field
- API Reference, sqlsrv_get_field
- retrieving data, as a single field
ms.assetid: fa17cc56-fb38-433b-a40d-65642f04dc23
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 3fd330fdfc43ae11f4958418afc1504a0e44502f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99154053"
---
# <a name="sqlsrv_get_field"></a>sqlsrv_get_field
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Récupère les données du champ spécifié de la ligne actuelle. Vous devez accéder aux données de champ dans l’ordre. Par exemple, les données du premier champ ne sont pas accessibles une fois que vous avez accédé aux données du deuxième champ.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
sqlsrv_get_field( resource $stmt, int $fieldIndex [, int $getAsType])  
```  
  
#### <a name="parameters"></a>Paramètres  
*$stmt* : ressource d’instruction correspondant à une instruction exécutée.  
  
*$fieldIndex*: index du champ à récupérer. Les index commencent à zéro.  
  
*$getAsType* [FACULTATIF] : constante **SQLSRV** (**SQLSRV_PHPTYPE_&#x2a;**)qui détermine le type de données PHP des données retournées. Pour plus d’informations sur les types de données pris en charge, consultez [Constantes &#40;Pilotes Microsoft SQL Server pour PHP&#41;](../../connect/php/constants-microsoft-drivers-for-php-for-sql-server.md). Si aucun type de retour n’est spécifié, un type PHP par défaut est retourné. Pour plus d’informations sur les types PHP par défaut, consultez [Default PHP Data Types](../../connect/php/default-php-data-types.md). Pour plus d’informations sur la spécification des types de données PHP, consultez [How to: Specify PHP Data Types](../../connect/php/how-to-specify-php-data-types.md).  
  
## <a name="return-value"></a>Valeur de retour  
Données du champ. Vous pouvez spécifier le type de données PHP des données retournées à l’aide du paramètre *$getAsType* . Si aucun type de données de retour n’est spécifié, le type de données PHP par défaut est retourné. Pour plus d’informations sur les types PHP par défaut, consultez [Default PHP Data Types](../../connect/php/default-php-data-types.md). Pour plus d’informations sur la spécification des types de données PHP, consultez [How to: Specify PHP Data Types](../../connect/php/how-to-specify-php-data-types.md).  
  
## <a name="remarks"></a>Notes  
La combinaison de **sqlsrv_fetch** et de **sqlsrv_get_field** fournit un accès avant uniquement aux données.  
  
La combinaison **sqlsrv_fetch**/**sqlsrv_get_field** charge un seul champ d’une ligne de jeu de résultats dans la mémoire de script et permet la spécification du type de retour PHP. (Pour plus d’informations sur la façon de spécifier le type de retour PHP, consultez [Procédure : spécifier des types de données PHP](../../connect/php/how-to-specify-php-data-types.md).) Cette combinaison de fonctions permet également aux données d’être récupérées sous la forme d’un flux. (Pour plus d’informations sur la récupération de données sous la forme d’un flux, consultez [Récupération des données sous la forme d’un flux à l’aide du pilote SQLSRV](../../connect/php/retrieving-data-as-a-stream-using-the-sqlsrv-driver.md).)  
  
## <a name="example"></a> Exemple  
L’exemple suivant récupère une ligne de données contenant une évaluation de produit et le nom de son auteur. Pour récupérer les données du jeu de résultats, on utilise **sqlsrv_get_field**. L’exemple part du principe que SQL Server et la base de données [AdventureWorks](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works) sont installés sur l’ordinateur local. Toute la sortie est écrite dans la console quand l’exemple est exécuté à partir de la ligne de commande.  
  
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
  
/* Set up and execute the query. Note that both ReviewerName and  
Comments are of the SQL Server nvarchar type. */  
$tsql = "SELECT ReviewerName, Comments   
         FROM Production.ProductReview  
         WHERE ProductReviewID=1";  
$stmt = sqlsrv_query( $conn, $tsql);  
if( $stmt === false )  
{  
     echo "Error in statement preparation/execution.\n";  
     die( print_r( sqlsrv_errors(), true));  
}  
  
/* Make the first row of the result set available for reading. */  
if( sqlsrv_fetch( $stmt ) === false )  
{  
     echo "Error in retrieving row.\n";  
     die( print_r( sqlsrv_errors(), true));  
}  
  
/* Note: Fields must be accessed in order.  
Get the first field of the row. Note that no return type is  
specified. Data will be returned as a string, the default for  
a field of type nvarchar.*/  
$name = sqlsrv_get_field( $stmt, 0);  
echo "$name: ";  
  
/*Get the second field of the row as a stream.  
Because the default return type for a nvarchar field is a  
string, the return type must be specified as a stream. */  
$stream = sqlsrv_get_field( $stmt, 1,   
                            SQLSRV_PHPTYPE_STREAM( SQLSRV_ENC_CHAR));  
while( !feof( $stream))  
{   
    $str = fread( $stream, 10000);  
    echo $str;  
}  
  
/* Free the statement and connection resources. */  
sqlsrv_free_stmt( $stmt);  
sqlsrv_close( $conn);  
?>  
```  
  
## <a name="see-also"></a>Voir aussi  
[Informations de référence sur l’API du pilote SQLSRV](../../connect/php/sqlsrv-driver-api-reference.md)  

[Récupération de données](../../connect/php/retrieving-data.md)  

[À propos des exemples de code dans la documentation](../../connect/php/about-code-examples-in-the-documentation.md)  
  
