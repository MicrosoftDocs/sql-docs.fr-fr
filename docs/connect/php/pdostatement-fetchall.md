---
title: PDOStatement::fetchAll
description: Référence API pour la fonction PDOStatement::fetchAll dans le Pilote Microsoft PDO_SQLSRV pour PHP pour SQL Server.
ms.custom: ''
ms.date: 08/10/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: be74188a-77cd-4d19-b16e-77278373c979
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b2e5e848e7496455d17512bf9f70483f7b5ee8a8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206325"
---
# <a name="pdostatementfetchall"></a>PDOStatement::fetchAll
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Retourne les lignes dans un jeu de résultats dans un tableau.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
array PDOStatement::fetchAll([ $fetch_style[, $column_index ][, ctor_args]] );  
```  
  
#### <a name="parameters"></a>Paramètres  
$*fetch_style* : symbole (entier) spécifiant le format des données de la ligne. Consultez [PDOStatement::fetch](../../connect/php/pdostatement-fetch.md) pour obtenir la liste des valeurs. PDO::FETCH_COLUMN est également autorisé. PDO::FETCH_BOTH est la valeur par défaut.  
  
$*column_index* : valeur entière représentant la colonne à retourner si $*fetch_style* correspond à PDO::FETCH_COLUMN. La valeur par défaut est 0.  
  
$*ctor_args* : tableau des paramètres d’un constructeur de classe, quand $*fetch_style* correspond à PDO::FETCH_CLASS ou PDO::FETCH_OBJ.  
  
## <a name="return-value"></a>Valeur de retour  
Tableau des lignes restantes dans le jeu de résultats, ou false si l’appel de méthode échoue.  
  
## <a name="remarks"></a>Notes  
La prise en charge de PDO a été ajoutée dans la version 2.0 de [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)].  
  
## <a name="example"></a> Exemple  
  
```  
<?php  
   $server = "(local)";  
   $database = "AdventureWorks";  
   $conn = new PDO( "sqlsrv:server=$server ; Database = $database", "", "");  
  
   print "-----------\n";  
   $stmt = $conn->query( "select * from Person.ContactType where ContactTypeID < 5 " );  
   $result = $stmt->fetchall(PDO::FETCH_BOTH);  
   print_r( $result );  
   print "\n-----------\n";  
  
   print "-----------\n";  
   $stmt = $conn->query( "select * from Person.ContactType where ContactTypeID < 5 " );  
   $result = $stmt->fetchall(PDO::FETCH_NUM);  
   print_r( $result );  
   print "\n-----------\n";  
  
   $stmt = $conn->query( "select * from Person.ContactType where ContactTypeID < 5 " );  
   $result = $stmt->fetchall(PDO::FETCH_COLUMN, 1);  
   print_r( $result );  
   print "\n-----------\n";  
  
   class cc {  
      function __construct( $arg ) {  
         echo "$arg\n";  
      }  
  
      function __toString() {  
         echo "To string\n";  
      }  
   };  
  
   $stmt = $conn->query( 'SELECT TOP(2) * FROM Person.ContactType' );  
   $all = $stmt->fetchAll( PDO::FETCH_CLASS, 'cc', array( 'Hi!' ));  
   var_dump( $all );  
?>  
```  
  
## <a name="see-also"></a>Voir aussi  
[PDOStatement, classe](../../connect/php/pdostatement-class.md)

[PDO](https://php.net/manual/book.pdo.php)  
  
