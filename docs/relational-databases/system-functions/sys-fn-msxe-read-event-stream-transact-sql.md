---
description: sys.fn_MSxe_read_event_stream (Transact-SQL)
title: sys.fn_MSxe_read_event_stream (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- fn_MSxe_read_event_stream_TSQL
- sys.fn_MSxe_read_event_stream_TSQL
- sys.fn_MSxe_read_event_stream
- fn_MSxe_read_event_stream
dev_langs:
- TSQL
helpviewer_keywords:
- sys.fn_MSxe_read_event_stream
- fn_MSxe_read_event_stream
ms.assetid: 5edb1162-625a-41e0-8ec9-1edc8ab9a74a
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: dc8eff18237f4796d36812cf0afee363effcbf99
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201931"
---
# <a name="sysfn_msxe_read_event_stream-transact-sql"></a>sys.fn_MSxe_read_event_stream (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Les événements étendus fournissent une fonction table (TVF) en vue d'un usage interne par l'interface utilisateur Événements étendus. La table ne fournit pas de données utilisables par le client.  
  
 Pour consulter les données d'événement, utilisez ce qui suit :  
  
-   Nouvelle interface utilisateur de session Événements étendus.  
  
-   fn_xe_file_target_read_file TVF. Pour plus d’informations, consultez [sys.fn_xe_file_target_read_file &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-xe-file-target-read-file-transact-sql.md).  
  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sys.fn_MSxe_read_event_stream ( session_name)  
```  
  
## <a name="arguments"></a>Arguments  
 *Session_name*  
 Nom d'une session qui s'exécute sur le serveur.  
  
## <a name="table-returned"></a>Table retournée  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|type|**Entier (4)**|Le type d'événement. N'accepte pas la valeur NULL.|  
|data|**Image (16)**|Données d'image d'événement. Autorise la valeur NULL.|  
  
## <a name="see-also"></a>Voir aussi  
 [Vues de gestion dynamique des événements étendus](../../relational-databases/system-dynamic-management-views/extended-events-dynamic-management-views.md)   
 [Affichages catalogue des événements étendus &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/extended-events-catalog-views-transact-sql.md)   
 [Événements étendus](../../relational-databases/extended-events/extended-events.md)  
  
  
