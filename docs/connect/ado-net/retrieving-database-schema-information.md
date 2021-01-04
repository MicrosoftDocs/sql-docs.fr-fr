---
title: Extraction des informations de schéma de base de données
description: Découvrez comment utiliser le fournisseur de données Microsoft SqlClient pour SQL Server pour extraire des informations de schéma de base de données.
ms.date: 11/26/2020
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 932f4ff2109e3754fb97c79274a5ffb93b9494d3
ms.sourcegitcommit: 2add15a99df7b85d271adb261523689984dfd134
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/10/2020
ms.locfileid: "97051329"
---
# <a name="retrieving-database-schema-information"></a>Extraction des informations de schéma de base de données

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

L'obtention des informations de schéma à partir d'une base de données est effectuée avec le processus de découverte de schéma. La découverte de schéma permet aux applications de demander que des fournisseurs managés trouvent et retournent des informations sur le schéma de base de données, également appelées *métadonnées*, d’une base de données déterminée. Différents éléments de schéma de base de données tels que des tables, des colonnes et des procédures stockées, sont exposés à l’aide de collections de schémas. Chaque collection de schémas contient une série d'informations de schéma spécifiques au fournisseur utilisé.

Le fournisseur de données Microsoft SqlClient pour SQL Server implémente la méthode **GetSchema** dans la classe **SqlConnection**, et les informations de schéma retournées par la méthode **GetSchema** se présentent sous la forme d’un <xref:System.Data.DataTable>. La méthode **GetSchema** est une méthode surchargée qui fournit des paramètres facultatifs pour spécifier la collection de schémas à retourner et limiter la quantité d’informations retournées. Le fournisseur de données SqlClient fournit également une méthode **GetSchemaTable** qui retourne un objet DataTable décrivant les métadonnées de colonne de **SqlDataReader**.

## <a name="in-this-section"></a>Dans cette section

[Collections GetSchema et Schema](getschema-and-schema-collections.md)  
Décrit la méthode **GetSchema** et la façon dont elle peut être utilisée pour extraire et restreindre les informations de schéma d’une base de données.

[Restrictions de schéma](schema-restrictions.md)  
Décrit les restrictions de schéma qui peuvent être utilisées avec **GetSchema**. 

[Collections de schémas courants](common-schema-collections.md)  
Décrit toutes les collections de schémas courants prises en charge par tous les fournisseurs managés .NET.  
  
[Collections de schémas SQL Server](sql-server-schema-collections.md)  
Décrit les collections de schémas supplémentaires prises en charge par le fournisseur de données Microsoft SqlClient pour SQL Server. 

## <a name="reference"></a>Référence

<xref:System.Data.Common.DbConnection.GetSchema%2A>  
Décrit la méthode **GetSchema** de la classe <xref:System.Data.Common.DbConnection>.

<xref:Microsoft.Data.SqlClient.SqlConnection.GetSchema%2A>  
Décrit la méthode **GetSchema** de la classe <xref:Microsoft.Data.SqlClient.SqlConnection>.

<xref:System.Data.Common.DbDataReader.GetSchemaTable%2A>  
Décrit la méthode **GetSchemaTable** de la classe <xref:System.Data.Common.DbDataReader>. 

<xref:Microsoft.Data.SqlClient.SqlDataReader.GetSchemaTable%2A>  
Décrit la méthode **GetSchemaTable** de la classe <xref:Microsoft.Data.SqlClient.SqlDataReader>.

## <a name="see-also"></a>Voir aussi

- [Récupération et modification de données dans ADO.NET](retrieving-modifying-data.md)
- [Microsoft ADO.NET pour SQL Server](microsoft-ado-net-sql-server.md)
