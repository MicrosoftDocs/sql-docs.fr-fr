---
description: sqlsrv_close
title: sqlsrv_close | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- sqlsrv_close
apitype: NA
helpviewer_keywords:
- sqlsrv_close
- API Reference, sqlsrv_close
ms.assetid: 6ac6209c-a134-4f8f-b88b-8eefaa1cbc7f
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 16dfb43ebc8a35ef1a3e6016092f67e328d1eda5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99190715"
---
# <a name="sqlsrv_close"></a>sqlsrv_close
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Ferme la connexion spécifiée et libère les ressources associées.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sqlsrv_close( resource $conn )  
```  
  
#### <a name="parameters"></a>Paramètres  
*$conn*: connexion à fermer.  
  
## <a name="return-value"></a>Valeur de retour  
Valeur booléenne **true** sauf si la fonction est appelée avec un paramètre non valide. Si la fonction est appelée avec un paramètre non valide, **false** est retourné.  
  
> [!NOTE]  
> **Null** est un paramètre valide pour cette fonction. Ainsi, la fonction peut être appelée plusieurs fois dans un script. Par exemple, si vous fermez une connexion dans une condition d’erreur et que vous la refermez à la fin du script, le deuxième appel à **sqlsrv_close** va retourner **true**, car le premier appel à **sqlsrv_close** (dans la condition d’erreur) affecte la valeur **null** à la ressource de connexion.  
  
## <a name="example"></a>Exemple  
L’exemple suivant ferme une connexion. L’exemple part du principe que SQL Server est installé sur l’ordinateur local. Toute la sortie est écrite dans la console quand l’exemple est exécuté à partir de la ligne de commande.  
  
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
  
//-----------------------------------------------  
// Perform operations with connection here.  
//-----------------------------------------------  
  
/* Close the connection. */  
sqlsrv_close( $conn);  
echo "Connection closed.\n";  
?>  
```  
  
## <a name="see-also"></a>Voir aussi  
[Informations de référence sur l’API du pilote SQLSRV](../../connect/php/sqlsrv-driver-api-reference.md)

[À propos des exemples de code dans la documentation](../../connect/php/about-code-examples-in-the-documentation.md)  
  
