---
title: Spécifications de capacité maximale pour SQL Server
description: Cet article présente la taille et le nombre maximal de divers objets définis dans les composants SQL Server ainsi que des informations complémentaires.
ms.date: 03/05/2020
ms.prod: sql
ms.reviewer: ''
ms.custom: ''
ms.technology: release-landing
ms.topic: conceptual
helpviewer_keywords:
- objects [SQL Server]
- number capacity specifications [SQL Server]
- size [SQL Server], capacity specifications
- number of objects
- capacity specifications [SQL Server]
- maximum capacity specifications [SQL Server]
- size [SQL Server]
- replication capacity specifications [SQL Server]
- objects [SQL Server], capacity specifications
- Database Engine [SQL Server], capacity specifications
ms.assetid: 13e95046-0e76-4604-b561-d1a74dd824d7
ms.author: mikeray
author: MikeRayMSFT
ms.openlocfilehash: b4f0e4ed75e992095cff1f9ae315e3036ba7ef90
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/25/2021
ms.locfileid: "98766223"
---
# <a name="maximum-capacity-specifications-for-sql-server"></a>Spécifications de capacité maximale pour SQL Server

[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

Cet article présente la taille maximale et le nombre maximal des différents objets définis dans les composants [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].

>[!NOTE]
>En plus des informations contenues dans cet article, nous vous conseillons de consulter les liens suivants :
>
>* [Télécharger SQL Server](https://www.microsoft.com/sql-server/sql-server-downloads)
>* [Configurations matérielle et logicielle requises pour l’installation de SQL Server](../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md)
>* [Paramètres de l’outil d’analyse de configuration système](../database-engine/install-windows/check-parameters-for-the-system-configuration-checker.md)
>

## <a name="ssde-objects"></a>Objets [!INCLUDE[ssDE](../includes/ssde-md.md)]

Taille maximale et nombre maximal des différents objets définis dans les bases de données [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] ou référencés dans les instructions [!INCLUDE[tsql](../includes/tsql-md.md)] .

|[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] [!INCLUDE[ssDE](../includes/ssde-md.md)] objet|Tailles maximales/nombres maximaux [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] (64-bit)|Informations supplémentaires|
|---------------------------------------------------------|------------------------------------------------------------------|----------------------------|
|Taille du lot|65 536 <sup>*</sup> (Taille de paquet réseau)|La taille des paquets réseau représente la taille des paquets TDS (Tabular Data Stream) utilisés pour la communication entre des applications et le [!INCLUDE[ssDE](../includes/ssde-md.md)] relationnel. La taille par défaut s'élève à 4 Ko ; elle est contrôlée par l'option de configuration Taille du paquet réseau.|
|Octets par colonne de chaîne courte|8,000||
|Octets par `GROUP BY`, `ORDER BY`|8,060||
|Octets par clé d’index|900 octets pour un index cluster. 1 700 pour un index non cluster.|Le nombre maximal d’octets dans une clé d’index cluster s’élève à 900 dans [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Pour une clé d’index non cluster, la valeur maximale est 1700 octets.<br /><br /> Vous pouvez définir une clé à l’aide de colonnes de longueur variable dont les tailles maximales, additionnées, dépasse la limite. Toutefois, la taille combinée des données de ces colonnes ne peut jamais dépasser la limite.<br /><br /> Dans un index non cluster, vous pouvez inclure des colonnes non-clés supplémentaires. Elles n’entrent pas en compte pour la limite de taille de la clé. Les colonnes non-clés peuvent améliorer les performances de certaines requêtes.|
|Octets par clé d’index pour les tables optimisées en mémoire|2500 octets pour un index non cluster. Aucune limite pour un index de hachage, tant que toutes les clés d’index s’ajustent sur la ligne.|Sur une table optimisée en mémoire, un index non cluster ne peut pas avoir de colonnes clés dont la taille maximale déclarée dépasse 2500 octets. Peu importe que la taille des données réelles dans les colonnes clés soit inférieure aux tailles maximales déclarées.<br /><br /> Pour une clé d’index de hachage, il n’existe aucune limite de taille.<br /><br /> Pour les index sur les tables optimisées en mémoire, il n’existe pas de concept de colonnes incluses, puisque tous les index couvrent, par nature, toutes les colonnes.<br /><br /> Pour une table optimisée en mémoire, même si la taille de ligne est de 8060 octets, certaines colonnes de longueur variable peuvent être physiquement stockées en dehors de ces 8060 octets. Toutefois, les tailles maximales déclarés de toutes les colonnes clés pour tous les index sur une table, plus les éventuelles colonnes de longueur fixe supplémentaires dans la table, doivent tenir dans les 8060 octets.|
|Octets par clé étrangère|900||
|Octets par clé primaire|900||
|Octets par ligne|8,060|[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] prend en charge le stockage avec dépassement de ligne, qui permet d’envoyer hors ligne les colonnes de longueur variable. Seule une racine de 24 octets est stockée dans l’enregistrement principal pour les colonnes de longueur variable envoyées hors ligne. Cette fonctionnalité autorise une limite supérieure à celle des versions précédentes de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Pour plus d’informations, consultez [Prise en charge des lignes de grande taille](../relational-databases/pages-and-extents-architecture-guide.md#large-row-support).|
|Octets par ligne dans les tables optimisées en mémoire|8,060|À compter de [!INCLUDE[ssSQL15](../includes/sssql16-md.md)] , les tables optimisées en mémoire prennent en charge le stockage hors ligne. Les colonnes de longueur variable sont envoyées hors ligne si la taille maximale de toutes les colonnes dans la table dépasse 8060 octets. Cette action est une décision prise au moment de la compilation. Seule une référence de huit octets est stockée dans la ligne pour les colonnes stockées hors ligne. Pour plus d’informations, consultez [Taille de la table et des lignes dans les tables optimisées en mémoire](../relational-databases/in-memory-oltp/table-and-row-size-in-memory-optimized-tables.md).|
|Octets dans le texte source d'une procédure stockée|Taille de lot inférieure ou 250 Mo||
|Octets par colonne `varchar(max) `, `varbinary(max)`, `xml`, `text` ou `image`|2^31-1||
|Caractères par colonne `ntext` ou `nvarchar(max)`|2^30-1||
|Index cluster par table|1||
|Colonnes dans `GROUP BY`, `ORDER BY`|Limité uniquement par le nombre d'octets||
|Colonnes ou expressions dans une instruction `GROUP BY WITH CUBE` ou `WITH ROLLUP`|10||
|Colonnes par clé d’index|32|Si la table contient au moins un index XML, la clé de clustering de la table d’utilisateur est limitée à 31 colonnes, car la colonne XML est ajoutée à la clé de clustering du principal index XML. Dans [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], vous pouvez inclure des colonnes non-clés dans un index non-cluster pour éviter la limitation à un maximum de 32 colonnes clés. Pour plus d’informations, consultez [Créer des index avec colonnes incluses](../relational-databases/indexes/create-indexes-with-included-columns.md).|
|Colonnes par clé étrangère ou par clé primaire|32||
|Colonnes par instruction `INSERT`|4 096||
|Colonnes par instruction `SELECT`|4 096||
|Colonnes par table|1 024|Les tables qui incluent des jeux de colonnes éparses incluent jusqu’à 30 000 colonnes. Consultez [Jeux de colonnes éparses](../relational-databases/tables/use-column-sets.md).|
|Colonnes par instruction `UPDATE`|4 096|Des limites différentes s’appliquent aux [jeux de colonnes éparses](../relational-databases/tables/use-column-sets.md).|
|Colonnes par vue|1 024||
|Connexions par client|Valeur maximale des connexions configurées||
|Taille de la base de données|524 272 téraoctets||
|Bases de données par instance de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]|32 767||
|Groupes de fichiers par base de données|32 767||
|Groupes de fichiers par base de données pour les données optimisées en mémoire|1||
|Fichiers par base de données|32 767||
|Taille de fichier (données)|16 téraoctets||
|Taille de fichier (journal)|2 téraoctets||
|Fichiers de données pour les données optimisées en mémoire par base de données|4 096 dans [!INCLUDE[ssSQL14](../includes/ssSQL14-md.md)]. Les versions ultérieures de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] n’imposent pas une telle limite stricte.||
|Fichier delta par fichier de données pour les données optimisées en mémoire|1||
|Références de table de clé étrangère par table|Sortantes = 253. Entrantes = 10 000.|Pour connaître les restrictions associées, consultez [Create Foreign Key Relationships](../relational-databases/tables/create-foreign-key-relationships.md).|
|Longueur d'identificateur (en caractères)|128||
|Instances par ordinateur|50 instances sur un serveur autonome.<br /><br />25 instances de cluster de basculement lors de l’utilisation d’un disque de cluster partagé comme stockage.<br/><br/>50 instances de cluster de basculement avec des partages de fichiers SMB comme option de stockage.||
|Index par table optimisée en mémoire|999 à partir de [!INCLUDE[ssSQL17](../includes/ssSQL17-md.md)] et dans [!INCLUDE[ssSDSFull](../includes/ssSDSFull-md.md)]<br/>8 dans [!INCLUDE[ssSQL14](../includes/ssSQL14-md.md)] et [!INCLUDE[ssSQL15](../includes/sssql16-md.md)]||
|Longueur d’une chaîne contenant des instructions SQL (taille du traitement)|65 536 (Taille de paquet réseau)|La taille des paquets réseau représente la taille des paquets TDS (Tabular Data Stream) utilisés pour la communication entre des applications et le [!INCLUDE[ssDE](../includes/ssde-md.md)] relationnel. La taille par défaut s'élève à 4 Ko ; elle est contrôlée par l'option de configuration Taille du paquet réseau.|
|Verrous par connexion|Verrous maximaux par serveur||
|Verrous par instance de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]|Limité uniquement par la mémoire|Cette valeur s’applique à l’allocation de verrouillage statique. Les verrous dynamiques sont uniquement limités par la mémoire.|
|Niveaux d’imbrication des procédures stockées|32|Si une procédure stockée accède à plus de 64 bases de données ou à plus de deux bases entrelacées, vous recevez un message d’erreur.|
|Sous-requêtes imbriquées|32||
|Transactions imbriquées|4,294,967,296|| 
|Niveaux de déclencheurs imbriqués|32||
|Index non cluster par table|999||
|Nombre d’expressions distinctes dans la clause `GROUP BY` quand l’un des éléments suivants est présent : `CUBE`, `ROLLUP`, `GROUPING SETS`, `WITH CUBE`, `WITH ROLLUP`|32||
|Nombre de jeux de regroupement générés par les opérateurs dans la clause `GROUP BY`|4 096||
|Paramètres par procédure stockée|2,100||
|Paramètres par fonction définie par l'utilisateur|2,100||
|REFERENCES par table|253||
|Lignes par table|Limité par le stockage disponible||
|Tables par base de données|Limité par le nombre total d’objets dans une base de données|Les objets comprennent les tables, les vues, les procédures stockées, les fonctions définies par l’utilisateur, les déclencheurs, les règles, les valeurs par défaut et les contraintes. Au total, le nombre de tous les objets d'une base de données ne peut pas dépasser 2 147 483 647.|
|Partitions par table ou index partitionné|15,000||
|Statistiques sur les colonnes non indexées|30,000|| 
|Tables par instruction `SELECT`|Limité uniquement par les ressources disponibles||
|Déclencheurs par table|Limité par le nombre d'objets dans une base de données|Les objets comprennent les tables, les vues, les procédures stockées, les fonctions définies par l’utilisateur, les déclencheurs, les règles, les valeurs par défaut et les contraintes. Au total, le nombre de tous les objets d'une base de données ne peut pas dépasser 2 147 483 647.|
|Connexions utilisateur|32 767||
|Index XML|249||

## <a name="ssnoversion-utility-objects"></a>Objets utilitaires [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]

Taille maximale et nombre maximal des différents objets testés dans l’utilitaire [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] .

|[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Objet utilitaire|Tailles maximales/nombres maximaux [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] (64-bit)|
|----------------------------------------------|------------------------------------------------------------------|
|Ordinateurs (ordinateurs physique ou ordinateurs virtuels) par utilitaire [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]|100|
|Instances de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] par ordinateur|5|
|Nombre total d'instances de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] par utilitaire [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]|200<sup>*</sup>|
|Bases de données utilisateur par instance de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], notamment les applications de la couche Données|50|
|Nombre total de bases de données utilisateur par utilitaire [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]|1 000|
|Groupes de fichiers par base de données|1|
|Fichiers de données par groupe de fichiers|1|
|Fichiers journaux par base de données|1|
|Volumes par ordinateur|3|

