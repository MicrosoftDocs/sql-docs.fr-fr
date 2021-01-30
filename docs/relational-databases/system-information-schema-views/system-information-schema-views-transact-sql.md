---
description: Vues de schémas d’informations système (Transact-SQL)
title: Vues de schémas d’informations système (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/30/2019
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- information schema views
- schemas [SQL Server], information schema views
- metadata [SQL Server], views
- views [SQL Server], information schema
- system views [SQL Server], information schema
ms.assetid: 7e9f1dfe-27e9-40e7-8fc7-bfc5cae6be10
author: markingmyname
ms.author: maghan
ms.openlocfilehash: a39ec8c22b4267e1c6b6bc2f213775836b073a48
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185484"
---
# <a name="system-information-schema-views-transact-sql"></a>Vues de schémas d’informations système (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Une vue de schémas d'informations est l'une des diverses méthodes que fournit [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour l'obtention de métadonnées. Les vues de schémas d'informations fournissent une vue interne indépendante des tables système des métadonnées [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Elles permettent aux applications de fonctionner correctement bien que des changements importants aient été apportés aux tables système sous-jacentes. Les vues de schémas d'informations incluses dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont conformes à la norme ISO pour INFORMATION_SCHEMA.

> [!IMPORTANT]
> Les vues de schémas d'informations ont fait l'objet de certaines modifications qui rompent la compatibilité descendante. Ces modifications sont décrites dans les rubriques spécifiques à chaque vue.

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prend en charge une convention de dénomination en trois parties pour les références au serveur actif. La norme ISO prend également en charge la convention des noms en trois parties. Cependant, les noms utilisés dans les deux conventions sont différents. Les  vues de schémas d'informations sont définies dans un schéma spécial appelé INFORMATION_SCHEMA. Ce schéma figure dans chaque base de données. Chaque vue de schémas d'informations comprend des métadonnées pour tous les objets de données stockés dans une base de données particulière. Le tableau suivant décrit la relation entre les noms [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et les noms standard SQL.

|Nom SQL Server|Nom standard SQL équivalent|
|---------------------|-----------------------------------------------|
|Base de données|Catalogue|
|schéma|schéma|
|Object|Object|
|type de données défini par l'utilisateur|Domain|

Cette convention de mappage de noms s'applique aux vues compatibles [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ISO suivantes.

:::row:::
    :::column:::
        [CHECK_CONSTRAINTS](../../relational-databases/system-information-schema-views/check-constraints-transact-sql.md)

        [COLUMN_DOMAIN_USAGE](../../relational-databases/system-information-schema-views/column-domain-usage-transact-sql.md)

        [COLUMN_PRIVILEGES](../../relational-databases/system-information-schema-views/column-privileges-transact-sql.md)

        [COLUMNS](../../relational-databases/system-information-schema-views/columns-transact-sql.md)

        [CONSTRAINT_COLUMN_USAGE](../../relational-databases/system-information-schema-views/constraint-column-usage-transact-sql.md)

        [CONSTRAINT_TABLE_USAGE](../../relational-databases/system-information-schema-views/constraint-table-usage-transact-sql.md)

        [DOMAIN_CONSTRAINTS](../../relational-databases/system-information-schema-views/domain-constraints-transact-sql.md)

        [DOMAINES](../../relational-databases/system-information-schema-views/domains-transact-sql.md)

        [KEY_COLUMN_USAGE](../../relational-databases/system-information-schema-views/key-column-usage-transact-sql.md)

        [PARAMETERS](../../relational-databases/system-information-schema-views/parameters-transact-sql.md)
    :::column-end:::
    :::column:::
        [REFERENTIAL_CONSTRAINTS](../../relational-databases/system-information-schema-views/referential-constraints-transact-sql.md)

        [ROUTINES](../../relational-databases/system-information-schema-views/routines-transact-sql.md)

        [ROUTINE_COLUMNS](../../relational-databases/system-information-schema-views/routine-columns-transact-sql.md)

        [SCHEMATA](../../relational-databases/system-information-schema-views/schemata-transact-sql.md)

        [TABLE_CONSTRAINTS](../../relational-databases/system-information-schema-views/table-constraints-transact-sql.md)

        [TABLE_PRIVILEGES](../../relational-databases/system-information-schema-views/table-privileges-transact-sql.md)

        [TABLES](../../relational-databases/system-information-schema-views/tables-transact-sql.md)

        [VIEW_COLUMN_USAGE](../../relational-databases/system-information-schema-views/view-column-usage-transact-sql.md)

        [VIEW_TABLE_USAGE](../../relational-databases/system-information-schema-views/view-table-usage-transact-sql.md)

        [VIEWS](../../relational-databases/system-information-schema-views/views-transact-sql.md)
    :::column-end:::
:::row-end:::

En outre, certaines vues comprennent des références à d'autres classes de données telles que les données de type caractère ou binaire.

Lorsque vous faites référence aux vues de schémas d'informations, vous devez utiliser un nom qualifié qui inclut le nom de schéma `INFORMATION_SCHEMA`. Par exemple :

```sql
SELECT TABLE_CATALOG, TABLE_SCHEMA, TABLE_NAME, COLUMN_NAME, COLUMN_DEFAULT
FROM AdventureWorks2012.INFORMATION_SCHEMA.COLUMNS
WHERE TABLE_NAME = N'Product';
```

## <a name="permissions"></a>Autorisations  
La visibilité des métadonnées dans les vues de schémas d’informations est limitée aux éléments sécurisables qu’un utilisateur possède ou sur lesquels une autorisation a été accordée à l’utilisateur. Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).

> [!NOTE]  
> Les vues de schémas d’informations sont définies au niveau du serveur et ne peuvent donc pas être refusées dans le contexte d’une base de données utilisateur. Pour RÉVOQUer ou refuser l’accès (SELECT), vous devez utiliser la base de données Master. Par défaut, le rôle public dispose de l’autorisation SELECT-SELECT pour toutes les vues de schémas d’informations, mais le contenu est limité par les règles de visibilité des métadonnées.

## <a name="see-also"></a>Voir aussi

- [Vues système &#40;&#41;Transact-SQL ](../../relational-databases/system-views/replication-views-transact-sql.md)
- [Types de données &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)
- [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md) 
