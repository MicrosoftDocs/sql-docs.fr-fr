---
description: FT:Crawl Aborted, classe d'événements
title: FT:Crawl Aborted, classe d’événements | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- Crawl Aborted event class
ms.assetid: eead8ea6-5051-4689-ab30-4dfbfda01fb9
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 3f1eeab215405e25f1b9d31f846cfe7d20ffbdcb
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160741"
---
# <a name="ftcrawl-aborted-event-class"></a>FT:Crawl Aborted, classe d'événements
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
  La classe d’événements **FT:Crawl Aborted** indique qu’une exception s’est produite au cours d’une analyse de texte intégral. Cette erreur entraîne généralement l'arrêt de l'analyse de texte intégral. Pour plus de détails sur l'erreur, consultez le journal des événements [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows ou le journal d'analyse.  
  
## <a name="ftcrawl-aborted-event-class-data-columns"></a>Colonnes de données de la classe d'événements FT:Crawl Aborted  
  
|Nom de la colonne de données|Type de données|Description|ID de la colonne|Filtrable|  
|----------------------|---------------|-----------------|---------------|----------------|  
|**DatabaseID**|**int**|ID de la base de données dans laquelle l'analyse de texte intégral est exécutée. Déterminez la valeur pour une base de données à l'aide de la fonction DB_ID.|3|Oui|  
|**Error**|**int**|Numéro d'erreur d'un événement donné. Il s’agit généralement du numéro d’erreur stocké dans la table **sysmessages** .|31|Oui|  
|**EventClass**|**int**|Type d’événement = 157.|27|Non|  
|**EventSequence**|**int**|Séquence d'un événement donné au sein de la demande.|51|Non|  
|**IsSystem**|**int**|Indique si l'événement s'est produit sur un processus système ou sur un processus utilisateur. 1 = système, 0 = utilisateur.|60|Oui|  
|**ObjectID**|**int**|ID attribué par le système à l'objet sur lequel l'analyse de texte intégral était exécutée lorsque l'échec a eu lieu.|22|Oui|  
|**SessionLoginName**|**nvarchar**|Nom de connexion de l'utilisateur à l'origine de la session. Par exemple, si vous vous connectez à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] au moyen de Login1 et que vous exécutez une commande en tant que Login2, **SessionLoginName** affiche Login1 et **LoginName** affiche Login2. Cette colonne affiche à la fois les connexions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et Windows.|64|Oui|  
|**SPID**|**int**|ID de la session au cours de laquelle l'événement s'est produit.|12|Oui|  
|**StartTime**|**datetime**|Heure à laquelle a débuté l'événement, si elle est connue.|14|Oui|  
|**State**|**int**|Équivalent à un code d'état d'erreur.|30|Oui|  
|**TransactionID**|**bigint**|ID affecté par le système à la transaction.|4|Oui|  
  
## <a name="see-also"></a>Voir aussi  
 [sp_trace_setevent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)  
  
  
