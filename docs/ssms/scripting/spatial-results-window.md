---
title: Fenêtre Résultats spatiaux
description: Découvrez comment utiliser la fenêtre Résultats spatiaux qui fournit des outils de mappage visuel pour l’affichage des données spatiales. Pour afficher des résultats spatiaux, vos résultats de requête doivent inclure une colonne spatiale avec des données géométriques ou géographiques.
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.prod: sql
ms.technology: ssms
ms.reviewer: ''
ms.topic: conceptual
ms.assetid: c2d5a477-6496-4d01-adee-7322ebdfadf3
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: b08fe5a2b40f07a6ffef93862512e5132dfbb6ae
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344155"
---
# <a name="spatial-results-window"></a>Fenêtre Résultats spatiaux
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  La fenêtre **Résultats spatiaux** propose des outils de mappage visuel permettant d'afficher des données spatiales. Pour afficher des résultats spatiaux, vos résultats de requête doivent inclure une colonne spatiale avec des données géométriques ou géographiques.  
  
> [!NOTE]  
>  La fenêtre **Résultats spatiaux** est uniquement disponible si vos résultats sont retournés dans une grille dans la fenêtre **Résultats** . Si vous spécifiez que les résultats doivent être retournés sous forme de texte, cette fenêtre n'est pas disponible.  
  
## <a name="options"></a>Options  
 **Sélectionner la colonne spatiale**  
 À partir des colonnes spatiales figurant dans les résultats de requête, spécifiez la colonne spatiale à afficher. Vous ne pouvez sélectionner qu'une seule colonne à la fois.  
  
 **Sélectionner la colonne d'étiquette**  
 À partir des colonnes retournées dans les résultats de requête, spécifiez la colonne non spatiale qui servira à étiqueter les données spatiales. Vous ne pouvez sélectionner qu'une seule colonne à la fois.  
  
 Cette option n'est pas disponible lorsque des instances de point seulement sont retournées dans une requête.  
  
 **Sélectionner la projection**  
 Affichez les données géographiques dans un des quatre projections : Équirectangulaire, de Mercator, de Robinson ou de Bonne.  
  
 Cette option n'est pas disponible pour les données géométriques.  
  
 **Zoom**  
 Ajustez l'affichage du mappage sur une échelle exponentielle.  
  
 **Afficher le quadrillage**  
 Activez/désactivez le quadrillage des coordonnées.  
  
 Pour les formes polygonales, l'étiquette est affichée uniquement lorsque la forme est suffisamment grande pour contenir le texte d'étiquette. Pour afficher les étiquettes des petites formes, ajustez le zoom.  
  
> [!NOTE]  
>  Il est impossible d'étiqueter les instances de point.  
  
## <a name="see-also"></a>Voir aussi  
 [Afficher des données spatiales dans l'Explorateur d'objets](./view-spatial-data-in-object-explorer.md)   
 [Données spatiales &#40;SQL Server&#41;](../../relational-databases/spatial/spatial-data-sql-server.md)   
 [Éditeur de requête du moteur de base de données &#40;SQL Server Management Studio&#41;](../f1-help/database-engine-query-editor-sql-server-management-studio.md)   
 [Éditeurs de texte et de requête &#40;SQL Server Management Studio&#41;](../f1-help/database-engine-query-editor-sql-server-management-studio.md)  
  
