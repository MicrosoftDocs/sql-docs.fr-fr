---
description: Modes SQL (MySQLToSQL)
title: Modes SQL (MySQLToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: d840ee51-b863-4e77-84aa-37d3f094bfed
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: e09a38ce70b4f144f6da324525a7fee925d586a9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100074470"
---
# <a name="sql-modes-mysqltosql"></a>Modes SQL (MySQLToSQL)
SSMA pour MySQL peut fonctionner dans différents modes SQL et peut appliquer ces modes différemment pour différents clients.  
  
Les modes définissent la syntaxe SQL que MySQL doit prendre en charge, ainsi que le type de vérification de validation des données qu’il doit effectuer. Cela facilite l’utilisation de MySQL dans différents environnements et l’utilisation de MySQL avec SQL Server.  
  
## <a name="sql-modes-grid"></a>Grille des modes SQL :  
  
-   La grille des modes SQL au niveau racine contient les colonnes suivantes : **nom du mode SQL**, **modes SQL chargés** et **modes SQL effectifs**.  
  
-   Grille des modes SQL dans les bases de données catégorie, base de données, catégorie de table, catégorie d’instructions, catégories de vues, table, vue, fonctions, procédures, UDF et niveau d’objet d’événement contient les colonnes suivantes : **nom du mode SQL**, **modes SQL hérités** et **modes SQL effectifs**.  
  
-   La grille des modes SQL au niveau des procédures stockées, des fonctions stockées et des déclencheurs contient les colonnes suivantes : **nom du mode SQL**,  **modes SQL d’origine** et **modes SQL effectifs**.  
  
> [!NOTE]  
> Les modes de groupe sont affichés en gras, sous la colonne « nom du mode SQL ».  
  
## <a name="loaded-sql-modes"></a>Modes SQL chargés  
Il s’agit des modes SQL, qui sont définis au niveau de la session ou de la racine. Les modes SQL qui ont été chargés dans la base de données cible ne peuvent pas être modifiés ou modifiés.  
  
## <a name="inherited-sql-modes"></a>Modes SQL hérités  
Il s’agit des modes SQL, qui sont hérités du nœud parent correspondant.  
  
À l’exception de la catégorie des fonctions, des procédures, des catégories d’événements et des déclencheurs, ces modes SQL sont présents à tous les niveaux (base de données, catégorie de table, catégorie d’instructions, catégorie des vues, table, vue, fonctions, procédures, UDF et objet d’événement).  
  
> [!NOTE]  
> En activant la case à cocher **hériter du parent** , les modes SQL hérités peuvent être hérités du nœud parent. Par défaut, cette case à cocher reste activée.  
  
## <a name="original-sql-modes"></a>Modes SQL d’origine  
Il s’agit des modes SQL présents uniquement au niveau de la fonction, de la procédure et du déclencheur.  
  
> [!NOTE]  
> En activant la case à cocher **utiliser l’original** , vous pouvez utiliser les modes SQL qui étaient utilisés à l’origine dans la fonction ou le déclencheur correspondant. Par défaut, cette case à cocher reste activée.  
  
## <a name="effective-sql-modes"></a>Modes SQL effectifs  
Les modes SQL efficaces peuvent être définis à différents niveaux de la façon suivante :  
  
-   **Au niveau de la session :**  
  
    1.  Tous les modes SQL chargés peuvent être appelés « modes SQL effectifs ».  
  
    2.  À ce niveau, les modes SQL effectifs peuvent être modifiés directement et explicitement.  
  
    3.  Le mode SQL effectif défini explicitement n’est pas reflété en mode SQL chargé et est enfin appliqué à l’objet.  
  
-   **Au niveau de la fonction ou de la procédure ou du déclencheur :**  
  
    1.  Tous les modes SQL d’origine peuvent être appelés « modes SQL effectifs ».  
  
    2.  À ce niveau, le mode SQL effectif peut être modifié explicitement uniquement lorsque la case à cocher **utiliser l’original** est désactivée.  
  
    3.  Le mode SQL effectif défini explicitement n’est pas reflété en mode SQL d’origine et est enfin appliqué à l’objet.  
  
-   **À des nœuds autres que les fonctions, les procédures ou le niveau de déclencheur :**  
  
    1.  Tous les modes SQL hérités peuvent être appelés « modes SQL effectifs ».  
  
    2.  À ce niveau, le mode SQL effectif peut être modifié explicitement uniquement lorsque la case à cocher **hériter du parent** est désactivée.  
  
    3.  Le mode SQL effectif défini explicitement n’est pas reflété en mode SQL hérité et est enfin appliqué à l’objet.  
  
