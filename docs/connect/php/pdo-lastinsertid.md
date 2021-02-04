---
title: PDO::lastInsertId
description: Référence API pour la fonction PDO::lastInsertId dans le Pilote Microsoft PDO_SQLSRV pour PHP pour SQL Server.
ms.custom: ''
ms.date: 08/10/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 0c617b53-a74b-4d5b-b76b-3ec7f1b8e8de
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: db2a2d3fdeffcf61621bda5af572271d5f918b70
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187587"
---
# <a name="pdolastinsertid"></a>PDO::lastInsertId
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Retourne l’identificateur de la dernière ligne insérée dans une table de la base de données. La table doit comporter une colonne IDENTITY NOT NULL. Si un nom de séquence est fourni, `lastInsertId` retourne le dernier numéro de séquence inséré pour le nom de séquence fourni (pour plus d’informations sur les numéros de séquence, voir [ici](../../relational-databases/sequence-numbers/sequence-numbers.md)).
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
string PDO::lastInsertId ([ $name = NULL ] );  
```  
  
#### <a name="parameters"></a>Paramètres  
$*name* : chaîne facultative qui vous permet de spécifier un nom de séquence. 
  
## <a name="return-value"></a>Valeur de retour  
Si aucun nom de séquence n’est fourni, chaîne de l’identificateur de la dernière ligne ajoutée.
Si un nom de séquence est fourni, chaîne de l’identificateur de la dernière séquence ajoutée.
Si l’appel de méthode échoue, une chaîne vide est retournée.
  
## <a name="remarks"></a>Notes  
La prise en charge de PDO a été ajoutée dans la version 2.0 de [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)].  
Entre les versions 2.0 et 4.3, le paramètre facultatif est un nom de table, et la valeur de retour est l’ID de la dernière ligne ajoutée à la table fournie.
À partir de la version 5.0, le paramètre facultatif est considéré comme un nom de séquence et la valeur de retour est la dernière séquence ajoutée pour le nom de séquence fourni.
Si un nom de table est fourni pour les versions ultérieures à 4.3, `lastInsertId` retourne une chaîne vide.
Les séquences sont prises en charge uniquement dans SQL Server 2012 et versions ultérieures.
  
## <a name="example"></a> Exemple
  
```
<?php
$server = "myserver";
$databaseName = "mydatabase";
$uid = "myusername";
$pwd = "mypasword";

