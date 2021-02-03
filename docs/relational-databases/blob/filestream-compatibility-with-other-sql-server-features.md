---
title: Compatibilité de FILESTREAM | Microsoft Docs
description: FILESTREAM stocke les données dans le système de fichiers. Découvrez les instructions, les limitations et les conseils que vous devez garder à l’esprit au moment d’utiliser FILESTREAM avec différentes fonctionnalités SQL Server.
ms.custom: seo-lt-2019
ms.date: 12/13/2019
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: filestream
ms.topic: conceptual
helpviewer_keywords:
- FILESTREAM [SQL Server], other SQL Server features and
- FILESTREAM [SQL Server], limitations
ms.assetid: d2c145dc-d49a-4f5b-91e6-89a2b0adb4f3
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 53ea4630f147ce576ad071d1ac593e00452944a3
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250623"
---
# <a name="filestream-compatibility-with-other-sql-server-features"></a>Compatibilité de FILESTREAM avec d’autres fonctionnalités SQL Server

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Les données FILESTREAM figurant dans le système de fichiers, cette rubrique fournit quelques considérations, indications et limitations relatives à l'utilisation de FILESTREAM avec les fonctionnalités suivantes de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]:  
  
-   [SQL Server Integration Services (SSIS)](#ssis)  
  
-   [Requêtes distribuées et serveurs liés](#distqueries)  
  
-   [Chiffrement](#encryption)  
  
-   [Instantanés de base de données](#DatabaseSnapshot)  
  
-   [Réplication](#Replication)  
  
-   [Copie des journaux de transaction](#LogShipping)  
  
-   [Mise en miroir de bases de données](#DatabaseMirroring)  
  
-   [Indexation de texte intégral](#FullText)  
  
-   [Clustering de basculement](#FailoverClustering)  
  
-   [SQL Server Express](#SQLServerExpress)  
  
-   [Bases de données autonomes](#contained)  
  
##  <a name="sql-server-integration-services-ssis"></a><a name="ssis"></a> SQL Server Integration Services (SSIS)  
 SQL Server Integration Services (SSIS) gère les données FILESTREAM dans le flux de données comme toutes les autres données BLOB en utilisant le type de données SSIS DT_IMAGE.  
  
 Vous pouvez utiliser la transformation d'importation de colonne pour charger des fichiers du système de fichiers dans une colonne FILESTREAM. Vous pouvez également utiliser la transformation d'exportation de colonne pour extraire des fichiers d'une colonne FILESTREAM à un autre emplacement dans le système de fichiers.  
  
##  <a name="distributed-queries-and-linked-servers"></a><a name="distqueries"></a> Requêtes distribuées et serveurs liés  
 Vous pouvez utiliser des données FILESTREAM dans vos requêtes distribuées et sur les serveurs liés en les traitant comme des données **varbinary(max)** . Vous ne pouvez pas utiliser la fonction FILESTREAM **PathName()** dans les requêtes distribuées qui utilisent un nom en quatre parties, même quand le nom fait référence au serveur local. En revanche, vous pouvez utiliser **PathName()** dans la requête interne d’une requête directe qui utilise **OPENQUERY()** .  
  
##  <a name="encryption"></a><a name="encryption"></a> Chiffrement  
 Les données FILESTREAM ne sont pas chiffrées, même lorsque le chiffrement transparent des données est activé.  
  
##  <a name="database-snapshots"></a><a name="DatabaseSnapshot"></a> Instantanés de base de données  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne prend pas en charge les [instantanés de base de données](../../relational-databases/databases/database-snapshots-sql-server.md) pour les groupes de fichiers FILESTREAM. Si un groupe de fichiers FILESTREAM est inclus dans une clause CREATE DATABASE ON, l'instruction échoue et une erreur est levée.  
  
 Lorsque vous utilisez FILESTREAM, vous pouvez créer des instantanés de base de données de groupes de fichiers standard (non-FILESTREAM). Les groupes de fichiers FILESTREAM sont marqués comme hors connexion pour ces instantanés de base de données.  
  
 Une instruction SELECT exécutée sur une table FILESTREAM dans un instantané de base de données ne doit pas inclure de colonne FILESTREAM ; autrement, le message d'erreur suivant est retourné :  
  
 `Could not continue scan with NOLOCK due to data movement.`  
  
##  <a name="replication"></a><a name="Replication"></a> Replication  
 Une colonne **varbinary(max)** qui a l’attribut FILESTREAM activé sur le serveur de publication peut être répliquée sur un abonné avec ou sans l’attribut FILESTREAM. Pour spécifier la façon dont la colonne est répliquée, utilisez la boîte de dialogue **Propriétés de l'article - \<Article>** ou le paramètre @schema_option de [sp_addarticle](../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md) ou [sp_addmergearticle](../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md). Les données répliquées sur une colonne **varbinary(max)** qui n’a pas l’attribut FILESTREAM ne doivent pas dépasser la limite de 2 Go pour ce type de données, autrement une erreur d’exécution est générée. Nous vous recommandons de répliquer l'attribut FILESTREAM, à moins que vous ne répliquiez des données sur [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]. La réplication de tables qui possèdent des colonnes FILESTREAM sur des abonnés [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] n'est pas prise en charge, quelle que soit l'option de schéma définie.  
  
> [!NOTE]  
>  La réplication de grandes valeurs de données à partir d'Abonnés [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] vers [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] est limitée à des valeurs de données de 256 Mo maximum. Pour plus d'informations, consultez [Spécifications de capacité maximale](../../sql-server/maximum-capacity-specifications-for-sql-server.md).  
  
### <a name="considerations-for-transactional-replication"></a>Considérations relatives à la réplication transactionnelle  
 Si vous utilisez des colonnes FILESTREAM dans des tables publiées pour la réplication transactionnelle, notez les considérations suivantes :  
  
-   Si des tables incluent des colonnes qui ont l’attribut FILESTREAM, vous ne pouvez pas utiliser la valeur *database snapshot* ou *database snapshot character* pour la propriété @sync_method de [sp_addpublication](../../relational-databases/system-stored-procedures/sp-addpublication-transact-sql.md).  
  
-   L'option max text repl size spécifie la quantité maximale de données qui peuvent être insérées dans une colonne publiée pour la réplication. Cette option peut être utilisée pour contrôler la taille des données FILESTREAM répliquées.  
  
-   Si vous spécifiez l'option de schéma pour répliquer l'attribut FILESTREAM, mais que vous filtrez la colonne **uniqueidentifier** requise par FILESTREAM ou que vous spécifiez qu'il ne faut pas répliquer la contrainte UNIQUE pour la colonne, la réplication ne réplique pas l'attribut FILESTREAM. La colonne est répliquée uniquement en tant que colonne **varbinary(max)** .  
  
### <a name="considerations-for-merge-replication"></a>Considérations relatives à la réplication de fusion  
 Si vous utilisez des colonnes FILESTREAM dans des tables publiées pour la réplication de fusion, notez les considérations suivantes :  
  
-   La réplication de fusion et FILESTREAM requièrent une colonne de type de données **uniqueidentifier** afin d'identifier chaque ligne dans une table. La réplication de fusion ajoute automatiquement une colonne si la table n'en a pas. La réplication de fusion requiert que la propriété ROWGUIDCOL de la colonne soit définie et que la valeur par défaut soit NEWID() ou NEWSEQUENTIALID(). En plus de ces spécifications, FILESTREAM requiert qu'une contrainte UNIQUE soit définie pour la colonne. Ces exigences entraînent les conséquences suivantes :  
  
    -   Si vous ajoutez une colonne FILESTREAM à une table qui est déjà publiée pour la réplication de fusion, assurez-vous que la colonne **uniqueidentifier** a une contrainte UNIQUE. Si elle n'a pas de contrainte UNIQUE, ajoutez une contrainte nommée à la table dans la base de données de publication. Par défaut, la réplication de fusion publiera cette modification de schéma et elle s'appliquera à chaque base de données d'abonnement.  
  
         Si vous ajoutez une contrainte UNIQUE manuellement comme décrit et que vous souhaitez supprimer la réplication de fusion, vous devez d'abord supprimer la contrainte UNIQUE, sinon la suppression de réplication échouera.  
  
    -   Par défaut, la réplication de fusion utilise NEWSEQUENTIALID() car ses performances peuvent être supérieures à celles de NEWID(). Si vous ajoutez une colonne **uniqueidentifier** à une table qui sera publiée pour la réplication de fusion, spécifiez NEWSEQUENTIALID() comme valeur par défaut.  
  
-   La réplication de fusion inclut une optimisation pour répliquer de grands types d'objets. Cette optimisation est contrôlée par le paramètre @stream_blob_columns de [sp_addmergearticle](../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md). Si vous définissez l’option de schéma de façon à répliquer l’attribut FILESTREAM, le paramètre @stream_blob_columns a la valeur **true**. Cette optimisation peut être substituée en utilisant [sp_changemergearticle](../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md). Cette procédure stockée vous permet d’affecter la valeur **false** à @stream_blob_columns. Si vous ajoutez une colonne FILESTREAM à une table qui est déjà publiée pour la réplication de fusion, nous vous recommandons d’affecter la valeur **true** à l’option en utilisant sp_changemergearticle.  
  
-   L'activation de l'option de schéma pour FILESTREAM après qu'un article a été créé peut provoquer l'échec de la réplication si les données dans une colonne FILESTREAM dépassent 2 Go et qu'il y a un conflit pendant la réplication. Si vous pensez que cette situation surviendra, il est recommandé de supprimer et de recréer l'article de table avec l'option de schéma FILESTREAM appropriée activée au moment de la création.  
  
-   La réplication de fusion peut synchroniser des données FILESTREAM sur une connexion HTTPS en utilisant la [Synchronisation Web](../../relational-databases/replication/web-synchronization-for-merge-replication.md). Ces données ne peuvent pas dépasser la limite de 50 Mo pour la Synchronisation Web, sinon une erreur d'exécution est générée.  
  
##  <a name="log-shipping"></a><a name="LogShipping"></a> Copie des journaux de transaction  
 La[copie des journaux de transaction](../../database-engine/log-shipping/about-log-shipping-sql-server.md) prend en charge FILESTREAM. Les serveurs principaux et secondaires doivent exécuter [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]ou une version ultérieure, et FILESTREAM doit être activé.  
  
##  <a name="database-mirroring"></a><a name="DatabaseMirroring"></a> Mise en miroir de bases de données  
 La mise en miroir de bases de données ne prend pas en charge FILESTREAM. Un groupe de fichiers FILESTREAM ne peut pas être créé sur le serveur principal. La mise en miroir de bases de données ne peut pas être configurée pour une base de données qui contient des groupes de fichiers FILESTREAM.  
  
##  <a name="full-text-indexing"></a><a name="FullText"></a> Indexation de texte intégral  
 L’[indexation de texte intégral](../../relational-databases/search/populate-full-text-indexes.md) fonctionne avec une colonne FILESTREAM de la même façon qu’avec une colonne **varbinary(max)** . La table FILESTREAM doit avoir une colonne qui contient l'extension de nom de fichier pour chaque objet blob FILESTREAM. Pour plus d’informations, consultez [Exécuter une requête avec une recherche en texte intégral](../../relational-databases/search/query-with-full-text-search.md), [Configurer et gérer des filtres pour la recherche](../../relational-databases/search/configure-and-manage-filters-for-search.md) et [sys.fulltext_document_types &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-document-types-transact-sql.md).  
  
 Le moteur de texte intégral indexe le contenu des objets blob FILESTREAM. L'indexation de fichiers tels que des images peut ne pas être utile. Lorsqu'un objet blob FILESTREAM est mis à jour, il est réindexé.  
  
##  <a name="failover-clustering"></a><a name="FailoverClustering"></a> Clustering de basculement  
 Pour le clustering de basculement, les groupes de fichiers FILESTREAM doivent être mis sur un disque partagé. FILESTREAM doit être activé sur chaque nœud dans le cluster qui hébergera l'instance FILESTREAM. Pour plus d’informations, consultez [Configurer FILESTREAM sur un cluster de basculement](../../relational-databases/blob/set-up-filestream-on-a-failover-cluster.md).  
  
##  <a name="sql-server-express"></a><a name="SQLServerExpress"></a> SQL Server Express  
 [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] prend en charge FILESTREAM. La limite de taille de base de données de 10 Go n'inclut pas le conteneur de données FILESTREAM.  
  
##  <a name="contained-databases"></a><a name="contained"></a> Bases de données autonomes  
 La fonctionnalité FILESTREAM requiert une configuration spécifique hors de la base de données. Par conséquent, une base de données qui utilise FILESTREAM ou FileTable n'est pas entièrement contenue.  
  
 Vous pouvez définir l’autonomie de la base de données sur PARTIAL si vous souhaitez utiliser certaines fonctionnalités des bases de données autonomes, telles que les utilisateurs autonomes. Dans ce cas, toutefois, vous devez savoir qu'une partie des paramètres de la base de données ne sont pas contenus dans la base de données et ne sont pas automatiquement déplacés avec celle-ci.  
  
## <a name="see-also"></a>Voir aussi  
 [Objets binaires volumineux &#40;Objet BLOB&#41; Données &#40;SQL Server&#41;](../../relational-databases/blob/binary-large-object-blob-data-sql-server.md)  
  
