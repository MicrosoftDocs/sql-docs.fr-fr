---
description: sys.dm_exec_xml_handles (Transact-SQL)
title: sys.dm_exec_xml_handles (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_exec_xml_handles
- dm_exec_xml_handles_TSQL
- sys.dm_exec_xml_handles_TSQL
- sys.dm_exec_xml_handles
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_xml_handles dynamic management function
ms.assetid: a873ce0f-6955-417a-96a1-b2ef11a83633
author: pmasl
ms.author: pelopes
monikerRange: =azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 3e8529f7626ffa25d86cbb73c704198082e32bcf
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208058"
---
# <a name="sysdm_exec_xml_handles-transact-sql"></a>sys.dm_exec_xml_handles (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-asdw-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-asdw-xxx-md.md)]

  Retourne des informations sur les handles actifs qui ont été ouverts par **sp_xml_preparedocument**.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
dm_exec_xml_handles (session_id | 0 )  
```  
  
## <a name="arguments"></a>Arguments  
 *session_id* | entre  
 ID de la session. Si *session_id* est spécifié, cette fonction retourne des informations sur les handles XML dans la session spécifiée.  
  
 Si 0 est spécifié, la fonction renvoie des informations sur tous les handles XML dans toutes les sessions.  
  
## <a name="table-returned"></a>Table retournée  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**session_id**|**int**|ID de session de la session qui détient ce handle de document XML.|  
|**document_id**|**int**|ID de handle de document XML retourné par **sp_xml_preparedocument**.|  
|**namespace_document_id**|**int**|ID de handle interne utilisé pour le document d’espace de noms associé qui a été passé comme troisième paramètre à **sp_xml_preparedocument**. NULL s'il n'y a pas de document d'espace de noms.|  
|**sql_handle**|**varbinary(64)**|Handle du texte du code SQL où le handle a été défini.|  
|**statement_start_offset**|**int**|Nombre de caractères dans le lot en cours d’exécution ou la procédure stockée à laquelle l’appel de **sp_xml_preparedocument** se produit. Peut être utilisé avec l' **sql_handle**, le **statement_end_offset** et la fonction de gestion dynamique **sys.dm_exec_sql_text** pour récupérer l’instruction en cours d’exécution pour la demande.|  
|**statement_end_offset**|**int**|Nombre de caractères dans le lot en cours d’exécution ou la procédure stockée à laquelle l’appel de **sp_xml_preparedocument** se produit. Peut être utilisé avec l' **sql_handle**, le **statement_start_offset** et la fonction de gestion dynamique **sys.dm_exec_sql_text** pour récupérer l’instruction en cours d’exécution pour la demande.|  
|**creation_time**|**datetime**|Horodateur lors de l’appel de **sp_xml_preparedocument** .|  
|**original_document_size_bytes**|**bigint**|Taille du document XML non analysé, en octets.|  
|**original_namespace_document_size_bytes**|**bigint**|Taille du document d'espace de noms XML non analysé, en octets. NULL s'il n'y a pas de document d'espace de noms.|  
|**num_openxml_calls**|**bigint**|Nombre d'appels OPENXML avec ce handle de document.|  
|**row_count**|**bigint**|Nombre de lignes retournées par tous les appels OPENXML précédents pour ce handle de document.|  
|**dormant_duration_ms**|**bigint**|Nombre de millisecondes depuis le dernier appel OPENXML. Si OPENXML n’a pas été appelé, retourne des millisecondes depuis l’appel de **sp_xml_preparedocumen** t.|  
  
## <a name="remarks"></a>Notes  
 La durée de vie des **sql_handles** utilisées pour récupérer le texte SQL qui a exécuté un appel à **sp_xml_preparedocument** met en avant le plan mis en cache utilisé pour exécuter la requête. Si le texte de la requête n'est pas disponible dans le cache, les données ne peuvent pas être récupérées à l'aide des informations fournies dans le résultat de fonction. Cela peut se produire si vous exécutez de nombreux traitements de grande taille.  
  
## <a name="permissions"></a>Autorisations  
 Requiert l'autorisation VIEW SERVER STATE sur le serveur afin d'afficher toutes les sessions ou les ID de session qui ne sont pas détenus par l'appelant. Un appelant peut toujours voir les données de son propre ID de session actuelle.      
  
## <a name="examples"></a>Exemples  
 L'exemple suivant sélectionne tous les handles actives.  
  
```  
SELECT * FROM sys.dm_exec_xml_handles(0);  
```  
  
## <a name="see-also"></a>Voir aussi  
 <br>[Fonctions et vues de gestion dynamique (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)
 <br>[Fonctions et vues de gestion dynamique liées à l’exécution (Transact-SQL)](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)
 <br>[sp_xml_preparedocument (Transact-SQL)](../system-stored-procedures/sp-xml-preparedocument-transact-sql.md)
 <br>[sp_xml_removedocument (Transact-SQL)](../system-stored-procedures/sp-xml-removedocument-transact-sql.md)


 
  
  
