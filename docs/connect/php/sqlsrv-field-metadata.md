---
description: sqlsrv_field_metadata
title: sqlsrv_field_metadata | Microsoft Docs
ms.custom: ''
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- sqlsrv_field_metadata
apitype: NA
helpviewer_keywords:
- API Reference, sqlsrv_field_metadata
- sqlsrv_field_metadata
ms.assetid: c02f6942-0484-4567-a78e-fe8aa2053536
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 82e1ad1147867abd9308dc89517855df830a38e6
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837152"
---
# <a name="sqlsrv_field_metadata"></a>sqlsrv_field_metadata
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Récupère les métadonnées des champs d’une instruction préparée. Pour plus d’informations sur la préparation d’une instruction, consultez [sqlsrv_query](../../connect/php/sqlsrv-query.md) ou [sqlsrv_prepare](../../connect/php/sqlsrv-prepare.md). Notez que **sqlsrv_field_metadata** peut être appelé sur toute instruction préparée, avant ou après l’exécution.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sqlsrv_field_metadata( resource $stmt)  
```  
  
#### <a name="parameters"></a>Paramètres  
*$stmt*: ressource d’instruction pour laquelle des métadonnées de champ sont recherchées.  
  
## <a name="return-value"></a>Valeur de retour  
**Tableau** de tableaux ou **false**. Le tableau se compose d’un tableau pour chaque champ dans le jeu de résultats. Chaque sous-tableau possède des clés décrites dans le tableau ci-dessous. En cas d’erreur lors de l’extraction des métadonnées de champ, **false** est retourné.  
  
|Clé|Description|  
|-------|---------------|  
|Name|Nom de la colonne à laquelle le champ correspond.|  
|Type|Valeur numérique qui correspond à un type SQL.|  
|Taille|Nombre de caractères des champs de type caractère (char(n), varchar(n), nchar(n), nvarchar(n), XML). Nombre d’octets des champs de type binaire (binary(n), varbinary(n), UDT). **NULL** pour les autres types de données SQL Server.|  
|Precision|Précision des types dont la précision varie (réel, numérique, décimal, DateHeure2, datetimeoffset et Heure). **NULL** pour les autres types de données SQL Server.|  
|Scale|Échelle des types dont l’échelle varie (numérique, décimal, DateHeure2, datetimeoffset et Heure). **NULL** pour les autres types de données SQL Server.|  
|Nullable|Valeur énumérée indiquant si la colonne est Nullable (**SQLSRV_NULLABLE_YES**), si la colonne n’est pas Nullable (**SQLSRV_NULLABLE_NO**) ou si l’indication que la colonne est Nullable est inconnu (**SQLSRV_NULLABLE_UNKNOWN**).|  
  
Le tableau suivant fournit plus d’informations sur les clés pour chaque sous-tableau (consultez la documentation de SQL Server pour plus d’informations sur ces types) :  
  
|Type de données de SQL Server 2008|Type|Précision minimale/maximale|Échelle minimale/maximale|Taille|  
|-----------------------------|--------|----------------------|------------------|--------|  
|bigint|SQL_BIGINT (-5)|||8|  
|binary|SQL_BINARY (-2)|||0 < *n* < 8000 <sup>1</sup>|  
|bit|SQL_BIT (-7)||||  
|char|SQL_CHAR (1)|||0 < *n* < 8000 <sup>1</sup>|  
|date|SQL_TYPE_DATE (91)|10/10|0/0||  
|DATETIME|SQL_TYPE_TIMESTAMP (93)|23/23|3/3||  
|datetime2|SQL_TYPE_TIMESTAMP (93)|19/27|0/7||  
|datetimeoffset|SQL_SS_TIMESTAMPOFFSET (-155)|26/34|0/7||  
|Décimal|SQL_DECIMAL (3)|1/38|0/valeur de précision||  
|float|SQL_FLOAT (6)|4/8|||  
|image|SQL_LONGVARBINARY (-4)|||2 Go|  
|int|SQL_INTEGER (4)||||  
|money|SQL_DECIMAL (3)|19/19|4/4||  
|NCHAR|SQL_WCHAR (-8)|||0 < *n* < 4000 <sup>1</sup>|  
|ntext|SQL_WLONGVARCHAR (-10)|||1 Go|  
|numeric|SQL_NUMERIC (2)|1/38|0/valeur de précision||  
|NVARCHAR|SQL_WVARCHAR (-9)|||0 < *n* < 4000 <sup>1</sup>|  
|real|SQL_REAL (7)|4/4|||  
|smalldatetime|SQL_TYPE_TIMESTAMP (93)|16/16|0/0||  
|SMALLINT|SQL_SMALLINT (5)|||2 octets|  
|Smallmoney|SQL_DECIMAL (3)|10/10|4/4||  
|sql_variant|SQL_SS_VARIANT (-150)|||variable|  
|texte|SQL_LONGVARCHAR (-1)|||2 Go|  
|time|SQL_SS_TIME2 (-154)|8/16|0/7||  
|timestamp|SQL_BINARY (-2)|||8 octets|  
|TINYINT|SQL_TINYINT (-6)|||1 octet|  
|udt|SQL_SS_UDT (-151)|||variable|  
|UNIQUEIDENTIFIER|SQL_GUID (-11)|||16|  
|varbinary|SQL_VARBINARY (-3)|||0 < *n* < 8000 <sup>1</sup>|  
|varchar|SQL_VARCHAR (12)|||0 < *n* < 8000 <sup>1</sup>|  
|Xml|SQL_SS_XML (-152)|||0|  
  
(1) Zéro (0) indique que la taille maximale est autorisée.  
  
La clé Nullable peut avoir la valeur oui ou non.  
  
## <a name="example"></a> Exemple  
L’exemple suivant crée une ressource d’instruction, puis récupère et affiche les métadonnées de champ. L’exemple part du principe que SQL Server et la base de données [AdventureWorks](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works) sont installés sur l’ordinateur local. Toute la sortie est écrite dans la console quand l’exemple est exécuté à partir de la ligne de commande.  
  
```
<?php
/* Connect to the local server using Windows Authentication and
specify the AdventureWorks database as the database in use. */
$serverName = "(local)";
$connectionInfo = array("Database"=>"AdventureWorks");
$conn = sqlsrv_connect($serverName, $connectionInfo);
if ($conn === false) {
    echo "Could not connect.\n";
    die( print_r( sqlsrv_errors(), true));
}

