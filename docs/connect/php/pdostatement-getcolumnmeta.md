---
title: PDOStatement::getColumnMeta
description: Référence API pour la fonction PDOStatement::getColumnMeta dans le Pilote Microsoft PDO_SQLSRV pour PHP pour SQL Server.
ms.custom: ''
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: c92a21cc-8e53-43d0-a4bf-542c77c100c9
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 8767da09f84be9c557238643e16c925756e0bede
ms.sourcegitcommit: c52a6aeb6fa6d7c3a86b3e84449361f4a0949ad0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/06/2021
ms.locfileid: "99623779"
---
# <a name="pdostatementgetcolumnmeta"></a>PDOStatement::getColumnMeta
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Récupère des métadonnées pour une colonne.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
array PDOStatement::getColumnMeta ( $column );  
```  
  
#### <a name="parameters"></a>Paramètres  
*$conn* : (entier) numéro en base zéro de la colonne dont vous voulez récupérer les métadonnées.  
  
## <a name="return-value"></a>Valeur de retour  
Tableau associatif (clé et valeur) contenant les métadonnées pour la colonne. Consultez la section Notes pour obtenir une description des champs du tableau.  
  
## <a name="remarks"></a>Notes  
Le tableau suivant décrit les champs du tableau retourné par getColumnMeta.  
  
|NOM|VALUES|  
|--------|----------|  
|native_type|Spécifie le type PHP de la colonne. Toujours une chaîne.|  
|driver:decl_type|Spécifie le type SQL utilisé pour représenter la valeur de la colonne dans la base de données. Si la colonne dans le jeu de résultats est le résultat d’une fonction, cette valeur n’est pas retournée par PDOStatement::getColumnMeta.|  
|flags|Spécifie les indicateurs définis pour cette colonne. Toujours 0.|  
|name|Spécifie le nom de la colonne dans la base de données.|  
|table|Spécifie le nom de la table qui contient la colonne dans la base de données. Toujours vide.|  
|len|Spécifie la longueur de la colonne.|  
|précision|Spécifie la précision numérique de cette colonne.|  
|pdo_type|Spécifie le type de cette colonne comme représenté par les constantes PDO::PARAM_*. Toujours PDO::PARAM_STR (2).|  
  
La prise en charge de PDO a été ajoutée dans la version 2.0 de [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)].  
  
## <a name="example"></a> Exemple  
  
```  
<?php  
$database = "AdventureWorks";  
$server = "(local)";  
$conn = new PDO( "sqlsrv:server=$server ; Database = $database", "", "");  
  
$stmt = $conn->query("select * from Person.ContactType");  
$metadata = $stmt->getColumnMeta(2);  
var_dump($metadata);  
  
print $metadata['sqlsrv:decl_type'] . "\n";  
print $metadata['native_type'] . "\n";  
print $metadata['name'];  
?>  
```  
  
## <a name="sensitivity-data-classification-metadata"></a>Métadonnées de classification des données sensibles

À compter de la version 5.8.0, un nouvel attribut d’instruction `PDO::SQLSRV_ATTR_DATA_CLASSIFICATION` permet aux utilisateurs d’accéder aux [métadonnées de classification des données sensibles](../../relational-databases/security/sql-data-discovery-and-classification.md) dans Microsoft SQL Server 2019 avec `PDOStatement::getColumnMeta`, ce qui implique d’utiliser la version 17.4.2 ou une version ultérieure de Microsoft ODBC Driver.

Il est à noter que l’attribut `PDO::SQLSRV_ATTR_DATA_CLASSIFICATION` a la valeur `false` par défaut. En revanche, s’il a la valeur `true`, le champ de tableau mentionné plus haut, `flags`, est rempli avec les métadonnées de classification des données sensibles, le cas échéant. 

Prenons par exemple une table Patients :

```
CREATE TABLE Patients 
      [PatientId] int identity,
      [SSN] char(11),
      [FirstName] nvarchar(50),
      [LastName] nvarchar(50),
      [BirthDate] date)
```

Nous pouvons classer ainsi les colonnes SSN et BirthDate :

```
ADD SENSITIVITY CLASSIFICATION TO [Patients].SSN WITH (LABEL = 'Highly Confidential - secure privacy', INFORMATION_TYPE = 'Credentials')
ADD SENSITIVITY CLASSIFICATION TO [Patients].BirthDate WITH (LABEL = 'Confidential Personal Data', INFORMATION_TYPE = 'Birthdays')
```

Pour accéder aux métadonnées, utilisez `PDOStatement::getColumnMeta` après avoir défini `PDO::SQLSRV_ATTR_DATA_CLASSIFICATION` sur true, comme dans l’extrait de code ci-dessous :

```
$options = array(PDO::SQLSRV_ATTR_DATA_CLASSIFICATION => true);
$tableName = 'Patients';
$tsql = "SELECT * FROM $tableName";
$stmt = $conn->prepare($tsql, $options);
$stmt->execute();
$numCol = $stmt->columnCount();

