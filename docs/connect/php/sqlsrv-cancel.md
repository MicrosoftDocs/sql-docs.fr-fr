---
description: sqlsrv_cancel
title: sqlsrv_cancel | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- sqlsrv_cancel
apitype: NA
helpviewer_keywords:
- sqlsrv_cancel
- API Reference, sqlsrv_cancel
ms.assetid: 75798c9b-f711-445d-9b8f-ba4d405ca50a
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 0d93f74d4b2df35e83ad4fbd72f0b0dcbd91e216
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99190265"
---
# <a name="sqlsrv_cancel"></a>sqlsrv_cancel
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Annule une instruction. Cela signifie que les résultats de l’instruction en attente sont ignorés. Une fois que cette fonction a été appelée, l’instruction peut être réexécutée si elle a été préparée avec [sqlsrv_prepare](../../connect/php/sqlsrv-prepare.md). L’appel de cette fonction n’est pas nécessaire si tous les résultats associés à l’instruction ont été consommés.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sqlsrv_cancel( resource $stmt)  
```  
  
#### <a name="parameters"></a>Paramètres  
*$stmt*: instruction à annuler.  
  
## <a name="return-value"></a>Valeur de retour  
Valeur booléenne : **true** si l’opération a réussi. Dans le cas contraire, la valeur est **false**.  
  
## <a name="example"></a> Exemple  
L’exemple suivant cible la base de données [AdventureWorks](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works) pour exécuter une requête, puis consomme et compte les résultats jusqu’à ce que la variable *$salesTotal* atteigne le montant spécifié. Les autres résultats de requête sont alors ignorés. L’exemple part du principe que SQL Server et la base de données AdventureWorks sont installés sur l’ordinateur local.  Toute la sortie est écrite dans la console quand l’exemple est exécuté à partir de la ligne de commande.  
  
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
  
/* Prepare and execute the query. */  
$tsql = "SELECT OrderQty, UnitPrice FROM Sales.SalesOrderDetail";  
$stmt = sqlsrv_prepare( $conn, $tsql);  
if( $stmt === false )  
{  
     echo "Error in statement preparation.\n";  
     die( print_r( sqlsrv_errors(), true));  
}  
if( sqlsrv_execute( $stmt ) === false)  
{  
     echo "Error in statement execution.\n";  
     die( print_r( sqlsrv_errors(), true));  
}  
  
/* Initialize tracking variables. */  
$salesTotal = 0;  
$count = 0;  
  
/* Count and display the number of sales that produce revenue  
of $100,000. */  
while( ($row = sqlsrv_fetch_array( $stmt)) && $salesTotal <=100000)  
{  
     $qty = $row[0];  
     $price = $row[1];  
     $salesTotal += ( $price * $qty);  
     $count++;  
}  
echo "$count sales accounted for the first $$salesTotal in revenue.\n";  
  
/* Cancel the pending results. The statement can be reused. */  
sqlsrv_cancel( $stmt);  
?>  
```  
  
## <a name="comments"></a>Commentaires  
Une instruction préparée et exécutée à l’aide de la combinaison de [sqlsrv_prepare](../../connect/php/sqlsrv-prepare.md) et [sqlsrv_execute](../../connect/php/sqlsrv-execute.md) peut être exécutée avec **sqlsrv_execute** après l’appel à **sqlsrv_cancel**. Une instruction exécutée avec [sqlsrv_query](../../connect/php/sqlsrv-query.md) ne peut pas être réexécutée après l’appel à **sqlsrv_cancel**.  
  
## <a name="see-also"></a>Voir aussi  
[Informations de référence sur l’API du pilote SQLSRV](../../connect/php/sqlsrv-driver-api-reference.md)

[Connexion au serveur](../../connect/php/connecting-to-the-server.md)

[Récupération de données](../../connect/php/retrieving-data.md)

[À propos des exemples de code dans la documentation](../../connect/php/about-code-examples-in-the-documentation.md)

[sqlsrv_free_stmt](../../connect/php/sqlsrv-free-stmt.md)

  
