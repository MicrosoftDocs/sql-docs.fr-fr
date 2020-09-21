---
title: Exécution de transactions avec le pilote JDBC
description: Découvrez comment JDBC Driver pour SQL Server prend en charge des transactions, y compris des niveaux d'isolation, des points de sauvegarde et la fonction Holdability du jeu de résultats.
ms.custom: ''
ms.date: 08/12/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: afbb776f-05dc-4e79-bb25-2c340483e401
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ff851fc3d62be939eab7983eee80a173160f0d5c
ms.sourcegitcommit: 129f8574eba201eb6ade1f1620c6b80dfe63b331
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/30/2020
ms.locfileid: "87435404"
---
# <a name="performing-transactions-with-the-jdbc-driver"></a>Exécution de transactions avec le pilote JDBC
[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

  Le traitement de transactions est une condition obligatoire pour toutes les applications qui doivent garantir la cohérence de leurs données permanentes. Avec [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)], le traitement transactionnel peut se faire localement ou de manière distribuée. Les transactions sont des modules d'exécution ACID (Atomicité, Cohérence, Isolation et Durabilité).  
  
 Les rubriques de cette section décrivent la manière dont le pilote JDBC prend en charge les transactions, y compris les niveaux d'isolation, les points d'enregistrement de transactions et la fonction holdability du jeu de résultats.  
  
## <a name="in-this-section"></a>Contenu de cette section  
  
|Rubrique|Description|  
|-----------|-----------------|  
|[Présentation des transactions](../../connect/jdbc/understanding-transactions.md)|Présente la manière dont les transactions sont utilisées avec le pilote JDBC.|  
|[Présentation des transactions XA](../../connect/jdbc/understanding-xa-transactions.md)|Présente la manière dont les transactions XA sont utilisées avec le pilote JDBC.|  
|[Présentation des niveaux d'isolation](../../connect/jdbc/understanding-isolation-levels.md)|Décrit les différents niveaux d'isolation pris en charge par le pilote JDBC.|  
|[Utilisation de points de sauvegarde](../../connect/jdbc/using-savepoints.md)|Décrit la manière d'utiliser le pilote JDBC avec des points d'enregistrement de transactions.|  
|[Utilisation de la fonctionnalité de mise en attente](../../connect/jdbc/using-holdability.md)|Décrit la manière d'utiliser le pilote JDBC avec la fonction holdability du jeu de résultats.|  
  
## <a name="see-also"></a>Voir aussi  
 [Présentation du pilote JDBC](../../connect/jdbc/overview-of-the-jdbc-driver.md)  
  
  
