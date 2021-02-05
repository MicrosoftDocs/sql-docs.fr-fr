---
description: sqlsrv_begin_transaction
title: sqlsrv_begin_transaction | Microsoft Docs
ms.custom: ''
ms.date: 03/26/2018
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- sqlsrv_begin_transaction
apitype: NA
helpviewer_keywords:
- sqlsrv_begin_transaction
- transaction support
- API Reference, sqlsrv_begin_transaction
ms.assetid: 0b223bc8-4047-4329-9cbf-d350ab0fb886
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 8b64d0d37ca716d11a9b1a945480fe8a19d238dc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99190277"
---
# <a name="sqlsrv_begin_transaction"></a>sqlsrv_begin_transaction
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Commence une transaction sur une connexion spécifiée. La transaction actuelle contient toutes les instructions sur la connexion spécifiée qui ont été exécutées après l’appel à **sqlsrv_begin_transaction** et avant tous les appels à [sqlsrv_rollback](../../connect/php/sqlsrv-rollback.md) ou [sqlsrv_commit](../../connect/php/sqlsrv-commit.md).  
  
> [!NOTE]  
> Le [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)] est en mode de validation automatique par défaut. Cela signifie que toutes les requêtes sont validées automatiquement en cas de réussite, sauf si elles ont été désignées comme faisant partie d’une transaction explicite à l’aide de **sqlsrv_begin_transaction**.  
  
> [!NOTE]  
> Si **sqlsrv_begin_transaction** est appelé après le lancement d’une transaction sur la connexion, qui n’est pas achevée par un appel à **sqlsrv_commit** ou **sqlsrv_rollback**, l’appel retourne **false** et une erreur *Already in Transaction* est ajoutée à la collection d’erreurs.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sqlsrv_begin_transaction( resource $conn)  
```  
  
#### <a name="parameters"></a>Paramètres  
*$conn*: connexion avec laquelle la transaction est associée.  
  
## <a name="return-value"></a>Valeur de retour  
Valeur booléenne : **true** si la transaction a été correctement commencée. Dans le cas contraire, la valeur est **false**.  
  
## <a name="example"></a> Exemple  
L’exemple ci-après exécute deux requêtes dans le cadre d’une transaction. Si les deux requêtes réussissent, la transaction est validée. Si l’une des requêtes (ou les deux) échoue, la transaction est annulée.  
  
La première requête de l’exemple insère une nouvelle commande dans la table *Sales.SalesOrderDetail* de la base de données AdventureWorks. La commande concerne cinq unités du produit dont l’ID est 709. La seconde requête réduit de cinq unités la quantité de stock du produit dont l’ID est 709. Ces requêtes sont incluses dans une transaction, car elles doivent toutes les deux réussir pour que la base de données reflète avec exactitude l’état des commandes et la disponibilité du produit.  
  
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
     die( print_r( sqlsrv_errors(), true ));  
}  
  
/* Initiate transaction. */  
/* Exit script if transaction cannot be initiated. */  
if ( sqlsrv_begin_transaction( $conn ) === false )  
{  
     echo "Could not begin transaction.\n";  
     die( print_r( sqlsrv_errors(), true ));  
}  
  
/* Initialize parameter values. */  
$orderId = 43659; $qty = 5; $productId = 709;  
$offerId = 1; $price = 5.70;  
  
/* Set up and execute the first query. */  
$tsql1 = "INSERT INTO Sales.SalesOrderDetail   
                     (SalesOrderID,   
                      OrderQty,   
                      ProductID,   
                      SpecialOfferID,   
                      UnitPrice)  
          VALUES (?, ?, ?, ?, ?)";  
$params1 = array( $orderId, $qty, $productId, $offerId, $price);  
$stmt1 = sqlsrv_query( $conn, $tsql1, $params1 );  
  
/* Set up and execute the second query. */  
$tsql2 = "UPDATE Production.ProductInventory   
          SET Quantity = (Quantity - ?)   
          WHERE ProductID = ?";  
$params2 = array($qty, $productId);  
$stmt2 = sqlsrv_query( $conn, $tsql2, $params2 );  
  
/* If both queries were successful, commit the transaction. */  
/* Otherwise, rollback the transaction. */  
if( $stmt1 && $stmt2 )  
{  
     sqlsrv_commit( $conn );  
     echo "Transaction was committed.\n";  
}  
else  
{  
     sqlsrv_rollback( $conn );  
     echo "Transaction was rolled back.\n";  
}  
  
/* Free statement and connection resources. */  
sqlsrv_free_stmt( $stmt1);  
sqlsrv_free_stmt( $stmt2);  
sqlsrv_close( $conn);  
?>  
```  
  
Pour mieux mettre l’accent sur le comportement de la transaction, aucune recommandation en matière de gestion des erreurs n’est incluse dans l’exemple ci-dessus. Pour une application de production, nous vous recommandons de vérifier s’il existe des erreurs et de les gérer en conséquence dans tout appel à la fonction **sqlsrv** .  
  
> [!NOTE]  
> N’utilisez pas Transact-SQL incorporé pour effectuer des transactions. Par exemple, n’exécutez pas une instruction avec « BEGIN TRANSACTION » en tant que requête Transact-SQL pour commencer une transaction. Le comportement transactionnel attendu ne peut pas être garanti durant l’utilisation d’une requête Transact-SQL incorporée pour effectuer des transactions.  
  
## <a name="see-also"></a>Voir aussi  
[Informations de référence sur l’API du pilote SQLSRV](../../connect/php/sqlsrv-driver-api-reference.md)

[Procédure : Effectuer des transactions](../../connect/php/how-to-perform-transactions.md)

[Vue d’ensemble de Microsoft Drivers for PHP for SQL Server](../../connect/php/overview-of-the-php-sql-driver.md) 
  
