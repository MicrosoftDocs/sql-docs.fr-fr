---
title: Types de données SQL Server et ADO.NET
description: Décrit l’utilisation des types de données SQL Server et leur interaction avec les types de données .NET.
ms.date: 08/15/2019
ms.assetid: 81b43550-23e8-43bb-b460-7eb8ac825c33
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-kaywon
ms.openlocfilehash: 57e0cec178c407cda530e6699e51743094c57dca
ms.sourcegitcommit: c7f40918dc3ecdb0ed2ef5c237a3996cb4cd268d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91725590"
---
# <a name="sql-server-data-types-and-adonet"></a>Types de données SQL Server et ADO.NET

[!INCLUDE[Driver_ADONET_Download](../../../includes/driver_adonet_download.md)]

SQL Server et .NET sont basés sur des systèmes de types différents, ce qui peut entraîner une perte de données potentielle. Pour préserver l’intégrité des données, le fournisseur de données Microsoft SqlClient pour SQL Server (<xref:Microsoft.Data.SqlClient>) fournit des méthodes d’accesseur typées pour travailler avec des données SQL Server. Vous pouvez utiliser les énumérations dans les classes <xref:System.Data.SqlDbType> pour spécifier les types de données <xref:Microsoft.Data.SqlClient.SqlParameter>.  
  
SQL Server 2008 introduit de nouveaux types de données conçus pour répondre aux besoins professionnels pour travailler avec des données de date et d’heure, structurées, semi-structurées et non structurées. Ceux-ci sont décrits dans la Documentation en ligne de SQL Server 2008.  
  
Les types de données SQL Server qui peuvent être utilisés dans votre application dépendent de la version de SQL Server que vous utilisez. Pour plus d’informations, consultez [Types de données (Moteur de base de données)](/previous-versions/sql/sql-server-2008-r2/ms187594(v=sql.105)) dans la documentation en ligne de SQL Server.
  
## <a name="in-this-section"></a>Contenu de cette section  
[SqlTypes et le DataSet](sqltypes-dataset.md)  
Décrit la prise en charge de type pour `SqlTypes` dans le `DataSet`.  
  
[Traitement des valeurs Null](handle-null-values.md)  
Montre comment utiliser des valeurs null et une logique à trois valeurs.  
  
[Comparaison du GUID et des valeurs uniqueidentifier](compare-guid-uniqueidentifier-values.md)  
Montre comment utiliser des valeurs GUID et uniqueidentifier dans SQL Server et .NET.  
  
[Données de date et d’heure](date-time-data.md)  
Décrit comment utiliser les nouveaux types de données de date et d’heure introduits dans SQL Server 2008.  
  
[UDT volumineux](large-udts.md)  
Montre comment récupérer des données à partir des UDT de valeur élevée introduits dans SQL Server 2008.  
  
[Données XML dans SQL Server](xml-data-sql-server.md)  
Décrit comment utiliser des données XML extraites de SQL Server.  
  
## <a name="reference"></a>Informations de référence  
<xref:System.Data.DataSet>  
Décrit la classe `DataSet` et tous ses membres.  
  
<xref:System.Data.SqlTypes>  
Décrit l’espace de noms `SqlTypes` et tous ses membres.  
  
<xref:System.Data.SqlDbType>  
Décrit l’énumération `SqlDbType` et tous ses membres.  
  
<xref:System.Data.DbType>  
Décrit l’énumération `DbType` et tous ses membres.  
  
## <a name="next-steps"></a>Étapes suivantes
- [Paramètres table](table-valued-parameters.md)
- [Données binaires et à valeurs élevées SQL Server](sql-server-binary-large-value-data.md)