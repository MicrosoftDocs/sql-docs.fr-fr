---
title: PDO::exec
description: Référence API pour la fonction PDO::exec dans le Pilote Microsoft PDO_SQLSRV pour PHP pour SQL Server.
ms.custom: ''
ms.date: 08/10/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 359a87c6-c13a-4518-8f23-a922e7f3b171
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ad79792d21e9b097f6aee681b1097c8aacfa44fa
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202013"
---
# <a name="pdoexec"></a>PDO::exec
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Prépare et exécute une instruction SQL dans un appel de fonction unique, en retournant le nombre de lignes affectées par l’instruction.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
int PDO::exec ($statement)  
```  
  
#### <a name="parameters"></a>Paramètres  
*$statement*: chaîne qui contient l’instruction SQL à exécuter.  
  
## <a name="return-value"></a>Valeur de retour  
Entier indiquant le nombre de lignes affectées.  
  
## <a name="remarks"></a>Notes  
Si *$statement* contient plusieurs instructions SQL, le nombre de lignes affectées est indiqué pour la dernière instruction uniquement.  
  
PDO::exec ne retourne pas de résultats pour une instruction SELECT.  
  
Les attributs suivants affectent le comportement de PDO::exec :  
  
-   PDO::ATTR_DEFAULT_FETCH_MODE  
  
-   PDO::SQLSRV_ATTR_ENCODING  
  
-   PDO::SQLSRV_ATTR_QUERY_TIMEOUT  
  
Pour plus d’informations, consultez [PDO::setAttribute](../../connect/php/pdo-setattribute.md). 
  
La prise en charge de PDO a été ajoutée dans la version 2.0 de [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)].  
  
## <a name="example"></a> Exemple  
Cet exemple supprime les lignes dans Table1 qui comportent « xxxyy » dans col1. L’exemple indique ensuite le nombre de lignes qui ont été supprimées.  
  
```  
<?php  
   $c = new PDO( "sqlsrv:server=(local)");  
  
   $c->exec("use Test");  
   $ret = $c->exec("delete from Table1 where col1 = 'xxxyy'");  
   echo $ret;  
?>  
```  
  
## <a name="see-also"></a>Voir aussi  
[PDO, classe](../../connect/php/pdo-class.md)

[PDO](https://php.net/manual/book.pdo.php)  
  
