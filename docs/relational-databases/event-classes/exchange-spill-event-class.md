---
description: Exchange Spill (classe d'événements)
title: Exchange Spill, classe d’événements | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- Exchange Spill event class
ms.assetid: fb876cec-f88d-4975-b3fd-0fb85dc0a7ff
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: a60e22ef72b86bb8136ee5ad7709212fe7b394b3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160774"
---
# <a name="exchange-spill-event-class"></a>Exchange Spill (classe d'événements)
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
  La classe d’événements **Exchange Spill** indique que les tampons d’un plan de requête parallèle ont été temporairement écrits dans la base de données **tempdb** . Cet événement intervient rarement, seulement lorsqu'un plan de requête inclut de multiples plages d'analyses.  
  
 Normalement, la requête [!INCLUDE[tsql](../../includes/tsql-md.md)] qui génère de telles plages d'analyses comprend de nombreux opérateurs BETWEEN, chacun d'entre eux permettant de sélectionner une plage de lignes à partir d'une table ou d'un index. Vous pouvez également obtenir de multiples plages via des expressions telles que (T.a > 10 AND T.a < 20) OR (T.a > 100 AND T.a < 120). De plus, les plans de requête imposent que ces plages soient analysées de façon ordonnée, dans la mesure ou une clause ORDER BY est spécifiée sur T.a, ou parce qu'un itérateur du plan demande que les lignes qu'il consomme soient triées.  
  
 Lorsque le plan de requête d’une telle requête contient plusieurs opérateurs **Parallelism** , les tampons de communication utilisés par ces opérateurs **Parallelism** sont saturés et il se peut que la progression de l’exécution de la requête soit stoppée. Dans cette situation, l’un des opérateurs **Parallelism** écrit sa mémoire tampon de sortie dans la base de données **tempdb** (une opération appelée *vidage d’échange*), de manière à pouvoir consommer des lignes en provenance de certains de ses tampons d’entrée. Éventuellement, les lignes vidées sont retournées à l'opérateur demandeur lorsqu'il est prêt à les utiliser.  
  
 Très rarement, de multiples vidages d'échange peuvent intervenir au sein du même plan d'exécution, ce qui entraîne la requête à s'exécuter lentement. Si vous remarquez plus de cinq vidages d'échange au sein de la même exécution de plan de requête, contactez votre service d'assistance.  
  
 Les vidages d'échange sont parfois transitoires et peuvent disparaître lorsque la distribution des données change.  
  
 Il existe plusieurs façons d'éviter les vidages d'échange :  
  
-   Omettez la clause ORDER BY si vous n'avez pas besoin d'avoir le jeu de résultats ordonné.  
  
-   Si ORDER BY est requis, éliminez la colonne impliquée dans les multiples plages d'analyses (T.a dans l'exemple ci-dessus) de la clause ORDER BY.  
  
-   L'utilisation d'un indicateur d'index force l'optimiseur à choisir un chemin d'accès différent à la table en question.  
  
-   Réécrivez la requête pour produire un plan d'exécution de la requête différent.  
  
-   Forcez l'exécution séquentielle de la requête en ajoutant l'option MAXDOP = 1 à la fin de la requête ou de l'opération d'index. Pour plus d’informations, consultez [Configurer l’option de configuration du serveur Degré maximal de parallélisme](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md) et [Configurer des opérations d’index parallèles](../../relational-databases/indexes/configure-parallel-index-operations.md).  
  
> [!IMPORTANT]  
>  Pour déterminer si l’événement **Exchange Spill** survient lorsque l’optimiseur de requête génère un plan d’exécution, vous devez également collecter une classe d’événements Showplan dans la trace. Vous pouvez choisir n’importe quelle classe d’événements Showplan à l’exception des classes d’événements **Showplan Text** et **Showplan Text (Unencoded)** , qui ne retournent aucun ID de nœud. Les ID de nœud dans les plans d'exécution Showplan identifient chaque opération effectuée par l'optimiseur de requête lorsqu'il crée un plan d'exécution de requête. Ces opérations sont appelées opérateurs et chaque opérateur d'un Showplan a un ID de nœud. La colonne **ObjectID** des événements **Exchange Spill** correspond à l’ID de nœud dans les plans d’exécution pour vous permettre de déterminer quel opérateur ou quelle opération est responsable de l’erreur.  
  
