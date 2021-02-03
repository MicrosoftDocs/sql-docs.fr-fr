---
description: SQL:StmtRecompile (classe d'événements)
title: SQL:StmtRecompile, classe d’événements | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- SQL:StmtRecompile event class
ms.assetid: 3a134751-3e93-4fe8-bf22-1e0561189293
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: cf9e300b77c76de9e6679144568a1556a041c194
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99161463"
---
# <a name="sqlstmtrecompile-event-class"></a>SQL:StmtRecompile (classe d'événements)
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
  La classe d’événements SQL:StmtRecompile indique des recompilations au niveau de l’instruction provoquées par tous les types de lots : procédures stockées, déclencheurs, lots ad hoc et requêtes. Les requêtes peuvent être envoyées avec sp_executesql, SQL dynamique, des méthodes Prepare, des méthodes Execute ou des interfaces analogues. La classe d’événements SQL:StmtRecompile doit être utilisée à la place de la classe d’événements SP:Recompile.  
  
## <a name="sqlstmtrecompile-event-class-data-columns"></a>Colonnes de la classe d'événements SQL:StmtRecompile  
  
|Nom de la colonne de données|Type de données|Description|ID de la colonne|Filtrable|  
|----------------------|---------------|-----------------|---------------|----------------|  
|ApplicationName|**nvarchar**|Nom de l’application cliente qui a créé la connexion à une instance de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Cette colonne est remplie des valeurs transmises par l'application et non pas du nom affiché du programme.|10|Oui|  
|ClientProcessID|**int**|ID affecté par l'ordinateur hôte au processus dans lequel s'exécute l'application cliente. Cette colonne de données est remplie si le client fournit l'ID du processus.|9|Oui|  
|DatabaseID|**int**|ID de la base de données dans laquelle la procédure stockée est en cours d'exécution. Déterminez la valeur pour une base de données à l'aide de la fonction DB_ID.|3|Oui|  
|nom_base_de_données|**nvarchar**|Nom de la base de données dans laquelle la procédure stockée est en cours d'exécution.|35|Oui|  
|EventSequence|**int**|Séquence d'un événement dans la demande.|51|Non|  
|EventSubClass|**int**|Décrit les causes de la recompilation :<br /><br /> 1 = Schéma modifié<br /><br /> 2 = Statistiques modifiées<br /><br /> 3 = Compilation différée<br /><br /> 4 = Option Set modifiée<br /><br /> 5 = Table temporaire modifiée<br /><br /> 6 = Ensemble de lignes distant modifié<br /><br /> 7 = Autorisations de recherche modifiées<br /><br /> 8 = Environnement de notification de requête modifié<br /><br /> 9 = Affichage partition modifié<br /><br /> 10 = Options de curseur modifiées<br /><br /> 11 = Option (recompilation) demandée|21|Oui|  
|GroupID|**int**|ID du groupe de charges de travail où l'événement Trace SQL se déclenche.|66|Oui|  
|HostName|**nvarchar**|Nom de l'ordinateur sur lequel s'exécute le client qui a envoyé cette instruction. La colonne de données est remplie si le client fournit le nom de l'hôte. Pour déterminer le nom de l'hôte, utilisez la fonction HOST_NAME.|8|Oui|  
|IntegerData2|**int**|Décalage de fin de l'instruction dans la procédure stockée ou le traitement qui a provoqué la recompilation. Le décalage de fin est -1 si l'instruction est la dernière de son traitement.|55|Oui|  
|IsSystem|**int**|Indique si l'événement s'est produit sur un processus système ou sur un processus utilisateur.<br /><br /> 1 = système<br /><br /> 0 = utilisateur|60|Oui|  
|LineNumber|**int**|Numéro de séquence de cette instruction dans le traitement, le cas échéant.|5|Oui|  
|LoginName|**nvarchar**|Nom de la connexion qui a envoyé ce traitement.|11|Oui|  
|LoginSid|**image**|Identificateur de sécurité (SID) de l'utilisateur actuellement connecté. Vous pouvez trouver ces informations dans l'affichage catalogue sys.server_principals. Chaque connexion possède un SID unique au niveau du serveur.|41|Oui|  
|NestLevel|**int**|Niveau d'imbrication de l'appel de la procédure stockée. Par exemple, la procédure stockée my_proc_a appelle my_proc_b. Dans ce cas, my_proc_a a un NestLevel égal à 1, my_proc_b a un NestLevel égal à 2.|29|Oui|  
|NTDomainName|**nvarchar**|Domaine Windows auquel appartient l'utilisateur.|7|Oui|  
|NTUserName|**nvarchar**|Nom d'utilisateur Windows de l'utilisateur connecté.|6|Oui|  
|ObjectID|**int**|L'identificateur, attribué par le système, de l'objet contenant l'instruction ayant provoqué la recompilation. Cet objet peut consister en une procédure stockée, une fonction définie par l'utilisateur ou un déclencheur. Pour les lots ad hoc ou les instructions SQL préparées, ObjectID et ObjectName retournent une valeur NULL.|22|Oui|  
|ObjectName|**nvarchar**|Nom de l'objet identifié par ObjectID.|34|Oui|  
|ObjectType|**int**|Valeur représentant le type de l'objet qui intervient dans l'événement. Pour plus d’informations, consultez [Colonne d’événements de trace ObjectType](../../relational-databases/event-classes/objecttype-trace-event-column.md).|28|Oui|  
|Offset|**int**|Décalage de début de l'instruction dans la procédure stockée ou le traitement qui a provoqué la recompilation.|61|Oui|  
|RequestID|**int**|ID de la demande contenant l'instruction.|49|Oui|  
|ServerName|**nvarchar**|Nom de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tracé.|26|Non|  
|SessionLoginName|**nvarchar**|Nom de connexion de l'utilisateur à l'origine de la session. Par exemple, si vous vous connectez à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en utilisant le nom Connexion1 et que vous exécutez une instruction en tant que Connexion2, SessionLoginName affiche Connexion1 et LoginName, Connexion2. Cette colonne affiche à la fois les connexions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et Windows.|64|Oui|  
|SPID|**int**|ID du processus serveur de la connexion.|12|Oui|  
|SqlHandle|**varbinary**|Hachage 64 bits basé sur le texte d'une requête ad hoc ou sur l'ID de base de données et d'objet d'un objet SQL. Cette valeur peut être passée à sys.dm_exec_sql_text pour récupérer le texte SQL associé.|63|Non|  
|StartTime|**datetime**|Heure à laquelle a débuté l'événement, si elle est connue.|14|Oui|  
|TextData|**ntext**|Texte de l'instruction Transact-SQL qui a été recompilée.|1|Oui|  
|TransactionID|**bigint**|ID affecté par le système à la transaction.|4|Oui|  
|XactSequence|**bigint**|Jeton qui décrit la transaction en cours.|50|Oui|  
  
## <a name="see-also"></a>Voir aussi  
 [Classe d'événements SP:Recompile](../../relational-databases/event-classes/sp-recompile-event-class.md)   
 [sp_trace_setevent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)  
  
  