try {
    $conn = new PDO("sqlsrv:Server=$server;Database=$databaseName", $uid, $pwd);
    
    // One sequence, two tables
    $tableName1 = 'seqtable1';
    $tableName2 = 'seqtable2';
    $sequenceName = 'sequence1';

    $stmt = $conn->query("IF OBJECT_ID('$sequenceName', 'SO') IS NOT NULL DROP SEQUENCE $sequenceName");
    $sql = "CREATE TABLE $tableName1 (seqnum INTEGER NOT NULL PRIMARY KEY, SomeNumber INT)";
    $stmt = $conn->query($sql);
    $sql = "CREATE TABLE $tableName2 (ID INT IDENTITY(1,2), SomeValue char(10))";
    $stmt = $conn->query($sql);

    $sql = "CREATE SEQUENCE $sequenceName AS INTEGER START WITH 1 INCREMENT BY 1 MINVALUE 1 MAXVALUE 100 CYCLE";
    $stmt = $conn->query($sql);

    $ret = $conn->exec("INSERT INTO $tableName1 VALUES( NEXT VALUE FOR $sequenceName, 20)");
    $ret = $conn->exec("INSERT INTO $tableName1 VALUES( NEXT VALUE FOR $sequenceName, 40)");
    $ret = $conn->exec("INSERT INTO $tableName1 VALUES( NEXT VALUE FOR $sequenceName, 60)");
    $ret = $conn->exec("INSERT INTO $tableName2 VALUES( '20' )");
    
    // return the last sequence number if sequence name is provided
    $lastSeq1 = $conn->lastInsertId($sequenceName);
    
    // defaults to $tableName2 -- because it returns the last inserted id value
    $lastRow = $conn->lastInsertId();
        
    // providing a table name in lastInsertId should return an empty string
    $lastSeq2 = $conn->lastInsertId($tableName2);
    
    echo "Last sequence number = $lastSeq1\n";
    echo "Last inserted ID     = $lastRow\n";
    echo "Last inserted ID when a table name is supplied = $lastSeq2\n";

    // One table, two sequences    
    $tableName = 'seqtable';
    $sequence1 = 'sequence1';
    $sequence2 = 'sequenceNeg1';
    $stmt = $conn->query("IF OBJECT_ID('$sequence1', 'SO') IS NOT NULL DROP SEQUENCE $sequence1");
    $stmt = $conn->query("IF OBJECT_ID('$sequence2', 'SO') IS NOT NULL DROP SEQUENCE $sequence2");
    $sql = "CREATE TABLE $tableName (ID INT IDENTITY(1,1), SeqNumInc INTEGER NOT NULL PRIMARY KEY, SomeNumber INT)";
    $stmt = $conn->query($sql);
    $sql = "CREATE SEQUENCE $sequence1 AS INTEGER START WITH 1 INCREMENT BY 1 MINVALUE 1 MAXVALUE 100";
    $stmt = $conn->query($sql);

    $sql = "CREATE SEQUENCE $sequence2 AS INTEGER START WITH 200 INCREMENT BY -1 MINVALUE 101 MAXVALUE 200";
    $stmt = $conn->query($sql);
    $ret = $conn->exec("INSERT INTO $tableName VALUES( NEXT VALUE FOR $sequence1, 20 )");
    $ret = $conn->exec("INSERT INTO $tableName VALUES( NEXT VALUE FOR $sequence2, 180 )");
    $ret = $conn->exec("INSERT INTO $tableName VALUES( NEXT VALUE FOR $sequence1, 40 )");
    $ret = $conn->exec("INSERT INTO $tableName VALUES( NEXT VALUE FOR $sequence2, 160 )");
    $ret = $conn->exec("INSERT INTO $tableName VALUES( NEXT VALUE FOR $sequence1, 60 )");
    $ret = $conn->exec("INSERT INTO $tableName VALUES( NEXT VALUE FOR $sequence2, 140 )");
    
    // return the last sequence number of 'sequence1'
    $lastSeq1 = $conn->lastInsertId($sequence1);

    // return the last sequence number of 'sequenceNeg1'
    $lastSeq2 = $conn->lastInsertId($sequence2);

    // providing a table name in lastInsertId should return an empty string
    $lastSeq3 = $conn->lastInsertId($tableName);
    
    echo "Last sequence number of sequence1    = $lastSeq1\n";
    echo "Last sequence number of sequenceNeg1 = $lastSeq2\n";
    echo "Last sequence number when a table name is supplied = $lastSeq3\n";

    $stmt = $conn->query("DROP TABLE $tableName1");
    $stmt = $conn->query("DROP TABLE $tableName2");
    $stmt = $conn->query("DROP SEQUENCE $sequenceName");
    $stmt = $conn->query("DROP TABLE $tableName");
    $stmt = $conn->query("DROP SEQUENCE $sequence1");
    $stmt = $conn->query("DROP SEQUENCE $sequence2");
    
    unset($stmt);
    unset($conn);
} catch (Exception $e) {
    echo "Exception $e\n";
}

?>
```

La sortie attendue est :

```
Last sequence number = 3
Last inserted ID     = 1
Last inserted ID when a table name is supplied =

Last sequence number of sequence1    = 3
Last sequence number of sequenceNeg1 = 198
Last sequence number when a table name is supplied = 

```

## <a name="see-also"></a>Voir aussi  
[PDO, classe](../../connect/php/pdo-class.md)

[PDO](https://php.net/manual/book.pdo.php)  
