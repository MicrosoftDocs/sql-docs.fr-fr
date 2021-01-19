---
description: Guide d’architecture des pages et des étendues
title: Guide d’architecture des pages et des étendues | Microsoft Docs
ms.custom: ''
ms.date: 03/12/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- page and extent architecture guide
- guide, page and extent architecture
ms.assetid: 83a4aa90-1c10-4de6-956b-7c3cd464c2d2
author: rothja
ms.author: jroth
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 22e1a4832e3ef02d2b596ecd0dd4af3a08a7ec6e
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171871"
---
# <a name="pages-and-extents-architecture-guide"></a>Guide d’architecture des pages et des étendues
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

L’unité fondamentale du stockage des données dans [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] est la page. Une étendue est une collection de huit pages physiquement contiguës. Les étendues sont une aide précieuse pour la gestion des pages. Ce guide décrit les structures de données utilisées pour gérer les pages et les étendues dans toutes les versions de SQL Server. Il est essentiel de comprendre l'architecture des pages et étendues pour concevoir et développer des bases de données performantes.

## <a name="pages-and-extents"></a>Pages et étendues

L’unité fondamentale du stockage des données dans [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] est la page. L’espace disque alloué à un fichier de données (.mdf ou .ndf) d’une base de données est logiquement divisé en pages numérotées consécutivement de 0 à n. Les opérations d'E/S disque sont effectuées au niveau page. En d’autres termes, SQL Server lit ou écrit des pages entières de données.

Les extensions sont une collection de huit pages physiques contiguës ; elles sont utilisées pour gérer les pages de manière efficace. Toutes les pages sont organisées en étendues.

### <a name="pages"></a>Pages

Considérez un livre ordinaire : tout son contenu est écrit sur des pages. À l’image d’un livre, dans SQL Server, toutes les lignes de données sont écrites sur des pages. Dans un livre, toutes les pages ont la même taille physique. De même, dans SQL Server, toutes les pages de données ont la même taille : 8 kilo-octets. Dans un livre, la plupart des pages contiennent les données (le contenu principal du livre), tandis que certaines pages contiennent des métadonnées sur le contenu (par exemple, une table des matières et un index). Là encore, SQL Server n’est pas différent : la plupart des pages contiennent des lignes de données réelles stockées par les utilisateurs ; il s’agit de pages de données et de pages de texte/image (pour les cas spéciaux). Les pages d’index contiennent des références d’index sur l’emplacement des données et, enfin, il existe des pages système qui stockent de nombreuses métadonnées sur l’organisation des données (pages BCM, PFS, GAM, SGAM, IAM, DCM). Consultez le tableau ci-dessous pour connaître les types de page et leur description.

Comme mentionné, dans [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], la taille des pages est de 8 Ko. Autrement dit, les bases de données [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] disposent de 128 pages par mégaoctet. Chaque page commence par un en-tête de 96 octets qui sert à stocker les informations système relatives à la page. Ces informations sont notamment le numéro de page, le type de page, la quantité d'espace disponible sur la page et l'ID de l'unité d'allocation de l'objet auquel appartient la page.

Le tableau suivant présente les types de page utilisés dans les fichiers de données d'une base de données [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].

|Type de page | Contents |
|-------|-------|
|Données |Lignes de données avec toutes les données, sauf text, ntext, image, nvarchar(max), varchar(max), varbinary(max) et xml, lorsque le texte dans la ligne est défini sur ACTIVÉ (ON). |
|Index |Des entrées d'index. |
|Texte/image |Types de données d’objets volumineux : (text, ntext, image, nvarchar(max), varchar(max), varbinary(max) et données xml) <br> Colonnes de longueur variable lorsque la ligne de données dépasse 8 Ko : (varchar, nvarchar, varbinary et sql_variant) |
|GAM (Global Allocation Map), SGAM (Shared Global Allocation Map) |Informations précisant si ces extensions sont allouées. |
|Page Free Space (PFS) |Informations sur l'allocation des pages et sur l'espace disponible sur ces pages. |
|page IAM (Index Allocation Map) |Informations sur les extensions utilisées par une table ou un index par unité l'allocation. |
|mappage modifié en bloc |Informations sur les extensions modifiées par des opérations en bloc depuis la dernière instruction BACKUP LOG par unité d'allocation. |
|mappage de modification différentielle |Informations sur les extensions qui ont été modifiées depuis la dernière instruction BACKUP DATABASE par unité d'allocation. |

