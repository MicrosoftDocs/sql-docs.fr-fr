---
title: PDO::errorCode
description: Référence API pour la fonction PDO::errorCode dans le Pilote Microsoft PDO_SQLSRV pour PHP pour SQL Server.
ms.custom: ''
ms.date: 08/10/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 5864b1d8-6814-41cd-a88d-415124484c13
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ed2a580a972375a14c5ed6532e40eb7db43373a5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202041"
---
# <a name="pdoerrorcode"></a>PDO::errorCode
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

PDO::errorCode récupère la valeur SQLSTATE de la dernière opération sur le handle de base de données.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
mixed PDO::errorCode();  
```  
  
## <a name="return-value"></a>Valeur de retour  
PDO::errorCode retourne une valeur SQLSTATE à cinq caractères sous la forme d’une chaîne, ou NULL si aucune opération n’a eu lieu sur le handle de base de données.  
  
## <a name="remarks"></a>Notes  
PDO::errorCode dans le pilote PDO_SQLSRV retourne des avertissements sur certaines opérations réussies. Par exemple, sur une connexion réussie, PDO::errorCode retourne 01000, ce qui indique SQL_SUCCESS_WITH_INFO.  
  
PDO::errorCode récupère uniquement les codes d’erreur des opérations exécutées directement sur la connexion de base de données. Si vous créez une instance PDOStatement par le biais de PDO::prepare ou PDO::query et qu’une erreur est générée sur l’objet d’instruction, PDO::errorCode ne récupère pas cette erreur. Vous devez appeler PDOStatement::errorCode pour retourner le code d’erreur d’une opération effectuée sur un objet d’instruction particulier.  
  
La prise en charge de PDO a été ajoutée dans la version 2.0 de [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)].  
  
## <a name="example"></a> Exemple  
Dans cet exemple, le nom de la colonne est mal orthographié (`Cityx` au lieu de `City`), ce qui provoque une erreur qui est ensuite signalée.  
  
```  
<?php  
$conn = new PDO( "sqlsrv:server=(local) ; Database = AdventureWorks ", "", "");  
$query = "SELECT * FROM Person.Address where Cityx = 'Essen'";  
  
$conn->query($query);  
print $conn->errorCode();  
?>  
```  
  
## <a name="see-also"></a>Voir aussi  
[PDO, classe](../../connect/php/pdo-class.md)

[PDO](https://php.net/manual/book.pdo.php)  
  
