---
description: Récupération de données
title: Récupération des données | Microsoft Docs
ms.custom: ''
ms.date: 03/26/2018
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 3414992c-61c0-4e7d-b509-72517e52c1bb
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 35a9d9131b7c1cb382ab4e64baa26809d58c90d3
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88414425"
---
# <a name="retrieving-data"></a>Récupération de données
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Cette rubrique et celles de cette section expliquent comment récupérer des données.  
  
## <a name="sqlsrv-driver"></a>Pilote SQLSRV  
Le pilote SQLSRV de [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)] fournit les options suivantes pour récupérer des données à partir d’un jeu de résultats :  
  
-   [sqlsrv_fetch_array](../../connect/php/sqlsrv-fetch-array.md)  
  
-   [sqlsrv_fetch_object](../../connect/php/sqlsrv-fetch-object.md)  
  
-   [sqlsrv_fetch](../../connect/php/sqlsrv-fetch.md)/[sqlsrv_get_field](../../connect/php/sqlsrv-get-field.md)  
  
> [!NOTE]  
> Quand vous utilisez une des fonctions mentionnées ci-dessus, évitez les comparaisons Null en tant que critères de sortie des boucles. Étant donné que les fonctions **sqlsrv** retournent false quand une erreur se produit, le code suivant peut entraîner une boucle infinie suite à une erreur dans [sqlsrv_fetch_array](../../connect/php/sqlsrv-fetch-array.md):  
>   
> `/*``This code could result in an infinite loop. It is recommended that`  
>   
> `you do NOT use null comparisons as the criterion for exiting loops,`  
>   
> `as is done here. */`  
>   
> `do{`  
>   
> `$result = sqlsrv_fetch_array($stmt);`  
>   
> `} while( !is_null($result));`  
  
Si votre requête récupère plusieurs jeux de résultats, vous pouvez passer au prochain jeu de résultats avec [sqlsrv_next_result](../../connect/php/sqlsrv-next-result.md).  
  
À compter de la version 1.1 de [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)], vous pouvez utiliser [sqlsrv_has_rows](../../connect/php/sqlsrv-has-rows.md) pour voir si un jeu de résultats comporte des lignes.  
  
## <a name="pdo_sqlsrv-driver"></a>Pilote PDO_SQLSRV  
Le pilote PDO_SQLSRV de [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)] fournit les options suivantes pour récupérer des données à partir d’un jeu de résultats :  
  
-   [PDOStatement::fetch](../../connect/php/pdostatement-fetch.md)  
  
-   [PDOStatement::fetchAll](../../connect/php/pdostatement-fetchall.md)  
  
-   [PDOStatement::fetchColumn](../../connect/php/pdostatement-fetchcolumn.md)  
  
-   [PDOStatement::fetchObject](../../connect/php/pdostatement-fetchobject.md)  
  
Si votre requête récupère plusieurs jeux de résultats, vous pouvez passer au prochain jeu de résultats avec [PDOStatement::nextRowset](../../connect/php/pdostatement-nextrowset.md).  
  
Vous pouvez voir le nombre de lignes dans un jeu de résultats si vous spécifiez un curseur de défilement, puis appelez [PDOStatement::rowCount](../../connect/php/pdostatement-rowcount.md).  
  
[PDO::prepare](../../connect/php/pdo-prepare.md) permet de spécifier un type de curseur. Ensuite, avec [PDOStatement::fetch](../../connect/php/pdostatement-fetch.md) , vous pouvez sélectionner une ligne. Consultez [PDO::prepare](../../connect/php/pdo-prepare.md) pour obtenir un exemple et plus d’informations.  
  
## <a name="in-this-section"></a>Dans cette section  
  
|Rubrique|Description|  
|---------|---------------|  
|[Extraction de données sous la forme d’une chaîne](../../connect/php/retrieving-data-as-a-stream-using-the-sqlsrv-driver.md)|Fournit une vue d’ensemble de la manière de diffuser en continu des données à partir du serveur et indique des liens vers des cas d’usage spécifiques.|  
|[Utilisation de paramètres directionnels](../../connect/php/using-directional-parameters.md)|Décrit comment utiliser les paramètres directionnels lors de l’appel d’une procédure stockée.|  
|[Spécification d’un type de curseur et sélection de lignes](../../connect/php/specifying-a-cursor-type-and-selecting-rows.md)|Montre comment créer un jeu de résultats avec des lignes auxquelles vous pouvez accéder dans n’importe quel ordre.|  
|[Procédure : Récupérer des types date et heure sous forme de chaînes à l’aide du pilote SQLSRV](../../connect/php/how-to-retrieve-date-and-time-type-as-strings-using-the-sqlsrv-driver.md)|Décrit comment récupérer des types date et heure sous forme de chaînes à l’aide du pilote SQLSRV.|  
|[Procédure : Récupérer des types date et heure sous forme de chaînes à l’aide du pilote PDO_SQLSRV](../../connect/php/how-to-retrieve-datetime-objects-using-pdo-sqlsrv-driver.md)|Décrit comment récupérer des types date et heure sous forme d’objets à l’aide du pilote PDO_SQLSRV.|  
|[Mise en forme des chaînes décimales avec le pilote SQLSRV](../../connect/php/formatting-decimals-sqlsrv-driver.md)|Montre comment mettre en forme des valeurs décimales ou monétaires à l’aide du pilote SQLSRV.|  
|[Mise en forme des chaînes décimales avec le pilote PDO_SQLSRV](../../connect/php/formatting-decimals-pdo-sqlsrv-driver.md)|Montre comment mettre en forme des valeurs décimales ou monétaires à l’aide du pilote PDO_SQLSRV.|  
  
## <a name="related-sections"></a>Sections connexes  
[Procédure : Spécifier des types de données PHP](../../connect/php/how-to-specify-php-data-types.md)  
  
## <a name="see-also"></a>Voir aussi  
[Guide de programmation pour les pilotes Microsoft pour PHP pour SQL Server](../../connect/php/programming-guide-for-php-sql-driver.md)

[Récupération de données](../../connect/php/retrieving-data.md)  
  
