---
description: fn_syscollector_get_execution_details (Transact-SQL)
title: fn_syscollector_get_execution_details (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- fn_syscollector_get_execution_details_TSQL
- fn_syscollector_get_execution_details
dev_langs:
- TSQL
helpviewer_keywords:
- fn_syscollector_get_execution_details function
ms.assetid: d59ddf0c-72c0-4c57-bc83-aef260e4e105
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 40201cb1fed3938da075b3fd5e97380eb0dddfc5
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093845"
---
# <a name="fn_syscollector_get_execution_details-transact-sql"></a>fn_syscollector_get_execution_details (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Retourne une partie du journal [!INCLUDE[ssIS](../../includes/ssis-md.md)] (sysssislog) qui correspond au package_execution_id pour le package donné. La table contient une ligne pour chaque entrée du journal générée par les packages ou par leurs tâches et conteneurs lors de l'exécution.  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
fn_syscollector_get_execution_details ( log_id )  
```  
  
## <a name="arguments"></a>Arguments  
 *log_id*  
 Identificateur unique local pour le journal des exécutions. *log_id* est de **type int**.  
  
## <a name="table-returned"></a>Table retournée  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|id|**int**|Identificateur unique de l'entrée d'enregistrement.|  
|événement|**sysname**|Nom de l'événement qui a généré l'entrée d'enregistrement.|  
|computer|**nvarchar**|Ordinateur sur lequel le package s'exécutait lors de la création de l'entrée d'enregistrement.|  
|operator|**nvarchar**|Nom d'utilisateur de la personne ou de l'agent qui exécutait le package qui a généré l'entrée du journal.|  
|source|**nvarchar**|Nom de l'exécutable qui a généré l'entrée du journal.|  
|sourceid|**uniqueidentifier**|GUID de l'exécutable qui a généré l'entrée du journal.|  
|executionid|**uniqueidentifier**|GUID de l'instance d'exécution de l'exécutable qui a généré l'entrée du journal.|  
|starttime|**datetime**|Heure de début de l'exécution du package.|  
|endtime|**datetime**|Heure de fin d'exécution du package.|  
|datacode|**int**|Valeur entière qui identifie l'événement associé à l'entrée du journal. La valeur « 0 » indique que l'événement ne fournit aucun identificateur.|  
|databytes|**image**|Tableau d'octets qui identifie une valeur de retour.|  
|message|**nvarchar**|Description de l'événement et des informations associées à l'événement.|  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l’autorisation SELECT pour **dc_operator**.  
  
## <a name="see-also"></a>Voir aussi  
 [Activer la journalisation des packages dans SQL Server Data Tools](../../integration-services/performance/integration-services-ssis-logging.md#server_logging)   
 [Collecte de données](../../relational-databases/data-collection/data-collection.md)  
  
  
