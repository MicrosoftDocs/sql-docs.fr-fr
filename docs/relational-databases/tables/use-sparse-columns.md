---
description: Utiliser des colonnes éparses
title: Utiliser des colonnes éparses | Microsoft Docs
ms.custom: ''
ms.date: 03/22/2016
ms.prod: sql
ms.prod_service: table-view-index, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- sparse columns, described
- null columns
- sparse columns
ms.assetid: ea7ddb87-f50b-46b6-9f5a-acab222a2ede
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 2b2d569c3f37370bd132485e843da005b1769b17
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211541"
---
# <a name="use-sparse-columns"></a>Utiliser des colonnes éparses

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

  Les colonnes éparses sont des colonnes ordinaires qui ont un stockage optimisé pour les valeurs NULL. Les colonnes éparses réduisent l'espace nécessaire pour les valeurs NULL, en échange d'une augmentation du coût d'extraction des valeurs autres que NULL. Envisagez d'utiliser des colonnes éparses lorsque l'espace économisé est d'au moins 20 à 40 pour cent. Les colonnes éparses et les jeux de colonnes sont définis à l'aide des instructions [CREATE TABLE](../../t-sql/statements/create-table-transact-sql.md) ou [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md) .  
  
 Les colonnes éparses peuvent être utilisées avec des jeux de colonnes et des index filtrés :  
  
-   Jeux de colonnes  
  
     Les instructions INSERT, UPDATE et DELETE peuvent faire référence aux colonnes éparses par nom. Toutefois, vous pouvez également afficher et utiliser toutes les colonnes éparses d'une table qui sont combinées dans une colonne XML unique. Cette colonne porte le nom de jeu de colonnes. Pour plus d’informations sur les jeux de colonnes, consultez [Utiliser des jeux de colonnes](../../relational-databases/tables/use-column-sets.md).  
  
-   Index filtrés  
  
     Les colonnes éparses ayant de nombreuses lignes évaluées à NULL, elles sont particulièrement appropriées pour les index filtrés. Un index filtré sur une colonne éparse peut indexer uniquement les lignes qui ont des valeurs remplies. Cela crée un index plus petit et plus efficace. Pour plus d'informations, consultez [Create Filtered Indexes](../../relational-databases/indexes/create-filtered-indexes.md).  
  
 Les colonnes éparses et les index filtrés permettent aux applications, telles que [!INCLUDE[winSPServ](../../includes/winspserv-md.md)], de stocker efficacement et d’accéder à un grand nombre de propriétés définies par l’utilisateur à l’aide de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)].  
  
## <a name="properties-of-sparse-columns"></a>Propriétés des colonnes éparses  
 Les colonnes éparses présentent les caractéristiques suivantes :  
  
-   Le [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] utilise le mot clé SPARSE dans une définition de colonne pour optimiser le stockage des valeurs dans cette colonne. Par conséquent, lorsque la valeur de colonne est NULL pour toute ligne dans la table, les valeurs ne requièrent pas de stockage.  
  
-   Les affichages catalogue pour une table qui a des colonnes éparses sont les mêmes que pour une table ordinaire. L'affichage catalogue sys.columns contient une ligne pour chaque colonne de la table et inclut un jeu de colonnes s'il y en a un de défini.  
  
-   Les colonnes éparses sont une propriété de la couche de stockage plutôt que de la table logique. Par conséquent, une instruction SELECT...INTO ne copie pas la propriété de colonne éparse dans une nouvelle table.  
  
-   La fonction COLUMNS_UPDATED renvoie une valeur **varbinary** pour indiquer toutes les colonnes qui ont été mises à jour pendant une action DML. Les bits retournés par la fonction COLUMNS_UPDATED sont les suivants :  
  
    -   Lorsqu'une colonne éparse est mise à jour de manière explicite, le bit correspondant pour cette colonne éparse est défini sur 1 et le bit pour le jeu de colonnes est défini sur 1.  
  
    -   Lorsqu'un jeu de colonnes est mis à jour de manière explicite, le bit pour le jeu de colonnes est défini sur 1 et les bits pour toutes les colonnes éparses dans cette table sont définis sur 1.  
  
    -   Pour les opérations d'insertion, tous les bits sont définis sur 1.  
  
     Pour plus d’informations sur les jeux de colonnes, consultez [Utiliser des jeux de colonnes](../../relational-databases/tables/use-column-sets.md).  
  
 Les types de données suivants ne peuvent pas être spécifiés comme SPARSE :  
  
