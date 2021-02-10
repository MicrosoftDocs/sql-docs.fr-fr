---
description: Vue d’ensemble des données et des schémas multidimensionnels
title: Vue d’ensemble des schémas et des données multidimensionnels | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/08/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- multidimensional schemas and data
ms.assetid: ce37fa06-c581-4d80-9a9b-c3aa66408909
author: rothja
ms.author: jroth
ms.openlocfilehash: 2dcd54dfb8ba5797588c615f987830dcb8aa5736
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032181"
---
# <a name="overview-of-multidimensional-schemas-and-data"></a>Vue d’ensemble des données et des schémas multidimensionnels
## <a name="understanding-multidimensional-schemas"></a>Fonctionnement des schémas multidimensionnels  
 L’objet de métadonnées central dans ADO MD est le *cube*, qui se compose d’un ensemble structuré de dimensions, de hiérarchies, de niveaux et de membres associés.  
  
 Une *dimension* est une catégorie indépendante de données de votre base de données multidimensionnelle, dérivée de vos entités métier. Une dimension contient généralement des éléments à utiliser comme critères de requête pour les mesures de la base de données.  
  
 Une *hiérarchie* est un chemin d’agrégation d’une dimension. Une dimension peut avoir plusieurs niveaux de granularité, qui ont des relations parent-enfant. Une hiérarchie définit le mode de relation de ces niveaux.  
  
 Un *niveau* est une étape d’agrégation dans une hiérarchie. Pour les dimensions avec plusieurs couches d’informations, chaque couche est un niveau.  
  
 Un *membre* est un élément de données dans une dimension. En général, vous créez une légende ou décrivez une mesure de la base de données à l’aide de membres.  
  
 Les cubes sont représentés par des objets [CubeDef](../../reference/ado-md-api/cubedef-object-ado-md.md) dans ADO MD. Les dimensions, les hiérarchies, les niveaux et les membres sont également représentés par leurs objets ADO MD correspondants : [dimension](../../reference/ado-md-api/dimension-object-ado-md.md), [hiérarchie](../../reference/ado-md-api/hierarchy-object-ado-md.md), [niveau](../../reference/ado-md-api/level-object-ado-md.md)et [membre](../../reference/ado-md-api/member-object-ado-md.md).  
  
### <a name="dimensions"></a>Dimensions  
 Les dimensions d’un cube dépendent de vos entités métier et des types de données à modéliser dans la base de données. En règle générale, chaque dimension est un point d’entrée indépendant ou un mécanisme de sélection des données.  
  
 Par exemple, un cube contenant des données de ventes possède les cinq dimensions suivantes : vendeur, géographie, temps, produits et mesures. La dimension de mesures contient des valeurs de données de ventes réelles, tandis que les autres dimensions représentent des méthodes pour classer et regrouper les valeurs des données de ventes.  
  
 La dimension Geography possède l’ensemble de membres suivant :  
  
```console
{All, North America, Europe, Canada, USA, UK, Germany, Canada-West,  
Canada-East, USA-NW, USA-SW, USA-NE, USA-SE, England, Scotland,   
Wales,Ireland, Germany-North, Germany-South, Ottawa, Toronto,   
Vancouver, Calgary, Seattle, Boise, Los Angeles, Houston,   
Shreveport, Miami, Boston, New York, London, Dover, Glasgow,   
Edinburgh, Cardiff, Pembroke, Belfast, Derry, Berlin,   
Hamburg, Munich, Stuttgart}  
```  
  
### <a name="hierarchies"></a>Hierarchies  
 Les hiérarchies définissent les façons dont les niveaux d’une dimension peuvent être « regroupés » ou regroupés. Une dimension peut avoir plusieurs hiérarchies. Il existe une hiérarchie naturelle dans la dimension Geography :  
  
### <a name="levels"></a>Niveaux  
 Dans l’exemple de dimension Geography illustrée dans la figure précédente, chaque zone représente un niveau dans la hiérarchie.  
  
 Chaque niveau a un ensemble de membres, comme suit :  
  
-   Le monde `= {All}`  
  
-   Continents `= {North America, Europe}`  
  
-   Provenance `= {Canada, USA, UK, Germany}`  
  
-   Certaines `= {Canada-East, Canada-West, USA-NE, USA-NW, USA-SE, USA-SW, England, Ireland, Scotland, Wales, Germany-North, Germany-South}`  
  
-   Villes `= {Ottawa, Toronto, Vancouver, Calgary, Seattle, Boise, Los Angeles, Houston, Shreveport, Miami, Boston, New York, London, Dover, Glasgow, Edinburgh, Cardiff, Pembroke, Belfast, Derry, Berlin, Hamburg, Munich, Stuttgart}`  
  
### <a name="members"></a>Membres  
 Les membres au niveau feuille d’une hiérarchie n’ont pas d’enfants et les membres au niveau de la racine n’ont pas de parent. Tous les autres membres ont au moins un parent et au moins un enfant. Par exemple, une traversée partielle de l’arborescence hiérarchique dans la dimension Geography produit les relations parent-enfant suivantes :  
  
-   `{All} (parent of) {Europe, North America}`  
  
-   `{North America} (parent of) {Canada, USA}`  
  
-   `{USA} (parent of) {USA-NE, USA-NW, USA-SE, USA-SW}`  
  
-   `{USA-NW} (parent of) {Boise, Seattle}`  
  
 Les membres peuvent être consolidés dans une ou plusieurs hiérarchies par dimension. Considérez une dimension de temps où il existe deux façons de cumuler jusqu’au niveau Year à partir du niveau Days :  
  
 Cet exemple illustre également une autre caractéristique : certains membres du niveau semaine de la hiérarchie Year-Week n’apparaissent dans aucun niveau de la hiérarchie Year-Quarter. Par conséquent, une hiérarchie n’a pas besoin d’inclure tous les membres d’une dimension.  
  
## <a name="see-also"></a>Voir aussi  
 [Modèle objet ADO MD](../../reference/ado-md-api/ado-md-object-model.md)   
 [ADO (multidimensionnel) (ADO MD)](./ado-multidimensional-ado-md.md)   
 [Programmation avec ADO MD](./programming-with-ado-md.md)   
 [Utilisation d’ADO avec ADO MD](./using-ado-with-ado-md.md)   
 [Utilisation de données multidimensionnelles](./working-with-multidimensional-data.md)