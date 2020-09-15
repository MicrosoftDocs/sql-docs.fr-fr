---
description: Boîte de dialogue Classement (Visual Database Tools)
title: Boîte de dialogue Classement
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- vdt.dlgbox.definecolumncollation
- vdtsql.chm:65561
ms.assetid: e4020f79-7abf-4839-b9b2-984ef7049817
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.openlocfilehash: c1e4c82b90c9b3c569f9f179ed16242c4ca76f40
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88314655"
---
# <a name="collation-dialog-box-visual-database-tools"></a>Boîte de dialogue Classement (Visual Database Tools)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
Cette boîte de dialogue permet de spécifier une séquence de classement pour la colonne. La séquence de classement est utilisée pour les opérations qui comparent les valeurs de la colonne à une autre colonne ou par rapport à des valeurs constantes. Elle affecte également le comportement de certaines fonctions de chaîne, telles que SUBSTRING et CHARINDEX. Pour une liste complète des effets du paramètre de classement d'une colonne, consultez la documentation sur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
Cette boîte de dialogue apparaît :  
  
-   Si vous entrez un nom de classement non valide dans le champ **Classement** de l'onglet **Propriétés des colonnes**  
  
-   Si vous cliquez dans le champ **Classement** de l’onglet **Propriétés des colonnes**, puis sur le bouton de sélection ( **...** ) à droite du champ.  
  
## <a name="options"></a>Options  
**Classement SQL**  
Choisi parmi les séquences de classement définies par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans la liste déroulante.  
  
**Classement Windows**  
Choisi parmi les séquences de classement définies par Windows dans la liste déroulante.  
  
**Tri binaire**  
Utilise les codes binaires des valeurs de caractères pour les comparaisons. Si vous sélectionnez cette option, certaines options de comparaison alphabétique ne seront plus disponibles. Par exemple, les comparaisons qui ne respectent pas la casse ne seront plus disponibles car les majuscules et les minuscules ont des encodages binaires différents. S'applique uniquement si vous sélectionnez **Classement Windows**.  
  
**Tri du dictionnaire**  
Utilise les options de comparaison alphabétique. S'applique uniquement si vous sélectionnez **Classement Windows**. Les options de comparaison alphabétique sont :  
  
-   **Respecter la casse** Sélectionnez cette option si vous souhaitez que les comparaisons différencient les majuscules et les minuscules.  
  
-   **Respecter les accents** Sélectionnez cette option si vous souhaitez que les comparaisons différencient les lettres accentuées et non accentuées. Si vous sélectionnez ceci, les comparaisons différencieront également les lettres différemment accentuées.  
  
-   **Respecter le jeu de caractères Kana** Sélectionnez cette option si vous souhaitez que les comparaisons différencient les syllabes japonaises katakana et hiragana.  
  
-   **Respecter la largeur** Sélectionnez cette option si vous souhaitez que les comparaisons différencient les caractères pleine largeur et demi-largeur.  
  
**Bouton Paramètres par défaut**  
Applique à la colonne la séquence de classement par défaut pour la base de données.  
  
## <a name="see-also"></a>Voir aussi  
[Utiliser des colonnes dans des requêtes d'agrégation &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/work-with-columns-in-aggregate-queries-visual-database-tools.md)  
  
