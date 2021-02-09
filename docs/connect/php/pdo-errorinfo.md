---
title: PDO::ErrorInfo
description: Référence API pour la fonction PDO::errorInfo dans le Pilote Microsoft PDO_SQLSRV pour PHP pour SQL Server.
ms.custom: ''
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 9d5481d5-13bc-4388-b3aa-78676c0fc709
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 46b57275d92f4eb6acb64c276e3b34ea67efb429
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202025"
---
# <a name="pdoerrorinfo"></a>PDO::ErrorInfo
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Récupère les informations d’erreur étendues de la dernière opération effectuée sur le handle de base de données.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
array PDO::errorInfo();  
```  
  
## <a name="return-value"></a>Valeur de retour  
Tableau d’informations d’erreur sur la dernière opération effectuée sur le handle de base de données. Le tableau comprend les champs suivants :  
  
-   Le code d’erreur SQLSTATE  
  
-   Le code d’erreur propre au pilote  
  
-   Le message d’erreur propre au pilote  
  
En l’absence d’erreur, ou si la valeur SQLSTATE n’est pas définie, les champs propres au pilote ont la valeur Null.  
  
## <a name="remarks"></a>Notes  
PDO::ErrorInfo récupère uniquement les informations d’erreur pour les opérations exécutées directement sur la base de données. Utilisez PDOStatement::errorInfo quand vous créez une instance de PDOStatement à l’aide de PDO::prepare ou PDO::query.  
  
La prise en charge de PDO a été ajoutée dans la version 2.0 de [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)].  
  
## <a name="example"></a> Exemple  
Dans cet exemple, le nom de la colonne est mal orthographié (`Cityx` au lieu de `City`), ce qui provoque une erreur qui est ensuite signalée.  
  
```php
<?php  
$conn = new PDO( "sqlsrv:server=(local) ; Database = AdventureWorks ", "");  
$query = "SELECT * FROM Person.Address where Cityx = 'Essen'";  
  
$conn->query($query);  
print $conn->errorCode();  
echo "\n";  
print_r ($conn->errorInfo());  
?>  
```  

## <a name="additional-odbc-messages"></a>Messages ODBC supplémentaires

Quand une exception se produit, le pilote ODBC peut retourner plusieurs erreurs pour aider à diagnostiquer les problèmes. Cependant, PDO::errorInfo montre toujours seulement la première erreur. En réponse au [signalement de ce bogue](https://bugs.php.net/bug.php?id=78196), [PDO::errorInfo](https://www.php.net/manual/en/pdo.errorinfo.php) et [PDOStatement::errorInfo](https://www.php.net/manual/en/pdostatement.errorinfo.php) ont été mis à jour pour indiquer que les pilotes devaient afficher *au moins* les trois champs suivants :
```
0   SQLSTATE error code (a five characters alphanumeric identifier defined in the ANSI SQL standard).
1   Driver specific error code.
2   Driver specific error message.
```

À compter de 5.9.0, le comportement par défaut de PDO::errorInfo est de montrer les erreurs ODBC supplémentaires, si elles sont disponibles. Par exemple :

```php
<?php  
try {
    $conn = new PDO("sqlsrv:server=$server;", $uid, $pwd);
    $conn->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
    $stmt = $conn->prepare("SET NOCOUNT ON; USE $database; SELECT 1/0 AS col1");
    $stmt->execute();
} catch (PDOException $e) {
    var_dump($e->errorInfo);
}
?>  
```  

L’exécution du script ci-dessus doit avoir levé une exception, et la sortie se présente comme suit :

```
array(6) {
  [0]=>
  string(5) "01000"
  [1]=>
  int(5701)
  [2]=>
  string(91) "[Microsoft][ODBC Driver 17 for SQL Server][SQL Server]Changed database context to 'tempdb'."
  [3]=>
  string(5) "22012"
  [4]=>
  int(8134)
  [5]=>
  string(87) "[Microsoft][ODBC Driver 17 for SQL Server][SQL Server]Divide by zero error encountered."
}
```

Si l’utilisateur préfère le fonctionnement précédent, la nouvelle option de configuration `pdo_sqlsrv.report_additional_errors` peut être utilisée pour le désactiver. Ajoutez simplement la ligne suivante au début d’un script PHP :

```
ini_set('pdo_sqlsrv.report_additional_errors', 0);
```

Dans ce cas, les informations d’erreur affichées sont similaires à celles-ci lors de l’exécution du même exemple de script :

```
array(3) {
  [0]=>
  string(5) "01000"
  [1]=>
  int(5701)
  [2]=>
  string(91) "[Microsoft][ODBC Driver 17 for SQL Server][SQL Server]Changed database context to 'tempdb'."
}
```

Si nécessaire, l’utilisateur peut choisir d’ajouter la ligne suivante au fichier php.ini pour désactiver cette fonctionnalité dans tous ses scripts PHP :

```
pdo_sqlsrv.report_additional_errors = 0
```

## <a name="warnings-and-errors"></a>Avertissements et erreurs

À compter de 5.9.0, les avertissements ODBC ne seront plus enregistrés comme erreurs. Les [codes d’erreur](https://docs.microsoft.com/sql/odbc/reference/appendixes/appendix-a-odbc-error-codes) avec le préfixe « 01 » sont consignés comme avertissements. En d’autres termes, si l’utilisateur veut consigner seulement les erreurs, mettez à jour le fichier php.ini comme suit :

```
[pdo_sqlsrv]  
pdo_sqlsrv.log_severity = 1
```

Dans ce cas, le fichier journal ne contiendra aucun message d’avertissement. Découvrez comment la [journalisation](https://docs.microsoft.com/sql/connect/php/logging-activity#logging-activity-using-the-pdo_sqlsrv-driver) fonctionne pour les utilisateurs pdo_sqlsrv.

## <a name="see-also"></a>Voir aussi  
[PDO, classe](../../connect/php/pdo-class.md)

[PDO](https://php.net/manual/book.pdo.php)  

[PDOStatement::errorInfo](../../connect/php/pdostatement-errorinfo.md)
