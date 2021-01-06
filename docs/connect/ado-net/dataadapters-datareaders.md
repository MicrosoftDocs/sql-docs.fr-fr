---
title: DataAdapters et DataReaders
description: Découvrez le Fournisseur de données Microsoft SqlClient pour SQL Server DataReader, qui récupère les données d’une base de données, et DataAdapter, qui récupère les données d’une source de données et remplit un DataSet.
ms.date: 11/30/2020
ms.assetid: cc952ca2-ec19-46ab-9189-15174b52cb74
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 3a252ca44683c062e531081f0b625a5e2359d236
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771383"
---
# <a name="dataadapters-and-datareaders"></a>DataAdapters et DataReaders

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Vous pouvez utiliser le Fournisseur de données Microsoft SqlClient pour SQL Server **DataReader** pour extraire d’une base de données un flux de données en lecture seule avant uniquement. Les résultats sont retournés au fil de l’exécution de la requête et sont stockés dans la mémoire tampon réseau sur le client, jusqu’à ce que vous les demandiez en utilisant la méthode **Read** du **DataReader**. L’utilisation du **DataReader** peut augmenter les performances de l’application en extrayant les données dès qu’elles sont disponibles et en ne stockant (par défaut) qu’une seule ligne à la fois dans la mémoire, ce qui réduit la charge du système.

Un objet <xref:System.Data.Common.DataAdapter> est utilisé pour extraire les données d'une source de données et remplir les tables d'un <xref:System.Data.DataSet>. Le `DataAdapter` répercute également les modifications apportées au `DataSet` dans la source de données. Le `DataAdapter` utilise l’objet `Connection` du Fournisseur de données Microsoft SqlClient pour SQL Server pour se connecter à une source de données, et il utilise des objets `Command` pour récupérer des données et résoudre les modifications apportées à la source de données.

.NET a un <xref:System.Data.Common.DbDataReader> et un objet <xref:System.Data.Common.DbDataAdapter> : le Fournisseur de données Microsoft SqlClient pour SQL Server inclut un <xref:Microsoft.Data.SqlClient.SqlDataReader> et un objet <xref:Microsoft.Data.SqlClient.SqlDataAdapter>.

## <a name="in-this-section"></a>Dans cette section

[Récupération de données par un DataReader](retrieve-data-by-datareader.md)  
Décrit l’objet **DataReader** d’ADO.NET et son utilisation pour retourner un flux de résultats à partir d’une source de données.

[Remplissage d’un DataSet à partir d'un DataAdapter](populate-dataset-from-dataadapter.md)  
Explique comment remplir un `DataSet` avec des tables, des colonnes et des lignes au moyen d'un `DataAdapter`.

[Paramètres des DataAdapter](dataadapter-parameters.md)  
Décrit l'utilisation des paramètres avec les propriétés de commande d'un `DataAdapter`, y compris le mappage du contenu d'une colonne d'un `DataSet` à un paramètre de commande.

[Ajout de contraintes existantes à un DataSet](add-existing-constraints-to-dataset.md)  
Décrit comment ajouter des contraintes existantes à un `DataSet`.

[Mappages de DataAdapter, DataTable et DataColumn](dataadapter-datatable-datacolumn-mappings.md)  
Décrit comment configurer des `DataTableMappings` et des `ColumnMappings` pour un `DataAdapter`.

[Pagination sur un résultat de requête](paging-through-query-result.md)  
Propose un exemple de visualisation des résultats d'une requête sous forme de pages de données.

[Mise à jour de sources de données avec des DataAdapter](update-data-sources-with-dataadapters.md)  
Explique comment utiliser un `DataAdapter` pour répercuter les modifications apportées à un objet `DataSet` dans la base de données.

[Gestion d’événements DataAdapter](handle-dataadapter-events.md)  
Décrit les événements `DataAdapter` et comment les utiliser.

[Traitement d’opérations par lots à l'aide de DataAdapter](batch-operations-using-dataadapters.md)  
Décrit l'amélioration des performances de l'application en réduisant le nombre d'allers-retours vers SQL Server lors de l'application de mises à jour à partir du `DataSet`.

## <a name="see-also"></a>Voir aussi

- [Connexion à une source de données](connecting-to-data-source.md)
- [Commandes et paramètres](commands-parameters.md)
- [Transactions et accès simultané](transactions-and-concurrency.md)
- [Microsoft ADO.NET pour SQL Server](microsoft-ado-net-sql-server.md)
