---
title: PDOStatement::rowCount
description: Référence API pour la fonction PDOStatement::rowCount dans le Pilote Microsoft PDO_SQLSRV pour PHP pour SQL Server.
ms.custom: ''
ms.date: 08/10/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 0569f26a-2376-4c20-8813-bd3c87d0ae9f
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 31671bc3db85a31cde4109cedb11f3ab44f07ea1
ms.sourcegitcommit: 331b8495e4ab37266945c81ff5b93d250bdaa6da
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/20/2020
ms.locfileid: "88646579"
---
# <a name="pdostatementrowcount"></a>PDOStatement::rowCount
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Retourne le nombre de lignes ajoutées, supprimées ou modifiées par la dernière instruction.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
int PDOStatement::rowCount ();  
```  
  
## <a name="return-value"></a>Valeur de retour  
Nombre de lignes ajoutées, supprimées ou modifiées.  
  
## <a name="remarks"></a>Notes  
Si la dernière instruction SQL exécutée par le PDOStatement associé était une instruction SELECT, un curseur PDO::CURSOR_FWDONLY retourne -1. Un curseur PDO::CURSOR_SCROLLABLE retourne le nombre de lignes dans le jeu de résultats.  
  
La prise en charge de PDO a été ajoutée dans la version 2.0 de [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)].  
  
## <a name="example"></a> Exemple  
Cet exemple montre deux utilisations de rowCount. La première utilisation retourne le nombre de lignes ajoutées à la table. La seconde utilisation montre que rowCount peut retourner le nombre de lignes dans un jeu de résultats quand vous spécifiez un curseur de défilement.  
  
```  
<?php  
$database = "Test";  
$server = "(local)";  
$conn = new PDO( "sqlsrv:server=$server ; Database = $database", "", "");  
  
$col1 = 'a';  
$col2 = 'b';  
  
$query = "insert into Table2(col1, col2) values(?, ?)";  
$stmt = $conn->prepare( $query );  
$stmt->execute( array( $col1, $col2 ) );  
print $stmt->rowCount();  
  
echo "\n\n";  
  
$con = null;  
$database = "AdventureWorks";  
$conn = new PDO( "sqlsrv:server=$server ; Database = $database", "", "");  
  
$query = "select * from Person.ContactType";  
$stmt = $conn->prepare( $query, array(PDO::ATTR_CURSOR => PDO::CURSOR_SCROLL));  
$stmt->execute();  
print $stmt->rowCount();  
?>  
```  
  
## <a name="see-also"></a>Voir aussi  
[PDOStatement, classe](../../connect/php/pdostatement-class.md)

[PDO](https://php.net/manual/book.pdo.php)  
  
