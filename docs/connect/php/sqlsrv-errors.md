---
title: sqlsrv_errors
description: Référence API pour la fonction sqlsrv_errors dans Microsoft SQLSRV Driver pour PHP pour SQL Server.
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- sqlsrv_errors
apitype: NA
helpviewer_keywords:
- API Reference, sqlsrv_errors
- sqlsrv_errors
- errors and warnings
ms.assetid: d1fcffec-f34f-46de-9a0e-343f3b5dbae2
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a74b89b93f924401d18372a04ab65180f2cbde8e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194214"
---
# <a name="sqlsrv_errors"></a>sqlsrv_errors
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Retourne des informations étendues sur les erreurs et/ou les avertissements concernant la dernière opération **sqlsrv** effectuée.  
  
La fonction **sqlsrv_errors** peut retourner des informations d’erreur et/ou d’avertissement en les appelant avec l’une des valeurs de paramètre spécifiées dans la section Paramètres ci-dessous.  
  
Par défaut, les avertissements générés sur un appel à une fonction **sqlsrv** sont traités comme des erreurs ; si un avertissement se produit sur un appel à une fonction **sqlsrv** , la fonction retourne false. Toutefois, les avertissements qui correspondent aux valeurs SQLSTATE 01000, 01001, 01003 et 01S02 ne sont jamais traités comme des erreurs.  
  
La ligne de code suivante désactive le comportement mentionné ci-dessus ; un avertissement généré par un appel à une fonction **sqlsrv** n’amène pas la fonction à retourner false :  
  
```  
sqlsrv_configure("WarningsReturnAsErrors", 0);  
```  
  
La ligne de code suivante rétablit le comportement par défaut ; les avertissements (avec les exceptions mentionnées ci-dessus) sont traités comme des erreurs :  
  
```  
sqlsrv_configure("WarningsReturnAsErrors", 1);  
```  
  
Quel que soit le paramètre, les avertissements peuvent uniquement être récupérés en appelant **sqlsrv_errors** avec la valeur de paramètre **SQLSRV_ERR_ALL** ou **SQLSRV_ERR_WARNINGS** (consultez la section Paramètres ci-dessous pour plus d’informations).  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sqlsrv_errors( [int $errorsAndOrWarnings] )  
```  
  
#### <a name="parameters"></a>Paramètres  
*$errorsAndOrWarnings*[FACULTATIF] : constante prédéfinie. Ce paramètre peut prendre l’une des valeurs répertoriées dans le tableau suivant :  
  
|Valeur|Description|  
|---------|---------------|  
|SQLSRV_ERR_ALL|Les erreurs et avertissements générés sur le dernier appel de fonction **sqlsrv** sont retournés.|  
|SQLSRV_ERR_ERRORS|Les erreurs générées sur le dernier appel de fonction **sqlsrv** sont retournées.|  
|SQLSRV_ERR_WARNINGS|Les avertissements générés sur le dernier appel de fonction **sqlsrv** sont retournés.|  
  
Si aucune valeur de paramètre n’est fournie, les erreurs et avertissements générés par le dernier appel de fonction **sqlsrv** sont retournés.  
  
## <a name="return-value"></a>Valeur de retour  
**Tableau** de tableaux ou **Null**. Chaque **tableau** inclus dans le **tableau** retourné contient trois paires clé-valeur. Le tableau suivant répertorie chaque clé et sa description :  
  
|Clé|Description|  
|-------|---------------|  
|SQLSTATE|Pour les erreurs qui proviennent du pilote ODBC, valeur SQLSTATE retournée par ODBC. Pour plus d’informations sur les valeurs SQLSTATE pour ODBC, consultez [Codes d’erreur ODBC](../../odbc/reference/appendixes/appendix-a-odbc-error-codes.md).<br /><br />Pour les erreurs qui proviennent de [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)], valeur SQLSTATE d’IMSSP.<br /><br />Pour les avertissements qui proviennent de [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)], valeur SQLSTATE de 01SSP.|  
|code|Pour les erreurs qui proviennent de SQL Server, code d’erreur SQL Server natif.<br /><br />Pour les erreurs qui proviennent du pilote ODBC, code d’erreur retourné par ODBC.<br /><br />Pour les erreurs qui proviennent de [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)], code d’erreur [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)] . Pour plus d’informations, consultez [Handling Errors and Warnings](../../connect/php/handling-errors-and-warnings.md).|  
|message|Description de l’erreur.|  
  
Les valeurs de tableau sont également accessibles avec des clés numériques 0, 1 et 2. Si aucune erreur ou aucun avertissement ne se produisent, **Null** est retourné.  
  
## <a name="example"></a> Exemple  
L’exemple suivant affiche les erreurs qui se produisent pendant une exécution d’instruction qui a échoué. (l’instruction échoue, car **InvalidColumName** n’est pas un nom de colonne valide dans la table spécifiée). L’exemple part du principe que SQL Server et la base de données [AdventureWorks](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works) sont installés sur l’ordinateur local. Toute la sortie est écrite dans la console quand l’exemple est exécuté à partir de la ligne de commande.  
  
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
  
/* Set up a query to select an invalid column name. */  
$tsql = "SELECT InvalidColumnName FROM Sales.SalesOrderDetail";  
  
/* Attempt execution. */  
/* Execution will fail because of the invalid column name. */  
$stmt = sqlsrv_query( $conn, $tsql);  
if( $stmt === false )  
{  
      if( ($errors = sqlsrv_errors() ) != null)  
      {  
         foreach( $errors as $error)  
         {  
            echo "SQLSTATE: ".$error[ 'SQLSTATE']."\n";  
            echo "code: ".$error[ 'code']."\n";  
            echo "message: ".$error[ 'message']."\n";  
         }  
      }  
}  
  
/* Free connection resources */  
sqlsrv_close( $conn);  
?>  
```  
  
## <a name="see-also"></a>Voir aussi  
[Informations de référence sur l’API du pilote SQLSRV](../../connect/php/sqlsrv-driver-api-reference.md)

[À propos des exemples de code dans la documentation](../../connect/php/about-code-examples-in-the-documentation.md)  
  