/* Prepare the statement. */
$tsql = "SELECT ReviewerName, Comments FROM Production.ProductReview";
$stmt = sqlsrv_prepare( $conn, $tsql);
  
/* Get and display field metadata. */
foreach( sqlsrv_field_metadata( $stmt) as $fieldMetadata) {
    foreach( $fieldMetadata as $name => $value) {
        echo "$name: $value\n";
    }  
    echo "\n";
}  
  
/* Note: sqlsrv_field_metadata can be called on any statement
resource, pre- or post-execution. */
  
/* Free statement and connection resources. */
sqlsrv_free_stmt($stmt);
sqlsrv_close($conn);
?>
```

## <a name="sensitivity-data-classification-metadata"></a>Métadonnées de classification des données sensibles

Une nouvelle option `DataClassification` est introduite dans la version 5.8.0 pour permettre aux utilisateurs d’accéder aux [métadonnées de classification des données sensibles](../../relational-databases/security/sql-data-discovery-and-classification.md) dans Microsoft SQL Server 2019 avec `sqlsrv_field_metadata`, ce qui implique d’utiliser la version 17.4.2 ou une version ultérieure de Microsoft ODBC Driver. 

Par défaut, l’option `DataClassification` a la valeur `false` par défaut. En revanche, lorsqu’elle a la valeur `true`, le tableau retourné par `sqlsrv_field_metadata` est rempli avec les métadonnées de classification des données sensibles, le cas échéant. 

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

Pour accéder aux métadonnées, appelez `sqlsrv_field_metadata` comme dans l’extrait de code ci-dessous :

```
$tableName = 'Patients';
$tsql = "SELECT * FROM $tableName";
$stmt = sqlsrv_prepare($conn, $tsql, array(), array('DataClassification' => true));
if (sqlsrv_execute($stmt)) {
    $fieldmeta = sqlsrv_field_metadata($stmt);

    foreach ($fieldmeta as $f) {
        if (count($f['Data Classification']) > 0) {
            echo $f['Name'] . ": \n";
            print_r($f['Data Classification']); 
        }
    }
}
```

La sortie se présente comme suit :

```
SSN: 
Array
(
    [0] => Array
        (
            [Label] => Array
                (
                    [name] => Highly Confidential - secure privacy
                    [id] => 
                )

            [Information Type] => Array
                (
                    [name] => Credentials
                    [id] => 
                )

        )

)
BirthDate: 
Array
(
    [0] => Array
        (
            [Label] => Array
                (
                    [name] => Confidential Personal Data
                    [id] => 
                )

            [Information Type] => Array
                (
                    [name] => Birthdays
                    [id] => 
                )

        )

)
```

Si vous utilisez `sqlsrv_query` au lieu de `sqlsrv_prepare`, vous pouvez modifier ainsi l’extrait de code ci-dessus :

```
$tableName = 'Patients';
$tsql = "SELECT * FROM $tableName";
$stmt = sqlsrv_query($conn, $tsql, array(), array('DataClassification' => true));
$fieldmeta = sqlsrv_field_metadata($stmt);