> [!NOTE]
> Les fichiers journaux ne contiennent pas de pages mais une série d'enregistrements de fichiers journaux.

Les lignes de données sont placées séquentiellement sur la page, immédiatement à partir de l'en-tête. Une table de décalage de lignes débute à la fin de la page et chaque table de décalage de lignes contient une entrée pour chaque ligne de la page. Chaque entrée de décalage de lignes enregistre la distance à laquelle se trouve le premier octet de la ligne par rapport au début de la page. Ainsi, la fonction de la table de décalage de lignes consiste à aider SQL Server à trouver des lignes dans une page très rapidement. Les entrées de la table de décalage de lignes sont inversées par rapport à l'ordre des lignes sur la page.

![page_architecture](../relational-databases/media/page-architecture.gif)

#### <a name="large-row-support"></a>Prise en charge des lignes de grande taille  

Les lignes ne peuvent pas couvrir plusieurs pages, mais des parties d'une ligne peuvent être déplacées hors de la page de la ligne, afin que la ligne puisse atteindre une très grande taille. La quantité maximale de données et de surcharge contenue dans une seule ligne d'une page est de 8 060 octets (8 Ko). Toutefois, cette valeur ne tient pas compte des données stockées dans le type de page Texte/Image. 

Cette restriction est assouplie pour les tables qui contiennent des colonnes varchar, nvarchar, varbinary ou sql_variant. Lorsque la taille totale de ligne de toutes les colonnes de longueur fixe et variable d'une table dépasse la limite des 8 060 octets, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] déplace de manière dynamique une ou plusieurs colonnes de longueur variable dans les pages de l'unité d'allocation ROW_OVERFLOW_DATA, en commençant par la colonne dont la largeur est la plus grande. 

Cette opération est réalisée chaque fois qu'une opération d'insertion ou de mise à jour augmente la taille totale de la ligne au-delà de la limite de 8 060 octets. Lorsqu'une colonne est déplacée dans une page de l'unité d'allocation ROW_OVERFLOW_DATA, un pointeur de 24 octets est conservé sur la page d'origine dans l'unité d'allocation IN_ROW_DATA. Si une opération ultérieure réduit la taille de la ligne, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] redéplace de manière dynamique les colonnes dans la page de données d'origine. 

##### <a name="row-overflow-considerations"></a>Observations relatives au dépassement de ligne 

Comme mentionné précédemment, une ligne ne peut pas résider dans plusieurs pages et peut provoquer un dépassement si la taille combinée des champs de type de données de longueur variable dépasse la limite de 8 060 octets. À titre d’illustration, une table peut être créée avec deux colonnes : une colonne varchar (7000) et une autre colonne varchar (2000). Individuellement, aucune colonne ne dépasse 8 060 octets, mais combinées, elles peuvent le faire si la largeur totale de chaque colonne est remplie. SQL Server peut déplacer dynamiquement la colonne de longueur variable varchar(7000) vers des pages de l’unité d’allocation ROW_OVERFLOW_DATA. Quand vous combinez des colonnes de type varchar, nvarchar, varbinary, sql_variant ou CLR défini par l’utilisateur qui dépassent 8 060 octets par ligne, tenez compte des points suivants :
-  Les enregistrements volumineux sont automatiquement déplacés vers une autre page dès lors que les enregistrements s'allongent suite à une opération de mise à jour. Les opérations de mise à jour qui raccourcissent les enregistrements peuvent provoquer le rapatriement d'enregistrements vers la page initiale dans l'unité d'allocation IN_ROW_DATA. L’interrogation et d’autres opérations de sélection, telles que les tris ou les jointures portant sur des enregistrements volumineux qui contiennent des données de dépassement de ligne, augmentent le temps de traitement car ces enregistrements sont traités de façon synchrone, et non de manière asynchrone.   
   Par conséquent, quand vous concevez une table comportant plusieurs colonnes de type varchar, nvarchar, varbinary, sql_variant ou CLR défini par l’utilisateur, évaluez le pourcentage de lignes susceptibles de dépasser et la fréquence à laquelle ces données de dépassement sont susceptibles d’être interrogées. S'il est probable qu'il y ait de fréquentes requêtes sur de nombreuses lignes de données de dépassement de ligne, pensez à normaliser la table de manière à ce que certaines colonnes soient déplacées vers une autre table. Celle-ci peut ensuite être interrogée lors d'une opération JOIN asynchrone. 
