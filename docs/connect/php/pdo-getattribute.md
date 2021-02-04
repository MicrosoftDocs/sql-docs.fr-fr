---
title: PDO::getAttribute
description: Référence API pour la fonction PDO::getAttribute dans le Pilote Microsoft PDO_SQLSRV pour PHP pour SQL Server.
ms.custom: ''
ms.date: 08/10/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: c81833ea-8b8a-459d-8f24-920098da994d
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ba4e6b81216933ea4367bb4b37feeb61dd646c88
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187615"
---
# <a name="pdogetattribute"></a>PDO::getAttribute
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Récupère la valeur d’un attribut d’un objet PDO ou de pilote prédéfini.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
mixed PDO::getAttribute ( $attribute )  
```  
  
#### <a name="parameters"></a>Paramètres  
*$attribute*: un des attributs pris en charge. Consultez la section Notes pour obtenir la liste des attributs pris en charge.  
  
## <a name="return-value"></a>Valeur de retour  
En cas de réussite, retourne la valeur d’une option de connexion, un attribut PDO prédéfini ou un attribut de pilote personnalisé. En cas d’échec, retourne la valeur Null.  
  
## <a name="remarks"></a>Notes  
Le tableau suivant contient la liste des attributs pris en charge.  
  
|Attribut|Traité par|Valeurs prises en charge|Description|  
|-------------|----------------|--------------------|---------------|  
|PDO::ATTR_CASE|PDO|PDO::CASE_LOWER<br /><br />PDO::CASE_NATURAL<br /><br />PDO::CASE_UPPER|Spécifie si les noms de colonne doivent respecter une casse spécifique. PDO::CASE_LOWER impose des noms de colonne en minuscules, PDO::CASE_NATURAL laisse le nom de colonne tel qu’il est retourné par la base de données et PDO::CASE_UPPER impose des noms de colonne en majuscules.<br /><br />La valeur par défaut est PDO::CASE_NATURAL.<br /><br />Cet attribut peut également être défini à l’aide de PDO::setAttribute.|  
|PDO::ATTR_CLIENT_VERSION|[!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]|Tableau de chaînes|Décrit les versions du pilote et des bibliothèques associées. Retourne un tableau avec les éléments suivants : la version ODBC (*VerMaj*.*VerMin*), le nom et la version de la DLL [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client et la version des [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)] (*VerMaj*.*VerMin*.*NuméroBuild*.*Révision*)|  
|PDO::ATTR_DEFAULT_STR_PARAM|PDO|PDO::PARAM_STR_CHAR<br /><br />PDO::PARAM_STR_NATL|Si non défini sur PDO::PARAM_STR_CHAR, PDO::PARAM_STR_NATL est retourné.|
|PDO::ATTR_DRIVER_NAME|PDO|String|Retourne toujours « sqlsrv ».|  
|PDO::ATTR_DRIVER_VERSION|[!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]|String|Indique la version des [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)] (*VerMaj*.*VerMin*.*NuméroBuild*.*Révision*)|  
|PDO::ATTR_ERRMODE|PDO|PDO::ERRMODE_SILENT<br /><br />PDO::ERRMODE_WARNING<br /><br />PDO::ERRMODE_EXCEPTION|Spécifie comment les échecs doivent être gérés par le pilote.<br /><br />PDO::ERRMODE_SILENT (valeur par défaut) définit les codes d’erreur et les informations.<br /><br />PDO::ERRMODE_WARNING déclenche un E_WARNING.<br /><br />PDO::ERRMODE_EXCEPTION lève une exception.<br /><br />Cet attribut peut également être défini à l’aide de PDO::setAttribute.|  
|PDO::ATTR_ORACLE_NULLS|PDO|Consultez la documentation de PDO.|Consultez la documentation de PDO.|  
|PDO::ATTR_SERVER_INFO|[!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]|Tableau de 3 éléments|Retourne la base de données, la version de SQL Server et l’instance SQL Server actuelles.|  
|PDO::ATTR_SERVER_VERSION|[!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]|String|Indique la version de SQL Server (*VerMaj*.*VerMin*.*NuméroBuild*)|  
|PDO::ATTR_STRINGIFY_FETCHES|PDO|Consultez la documentation de PDO.|Consultez la documentation de PDO.|  
|PDO::SQLSRV_ATTR_CLIENT_BUFFER_MAX_KB_SIZE|[!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]|De 1 jusqu’à la limite de la mémoire PHP.|Configure la taille de la mémoire tampon qui contient le jeu de résultats pour un curseur côté client.<br /><br />La valeur par défaut est 10 240 Ko (10 Mo).<br /><br />Pour plus d’informations sur les curseurs côté client, consultez [Types de curseurs &#40;pilote SQLSRV&#41;](../../connect/php/cursor-types-sqlsrv-driver.md).|  
|PDO::SQLSRV_ATTR_DIRECT_QUERY|[!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]|true<br /><br />false|Spécifie une exécution de requête directe ou préparée. Pour plus d’informations, consultez [Exécution d’instruction directe et exécution d’instruction préparée dans le pilote PDO_SQLSRV](../../connect/php/direct-statement-execution-prepared-statement-execution-pdo-sqlsrv-driver.md).|  
|PDO::SQLSRV_ATTR_ENCODING|[!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]|PDO::SQLSRV_ENCODING_UTF8<br /><br />PDO::SQLSRV_ENCODING_SYSTEM|Spécifie l’encodage de jeu de caractères utilisé par le pilote pour communiquer avec le serveur.<br /><br />La valeur par défaut est PDO::SQLSRV_ENCODING_UTF8.|  
|PDO::SQLSRV_ATTR_FETCHES_NUMERIC_TYPE|[!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]|True ou False|Gère les extractions de nombres à partir de colonnes avec des types SQL numériques (bit, entier, smallint, tinyint, float ou real).<br /><br />Quand l’indicateur d’option de connexion ATTR_STRINGIFY_FETCHES est activé, la valeur de retour est une chaîne, même si SQLSRV_ATTR_FETCHES_NUMERIC_TYPE est activé.<br /><br />Quand le type PDO retourné dans la colonne de liaison est PDO_PARAM_INT, la valeur de retour à partir d’une colonne d’entiers est int, même si SQLSRV_ATTR_FETCHES_NUMERIC_TYPE est désactivé.|  
|PDO::SQLSRV_ATTR_QUERY_TIMEOUT|[!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]|entier|Définit le délai d’expiration de la requête, en secondes.<br /><br />La valeur par défaut est 0, ce qui signifie que le pilote attend indéfiniment les résultats.<br /><br />Les nombres négatifs ne sont pas autorisés.|  

  
PDO traite certains des attributs prédéfinis tandis qu’il a besoin que le pilote en gère d’autres. Tous les attributs et toutes les options de connexion personnalisés sont gérés par le pilote ; un attribut non pris en charge ou une option de connexion retournent la valeur Null.  
  
La prise en charge de PDO a été ajoutée dans la version 2.0 de [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)].  
  
## <a name="example"></a> Exemple  
Cet exemple montre la valeur de l’attribut PDO::ATTR_ERRMODE, avant et après la modification de sa valeur.  
  
```  
<?php  
$database = "AdventureWorks";  
$conn = new PDO( "sqlsrv:server=(local) ; Database = $database", "", "");  
  
$attributes1 = array( "ERRMODE" );  
foreach ( $attributes1 as $val ) {  
     echo "PDO::ATTR_$val: ";  
     var_dump ($conn->getAttribute( constant( "PDO::ATTR_$val" ) ));  
}  
  
$conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );  
  
$attributes1 = array( "ERRMODE" );  
foreach ( $attributes1 as $val ) {  
     echo "PDO::ATTR_$val: ";  
     var_dump ($conn->getAttribute( constant( "PDO::ATTR_$val" ) ));  
}  
  
// An example using PDO::ATTR_CLIENT_VERSION  
print_r($conn->getAttribute( PDO::ATTR_CLIENT_VERSION ));  
?>  
```  
  
## <a name="see-also"></a>Voir aussi  
[PDO, classe](../../connect/php/pdo-class.md)

[PDO](https://php.net/manual/book.pdo.php)  
  
