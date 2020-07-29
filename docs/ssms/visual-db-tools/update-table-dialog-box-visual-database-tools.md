---
title: Boîte de dialogue Mettre à jour une table
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- vdt.dlgbox.updatetable
- vdtsql.chm:69643
ms.assetid: 174c7275-5b15-42a9-b172-5ff30de575a1
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.openlocfilehash: 19251316d4674ad7632c6f220f85e2178f4365ca
ms.sourcegitcommit: f3321ed29d6d8725ba6378d207277a57cb5fe8c2
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/06/2020
ms.locfileid: "86004125"
---
# <a name="update-table-dialog-box-visual-database-tools"></a>Mettre à jour une table, boîte de dialogue (Visual Database Tools)

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Cette boîte de dialogue permet de spécifier la table à mettre à jour.

Elle apparaît si plusieurs tables sont affichées dans le volet Schéma lorsque vous modifiez le type d'une requête pour en faire une requête Update.  

Sélectionnez la table à mettre à jour, puis choisissez **OK**.

> [!NOTE]
> Si la table est publiée pour réplication, vous devez apporter vos modifications au schéma à l’aide de l’instruction Transact-SQL [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md) ou de SMO (SQL Server Management Objects). Lorsque les modifications sont apportées au diagramme à l’aide du Concepteur de tables ou du Concepteur de diagrammes de base de données, celui-ci tente d’abandonner la table et de la recréer. Toutefois, il est impossible d'abandonner les objets publiés, par conséquent les modifications du schéma échoueront.

## <a name="see-also"></a>Voir aussi

[Créer des requêtes de mise à jour](../../ssms/visual-db-tools/create-update-queries-visual-database-tools.md)