-  La longueur des différentes colonnes ne doit pas dépasser la limite de 8 000 octets par colonne de type varchar, nvarchar, varbinary, sql_variant et CLR défini par l’utilisateur. Seule la combinaison de leurs longueurs peut dépasser la limite de 8 060 octets par ligne d'une table.
-  La somme des longueurs des colonnes d’autres types de données, notamment char et nchar, ne doit pas dépasser la limite de 8 060 octets par ligne. En outre, les données d'objet volumineux ne sont pas soumises à la limite de 8 060 octets par ligne. 
-  La clé d’un index cluster ne peut pas contenir de colonnes varchar qui possèdent des données dans l’unité d’allocation ROW_OVERFLOW_DATA. Si un index cluster est créé sur une colonne varchar et que les données existantes se trouvent dans l’unité d’allocation IN_ROW_DATA, les actions d’insertion ou de mise à jour réalisées ultérieurement sur la colonne et susceptibles d’envoyer les données hors ligne sont vouées à l’échec. Pour plus d’informations sur les unités d’allocation, consultez Organisation des tables et des index.
-  Vous pouvez inclure des colonnes qui contiennent des données de dépassement de ligne en tant que colonnes clés ou non clés d'un index non-cluster.
-  La limite de taille d'enregistrement pour les tables qui utilisent des colonnes éparses est de 8 018 octets. Quand les données converties plus les données de l’enregistrement existant dépassent 8 018 octets, [MSSQLSERVER ERROR 576](../relational-databases/errors-events/database-engine-events-and-errors.md) est retourné. Quand des colonnes sont converties entre les types éparse et non éparse, le moteur de base de données garde une copie des données de l’enregistrement actif. Cela double temporairement le stockage requis pour l'enregistrement.
-  Pour obtenir des informations sur les tables ou les index pouvant contenir des données de dépassement de ligne, utilisez la fonction de gestion dynamique [sys.dm_db_index_physical_stats](../relational-databases/system-dynamic-management-views/sys-dm-db-index-physical-stats-transact-sql.md).

### <a name="extents"></a>Étendues 

Les extensions constituent l'unité de base dans laquelle l'espace est géré. Une extension est constituée de 8 pages contiguës, soit 64 Ko. Autrement dit, les bases de données SQL Server disposent de 16 étendues par mégaoctet.

[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] contient deux types d’étendues : 

* Les extensions **uniformes** appartiennent à un objet unique ; les huit pages de l’extension ne peuvent être utilisées que par l’objet propriétaire.
* Les extensions **mixtes** sont partagées par huit objets au plus. Chacune des huit pages de l'extension peut être la propriété d'un objet différent.

![Étendues uniformes et mixtes](../relational-databases/media/extents.gif)

Jusqu’à [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] compris, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] n’affecte pas d’étendues complètes aux tables possédant de petites quantités de données. Une nouvelle table ou un nouvel index affecte en général des pages issues d'extensions mixtes. Lorsque la table ou l'index atteint huit pages, il bascule à l'utilisation des extensions uniformes pour les allocations suivantes. Si vous créez un index sur une table existante qui possède un nombre de lignes suffisant pour générer huit pages dans l'index, toutes les allocations à l'index se trouvent dans des extensions uniformes. 

