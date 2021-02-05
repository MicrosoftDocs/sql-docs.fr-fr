---
title: PDO::commit
description: Référence API pour la fonction PDO::commit dans le Pilote Microsoft PDO_SQLSRV pour PHP pour SQL Server.
ms.custom: ''
ms.date: 08/10/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: a0db4a00-9700-4f49-ab16-6522dd1101d3
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 2a498e78ce994907d666e8fdc01a494234d858d2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202063"
---
# <a name="pdocommit"></a>PDO::commit
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Envoie à la base de données des commandes qui ont été émises après l’appel à [PDO::beginTransaction](../../connect/php/pdo-begintransaction.md) , et rétablit la connexion en mode de validation automatique.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
bool PDO::commit();  
```  
  
## <a name="return-value"></a>Valeur de retour  
La valeur est true si l’appel de méthode a réussi, false dans le cas contraire.  
  
## <a name="remarks"></a>Notes  
PDO::commit n’est pas affecté par (et n’affecte pas) la valeur de PDO::ATTR_AUTOCOMMIT.  
  
Consultez [PDO::beginTransaction](../../connect/php/pdo-begintransaction.md) pour obtenir un exemple qui utilise PDO::commit.  
  
La prise en charge de PDO a été ajoutée dans la version 2.0 de [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)].  
  
## <a name="see-also"></a>Voir aussi  
[PDO, classe](../../connect/php/pdo-class.md)

[PDO](https://php.net/manual/book.pdo.php)  
  
