---
title: Boîte de dialogue Joindre
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- vdt.ppg.joinline
- vdtsql.chm:69638
ms.assetid: 0d9516bb-4ad3-4fcf-bb77-93474dea698f
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.openlocfilehash: d049e23db7020ca84c4cec1e4ddc1ba5bbe84b4d
ms.sourcegitcommit: f3321ed29d6d8725ba6378d207277a57cb5fe8c2
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/06/2020
ms.locfileid: "86011715"
---
# <a name="join-dialog-box-visual-database-tools"></a>Boîte de dialogue Joindre (Visual Database Tools)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
Utilisez cette boîte de dialogue afin de spécifier des options de jointure de tables. Pour accéder à cette boîte de dialogue, sélectionnez une ligne de jointure dans le volet **Design** . Ensuite, dans la fenêtre **Propriétés**, cliquez sur **Condition et type de jointure**, puis sur le bouton de sélection **(…)** qui s’affiche à droite de la propriété.  
  
Par défaut, les tables connexes sont jointes par une jointure interne qui crée un jeu de résultats à partir des lignes contenant des informations correspondantes dans les colonnes de jointure. Les options de la boîte de dialogue **Joindre** vous permettent de définir une jointure basée sur un opérateur différent et de définir une jointure externe.  
  
Pour plus d’informations sur la jointure de tables, consultez [Interroger avec des jointures &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/query-with-joins-visual-database-tools.md).  
  
## <a name="options"></a>Options  
  
|**Terme**|**Définition**|  
|------------|------------------|  
|**Table**|Noms des tables ou des objets table impliqués dans la jointure. Les noms des tables sont affichés ici à titre d’information et ne peuvent pas être modifiés.|  
|**Colonne**|Noms des colonnes utilisées pour joindre les tables. L'opérateur dans la liste Opérateur définit la relation entre les données présentes dans les colonnes. Les noms des colonnes sont affichés ici à titre d’information et ne peuvent pas être modifiés.|  
|**Opérateur**|Spécifiez l'opérateur utilisé pour mettre en relation les colonnes de jointure. Pour spécifier un opérateur autre que le signe (=), sélectionnez-le dans la liste. Lorsque vous fermez la page de propriétés, l'opérateur sélectionné apparaît dans le graphique en forme de losange de la ligne de jointure, comme dans l'illustration suivante :<br /><br />![Icône Visual Database Tools](../../ssms/visual-db-tools/media/dv3wbii.gif "Icône Visual Database Tools")|  
|**Toutes les lignes de : <table1>**|Spécifie que toutes les lignes de la table de gauche apparaissent dans la sortie, y compris s'il n'existe aucune correspondance dans la table de droite. Les colonnes auxquelles ne correspond aucune donnée dans la table de droite apparaissent avec la valeur NULL. Choisir cette option équivaut à spécifier LEFT OUTER JOIN dans l'instruction SQL.|  
|**Toutes les lignes de : <table2>**|Spécifie que toutes les lignes de la table de droite apparaissent dans la sortie, y compris s'il n'existe aucune correspondance dans la table de gauche. Les colonnes auxquelles ne correspond aucune donnée dans la table de gauche apparaissent avec la valeur NULL. Choisir cette option équivaut à spécifier RIGHT OUTER JOIN dans l'instruction SQL.|  
  
Sélectionner **Toutes les lignes à partir de <table1>** : et **Toutes les lignes à partir de <table2>** : équivaut à spécifier FULL OUTER JOIN dans l’instruction SQL.  
  
Lorsque vous sélectionnez une option pour créer une jointure externe, le losange sur la ligne de jointure change pour indiquer qu'il s'agit d'une jointure externe gauche, droite ou entière.  
  
> [!NOTE]  
> Les mots « gauche » et « droite » ne correspondent pas forcément à la position des tables dans le volet Schéma. « Gauche » désigne la table dont le nom apparaît à gauche du mot clé JOIN dans l'instruction SQL et « droite » la table dont le nom apparaît à droite du mot clé JOIN. Si vous déplacez les tables dans le volet **Schéma** , vous ne changez en rien cette désignation.  
  
## <a name="see-also"></a>Voir aussi  
[Interroger avec des jointures &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/query-with-joins-visual-database-tools.md)  
[Rubriques de procédures relatives à la conception de requêtes et de vues &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/design-queries-and-views-how-to-topics-visual-database-tools.md)  
  
