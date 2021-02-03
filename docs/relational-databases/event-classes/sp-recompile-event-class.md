---
description: SP:Recompile (classe d'événements)
title: SP:Recompile, classe d’événements | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- SP:Recompile event class
ms.assetid: 526c8eae-a07b-4d0e-b91e-8e537835d77d
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 3772823e0d7dba3ac83e152da2634b8841102520
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99161574"
---
# <a name="sprecompile-event-class"></a>SP:Recompile (classe d'événements)
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
  La classe d'événements SP:Recompile indique qu'une procédure stockée, un déclencheur ou une fonction définie par l'utilisateur a été compilé. Les recompilations signalées par cette classe d'événements se produisent au niveau de l'instruction.  
  
 Le meilleur moyen d’assurer la trace des recompilations de niveau instruction consiste à utiliser la classe d’événements SQL:StmtRecompile. La classe d’événements SP:Recompile a été déconseillée. Pour plus d’informations, consultez [Classe d’événements SQL:StmtRecompile](../../relational-databases/event-classes/sql-stmtrecompile-event-class.md).  
  
## <a name="sprecompile-event-class-data-columns"></a>Colonnes de la classe d'événements SP:Recompile  
  
|Nom de la colonne de données|**Type de données**|Description|ID de la colonne|Filtrable|  
|----------------------|-------------------|-----------------|---------------|----------------|  
|ApplicationName|**nvarchar**|Nom de l’application cliente qui a créé la connexion à une instance de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Cette colonne est remplie avec les valeurs passées par l'application plutôt que par le nom affiché du programme.|10|Oui|  
|ClientProcessID|**int**|ID affecté par l'ordinateur hôte au processus dans lequel s'exécute l'application cliente. Cette colonne de données est remplie si le client fournit l'ID du processus.|9|Oui|  
|DatabaseID|**int**|ID de la base de données dans laquelle la procédure stockée est en cours d'exécution. Déterminez la valeur pour une base de données à l'aide de la fonction DB_ID.|3|Oui|  
|nom_base_de_données|**nvarchar**|Nom de la base de données dans laquelle la procédure stockée est en cours d'exécution.|35|Oui|  
|EventClass|**int**|Type d’événement = 37.|27|Non|  
|EventSequence|**int**|Séquence d'un événement donné au sein de la demande.|51|Non|  
|EventSubClass|**int**|Type de sous-classe d'événements. Indique la raison de la recompilation.<br /><br /> 1 = Schéma modifié<br /><br /> 2 = Statistiques modifiées<br /><br /> 3 = Recompiler DNR<br /><br /> 4 = Option Set modifiée<br /><br /> 5 = Table temporaire modifiée<br /><br /> 6 = Ensemble de lignes distant modifié<br /><br /> 7 = Autorisations de recherche modifiées<br /><br /> 8 = Environnement de notification de requête modifié<br /><br /> 9 = Vue MPI modifiée<br /><br /> 10 = Options de curseur modifiées<br /><br /> 11 = Avec option de recompilation|21|Oui|  
|GroupID|**int**|ID du groupe de charges de travail où l'événement Trace SQL se déclenche.|66|Oui|  
|HostName|**nvarchar**|Nom de l'ordinateur sur lequel le client est exécuté. La colonne de données est remplie si le client fournit le nom de l'hôte. Pour déterminer le nom de l'hôte, utilisez la fonction HOST_NAME.|8|Oui|  
|IntegerData2|**int**|Décalage de fin de l'instruction dans la procédure stockée ou le traitement qui a provoqué la recompilation. Le décalage de fin est -1 si l'instruction est la dernière de son traitement.|55|Oui|  
|IsSystem|**int**|Indique si l'événement s'est produit sur un processus système ou sur un processus utilisateur. 1 = système, 0 = utilisateur.|60|Oui|  
|LoginName|**nvarchar**|Nom de la connexion de l'utilisateur (soit la connexion de sécurité [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , soit les informations d'identification de connexion [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows au format DOMAINE\nom_utilisateur).|11|Oui|  
|LoginSid|**image**|Numéro d'identification de sécurité (SID) de l'utilisateur connecté. Vous pouvez trouver ces informations dans l'affichage catalogue sys.server_principals. Chaque connexion possède un SID unique au niveau du serveur.|41|Oui|  
|NestLevel|**int**|Niveau d'imbrication de la procédure stockée.|29|Oui|  
|NTDomainName|**nvarchar**|Domaine Windows auquel appartient l'utilisateur.|7|Oui|  
|NTUserName|**nvarchar**|Nom d'utilisateur Windows.|6|Oui|  
|ObjectID|**int**|ID affecté par le système à la procédure stockée.|22|Oui|  
|ObjectName|**nvarchar**|Nom de l'objet qui a déclenché la recompilation.|34|Oui|  
|ObjectType|**int**|Valeur représentant le type de l'objet qui intervient dans l'événement. Pour plus d’informations, consultez [Colonne d’événements de trace ObjectType](../../relational-databases/event-classes/objecttype-trace-event-column.md).|28|Oui|  
|Offset|**int**|Décalage de début de l'instruction dans la procédure stockée ou le traitement qui a provoqué la recompilation.|61|Oui|  
|RequestID|**int**|ID de la demande contenant l'instruction.|49|Oui|  
|ServerName|**nvarchar**|Nom de l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tracée.|26|Non|  
|SessionLoginName|**nvarchar**|Nom de connexion de l'utilisateur à l'origine de la session. Par exemple, si vous vous connectez à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en utilisant le nom Connexion1 et que vous exécutez une instruction en tant que Connexion2, SessionLoginName affiche Connexion1 et LoginName, Connexion2. Cette colonne affiche à la fois les connexions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et Windows.|64|Oui|  
|SPID|**int**|ID de la session au cours de laquelle l'événement s'est produit.|12|Oui|  
|SqlHandle|**varbinary**|Hachage 64 bits basé sur le texte d'une requête ad hoc ou sur l'ID de base de données et d'objet d'un objet SQL. Cette valeur peut être passée à sys.dm_exec_sql_text pour récupérer le texte SQL associé.|63|Oui|  
|StartTime|**datetime**|Heure à laquelle a débuté l'événement, si elle est connue.|14|Oui|  
|TextData|**ntext**|Texte de l'instruction Transact-SQL qui a entraîné la recompilation de niveau instruction.|1|Oui|  
|TransactionID|**bigint**|ID affecté par le système à la transaction.|4|Oui|  
|XactSequence|**bigint**|Jeton utilisé pour décrire la transaction en cours.|50|Oui|  
  
## <a name="see-also"></a>Voir aussi  
 [sp_trace_setevent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)   
 [Classe d'événements SQL:StmtRecompile](../../relational-databases/event-classes/sql-stmtrecompile-event-class.md)  
  
  
