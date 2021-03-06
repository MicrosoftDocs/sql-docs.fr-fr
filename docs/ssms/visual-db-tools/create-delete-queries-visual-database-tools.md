---
description: Créer des requêtes Delete (Visual Database Tools)
title: Créer des requêtes Delete
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- row removal [SQL Server], Delete query
- Delete query
- queries [SQL Server], types
- removing rows
- removing data
- data deletions [SQL Server]
- deleting rows
- deleting data
ms.assetid: 0db3af43-1ec4-48c8-b769-2bb9c76d3434
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.openlocfilehash: 042d134a70c673052ed186aa94ca34fd4bc01faf
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88462875"
---
# <a name="create-delete-queries-visual-database-tools"></a>Créer des requêtes Delete (Visual Database Tools)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
Vous pouvez supprimer toutes les lignes d'une table à l'aide d'une requête Delete.  
  
> [!NOTE]  
> La suppression de toutes les lignes d'une table supprime les données de la table, mais ne supprime pas la table elle-même. Pour supprimer une table d’une base de données, cliquez avec le bouton droit sur la table dans l’Explorateur d’objets et cliquez sur **Supprimer**.  
  
Lorsque vous créez une requête Delete, le volet Critères change afin de refléter les options de suppression de lignes disponibles. Dans la mesure où vous n'affichez pas de données dans une requête Delete, les colonnes Sortie, Trier par et Ordre de tri sont supprimées. En outre, les cases à cocher en regard des noms de colonnes dans le rectangle représentant la table ou l'objet table sont supprimées puisqu'il est impossible de spécifier des colonnes individuelles à supprimer.  
  
Si le Concepteur de requêtes et de vues ne peut pas supprimer une ou plusieurs lignes, aucune ligne n'est supprimée et un message s'affiche pour indiquer la ligne contenant les informations qui ne peuvent pas être supprimées de la base de données.  
  
> [!CAUTION]  
> Il est impossible d'annuler l'action entraînée par l'exécution d'une requête Delete. Par sécurité, sauvegardez vos données avant d'exécuter une requête Delete.  
  
### <a name="to-create-a-delete-query"></a>Pour créer une requête Delete  
  
1.  Ajoutez la table dont vous souhaitez supprimer des lignes dans le volet Schéma.  
  
2.  Dans le menu **Concepteur de requêtes** , pointez sur **Modifier le type**, puis cliquez sur **Supprimer**. **Remarque** Si plusieurs tables sont affichées dans le volet Schéma quand vous commencez la requête Delete, le Concepteur de requêtes et de vues affiche la [boîte de dialogue Supprimer une table](../../ssms/visual-db-tools/delete-table-dialog-box-visual-database-tools.md) en vous invitant à indiquer le nom de la table dont vous souhaitez supprimer des lignes.  
  
Quand vous exécutez la requête Delete, aucun résultat n’apparaît dans le [volet Résultats](../../ssms/visual-db-tools/results-pane-visual-database-tools.md). En fait, un message indiquant le nombre de lignes supprimées s'affiche.  
  
## <a name="see-also"></a>Voir aussi  
[Types de requêtes pris en charge &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/supported-query-types-visual-database-tools.md)  
[Rubriques de procédures relatives à la conception de requêtes et de vues &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/design-queries-and-views-how-to-topics-visual-database-tools.md)  
  