## <a name="exchange-spill-event-class-data-columns"></a>Colonnes de données de la classe d'événements Vidage d'échange  
  
|Nom de la colonne de données|Type de données|Description|ID de la colonne|Filtrable|  
|----------------------|---------------|-----------------|---------------|----------------|  
|**ApplicationName**|**nvarchar**|Nom de l'application cliente qui a créé la connexion à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Cette colonne est remplie avec les valeurs passées par l'application plutôt que par le nom affiché du programme.|10|Oui|  
|**ClientProcessID**|**int**|ID affecté par l'ordinateur hôte au processus dans lequel s'exécute l'application cliente. La colonne de données est remplie si le client fournit l'ID du processus client.|9|Oui|  
|**DatabaseID**|**int**|ID de la base de données spécifiée par l'instruction USE *database* ou celui de la base de données par défaut si aucune instruction USE *database* n'a été spécifiée pour une instance donnée. [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] affiche le nom de la base de données si la colonne de données **ServerName** du serveur est capturée dans la trace et que le serveur est disponible. Déterminez la valeur pour une base de données à l'aide de la fonction DB_ID.|3|Oui|  
|**DatabaseName**|**nvarchar**|Nom de la base de données dans laquelle l'instruction de l'utilisateur est exécutée.|35|Oui|  
|**EventClass**|**int**|Type d’événement = 127.|27|Non|  
|**EventSequence**|**int**|Séquence d'un événement donné au sein de la demande.|51|Non|  
|**EventSubClass**|**int**|Type de sous-classe d'événements.<br /><br /> 1=Début du vidage<br /><br /> 2=Fin du vidage|21|Oui|  
|**GroupID**|**int**|ID du groupe de charges de travail où l'événement Trace SQL se déclenche.|66|Oui|  
|**HostName**|**nvarchar**|Nom de l'ordinateur sur lequel le client est exécuté. La colonne de données est remplie si le client fournit le nom de l'hôte. Pour déterminer le nom de l'hôte, utilisez la fonction HOST_NAME.|8|Oui|  
|**IsSystem**|**int**|Indique si l'événement s'est produit sur un processus système ou sur un processus utilisateur. 1 = système, 0 = utilisateur.|60|Oui|  
|**LoginName**|**nvarchar**|Nom de la connexion de l’utilisateur (soit la connexion de sécurité [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], soit les informations d’identification Windows au format *\<DOMAIN>\\<nom_utilisateur\>* ).|11|Oui|  
|**LoginSid**|**image**|Numéro d'identification de sécurité (SID) de l'utilisateur connecté. Vous pouvez trouver ces informations dans la table **syslogins** de la base de données **master** . Chaque connexion possède un SID unique au niveau du serveur.|41|Oui|  
|**NTDomainName**|**nvarchar**|Domaine Windows auquel appartient l'utilisateur.|7|Oui|  
|**NTUserName**|**nvarchar**|Nom d'utilisateur Windows.|6|Oui|  
|**ObjectID**|**int**|ID affecté à l'objet par le système. Correspond à l'ID de nœud dans les plans d'exécution Showplan.|22|Oui|  
|**RequestID**|**int**|ID de la demande contenant l'instruction.|49|Oui|  
|**ServerName**|**nvarchar**|Nom de l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tracée.|26|Non|  
|**SessionLoginName**|**nvarchar**|Nom de connexion de l'utilisateur à l'origine de la session. Par exemple, si vous vous connectez à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] au moyen de Login1 et que vous exécutez une commande en tant que Login2, **SessionLoginName** affiche Login1 et **LoginName** affiche Login2. Cette colonne affiche à la fois les connexions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et Windows.|64|Oui|  
|**SPID**|**int**|ID de la session au cours de laquelle l'événement s'est produit.|12|Oui|  
|**StartTime**|**datetime**|Heure à laquelle a débuté l'événement, si elle est connue.|14|Oui|  
|**TransactionID**|**bigint**|ID affecté par le système à la transaction.|4|Oui|  
|**XactSequence**|**bigint**|Jeton qui décrit la transaction en cours.|50|Oui|  
  
## <a name="see-also"></a>Voir aussi  
 [sp_trace_setevent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)   
 [Définir les options d'index](../../relational-databases/indexes/set-index-options.md)   
 [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md)  
  
  