foreach ($fieldmeta as $f) {
    $jstr = json_encode($f);
    echo $jstr . PHP_EOL;
}
```

Comme on le voit dans la représentation JSON ci-dessous, les métadonnées de classification des données sont affichées si elles sont associées aux colonnes :

```
{"Name":"PatientId","Type":4,"Size":null,"Precision":10,"Scale":null,"Nullable":0,"Data Classification":[]}
{"Name":"SSN","Type":1,"Size":11,"Precision":null,"Scale":null,"Nullable":1,"Data Classification":[{"Label":{"name":"Highly Confidential - secure privacy","id":""},"Information Type":{"name":"Credentials","id":""}}]}
{"Name":"FirstName","Type":-9,"Size":50,"Precision":null,"Scale":null,"Nullable":1,"Data Classification":[]}
{"Name":"LastName","Type":-9,"Size":50,"Precision":null,"Scale":null,"Nullable":1,"Data Classification":[]}
{"Name":"BirthDate","Type":91,"Size":null,"Precision":10,"Scale":0,"Nullable":1,"Data Classification":[{"Label":{"name":"Confidential Personal Data","id":""},"Information Type":{"name":"Birthdays","id":""}}]}
```

## <a name="sensitivity-rank-using-a-predefined-set-of-values"></a>Classement de la sensibilité avec un ensemble prédéfini de valeurs

À compter de 5.9.0, les pilotes PHP ont ajouté la récupération de la classification de sensibilité lors de l’utilisation du pilote ODBC 17.4.2 ou ultérieur. L’utilisateur peut définir le classement lors de l’utilisation de [ADD SENSITIVITY CLASSIFICATION](../../t-sql/statements/add-sensitivity-classification-transact-sql.md) pour classifier n’importe quelle colonne de données. 

Par exemple, si l’utilisateur affecte `NONE` et `LOW` respectivement à BirthDate et à SSN, la représentation JSON est la suivante :

```
{"0":{"Label":{"name":"Confidential Personal Data","id":""},"Information Type":{"name":"Birthdays","id":""},"rank":0},"rank":0}
{"0":{"Label":{"name":"Highly Confidential - secure privacy","id":""},"Information Type":{"name":"Credentials","id":""},"rank":10},"rank":10}
```

Comme indiqué dans [classification de sensibilité](../../relational-databases/system-catalog-views/sys-sensitivity-classifications-transact-sql.md), les valeurs numériques des degrés sont les suivantes :

```
0 for NONE
10 for LOW
20 for MEDIUM
30 for HIGH
40 for CRITICAL
```

Par conséquent, si au lieu de `RANK=NONE`, l’utilisateur définit `RANK=CRITICAL` lors de la classification de la colonne BirthDate, les métadonnées de classification sont :

```
  array(7) {
    ["Name"]=>
    string(9) "BirthDate"
    ["Type"]=>
    int(91)
    ["Size"]=>
    NULL
    ["Precision"]=>
    int(10)
    ["Scale"]=>
    int(0)
    ["Nullable"]=>
    int(1)
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
[Informations de référence sur l’API du pilote SQLSRV](../../connect/php/sqlsrv-driver-api-reference.md)  

[Constantes &#40;Microsoft Drivers for PHP for SQL Server&#41;](../../connect/php/constants-microsoft-drivers-for-php-for-sql-server.md)  

[À propos des exemples de code dans la documentation](../../connect/php/about-code-examples-in-the-documentation.md)