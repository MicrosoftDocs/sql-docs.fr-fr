---
description: Arguments et propriétés de procédures stockées d'index spatial
title: Arguments et propriétés des procédures stockées d’index spatial | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- spatial indexes [SQL Server], stored procedures
ms.assetid: ee26082b-c0ed-40ff-b5ad-f5f6b00f0475
author: markingmyname
ms.author: maghan
ms.openlocfilehash: f9faf0901643b07420f8627d6afda4b9713d034a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201755"
---
# <a name="spatial-index-stored-procedures---arguments-and-properties"></a>Procédures stockées d’index spatial-arguments et propriétés
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Cette rubrique documente les arguments et propriétés pour les procédures stockées d'index spatial.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
 Pour la syntaxe de procédures stockées d'index spatial spécifiques, consultez les rubriques suivantes :  
  
-   [sp_help_spatial_geometry_index &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-spatial-geometry-index-transact-sql.md)  
  
-   [sp_help_spatial_geometry_index_xml &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-spatial-geometry-index-xml-transact-sql.md)  
  
-   [sp_help_spatial_geography_index &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-spatial-geography-index-transact-sql.md)  
  
-   [sp_help_spatial_geography_index_xml &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-spatial-geography-index-xml-transact-sql.md)  
  
## <a name="arguments"></a>Arguments  
`[ @tabname = ] 'tabname'` Nom qualifié ou non qualifié de la table pour laquelle l’index spatial a été spécifié.  
  
 Les guillemets ne sont nécessaires que si une une table qualifiée est spécifiée. Si un nom qualifié complet (incluant un nom de base de données) est fourni, le nom de base de données doit être celui de la base de données active. *tabname* est de type **nvarchar**(776), sans valeur par défaut.  
  
`[ @indexname = ] 'indexname'` Nom de l’index spatial spécifié. *IndexName* est de **type sysname** et n’a pas de valeur par défaut.  
  
`[ @verboseoutput = ] 'verboseoutput'` Plage de noms et de valeurs de propriété à retourner.  
  
 0 = propriétés principales  
  
 \>0 = toutes les propriétés  
  
 *verboseoutput* est de **type tinyint** et n’a pas de valeur par défaut.  
  
`[ @query_sample = ] 'query_sample'` Est un exemple de requête représentative qui peut être utilisé pour tester l’utilité de l’index. Il peut s'agir d'un objet représentatif ou d'une fenêtre de requête. *query_sample* est une **géométrie** sans valeur par défaut.  
  
`[ @xml_output = ] 'xml_output'` Paramètre de sortie qui retourne le jeu de résultats dans un fragment XML. *xml_output* est de **XML** sans valeur par défaut.  
  
