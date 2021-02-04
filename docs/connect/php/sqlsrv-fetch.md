---
description: sqlsrv_fetch
title: sqlsrv_fetch | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- sqlsrv_fetch
apitype: NA
helpviewer_keywords:
- sqlsrv_fetch
- API Reference, sqlsrv_fetch
- retrieving data, as a single field
ms.assetid: a5a640a1-6e7d-452e-8b66-850a4dc2ce89
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 1ab164eb61bcde26862d47c544e366a296117e5a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99154390"
---
# <a name="sqlsrv_fetch"></a>sqlsrv_fetch
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Rend la ligne suivante d’un jeu de résultats disponible pour la lecture. Utilisez [sqlsrv_get_field](../../connect/php/sqlsrv-get-field.md) pour lire les champs de la ligne.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sqlsrv_fetch( resource $stmt[, row[, ]offset])  
```  
  
#### <a name="parameters"></a>Paramètres  
*$stmt* : ressource d’instruction correspondant à une instruction exécutée.  
  
> [!NOTE]  
> Une instruction doit être exécutée avant que les résultats puissent être récupérés. Pour plus d’informations sur l’exécution d’une instruction, consultez [sqlsrv_query](../../connect/php/sqlsrv-query.md) et [sqlsrv_execute](../../connect/php/sqlsrv-execute.md).  
  
*ligne* [FACULTATIF] : L’une des valeurs suivantes, spécifiant la ligne à laquelle accéder dans un jeu de résultats qui utilise un curseur à défilement :  
  
-   SQLSRV_SCROLL_NEXT  
  
-   SQLSRV_SCROLL_PRIOR  
  
-   SQLSRV_SCROLL_FIRST  
  
-   SQLSRV_SCROLL_LAST  
  
-   SQLSRV_SCROLL_ABSOLUTE  
  
-   SQLSRV_SCROLL_RELATIVE  
  
Pour plus d’informations sur ces valeurs, consultez [Spécification d’un type de curseur et sélection de lignes](../../connect/php/specifying-a-cursor-type-and-selecting-rows.md).  
  
*décaler* [FACULTATIF] : Utilisé avec SQLSRV_SCROLL_ABSOLUTE et SQLSRV_SCROLL_RELATIVE pour spécifier la ligne à récupérer. Le premier enregistrement dans le jeu de résultats est 0.  
  
## <a name="return-value"></a>Valeur de retour  
Si la ligne suivante du jeu de résultats a été récupérée avec succès, la valeur **true** est retournée. S’il n’existe plus aucun résultat dans le jeu de résultats, la valeur **null** est retournée. Si une erreur s’est produite, la valeur **false** est retournée.  
  
## <a name="example"></a> Exemple  
L’exemple suivant utilise **sqlsrv_fetch** pour récupérer une ligne de données contenant une évaluation de produit et le nom du réviseur. Pour récupérer les données du jeu de résultats, on utilise [sqlsrv_get_field](../../connect/php/sqlsrv-get-field.md). L’exemple part du principe que SQL Server et la base de données [AdventureWorks](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works) sont installés sur l’ordinateur local. Toute la sortie est écrite dans la console quand l’exemple est exécuté à partir de la ligne de commande.  
  
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
Comments are of SQL Server type nvarchar. */  
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
if( sqlsrv_fetch( $stmt ) === false)  
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
while( !feof( $stream ))  
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
[Récupération de données](../../connect/php/retrieving-data.md)  

[Informations de référence sur l’API du pilote SQLSRV](../../connect/php/sqlsrv-driver-api-reference.md)  

[À propos des exemples de code dans la documentation](../../connect/php/about-code-examples-in-the-documentation.md)  
  
