---
title: PDOStatement::bindValue
description: Référence API pour la fonction PDOStatement::bindValue dans le Pilote Microsoft PDO_SQLSRV pour PHP pour SQL Server.
ms.custom: ''
ms.date: 08/10/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 13bc4ece-420e-4887-8809-bf0705ddf126
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 40b696cf3cd8cc0c475a052d78966ee45b5f7b72
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195205"
---
# <a name="pdostatementbindvalue"></a>PDOStatement::bindValue
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Lie une valeur à un espace réservé nommé ou constitué de points d’interrogation dans l’instruction SQL.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
bool PDOStatement::bindValue($parameter, $value[, $data_type]);  
```  
  
#### <a name="parameters"></a>Paramètres  
$*parameter* : identificateur de paramètre (mixte). Pour une instruction qui utilise des espaces réservés nommés, utilisez un nom de paramètre (:name). Pour une instruction préparée qui utilise la syntaxe constituée de points d’interrogation, il s’agit de l’index en base 1 du paramètre.
  
$*value* : valeur (mixte) à lier au paramètre.  
  
$*data_type* : type de données (entier) facultatif représenté par une constante PDO::PARAM_*. La valeur par défaut est PDO::PARAM_STR.  
  
## <a name="return-value"></a>Valeur de retour  
TRUE en cas de réussite ; sinon, FALSE.  
  
## <a name="remarks"></a>Notes  
  
La prise en charge de PDO a été ajoutée dans la version 2.0 de [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)].  
  
## <a name="parameter-example"></a>Exemple de paramètre  
Cet exemple montre qu’une fois que la valeur de $contact est liée, sa modification ne change pas la valeur transmise dans la requête.  
  
```  
<?php  
$database = "AdventureWorks";  
$server = "(local)";  
$conn = new PDO("sqlsrv:server=$server ; Database = $database", "", "");  
  
$contact = "Sales Agent";  
$stmt = $conn->prepare("select * from Person.ContactType where name = ?");  
$stmt->bindValue(1, $contact);  
$contact = "Owner";  
$stmt->execute();  
  
while ($row = $stmt->fetch(PDO::FETCH_ASSOC)) {  
   print "$row[Name]\n\n";  
}  
  
$stmt = null;  
$contact = "Sales Agent";  
$stmt = $conn->prepare("select * from Person.ContactType where name = :contact");  
$stmt->bindValue(':contact', $contact);  
$contact = "Owner";  
$stmt->execute();  
  
while ($row = $stmt->fetch(PDO::FETCH_ASSOC)) {  
   print "$row[Name]\n\n";  
}  
?>  
```

> [!NOTE]
> Il est recommandé d’utiliser des chaînes en entrée pour lier des valeurs à une [colonne décimale ou numérique](../../t-sql/data-types/decimal-and-numeric-transact-sql.md) pour des raisons de précision et d’exactitude, car PHP n’offre qu’une précision limitée pour les [nombres à virgule flottante](https://php.net/manual/en/language.types.float.php). Il en va de même pour les colonnes bigint, en particulier si les valeurs sont en dehors de la plage des [entiers](../../t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql.md).

## <a name="decimal-input-example"></a>Exemple d’entrée décimale  
Cet exemple de code montre comment lier une valeur décimale comme paramètre d’entrée.  

```
<?php  
$database = "Test";  
$server = "(local)";  
$conn = new PDO("sqlsrv:server=$server ; Database = $database", "", "");  

// Assume TestTable exists with a decimal field 
$input = 9223372036854.80000;
$stmt = $conn->prepare("INSERT INTO TestTable (DecimalCol) VALUES (?)");
// by default it is PDO::PARAM_STR, rounding of a large input value may
// occur if PDO::PARAM_INT is specified
$stmt->bindValue(1, $input, PDO::PARAM_STR);
$stmt->execute();
```
  
## <a name="see-also"></a>Voir aussi  
[PDOStatement, classe](../../connect/php/pdostatement-class.md)

[PDO](https://php.net/manual/book.pdo.php)  
