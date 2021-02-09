---
description: catalog.event_messages
title: catalog.event_messages | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: a31a654f-31e9-4da1-aabf-182b07848e36
author: chugugrace
ms.author: chugu
ms.openlocfilehash: da96f49143a237741722c37cb9eda82754b24f52
ms.sourcegitcommit: 868c60aa3a76569faedd9b53187e6b3be4997cc9
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/08/2021
ms.locfileid: "99835348"
---
# <a name="catalogevent_messages"></a>catalog.event_messages 

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

  Affiche des informations sur les messages qui ont été enregistrés pendant les opérations.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|Event_message_ID|bigint|ID unique du message d'événement.|  
|Operation_id|bigint|Type d’opération.<br /><br /> Pour obtenir la liste des types d’opération, consultez [catalog.operations &#40;base de données SSISDB&#41;](../../integration-services/system-views/catalog-operations-ssisdb-database.md).|  
|Message_time|datetimeoffset(7)|Heure à laquelle le message a été créé.|  
|Message_type|SMALLINT|Type de message affiché. Pour plus d’informations sur les types de messages, consultez [catalog.operation_messages &#40;base de données SSISDB&#41;](../../integration-services/system-views/catalog-operation-messages-ssisdb-database.md).|  
|Message_source_type|SMALLINT|Source du message.|  
|message|nvarchar(max)|Texte du message.|  
|Extended_info_id|bigint|ID des informations supplémentaires relatives au message d’opération, trouvé dans la vue [catalog.extended_operation_info &#40;base de données SSISDB&#41;](../../integration-services/system-views/catalog-extended-operation-info-ssisdb-database.md).|  
|Package_name|nvarchar(260)|Nom du fichier de package.|  
|Event_name|nvarchar(1024)|Événement d'exécution associé au message.|  
|Message_source_name|nvarchar(4000)|Composant de package qui est la source du message.|  
|Message_source_id|nvarchar(38)|ID unique de la source du message.|  
|Subcomponent_name|nvarchar(4000)|Composant de flux de données qui est la source du message.<br /><br /> Lorsque des messages sont retournés par le moteur [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], SSIS.Pipeline s'affiche dans cette colonne.|  
|Package_path|nvarchar(max)|Chemin d'accès unique du composant dans le package.|  
|Execution_path|nvarchar(max)|Chemin d'accès complet du package parent jusqu'au point auquel le composant est exécuté.<br /><br /> Ce chemin d'accès capture également les itérations d'un composant.|  
|threadID|int|ID du thread qui s'exécute lorsque le message est enregistré.|  
|Message_code|int|Code associée au message.|  
  
## <a name="remarks"></a>Notes  
 Cette vue affiche les types de source de message suivants.  
  
|**message_source_type**|Description|  
|-------------------------------|-----------------|  
|10|API d'entrée, telles que T-SQL et les procédures stockées CLR|  
|20|Processus externe utilisé pour exécuter le package (ISServerExec.exe)|  
|30|Objets de niveau package|  
|40|Tâches de flux de contrôle|  
|50|Conteneurs de flux de contrôle|  
|60|Tâche de flux de données|  
  
## <a name="permissions"></a>Autorisations  
 Cette vue requiert l'une des autorisations suivantes :  
  
-   Autorisation READ sur l'opération  
  
-   Appartenance au rôle de base de données **ssis_admin**.  
  
-   Appartenance au rôle serveur **sysadmin**.  
  
## <a name="see-also"></a>Voir aussi  
 [catalog.event_message_context](../../integration-services/system-views/catalog-event-message-context.md)  
  
  
