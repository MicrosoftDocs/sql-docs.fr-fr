---
description: Classe d'événements OLEDB DataRead
title: OLEDB DataRead, classe d’événements | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- OLEDB DataRead event class
ms.assetid: fb6869ba-3199-4e32-a650-60a5dda2571e
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: ad4603f5179755a8d6acac79cec2bdb888188be0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185074"
---
# <a name="oledb-dataread-event-class"></a>Classe d'événements OLEDB DataRead
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
  La classe d'événements OLEDB DataRead se produit lorsque [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] appelle un fournisseur OLE DB dans le cadre de requêtes distribuées ou de procédures stockées distantes. Incluez cette classe d'événements au sein des traces qui surveillent les moments où [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] effectue un appel de type demande de données à l'attention du fournisseur OLE DB.  
  
 Lorsque la classe OLEDB DataRead est incluse dans la trace, la surcharge système générée est élevée. Il vous est recommandé de limiter l'utilisation de cette classe d'événements aux traces qui analysent l'apparition de problèmes particuliers, durant un bref laps de temps.  
  
## <a name="oledb-dataread-event-class-data-columns"></a>Colonnes de données de la classe d'événements OLEDB DataRead  
  
|Nom de la colonne de données|Type de données|Description|ID de la colonne|Filtrable|  
|----------------------|---------------|-----------------|---------------|----------------|  
|ApplicationName|**nvarchar**|Nom de l'application cliente qui a créé la connexion à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Cette colonne est remplie avec les valeurs passées par l'application plutôt que par le nom affiché du programme.|10|Oui|  
|ClientProcessID|**int**|ID affecté par l'ordinateur hôte au processus dans lequel s'exécute l'application cliente. La colonne de données est remplie si le client fournit l'ID du processus client.|9|Oui|  
|DatabaseID|**int**|ID de la base de données spécifiée par l’instruction USE *database* ou ID de la base de données par défaut si aucune instruction USE *database* n’a été émise pour une instance donnée. [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] affiche le nom de la base de données si la colonne de données ServerName est capturée dans la trace et que le serveur est disponible. Déterminez la valeur pour une base de données à l'aide de la fonction DB_ID.|3|Oui|  
|nom_base_de_données|**nvarchar**|Nom de la base de données dans laquelle l'instruction de l'utilisateur est exécutée.|35|Oui|  
|Duration|**bigint**|Durée nécessaire pour exécuter l'événement d'appel OLE DB.|13|Non|  
|EndTime|**datetime**|Heure de fin de l'événement.|15|Oui|  
|Error|**int**|Numéro d'erreur d'un événement donné. Il s'agit souvent du numéro d'erreur stocké dans l'affichage catalogue sys.messages.|31|Oui|  
|EventClass|**int**|Type d’événement = 121.|27|Non|  
|EventSequence|**int**|Séquence de la classe d'événements OLE DB dans le lot.|51|Non|  
|EventSubClass|**int**|Type de sous-classe d'événements.<br /><br /> 0=Démarrage<br /><br /> 1=Terminée|21|Non|  
|GroupID|**int**|ID du groupe de charges de travail où l'événement Trace SQL se déclenche.|66|Oui|  
|HostName|**nvarchar**|Nom de l'ordinateur sur lequel le client est exécuté. La colonne de données est remplie si le client fournit le nom de l'hôte. Pour déterminer le nom de l'hôte, utilisez la fonction HOST_NAME.|8|Oui|  
|IsSystem|**int**|Indique si l'événement s'est produit sur un processus système ou sur un processus utilisateur. 1 = système, 0 = utilisateur.|60|Oui|  
|LinkedServerName|**nvarchar**|Nom du serveur lié.|45|Oui|  
|LoginName|**nvarchar**|Nom de la connexion de l'utilisateur (soit la connexion de sécurité [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , soit les informations d'identification de connexion [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows au format DOMAINE\nom_utilisateur).|11|Oui|  
|LoginSid|**image**|Identificateur de sécurité (SID) de l'utilisateur connecté. Vous pouvez trouver ces informations dans l'affichage catalogue sys.server_principals. Chaque connexion possède un SID unique au niveau du serveur.|41|Oui|  
|MethodName|**nvarchar**|Nom de la méthode appelante.|47|Non|  
|NTDomainName|**nvarchar**|Domaine Windows auquel appartient l'utilisateur.|7|Oui|  
|NTUserName|**nvarchar**|Nom d'utilisateur Windows.|6|Oui|  
|ProviderName|**nvarchar**|Nom du fournisseur OLE DB.|46|Oui|  
|RequestID|**int**|ID de la demande contenant l'instruction.|49|Oui|  
|SessionLoginName|**nvarchar**|Nom de connexion de l'utilisateur à l'origine de la session. Par exemple, si vous vous connectez à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en utilisant le nom Connexion1 et que vous exécutez une instruction en tant que Connexion2, SessionLoginName affiche Connexion1 et LoginName, Connexion2. Cette colonne affiche à la fois les connexions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et Windows.|64|Oui|  
|SPID|**Int**|ID de la session au cours de laquelle l'événement s'est produit.|12|Oui|  
|StartTime|**datetime**|Heure à laquelle a débuté l'événement, si elle est connue.|14|Oui|  
|TextData|**nvarchar**|Paramètres envoyés et reçus au sein de l'appel OLE DB.|1|Non|  
|TransactionID|**bigint**|ID affecté par le système à la transaction.|4|Oui|  
  
## <a name="see-also"></a>Voir aussi  
 [Événements étendus](../../relational-databases/extended-events/extended-events.md)   
 [sp_trace_setevent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)   
 [Objets OLE Automation dans Transact-SQL](../../relational-databases/stored-procedures/ole-automation-objects-in-transact-sql.md)  
  
  
