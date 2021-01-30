---
description: sys.dm_exec_cursors (Transact-SQL)
title: sys.dm_exec_cursors (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_exec_cursors_TSQL
- dm_exec_cursors
- dm_exec_cursors_TSQL
- sys.dm_exec_cursors
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_cursors dynamic management function
ms.assetid: f520b63c-36af-40f1-bf71-6901d6331d3d
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 30e47feb0650e5e345c78f431f27bd0a953b31bc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99106400"
---
# <a name="sysdm_exec_cursors-transact-sql"></a>sys.dm_exec_cursors (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Retourne des informations sur les curseurs ouverts dans diverses bases de données.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
dm_exec_cursors (session_id | 0 )  
```  
  
## <a name="arguments"></a>Arguments  
 *session_id* | entre  
 ID de la session. Si *session_id* est spécifié, cette fonction retourne des informations sur les curseurs dans la session spécifiée.  
  
 Si 0 est spécifié, la fonction retourne des informations sur tous les curseurs dans toutes les sessions.  
  
## <a name="table-returned"></a>Table retournée  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**session_id**|**int**|ID de la session propriétaire de ce curseur.|  
|**cursor_id**|**int**|ID de l'objet curseur.|  
|**name**|**nvarchar (256)**|Nom du curseur, défini par l'utilisateur.|  
|**properties**|**nvarchar (256)**|Spécifie les propriétés du curseur. Les valeurs des propriétés suivantes sont concaténées pour former la valeur de cette colonne :<br />Interface déclaration<br />Type de curseur <br />Accès simultané au curseur<br />Étendue du curseur<br />Niveau d'imbrication du curseur<br /><br /> Par exemple, la valeur retournée dans cette colonne peut être « TSQL &#124; Dynamic &#124; OPTIMISTIC &#124; global (0) ».|  
|**sql_handle**|**varbinary(64)**|Descripteur du texte du traitement qui a déclaré le curseur.|  
|**statement_start_offset**|**int**|Nombre de caractères dans le traitement ou la procédure stockée en cours d'exécution où les instructions en cours d'exécution commencent. Peut être utilisé avec l' **sql_handle**, le **statement_end_offset** et la fonction de gestion dynamique [sys.dm_exec_sql_text](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md) pour récupérer l’instruction en cours d’exécution pour la demande.|  
|**statement_end_offset**|**int**|Nombre de caractères dans le traitement ou la procédure stockée en cours d'exécution où les instructions en cours d'exécution se terminent. Peut être utilisé avec l' **sql_handle**, le **statement_start_offset** et la fonction de gestion dynamique **sys.dm_exec_sql_text** pour récupérer l’instruction en cours d’exécution pour la demande.|  
|**plan_generation_num**|**bigint**|Numéro de séquence permettant de distinguer les instances de plans après une recompilation.|  
|**creation_time**|**datetime**|Heure de création du curseur.|  
|**is_open**|**bit**|Indique si le curseur est ouvert.|  
|**is_async_population**|**bit**|Spécifie si le thread d'arrière-plan remplit toujours de manière asynchrone un curseur KEYSET ou STATIC.|  
|**is_close_on_commit**|**bit**|Spécifie si le curseur a été déclaré à l'aide de CURSOR_CLOSE_ON_COMMIT.<br /><br /> 1 = Le curseur est fermé à la fin de la transaction.|  
|**fetch_status**|**int**|Retourne le dernier état d'extraction du curseur. Il s’agit de la dernière valeur @ retournée @FETCH_STATUS .|  
|**fetch_buffer_size**|**int**|Retourne des informations sur la taille du tampon d'extraction.<br /><br /> 1 = Curseurs Transact-SQL. Il est possible de définir une valeur supérieure pour les curseurs des API.|  
|**fetch_buffer_start**|**int**|Pour les curseurs FAST_FORWARD et DYNAMIC, retourne 0 si le curseur n'est pas ouvert ou s'il est positionné devant la première ligne. Sinon, retourne -1.<br /><br /> Pour les curseurs STATIC et KEYSET, retourne 0 si le curseur n'est pas ouvert et -1 si le curseur est positionné au-delà de la dernière ligne.<br /><br /> Sinon, retourne le numéro de la ligne où il est positionné.|  
|**ansi_position**|**int**|Position du curseur dans le tampon d'extraction.|  
|**worker_time**|**bigint**|Temps, en microsecondes, passés par les travaux qui exécutent ce curseur.|  
|**mention**|**bigint**|Nombre de lectures effectuées par le curseur.|  
|**Ecrit**|**bigint**|Nombre d'écritures effectuées par le curseur.|  
|**dormant_duration**|**bigint**|Millisecondes écoulées depuis le début de la dernière requête (ouverture ou extraction) sur ce curseur.|  
  
## <a name="permissions"></a>Autorisations  
 requièrent l'autorisation VIEW SERVER STATE sur le serveur.  
  
## <a name="remarks"></a>Notes  
 Le tableau ci-dessous fournit des informations sur l'interface de déclaration du curseur et inclut les valeurs possibles pour la colonne des propriétés.  
  
|Propriété|Description|  
|--------------|-----------------|  
|API|Le curseur a été déclaré en utilisant une des API d'accès aux données (ODBC, OLEDB).|  
|TSQL|Le curseur a été déclaré à l'aide de la syntaxe Transact-SQL DECLARE CURSOR.|  
  
 Le tableau ci-dessous fournit des informations sur le type du curseur et inclut les valeurs possibles pour la colonne des propriétés.  
  
|Type|Description|  
|----------|-----------------|  
|Keyset|Le curseur est déclaré comme Keyset.|  
|Dynamique|Le curseur est déclaré comme dynamique.|  
|Instantané|Le curseur est déclaré comme instantané ou statique.|  
|Fast_Forward|Le curseur est déclaré comme curseur avant.|  
  
 Le tableau ci-dessous fournit des informations sur l'accès concurrentiel au curseur et inclut les valeurs possibles pour la colonne des propriétés.  
  
|Accès concurrentiel|Description|  
|-----------------|-----------------|  
|Lecture seule|Le curseur est déclaré en lecture seule|  
|Scroll Locks|Le curseur utilise des défilements.|  
|Optimistic|Le curseur utilise l'accès concurrentiel optimiste.|  
  
 Le tableau ci-dessous fournit des informations sur l'étendue du curseur et inclut les valeurs possibles pour la colonne des propriétés.  
  
|Étendue|Description|  
|-----------|-----------------|  
|Local|Spécifie que l'étendue du curseur est locale pour le traitement d'instructions, la procédure stockée ou le déclencheur dans lequel il a été créé.|  
|Global|Spécifie que l'étendue du curseur est globale pour la connexion.|  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-detecting-old-cursors"></a>R. Détection des anciens curseurs  
 Cet exemple retourne des informations sur les curseurs ouverts sur le serveur pendant une durée supérieure aux 36 heures spécifiées.  
  
```  
SELECT creation_time, cursor_id, name, c.session_id, login_name   
FROM sys.dm_exec_cursors(0) AS c   
JOIN sys.dm_exec_sessions AS s ON c.session_id = s.session_id   
WHERE DATEDIFF(hh, c.creation_time, GETDATE()) > 36;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Fonctions et vues de gestion dynamique liées à l’exécution &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)   
 [sys.dm_exec_sessions &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sessions-transact-sql.md)  
  
  

