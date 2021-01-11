---
title: Exécution d’une commande
description: Décrit l’objet `Command` Fournisseur de données Microsoft SqlClient pour SQL Server et comment l’utiliser pour exécuter des requêtes et des commandes sur une source de données.
ms.date: 11/25/2020
dev_langs:
- csharp
ms.assetid: 40494916-c25a-4cb8-8f7c-fcb8d378464e
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 772ecef470b69a34e4bd4b0eba2ca60255657eaf
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771421"
---
# <a name="executing-a-command"></a>Exécution d’une commande

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Le Fournisseur de données Microsoft SqlClient pour SQL Server a le projet <xref:Microsoft.Data.SqlClient.SqlCommand> qui hérite de <xref:System.Data.Common.DbCommand>. Cet objet expose des méthodes pour exécuter des commandes en fonction du type de commande et de la valeur de retour souhaitée, comme décrit dans le tableau suivant.

|Commande|Valeur renvoyée|  
|-------------|------------------|  
|`ExecuteReader`|Retourne un objet `DataReader`.|  
|`ExecuteScalar`|Retourne une valeur scalaire unique.|  
|`ExecuteNonQuery`|Exécute une commande qui ne retourne aucune ligne.|  
|`ExecuteXMLReader`|Retourne un <xref:System.Xml.XmlReader>. Disponible pour un objet `SqlCommand` uniquement.|

Chaque objet de commande fortement typé prend également en charge une énumération <xref:System.Data.CommandType> qui spécifie la manière dont une chaîne de commande est interprétée, comme cela est décrit dans le tableau ci-dessous.

|CommandType|Description|
|-----------------|-----------------|  
|`Text`|Commande SQL qui définit les instructions à exécuter au niveau de la source de données.|  
|`StoredProcedure`|Nom de la procédure stockée. Vous pouvez utiliser la propriété `Parameters` d'une commande pour accéder aux paramètres d'entrée et de sortie et aux valeurs de retour, quelle que soit la méthode `Execute` appelée.|  
|`TableDirect`|Nom d'une table.|

> [!IMPORTANT]
> Lorsque vous utilisez `ExecuteReader`, les valeurs de retour et les paramètres de sortie ne seront pas accessibles tant que `DataReader` ne sera pas fermé.

## <a name="example"></a>Exemple

L'exemple de code ci-dessous montre comment créer un objet <xref:Microsoft.Data.SqlClient.SqlCommand> pour exécuter une procédure stockée en définissant ses propriétés. Un objet <xref:Microsoft.Data.SqlClient.SqlParameter> permet de spécifier le paramètre d'entrée de la procédure stockée. La commande est exécutée à l'aide de la méthode <xref:Microsoft.Data.SqlClient.SqlCommand.ExecuteReader%2A> et la sortie de <xref:Microsoft.Data.SqlClient.SqlDataReader> est affichée dans la fenêtre de console.

[!code-csharp[DataWorks SqlClient.StoredProcedure#1](~/../sqlclient/doc/samples/SqlCommand_StoredProcedure.cs#1)]

### <a name="troubleshooting-commands"></a>Commandes de dépannage

[!INCLUDE[appliesto-netfx-xxxx-xxxx-md](../../includes/appliesto-netfx-xxxx-xxxx-md.md)]

Le Fournisseur de données Microsoft SqlClient pour SQL Server ajoute des **compteurs de performances** pour vous permettre de détecter les problèmes intermittents liés aux exécutions de commandes qui ont échoué. Pour plus d’informations, consultez [Compteurs de performances dans SqlClient](performance-counters.md).

## <a name="see-also"></a>Voir aussi

- [Commandes et paramètres](commands-parameters.md)
- [DataAdapters et DataReaders](dataadapters-datareaders.md)
- [Microsoft ADO.NET pour SQL Server](microsoft-ado-net-sql-server.md)
