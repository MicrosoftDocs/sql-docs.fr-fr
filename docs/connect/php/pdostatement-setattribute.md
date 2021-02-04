---
title: PDOStatement::setAttribute
description: Référence API pour la fonction PDOStatement::setAttribute dans le Pilote Microsoft PDO_SQLSRV pour PHP pour SQL Server.
ms.custom: ''
ms.date: 08/10/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 329d9b5e-1c5d-40b0-9127-1051d0646fc7
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d557da4c19a6a4825c9b83c8e8c0387431615456
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199316"
---
# <a name="pdostatementsetattribute"></a>PDOStatement::setAttribute
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Définit une valeur d’attribut, à savoir un attribut PDO prédéfini ou un attribut de pilote personnalisé.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
bool PDOStatement::setAttribute ($attribute, $value );  
```  
  
#### <a name="parameters"></a>Paramètres  
$*attribute* : entier, une des constantes PDO::ATTR_* ou PDO::SQLSRV_ATTR_\*. Consultez la section Notes pour obtenir la liste des attributs disponibles.  
  
$*valeur* : valeur (mixte) à définir pour le $*attribute* spécifié.  
  
## <a name="return-value"></a>Valeur de retour  
TRUE en cas de réussite, FALSE dans le cas contraire.  
  
## <a name="remarks"></a>Notes  
Le tableau suivant contient la liste des attributs disponibles :  
  
|Attribut|Valeurs|Description|  
|-------------|----------|---------------|  
|PDO::SQLSRV_ATTR_CLIENT_BUFFER_MAX_KB_SIZE|De 1 jusqu’à la limite de la mémoire PHP.|Configure la taille de la mémoire tampon qui contient le jeu de résultats pour un curseur côté client.<br /><br />La valeur par défaut est 10 240 Ko (10 Mo).<br /><br />Pour plus d’informations sur les curseurs côté client, consultez [Types de curseurs &#40;pilote PDO_SQLSRV&#41;](../../connect/php/cursor-types-pdo-sqlsrv-driver.md).|  
|PDO::SQLSRV_ATTR_DATA_CLASSIFICATION|True ou False|Spécifie s’il faut récupérer les métadonnées de classification des données lors de l’appel de [PDOStatement::getColumnMeta](../../connect/php/pdostatement-getcolumnmeta.md). La valeur par défaut est false.|
|PDO::SQLSRV_ATTR_DECIMAL_PLACES|Entier compris entre 0 et 4 (inclus)|Spécifie le nombre de décimales pour la mise en forme des valeurs monétaires extraites.<br /><br />Les entiers négatifs et les valeurs supérieures à 4 sont ignorés.<br /><br />Cette option fonctionne uniquement quand PDO::SQLSRV_ATTR_FORMAT_DECIMALS a la valeur true.<br /><br />Vous pouvez aussi définir cette option au niveau de la connexion. Dans ce cas, cette option remplace l’option au niveau de la connexion.<br /><br />Pour plus d’informations, consultez [Mise en forme des chaînes décimales et valeurs monétaires (pilote PDO_SQLSR)](../../connect/php/formatting-decimals-pdo-sqlsrv-driver.md).|
|PDO::SQLSRV_ATTR_ENCODING|Integer<br /><br />PDO::SQLSRV_ENCODING_UTF8 (valeur par défaut)<br /><br />PDO::SQLSRV_ENCODING_SYSTEM<br /><br />PDO::SQLSRV_ENCODING_BINARY|Définit l’encodage de jeu de caractères utilisé par le pilote pour communiquer avec le serveur.|  
|PDO::SQLSRV_ATTR_FETCHES_DATETIME_TYPE|True ou False|Spécifie s’il faut récupérer les types de date et d’heure en tant qu’objets [DateTime PHP](http://php.net/manual/en/class.datetime.php). Si vous conservez la valeur false, le comportement par défaut consiste à les retourner sous forme de chaînes.<br /><br />Vous pouvez aussi définir cette option au niveau de la connexion. Dans ce cas, cette option remplace l’option au niveau de la connexion.<br /><br />Pour plus d’informations, consultez [Procédure : Récupérer des types date et heure sous forme d’objets datetime PHP à l’aide du pilote PDO_SQLSRV](../../connect/php/how-to-retrieve-datetime-objects-using-pdo-sqlsrv-driver.md).|  
|PDO::SQLSRV_ATTR_FETCHES_NUMERIC_TYPE|True ou False|Gère les extractions de nombres à partir de colonnes avec des types SQL numériques (bit, entier, smallint, tinyint, float ou real).<br /><br />Quand l’indicateur d’option de connexion ATTR_STRINGIFY_FETCHES est activé, la valeur de retour est une chaîne, même si SQLSRV_ATTR_FETCHES_NUMERIC_TYPE est activé.<br /><br />Quand le type PDO retourné dans la colonne de liaison est PDO_PARAM_INT, la valeur de retour à partir d’une colonne d’entiers est int, même si SQLSRV_ATTR_FETCHES_NUMERIC_TYPE est désactivé.|  
|PDO::SQLSRV_ATTR_FORMAT_DECIMALS|True ou False|Spécifie s’il faut ajouter des zéros au début des chaînes décimales si nécessaire. Si elle est définie, cette option active l’option PDO::SQLSRV_ATTR_DECIMAL_PLACES pour mettre en forme les types monétaires. Si vous conservez la valeur false, le comportement par défaut qui consiste à retourner la précision exacte et à omettre les zéros non significatifs pour les valeurs inférieures à 1 est utilisé.<br /><br />Vous pouvez aussi définir cette option au niveau de la connexion. Dans ce cas, cette option remplace l’option au niveau de la connexion.<br /><br />Pour plus d’informations, consultez [Mise en forme des chaînes décimales et valeurs monétaires (pilote PDO_SQLSR)](../../connect/php/formatting-decimals-pdo-sqlsrv-driver.md).| 
|PDO::SQLSRV_ATTR_QUERY_TIMEOUT|Integer|Définit le délai d’expiration de la requête, en secondes.<br /><br />Par défaut, le pilote attend les résultats indéfiniment. Les nombres négatifs ne sont pas autorisés.<br /><br />0 signifie aucun délai d’attente.|  
  
## <a name="example"></a> Exemple  
  
```  
<?php  
$database = "AdventureWorks";  
$server = "(local)";  
$conn = new PDO( "sqlsrv:server=$server ; Database = $database", "", "", array('MultipleActiveResultSets'=>false )  );  
  
$stmt = $conn->prepare('SELECT * FROM Person.ContactType');  
  
echo $stmt->getAttribute( constant( "PDO::ATTR_CURSOR" ) );  
  
echo "\n";  
  
$stmt->setAttribute(PDO::SQLSRV_ATTR_QUERY_TIMEOUT, 2);  
echo $stmt->getAttribute( constant( "PDO::SQLSRV_ATTR_QUERY_TIMEOUT" ) );  
?>  
```  
  
## <a name="see-also"></a>Voir aussi  
[PDOStatement, classe](../../connect/php/pdostatement-class.md)

[PDO](https://php.net/manual/book.pdo.php)  
  
