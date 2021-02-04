---
title: ISSCommandWithParameters (pilote OLE DB)
description: Découvrez comment l’interface ISSCommandWithParameters prend en charge XML SQL Server et les types définis par l’utilisateur dans OLE DB Driver pour SQL Server.
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- ISSCommandWithParameters (OLE DB)
apitype: COM
helpviewer_keywords:
- ISSCommandWithParameters interface
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 3a30947176efb820a7105f005e3d372f090d4190
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99172163"
---
# <a name="isscommandwithparameters-ole-db"></a>ISSCommandWithParameters (OLE DB)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  L’interface **ISSCommandWithParameters** expose la prise en charge de XML et des types définis par l’utilisateur (UDT) de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Il s’agit d’une interface facultative qui hérite de l’interface OLE DB de base **ICommandWithParameters**. En plus des trois méthodes héritées de **ICommandWithParameters** (**GetParameterInfo**, **MapParameterNames** et **SetParameterInfo**) **ISSCommandWithParameters** fournit deux nouvelles méthodes permettant de gérer des types de données spécifiques au serveur.  
  
> [!NOTE]  
>  L’interface **ISSCommandWithParameters** peut être utilisée quand des composants de service sont utilisés, mais ceux-ci n’utilisent pas cette interface.  
  
|Méthode|Description|  
|------------|-----------------|  
|[ISSCommandWithParameters::GetParameterProperties &#40;OLE DB&#41;](../../oledb/ole-db-interfaces/isscommandwithparameters-getparameterproperties-ole-db.md)|Retourne une structure de jeu de propriétés **SSPARAMPROPS** dans le tableau pour chaque paramètre UDT ou XML passé à la commande, mais rien n'est retourné pour d'autres types de paramètres.|  
|[ISSCommandWithParameters::SetParameterProperties &#40;OLE DB&#41;](../../oledb/ole-db-interfaces/isscommandwithparameters-setparameterproperties-ole-db.md)|Définit les propriétés de paramètre pour chaque paramètre par ordinal ou définit des propriétés de paramètre en bloc en spécifiant un tableau de structures **SSPARAMPROPS** .|  
  
## <a name="see-also"></a>Voir aussi  
 [Interfaces &#40;OLE DB&#41;](../../oledb/ole-db-interfaces/oledb-driver-for-sql-server-ole-db-interfaces.md)    
 [Utilisation de types de données XML](../../oledb/features/using-xml-data-types.md)   
 [Utilisation de types définis par l’utilisateur](../../oledb/features/using-user-defined-types.md)  
  
  