À partir de [!INCLUDE[ssSQL15](../includes/sssql16-md.md)], la valeur par défaut pour la plupart des allocations dans une base de données utilisateur et tempdb consiste à utiliser des étendues uniformes, à l’exception des allocations appartenant aux huit premières pages d’une [chaîne IAM](#IAM). Les allocations pour les bases de données MASTER, msdb et model conservent toujours le comportement précédent. 

> [!NOTE]
> Jusqu'à [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] compris, l’indicateur de trace 1118 peut être utilisé pour modifier l’allocation par défaut afin de toujours utiliser des extensions uniformes. Pour plus d’informations sur cet indicateur de trace, consultez [DBCC TRACEON - Indicateurs de Trace](../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md).   
>   
> À partir de [!INCLUDE[ssSQL15](../includes/sssql16-md.md)], la fonctionnalité fournie par l’indicateur de trace 1118 est automatiquement activée pour tempdb et toutes les bases de données utilisateur. Pour les bases de données utilisateur, ce comportement est contrôlé par l’option `SET MIXED_PAGE_ALLOCATION` de `ALTER DATABASE`, avec la valeur par défaut définie sur OFF, et l’indicateur de trace 1118 n’a aucun effet. Pour plus d’informations, consultez [Options SET d’ALTER DATABASE (Transact-SQL)](../t-sql/statements/alter-database-transact-sql-set-options.md).

À partir de [!INCLUDE[ssSQL11](../includes/sssql11-md.md)], la fonction système `sys.dm_db_database_page_allocations` peut fournir des informations d’allocation de page pour une base de données, une table, un index et une partition.

> [!IMPORTANT]
> La fonction système `sys.dm_db_database_page_allocations` n’est pas documentée et est susceptible d’être modifiée. La compatibilité n'est pas garantie. 

À partir de [!INCLUDE[sql-server-2019](../includes/sssqlv15-md.md)], la fonction système [sys.dm_db_page_info](../relational-databases/system-dynamic-management-views/sys-dm-db-page-info-transact-sql.md) est disponible et retourne des informations sur une page d’une base de données. La fonction retourne une ligne qui contient les informations d’en-tête de la page, notamment object_id, index_id et partition_id. Cette fonction rend superflue l’utilisation de `DBCC PAGE` dans la plupart des cas.

## <a name="managing-extent-allocations-and-free-space"></a>Gestion des allocations des extensions et de l'espace libre 

Les structures de données [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] qui gèrent les allocations des extensions et l’espace libre ont une structure relativement simple. Cette solution offre les avantages suivants : 

* Les informations sur l'espace libre sont très compactes, d'où un nombre de pages d'informations relativement faible.   
  La vitesse s'en trouve augmentée grâce à la réduction du nombre de lectures sur le disque nécessaires à la récupération des informations d'allocation et à l'augmentation de la possibilité de garder en mémoire l'affectation des pages, ce qui réduit encore le nombre de lectures. 

* La plupart des informations d'allocation ne sont pas enchaînées les unes aux autres, ce qui simplifie leur gestion.    
  Chaque allocation ou désallocation de page peut être effectuée rapidement, ce qui diminue les problèmes de contention entre les tâches simultanées nécessitant une allocation ou une désallocation de pages. 

### <a name="managing-extent-allocations"></a>Gestion des allocations des extensions

[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] utilise deux types de tables d’allocation pour enregistrer l’allocation des extensions : 

- **Pages GAM (Global Allocation Map)**    
  Les pages GAM enregistrent les extensions qui ont été allouées. Chaque page GAM couvre 64 000 extensions, soit près de 4 gigaoctets (Go) de données. La page GAM compte un bit pour chaque extension dans l'intervalle couvert. Si la valeur du bit est 1, l'extension est libre. En revanche, si sa valeur est 0, l'extension est allouée. 

- **Pages SGAM (Shared Global Allocation Map)**    
  Les pages SGAM enregistrent les extensions actuellement utilisées comme extensions mixtes et possédant au moins une page inutilisée. Chaque page SGAM couvre 64 000 étendues, soit près de 4 Go de données. La page SGAM compte un bit pour chaque extension dans l'intervalle couvert. Si la valeur du bit est 1, l'extension est utilisée comme extension mixte et possède une page libre. Si la valeur du bit est 0, l'extension n'est pas utilisée comme extension mixte ou correspond à une extension mixte dont toutes les pages sont utilisées. 

Chaque extension possède les schémas de bits suivants dans les tables GAM et SGAM, en fonction de son utilisation actuelle. 

|Utilisation actuelle de l'extension | Valeur du bit GAM | Valeur du bit SGAM |
|---------|----------|------| 
|Libre, inutilisée |1 |0 |
|Extension uniforme ou extension mixte complète |0 |0 |
|Extension mixte avec pages libres |0 |1 |
 
Ceci se traduit par des algorithmes simples de gestion des extensions. 
-   Pour allouer une extension uniforme, le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] recherche un bit 1 dans la table GAM et lui affecte la valeur 0. 
-   Pour trouver une extension mixte comportant des pages libres, le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] recherche un bit 1 dans la table SGAM. 
-   Pour allouer une extension mixte, le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] recherche un bit 1 dans la table GAM, lui affecte la valeur 0, puis affecte la valeur 1 au bit correspondant dans la table SGAM. 
-   Pour désallouer une extension, le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] s’assure que le bit de la table GAM a la valeur 1 et que le bit de la table SGAM a la valeur 0. Les algorithmes qui sont effectivement utilisés en interne par le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] sont plus complexes que ceux décrits dans cet article, du fait que le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] répartit les données uniformément dans une base de données. Toutefois, même les algorithmes réels sont simplifiés afin de ne plus devoir gérer les chaînes d'informations d'allocation des extensions.