|||  
|-|-|  
|**Geography**|**text**|  
|**geometry**|**timestamp**|  
|**image**|**types de données définis par l'utilisateur**|  
|**ntext**||  
  
## <a name="estimated-space-savings-by-data-type"></a>Évaluation des économies d'espace par type de données  
 Les colonnes éparses requièrent davantage d'espace de stockage pour les valeurs autres que Null, comparé à l'espace requis pour les données identiques qui ne sont pas marquées SPARSE. Les tableaux suivants indiquent l'utilisation d'espace pour chaque type de données. La colonne **Pourcentage NULL** indique le pourcentage des données qui doivent être NULL pour une économie d'espace nette de 40 pour cent.  
  
 **Types de données de longueur fixe**  
  
|Type de données|Octets non alloués|Octets alloués|Pourcentage NULL|  
|---------------|---------------------|------------------|---------------------|  
|**bit**|0,125|5|98%|  
|**tinyint**|1|5|86%|  
|**smallint**|2|6|76 %|  
|**int**|4|8|64%|  
|**bigint**|8|12|52 %|  
|**real**|4|8|64%|  
|**float**|8|12|52 %|  
|**smallmoney**|4|8|64%|  
|**money**|8|12|52 %|  
|**smalldatetime**|4|8|64%|  
|**datetime**|8|12|52 %|  
|**uniqueidentifier**|16|20|43%|  
|**date**|3|7|69%|  
  
 **Types de données de longueur dépendante de la précision**  
  
|Type de données|Octets non alloués|Octets alloués|Pourcentage NULL|  
|---------------|---------------------|------------------|---------------------|  
|**datetime2(0)**|6|10|57%|  
|**datetime2(7)**|8|12|52 %|  
|**time(0)**|3|7|69%|  
|**time(7)**|5|9|60%|  
|**datetimetoffset(0)**|8|12|52 %|  
|**datetimetoffset (7)**|10|14|49%|  
|**decimal/numeric(1,s)**|5|9|60%|  
|**decimal/numeric(38,s)**|17|21|42%|  
|**vardecimal(p,s)**|Utilisez le type **decimal** comme évaluation pessimiste.|||  
  
 **Types de données de longueur dépendante des données**  
  
|Type de données|Octets non alloués|Octets alloués|Pourcentage NULL|  
|---------------|---------------------|------------------|---------------------|  
|**sql_variant**|Varie selon le type de données sous-jacent|||  
|**varchar** ou **char**|2*|4*|60%|  
|**nvarchar** ou **nchar**|2*|4*+|60%|  
|**varbinary** ou **binary**|2*|4*|60%|  
|**xml**|2*|4*|60%|  
|**hierarchyid**|2*|4*|60%|  
  
 *La longueur est égale à la moyenne des données contenues dans le type, plus 2 ou 4 octets.  
  
## <a name="in-memory-overhead-required-for-updates-to-sparse-columns"></a>Charge en mémoire requise pour les mises à jour de colonnes éparses  
 Lorsque vous concevez des tables comportant des colonnes éparses, gardez à l'esprit qu'une charge supplémentaire de 2 octets est requise pour chaque colonne éparse non Null dans la table lorsqu'une ligne est mise à jour. Conséquemment à cette exigence de mémoire supplémentaire, les mises à jour peuvent échouer de façon inattendue avec l'erreur 576 lorsque la taille totale de la ligne, y compris sa charge de mémoire, dépasse 8019 et qu'aucune colonne ne peut être sortie de la ligne.  
  
 Prenons l'exemple d'une table contenant 600 colonnes éparses de type bigint. S'il y a 571 colonnes non Null, alors la taille totale sur le disque est de 571 * 12 = 6852 octets. Après l'ajout de la charge de ligne supplémentaire et de l'en-tête de colonne éparse, ce chiffre augmente pour atteindre 6895 octets environ. La page dispose toujours d'environ 1124 octets disponibles sur le disque. Cela peut donner l'impression que des colonnes supplémentaires peuvent être mises à jour sans problème. Cependant, pendant la mise à jour, une charge supplémentaire en mémoire équivalente à 2\*le nombre de colonnes éparses non Null est requise. Dans cet exemple, en incluant la charge supplémentaire (2 \* 571 = 1 142 octets), la taille de la ligne sur le disque atteint environ 8 037 octets. Cette valeur dépasse la taille de ligne autorisée maximale de 8019 octets. Étant donné que toutes les colonnes ont un type de données de longueur fixe, elles ne peuvent pas être sorties de la ligne. En conséquence, la mise à jour échoue avec l'erreur 576.  
  
