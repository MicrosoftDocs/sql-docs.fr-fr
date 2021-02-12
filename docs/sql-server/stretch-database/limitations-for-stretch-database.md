---
description: Limitations concernant Stretch Database
title: Limitations concernant Stretch Database
ms.date: 06/14/2016
ms.service: sql-server-stretch-database
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Stretch Database, limitations
- Stretch Database, blocking issues
- limitations (Stretch Database)
- blocking issues (Stretch Database)
ms.assetid: 2b1fbec1-7859-44fc-8417-724fc57a59c0
author: rothja
ms.author: jroth
ms.custom: seo-dt-2019
ms.openlocfilehash: 7aeb0f7cdbb911ace2ab5701e2a61befde568f2a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100080110"
---
# <a name="limitations-for-stretch-database"></a>Limitations concernant Stretch Database
[!INCLUDE [sqlserver2016-windows-only](../../includes/applies-to-version/sqlserver2016-windows-only.md)]


  En savoir plus sur les limitations concernant les tables compatibles Stretch et sur les limitations qui vous empêchent d’activer Stretch pour une table.  
  
##  <a name="limitations-for-stretch-enabled-tables"></a><a name="Caveats"></a> Limites des tables Stretch  
  
Les tables compatibles Stretch présentent les limitations suivantes.  
  
### <a name="constraints"></a>Contraintes  
-   L’unicité n’est pas appliquée pour les contraintes UNIQUE et les contraintes PRIMARY KEY dans la table Azure qui contient les données migrées.  
  
### <a name="dml-operations"></a>Opérations DML  
-   Vous ne pouvez pas mettre à jour ni supprimer des lignes qui ont été migrées, ou qui sont éligibles à la migration, dans une table compatible Stretch ou une vue qui inclut des tables compatibles Stretch.  
  
-   Vous ne pouvez pas insérer des lignes dans une table compatible Stretch sur un serveur lié.  
  
### <a name="indexes"></a>Index  
-   Vous ne pouvez pas créer d’index pour une vue qui inclut des tables compatibles avec Stretch.  
  
-   Les filtres sur les index [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne sont pas propagés à la table distante.  
  
##  <a name="limitations-that-currently-prevent-you-from-enabling-stretch-for-a-table"></a><a name="Limitations"></a> Limites empêchant l’activation de Stretch pour une table  
   
 Les éléments suivants vous empêchent d’activer Stretch pour une table.  
  
 ### <a name="table-properties"></a>Propriétés des tables  
-   Tables avec plus de 1 023 colonnes ou plus de 998 index  
  
-   FileTables ou tables qui contiennent des données FILESTREAM  
  
-   Tables qui sont répliquées ou qui utilisent activement le suivi des modifications ou la capture de données modifiées  
  
-   Tables optimisées en mémoire  
  
### <a name="data-types"></a>Types de données  
-   text, ntext et image  
  
-   timestamp  
  
-   sql_variant  
  
-   XML  
  
-   Types de données CLR, dont geometry, geography, hierarchyid et les types CLR définis par l’utilisateur  
  
 ### <a name="column-types"></a>Types de colonnes  
 -   COLUMN_SET  
  
-   Colonnes calculées  
  
### <a name="constraints"></a>Contraintes  
-   Contraintes par défaut et contraintes de validation  
  
-   Contraintes de clé étrangère référençant la table. Dans une relation parent-enfant (par exemple, Order et Order_Detail), vous pouvez activer Stretch pour la table enfant (Order_Detail), mais pas pour la table parent (Order).  
  
### <a name="indexes"></a>Index  
-   Index de texte intégral  
  
-   Index XML  
  
-   Index spatiaux  
  
-   Vues indexées qui référencent la table  
  
## <a name="see-also"></a>Voir aussi  
 [Identifier des bases de données et tables pour Stretch Database en exécutant Stretch Database Advisor](../../sql-server/stretch-database/stretch-database-databases-and-tables-stretch-database-advisor.md)   
 [Activer Stretch Database pour une base de données](../../sql-server/stretch-database/enable-stretch-database-for-a-database.md)   
 [Activer Stretch Database pour une table](../../sql-server/stretch-database/enable-stretch-database-for-a-table.md)  
  
  