### <a name="tracking-free-space"></a>Suivi de l’espace libre

Les pages **PFS (Page Free Space)** enregistrent quand une page individuelle a été allouée, le statut d’allocation et la quantité d’espace libre de chaque page. Chaque page correspond à un octet qui enregistre si la page a été allouée et, le cas échéant, si elle est vide, pleine à 1-50 %, 51-80 %, 81-95 % ou 96-100 %.

Une fois une extension allouée à un objet, le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] utilise les pages PFS pour enregistrer les pages de l'extension qui sont allouées ou libres. Ces informations sont alors utilisées par le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] pour l'allocation de toute nouvelle page. La quantité d'espace libre d'une page n'est conservée que pour les pages de segment, de texte et d'image. Ces informations sont exploitées par le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] pour rechercher une page disposant de suffisamment d'espace libre pour accueillir une nouvelle ligne. Pour les index, le suivi de l'espace libre des pages n'est pas nécessaire étant donné que le point d'insertion d'une nouvelle ligne est défini par les valeurs de clés de l'index.

Une nouvelle page PFS, GAM ou SGAM est ajoutée au fichier de données pour chaque plage supplémentaire dont elle effectue le suivi. Ainsi, il y a une nouvelle page PFS 8 088 pages après la première page PFS, et des pages PFS supplémentaires toutes les 8 088 pages. À titre d’illustration, l’ID de page 1 est une page PFS, l’ID de page 8088 est une page PFS, l’ID de page 16176 est une page PFS, et ainsi de suite. Il y a une nouvelle page GAM 64 000 étendues après la première page GAM, qui effectue le suivi des 64 000 étendues qui la suivent. La séquence continue toutes les 64 000 étendues. De même, il y a une nouvelle page SGAM 64 000 étendues après la première page SGAM et des pages SGAM supplémentaires toutes les 64 000 étendues. L’illustration suivante indique l’ordre des pages utilisées par le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] pour l’allocation et la gestion des extensions.

![manage_extents](../relational-databases/media/manage-extents.gif)

## <a name="managing-space-used-by-objects"></a><a name="IAM"></a> Gestion de l’espace utilisé par les objets 

Une page **IAM (Index Allocation Map)** mappe les étendues d’une portion de 4 Go d’un fichier de base de données utilisées par une unité d’allocation. Une unité d'allocation peut être de trois types :

