---
title: PDOStatement::errorInfo
description: Référence API pour la fonction PDOStatement::errorInfo dans le Pilote Microsoft PDO_SQLSRV pour PHP pour SQL Server.
ms.custom: ''
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: e45bebe8-ea4c-49b6-93db-cf1ae65f530c
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 0474b0c1225a248b47f0892ac940f2e58ab0efa0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206355"
---
# <a name="pdostatementerrorinfo"></a>PDOStatement::errorInfo
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Récupère les informations d’erreur étendues de la dernière opération effectuée sur le handle d’instruction.  
  
## <a name="syntax"></a>Syntaxe  

```
array PDOStatement::errorInfo();
```
  
## <a name="return-value"></a>Valeur de retour  
Tableau d’informations d’erreur sur la dernière opération effectuée sur le handle d’instruction. Le tableau comprend les champs suivants :  
  
-   Code d’erreur SQLSTATE  
  
-   Code d’erreur spécifique du pilote  
  
-   Message d’erreur spécifique du pilote  
  
En l’absence d’erreur, ou si la valeur SQLSTATE n’est pas définie, les champs propres au pilote ont la valeur NULL.  
  
## <a name="remarks"></a>Notes  
La prise en charge de PDO a été ajoutée dans la version 2.0 de [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)].  
  
## <a name="example"></a> Exemple  
Dans cet exemple, l’instruction SQL comporte une erreur, qui est par la suite signalée.  
  
```php
<?php  
$conn = new PDO( "sqlsrv:server=(local) ; Database = AdventureWorks", "", "");  
$stmt = $conn->prepare('SELECT * FROM Person.Addressx');  
  
$stmt->execute();  
print_r ($stmt->errorInfo());  
?>  
```

## <a name="additional-odbc-messages"></a>Messages ODBC supplémentaires

Quand une exception se produit, le pilote ODBC peut retourner plusieurs erreurs pour aider à diagnostiquer les problèmes. Cependant, PDOStatement::errorInfo montre toujours seulement la première erreur. En réponse au [signalement de ce bogue](https://bugs.php.net/bug.php?id=78196), [PDO::errorInfo](https://www.php.net/manual/en/pdo.errorinfo.php) et [PDOStatement::errorInfo](https://www.php.net/manual/en/pdostatement.errorinfo.php) ont été mis à jour pour indiquer que les pilotes devaient afficher *au moins* les trois champs suivants :
```
0   SQLSTATE error code (a five characters alphanumeric identifier defined in the ANSI SQL standard).
1   Driver specific error code.
2   Driver specific error message.
```

À compter de 5.9.0, le comportement par défaut de PDOStatement::errorInfo est de montrer les erreurs ODBC supplémentaires, si elles sont disponibles. Pour plus d’informations, consultez [PDO::errorInfo](../../connect/php/pdo-errorinfo.md).
  
## <a name="see-also"></a>Voir aussi  
[PDOStatement, classe](../../connect/php/pdostatement-class.md)

[PDO::ErrorInfo](../../connect/php/pdo-errorinfo.md)

[PDO](https://php.net/manual/book.pdo.php)  
  
