---
description: Boîte de dialogue Contraintes de validation (Visual Database Tools)
title: Boîte de dialogue Contrainte de validation
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- vdt.dlgbox.checkconstraint
ms.assetid: ad0bbf7f-b0de-406a-bd0a-cb779816b101
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.openlocfilehash: b37e339dc13577bd30491f1c155ebb8114868d60
ms.sourcegitcommit: 22dacedeb6e8721e7cdb6279a946d4002cfb5da3
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/14/2020
ms.locfileid: "92038936"
---
# <a name="check-constraint-dialog-box-visual-database-tools"></a>Boîte de dialogue Contraintes de validation (Visual Database Tools)

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

 Cette boîte de dialogue apparaît quand vous cliquez avec le bouton droit sur une grille de définition de table dans le Concepteur de tables puis cliquez sur **Vérifier les contraintes**. Elle contient un jeu de propriétés pour les contraintes non uniques jointes aux tables de votre base de données. Les propriétés qui s’appliquent aux contraintes uniques apparaissent dans la boîte de dialogue **Index/Clés** .  
  
> [!NOTE]  
> Si la table est publiée pour réplication, vous devez apporter vos modifications au schéma à l’aide de l’instruction Transact-SQL [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md) ou de SMO (SQL Server Management Objects). Lorsque les modifications sont apportées au diagramme à l’aide du Concepteur de tables ou du Concepteur de diagrammes de base de données, celui-ci tente d’abandonner la table et de la recréer. Toutefois, il est impossible d'abandonner les objets publiés, par conséquent les modifications du schéma échoueront.  
  
## <a name="options"></a>Options

**Contraintes de validation sélectionnées**  
Répertorie les contraintes de validation disponibles. Pour afficher les propriétés d'une contrainte, sélectionnez-la dans la liste.  
  
**Ajouter**  
Crée une nouvelle contrainte pour la table de base de données sélectionnée et fournit un nom par défaut ainsi que d'autres valeurs pour la contrainte. La contrainte ne devient valide que lorsqu'une expression est entrée pour celle-ci.  
  
**Supprimer**  
Supprime la contrainte sélectionnée de la table. Pour annuler l'ajout d'une contrainte de validation, supprimez la contrainte à l'aide de ce bouton.  
  
**Catégorie Général**  
Se développe pour afficher le champ de la propriété **Expression** .  
  
**Expression**  
Affiche l'expression pour la contrainte de validation sélectionnée. Pour les nouvelles contraintes, vous devez entrer l'expression avant de quitter cette zone. Vous pouvez également modifier des contraintes de validation existantes. Pour plus d’informations, consultez [Utilisation des contraintes](../../relational-databases/tables/unique-constraints-and-check-constraints.md).  
  
**Catégorie Identité**  
Se développe pour afficher les propriétés de **Nom** et **Description**.  
  
**Nom**  
Indique le nom de la contrainte de validation sélectionnée. Pour modifier le nom de cette contrainte, tapez directement le texte dans le champ de la propriété.  
  
**Description**  
Description de cette contrainte de validation. Vous pouvez modifier la description en la tapant dans le champ de propriété ou cliquer sur le bouton de sélection (**...**) qui s’affiche à droite du champ de propriété et modifier la description dans la boîte de dialogue **Propriété de la description**.  
  
**Catégorie Concepteur de tables**  
Se développe pour afficher les propriétés de **Vérifier les données existantes à la création ou à la réactivation**, **Appliquer INSERTs et UPDATEs**et **Appliquer la réplication**.  
  
**Check Existing Data On Creation or Re-Enabling**  
Spécifie si toutes les données préexistantes (données qui existaient dans la table avant la création de la contrainte) sont vérifiées par rapport à la contrainte.  
  
**Appliquer INSERTs et UPDATEs**  
Spécifie si la contrainte est appliquée lors de l'insertion ou de la mise à jour de données dans la table.  
  
**Appliquer la réplication**  
Indique si la contrainte doit être appliquée lorsqu'un Agent de réplication effectue une requête Insert ou Update sur cette table.  
  
## <a name="see-also"></a>Voir aussi

[Utilisation des contraintes](../../relational-databases/tables/unique-constraints-and-check-constraints.md)
[Boîtes de dialogue Index - Clés &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/indexes-keys-dialog-box-visual-database-tools.md)