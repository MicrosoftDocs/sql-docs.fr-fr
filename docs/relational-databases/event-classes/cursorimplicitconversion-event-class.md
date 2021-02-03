---
description: Classe d'événements CursorImplicitConversion
title: CursorImplicitConversion, classe d’événements | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- CursorImplicitConversion event class
ms.assetid: 44d12e23-146a-42e6-bb38-1f2f6a035bad
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: c918fbc3b12593927d27638446c4456dc1219ef8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205385"
---
# <a name="cursorimplicitconversion-event-class"></a>Classe d'événements CursorImplicitConversion
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
   La classe d’événements **CursorImplicitConversion** décrit des événements de conversions implicites de curseur qui se produisent dans les interfaces de programmation d’applications (API) ou les curseurs [!INCLUDE[tsql](../../includes/tsql-md.md)]. Les événements de conversions implicites de curseur se produisent lorsque [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] exécute une instruction Transact-SQL non prise en charge par les curseurs côté serveur du type demandé. Le [!INCLUDE[ssDE](../../includes/ssde-md.md)] renvoie une erreur indiquant que le type du curseur a changé.  
  
 La classe d’événements **CursorImplicitConversion** s’intègre aux traces chargées d’enregistrer les performances des curseurs.  
  
 Lorsque cette classe d'événements est incluse dans une trace, le niveau de surcharge induite dépend de la fréquence à laquelle les curseurs nécessitant une conversion implicite sont utilisés sur la base de données au cours du traçage. Si les curseurs sont fortement utilisés, la trace peut dégrader notablement les performances.  
  
## <a name="cursorimplicitconversion-event-class-data-columns"></a>Colonnes de données de la classe d'événements CursorImplicitConversion  
  
|Nom de la colonne de données|Type de données|Description|ID de la colonne|Filtrable|  
|----------------------|---------------|-----------------|---------------|----------------|  
|**ApplicationName**|**nvarchar**|Nom de l'application cliente qui a créé la connexion à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Cette colonne est remplie avec les valeurs passées par l'application plutôt que par le nom affiché du programme.|10|Oui|  
|**BinaryData**|**image**|Type de curseur résultant. Les valeurs sont les suivantes :<br /><br /> 1 = Jeu de clés<br /><br /> 2 = Dynamique<br /><br /> 4 = Avant uniquement<br /><br /> 8 = Statique<br /><br /> 16 = Avance rapide|2|Oui|  
|**ClientProcessID**|**int**|ID affecté par l'ordinateur hôte au processus dans lequel s'exécute l'application cliente. La colonne de données est remplie si le client fournit l'ID du processus client.|9|Oui|  
|**DatabaseID**|**int**|ID de la base de données spécifiée par l’instruction USE *database* ou ID de la base de données par défaut si aucune instruction USE *database* n’a été émise pour une instance donnée. [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] affiche le nom de la base de données si la colonne de données **ServerName** du serveur est capturée dans la trace et que le serveur est disponible. Déterminez la valeur pour une base de données à l'aide de la fonction DB_ID.|3|Oui|  
|**DatabaseName**|**nvarchar**|Nom de la base de données dans laquelle l'instruction de l'utilisateur est exécutée.|35|Oui|  
|**EventClass**|**int**|Type d’événement enregistré = 76.|27|Non|  
|**EventSequence**|**int**|Séquence de la classe d’événements **CursorClose** dans le lot.|51|Non|  
|**GroupID**|**int**|ID du groupe de charges de travail où l'événement Trace SQL se déclenche.|66|Oui|  
|**Handle**|**int**|Handle de l'objet référencé dans l'événement.|33|Oui|  
|**HostName**|**nvarchar**|Nom de l'ordinateur sur lequel le client est exécuté. La colonne de données est remplie si le client fournit le nom de l'hôte. Pour déterminer le nom de l'hôte, utilisez la fonction HOST_NAME.|8|Oui|  
|**IntegerData**|**int**|Type de curseur demandé. Les valeurs sont les suivantes :<br /><br /> 1 = Jeu de clés<br /><br /> 2 = Dynamique<br /><br /> 4 = Avant uniquement<br /><br /> 8 = Statique<br /><br /> 16 = Avance rapide|25|Non|  
|**IsSystem**|**int**|Indique si l'événement s'est produit sur un processus système ou sur un processus utilisateur. 1 = système, 0 = utilisateur.|60|Oui|  
|**LoginName**|**nvarchar**|Nom de la connexion de l'utilisateur (soit la connexion de sécurité [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , soit les informations d'identification de connexion [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows au format DOMAINE\nom_utilisateur).|11|Oui|  
|**LoginSid**|**image**|Identificateur de sécurité (SID) de l'utilisateur connecté. Vous pouvez trouver ces informations dans l’affichage catalogue **sys.server_principals** . Chaque connexion possède un SID unique au niveau du serveur.|41|Oui|  
|**NTDomainName**|**nvarchar**|Domaine Windows auquel appartient l'utilisateur.|7|Oui|  
|**NTUserName**|**nvarchar**|Nom d'utilisateur Windows.|6|Oui|  
|**RequestID**|**int**|Identificateur de demande de la conversion implicite.|49|Oui|  
|**ServerName**|**nvarchar**|Nom de l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tracée.|26|Non|  
|**SessionLoginName**|**nvarchar**|Nom de connexion de l'utilisateur à l'origine de la session. Par exemple, si vous vous connectez à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] au moyen de Login1 et que vous exécutez une commande en tant que Login2, **SessionLoginName** affiche Login1 et **LoginName** affiche Login2. Cette colonne affiche à la fois les connexions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et Windows.|64|Oui|  
|**SPID**|**int**|ID de la session au cours de laquelle l'événement s'est produit.|12|Oui|  
|**StartTime**|**datetime**|Heure à laquelle a débuté l'événement, si elle est connue.|14|Oui|  
|**TransactionID**|**bigint**|ID affecté par le système à la transaction.|4|Oui|  
|**XactSequence**|**bigint**|Jeton qui décrit la transaction en cours.|50|Oui|  
  
## <a name="see-also"></a>Voir aussi  
 [sp_trace_setevent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)  
  
  