<sup>*</sup> Le nombre maximal d’instances managées de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] prises en charge par l’utilitaire [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] peut varier en fonction de la configuration matérielle du serveur. Pour obtenir des informations de prise en main, consultez [Fonctionnalités et tâches de l’utilitaire SQL Server](../relational-databases/manage/sql-server-utility-features-and-tasks.md). [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] n’est pas disponible dans toutes les éditions de [!INCLUDE[ssnoversion](../includes/ssnoversion-md.md)]. Pour obtenir la liste des fonctionnalités prises en charge par les éditions de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], consultez [Fonctionnalités prises en charge par les éditions de SQL Server 2019](./editions-and-components-of-sql-server-version-15.md), [Fonctionnalités prises en charge par les éditions de SQL Server 2017](./editions-and-components-of-sql-server-2017.md) et [Fonctionnalités prises en charge par les éditions de SQL Server 2016](./editions-and-components-of-sql-server-2016.md).

## <a name="ssnoversion-data-tier-application-objects"></a>[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Objets d’application de la couche Données

Taille maximale et nombre maximal des différents objets testés dans les applications de la couche Données (DAC) [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] .

|[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Object DAC|Tailles maximales/nombres maximaux [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] (64-bit)|
|------------------------------------------|------------------------------------------------------------------|
|Bases de données par DAC|1|
|Objets par DAC <sup>*</sup>|Limité par le nombre d'objets dans une base de données ou la mémoire disponible.|

<sup>*</sup> Les types d’objets inclus dans la limite sont les utilisateurs, les tables, les vues, les procédures stockées, les fonctions définies par l’utilisateur, les types de données définis par l’utilisateur, les rôles de base de données, les schémas et les types de table définis par l’utilisateur.

## <a name="replication-objects"></a>Objets de réplication

Taille maximale et nombre maximal des différents objets définis dans la réplication [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] .

|[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Objet de réplication|Taille maximale/nombre maximal dans SQL Server (64 bits)|
|--------------------------------------------------|---------------------------------------------------|
|Articles (publication de fusion)|2 048|
|Articles (publication d'instantané ou transactionnelle)|32 767|
|Colonnes dans une table<sup>*</sup> (publication de fusion)|246|
|Colonnes dans une table<sup>**</sup> (publication d’instantané ou transactionnelle [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)])|1 000|
|Colonnes dans une table<sup>**</sup> (publication d’instantané ou transactionnelle Oracle)|995|
|Octets pour une colonne utilisée dans un filtre de lignes (publication de fusion)|1 024|
|Octets pour une colonne utilisée dans un filtre de lignes (publication d'instantané ou transactionnelle)|8,000|

<sup>*</sup>Si le suivi de lignes est utilisé pour la détection de conflits (valeur par défaut), la table de base peut inclure 1024 colonnes au maximum, mais les colonnes doivent être filtrées à partir de l’article afin que 246 colonnes au maximum soient publiées. Si le suivi de colonnes est utilisé, la table de base peut inclure 246 colonnes au maximum.

<sup>**</sup>La table de base peut inclure le nombre maximal de colonnes autorisées dans la base de données de publication (1024 pour [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]), mais les colonnes doivent être filtrées à partir de l’article si elles sont plus nombreuses que le maximum spécifié pour le type de publication.

## <a name="see-also"></a>Voir aussi

[Fonctionnalités et tâches de l’utilitaire SQL Server](../relational-databases/manage/sql-server-utility-features-and-tasks.md)