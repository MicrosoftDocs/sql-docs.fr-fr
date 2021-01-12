---
description: sys.filetables (Transact-SQL)
title: sys. filetables (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- filetables
- filetables_TSQL
- sys.filetables
- sys.filetables_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.filetables catalog view
ms.assetid: a740be59-cd52-4707-9ad2-5203669a63ac
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 990965f5f3af485437cd2e97d5eb952a53b157cf
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093149"
---
# <a name="sysfiletables-transact-sql"></a>sys.filetables (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Retourne une ligne pour chaque FileTable dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]. Pour plus d’informations sur les FileTables, consultez [FileTables &#40;SQL Server&#41;](../../relational-databases/blob/filetables-sql-server.md).    
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**object_id**||Numéro d'identification de l'objet. Unique dans une base de données.<br /><br /> Pour plus d’informations, [sys. objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md).|  
|**is_enabled**|**bit**|1 = FileTable se trouve dans l'état 'activé'.|  
|**directory_name**|**varchar(255)**|Nom du répertoire racine pour un FileTable.|  
|**filename_collation_id**||Identificateur de classement défini pour le FileTable|  
|**filename_collation_name**||Nom de classement défini pour le FileTable.|  
  
## <a name="see-also"></a>Voir aussi  
 [Gérer FileTables](../../relational-databases/blob/manage-filetables.md)   
 [FileTables &#40;SQL Server&#41;](../../relational-databases/blob/filetables-sql-server.md)  
  
  
