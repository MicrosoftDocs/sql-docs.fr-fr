---
description: Affichages catalogue système (Transact-SQL)
title: Affichages catalogue (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/02/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sql13.SysViewExpandPortal.f1
dev_langs:
- TSQL
helpviewer_keywords:
- catalog metadata [SQL Server]
- system views [SQL Server], catalog
- metadata [SQL Server], views
- catalogs [SQL Server], metadata
- catalog views [SQL Server]
- Database Engine [SQL Server], metadata
- catalog views [SQL Server], about catalog views
ms.assetid: 13bccc2f-ed3c-4b58-abd0-ca8bf34a66b8
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 6763194c3a7d12df95caa6e4696c37befe8f6ca8
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093223"
---
# <a name="system-catalog-views-transact-sql"></a>Affichages catalogue système (Transact-SQL)

[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]

Les affichages catalogue retournent des informations utilisées par le [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]. Il est conseillé d'utiliser les affichages catalogue puisqu'ils représentent l'interface la plus générale vers les métadonnées de catalogue et le moyen le plus efficace pour obtenir, transformer et présenter des formulaires personnalisés de ces informations. Toutes les métadonnées de catalogue accessibles à l'utilisateur sont exposées dans des affichages catalogue.

> [!NOTE]
> Les affichages catalogue ne contiennent pas d'informations sur la réplication, la sauvegarde, le plan de maintenance de base de données ou les données de catalogue de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

 Certains affichages de catalogue héritent de lignes d'autres affichages catalogue. Par exemple, l’affichage catalogue [sys. tables](../../relational-databases/system-catalog-views/sys-tables-transact-sql.md) hérite de l’affichage catalogue [sys. Objects](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md) . L'affichage catalogue sys.objects est appelé affichage de base, et l'affichage sys.tables est appelé affichage dérivé. L'affichage catalogue sys.tables retourne les colonnes qui sont spécifiques aux tables, ainsi que toutes les colonnes retournées par l'affichage catalogue sys.objects. L'affichage catalogue sys.objects retourne des lignes pour les objets autres que les tables, notamment les procédures stockées et les vues. Lorsqu'une table est créée, les métadonnées de la table sont retournées dans les deux affichages. Bien que les deux affichages catalogue retournent différents niveaux d'informations concernant la table, il n'existe qu'une seule entrée dans les métadonnées de cette table, avec un nom et un object_id. Cela peut être résumé comme suit :

- L'affichage de base contient un sous-ensemble de colonnes et un sur-ensemble de lignes.
- L'affichage dérivé contient un sur-ensemble de colonnes et un sous-ensemble de lignes.

> [!IMPORTANT]
> Dans les versions ultérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], [!INCLUDE[msCoName](../../includes/msconame-md.md)] peut augmenter la définition de l'affichage catalogue système en ajoutant des colonnes à la fin de la liste des colonnes. Nous vous recommandons d’utiliser la syntaxe SELECT \* FROM *sys.catalog_view_name* dans le code de production, car le nombre de colonnes retournées peut changer et rompre votre application.

Les affichages catalogue de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont classés en plusieurs catégories :

:::row:::
    :::column:::
        [Affichages catalogue des groupes de disponibilité Always On &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/always-on-availability-groups-catalog-views-transact-sql.md)
        
        [Affichages catalogue Azure SQL Database](../../relational-databases/system-catalog-views/azure-sql-database-catalog-views.md)
        
        [Affichages catalogue Change Tracking &#40;Transact-SQL&#41;](../system-catalog-views/change-tracking-catalog-views-sys-change-tracking-databases.md)
        
        [Affichages catalogue de l’assembly CLR &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/clr-assembly-catalog-views-transact-sql.md)
        
        [Vues du collecteur de données &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/data-collector-views-transact-sql.md)
        
        [Les espaces de données &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/data-spaces-transact-sql.md)
        
        [Affichages de Database Mail &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/database-mail-views-transact-sql.md)
        
        [Affichages catalogue du témoin de mise en miroir de bases de données &#40;Transact-SQL&#41;](../system-catalog-views/database-mirroring-witness-catalog-views-sys-database-mirroring-witnesses.md)
        
        [Affichages catalogue de bases de données et de fichiers &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/databases-and-files-catalog-views-transact-sql.md)
        
        [Vues de catalogue des points de terminaison &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/endpoints-catalog-views-transact-sql.md)
        
        [Affichages catalogue des événements étendus &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/extended-events-catalog-views-transact-sql.md)
        
        [Vues de catalogue des propriétés étendues &#40;Transact-SQL&#41;](../system-catalog-views/extended-properties-catalog-views-sys-extended-properties.md)
        
        [Affichages catalogue des opérations externes &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/external-operations-catalog-views-transact-sql.md)
        
        [Affichages catalogue FileStream et filetable &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/filestream-and-filetable-catalog-views-transact-sql.md)
        
        [Affichages catalogue de recherche en texte intégral et de recherche sémantique &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/full-text-search-and-semantic-search-catalog-views-transact-sql.md)
        
        [Affichages catalogue des serveurs liés &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/linked-servers-catalog-views-transact-sql.md)
    :::column-end:::
    :::column:::
        [Messages &#40;pour les erreurs&#41; les affichages catalogue &#40;Transact-SQL&#41;](../system-catalog-views/messages-for-errors-catalog-views-sys-messages.md)
        
        [Vues de catalogue d’objets &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)
        
        [Affichages catalogue des fonctions de partition &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/partition-function-catalog-views-transact-sql.md)
        
        [Vues de la Gestion basée sur des stratégies &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/policy-based-management-views-transact-sql.md)
        
        [Affichages catalogue Resource Governor &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/resource-governor-catalog-views-transact-sql.md)
        
        [Affichages catalogue du magasin de requêtes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/query-store-catalog-views-transact-sql.md)
        
        [Affichages catalogue types scalaires &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/scalar-types-catalog-views-transact-sql.md)
        
        [Affichages catalogue de schémas &#40;Transact-SQL&#41;](../system-catalog-views/schemas-catalog-views-sys-schemas.md)
        
        [Affichages catalogue de sécurité &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)
        
        [Affichages catalogue relatifs à Service Broker &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/service-broker-catalog-views-transact-sql.md)
        
        [Affichages catalogue de configurations à l’ensemble du serveur &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/server-wide-configuration-catalog-views-transact-sql.md)
        
        [Affichages catalogue des données spatiales](../../relational-databases/system-catalog-views/spatial-data-catalog-views.md)
        
        [Vues de catalogue Azure Synapse Analytics et Parallel Data Warehouse](../../relational-databases/system-catalog-views/sql-data-warehouse-and-parallel-data-warehouse-catalog-views.md)
        
        [Affichages catalogue Stretch Database &#40;Transact-SQL&#41;](../system-catalog-views/stretch-database-catalog-views-sys-remote-data-archive-databases.md)
        
        [Schémas XML &#40;les affichages catalogue du système de type XML&#41; &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/xml-schemas-xml-type-system-catalog-views-transact-sql.md)
    :::column-end:::
:::row-end:::

## <a name="see-also"></a>Voir aussi

- [Vues de schémas d’informations &#40;Transact-SQL&#41;](../../relational-databases/system-information-schema-views/system-information-schema-views-transact-sql.md)
- [Tables système &#40;Transact-SQL&#41;](../../relational-databases/system-tables/system-tables-transact-sql.md)
- [Questions fréquentes sur l'interrogation des catalogues système de SQL Server](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.md)
