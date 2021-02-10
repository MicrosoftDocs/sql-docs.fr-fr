---
description: Utilisation de données multidimensionnelles
title: Utilisation de données multidimensionnelles | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/08/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- multidimensional data [ADO]
ms.assetid: 84387746-aa3e-44fd-ad6c-a8214a6966dc
author: rothja
ms.author: jroth
ms.openlocfilehash: 33ae3c1ca1cb8f89378b4f2ea468b41d6d3a4ac5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036619"
---
# <a name="working-with-multidimensional-data"></a>Utilisation de données multidimensionnelles
Un jeu de *cellules* est le résultat d’une requête sur des données multidimensionnelles. Il se compose d’une collection d’axes, généralement pas plus de quatre axes et en général seulement de deux ou trois. Un *axe* est une collection de membres d’une ou de plusieurs dimensions, qui est utilisée pour rechercher ou filtrer des valeurs spécifiques dans un cube.  
  
 Une *position* est un point le long d’un axe. Pour un axe constitué d’une seule dimension, ces positions sont un sous-ensemble des membres de dimension. Si un axe est constitué de plusieurs dimensions, chaque position est une entité composée, qui comporte *n* parties, où *n* est le nombre de dimensions orientées le long de cet axe. Chaque partie de la position est un membre d’une dimension constitutive.  
  
 Par exemple, si les dimensions Geography et Product d’un cube contenant des données de ventes sont orientées le long de l’axe x d’un CellSet, une position le long de cet axe peut contenir les membres « USA » et « Computers ». Dans cet exemple, la détermination d’une position le long de l’axe des x requiert que les membres de chaque dimension soient orientés le long de l’axe.  
  
 Une *cellule* est un objet positionné à l’intersection des coordonnées de l’axe. Plusieurs éléments d’information sont associés à chaque cellule, y compris les données elles-mêmes, une chaîne mise en forme (forme affichable des données de cellule) et la valeur ordinale de la cellule. (Chaque cellule est une valeur ordinale unique dans l’CellSet. La valeur ordinale de la première cellule de l’CellSet est égale à zéro, tandis que la cellule la plus à gauche de la deuxième ligne d’un CellSet avec huit colonnes a une valeur ordinale de huit.  
  
 Par exemple, un cube présente les six dimensions suivantes (Notez que ce schéma de cube diffère légèrement de l’exemple donné dans [vue d’ensemble des schémas et des données multidimensionnels](./overview-of-multidimensional-schemas-and-data.md)) :  
  
-   Salesperson  
  
-   Géographie (hiérarchie naturelle)-continents, pays, États, etc.  
  
-   Trimestres-trimestres, mois, jours  
  
-   Années  
  
-   Measures-Sales, PercentChange, BudgetedSales  
  
-   Produits  
  
 L’ensemble de cellules suivant représente les ventes de 1991 pour tous les produits :  
  
> [!NOTE]
>  Les valeurs de cellule de l’exemple peuvent être affichées sous forme de paires ordonnées d’ordinaux de position d’axe où le premier chiffre représente la position de l’axe x et le second chiffre de la position de l’axe y.  
  
 Les caractéristiques de cet CellSet sont les suivantes :  
  
-   Dimensions de l’axe : trimestres, vendeur, géographie  
  
-   Dimensions de filtre : mesures, années, produits  
  
-   Deux axes : colonne (x ou axe 0) et ligne (y ou axe 1)  
  
-   axe des x : deux dimensions imbriquées, commercial et géographie  
  
-   axe des y : dimension quartiers  
  
 L’axe des x comporte deux dimensions imbriquées : SalesPerson et geography. À partir de la zone géographique, quatre membres sont sélectionnés : Seattle, Boston, USA-Sud et Japon. Deux membres sont sélectionnés dans vendeur : Valentine et Nash. Cela donne un total de huit positions sur cet axe (8 = 4 * 2).  
  
 Chaque coordonnée est représentée comme une position avec deux membres, une de la dimension SalesPerson et une autre de la dimension Geography :  
  
```console
(Valentine, Seattle), (Valentine, Boston), (Valentine, USA_North),  
(Valentine, Japan), (Nash, Seattle), (Nash, Boston), (Nash, USA_North),  
(Nash, Japan)  
```  
  
 L’axe des y n’a qu’une seule dimension, contenant les huit positions suivantes :  
  
```console
Jan, Feb, Mar, Qtr2, Qtr3, Oct, Nov, Dec  
```  
  
 Les cellules, les cellules, les axes et les positions sont tous représentés dans ADO MD par des objets [correspondants : ensemble de cellules,](../../reference/ado-md-api/cellset-object-ado-md.md) [cellule](../../reference/ado-md-api/cell-object-ado-md.md), [axe](../../reference/ado-md-api/axis-object-ado-md.md)et [position](../../reference/ado-md-api/position-object-ado-md.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Modèle objet ADO MD](../../reference/ado-md-api/ado-md-object-model.md)   
 [ADO (multidimensionnel) (ADO MD)](./ado-multidimensional-ado-md.md)   
 [Vue d’ensemble des schémas et des données multidimensionnels](./overview-of-multidimensional-schemas-and-data.md)   
 [Programmation avec ADO MD](./programming-with-ado-md.md)   
 [Utilisation d’ADO avec ADO MD](./using-ado-with-ado-md.md)