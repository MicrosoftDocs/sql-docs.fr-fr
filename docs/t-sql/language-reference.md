---
title: Référence Transact-SQL (moteur de base de données)
description: Référence Transact-SQL (moteur de base de données)
ms.custom: ''
ms.date: 04/29/2020
ms.prod: sql
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- sql13.tsqlref.f1
- devlang-tsql
helpviewer_keywords:
- Transact-SQL
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: ''
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: dfe9a07eb1b37f0bddf128b1cbdf39de3d74a643
ms.sourcegitcommit: 713e5a709e45711e18dae1e5ffc190c7918d52e7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/22/2021
ms.locfileid: "98689078"
---
# <a name="transact-sql-reference-database-engine"></a>Référence Transact-SQL (moteur de base de données)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../includes/tsql-appliesto-ss2008-all-md.md)]

Cette rubrique présente les principes de base pour rechercher et utiliser les rubriques de référence sur Microsoft [!INCLUDE[tsql](../includes/tsql-md.md)] (T-SQL). T-SQL est un élément essentiel de l’utilisation des produits et services Microsoft SQL. Tous les outils et applications qui communiquent avec une base de données SQL envoient des commandes T-SQL.  

## <a name="t-sql-compliance-with-sql-standard"></a>Conformité de T-SQL à la norme SQL
Pour obtenir des documents techniques détaillés sur la façon dont certaines normes sont implémentées dans [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], consultez la [documentation de prise en charge des normes dans Microsoft SQL Server](/openspecs/sql_standards/ms-sqlstandlp/89fb00b1-4b9e-4296-92ce-a2b3f7ca01d2).

## <a name="tools-that-use-t-sql"></a>Outils qui utilisent T-SQL
Voici des exemples d’outils Microsoft qui envoient des commandes T-SQL :

- [SQL Server Management Studio (SSMS)](../ssms/download-sql-server-management-studio-ssms.md)
- [Azure Data Studio](../azure-data-studio/download-azure-data-studio.md)
- [Outils SQL Server Data Tools (SSDT)](../ssdt/download-sql-server-data-tools-ssdt.md)
- [sqlcmd](../tools/sqlcmd-utility.md)

## <a name="locate-the-transact-sql-reference-topics"></a>Rechercher les rubriques de référence Transact-SQL  
Pour rechercher les rubriques T-SQL, utilisez la recherche en haut à droite de cette page ou utilisez la table des matières à gauche de la page. Vous pouvez aussi taper un mot clé T-SQL dans la fenêtre de l’éditeur de requête Management Studio et appuyer sur F1.

## <a name="find-system-views"></a>Rechercher des vues système
Pour rechercher des tables, vues, fonctions et procédures système, consultez les liens suivants dans la section [Utilisation des bases de données relationnelles](../relational-databases/databases/databases.md) de la documentation SQL.

- [Vues de catalogue système](../relational-databases/system-catalog-views/catalog-views-transact-sql.md)
- [Vues de compatibilité système](../relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)
- [Vues de gestion dynamique système](../relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)
- [Fonctions système](../relational-databases/system-functions/system-functions-category-transact-sql.md)
- [Vues de schémas d’information système](../relational-databases/system-information-schema-views/system-information-schema-views-transact-sql.md)
- [Procédures stockées sur système](../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)
- [Tables système](../relational-databases/system-tables/system-tables-transact-sql.md)

## <a name="applies-to-references"></a>Informations de référence de type « S’applique à »  

Les rubriques de référence T-SQL englobent plusieurs versions de SQL Server, à partir de 2008, ainsi que d’autres services Azure SQL. En haut de chaque rubrique se trouve une section indiquant les produits et services concernés par le sujet de la rubrique. 

Par exemple, cette rubrique s’applique à toutes les versions et contient l’étiquette suivante.

[!INCLUDE[tsql-appliesto-ss2008-all_md](../includes/tsql-appliesto-ss2008-all-md.md)]

Autre exemple, l’étiquette suivante indique une rubrique qui s’applique uniquement à Azure Synapse Analytics et Parallel Data Warehouse.

[!INCLUDE[tsql-appliesto-xxxxxx-xxxx-asdw-pdw_md](../includes/applies-to-version/asa-pdw.md)]

Dans certains cas, la rubrique est utilisée par un produit ou service, mais les arguments ne sont pas tous pris en charge. Dans ce cas, d’autres sections **S’applique à** sont insérées dans les descriptions des arguments appropriés dans le corps de la rubrique.

## <a name="get-help-from-microsoft-q--a"></a>Obtenir de l’aide des questions et réponses de Microsoft

Pour obtenir une aide en ligne, consultez le [Forum Transact-SQL questions et réponses de Microsoft](/answers/topics/sql-server-transact-sql.html).

## <a name="see-other-language-references"></a>Consulter d’autres informations de référence du langage

La documentation SQL comprend les autres informations de référence du langage suivantes :

- [Informations de référence sur le langage XQuery](../xquery/xquery-language-reference-sql-server.md)
- [Référence du langage Integration Services](../integration-services/integration-services-language-reference.md)
- [Informations de référence sur le langage de réplication](../relational-databases/replication/replication-language-reference.md)
- [Informations de référence sur le langage Analysis Services](../mdx/multidimensional-expressions-mdx-reference.md)

## <a name="next-steps"></a>Étapes suivantes
Maintenant que vous savez comment rechercher les rubriques de référence T-SQL, vous êtes prêt à effectuer ce qui suit :

- Suivre un court tutoriel sur l’écriture de T-SQL, consultez [Tutoriel : Écriture d’instructions Transact-SQL](../t-sql/tutorial-writing-transact-sql-statements.md).
- Consulter les [Conventions de la syntaxe Transact-SQL &#40;Transact-SQL&#41;](../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).