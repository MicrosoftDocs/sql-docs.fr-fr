---
description: Capture de données modifiées-sys.dm_cdc_log_scan_sessions
title: sys.dm_cdc_log_scan_sessions (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_cdc_log_scan_sessions
- dm_cdc_log_scan_sessions_TSQL
- sys.dm_cdc_log_scan_sessions_TSQL
- sys.dm_cdc_log_scan_sessions
dev_langs:
- TSQL
helpviewer_keywords:
- change data capture [SQL Server], log scan reporting
- sys.dm_cdc_log_scan_sessions dynamic management view
ms.assetid: d337e9d0-78b1-4a07-8820-2027d0b9f87c
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: e8f69ed4f8b1ca90bc6de468b4136c6c36687cfc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99148199"
---
# <a name="change-data-capture---sysdm_cdc_log_scan_sessions"></a>Capture de données modifiées-sys.dm_cdc_log_scan_sessions
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Retourne une ligne pour chaque session d'analyse du journal dans la base de données actuelle. La dernière ligne retournée représente la session active. Vous pouvez utiliser cette vue pour retourner des informations d'état sur la session d'analyse du journal actuelle, ou des informations de synthèse sur toutes les sessions depuis le dernier démarrage de l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
   
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**session_id**|**int**|ID de la session.<br /><br /> 0 = les données retournées dans cette ligne sont un agrégat de toutes les sessions depuis le dernier démarrage de l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**heure-début**|**datetime**|Heure de début de la session.<br /><br /> Lorsque **session_id** = 0, le temps de collecte des données agrégées a commencé.|  
|**heure-fin**|**datetime**|Heure à laquelle la session s’est terminée.<br /><br /> NULL = la session est active.<br /><br /> Lorsque **session_id** = 0, heure de fin de la dernière session.|  
|**duration**|**bigint**|Durée (en secondes) de la session.<br /><br /> 0 = la session ne contient pas de transactions de capture des données modifiées.<br /><br /> Lorsque **session_id** = 0, somme de la durée (en secondes) de toutes les sessions avec des transactions de capture de données modifiées.|  
|**scan_phase**|**nvarchar(200)**|Phase actuelle de la session. Voici les valeurs possibles et leurs descriptions :<br /><br /> 1 : lecture de la configuration<br />2 : première analyse, création de la table de hachage<br />3 : deuxième analyse<br />4 : deuxième analyse<br />5 : deuxième analyse<br />6 : contrôle de version de schéma<br />7 : dernière analyse<br />8 : terminé<br /><br /> Quand **session_id** = 0, cette valeur est toujours « Aggregate ».|  
|**error_count**|**int**|Nombre d'erreurs rencontrées.<br /><br /> Lorsque **session_id** = 0, nombre total d’erreurs dans toutes les sessions.|  
|**start_lsn**|**nvarchar (23)**|Numéro séquentiel dans le journal de démarrage pour la session.<br /><br /> Lorsque **session_id** = 0, numéro LSN de départ pour la dernière session.|  
|**current_lsn**|**nvarchar (23)**|Numéro séquentiel dans le journal actuel qui est analysé.<br /><br /> Lorsque **session_id** = 0, le LSN actuel est 0.|  
|**end_lsn**|**nvarchar (23)**|Numéro séquentiel dans le journal de fin pour la session.<br /><br /> NULL = la session est active.<br /><br /> Lorsque **session_id** = 0, LSN de fin de la dernière session.|  
|**tran_count**|**bigint**|Nombre de transactions de capture des données modifiées traitées. Ce compteur est rempli au cours de la phase 2.<br /><br /> Lorsque **session_id** = 0, nombre de transactions traitées dans toutes les sessions.|  
|**last_commit_lsn**|**nvarchar (23)**|Numéro séquentiel dans le journal du dernier enregistrement du journal de validation traité.<br /><br /> Lorsque **session_id** = 0, le numéro LSN du dernier enregistrement du journal de validation pour toute session.|  
|**last_commit_time**|**datetime**|Heure de traitement du dernier enregistrement du journal de validation.<br /><br /> Lorsque **session_id** = 0, heure du dernier enregistrement du journal de validation pour toute session.|  
|**log_record_count**|**bigint**|Nombre d'enregistrements du journal analysés.<br /><br /> Lorsque **session_id** = 0, nombre d’enregistrements analysés pour toutes les sessions.|  
|**schema_change_count**|**int**|Nombre d'opérations de langage de définition de données (DDL) détectées. Ce compteur est rempli lors de la phase 6.<br /><br /> Lorsque **session_id** = 0, nombre d’opérations DDL traitées dans toutes les sessions.|  
|**command_count**|**bigint**|Nombre de commandes traitées.<br /><br /> Lorsque **session_id** = 0, le nombre de commandes traitées dans toutes les sessions.|  
|**first_begin_cdc_lsn**|**nvarchar (23)**|Premier numéro séquentiel dans le journal qui contenait des transactions de capture des données modifiées.<br /><br /> Lorsque **session_id** = 0, le premier LSN qui contenait des transactions de capture de données modifiées.|  
|**last_commit_cdc_lsn**|**nvarchar (23)**|Numéro séquentiel dans le journal du dernier enregistrement du journal de validation qui contenait des transactions de capture des données modifiées.<br /><br /> Lorsque **session_id** = 0, numéro LSN du dernier enregistrement du journal de validation pour toute session qui contenait des transactions de capture de données modifiées|  
|**last_commit_cdc_time**|**datetime**|Heure de traitement du dernier enregistrement du journal de validation qui contenait des transactions de capture des données modifiées.<br /><br /> Lorsque **session_id** = 0, heure du dernier enregistrement du journal de validation pour toute session qui contenait des transactions de capture de données modifiées.|  
|**latence**|**int**|Différence, en secondes, entre **end_time** et **last_commit_cdc_time** dans la session. Ce compteur est rempli à la fin de la phase 7.<br /><br /> Lorsque **session_id** = 0, il s’agit de la dernière valeur de latence différente de zéro enregistrée par une session.|  
|**empty_scan_count**|**int**|Nombre de sessions consécutives qui ne contenaient aucune transaction de capture des données modifiées.|  
|**failed_sessions_count**|**int**|Nombre de sessions qui ont échoué.|  
  
## <a name="remarks"></a>Notes  
 Les valeurs dans cette vue de gestion dynamique sont réinitialisées à chaque démarrage de l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l’autorisation VIEW DATABASE STATE pour interroger la vue de gestion dynamique **sys.dm_cdc_log_scan_sessions** . Pour plus d’informations sur les autorisations sur les vues de gestion dynamique, consultez [fonctions et vues de gestion dynamique &#40;&#41;Transact-SQL ](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md).  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant retourne des informations pour la session la plus active.  
  
```  
USE AdventureWorks2012;  
GO  
SELECT session_id, start_time, end_time, duration, scan_phase,  
    error_count, start_lsn, current_lsn, end_lsn, tran_count,  
    last_commit_lsn, last_commit_time, log_record_count, schema_change_count,  
    command_count, first_begin_cdc_lsn, last_commit_cdc_lsn,   
    last_commit_cdc_time, latency, empty_scan_count, failed_sessions_count  
FROM sys.dm_cdc_log_scan_sessions  
WHERE session_id = (SELECT MAX(b.session_id) FROM sys.dm_cdc_log_scan_sessions AS b);  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sys.dm_cdc_errors &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/change-data-capture-sys-dm-cdc-errors.md)  
  
  