- IN_ROW_DATA   
    Contient une partition d'un segment ou d'un index.

- LOB_DATA   
   Contient des types de données d’objets volumineux (LOB), tels que XML, VARBINARY(max) et VARCHAR(max).

- ROW_OVERFLOW_DATA   
   Contient des données de longueur variable stockées dans des colonnes VARCHAR, NVARCHAR, VARBINARY ou SQL_VARIANT qui dépassent le seuil de 8 060 octets par ligne. 

Chaque partition d'un segment ou d'un index contient au moins une unité d'allocation IN_ROW_DATA. Elle peut aussi contenir une unité d'allocation LOB_DATA ou ROW_OVERFLOW_DATA, selon le schéma de segment ou d'index.

Une page IAM couvre une plage de 4 Go dans un fichier, comme une page GAM ou SGAM. Si l'unité d'allocation contient des étendues provenant de plusieurs fichiers, ou plusieurs plages de 4 Go dans un fichier, il y aura plusieurs pages IAM liées entre elles dans une chaîne IAM. Ainsi, chaque unité d'allocation contient au moins une page IAM pour chaque fichier dans lequel elle possède des étendues. Un fichier peut aussi contenir plusieurs pages IAM si la plage d'étendues du fichier allouée à l'unité d'allocation dépasse la plage que peut enregistrer une page IAM unique. 

![iam_pages](../relational-databases/media/iam-pages.gif)

Les pages IAM sont allouées au fur et à mesure des besoins pour chaque unité d'allocation et elles sont placées aléatoirement dans le fichier. La vue système `sys.system_internals_allocation_units` pointe vers la première page IAM d’une unité d’allocation. Toutes les pages IAM de cette unité d'allocation sont liées entre elles et forment une chaîne IAM.

> [!IMPORTANT]
> La vue système `sys.system_internals_allocation_units` est destinée exclusivement à un usage interne et elle est susceptible de changer. La compatibilité n'est pas garantie. Cette vue n’est pas disponible dans [!INCLUDE[ssSDSfull](../includes/sssdsfull-md.md)]. 

![iam_chain](../relational-databases/media/iam-chain.gif)
 
Pages IAM liées dans une chaîne par unité d’allocation. Une page IAM possède un en-tête indiquant l’étendue de départ de la plage d’étendues mappée par la page IAM. La page IAM contient aussi une grande image dans laquelle chaque bit représente une étendue. Le premier bit représente la première étendue de la plage, le second la deuxième étendue, et ainsi de suite. Si un bit est égal à 0, l'étendue qu'il représente n'est pas allouée à l'unité d'allocation possédant la page IAM. Si un bit est égal à 1, l'étendue qu'il représente est allouée à l'unité d'allocation possédant la page IAM.

Quand le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] a besoin d’insérer une nouvelle ligne et qu’il n’y a pas de place sur la page active, il a recours aux pages IAM et PFS pour rechercher une page à allouer ou, pour un segment ou une page de texte/image, une page suffisamment grande pour accueillir la ligne. Le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] utilise les pages IAM pour rechercher les étendues allouées à l’unité d’allocation. Pour chaque étendue, le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] recherche les pages PFS afin de vérifier si l’une d’elles peut être utilisée. Chaque page IAM et PFS couvre de nombreuses pages de données de sorte qu'il y a peu de pages IAM et PFS dans une base de données. C'est pourquoi elles se trouvent en général dans la mémoire du pool de mémoires tampons de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], d'où il est possible de les rechercher plus rapidement. Pour les index, le point d’insertion d’une nouvelle ligne est défini par la clé d’index, mais quand une nouvelle page est nécessaire, le processus décrite précédemment se produit.

Le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] n’alloue une nouvelle étendue à une unité d’allocation que s’il ne trouve pas rapidement une page suffisamment grande dans une étendue existante pour accueillir la ligne à insérer. 

