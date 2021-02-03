---
description: Tables
title: Tables | Microsoft Docs
ms.custom: ''
ms.date: 09/18/2019
ms.prod: sql
ms.prod_service: table-view-index, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- tables [SQL Server]
- table components [SQL Server]
ms.assetid: 82d7819c-b801-4309-a849-baa63083e83f
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 4e8b8a82580fbe6f1a60a82d54e90c5ee106280f
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236787"
---
# <a name="tables"></a>Tables
[!INCLUDE [sqlserver2016-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa-pdw.md)]

Les tables sont des objets de base de données qui contiennent toutes les données d'une base de données. Dans les tables, les données sont logiquement organisées en lignes et en colonnes, à la manière d'un tableur. Chaque ligne représente un enregistrement unique et chaque colonne représente un champ dans l'enregistrement. Par exemple, une table qui contient les données relatives aux employés d'une société peut contenir une ligne pour chaque employé et des colonnes représentant des informations sur l'employé, telles que son numéro, son nom, son adresse, son poste et son numéro de téléphone privé. 

- Le nombre de tables dans une base de données n'est limité que par le nombre d'objets autorisés dans une base de données (2 147 483 647). Une table standard définie par l'utilisateur peut comporter jusqu'à 1 024 colonnes. Le nombre de lignes dans la table n'est limité que par la capacité de stockage du serveur. 

- Vous pouvez affecter des propriétés à la table et à chaque colonne de la table pour contrôler les données qui sont autorisées et d'autres propriétés. Par exemple, vous pouvez créer des contraintes sur une colonne pour interdire les valeurs NULL ou fournir une valeur par défaut si aucune valeur n'est spécifiée, ou vous pouvez affecter une contrainte de clé sur la table qui applique l'unicité ou définit une relation entre les tables. 

- Les données de la table peuvent être compressées par ligne ou par page. La compression des données peut permettre de stocker plus de lignes sur une page. Pour plus d’informations, consultez [Compression de données](../../relational-databases/data-compression/data-compression.md). 

## <a name="types-of-tables"></a>Types de tables
 Outre le rôle standard des tables de base définies par l'utilisateur, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] met à votre disposition les types de tables suivants, qui remplissent une fonction particulière dans une base de données. 

### <a name="partitioned-tables"></a>Tables partitionnées

Les tables partitionnées sont des tables dont les données sont divisées horizontalement en unités qui peuvent être réparties sur plusieurs groupes de fichiers dans une base de données. Le partitionnement permet une gestion plus simple des tables et index volumineux. Vous pouvez en effet accéder à des sous-ensembles de données ou les gérer de manière rapide et efficace, tout en préservant l'intégrité de la collection globale. Par défaut, [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] prend en charge jusqu'à 15 000 partitions. Pour plus d’informations, consultez [Tables et index partitionnés](../../relational-databases/partitions/partitioned-tables-and-indexes.md).

### <a name="temporary-tables"></a>Tables temporaires