## <a name="properties"></a>Propriétés  
 Définissez **\@ verboseoutput** = 0 pour retourner les propriétés principales, comme indiqué dans le tableau ci-dessous. **\@ verboseoutput** > 0 pour retourner toutes les propriétés de l’index spatial.  
  
 **Base_Table_Rows**  
 Nombre de lignes dans la table de base. La valeur est de type **bigint**.  
  
 **Bounding_Box_xmin**  
 Propriétés du cadre englobant minimal X de l’index spatial pour le type **Geometry** . Cette valeur de propriété est NULL pour le type **Geography**. La valeur est **float**.  
  
 **Bounding_Box_ymin**  
 Propriétés du cadre englobant minimal Y de l’index spatial pour le type **Geometry** . Cette valeur de propriété est NULL pour le type **Geography** . La valeur est **float**.  
  
 **Bounding_Box_xmax**  
 Propriétés du cadre englobant maximal X de l’index spatial pour le type **Geometry** . Cette valeur de propriété est NULL pour le type **Geography** . La valeur est **float**.  
  
 **Bounding_Box_ymax**  
 Propriétés du cadre englobant maximal Y de l’index spatial pour le type **Geometry** . Cette valeur de propriété est NULL pour le type **Geography** . La valeur est **float**.  
  
 **Grid_Size_Level_1**  
 Densité de la grille de niveau 1 de l’index spatial :  
  
 16 pour LOW  
  
 64 pour MEDIUM  
  
 256 pour HIGH  
  
 La valeur est **int**.  
  
 **Grid_Size_Level_2**  
 Densité de la grille de niveau 2 de l’index spatial :  
  
 16 pour LOW  
  
 64 pour MEDIUM  
  
 256 pour HIGH  
  
 La valeur est **int**.  
  
 **Grid_Size_Level_3**  
 Densité de grille de niveau 3 de l’index spatial :  
  
 16 pour LOW  
  
 64 pour MEDIUM  
  
 256 pour HIGH  
  
 La valeur est **int**.  
  
 **Grid_Size_Level_4**  
 Densité de la grille de niveau 4 de l'index spatial :  
  
 16 pour LOW  
  
 64 pour MEDIUM  
  
 256 pour HIGH  
  
 La valeur est **int**.  
  
 **Cells_Per_Object**  
 Nombre de cellules par objet spatial (propriété d'index). La valeur est **int**.  
  
 **Total_Primary_Index_Rows**  
 Nombre de lignes dans l'index. La valeur est de type **bigint**.  
  
 **Total_Primary_Index_Pages**  
 Nombre de pages dans l'index. La valeur est de type **bigint**.  
  
 **Average_Number_Of_Index_Rows_Per_Base_Row**  
 Nombre de lignes d'index / nombre de lignes de table de base. La valeur est de type **bigint**.  
  
 **Total_Number_Of_ObjectCells_In_Level0_For_QuerySample**  
 Indique si l’exemple de requête représentative se situe en dehors du cadre englobant de l’index **Geometry** et dans la cellule racine (cellule de niveau 0). Il s'agit de 0 (pas dans la cellule de niveau 0) ou de 1. Si c'est dans la cellule de niveau 0, l'index exploré n'est pas un index approprié pour l'exemple de requête. Il s'agit d'une propriété principale. La valeur est de type **bigint**.  
  
 **Total_Number_Of_ObjectCells_In_Level0_In_Index**  
 Nombre d’instances de cellule d’objets indexés qui sont fractionnées dans le niveau 0 (cellule racine, en dehors du cadre englobant pour la **géométrie**). Il s'agit d'une propriété principale. La valeur est de type **bigint**.  
  
 Pour les index de **géométrie** , cela se produit si le cadre englobant de l’index est plus petit que le domaine de données. Un nombre élevé d’objets au niveau 0 peut nécessiter des filtres secondaires si la fenêtre de requête se trouve partiellement à l’extérieur du cadre englobant et diminue les performances de l’index (par exemple, **Total_Number_Of_ObjectCells_In_Level0_For_QuerySample** est 1). Si la fenêtre de requête se situe à l'intérieur du cadre englobant, un nombre élevé d'objets au niveau 0 peut améliorer réellement la performance de l'index.  
  
 Les instances NULL et vides sont comptées au niveau 0 mais n'ont pas d'incidence sur les performances. Le niveau 0 aura autant de cellules que les instances NULL et vides à la table de base. Pour les index **Geography** , le niveau 0 aura autant de cellules que les instances null et vides + 1 cellule, car l’exemple de requête est compté comme 1.  
  
 **Total_Number_Of_ObjectCells_In_Level1_In_Index**  
 Nombre d’instances de cellule d’objets indexés qui sont fractionnées avec la précision de niveau 1. Il s'agit d'une propriété principale. La valeur est de type **bigint**.  
  
 **Total_Number_Of_ObjectCells_In_Level2_In_Index**  
 Nombre d’instances de cellule d’objets indexés qui sont fractionnées avec la précision de niveau 2. Il s'agit d'une propriété principale. La valeur est de type **bigint**.  
  
 **Total_Number_Of_ObjectCells_In_Level3_In_Index**  
 Nombre d’instances de cellule d’objets indexés qui sont fractionnées avec la précision de niveau 3. Il s'agit d'une propriété principale. La valeur est de type **bigint**.  
  
 **Total_Number_Of_ObjectCells_In_Level4_In_Index**  
 Nombre d'instances de cellule d'objets indexés qui sont pavées avec la précision de niveau 4. Il s'agit d'une propriété principale. La valeur est de type **bigint**.  
  
 **Total_Number_Of_interior_ObjectCells_In_Level1_In_Index**  
 Nombre de cellules qui sont complètement couvertes par un objet au niveau de pavage 1 et qui sont donc à l’intérieur de l’objet. (Cell_attributevalue est 2.) Il s’agit d’une propriété de base. La valeur est de type **bigint**.  
  
 **Total_Number_Of_interior_ObjectCells_In_Level2_In_Index**  
 Nombre de cellules entièrement couvertes par un objet au niveau de pavage 2 et qui sont donc à l’intérieur de l’objet. (Cell_attribute valeur est 2.) Il s’agit d’une propriété de base. La valeur est de type **bigint**.  
  
 **Total_Number_Of_interior_ObjectCells_In_Level3_In_Index**  
 Nombre de cellules entièrement couvertes par un objet au niveau de pavage 3 et, par conséquent, à l’intérieur de l’objet. (Cell_attribute valeur est 2.) Il s’agit d’une propriété de base. La valeur est de type **bigint**.  
  
 **Total_Number_Of_interior_ObjectCells_In_Level4_In_Index**  
 Nombre de cellules qui sont complètement couvertes par un objet au pavage niveau 4 et se situent donc à l'intérieur de l'objet. (Cell_attribute valeur est 2.) Il s’agit d’une propriété de base. La valeur est de type **bigint**.  
  
 **Total_Number_Of_intersecting_ObjectCells_In_Level1_In_Index**  
 Nombre de cellules qui sont croisées par un objet au niveau de pavage 1. (Cell_attribute valeur est 1.) Il s’agit d’une propriété de base. La valeur est de type **bigint**.  
  
 **Total_Number_Of_intersecting_ObjectCells_In_Level2_In_Index**  
 Nombre de cellules qui sont croisées par un objet au niveau de pavage 2. (Cell_attribute valeur est 1.) Il s’agit d’une propriété de base. La valeur est de type **bigint**.  
  
 **Total_Number_Of_intersecting_ObjectCells_In_Level3_In_Index**  
 Nombre de cellules qui sont croisées par un objet au niveau de pavage 3. (Cell_attribute valeur est 1.) Il s’agit d’une propriété de base. La valeur est de type **bigint**.  
  
 **Total_Number_Of_intersecting_ObjectCells_In_Level4_In_Index**  
 Nombre de cellules qui sont croisées par un objet au pavage niveau 4. (Cell_attribute valeur est 1.) Il s’agit d’une propriété de base. La valeur est de type **bigint**.  
  
 **Total_Number_Of_Border_ObjectCells_In_Level0_For_QuerySample**  
 Indique si l'exemple de requête est dans la cellule racine 0 à l'extérieur du cadre englobant, mais le touche. Il s'agit d'une propriété principale. La valeur est de type **bigint**.  
  
> [!NOTE]  
>  Cette information est uniquement utile pour déterminer s'il y a des objets que le cadre englobant a pu manquer de peu.  
  
 **Total_Number_Of_Border_ObjectCells_In_Level0_In_Index**  
 Nombre d'objets au niveau 0 qui touchent le cadre englobant. (Cell_attribute valeur est 0.)  La valeur est de type **bigint**.  
  
 **Total_Number_Of_Border_ObjectCells_In_Level1_In_Index**  
 Nombre de cellules d’objet qui touchent une limite de cellule de grille au niveau de pavage 1. (Cell_attribute valeur est 0.) Il s’agit d’une propriété de base. La valeur est de type **bigint**.  
  
 **Total_Number_Of_Border_ObjectCells_In_Level2_In_Index**  
 Nombre de cellules d’objet qui touchent une limite de cellule de grille au niveau de pavage 2. (Cell_attribute valeur est 0.) Il s’agit d’une propriété de base. La valeur est de type **bigint**.  
  
 **Total_Number_Of_Border_ObjectCells_In_Level3_In_Index**  
 Nombre de cellules d’objet qui touchent une limite de cellule de grille au niveau de pavage 3. (Cell_attribute valeur est 0.) Il s’agit d’une propriété de base. La valeur est de type **bigint**.  
  
 **Total_Number_Of_Border_ObjectCells_In_Level4_In_Index**  
 Nombre de cellules d'objet qui touchent une limite de cellule de grille au pavage niveau 4. (Cell_attribute valeur est 0.) Il s’agit d’une propriété de base. La valeur est de type **bigint**.  
  
 **Interior_To_Total_Cells_Normalized_To_Leaf_Grid_Percentage**  
 Pourcentage de la zone totale (cellules feuilles totales) de la grille qui contient des cellules feuilles couvertes par un objet.  
  
 Par exemple, un objet est pavé dans 10 cellules à 4 niveaux de grille différents couvrant une zone qui est équivalente à 100 cellules feuilles au total. Supposons l'existence de 3 cellules intérieures qui sont complètement couvertes par l'objet. La zone couverte par les 3 cellules intérieures est équivalente à 42 cellules feuilles. Le pourcentage de zone couverte est donc de 42 pour cent. Il s'agit d'une bonne mesure de la manière dont les objets dans l'index sont déchiquetés.  
  
 La valeur est **float**.  
  
 **Intersecting_To_Total_Cells_Normalized_To_Leaf_Grid_Percentage**  
 Identique à **Interior_To_Total_Cells_Normalized_To_Leaf_Grid_Percentage**, sauf qu’il s’agit de cellules partiellement couvertes. La valeur est **float**.  
  
 **Border_To_Total_Cells_Normalized_To_Leaf_Grid_Percentage**  
 Identique à **Interior_To_Total_Cells_Normalized_To_Leaf_Grid_Percentage** sauf qu’il s’agit de cellules de bordure. La valeur est **float**.  
  
 **Average_Cells_Per_Object_Normalized_To_Leaf_Grid**  
 Cellules moyennes par objet normalisées selon la grille feuilles. Cela nous donne une indication de la taille spatiale de l'objet, ou le volume des objets. La valeur est **float**.  
  
 **Average_Objects_PerLeaf_GridCell**  
 Fragmentation de l'index Nombre moyen d'objets par cellule feuille. La valeur est **float**.  
  
 **Number_Of_SRIDs_Found**  
 Nombre de SRID uniques dans l'index et la colonne. La valeur est **int**.  
  
 Dans la mesure où une colonne peut contenir plusieurs SRID et que les objets de SRID différent ne se croisent jamais, le nombre de SRID indique la sélectivité de l'index.  
  
 **Width_Of_Cell_In_Level1**  
 Propriété de largeur (Width) de cellule dans la grille d'indexation. L’unité de mesure est fournie par l’index et dépend de la SRID des données indexées. La valeur est **float**.  
  
 **Width_Of_Cell_In_Level2**  
 Propriété de largeur (Width) de cellule dans la grille d'indexation. L’unité de mesure est fournie par l’index et dépend de la SRID des données indexées. La valeur est **float**.  
  
 **Width_Of_Cell_In_Level3**  
 Propriété de largeur (Width) de cellule dans la grille d'indexation. L’unité de mesure est fournie par l’index et dépend de la SRID des données indexées. La valeur est **float**.  
  
 **Width_Of_Cell_In_Level4**  
 Propriété de largeur (Width) de cellule dans la grille d'indexation. L'unité de mesure est fournie par l'index et dépend du SRID des données indexées. La valeur est **float**.  
  
 **Height_Of_Cell_In_Level1**  
 Propriété de hauteur (Height) de cellule dans la grille d'indexation. L’unité de mesure est fournie par l’index et dépend de la SRID des données indexées. La valeur est **float**.  
  
 **Height_Of_Cell_In_Level2**  
 Propriété de hauteur (Height) de cellule dans la grille d'indexation. L’unité de mesure est fournie par l’index et dépend de la SRID des données indexées. La valeur est **float**.  
  
 **Height_Of_Cell_In_Level3**  
 Propriété de hauteur (Height) de cellule dans la grille d'indexation. L’unité de mesure est fournie par l’index et dépend de la SRID des données indexées. La valeur est **float**.  
  
 **Height_Of_Cell_In_Level4**  
 Propriété de hauteur (Height) de cellule dans la grille d'indexation. L’unité de mesure est fournie par l’index et dépend de la SRID des données indexées. La valeur est **float**.  
  
 **Area_Of_Cell_In_Level1**  
 Propriété de zone (Area) de cellule dans la grille d'indexation. L’unité de mesure est fournie par l’index et dépend de la SRID des données indexées. La valeur est **float**.  
  
 **Area_Of_Cell_In_Level2**  
 Propriété de zone (Area) de cellule dans la grille d'indexation. L’unité de mesure est fournie par l’index et dépend de la SRID des données indexées. La valeur est **float**.  
  
 **Area_Of_Cell_In_Level3**  
 Propriété de zone (Area) de cellule dans la grille d'indexation. L’unité de mesure est fournie par l’index et dépend de la SRID des données indexées. La valeur est **float**.  
  
 **Area_Of_Cell_In_Level4**  
 Propriété de zone (Area) de cellule dans la grille d'indexation. L’unité de mesure est fournie par l’index et dépend de la SRID des données indexées. La valeur est **float**.  
  
 **CellArea_To_BoundingBoxArea_Percentage_In_Level1**  
 Pourcentage de la couverture du cadre englobant par une cellule de niveau 1. La valeur est **float**.  
  
 **CellArea_To_BoundingBoxArea_Percentage_In_Level2**  
 Pourcentage de la couverture du cadre englobant par une cellule de niveau 2. La valeur est **float**.  
  
 **CellArea_To_BoundingBoxArea_Percentage_In_Level3**  
 Pourcentage de la couverture du cadre englobant par une cellule de niveau 3. La valeur est **float**.  
  
 **CellArea_To_BoundingBoxArea_Percentage_In_Level4**  
 Le pourcentage de couverture du cadre englobant par une cellule de niveau 4. La valeur est **float**.  
  
 **Number_Of_Rows_Selected_By_Primary_Filter**  
 Nombre de lignes sélectionnées par le filtre principal. Il s'agit d'une propriété principale. La valeur est de type **bigint.**  
  
 **Number_Of_Rows_Selected_By_Internal_Filter**  
 Nombre de lignes sélectionnées par le filtre interne. Le filtre secondaire n'est pas appelé pour ces lignes. Il s'agit d'une propriété principale. La valeur est de type **bigint.**  
  
 Le nombre retourné n’est applicable que pour **STIntersects**.  
  
 **Number_Of_Times_Secondary_Filter_Is_Called**  
 Nombre de fois que le filtre secondaire est appelé. Il s'agit d'une propriété principale. La valeur est de type **bigint.**  
  
 **Percentage_Of_Rows_NotSelected_By_Primary_Filter**  
 S'il y a N lignes dans la table de base, et si P est sélectionné par le filtre principal, cela retourne (N-P)/N comme pourcentage. Il s'agit d'une propriété principale. La valeur est **float.**  
  
 **Percentage_Of_Primary_Filter_Rows_Selected_By_internal_Filter**  
 Si les lignes P sont sélectionnées par les filtre principal et si les lignes S sont sélectionnées par le filtre interne, cela retourne S/P comme pourcentage. Plus le pourcentage est élevé, mieux l'index se comporte pour éviter le filtre secondaire qui est plus pénalisant en termes de performances. Il s'agit d'une propriété principale. La valeur est **float.**  
  
 **Number_Of_Rows_Output**  
 Nombre réel de lignes générées par la requête. Il s'agit d'une propriété principale. La valeur est de type **bigint.**  
  
 **Internal_Filter_Efficiency**  
 Si O est le nombre de lignes générées, cela retourne S/O comme pourcentage. Il s'agit d'une propriété principale. La valeur est **float.**  
  
 **Primary_Filter_Efficiency**  
 Si les lignes P sont sélectionnées par le filtre principal et que O est le nombre de lignes générées, cette valeur est retournée/P sous la forme d’un pourcentage. Plus le rendement du filtre principal est principal, moins il y a de faux positifs que le filtre secondaire doit traiter. Il s'agit d'une propriété principale. La valeur est **float.**  
  
## <a name="permissions"></a>Autorisations  
 L’utilisateur doit être membre du rôle **public** . Nécessite une autorisation READ ACCESS sur le serveur et l'objet. Cela s'applique à toutes les procédures stockées de l'index spatial.  
  
## <a name="remarks"></a>Notes  
 Les propriétés qui contiennent des valeurs NULL ne sont pas incluses dans le jeu de retour.  
  
## <a name="examples"></a>Exemples  
 Pour les exemples, consultez les rubriques suivantes :  
  
-   [sp_help_spatial_geometry_index &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-spatial-geometry-index-transact-sql.md)  
  
-   [sp_help_spatial_geometry_index_xml &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-spatial-geometry-index-xml-transact-sql.md)  
  
-   [sp_help_spatial_geography_index &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-spatial-geography-index-transact-sql.md)  
  
-   [sp_help_spatial_geography_index_xml &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-spatial-geography-index-xml-transact-sql.md)  
  
## <a name="requirements"></a>Spécifications  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées d’index spatial &#40;Transact-SQL&#41;]()   
 [sp_help_spatial_geometry_index &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-spatial-geometry-index-transact-sql.md)   
 [Vue d’ensemble des index spatiaux](../../relational-databases/spatial/spatial-indexes-overview.md)   
 [Notions de base de XQuery](../../xquery/xquery-basics.md)   
 [Références relatives au langage Xquery &#40;SQL Server&#41;](../../xquery/xquery-language-reference-sql-server.md)  
  
