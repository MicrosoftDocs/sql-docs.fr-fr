---
title: Afficher les différences entre les données
description: Découvrez comment comparer deux bases de données et comment leurs objets de base de données diffèrent. Consultez comment afficher des enregistrements dans des objets et comment filtrer la vue.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- sql.data.tools.datacompare.f1
ms.assetid: f88d3350-2eaf-44cc-96a8-84008b6cd071
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 6516a9c39debe29befb106340c90920a70798044
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100017969"
---
# <a name="how-to-view-data-differences"></a>Procédure : Afficher les différences entre les données

Après avoir comparé les données de deux bases de données, vous pourrez voir chacun des *objets de base de données* que vous avez comparés ainsi que son état. Vous pouvez également afficher les résultats des enregistrements dans chaque objet, regroupés par état.  
  
Après avoir affiché les différences, vous pourrez mettre à jour la *cible* de sorte qu’elle corresponde à la *source* pour une partie ou l’ensemble des objets ou des enregistrements qui sont différents, manquants ou nouveaux.  
  
### <a name="to-view-data-differences"></a>Pour afficher les différences entre les données  
  
1.  Comparez les données dans une source et une cible.  
  
2.  (Facultatif) Effectuez une des actions suivantes ou les deux :  
  
    -   Par défaut, les résultats pour tous les objets apparaissent, indépendamment de leur état. Pour afficher uniquement les objets qui ont un état particulier, cliquez sur une option dans la liste **Filtrer**.  
  
    -   Pour afficher les résultats des enregistrements d’un objet en particulier, cliquez sur cet objet dans le volet de résultats principal, puis sur un onglet du volet Vue des enregistrements. Chaque onglet affiche tous les enregistrements de cet objet qui ont un état particulier : différent, uniquement dans la source, uniquement dans la cible et en double. Les données s'affichent par enregistrement et colonne.  
  
## <a name="see-also"></a>Voir aussi  
[Procédure : utiliser le schéma pour comparer différentes définitions de base de données](../ssdt/how-to-use-schema-compare-to-compare-different-database-definitions.md)  
  
