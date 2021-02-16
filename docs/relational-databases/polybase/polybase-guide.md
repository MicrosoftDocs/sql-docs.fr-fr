---
title: Qu’est-ce que PolyBase ? | Microsoft Docs
description: PolyBase permet à votre instance SQL Server de traiter des requêtes Transact-SQL qui lisent des données dans des sources de données externes comme Hadoop et le stockage Blob Azure.
ms.date: 12/14/2019
ms.prod: sql
ms.technology: polybase
ms.topic: overview
f1_keywords:
- PolyBase
- PolyBase, guide
helpviewer_keywords:
- PolyBase
- PolyBase, overview
- Hadoop import
- Hadoop export
- Hadoop export, PolyBase overview
- Hadoop import, PolyBase overview
ms.custom: contperf-fy21q2
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: ''
monikerRange: '>=sql-server-2016||>=sql-server-linux-2017||>=aps-pdw-2016||=azure-sqldw-latest'
ms.openlocfilehash: e4f0bfbac2fe4e49261f9940a97fd73e05676661
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100062224"
---
# <a name="what-is-polybase"></a>Qu’est-ce que PolyBase ?

[!INCLUDE[appliesto-ss-xxxx-asdw-pdw-md](../../includes/appliesto-ss-xxxx-asdw-pdw-md.md)]

PolyBase permet à votre instance SQL Server de traiter des requêtes Transact-SQL qui lisent des données dans des sources de données externes. Cette même requête peut aussi accéder aux tables relationnelles dans votre instance SQL Server. PolyBase lui permet de joindre également les données issues de sources externes et de SQL Server.

Pour utiliser Polybase dans une instance SQL Server :

1. [Installer PolyBase sur Windows](polybase-installation.md)
1. Créer une [source de données externe](../../t-sql/statements/create-external-data-source-transact-sql.md)
1. Créer une [table externe](../../t-sql/statements/create-external-table-transact-sql.md)

Ensemble, ils fournissent la connexion à la source de données externe.

SQL Server 2016 introduit Polybase avec prise en charge des connexions à Hadoop et au stockage d’objets Blob Azure.

SQL Server 2019 introduit des connecteurs supplémentaires, notamment SQL Server, Oracle, Teradata et MongoDB.

![Logique PolyBase](../../relational-databases/polybase/media/polybase-logical.png "Logique PolyBase")

Polybase transmet certains calculs à la source externe pour optimiser la requête globale. L’accès externe PolyBase n’est pas limité à Hadoop. D’autres tables non relationnelles et non structurées sont également prises en charge, comme les fichiers texte délimités.

Voici quelques exemples de connecteurs externes :

- [SQL Server](polybase-configure-sql-server.md)
- [Oracle](polybase-configure-oracle.md)
- [Teradata](polybase-configure-teradata.md)
- [MongoDB](polybase-configure-mongodb.md)

### <a name="supported-sql-products-and-services"></a>Produits et services SQL pris en charge

PolyBase propose ces fonctionnalités pour les produits SQL de Microsoft suivants :

- SQL Server 2016 et ultérieur (Windows uniquement)
- Analytics Platform System (anciennement Parallel Data Warehouse) ;
- Azure Synapse Analytics

### <a name="azure-integration"></a>Intégration d’Azure

Avec l’aide sous-jacente de PolyBase, les requêtes T-SQL peuvent également importer et exporter des données dans le Stockage Blob Azure. Par ailleurs, PolyBase permet à Azure Synapse Analytics d’importer et d’exporter des données dans Azure Data Lake Storage et le Stockage Blob Azure.

## <a name="why-use-polybase"></a>Pourquoi utiliser PolyBase ?

Polybase vous permet de joindre des données d’une instance SQL Server à des données externes. Avant Polybase, pour joindre des données à des sources de données externes, vous pouviez :

- Transférer la moitié de vos données afin que toutes les données se trouvent à un seul emplacement.
- interroger les deux sources de données, puis écrire une logique de requête personnalisée pour joindre et intégrer les données au niveau du client.

Polybase vous permet d’utiliser simplement Transact-SQL pour joindre les données.

PolyBase vous dispense d’installer des logiciels supplémentaires dans votre environnement Hadoop. On interroge les données externes suivant la même syntaxe T-SQL que pour une table de base de données. Toutes les actions implémentées par PolyBase se produisent en toute transparence. L’auteur de la requête n’a besoin d’aucune connaissance de la source externe.

### <a name="polybase-uses"></a>Utilisations de PolyBase

PolyBase permet les scénarios suivants dans SQL Server :

- **Interrogation des données stockées dans Hadoop à partir d’une instance SQL Server ou PDW.** Les utilisateurs stockent des données dans des systèmes distribués et évolutifs économiques, tels que Hadoop. Avec PolyBase, il est facile d’interroger les données via T-SQL.

- **Interrogation des données stockées dans Stockage Blob Azure.** Pratique, Azure Blob Storage est un emplacement qui permet de stocker les données destinées à être utilisées par les services Azure.  PolyBase permet d’accéder facilement aux données à l’aide de T-SQL.

- **Importation de données provenant d’Hadoop, du Stockage Blob Azure ou d’Azure Data Lake Storage.** Profitez de la rapidité des fonctionnalités d’analyse et de la technologie columnstore de Microsoft SQL en important les données issues d’Hadoop, du Stockage Blob Azure ou d’Azure Data Lake Storage dans des tables relationnelles. Aucun outil d’ETL ou d’importation distinct n’est nécessaire.

- **Exportation de données vers Hadoop, Stockage Blob Azure ou Azure Data Lake Store.** Archivez les données sur Hadoop, Stockage Blob Azure ou Azure Data Lake Store. Vous bénéficierez ainsi d’un mode de stockage économique et vos données resteront en ligne et donc facilement accessibles.

- **Intégration à des outils d’aide à la décision (BI).** Utilisez PolyBase avec la pile d’analyse et d’aide à la décision de Microsoft, ou n’importe quel outil tiers compatible avec SQL Server.

## <a name="performance"></a>Performances

- **Déléguer les calculs à Hadoop.** En fonction des coûts, l’optimiseur de requête prend la décision de déléguer les calculs à Hadoop, si cela permet d’améliorer les performances des requêtes.  L’optimiseur de requête utilise des statistiques sur des tables externes pour prendre la décision basée sur les coûts. La délégation des calculs a pour effet de créer des tâches MapReduce et de tirer parti des ressources de calcul distribuées de Hadoop.

- **Mise à l’échelle des ressources de calcul.** Pour améliorer les performances des requêtes, vous pouvez utiliser des [groupes de scale-out PolyBase](../../relational-databases/polybase/polybase-scale-out-groups.md)SQL Server. Cela autorise un transfert de données en parallèle entre les instances SQL Server et les nœuds Hadoop, et cela ajoute des ressources de calcul qui permettent d’exploiter les données externes.

## <a name="next-steps"></a>Étapes suivantes

Avant d’utiliser PolyBase, vous devez [installer la fonctionnalité PolyBase](polybase-installation.md). Consultez ensuite les guides de configuration suivants, en fonction de votre source de données :

- [Hadoop](polybase-configure-hadoop.md)
- [Stockage Blob Azure](polybase-configure-azure-blob-storage.md)
- [SQL Server](polybase-configure-sql-server.md)
- [Oracle](polybase-configure-oracle.md)
- [Teradata](polybase-configure-teradata.md)
- [MongoDB](polybase-configure-mongodb.md)
- [Types génériques ODBC](polybase-configure-odbc-generic.md)