for ($i = 0; $i < $numCol; $i++) {
    $metadata = $stmt->getColumnMeta($i);
    $jstr = json_encode($metadata);
    echo $jstr . PHP_EOL;
}
```

Voici la sortie des métadonnées de toutes les colonnes :

```
{"flags":{"Data Classification":[]},"sqlsrv:decl_type":"int identity","native_type":"string","table":"","pdo_type":2,"name":"PatientId","len":10,"precision":0}
{"flags":{"Data Classification":[{"Label":{"name":"Highly Confidential - secure privacy","id":""},"Information Type":{"name":"Credentials","id":""}}]},"sqlsrv:decl_type":"char","native_type":"string","table":"","pdo_type":2,"name":"SSN","len":11,"precision":0}
{"flags":{"Data Classification":[]},"sqlsrv:decl_type":"nvarchar","native_type":"string","table":"","pdo_type":2,"name":"FirstName","len":50,"precision":0}
{"flags":{"Data Classification":[]},"sqlsrv:decl_type":"nvarchar","native_type":"string","table":"","pdo_type":2,"name":"LastName","len":50,"precision":0}
{"flags":{"Data Classification":[{"Label":{"name":"Confidential Personal Data","id":""},"Information Type":{"name":"Birthdays","id":""}}]},"sqlsrv:decl_type":"date","native_type":"string","table":"","pdo_type":2,"name":"BirthDate","len":10,"precision":0}
```

Si nous modifions l’extrait de code ci-dessus en affectant à `PDO::SQLSRV_ATTR_DATA_CLASSIFICATION` la valeur `false` (cas par défaut), le champ `flags` sera toujours `0` comme avant :

```
{"flags":0,"sqlsrv:decl_type":"int identity","native_type":"string","table":"","pdo_type":2,"name":"PatientId","len":10,"precision":0}
{"flags":0,"sqlsrv:decl_type":"char","native_type":"string","table":"","pdo_type":2,"name":"SSN","len":11,"precision":0}
{"flags":0,"sqlsrv:decl_type":"nvarchar","native_type":"string","table":"","pdo_type":2,"name":"FirstName","len":50,"precision":0}
{"flags":0,"sqlsrv:decl_type":"nvarchar","native_type":"string","table":"","pdo_type":2,"name":"LastName","len":50,"precision":0}
{"flags":0,"sqlsrv:decl_type":"date","native_type":"string","table":"","pdo_type":2,"name":"BirthDate","len":10,"precision":0}
```

## <a name="sensitivity-rank-using-a-predefined-set-of-values"></a>Classement de la sensibilité avec un ensemble prédéfini de valeurs

À compter de 5.9.0, les pilotes PHP ont ajouté la récupération de la classification de sensibilité lors de l’utilisation du pilote ODBC 17.4.2 ou ultérieur. L’utilisateur peut définir le classement lors de l’utilisation de [ADD SENSITIVITY CLASSIFICATION](/sql/t-sql/statements/add-sensitivity-classification-transact-sql) pour classifier n’importe quelle colonne de données. 

Par exemple, si l’utilisateur affecte `NONE` et `LOW` respectivement à BirthDate et à SSN, la représentation JSON est la suivante :

```
{"0":{"Label":{"name":"Confidential Personal Data","id":""},"Information Type":{"name":"Birthdays","id":""},"rank":0},"rank":0}
{"0":{"Label":{"name":"Highly Confidential - secure privacy","id":""},"Information Type":{"name":"Credentials","id":""},"rank":10},"rank":10}
```

Comme indiqué dans [classification de sensibilité](/sql/relational-databases/system-catalog-views/sys-sensitivity-classifications-transact-sql), les valeurs numériques des degrés sont les suivantes :

```
0 for NONE
10 for LOW
20 for MEDIUM
30 for HIGH
40 for CRITICAL
```

Par conséquent, si au lieu de `RANK=NONE`, l’utilisateur définit `RANK=CRITICAL` lors de la classification de la colonne BirthDate, les métadonnées de classification sont :

```
array(1) {
  ["Data Classification"]=>
  array(2) {
    [0]=>
    array(3) {
      ["Label"]=>
      array(2) {
        ["name"]=>
        string(26) "Confidential Personal Data"
        ["id"]=>
        string(0) ""
      }
      ["Information Type"]=>
      array(2) {
        ["name"]=>
        string(9) "Birthdays"
        ["id"]=>
        string(0) ""
      }
      ["rank"]=>
      int(40)
    }
    ["rank"]=>
    int(40)
  }
}
```

La représentation JSON mise à jour est montrée ci-dessous :

```
{"0":{"Label":{"name":"Confidential Personal Data","id":""},"Information Type":{"name":"Birthdays","id":""},"rank":40},"rank":40}
```

## <a name="see-also"></a>Voir aussi  
[PDOStatement, classe](../../connect/php/pdostatement-class.md)

[PDO](https://php.net/manual/book.pdo.php)  
