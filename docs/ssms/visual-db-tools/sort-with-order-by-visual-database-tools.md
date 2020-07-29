---
title: Trier avec ORDER BY
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- ORDER BY clause [Visual Database Tools]
ms.assetid: 459f5640-8058-4c24-97e7-7bbd6168bc39
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.openlocfilehash: 840ceec41624cf11604d4db2b2eb90f9b4c03c53
ms.sourcegitcommit: f3321ed29d6d8725ba6378d207277a57cb5fe8c2
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/06/2020
ms.locfileid: "85999315"
---
# <a name="sort-with-order-by-visual-database-tools"></a>Trier avec ORDER BY (Visual Database Tools)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
Vous pouvez trier les résultats d'une requête sur la base d'une ou plusieurs colonnes des lignes retournées à l'aide d'une clause ORDER BY. Vous pouvez définir une clause ORDER BY en choisissant des options dans le volet Critères.  
  
### <a name="to-sort-a-query-using-an-order-by-clause"></a>Pour trier une requête à l'aide d'une clause ORDER BY  
  
1.  Ouvrez une requête ou créez-en une nouvelle.  
  
2.  Dans le [volet Critères](../../ssms/visual-db-tools/criteria-pane-visual-database-tools.md), cliquez sur la colonne **Type de tri** pour la ligne correspondant à la colonne que vous souhaitez utiliser pour trier les résultats de votre requête.  
  
3.  Choisissez *Croissant* ou *Décroissant* dans la liste déroulante.  
  
> [!NOTE]  
> Si vous désactivez l’entrée **Type de tri** d’une colonne, celle-ci est supprimée de la clause ORDER BY.  
  
Remarquez qu'au fur et à mesure que vous travaillez dans le volet Critères, la clause UNION de votre requête se modifie pour correspondre à vos actions les plus récentes.  
  
> [!NOTE]  
> Quand vous triez des résultats par plusieurs colonnes, spécifiez l’ordre dans lequel la recherche doit être effectuée à l’aide de la colonne **Ordre de tri** . Pour plus d’informations, consultez **Procédure : tri de plusieurs colonnes dans des requêtes**.  
  
## <a name="see-also"></a>Voir aussi  
[Trier et regrouper des résultats de requête &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/sort-and-group-query-results-visual-database-tools.md)  
[Résumer les résultats de la requête &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/summarize-query-results-visual-database-tools.md)  
[Rubriques de procédures relatives à la conception de requêtes et de vues &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/design-queries-and-views-how-to-topics-visual-database-tools.md)  
  
