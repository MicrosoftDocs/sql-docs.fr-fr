---
title: Mappages de types de données
description: Décrit les types de données utilisés par le Fournisseur de données Microsoft SqlClient pour SQL Server.
ms.date: 11/13/2020
ms.assetid: d4afab94-ada6-4c77-a73c-41f17bae6b5a
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 808acdf89350f539f03f6fdc75f2f7a5ed5b7707
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771611"
---
# <a name="data-type-mappings-in-adonet"></a>Mappages des types de données dans ADO.NET

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

ADO.NET est basé sur le système de type commun, qui définit la manière dont les types sont déclarés, utilisés et managés dans le runtime. Il est constitué de types de valeur et de types de référence, qui dérivent tous du type de base <xref:System.Object>. Lorsque vous travaillez avec une source de données, le type de données est déduit du fournisseur de données s'il n'est pas explicitement spécifié. Par exemple, un objet <xref:System.Data.DataSet> est indépendant de toute source de données spécifique. Les données d'un `DataSet` sont extraites d'une source de données et les modifications y sont répercutées à l'aide d'un `DataAdapter`. Ce flux de programme signifie que lorsqu’un `DataAdapter` remplit un <xref:System.Data.DataTable> dans un `DataSet` avec des valeurs provenant d’une source de données, les types de données résultants des colonnes de `DataTable` sont des types .NET Framework, au lieu de types spécifiques au Fournisseur de données SqlClient Microsoft pour SQL Server utilisé pour la connexion à la source de données.

De même, lorsque `DataReader` retourne une valeur d’une source de données, la valeur résultante est stockée dans une variable locale qui a un type .NET Framework. Pour les opérations `Fill` de `DataAdapter` comme pour les méthodes `Get` de `DataReader`, le type .NET Framework est déduit de la valeur retournée du Fournisseur de données Microsoft SqlClient pour SQL Server.

Si vous ne souhaitez pas utiliser le type de données déduit, vous pouvez appeler les méthodes d’accesseur typé du `DataReader`, lorsque vous connaissez le type spécifique de la valeur retournée. Les méthodes d’accesseurs typées offrent un meilleur niveau de performance en retournant une valeur en tant que type de .NET Framework spécifique, ce qui élimine la nécessité d’une conversion de type supplémentaire.

> [!NOTE]
> Les valeurs null pour les types de données du Fournisseur de données Microsoft SqlClient pour SQL Server sont représentées par `DBNull.Value`.

## <a name="in-this-section"></a>Dans cette section

[Mappages de types de données SQL Server](sql-server-data-type-mappings.md) répertorie les mappages de types de données inférés et les méthodes d’accesseurs de données pour <xref:Microsoft.Data.SqlClient>.

[Nombres à virgule flottante](floating-point-numbers.md) décrit les problèmes que les développeurs rencontrent fréquemment lors de l’utilisation de nombres à virgule flottante.

## <a name="see-also"></a>Voir aussi

- [Types de données SQL Server et ADO.NET](./sql/sql-server-data-types.md)
- [Configuration des paramètres](configure-parameters.md)
- [Extraction des informations de schéma de base de données](retrieving-database-schema-information.md)
- [Microsoft ADO.NET pour SQL Server](microsoft-ado-net-sql-server.md)
