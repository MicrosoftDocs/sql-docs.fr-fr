---
title: 'Procédure : Envoyer et récupérer des données ASCII sur Linux et macOS (SQL)'
description: Cette rubrique explique comment envoyer et récupérer des données ASCII dans Linux et macOS lors de l’utilisation des Pilotes Microsoft pour PHP pour SQL Server
ms.custom: ''
ms.date: 01/16/2018
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- retrieving data, ASCII data
- sending data
- linux
- macOS
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 015562e9a783cef79a9466778b89edecffee5fe0
ms.sourcegitcommit: d1051f05a7db81ec62d9785bb6af572408f3d4e0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/20/2020
ms.locfileid: "88680414"
---
# <a name="how-to-send-and-retrieve-ascii-data-in-linux-and-macos"></a>Procédure : Envoyer et récupérer des données ASCII dans Linux et macOS 
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Cet article suppose que les paramètres régionaux ASCII (non UTF-8) ont été générés ou installés sur vos systèmes Linux ou macOS. 

Pour envoyer ou récupérer des jeux de caractères ASCII sur le serveur :  

1.  Si les paramètres régionaux souhaités ne sont pas définis par défaut dans votre environnement système, veillez à appeler `setlocale(LC_ALL, $locale)` avant d’établir la première connexion. La fonction PHP setlocale() modifie les paramètres régionaux uniquement pour le script en cours et, si elle est appelée après la première connexion, elle peut être ignorée.
 
2.  Lorsque vous utilisez le pilote SQLSRV, vous pouvez spécifier `'CharacterSet' => SQLSRV_ENC_CHAR` comme option de connexion, mais cette étape est facultative car il s’agit de l’encodage par défaut.

3.  Lorsque vous utilisez le pilote PDO_SQLSRV, il existe deux façons de procéder. Tout d’abord, lorsque vous établissez la connexion, définissez `PDO::SQLSRV_ATTR_ENCODING` sur `PDO::SQLSRV_ENCODING_SYSTEM` (pour obtenir un exemple de définition d’une option de connexion, consultez [PDO::__construct](../../connect/php/pdo-construct.md)). Une fois la connexion établie, vous pouvez également ajouter la ligne `$conn->setAttribute(PDO::SQLSRV_ATTR_ENCODING, PDO::SQLSRV_ENCODING_SYSTEM);` 
  
Lorsque vous spécifiez l’encodage d’une ressource de connexion (dans SQLSRV) ou d’un objet de connexion (PDO_SQLSRV), le pilote suppose que les autres chaînes d’option de connexion utilisent ce même encodage. Les chaînes de nom de serveur et de requête sont également supposées utiliser le même jeu de caractères.  
  