## <a name="restrictions-for-using-sparse-columns"></a>Restrictions relatives à l'utilisation des colonnes éparses  
 Les colonnes éparses peuvent contenir n'importe quel type de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ; en outre, elles se comportent comme n'importe quelle autre colonne avec les restrictions suivantes :  
  
-   Une colonne éparse doit être nullable et ne peut pas avoir les propriétés ROWGUIDCOL ou IDENTITY. Une colonne éparse ne peut pas être des types de données suivants : **text**, **ntext**, **image**, **timestamp**, type de données défini par l’utilisateur, **geometry** ou **geography**; ni avoir l’attribut FILESTREAM.  
  
-   Une colonne éparse ne peut pas avoir de valeur par défaut.  
  
-   Une colonne éparse ne peut pas être liée à une règle.  
  
-   Bien qu'une colonne calculée puisse contenir une colonne éparse, une colonne calculée ne peut pas être marquée comme SPARSE.  
  
-   Un masque de données peut être défini sur une colonne éparse, mais pas sur une colonne éparse qui fait partie d’un jeu de colonnes.  
  
-   Une colonne éparse ne peut pas faire partie d'un index cluster ou d'un index de clé primaire unique. Toutefois, les colonnes calculées persistantes et non persistantes définies sur des colonnes éparses peuvent faire partie d'une clé cluster.  
  
-   Une colonne éparse ne peut pas être utilisée comme clé de partition d'un index cluster ou d'un segment de mémoire. Toutefois, une colonne éparse peut être utilisée comme clé de partition d'un index non-cluster.  
  
-   Une colonne éparse ne peut pas faire partie d'un type de table défini par l'utilisateur, qui est utilisé dans des variables de table et des paramètres table.  
  
-   Les colonnes éparses sont incompatibles avec la compression de données. Par conséquent, les colonnes éparses ne peuvent pas être ajoutées aux tables compressées et les tables contenant des colonnes éparses ne peuvent pas être compressées.  
  
-   Le changement d'une colonne éparse en colonne non éparse ou d'une colonne non éparse en colonne éparse requiert la modification du format de stockage. Le moteur de base de données SQL Server effectue cette modification en procédant comme suit :  
  
    1.  Il ajoute une nouvelle colonne à la table en fonction de la nouvelle taille et du nouveau format de stockage.  
  
    2.  Pour chaque ligne de la table, il met à jour et copie la valeur stockée dans l'ancienne colonne vers la nouvelle colonne.  
  
    3.  Il supprime l'ancienne colonne du schéma de la table.  
  
    4.  Reconstruit la table (en l'absence d'index cluster) ou reconstruit l'index cluster pour récupérer de l'espace utilisé par l'ancienne colonne.  
  
    > [!NOTE]  
    >  L'étape 2 peut échouer lorsque la taille des données de la ligne dépasse la taille de ligne maximale autorisée. Cette taille inclut la taille des données stockées dans l'ancienne colonne et celle des données mises à jour stockées dans la nouvelle colonne. Cette limite est de 8 060 octets pour les tables qui ne contiennent pas de colonnes éparses ou de 8 018 octets pour les tables qui contiennent des colonnes éparses. Cette erreur peut se produire même si toutes les colonnes éligibles ont été déplacées hors des lignes.  
  
-   Lorsque vous modifiez une colonne non éparse en colonne éparse, la colonne éparse consomme davantage d'espace pour les valeurs non Null. Lorsqu'une ligne est proche de la limite de taille de ligne maximale, l'opération peut échouer.  
  
## <a name="sql-server-technologies-that-support-sparse-columns"></a>Technologies SQL Server qui prennent en charge les colonnes éparses  
 Cette section décrit comment les colonnes éparses sont prises en charge dans les technologies [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] suivantes :  
  
