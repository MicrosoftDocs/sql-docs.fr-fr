---
description: Auto Stats (classe d'événements)
title: Auto Stats, classe d’événements | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- Auto Stats event class
ms.assetid: cd613fce-01e1-4d8f-86cc-7ffbf0759f9e
author: stevestein
ms.author: sstein
ms.openlocfilehash: d84664b1ad1dbddc8ab1595c8cddf6ee1abd33b6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196333"
---
# <a name="auto-stats-event-class"></a>Auto Stats (classe d'événements)
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
  La classe d’événements **Auto Stats** indique qu’une mise à jour automatique des statistiques d’index et de colonne s’est produite.  **Auto Stats** se déclenche également quand des statistiques sont chargées pour une utilisation par l’optimiseur.
  
## <a name="auto-stats-event-class-data-columns"></a>Colonnes de la classe d'événements Auto Stats  
  
|Nom de la colonne de données|Type de données|Description|ID de la colonne|Filtrable|  
|----------------------|---------------|-----------------|---------------|----------------|  
|**ApplicationName**|**nvarchar**|Nom de l'application cliente qui a créé la connexion à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Cette colonne est remplie avec les valeurs passées par l'application plutôt que par le nom affiché du programme.|10|Oui|  
|**ClientProcessID**|**int**|ID affecté par l'ordinateur hôte au processus dans lequel s'exécute l'application cliente. La colonne de données est remplie si le client fournit l'ID du processus client.|9|Oui|  
|**DatabaseID**|**int**|Identificateur de la base de données spécifiée par l’instruction USE *base de données* ou celui de la base de données par défaut si aucune instruction USE *base de données* n’a été spécifiée pour une instance donnée. [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] affiche le nom de la base de données si la colonne de données **ServerName** du serveur est capturée dans la trace et que le serveur est disponible. Déterminez la valeur pour une base de données à l'aide de la fonction DB_ID.|3|Oui|  
|**DatabaseName**|**nvarchar**|Nom de la base de données dans laquelle l'instruction de l'utilisateur est exécutée.|35|Oui|  
|**Durée**|**bigint**|Temps (en microsecondes) pris par l'événement.|13|Oui|  
|**EndTime**|**datetime**|Heure de fin de l'événement.|15|Oui|  
|**Error**|**int**|Numéro d'erreur d'un événement donné. Il s’agit souvent du numéro d’erreur stocké dans l’affichage catalogue **sys.messages** .|31|Oui|  
|**EventClass**|**int**|Type d’événement = 58|27|Non|  
|**EventSequence**|**int**|Séquence d'un événement donné au sein de la demande.|51|Non|  
|**EventSubClass**|**int**|Type de sous-classe d'événements.<br /><br /> 1 : Statistiques créées/mises à jour de façon synchrone ; la colonne **TextData** indique de quelles statistiques il s’agit et la nature de l’opération exécutée, création ou mise à jour.<br /><br /> 2 : Mise à jour asynchrone des statistiques ; mise en file d’attente du travail.<br /><br /> 3 : Mise à jour asynchrone des statistiques ; démarrage du travail.<br /><br /> 4 : Mise à jour asynchrone des statistiques ; fin du travail.|21|Oui|  
|**GroupID**|**int**|ID du groupe de charges de travail où l'événement Trace SQL se déclenche.|66|Oui|  
|**HostName**|**nvarchar**|Nom de l'ordinateur sur lequel le client est exécuté. Cette colonne de données est remplie si le nom de l'hôte est fourni par le client. Pour déterminer le nom de l'hôte, utilisez la fonction HOST_NAME.|8|Oui|  
|**IndexID**|**int**|ID de l'entrée index/statistiques sur l'objet affecté par l'événement. Pour déterminer l’ID d’index d’un objet, utilisez la colonne **index_id** de l’affichage catalogue **sys.indexes** .|24|Oui|  
|**IntegerData**|**int**|Nombre de collections de statistiques mises à jour.|25|Oui|  
|**IntegerData2**|**int**|Numéro de séquence du travail.|55|Oui|  
|**IsSystem**|**int**|Indique si l'événement s'est produit sur un processus système ou sur un processus utilisateur. 1 = système, 0 = utilisateur.|60|Oui|  
|**LoginName**|**nvarchar**|Nom de la connexion de l'utilisateur (soit la connexion de sécurité [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , soit les informations d'identification Windows sous la forme DOMAINE\Nomutilisateur).|11|Oui|  
|**LoginSid**|**image**|Numéro d'identification de sécurité (SID) de l'utilisateur connecté. Vous pouvez trouver ces informations dans l’affichage catalogue **sys.server_principals** . Chaque connexion possède un SID unique au niveau du serveur.|41|Oui|  
|**NTDomainName**|**nvarchar**|Domaine Windows auquel appartient l'utilisateur.|7|Oui|  
|**NTUserName**|**nvarchar**|Nom d'utilisateur Windows.|6|Oui|  
|**ObjectID**|**int**|ID affecté à l'objet par le système.|22|Oui|  
|**RequestID**|**int**|ID de la demande contenant l'instruction.|49|Oui|  
|**ServerName**|**nvarchar**|Nom de l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tracée.|26|Non|  
|**SessionLoginName**|**nvarchar**|Nom de connexion de l'utilisateur à l'origine de la session. Par exemple, si vous vous connectez à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] au moyen de Login1 et que vous exécutez une commande en tant que Login2, **SessionLoginName** affiche Login1 et **LoginName** affiche Login2. Cette colonne affiche à la fois les connexions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et Windows.|64|Oui|  
|**SPID**|**int**|ID de la session au cours de laquelle l'événement s'est produit.|12|Oui|  
|**StartTime**|**datetime**|Heure à laquelle a débuté l'événement, si elle est connue.|14|Oui|  
|**Success**|**int**|0 = erreur<br /><br /> 1 = réussite.<br /><br /> 2 = ignoré en raison de l'accélération du serveur (MSDE).|23|Oui|  
|**TextData**|**ntext**|Le contenu de cette colonne dépend si les statistiques sont mises à jour de façon synchrone (**EventSubClass** 1) ou asynchrone (**EventSubClass** 2, 3 ou 4) :<br /><br /> 1 : Affiche les statistiques qui ont été mises à jour/créées.<br /><br /> 2, 3 ou 4 : NULL ; la colonne **IndexID** contient les ID d’index ou de statistiques des statistiques mises à jour.|1|Oui|  
|**TransactionID**|**bigint**|ID affecté par le système à la transaction.|4|Oui|  
|**Type**|**int**|Type du travail.|57|Oui|  
  
## <a name="see-also"></a>Voir aussi  
 [Événements étendus](../../relational-databases/extended-events/extended-events.md)   
 [sp_trace_setevent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)  
  
  