L’encodage par défaut pour PDO_SQLSRV pilote est UTF-8 (PDO ::SQLSRV_ENCODING_UTF8), contrairement au pilote SQLSRV. Pour plus d’informations sur ces constantes, consultez [Constantes &#40;Microsoft Drivers for PHP for SQL Server&#41;](../../connect/php/constants-microsoft-drivers-for-php-for-sql-server.md). 
  
## <a name="example"></a> Exemple  
Les exemples suivants montrent comment envoyer et récupérer des données ASCII à l’aide des pilotes PHP pour SQL Server en spécifiant des paramètres régionaux particuliers avant d’établir la connexion. Les paramètres régionaux de différentes plateformes Linux peuvent être nommés différemment des mêmes paramètres régionaux dans macOS. Par exemple, les paramètres régionaux ISO-8859-1 (latin 1) pour les États-Unis sont `en_US.ISO-8859-1` dans Linux, tandis que dans macOS ils s’appellent `en_US.ISO8859-1`.
  
Les exemples supposent que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est installé sur un serveur. Toutes les sorties sont écrites dans le navigateur quand les exemples sont exécutés à partir du navigateur.  
  
```  
<?php  
  
// SQLSRV Example
//
// Setting locale for the script is only necessary if Latin 1 is not the default 
// in the environment
$locale = strtoupper(PHP_OS) === 'LINUX' ? 'en_US.ISO-8859-1' : 'en_US.ISO8859-1';
setlocale(LC_ALL, $locale);
        
$serverName = 'MyServer';
$database = 'Test';
$connectionInfo = array('Database'=>'Test', 'UID'=>$uid, 'PWD'=>$pwd);
$conn = sqlsrv_connect($serverName, $connectionInfo);
  
if ($conn === false) {
    echo "Could not connect.<br>";  
    die(print_r(sqlsrv_errors(), true));
}  
  
// Set up the Transact-SQL query to create a test table
//   
$stmt = sqlsrv_query($conn, "CREATE TABLE [Table1] ([c1_int] int, [c2_varchar] varchar(512))");

// Insert data using a parameter array 
//
$tsql = "INSERT INTO [Table1] (c1_int, c2_varchar) VALUES (?, ?)";
  
// Execute the query, $value being some ASCII string
//   
$stmt = sqlsrv_query($conn, $tsql, array(1, array($value, SQLSRV_PARAM_IN, SQLSRV_PHPTYPE_STRING(SQLSRV_ENC_CHAR))));
  
if ($stmt === false) {
    echo "Error in statement execution.<br>";  
    die(print_r(sqlsrv_errors(), true));  
}  
else {  
    echo "The insertion was successfully executed.<br>";  
}  
  
// Retrieve the newly inserted data
//   
$stmt = sqlsrv_query($conn, "SELECT * FROM Table1");
$outValue = null;  
if ($stmt === false) {  
    echo "Error in statement execution.<br>";  
    die(print_r(sqlsrv_errors(), true));  
}  
  
// Retrieve and display the data
//   
if (sqlsrv_fetch($stmt)) {  
    $outValue = sqlsrv_get_field($stmt, 1, SQLSRV_PHPTYPE_STRING(SQLSRV_ENC_CHAR));
    echo "Value: " . $outValue . "<br>";
    if ($value !== $outValue) {
        echo "Data retrieved, \'$outValue\', is unexpected!<br>";
    }
}  
else {  
    echo "Error in fetching data.<br>";  
    die(print_r(sqlsrv_errors(), true));  
}  

// Free statement and connection resources
//   
sqlsrv_free_stmt($stmt);  
sqlsrv_close($conn);  
?>  
```  
  
```
<?php  
  
// PDO_SQLSRV Example:
//
// Setting locale for the script is only necessary if Latin 1 is not the default 
// in the environment
$locale = strtoupper(PHP_OS) === 'LINUX' ? 'en_US.ISO-8859-1' : 'en_US.ISO8859-1';
setlocale(LC_ALL, $locale);
        
$serverName = 'MyServer';
$database = 'Test';

try {
    $conn = new PDO("sqlsrv:Server=$serverName;Database=$database;", $uid, $pwd);
    $conn->setAttribute(PDO::SQLSRV_ATTR_ENCODING, PDO::SQLSRV_ENCODING_SYSTEM);
    
    // Set up the Transact-SQL query to create a test table
    //   
    $stmt = $conn->query("CREATE TABLE [Table1] ([c1_int] int, [c2_varchar] varchar(512))");
    
    // Insert data using parameters, $value being some ASCII string
    //
    $stmt = $conn->prepare("INSERT INTO [Table1] (c1_int, c2_varchar) VALUES (:var1, :var2)");
    $stmt->bindValue(1, 1);
    $stmt->bindParam(2, $value);
    $stmt->execute();
    
    // Retrieve and display the data
    //
    $stmt = $conn->query("SELECT * FROM [Table1]");
    $outValue = null;
    if ($row = $stmt->fetch()) {
        $outValue = $row[1];
        echo "Value: " . $outValue . "<br>";
        if ($value !== $outValue) {
            echo "Data retrieved, \'$outValue\', is unexpected!<br>";
        }
    }
} catch (PDOException $e) {
    echo $e->getMessage() . "<br>";
} finally {
    // Free statement and connection resources
    //
    unset($stmt);
    unset($conn);
}

?>  
```  

## <a name="see-also"></a>Voir aussi  
[Récupération de données](../../connect/php/retrieving-data.md)  
[Utilisation des données UTF-8](../../connect/php/how-to-send-and-retrieve-utf-8-data-using-built-in-utf-8-support.md)
[Mise à jour des données &#40;Pilotes Microsoft SQL Server pour PHP&#41;](../../connect/php/updating-data-microsoft-drivers-for-php-for-sql-server.md)  
[Informations de référence sur l’API du pilote SQLSRV](../../connect/php/sqlsrv-driver-api-reference.md)  
[Constantes &#40;Microsoft Drivers for PHP for SQL Server&#41;](../../connect/php/constants-microsoft-drivers-for-php-for-sql-server.md)  
[Exemple d’application &#40;pilote SQLSRV&#41;](../../connect/php/example-application-sqlsrv-driver.md)  
  
