---
title: Regroupement de connexions (Microsoft Drivers for PHP for SQL Server)
description: Découvrez les détails du regroupement des connexions lorsque vous utilisez les Pilotes Microsoft pour PHP pour SQL Server et comment l’expérience peut varier en fonction de votre système d’exploitation.
ms.custom: ''
ms.date: 08/01/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- connection pooling support
ms.assetid: 4d9a83d4-08de-43a1-975c-0a94005edc94
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 7aa190aca90d72a2d99c260acba85056c850e334
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100038489"
---
# <a name="connection-pooling-microsoft-drivers-for-php-for-sql-server"></a>Regroupement de connexions (Microsoft Drivers for PHP for SQL Server)
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Voici des points importants à noter sur le regroupement de connexions dans [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]:  
  
-   [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)] utilise le regroupement de connexions ODBC.  
  
-   Par défaut, le regroupement de connexions est activé dans Windows. Dans Linux et macOS, les connexions sont regroupées uniquement si le regroupement de connexions est activé pour ODBC (voir [Activation/désactivation du regroupement de connexions](#enablingdisabling-connection-pooling)). Quand le regroupement de connexions est activé et que vous vous connectez à un serveur, le pilote tente d’utiliser une connexion regroupée avant d’en créer une nouvelle. Si une connexion équivalente est introuvable dans le regroupement, une nouvelle connexion est créée et ajoutée à ce regroupement. Le pilote détermine si les connexions sont équivalentes, par comparaison avec des chaînes de connexion.  
  
-   Quand une connexion du regroupement est utilisée, l’état de la connexion est réinitialisé (Windows uniquement).  
  
-   La fermeture de la connexion retourne la connexion au regroupement.  
  
Pour plus d’informations sur le regroupement de connexions, consultez [Regroupement de connexions du Gestionnaire de pilotes](../../odbc/reference/develop-app/driver-manager-connection-pooling.md).  
  
## <a name="enablingdisabling-connection-pooling"></a>Activation/désactivation du regroupement de connexions
### <a name="windows"></a>Windows
Vous pouvez forcer le pilote à créer une connexion (au lieu de rechercher une connexion équivalente dans le pool de connexions) en affectant à l’attribut *ConnectionPooling* de la chaîne de connexion la valeur **false** (ou 0).  
  
Si l’attribut *ConnectionPooling* est omis de la chaîne de connexion ou si sa valeur est **true** (ou 1), le pilote crée uniquement une connexion si aucune connexion équivalente n’existe dans le pool de connexions.  

> [!NOTE]  
> MARS (Multiple Active Result Sets) est désactivé par défaut. Lorsque MARS et le regroupement sont utilisés, pour que MARS fonctionne correctement, le pilote nécessite plus de temps pour réinitialiser la connexion sur la *première* requête, ignorant ainsi tout délai d’attente de requête spécifié. Toutefois, le paramètre du délai d’attente de la requête prendra effet dans les requêtes suivantes.
  
Si nécessaire, vérifiez [Procédure : Désactiver MARS (Multiple Active Resultsets)](../../connect/php/how-to-disable-multiple-active-resultsets-mars.md). Pour plus d’informations sur les autres attributs de connexion, consultez [Connection Options](../../connect/php/connection-options.md).  

### <a name="linux-and-macos"></a>Linux et macOS
L’attribut *ConnectionPooling* ne peut pas être utilisé pour activer/désactiver le regroupement de connexions. 

Le regroupement de connexions peut être activé/désactivé en modifiant le fichier de configuration odbcinst.ini. Le pilote doit être rechargé pour que les modifications soient prises en compte.

La définition de `Pooling` sur `Yes` et une valeur de `CPTimeout` positive dans le fichier odbcinst. ini active le regroupement de connexions. 
```
[ODBC]
Pooling=Yes

[ODBC Driver 17 for SQL Server]
CPTimeout=<int value>
```
  
Au minimum, le fichier odbcinst.ini devrait ressembler à l’exemple suivant :

```
[ODBC]
Pooling=Yes

[ODBC Driver 17 for SQL Server]
Description=Microsoft ODBC Driver 17 for SQL Server
Driver=/opt/microsoft/msodbcsql17/lib64/libmsodbcsql-17.5.so.2.1
UsageCount=1
CPTimeout=120
```

La définition de `Pooling` sur `No` dans le fichier odbcinst.ini force le pilote à créer une nouvelle connexion.
```
[ODBC]
Pooling=No
```

## <a name="remarks"></a>Notes
- Sous Linux ou macOS, le regroupement des connexions n’est pas recommandé avec unixODBC < 2.3.7. Toutes les connexions sont regroupées si le regroupement est activé dans le fichier Odbcinst.ini, ce qui signifie que l’option de connexion ConnectionPooling n’a aucun effet. Pour désactiver le regroupement, définissez Pooling=No dans le fichier odbcinst.ini et rechargez les pilotes. 
  - unixODBC <= 2.3.4 (Linux et macOS) risque de ne pas renvoyer les informations de diagnostic appropriées, notamment les messages d’erreur, les avertissements et les messages informatifs
  - Pour cette raison, les pilotes SQLSRV et PDO_SQLSRV risquent de ne pas récupérer correctement les données de type Long (XML, binaires, par exemple) sous forme de chaînes. Pour contourner le problème, les données de type Long peuvent être extraites en tant que flux de données. Consultez l’exemple ci-dessous pour SQLSRV.

```
<?php
$connectionInfo = array("Database"=>"test", "UID"=>"username", "PWD"=>"password");

$conn1 = sqlsrv_connect("servername", $connectionInfo);

$longSample = str_repeat("a", 8500);
$xml1 = 
'<ParentXMLTag>
  <ChildTag01>'.$longSample.'</ChildTag01>
</ParentXMLTag>';

// Create table and insert xml string into it
sqlsrv_query($conn1, "CREATE TABLE xml_table (field xml)");
sqlsrv_query($conn1, "INSERT into xml_table values ('$xml1')");

// retrieve the inserted xml
$column1 = getColumn($conn1);

// return the connection to the pool
sqlsrv_close($conn1);

// This connection is from the pool
$conn2 = sqlsrv_connect("servername", $connectionInfo);
$column2 = getColumn($conn2);

sqlsrv_query($conn2, "DROP TABLE xml_table");
sqlsrv_close($conn2);

function getColumn($conn)
{
    $tsql = "SELECT * from xml_table";
    $stmt = sqlsrv_query($conn, $tsql);
    sqlsrv_fetch($stmt);
    // This might fail in Linux and macOS
    // $column = sqlsrv_get_field($stmt, 0, SQLSRV_PHPTYPE_STRING(SQLSRV_ENC_CHAR));
    // The workaround is to fetch it as a stream
    $column = sqlsrv_get_field($stmt, 0, SQLSRV_PHPTYPE_STREAM(SQLSRV_ENC_CHAR));
    sqlsrv_free_stmt($stmt);
    return ($column);
}
?>
```


## <a name="see-also"></a>Voir aussi  
[Procédure : Se connecter avec l’authentification Windows](../../connect/php/how-to-connect-using-windows-authentication.md)

[Procédure : Se connecter avec l’authentification SQL Server](../../connect/php/how-to-connect-using-sql-server-authentication.md)  
  
