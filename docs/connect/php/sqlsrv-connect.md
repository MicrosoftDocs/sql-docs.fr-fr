---
title: sqlsrv_connect
description: Crée une ressource de connexion et ouvre une connexion avec le pilote sql_srv pour PHP. Par défaut, la connexion est tentée en utilisant l’authentification Windows.
ms.custom: ''
ms.date: 03/26/2018
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- sqlsrv_connect
apitype: NA
helpviewer_keywords:
- connecting to the server
- API Reference, sqlsrv_connect
- connection pooling support
- sqlsrv_connect
ms.assetid: 37836b49-258e-45ce-9549-b8bd85d6952d
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ea1fdf0513e8ba793425154020cb4dfc837a1045
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195081"
---
# <a name="sqlsrv_connect"></a>sqlsrv_connect
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Crée une ressource de connexion et ouvre une connexion. Par défaut, la connexion est tentée en utilisant l’authentification Windows.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sqlsrv_connect( string $serverName [, array $connectionInfo])  
```  
  
#### <a name="parameters"></a>Paramètres  
*$serverName* : chaîne spécifiant le nom du serveur auquel une connexion est établie. Un nom d’instance (par exemple, « monServeur\nomInstance ») ou un numéro de port (par exemple, « monServeur, 1521 ») peuvent être inclus dans cette chaîne. Pour obtenir la description complète des options disponibles pour ce paramètre, consultez le mot clé Server dans la section Mots clés de chaîne de connexion du pilote ODBC dans [Utilisation de mots clés de chaîne de connexion avec SQL Server Native Client](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md).  
  
À compter de la version 3.0 de [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)], vous pouvez aussi spécifier une instance LocalDB avec `"(localdb)\instancename"`. Pour plus d’informations, consultez [Prise en charge de LocalDB](php-driver-for-sql-server-support-for-localdb.md).  
  
Également depuis la version 3.0 de [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)], vous pouvez spécifier un nom de réseau virtuel, pour établir une connexion à un groupe de disponibilité AlwaysOn. Pour plus d’informations sur la prise en charge de [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)] pour [!INCLUDE[ssHADR](../../includes/sshadr_md.md)], consultez [Prise en charge de la haute disponibilité et de la récupération d'urgence](php-driver-for-sql-server-support-for-high-availability-disaster-recovery.md).  
  
*$connectionInfo* [facultatif] : **tableau** associatif qui contient des attributs de connexion (par exemple, **array**("Database" => "AdventureWorks")). Consultez [Connection Options](connection-options.md) pour obtenir la liste des clés prises en charge pour le tableau.  
  
## <a name="return-value"></a>Valeur de retour  
Ressource de connexion PHP. Si aucune connexion ne peut être créée et ouverte correctement, **false** est retourné.  
  
## <a name="remarks"></a>Notes  
Si les valeurs des clés *UID* et *PWD* ne sont pas spécifiées dans le paramètre facultatif *$connectionInfo* , la connexion est tentée en utilisant l’authentification Windows. Pour plus d’informations sur la connexion au serveur, consultez[Guide pratique pour se connecter à l’aide de l’authentification Windows](how-to-connect-using-windows-authentication.md) et [Guide pratique pour se connecter à l’aide de l’authentification SQL Server](how-to-connect-using-sql-server-authentication.md).  
  
## <a name="example"></a>Exemple  
L’exemple suivant crée et ouvre une connexion en utilisant l’authentification Windows. L’exemple part du principe que SQL Server et la base de données [AdventureWorks](https://www.codeplex.com/SqlServerSamples) sont installés sur l’ordinateur local. Toute la sortie est écrite dans la console quand l’exemple est exécuté à partir de la ligne de commande.  
  
```  
<?php  
/*  
Connect to the local server using Windows Authentication and specify  
the AdventureWorks database as the database in use. To connect using  
SQL Server Authentication, set values for the "UID" and "PWD"  
 attributes in the $connectionInfo parameter. For example:  
$connectionInfo = array("UID" => $uid, "PWD" => $pwd, "Database"=>"AdventureWorks");  
*/  
$serverName = "(local)";  
$connectionInfo = array( "Database"=>"AdventureWorks");  
$conn = sqlsrv_connect( $serverName, $connectionInfo);  
  
if( $conn )  
{  
     echo "Connection established.\n";  
}  
else  
{  
     echo "Connection could not be established.\n";  
     die( print_r( sqlsrv_errors(), true));  
}  
  
//-----------------------------------------------  
// Perform operations with connection.  
//-----------------------------------------------  
  
/* Close the connection. */  
sqlsrv_close( $conn);  
?>  
```  
  
## <a name="see-also"></a>Voir aussi  
[Informations de référence sur l’API du pilote SQLSRV](sqlsrv-driver-api-reference.md)

[Connexion au serveur](connecting-to-the-server.md)

[À propos des exemples de code dans la documentation](about-code-examples-in-the-documentation.md)  
  
