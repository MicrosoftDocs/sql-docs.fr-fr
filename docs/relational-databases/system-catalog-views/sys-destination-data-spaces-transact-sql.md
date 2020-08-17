---
description: sys.destination_data_spaces (Transact-SQL)
title: sys. destination_data_spaces (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.destination_data_spaces
- destination_data_spaces_TSQL
- destination_data_spaces
- sys.destination_data_spaces_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.destination_data_spaces catalog view
ms.assetid: 92df932b-ad5c-43f8-81f4-b158823ab189
author: CarlRabeler
ms.author: carlrab
ms.openlocfilehash: 03540e6fdf5f1216d61a43af011e89b5e3d777c3
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88378355"
---
# <a name="sysdestination_data_spaces-transact-sql"></a>sys.destination_data_spaces (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contient une ligne par destination d'espace de données d'un schéma de partition.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**partition_scheme_id**|**int**|ID du schéma de partition dont l'opération de partitionnement est réalisée à destination de l'espace de données.|  
|**destination_id**|**int**|ID (ordinal à partir de 1) du mappage de destination, unique dans le schéma de partition.|  
|**data_space_id**|**int**|ID de l'espace de données avec lequel sont mappées les données pour la destination de ce schéma.|  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle **public** . Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)  
  
  
