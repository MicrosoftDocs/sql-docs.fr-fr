---
description: Représentation des jointures dans le Concepteur de requêtes et de vues (Visual Database Tools)
title: Représentation des jointures dans le Concepteur de requêtes et de vues
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- SQL pane [Visual Database Tools]
- joins [SQL Server], Query and View Designer
- Diagram pane [Visual Database Tools]
ms.assetid: 20a99dcb-83bd-4aa6-9139-92e2e5ba4887
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.openlocfilehash: 11b6399adb28a81f5364e6073a6fb0fde757ffe7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338058"
---
# <a name="how-the-query-and-view-designer-represents-joins-visual-database-tools"></a>Représentation des jointures dans le Concepteur de requêtes et de vues (Visual Database Tools)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
 Dans le cas de tables jointes, le [Concepteur de requêtes et de vues](../../ssms/visual-db-tools/query-and-view-designer-tools-visual-database-tools.md) représente la jointure graphiquement dans le [volet Schéma](../../ssms/visual-db-tools/diagram-pane-visual-database-tools.md) et il utilise la syntaxe SQL dans le [volet SQL](../../ssms/visual-db-tools/sql-pane-visual-database-tools.md).  
  
## <a name="diagram-pane"></a>Volet Schéma  
Dans le volet Schéma, le Concepteur de requêtes et de vues affiche une ligne de jointure entre les colonnes de données impliquées. Il affiche une ligne de jointure par condition de jointure. Par exemple, l'illustration suivante montre une ligne de jointure entre deux tables jointes :  
  
![La ligne de jointure illustre la relation entre deux tables](../../ssms/visual-db-tools/media/dv3wbig.gif "La ligne de jointure illustre la relation entre deux tables")  
  
Si des tables ont plusieurs conditions de jointure, le Concepteur de requêtes et de vues affiche plusieurs lignes de jointure, comme l'illustre l'exemple suivant :  
  
![Tables jointes à l'aide de plusieurs conditions de jointure](../../ssms/visual-db-tools/media/dv3w9n1.gif "Tables jointes à l'aide de plusieurs conditions de jointure")  
  
Si les colonnes de données jointes ne sont pas affichées (parce que, par exemple, le rectangle représentant la table ou l'objet table est réduit ou la jointure implique une expression), le Concepteur de requêtes et de vues place la ligne de jointure dans la barre de titre du rectangle représentant la table ou l'objet structuré en table.  
  
La forme de l'icône au milieu de la ligne de jointure indique la façon dont les tables ou les objets structurés en table sont joints. Si la clause de jointure utilise un autre opérateur que le signe égal (=), il apparaît dans l’icône de la ligne de jointure. Le tableau suivant fait la liste des icônes qui apparaissent dans la ligne de jointure.  
  
|**Icône de la ligne de jointure**|**Description**|  
|----------------------|-------------------|  
|:::image type="icon" source="../../ssms/visual-db-tools/media/dv3wbih.gif":::|Jointure interne (créée avec un opérateur « égal »).|  
|:::image type="icon" source="../../ssms/visual-db-tools/media/dv3wbii.gif":::|Jointure interne basée sur l'opérateur « plus grand que ».|  
|:::image type="icon" source="../../ssms/visual-db-tools/media/dv3wbij.gif":::|Jointure externe dans laquelle toutes les lignes de la table représentée à gauche seront incluses, même si elles n'ont pas de correspondance dans la table en relation.|  
|:::image type="icon" source="../../ssms/visual-db-tools/media/dv3wbik.gif":::|Jointure externe dans laquelle toutes les lignes de la table représentée à droite seront incluses, même si elles n'ont pas de correspondance dans la table en relation.|  
|:::image type="icon" source="../../ssms/visual-db-tools/media/dv3wbil.gif":::|Jointure externe entière dans laquelle toutes les lignes des deux tables seront incluses, même si elles n'ont pas de correspondance dans la table en relation.|  
  
Les symboles présents aux extrémités de la ligne de jointure indiquent le type de jointure. Le tableau suivant fait la liste des types de jointures et des icônes affichées aux extrémités de la ligne de jointure.  
  
|**Icône aux extrémités de la ligne de jointure**|**Type de jointure**|  
|---------------------------------|--------------------|  
|:::image type="icon" source="../../ssms/visual-db-tools/media/dv3wbim.gif":::|Jointure Un-à-un.|  
|:::image type="icon" source="../../ssms/visual-db-tools/media/dv3wbin.gif":::|Jointure Un-à-plusieurs.|  
|:::image type="icon" source="../../ssms/visual-db-tools/media/dv3wbio.gif":::|Le Concepteur de requêtes et de vues ne peut pas déterminer le type de jointure. Ce cas s'observe surtout lorsque vous avez créé une jointure manuellement.|  
  
## <a name="sql-pane"></a>volet SQL  
Dans une instruction SQL, une jointure peut être exprimée de plusieurs façons différentes. La syntaxe exacte dépend de la base de données utilisée et de la façon dont la jointure a été définie.  
  
Pour spécifier une jointure de tables, vous disposez de plusieurs options de syntaxe :  
  
-   **Qualificateur JOIN pour la clause FROM**.   Les mots clés INNER et OUTER spécifient le type de jointure. Il s'agit de la syntaxe standard du SQL ANSI 92.  
  
    Par exemple, si vous joignez les tables `publishers` et `pub_info` sur la base de la colonne `pub_id` de chaque table, vous obtenez une instruction SQL similaire à la suivante :  
  
    ```  
    SELECT *  
    FROM publishers INNER JOIN pub_info ON  
       publishers.pub_id = pub_info.pub_id  
    ```  
  
    En cas de jointure externe, le mot INNER est remplacé par les mots LEFT OUTER ou RIGHT OUTER.  
  
-   **Clause WHERE comparant des colonnes des deux tables**.   Une clause WHERE apparaît si la base de données ne prend pas en charge la syntaxe JOIN (ou si vous l'avez entrée vous-même). Si la jointure est créée dans la clause WHERE, les noms des deux tables apparaissent dans la clause FROM.  
  
    Par exemple, l'instruction suivante joint les tables `publishers` et `pub_info` .  
  
    ```  
    SELECT *  
    FROM publishers, pub_info  
    WHERE publishers.pub_id = pub_info.pub_id  
    ```  
  
## <a name="see-also"></a>Voir aussi  
[Interroger avec des jointures &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/query-with-joins-visual-database-tools.md)  
[Boîte de dialogue Joindre &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/join-dialog-box-visual-database-tools.md)  
  
