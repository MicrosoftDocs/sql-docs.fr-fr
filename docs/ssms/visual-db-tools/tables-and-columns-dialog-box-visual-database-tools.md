---
description: Boîte de dialogue Tables et colonnes (Visual Database Tools)
title: Tables et colonnes, boîte de dialogue
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- vdt.dlgbox.tablesandcolumns
ms.assetid: 8cf27be1-e66d-4735-a428-9ab4b33af4f5
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.openlocfilehash: ce49303d14dc92811b383349c7fa8d74202ab8ea
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/04/2021
ms.locfileid: "101836936"
---
# <a name="tables-and-columns-dialog-box-visual-database-tools"></a>Boîte de dialogue Tables et colonnes (Visual Database Tools)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
Utilisez cette boîte de dialogue pour mapper une clé primaire dans une table à une clé étrangère dans une autre. Pour accéder à cette boîte de dialogue, dans le menu **Concepteur de tables** , cliquez sur **Relations**. Dans la boîte de dialogue **Relations de clé étrangère**, cliquez sur le champ **Spécification de tables et colonnes**, puis cliquez sur le bouton de sélection **(…)** situé à droite de la propriété.  
  
> [!NOTE]  
> Si la table est publiée pour réplication, vous devez apporter vos modifications au schéma à l’aide de l’instruction Transact-SQL [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md) ou de SMO (SQL Server Management Objects). Lorsque les modifications sont apportées au diagramme à l’aide du Concepteur de tables ou du Concepteur de diagrammes de base de données, celui-ci tente d’abandonner la table et de la recréer. Toutefois, il est impossible d'abandonner les objets publiés, par conséquent les modifications du schéma échoueront.  
  
## <a name="options"></a>Options  
**Nom de la relation**  
Indique le nom de la relation.  
  
**Table de clé primaire**  
Répertorie les tables contenues dans la base de données. Choisissez la table qui se trouvera du côté clé primaire de la relation.  
  
**Table de clé étrangère**  
Affiche la table située du côté clé étrangère de la relation. Il s'agit de la table actuellement sélectionnée dans le Concepteur de tables.  
  
**Grille sous la table de clé primaire**  
Répertorie les colonnes de la table sélectionnées dans la liste **Table de clé primaire** . Entrez les colonnes qui contribuent à la clé primaire de cette table.  
  
**Grille sous la table de clé étrangère**  
Répertorie les colonnes de la table sélectionnées dans la liste **Table de clé étrangère** . Entrez la colonne clé étrangère de la table de clé étrangère qui correspond à la colonne clé primaire.  
  
> [!NOTE]  
> Les colonnes que vous choisissez pour la clé étrangère doivent posséder le même type de données que les colonnes primaires auxquelles elles correspondent. Il doit exister un nombre égal de colonnes dans chacune des clés. Par exemple, si la clé primaire de la table située du côté clé primaire de la relation est composée de deux colonnes, vous devez faire correspondre chacune de ces colonnes à une colonne de la table située du côté clé étrangère de la relation.  
  
## <a name="see-also"></a>Voir aussi  
[Procédure : Créer des relations entre les tables](../../relational-databases/tables/create-foreign-key-relationships.md)  
