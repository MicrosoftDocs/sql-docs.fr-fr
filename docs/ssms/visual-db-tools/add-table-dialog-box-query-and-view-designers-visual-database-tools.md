---
description: Boîte de dialogue Ajouter une table (Concepteurs de requêtes et de vues)
title: Ajouter une table, boîte de dialogue (Concepteurs de requêtes et de vues)
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- vdt.dlgbox.query.addtable
- vdtsql.chm:65565
ms.assetid: fce7adcc-4cf5-4a52-9203-11c13d1ecf08
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.openlocfilehash: 76fcb614bfab599f6f6a9e279e8b4f9341f6209c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100270797"
---
# <a name="add-table-dialog-box-query-and-view-designers-visual-database-tools"></a>Boîte de dialogue Ajouter une table (Concepteurs de requêtes et de vues)

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
Cette boîte de dialogue permet d'ajouter à une requête ou à une vue, des tables, des vues, des fonctions définies par l'utilisateur ou des synonymes.  
  
> [!NOTE]  
> Si la table est publiée pour réplication, vous devez apporter vos modifications au schéma à l’aide de l’instruction Transact-SQL [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md) ou de SMO (SQL Server Management Objects). Lorsque les modifications sont apportées au diagramme à l’aide du Concepteur de tables ou du Concepteur de diagrammes de base de données, celui-ci tente d’abandonner la table et de la recréer. Toutefois, il est impossible d'abandonner les objets publiés, par conséquent les modifications du schéma échoueront.  
  
## <a name="options"></a>Options  
**Tables**  
Énumère les tables que vous pouvez ajouter au volet **Schéma** . Pour ajouter une table, sélectionnez-la et cliquez sur **Ajouter**. Pour ajouter plusieurs tables à la fois, sélectionnez-les et cliquez sur **Ajouter**.  
  
**Views**  
Énumère les vues que vous pouvez ajouter au volet **Schéma** . Pour ajouter une vue, sélectionnez-la et cliquez sur **Ajouter**. Pour ajouter plusieurs vues à la fois, sélectionnez-les et cliquez sur **Ajouter**.  
  
**Fonctions**  
Énumère les fonctions définies par l’utilisateur que vous pouvez ajouter au volet **Schéma** . Pour ajouter une fonction, sélectionnez-la et cliquez sur **Ajouter**. Pour ajouter plusieurs fonctions à la fois, sélectionnez-les et cliquez sur **Ajouter**.  
  
**Synonymes**  
Énumère les synonymes que vous pouvez ajouter au volet **Schéma** . Pour ajouter un synonyme, sélectionnez-le et cliquez sur **Ajouter**. Pour ajouter plusieurs synonymes à la fois, sélectionnez-les et cliquez sur **Ajouter**.  
  
**Actualiser**  
Met à jour la liste pour ajouter toutes les modifications apportées à la base de données depuis la dernière récupération de la liste.  
  
**Ajouter**  
Ajoute chaque élément sélectionné.  
  
## <a name="see-also"></a>Voir aussi  
[Rubriques de procédures relatives à la conception de requêtes et de vues &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/design-queries-and-views-how-to-topics-visual-database-tools.md)  
  
