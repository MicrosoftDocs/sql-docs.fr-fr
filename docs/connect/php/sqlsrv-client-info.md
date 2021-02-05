---
title: sqlsrv_client_info
description: Référence API pour la fonction sqlsrv_client_info dans Microsoft SQLSRV Driver pour PHP pour SQL Server.
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- sqlsrv_client_info
apitype: NA
helpviewer_keywords:
- API Reference, sqlsrv_client_info
- sqlsrv_client_info
ms.assetid: 3e2d3679-436a-45d8-8bdc-7c633b65a720
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 041df13402ce55106474d3b8595df12dcc576c52
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99190724"
---
# <a name="sqlsrv_client_info"></a>sqlsrv_client_info
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Retourne des informations sur la pile de connexion et de client.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sqlsrv_client_info( resource $conn)  
```  
  
#### <a name="parameters"></a>Paramètres  
*$conn*: ressource de connexion par laquelle le client est connecté.  
  
## <a name="return-value"></a>Valeur de retour  
Tableau associatif avec les clés décrites dans le tableau ci-dessous, ou **false** si la ressource de connexion est Null.  
  
**Pour PHP pour SQL Server versions 3.2 et 3.1**:  
  
|Clé|Description|  
|-------|---------------|  
|DriverDllName|MSODBCSQL11.DLL (ODBC Driver 11 for SQL Server)|  
|DriverODBCVer|Version ODBC (xx.yy)|  
|DriverVer|Version de la DLL ODBC Driver 11 for SQL Server :<br /><br />xx.yy.zzzz ([!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)] version 3.2 ou 3.1)|  
|ExtensionVer|Version php_sqlsrv.dll :<br /><br />3.2.xxxx.x (pour [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)] version 3.2)<br /><br />3.1.xxxx.x (pour [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)] version 3.1)|  
  
**Pour PHP pour SQL Server versions 3.0 et 2.0**:  
  
|Clé|Description|  
|-------|---------------|  
|DriverDllName|SQLNCLI10.DLL ([!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)] version 2.0)|  
|DriverODBCVer|Version ODBC (xx.yy)|  
|DriverVer|Version de la DLL SQL Server Native Client :<br /><br />10.50.xxx ([!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)] version 2.0)|  
|ExtensionVer|Version php_sqlsrv.dll :<br /><br />2.0.xxxx.x ([!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)] version 2.0)|  
  
## <a name="example"></a> Exemple  
L’exemple suivant écrit des informations de client dans la console quand l’exemple est exécuté à partir de la ligne de commande. L’exemple part du principe que SQL Server est installé sur l’ordinateur local. Toute la sortie est écrite dans la console quand l’exemple est exécuté à partir de la ligne de commande.  
  
```  
<?php  
/*Connect to the local server using Windows Authentication and   
specify the AdventureWorks database as the database in use. */  
$serverName = "(local)";  
$conn = sqlsrv_connect( $serverName);  
  
if( $conn === false )  
{  
     echo "Could not connect.\n";  
     die( print_r( sqlsrv_errors(), true));  
}  
  
if( $client_info = sqlsrv_client_info( $conn))  
{  
       foreach( $client_info as $key => $value)  
      {  
              echo $key.": ".$value."\n";  
      }  
}  
else  
{  
       echo "Client info error.\n";  
}  
  
/* Close connection resources. */  
sqlsrv_close( $conn);  
?>  
```  
  
## <a name="see-also"></a>Voir aussi  
[Informations de référence sur l’API du pilote SQLSRV](../../connect/php/sqlsrv-driver-api-reference.md)

[À propos des exemples de code dans la documentation](../../connect/php/about-code-examples-in-the-documentation.md)  
  