Les tables temporaires sont stockées dans **tempdb**. Il en existe deux types : locale et globale. Elles se différencient par leur nom, leur visibilité et leur disponibilité. Le premier caractère du nom des tables temporaires locales est un signe dièse (#) unique. Ces tables sont visibles uniquement à la connexion courante de l'utilisateur et sont supprimées dès que l'utilisateur se déconnecte de l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. En revanche, le nom des tables temporaires globales commence par deux signes dièse (##) ; ces tables sont visibles à tout utilisateur après avoir été créées et ne sont supprimées qu'une fois que l'ensemble des utilisateurs ayant fait référence à la table se sont déconnectés de l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. 


#### <a name="reduced-recompilations-for-workloads-using-temporary-tables-across-multiple-scopes"></a><a name="ctp23"></a> Recompilations réduites pour les charges de travail qui utilisent des tables temporaires sur plusieurs étendues

[!INCLUDE[ss2019](../../includes/sssql19-md.md)] pour tous les niveaux de compatibilité de base de données réduit les recompilations pour les charges de travail qui utilisent des tables temporaires sur plusieurs étendues. Cette fonctionnalité est également activée dans Azure SQL Database sous le niveau de compatibilité de base de données 150 pour tous les modèles de déploiement.  Avant cette fonctionnalité, quand vous référenciez une table temporaire avec une instruction de langage de manipulation de données (DML) (`SELECT`, `INSERT`, `UPDATE`, `DELETE`), si la table temporaire était créée par un lot d’étendue externe, une recompilation de l’instruction DML se produisait à chacune de ses exécutions. Avec cette amélioration, SQL Server effectue de légères vérifications supplémentaires pour éviter les recompilations inutiles :

- Vérifiez si le module d’étendue externe utilisé pour la création de la table temporaire au moment de la compilation est le même que celui utilisé pour les exécutions consécutives. 
- Gardez une trace de toutes les modifications de langage de définition de données (DDL) apportées au moment de la compilation initiale et comparez-les aux opérations DDL des exécutions consécutives.

Le résultat final est une réduction des recompilations superflues et du temps processeur.

### <a name="system-tables"></a>Tables système

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] stocke les données qui définissent la configuration du serveur et de toutes ses tables dans un ensemble spécial de tables appelées « tables système ». Les utilisateurs ne peuvent pas directement interroger ou mettre à jour les tables système. Les informations contenues dans les tables système sont disponibles via les affichages système. Pour plus d’informations, consultez [Vues système &#40;Transact-SQL&#41;](../../t-sql/language-reference.md). 
 
### <a name="wide-tables"></a>Tableaux larges

Les tableaux larges utilisent des [colonnes éparses](../../relational-databases/tables/use-sparse-columns.md) pour porter à 30 000 le total de colonnes qu’un tableau peut contenir. Les colonnes éparses sont des colonnes ordinaires qui ont un stockage optimisé pour les valeurs NULL. Les colonnes éparses réduisent l'espace nécessaire pour les valeurs NULL, en échange d'une augmentation du coût d'extraction des valeurs autres que NULL. Un tableau large a défini un [jeu de colonnes](../../relational-databases/tables/use-column-sets.md), qui est une représentation XML non typée qui combine toutes les colonnes éparses d’une table dans une sortie structurée. Le nombre d'index et de statistiques est également porté à 1 000 et à 30 000, respectivement. La taille maximale d'une ligne de tableau large est de 8 019 octets. Par conséquent, la plupart des données dans une ligne particulière doivent avoir la valeur NULL. Le nombre maximal de colonnes non éparses plus les colonnes calculées dans un tableau large reste 1 024. 

Les tableaux larges ont les conséquences suivantes sur les performances :

- Les tableaux larges peuvent augmenter le coût de gestion des index dans la table. Nous recommandons que le nombre d'index dans un tableau large soit limité aux index requis par la logique métier. L'augmentation du nombre d'index a une incidence directe sur le temps de compilation DML et les besoins en mémoire. Les index non cluster définis doivent être des index filtrés appliqués aux sous-ensembles de données. Pour plus d'informations, consultez [Create Filtered Indexes](../../relational-databases/indexes/create-filtered-indexes.md). 

- Les applications peuvent ajouter et supprimer dynamiquement des colonnes dans les tableaux larges. Lorsque des colonnes sont ajoutées ou supprimées, des plans de requête compilés sont également invalidés. Nous vous recommandons de concevoir une application qui correspond à la charge de travail prévue afin que les modifications de schéma soient réduites. 

- Lorsque des données sont ajoutées et supprimées dans un tableau large, la performance peut en être affectée. Les applications doivent être conçues pour la charge de travail prévue afin que les modifications apportées aux données de table soient réduites. 

- Limitez l'exécution d'instructions DML dans un tableau large qui mettent à jour plusieurs lignes d'une clé de clustering. Ces instructions peuvent requérir des ressources de mémoire significatives pour la compilation et l'exécution. 

- Basculez les opérations de partitions dans les tableaux larges qui peuvent être lentes et requérir de grandes quantités de mémoire pour le traitement. Les performances et besoins en mémoire sont proportionnels au nombre total de colonnes dans les partitions à la fois sources et cibles. 

- Mettez à jour les curseurs qui mettent à jour des colonnes spécifiques dans un tableau large qui doivent répertorier explicitement les colonnes dans la clause FOR UPDATE. Cela permettra de mieux optimiser la performance lorsque vous utilisez des curseurs. 

## <a name="common-table-tasks"></a>Tâches de table communes
 Le tableau suivant fournit des liens vers les tâches couramment associées à la création ou à la modification d'une table. 

|Tâches de table|Rubrique|
|-----------------|-----------|
|Explique comment créer une table.|[Créer des tables &#40;moteur de base de données&#41;](../../relational-databases/tables/create-tables-database-engine.md)|
|Explique comment supprimer une table.|[Supprimer des tables &#40;moteur de base de données&#41;](../../relational-databases/tables/delete-tables-database-engine.md)|
|Explique comment créer une table qui contient une partie ou la totalité des colonnes d'une table existante.|[Dupliquer des tables](../../relational-databases/tables/duplicate-tables.md)|
|Explique comment renommer une table.|[Renommer des tables &#40;moteur de base de données&#41;](../../relational-databases/tables/rename-tables-database-engine.md)|
|Explique comment afficher les propriétés de la table.|[Afficher la définition de table](../../relational-databases/tables/view-the-table-definition.md)|
|Explique comment déterminer si d'autres objets, tels qu'une vue ou une procédure stockée, dépendent d'une table.|[Afficher les dépendances d'une table](../../relational-databases/tables/view-the-dependencies-of-a-table.md)|

 Le tableau suivant fournit des liens vers les tâches couramment associées à la création ou à la modification de colonnes dans une table. 

|Tâches de colonne|Rubrique|
|------------------|-----------|
|Explique comment ajouter des colonnes à une table existante.|[Ajouter des colonnes à une table &#40;moteur de base de données&#41;](../../relational-databases/tables/add-columns-to-a-table-database-engine.md)|
|Explique comment supprimer des colonnes d'une table.|[Supprimer des colonnes d'une table](../../relational-databases/tables/delete-columns-from-a-table.md)|
|Explique comment modifier le nom d'une colonne.|[Renommer des colonnes &#40;moteur de base de données&#41;](../../relational-databases/tables/rename-columns-database-engine.md)|
|Explique comment copier des colonnes d'une table vers une autre, en copiant uniquement la définition de la colonne ou la définition et les données.|[Copier des colonnes d’une table vers une autre &#40;moteur de base de données&#41;](../../relational-databases/tables/copy-columns-from-one-table-to-another-database-engine.md)|
|Explique comment modifier une définition de colonne, en modifiant le type de données ou une autre propriété.|[Modifier des colonnes &#40;moteur de base de données&#41;](../../relational-databases/tables/modify-columns-database-engine.md)|
|Explique comment modifier l'ordre dans lequel les colonnes apparaissent.|[Modifier l’ordre des colonnes dans une table](../../relational-databases/tables/change-column-order-in-a-table.md)|
|Explique comment créer une colonne calculée dans une table.|[Spécifier les colonnes calculées dans une table](../../relational-databases/tables/specify-computed-columns-in-a-table.md)|
|Explique comment spécifier une valeur par défaut pour une colonne. Cette valeur est utilisée si une autre valeur n'est pas fournie.|[Spécifier des valeurs par défaut pour les colonnes](../../relational-databases/tables/specify-default-values-for-columns.md)|

## <a name="see-also"></a>Voir aussi
 [Contraintes de clé primaire et de clé étrangère](../../relational-databases/tables/primary-and-foreign-key-constraints.md) [Contraintes uniques et contraintes de validation](../../relational-databases/tables/unique-constraints-and-check-constraints.md)