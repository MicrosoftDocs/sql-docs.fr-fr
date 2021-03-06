---
title: Résilience des connexions inactives
description: Découvrez ce qu’est la résilience des connexions inactives et comment elle se comporte dans les pilotes Microsoft pour PHP pour SQL Server.
ms.date: 03/04/2021
ms.prod: sql
ms.prod_service: connectivity
ms.custom: ''
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 07223e87c82a64fe17cd1b2969b5c93227c70af2
ms.sourcegitcommit: 0bcda4ce24de716f158a3b652c9c84c8f801677a
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2021
ms.locfileid: "102247485"
---
# <a name="idle-connection-resiliency"></a>Résilience des connexions inactives
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

La [résilience de connexion](../odbc/windows/connection-resiliency-in-the-windows-odbc-driver.md) est le principe selon lequel une connexion inactive interrompue peut être rétablie, dans certaines limites. Si la connexion à Microsoft SQL Server échoue, la résilience de connexion permet au client de tenter automatiquement de rétablir la connexion. La résilience de connexion est une propriété de la source de données ; seuls SQL Server 2014 (et versions ultérieures) et Azure SQL Database prennent en charge la résilience de connexion.

La résilience de connexion s’implémente en ajoutant deux mots clés de connexion aux chaînes de connexion : **ConnectRetryCount** et **ConnectRetryInterval**.

|Mot clé|Valeurs|Default|Description|
|-|-|-|-|
|**ConnectRetryCount**| Entier compris entre 0 et 255 (inclus)|1|Nombre maximal de tentatives de rétablissement d’une connexion interrompue avant abandon. Par défaut, une seule tentative est effectuée pour rétablir une connexion interrompue. La valeur 0 signifie qu’aucune reconnexion n’est tentée.|
|**ConnectRetryInterval**| Entier compris entre 1 et 60 (inclus)|10| Durée, en secondes, entre les tentatives de rétablissement d’une connexion. L’application tente de se reconnecter immédiatement en cas de détection d’une connexion interrompue, puis attend **ConnectRetryInterval** secondes avant de réessayer. Ce mot clé est ignoré si **ConnectRetryCount** est égal à 0.

Si le produit de **ConnectRetryCount** et de **ConnectRetryInterval** est supérieur à **LoginTimeout**, le client cesse d’essayer de se connecter une fois **LoginTimeout** atteint. Sinon, il continuera à tenter de se reconnecter jusqu’à ce que **ConnectRetryCount** soit atteint.

#### <a name="remarks"></a>Notes

La résilience de connexion s’applique lorsque la connexion est inactive. Les défaillances qui se produisent lors de l’exécution d’une transaction, par exemple, ne déclencheront pas de tentatives de reconnexion. Elles échoueront normalement. Les situations suivantes, appelées états de session non récupérables, ne déclenchent pas de tentatives de reconnexion :

* tables temporaires ;
* Curseurs globaux et locaux
* Verrous de transaction au niveau du contexte de transaction et de la session
* Verrous d’applications
* Contexte de sécurité EXECUTE AS/REVERT
* Handles d’automatisation OLE
* Handles XML préparés
* Indicateurs de trace

## <a name="example"></a>Exemple

Le code suivant se connecte à une base de données et exécute une requête. La connexion est interrompue en arrêtant la session et une nouvelle requête est tentée à l’aide de la connexion interrompue. Cet exemple utilise l’exemple de base de données [AdventureWorks](/previous-versions/sql/sql-server-2008/ms124501(v=sql.100)).

Dans cet exemple, nous spécifions un curseur mis en mémoire tampon avant d’interrompre la connexion. Sinon, la connexion ne serait pas rétablie, car il y aurait un curseur actif côté serveur : la connexion ne serait donc pas inactive en cas d’interruption. Toutefois, nous appellerions dans ce cas sqlsrv_free_stmt() avant d’interrompre la connexion pour libérer le curseur, et la connexion serait correctement rétablie.

```php
<?php
// This function breaks the connection by determining its session ID and killing it.
// A separate connection is used to break the main connection because a session
// cannot kill itself. The sleep() function ensures enough time has passed for KILL
// to finish ending the session.
function BreakConnection( $conn, $conn_break )
{
    $stmt1 = sqlsrv_query( $conn, "SELECT @@SPID" );
    if ( sqlsrv_fetch( $stmt1 ) )
    {
        $spid=sqlsrv_get_field( $stmt1, 0 );
    }

    $stmt2 = sqlsrv_prepare( $conn_break, "KILL ".$spid );
    sqlsrv_execute( $stmt2 );
    sleep(1);
}

// Connect to the local server using Windows authentication and specify
// AdventureWorks as the database in use. Specify values for
// ConnectRetryCount and ConnectRetryInterval as well.
$databaseName = 'AdventureWorks2014';
$serverName = '(local)';
$connectionInfo = array( "Database"=>$databaseName, "ConnectRetryCount"=>10, "ConnectRetryInterval"=>10 );

$conn = sqlsrv_connect( $serverName, $connectionInfo );
if( $conn === false)  
{  
     echo "Could not connect.\n";  
     die( print_r( sqlsrv_errors(), true));  
}

// A separate connection that will be used to break the main connection $conn
$conn_break = sqlsrv_connect( $serverName, array( "Database"=>$databaseName) );

// Create a statement to retrieve the contents of a table
$stmt1 = sqlsrv_query( $conn, "SELECT * FROM HumanResources.Employee",
                       array(), array( "Scrollable"=>"buffered" ) );
if( $stmt1 === false )
{
     echo "Error in statement 1.\n";
     die( print_r( sqlsrv_errors(), true ));
}
else
{
    echo "Statement 1 successful.\n";
    $rowcount = sqlsrv_num_rows( $stmt1 );
    echo $rowcount." rows in result set.\n";
}

// Now break the connection $conn
BreakConnection( $conn, $conn_break );

// Create another statement. The connection will be reestablished.
$stmt2 = sqlsrv_query( $conn, "SELECT * FROM HumanResources.Department",
                       array(), array( "Scrollable"=>"buffered" ) );
if( $stmt2 === false )
{
     echo "Error in statement 2.\n";
     die( print_r( sqlsrv_errors(), true ));
}
else
{
    echo "Statement 2 successful.\n";
    $rowcount = sqlsrv_num_rows( $stmt2 );
    echo $rowcount." rows in result set.\n";
}

sqlsrv_close( $conn );
sqlsrv_close( $conn_break );
?>
```
Sortie attendue :
```
Statement 1 successful.
290 rows in result set.
Statement 2 successful.
16 rows in result set.
```

## <a name="see-also"></a>Voir aussi
[Résilience de connexion du pilote ODBC Windows](../odbc/windows/connection-resiliency-in-the-windows-odbc-driver.md)