<a name="ProportionalFill"></a> Le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] alloue des étendues à partir de celles qui sont disponibles dans le groupe de fichiers à l’aide d’un **algorithme d’allocation de remplissage proportionnel**. Si, dans un même groupe de fichiers composé de deux fichiers, l’un d’entre eux dispose de deux fois plus d’espace disponible que l’autre, deux pages sont allouées à partir du fichier ayant le plus d’espace disponible pour chacune des pages allouées à partir de l’autre fichier. Cela signifie que chaque fichier d'un groupe doit avoir un pourcentage identique d'espace utilisé. 

## <a name="tracking-modified-extents"></a>Suivi des extensions modifiées 

[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] utilise deux structures de données internes pour suivre les extensions modifiées par les opérations de copie en bloc et les extensions modifiées depuis la dernière sauvegarde complète. Ces structures de données accélèrent considérablement les sauvegardes différentielles. Elles accélèrent également l'enregistrement des opérations de copie en bloc dans le journal lorsqu'une base de données utilise le mode de récupération utilisant les journaux de transactions. À l'instar des pages GAM (Global Allocation Map) et SGAM (Shared Global Allocation Map), ces structures sont des bitmaps où chaque bit représente une extension unique. 

- **DCM (Differential Changed Map)**    
   Ces pages suivent les extensions qui ont été modifiées depuis la dernière instruction `BACKUP DATABASE`. Si le bit d’une extension est à 1, l’extension a été modifiée depuis la dernière instruction `BACKUP DATABASE`. Si le bit est à 0, l'extension n'a pas été modifiée. Les sauvegardes différentielles lisent uniquement les pages DCM pour déterminer les extensions qui ont été modifiées. Cela réduit considérablement le nombre de pages qu'une sauvegarde différentielle doit analyser. La durée d'exécution d'une sauvegarde différentielle est proportionnelle au nombre d'extensions modifiées depuis la dernière instruction BACKUP DATABASE, et non à la taille globale de la base de données. 

- **BCM (Bulk Changed Map)**    
   Ces pages suivent les extensions qui ont été modifiées par des opérations journalisées en bloc depuis la dernière instruction `BACKUP LOG`. Si le bit d’une extension est à 1, l’extension a été modifiée par une opération journalisée en bloc après la dernière instruction `BACKUP LOG`. Si le bit est à 0, l'extension n'a pas été modifiée par les opérations journalisées en bloc. Bien que les pages BCM existent dans toutes les bases de données, elles sont uniquement significatives lorsque la base de données emploie le mode de récupération utilisant les journaux de transactions. Dans ce mode de récupération, quand une procédure `BACKUP LOG` est effectuée, le processus de sauvegarde analyse les pages BCM pour identifier les extensions qui ont été modifiées. Il inclut ensuite ces extensions dans la sauvegarde du journal. Ceci permet de récupérer les opérations journalisées en bloc si la base de données est restaurée à partir d'une sauvegarde de la base de données et d'une séquence de sauvegardes de journaux de transactions. Les pages BCM ne sont pas significatives dans une base de données qui utilise le mode de récupération simple, car aucune opération journalisée en bloc n'est enregistrée dans le journal. Elles ne sont pas significatives dans une base de données utilisant le mode de récupération complet puisque ce dernier traite les opérations journalisées en bloc comme des opérations entièrement enregistrées dans le journal. 

L'intervalle entre les pages DCM et les pages BCM est le même que l'intervalle entre les pages GAM et SGAM : 64 000 extensions. Les pages DCM et BCM sont situées juste derrière les pages GAM et SGAM dans un fichier physique :

![special_page_order](../relational-databases/media/special-page-order.gif)

## <a name="see-also"></a>Voir aussi
[sys.allocation_units &#40;Transact-SQL&#41;](../relational-databases/system-catalog-views/sys-allocation-units-transact-sql.md)     
[Segments &#40;tables sans index cluster&#41;](../relational-databases/indexes/heaps-tables-without-clustered-indexes.md#heap-structures)       
[sys.dm_db_page_info](../relational-databases/system-dynamic-management-views/sys-dm-db-page-info-transact-sql.md)     
[Lecture de pages](../relational-databases/reading-pages.md)   
[Écritures de pages](../relational-databases/writing-pages.md)   
