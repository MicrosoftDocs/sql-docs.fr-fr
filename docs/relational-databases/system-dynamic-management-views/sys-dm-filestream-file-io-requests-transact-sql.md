---
description: sys.dm_filestream_file_io_requests (Transact-SQL)
title: sys.dm_filestream_file_io_requests (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_filestream_file_io_requests
- dm_filestream_file_io_requests
- sys.dm_filestream_file_io_requests_TSQL
- dm_filestream_file_io_requests_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_filestream_file_io_requests catalog view
ms.assetid: d41e39a5-14d5-4f3d-a2e3-a822b454c1ed
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: e4ee0854a59ae465f8eff35dadb90ac33a499651
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186095"
---
# <a name="sysdm_filestream_file_io_requests-transact-sql"></a>sys.dm_filestream_file_io_requests (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Affiche une liste des requêtes d'E/S en cours de traitement par le propriétaire de l'espace de noms (NSO, Namespace Owner) à un moment donné.  
  
|Colonne|Type|Description|  
|------------|----------|-----------------|  
|**request_context_address**|**varbinary (8)**|Affiche l'adresse interne du bloc de mémoire NSO qui contient la requête d'E/S du pilote. N'accepte pas la valeur NULL.|  
|**current_spid**|**smallint**|Affiche l’ID du processus système (SPID) pour la connexion du SQL Server actuel. N'accepte pas la valeur NULL.|  
|**request_type**|**nvarchar(60)**|Affiche le type de paquet de requête d'E/S (IRP). Les types de demande possibles sont REQ_PRE_CREATE, REQ_POST_CREATE, REQ_RESOLVE_VOLUME, REQ_GET_VOLUME_INFO, REQ_GET_LOGICAL_NAME, REQ_GET_PHYSICAL_NAME, REQ_PRE_CLEANUP, REQ_POST_CLEANUP, REQ_CLOSE, REQ_FSCTL, REQ_QUERY_INFO, REQ_SET_INFO, REQ_ENUM_DIRECTORY, REQ_QUERY_SECURITY et REQ_SET_SECURITY. N'accepte pas la valeur NULL|  
|**request_state**|**nvarchar(60)**|Affiche l'état de la requête d'E/S dans NSO. Les valeurs possibles sont REQ_STATE_RECEIVED, REQ_STATE_INITIALIZED, REQ_STATE_ENQUEUED, REQ_STATE_PROCESSING, REQ_STATE_FORMATTING_RESPONSE, REQ_STATE_SENDING_RESPONSE, REQ_STATE_COMPLETING et REQ_STATE_COMPLETED. N'accepte pas la valeur NULL.|  
|**request_id**|**int**|Affiche l'ID de requête unique attribué par le pilote à cette requête. N'accepte pas la valeur NULL.|  
|**irp_id**|**int**|Affiche l'ID IRP unique. C'est utile pour identifier toutes les requêtes d'E/S associées à l'IRP donné. N'accepte pas la valeur NULL.|  
|**handle_id**|**int**|Indique l'ID de handle de l'espace de noms. Il s'agit d'un identificateur NSO spécifique, lequel est unique dans une instance. N'accepte pas la valeur NULL.|  
|**client_thread_id**|**varbinary (8)**|Affiche l’ID de thread de l’application cliente qui est à l’origine de la demande.<br /><br /> AVERTISSEMENT cela est explicite uniquement si l’application cliente s’exécute sur le même ordinateur que SQL Server. **\* \* \* \*** Lorsque l’application cliente s’exécute à distance, le **client_thread_id** affiche l’ID de thread d’un processus système qui fonctionne pour le compte du client distant.<br /><br /> Autorise la valeur NULL.|  
|**client_process_id**|**varbinary (8)**|Affiche l'ID de processus de l'application cliente si cette dernière s'exécute sur le même ordinateur que SQL Server. Pour un client distant, il s'agit de l'ID de processus système qui fonctionne pour le compte de l'application cliente. Autorise la valeur NULL.|  
|**handle_context_address**|**varbinary (8)**|Affiche l’adresse de la structure NSO interne associée au handle du client. Autorise la valeur NULL.|  
|**filestream_transaction_id**|**varbinary(128)**|Affiche l'ID de la transaction associée au handle donné, ainsi que toutes les requêtes associées à ce handle. Il s’agit de la valeur retournée par la fonction **GET_FILESTREAM_TRANSACTION_CONTEXT** . Autorise la valeur NULL.|  
  
## <a name="permissions"></a>Autorisations  
 requièrent l'autorisation VIEW SERVER STATE sur le serveur.  
  
## <a name="see-also"></a>Voir aussi  
 [Vues de gestion dynamique FileStream et filetable &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/filestream-and-filetable-dynamic-management-views-transact-sql.md)  
  
  
