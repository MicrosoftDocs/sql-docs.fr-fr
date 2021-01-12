---
description: sys.dm_filestream_file_io_handles (Transact-SQL)
title: sys.dm_filestream_file_io_handles (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_filestream_file_io_handles
- sys.dm_filestream_file_io_handles
- dm_filestream_file_io_handles_TSQL
- sys.dm_filestream_file_io_handles_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_filestream_file_io_handle catalog view
ms.assetid: e59632f4-3292-419f-9217-ca375749f1a5
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 4c92c42554123c96013b8d17721b3d0861154a35
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097665"
---
# <a name="sysdm_filestream_file_io_handles-transact-sql"></a>sys.dm_filestream_file_io_handles (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Affiche les descripteurs de fichiers connus par le propriétaire de l'espace de noms (NSO, Namespace Owner). Les handles FileStream qu’un client a obtenu à l’aide de **OpenSqlFilestream** sont affichés par cet affichage.  
  
|Colonne|Type|Description|  
|------------|----------|-----------------|  
|**handle_context_address**|**varbinary (8)**|Affiche l’adresse de la structure NSO interne associée au handle du client. Autorise la valeur NULL.|  
|**creation_request_id**|**int**|Affiche un champ de la requête d'E/S REQ_PRE_CREATE utilisé pour créer ce handle. N'accepte pas la valeur NULL.|  
|**creation_irp_id**|**int**|Affiche un champ de la requête d'E/S REQ_PRE_CREATE utilisé pour créer ce handle. N'accepte pas la valeur NULL|  
|**handle_id**|**int**|Affiche l'ID unique de ce handle qui est attribué par le pilote. N'accepte pas la valeur NULL.|  
|**creation_client_thread_id**|**varbinary (8)**|Affiche un champ de la requête d'E/S REQ_PRE_CREATE utilisé pour créer ce handle. Autorise la valeur NULL.|  
|**creation_client_process_id**|**varbinary (8)**|Affiche un champ de la requête d'E/S REQ_PRE_CREATE utilisé pour créer ce handle. Autorise la valeur NULL.|  
|**filestream_transaction_id**|**varbinary(128)**|Affiche l'ID de la transaction associée au handle donné. Il s’agit de la valeur retournée par la fonction **GET_FILESTREAM_TRANSACTION_CONTEXT** . Utilisez ce champ pour effectuer une jointure à la vue **sys.dm_filestream_file_io_requests** . Autorise la valeur NULL.|  
|**access_type**|**nvarchar(60)**|N'accepte pas la valeur NULL.|  
|**logical_path**|**nvarchar (256)**|Affiche le nom de chemin d'accès logique du fichier ouvert par ce handle. Il s’agit du même chemin d’accès que celui retourné par **. Méthode PathName** de **varbinary**(**Max**) FileStream. Autorise la valeur NULL.|  
|**physical_path**|**nvarchar (256)**|Affiche le nom de chemin d'accès NTFS réel du fichier. Il s’agit du même nom de chemin d’accès retourné par **. Méthode PhysicalPathName** de **varbinary**(**Max**) FileStream. Il est activé par l'indicateur de suivi 5556. Autorise la valeur NULL.|  
  
## <a name="permissions"></a>Autorisations  
 requièrent l'autorisation VIEW SERVER STATE sur le serveur.  
  
## <a name="see-also"></a>Voir aussi  
 [Vues de gestion dynamique FileStream et filetable &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/filestream-and-filetable-dynamic-management-views-transact-sql.md)  
  
  
