---
title: Collections GetSchema et Schema
description: Décrit l’utilisation de la méthode GetSchema pour extraire et limiter les informations de schéma d’une base de données.
ms.date: 11/26/2020
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 113ff460017cb5fabddd4763b28294ef6f19954c
ms.sourcegitcommit: 2add15a99df7b85d271adb261523689984dfd134
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/10/2020
ms.locfileid: "97051335"
---
# <a name="get-schema-and-schema-collections"></a>Collections GetSchema et Schema

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Les classes **SqlConnection** du fournisseur de données Microsoft SqlClient pour SQL Server implémentent une méthode **GetSchema** qui sert à extraire les informations de schéma sur la base de données actuellement connectée, et les informations de schéma retournées par la méthode **GetSchema** se présentent sous la forme d’un <xref:System.Data.DataTable>. La méthode **GetSchema** est une méthode surchargée qui fournit des paramètres facultatifs pour spécifier la collection de schémas à retourner et limiter la quantité d’informations retournées.

## <a name="specifying-the-schema-collections"></a>Spécification des collections de schémas

Le premier paramètre facultatif de la méthode **GetSchema** est le nom de collection spécifié sous la forme d’une chaîne. Il y a deux types de collection de schémas : les collections de schémas communes qui sont communes à tous les fournisseurs et les collections de schémas spécifiques qui sont spécifiques à chaque fournisseur.  

Vous pouvez interroger le fournisseur de données Microsoft SqlClient pour SQL Server pour établir la liste des collections de schémas prises en charge en appelant la méthode **GetSchema** sans argument ou avec le nom de collection de schémas « MetaDataCollections ». Cette opération retourne un <xref:System.Data.DataTable> avec une liste des collections de schémas prises en charge, le nombre de restrictions qu’elles prennent en charge et le nombre d’éléments d’identification qu’elles utilisent.  

### <a name="retrieving-schema-collections-example"></a>Exemple d’extraction de collections de schémas

Les exemples suivants montrent comment utiliser la méthode <xref:Microsoft.Data.SqlClient.SqlConnection.GetSchema%2A> de la classe <xref:Microsoft.Data.SqlClient.SqlConnection> du fournisseur de données Microsoft SqlClient pour SQL Server pour extraire les informations de schéma sur toutes les tables contenues dans l’exemple de base de données **AdventureWorks** :  

[!code-csharp[SqlClient GetSchema#1](~/../sqlclient/doc/samples/SqlConnection_GetSchema_Tables.cs#1)]  

## <a name="see-also"></a>Voir aussi

- [Extraction des informations de schéma de base de données](retrieving-database-schema-information.md)
- [Microsoft ADO.NET pour SQL Server](microsoft-ado-net-sql-server.md)
