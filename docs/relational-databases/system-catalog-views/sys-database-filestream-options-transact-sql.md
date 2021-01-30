---
description: sys.database_filestream_options (Transact-SQL)
title: sys.database_filestream_options (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- database_filestream_options
- sys.database_filestream_options_TSQL
- database_filestream_options_TSQL
- sys.database_filestream_options
dev_langs:
- TSQL
helpviewer_keywords:
- sys.database_filestream_options catalog view
ms.assetid: 3383c607-0bbc-456a-ab37-7230f4cbf0e9
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 748cf35251738123552efacdc4f696f6d3105c7f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195899"
---
# <a name="sysdatabase_filestream_options-transact-sql"></a>sys.database_filestream_options (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Affiche des informations relatives au niveau d'accès non transactionnel aux données FILESTREAM dans les FileTables activés. Contient une ligne pour chaque base de données dans l'instance SQL Server.  
  
 Pour plus d’informations sur les FileTables, consultez [FileTables &#40;SQL Server&#41;](../../relational-databases/blob/filetables-sql-server.md).  
  
  
|Colonne|Type|Description|  
|------------|----------|-----------------|  
|**database_id**|**int**|ID de la base de données. Cette valeur est unique dans l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**directory_name**|**nvarchar(255)**|Répertoire au niveau de la base de données pour tous les espaces de noms FileTable.|  
|**non_transacted_access**|**tinyint**|Niveau d'accès non transactionnel aux données FILESTREAM activées. Le niveau d’accès est défini par l’option NON_TRANSACTED_ACCESS de l’instruction **Create Database** ou **ALTER DATABASE** .<br /><br /> Les valeurs possibles pour ce paramètre sont les suivantes :<br /><br /> 0-non activé. Il s’agit de la valeur par défaut. Ce niveau est défini en fournissant la valeur **off** pour l’option **NON_TRANSACTED_ACCESS** .<br /><br /> 1-accès en lecture seule. Ce niveau est défini en fournissant la valeur **READ_ONLY** pour l’option **NON_TRANSACTED_ACCESS** .<br /><br /> 3-accès complet. Ce niveau est défini en fournissant la valeur **Full** pour l’option **NON_TRANSACTED_ACCESS** .<br /><br /> 5 - En transition vers l'état READONLY<br /><br /> 6-en transition vers OFF|  
|**non_transacted_access_desc**|**nvarchar(60)**|Description du niveau d’accès non transactionnel identifié dans non_transacted_access.<br /><br /> Les valeurs possibles pour ce paramètre sont les suivantes :<br /><br /> NONE : il s’agit de la valeur par défaut.<br /><br /> READ_ONLY<br /><br /> FULL<br /><br /> IN_TRANSITION_TO_READ_ONLY<br /><br /> IN_TRANSITION_TO_OFF|  
  
## <a name="see-also"></a>Voir aussi  
 [Activer les conditions préalables pour les FileTables](../../relational-databases/blob/enable-the-prerequisites-for-filetable.md)  
  
  