-   Réplication transactionnelle  
  
     La réplication transactionnelle prend en charge les colonnes éparses, mais pas les jeux de colonnes, qui peuvent être utilisés avec les colonnes éparses. Pour plus d’informations sur les jeux de colonnes, consultez [Utiliser des jeux de colonnes](../../relational-databases/tables/use-column-sets.md).  
  
     La réplication de l’attribut SPARSE est déterminée par une option de schéma spécifiée à l’aide de [sp_addarticle](../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md) ou de la boîte de dialogue **Propriétés de l’article** dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Les versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne prennent pas en charge les colonnes éparses. Si vous devez répliquer des données vers une version antérieure, spécifiez que l'attribut SPARSE ne doit pas être répliqué.  
  
     Pour les tables publiées, vous ne pouvez pas ajouter de nouvelles colonnes éparses à une table ni modifier la propriété de fragmentation d'une colonne existante. Si une telle opération est requise, supprimez et recréez la publication.  
  
-   Réplication de fusion  
  
     La réplication de fusion ne prend pas en charge les colonnes éparses ni les jeux de colonnes.  
  
-   Suivi des modifications  
  
     Le suivi des modifications prend en charge les colonnes éparses et les jeux de colonnes. Lorsqu'un jeu de colonnes est mis à jour dans une table, le suivi des modifications traite cela comme une mise à jour de la ligne entière. Aucun suivi des modifications détaillé n'est fourni pour obtenir le jeu exact des colonnes éparses mises à jour par le biais de l'opération de mise à jour de jeu de colonnes. Si les colonnes éparses sont mises à jour de manière explicite par le biais d'une instruction DML, le suivi des modifications sur ces colonnes fonctionne de façon ordinaire et peut identifier le jeu exact de colonnes modifiées.  
  
-   Capture des données modifiées  
  
     La capture de données modifiées prend en charge les colonnes éparses, mais pas les jeux de colonnes.  
  
-   La propriété Sparse d'une colonne n'est pas conservée lorsque la table est copiée.  
  
## <a name="examples"></a>Exemples  
 Dans cet exemple, une table de documents contient un jeu commun qui a les colonnes `DocID` et `Title`. Le groupe Production souhaite avoir une colonne `ProductionSpecification` et `ProductionLocation` pour tous les documents de production. Le groupe Marketing souhaite avoir une colonne `MarketingSurveyGroup` pour les documents de marketing. Le code dans cet exemple crée une table qui utilise des colonnes éparses, insère deux lignes dans la table, puis sélectionne des données de la table.  
  
> [!NOTE]  
>  Cette table ne possède que cinq colonnes, de manière à simplifier son affichage et sa lecture. La déclaration des colonnes éparses comme nullables est facultative si l'option ANSI_NULL_DFLT_ON est définie.  
  
```  
USE AdventureWorks2012;  
GO  
  
CREATE TABLE DocumentStore  
    (DocID int PRIMARY KEY,  
     Title varchar(200) NOT NULL,  
     ProductionSpecification varchar(20) SPARSE NULL,  
     ProductionLocation smallint SPARSE NULL,  
     MarketingSurveyGroup varchar(20) SPARSE NULL ) ;  
GO  
  
INSERT DocumentStore(DocID, Title, ProductionSpecification, ProductionLocation)  
VALUES (1, 'Tire Spec 1', 'AXZZ217', 27);  
GO  
  
INSERT DocumentStore(DocID, Title, MarketingSurveyGroup)  
VALUES (2, 'Survey 2142', 'Men 25 - 35');  
GO  
```  
  
 La sélection de toutes les colonnes de la table retourne un jeu de résultats ordinaire.  
  
```  
SELECT * FROM DocumentStore ;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 `DocID  Title        ProductionSpecification  ProductionLocation  MarketingSurveyGroup`  
  
 `1      Tire Spec 1  AXZZ217                  27                  NULL`  
  
 `2      Survey 2142  NULL                     NULL                Men 25-35`  
  
 Le département Production ne s'intéressant pas aux données de marketing, il souhaite utiliser une liste de colonnes qui retourne uniquement les colonnes pertinentes, comme illustré dans la requête suivante.  
  
```  
SELECT DocID, Title, ProductionSpecification, ProductionLocation   
FROM DocumentStore   
WHERE ProductionSpecification IS NOT NULL ;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 `DocID  Title        ProductionSpecification  ProductionLocation`  
  
 `1      Tire Spec 1  AXZZ217                  27`  
  
## <a name="see-also"></a>Voir aussi  
 [Utiliser des jeux de colonnes](../../relational-databases/tables/use-column-sets.md)   
 [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)   
 [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)   
 [sys.columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md)  
  
  
