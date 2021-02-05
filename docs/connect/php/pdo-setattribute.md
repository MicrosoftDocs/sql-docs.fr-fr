---
title: PDO::setAttribute
description: Référence API pour la fonction PDO::setAttribute dans le Pilote Microsoft PDO_SQLSRV pour PHP pour SQL Server.
ms.custom: ''
ms.date: 08/10/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 56f9ee96-e1d2-46cc-b137-38f06a251863
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ac1b72ea564df077ae7edb0f771becd9b76cda7b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195239"
---
# <a name="pdosetattribute"></a>PDO::setAttribute
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Définit un attribut PDO prédéfini ou un attribut de pilote personnalisé.  
  
## <a name="syntax"></a>Syntaxe  
  
```
bool PDO::setAttribute ( $attribute, $value );  
```  
  
#### <a name="parameters"></a>Paramètres  
*$attribute*: attribut à définir. Consultez la section Notes pour obtenir la liste des attributs pris en charge.  
  
*$value*: valeur (type mixte).  
  
## <a name="return-value"></a>Valeur de retour  
Retourne true en cas de réussite ; sinon, false.  
  
## <a name="remarks"></a>Notes  
  
|Attribut|Traité par|Valeurs prises en charge|Description|  
|-------------|----------------|--------------------|---------------|  
|PDO::ATTR_CASE|PDO|PDO::CASE_LOWER<br /><br />PDO::CASE_NATURAL<br /><br />PDO::CASE_UPPER|Spécifie la casse des noms de colonne.<br /><br />PDO::CASE_LOWER impose des noms de colonnes en minuscules.<br /><br />PDO::CASE_NATURAL (valeur par défaut) affiche les noms de colonne tels qu’ils sont retournés par la base de données.<br /><br />PDO::CASE_UPPER impose des noms de colonne en majuscules.<br /><br />Cet attribut peut être défini à l’aide de PDO::setAttribute.|  
|PDO::ATTR_DEFAULT_FETCH_MODE|PDO|Consultez la documentation de PDO.|Consultez la documentation de PDO.|  
|PDO::ATTR_DEFAULT_STR_PARAM|PDO|PDO::PARAM_STR_CHAR<br /><br />PDO::PARAM_STR_NATL|Pour plus d’informations, consultez les exemples de [PDO::quote](../../connect/php/pdo-quote.md).|
|PDO::ATTR_ERRMODE|PDO|PDO::ERRMODE_SILENT<br /><br />PDO::ERRMODE_WARNING<br /><br />PDO::ERRMODE_EXCEPTION|Spécifie comment le pilote signale les échecs.<br /><br />PDO::ERRMODE_SILENT (valeur par défaut) définit les codes d’erreur et les informations.<br /><br />PDO::ERRMODE_WARNING déclenche E_WARNING.<br /><br />PDO::ERRMODE_EXCEPTION entraîne la levée d’une exception.<br /><br />Cet attribut peut être défini à l’aide de PDO::setAttribute.|  
|PDO::ATTR_ORACLE_NULLS|PDO|Consultez la documentation de PDO.|Spécifie comment les valeurs Null doivent être retournées.<br /><br />PDO::NULL_NATURAL n’effectue aucune conversion.<br /><br />PDO::NULL_EMPTY_STRING convertit une chaîne vide en valeur Null.<br /><br />PDO::NULL_TO_STRING convertit la valeur Null en chaîne vide.|  
|PDO::ATTR_STATEMENT_CLASS|PDO|Consultez la documentation de PDO.|Définit la classe d’instruction fournie par l’utilisateur dérivée de PDOStatement.<br /><br />Exige `array(string classname, array(mixed constructor_args))`.<br /><br />Pour plus d’informations, consultez la documentation de PDO.|  
|PDO::ATTR_STRINGIFY_FETCHES|PDO|True ou False|Convertit les valeurs numériques en chaînes lors de la récupération des données.|  
|PDO::SQLSRV_ATTR_CLIENT_BUFFER_MAX_KB_SIZE|[!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]|De 1 jusqu’à la limite de la mémoire PHP.|Définit la taille de la mémoire tampon qui contient le jeu de résultats lors de l’utilisation d’un curseur côté client.<br /><br />La valeur par défaut est 10 240 Ko, si elle n’est pas spécifiée dans le fichier php.ini.<br /><br />Zéro et les nombres négatifs ne sont pas autorisés.<br /><br />Pour plus d’informations sur les requêtes qui créent un curseur côté client, consultez [Types de curseur &#40;pilote PDO_SQLSRV&#41;](../../connect/php/cursor-types-pdo-sqlsrv-driver.md).|  
|PDO::SQLSRV_ATTR_DECIMAL_PLACES|[!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]|Entier compris entre 0 et 4 (inclus)|Spécifie le nombre de décimales pour la mise en forme des valeurs monétaires extraites.<br /><br />Les entiers négatifs et les valeurs supérieures à 4 sont ignorés.<br /><br />Cette option fonctionne uniquement quand PDO::SQLSRV_ATTR_FORMAT_DECIMALS a la valeur true.<br /><br />Cette option peut également être définie au niveau de l’instruction. Dans ce cas, l’option au niveau de l’instruction remplace celle-ci.<br /><br />Pour plus d’informations, consultez [Mise en forme des chaînes décimales et valeurs monétaires (pilote PDO_SQLSR)](../../connect/php/formatting-decimals-pdo-sqlsrv-driver.md).|
|PDO::SQLSRV_ATTR_DIRECT_QUERY|[!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]|True ou False|Spécifie une exécution de requête directe ou préparée. Pour plus d’informations, consultez [Exécution d’instruction directe et exécution d’instruction préparée dans le pilote PDO_SQLSRV](../../connect/php/direct-statement-execution-prepared-statement-execution-pdo-sqlsrv-driver.md).|  
|PDO::SQLSRV_ATTR_ENCODING|[!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]|PDO::SQLSRV_ENCODING_UTF8<br /><br />PDO::SQLSRV_ENCODING_SYSTEM.|Définit l’encodage de jeu de caractères utilisé par le pilote pour communiquer avec le serveur.<br /><br />PDO::SQLSRV_ENCODING_BINARY n’est pas pris en charge.<br /><br />La valeur par défaut est PDO::SQLSRV_ENCODING_UTF8.|  
|PDO::SQLSRV_ATTR_FETCHES_DATETIME_TYPE|[!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]|True ou False|Spécifie s’il faut récupérer les types de date et d’heure en tant qu’objets [DateTime PHP](http://php.net/manual/en/class.datetime.php). Si vous conservez la valeur false, le comportement par défaut consiste à les retourner sous forme de chaînes.<br /><br />Cette option peut également être définie au niveau de l’instruction. Dans ce cas, l’option au niveau de l’instruction remplace celle-ci.<br /><br />Pour plus d’informations, consultez [Procédure : Récupérer des types date et heure sous forme d’objets datetime PHP à l’aide du pilote PDO_SQLSRV](../../connect/php/how-to-retrieve-datetime-objects-using-pdo-sqlsrv-driver.md).|  
|PDO::SQLSRV_ATTR_FETCHES_NUMERIC_TYPE|[!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]|True ou False|Gère les extractions de nombres à partir de colonnes avec des types SQL numériques (bit, entier, smallint, tinyint, float ou real).<br /><br />Quand l’indicateur d’option de connexion ATTR_STRINGIFY_FETCHES est activé, la valeur de retour est une chaîne, même si SQLSRV_ATTR_FETCHES_NUMERIC_TYPE est activé.<br /><br />Quand le type PDO retourné dans la colonne de liaison est PDO_PARAM_INT, la valeur de retour à partir d’une colonne d’entiers est int, même si SQLSRV_ATTR_FETCHES_NUMERIC_TYPE est désactivé.|  
|PDO::SQLSRV_ATTR_FORMAT_DECIMALS|[!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]|True ou False|Spécifie s’il faut ajouter des zéros au début des chaînes décimales si nécessaire. Si elle est définie, cette option active l’option PDO::SQLSRV_ATTR_DECIMAL_PLACES pour mettre en forme les types monétaires. Si vous conservez la valeur false, le comportement par défaut qui consiste à retourner la précision exacte et à omettre les zéros non significatifs pour les valeurs inférieures à 1 est utilisé.<br /><br />Cette option peut également être définie au niveau de l’instruction. Dans ce cas, l’option au niveau de l’instruction remplace celle-ci.<br /><br />Pour plus d’informations, consultez [Mise en forme des chaînes décimales et valeurs monétaires (pilote PDO_SQLSR)](../../connect/php/formatting-decimals-pdo-sqlsrv-driver.md).| 
|PDO::SQLSRV_ATTR_QUERY_TIMEOUT|[!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]|entier|Définit le délai d’expiration de la requête, en secondes.<br /><br />La valeur par défaut est 0, ce qui signifie que le pilote attend indéfiniment les résultats.<br /><br />Les nombres négatifs ne sont pas autorisés.|  
  
PDO traite certains des attributs prédéfinis et exige que le pilote en traite d’autres. Tous les attributs personnalisés et toutes les options de connexion sont traités par le pilote. Un attribut, une option de connexion ou une valeur qui ne sont pas pris en charge sont signalés selon le paramètre de PDO::ATTR_ERRMODE.  
  
La prise en charge de PDO a été ajoutée dans la version 2.0 de [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)].  
  
## <a name="example"></a> Exemple  
Cet exemple montre comment définir l’attribut PDO::ATTR_ERRMODE.  
  
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
?>  
```  
  
## <a name="see-also"></a>Voir aussi  
[PDO, classe](../../connect/php/pdo-class.md)

[PDO](https://php.net/manual/book.pdo.php)  
  
