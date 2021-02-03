---
description: Hash Warning (classe d'événements)
title: Hash Warning, classe d’événements | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- Hash Warning event class
ms.assetid: cb93c620-4be9-4362-8bf0-af3f2048bdaf
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 3353313a9ee65ab1de34fa8c1e90517950eed0e9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199919"
---
# <a name="hash-warning-event-class"></a>Hash Warning (classe d'événements)
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
  La classe d'événements Hash Warning peut être utilisée pour analyser une récurrence de hachage ou une cessation de hachage (interruption de hachage) qui s'est produite lors d'une opération de hachage.  
  
 Une récurrence de hachage se produit lorsque l'entrée de construction n'est pas adaptée à la mémoire disponible, ce qui entraîne une fragmentation de l'entrée en plusieurs parties traitées séparément. Si l'une de ces parties n'est toujours pas adaptée à la mémoire disponible, elle est à nouveau fragmentée en sous-parties, également traitées séparément. Ce processus de fragmentation se poursuit jusqu'à ce que toutes les parties soient s'adaptées à la mémoire disponible ou jusqu'à ce que le niveau maximal de récursivité soit atteint (affiché dans la colonne de données IntegerData).  
  
 L'interruption de hachage a lieu lorsqu'une opération de hachage atteint son niveau maximal de récursivité et se décale à un plan auxiliaire pour traiter les données partitionnées restantes. L'interruption de hachage se produit généralement en raison de données rétrécies.  
  
 La récurrence de hachage et l'interruption de hachage entraînent une réduction des performances de votre serveur. Pour supprimer ou réduire la fréquence des récurrences et interruptions de hachage, effectuez l'une des opérations suivantes :  
  
-   Assurez-vous qu'il existe des statistiques sur les colonnes qui sont jointes ou groupées.  
  
-   S'il existe des statistiques sur les colonnes, mettez-les à jour.  
  
-   Utilisez un type de jointure différent. Utilisez, par exemple, une jointure MERGE ou LOOP à la place, le cas échéant.  
  
-   Augmentez la quantité de mémoire disponible de l'ordinateur. La récurrence ou l'interruption de hachage se produit lorsque la quantité de mémoire s'avère insuffisante pour traiter les requêtes en place et qu'elles doivent déborder du disque.  
  
 La création ou la mise à jour de statistiques sur la colonne impliquée dans la jointure s'avère le moyen le plus efficace pour réduire le nombre de récurrences ou d'interruptions de hachage se produisant.  
  
> [!NOTE]  
>  Les termes *jointure de hachage progressive* et *jointure de hachage récursive* servent aussi à décrire une interruption de hachage.  
  
> [!IMPORTANT]  
>  Pour déterminer l'emplacement où se produit l'événement Hash Warning lorsque l'optimiseur de requête génère un plan d'exécution, vous pouvez également recueillir une classe d'événements Showplan dans la trace. Vous pouvez choisir n'importe quelle classe d'événements Showplan à l'exception des classes d'événements Showplan Text and Showplan Text (non encodées), qui ne retournent aucun ID de nœud. Les ID de nœud dans les plans d'exécution Showplan identifient chaque opération effectuée par l'optimiseur de requête lorsqu'il crée un plan d'exécution de requête. Ces opérations sont appelées *opérateurs* et chaque opérateur d’un Showplan a un ID de nœud. La colonne ObjectID des événements Hash Warning correspond à l'ID de nœud dans Showplans pour vous permettre de déterminer quel opérateur ou quelle opération est responsable de l'erreur.  
  
## <a name="hash-warning-event-class-data-columns"></a>Colonnes de la classe d'événements Hash Warning  
  
|Nom de la colonne de données|Type de données|Description|ID de la colonne|Filtrable|  
|----------------------|---------------|-----------------|---------------|----------------|  
|ApplicationName|**nvarchar**|Nom de l'application cliente qui a créé la connexion à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Cette colonne est remplie des valeurs transmises par l'application et non pas du nom affiché du programme.|10|Oui|  
|ClientProcessID|**int**|ID affecté par l'ordinateur hôte au processus dans lequel s'exécute l'application cliente. Cette colonne de données est remplie si le client fournit l'ID du processus client.|9|Oui|  
|DatabaseID|**int**|ID de la base de données spécifiée par l'instruction USE *database* ou celui de la base de données par défaut si aucune instruction USE *database* n'a été spécifiée pour une instance donnée. [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] affiche le nom de la base de données si la colonne de données ServerName est capturée dans la trace et que le serveur est disponible. Déterminez la valeur pour une base de données à l'aide de la fonction DB_ID.|3|Oui|  
|nom_base_de_données|**nvarchar**|Nom de la base de données dans laquelle l'instruction de l'utilisateur est exécutée.|35|Oui|  
|EventClass|**int**|Type d’événement = 55.|27|Non|  
|EventSequence|**int**|Séquence d'un événement donné au sein de la demande.|51|Non|  
|EventSubClass|**int**|Type de sous-classe d'événements.<br /><br /> 0=Récurrence<br /><br /> 1=Interruption|21|Oui|  
|GroupID|**int**|ID du groupe de charges de travail où l'événement Trace SQL se déclenche.|66|Oui|  
|HostName|**nvarchar**|Nom de l'ordinateur sur lequel le client est exécuté. La colonne de données est remplie si le client fournit le nom de l'hôte. Pour déterminer le nom de l'hôte, utilisez la fonction HOST_NAME.|8|Oui|  
|IntegerData|**int**|Niveau de récurrence (récurrence du hachage uniquement).|25|Oui|  
|IsSystem|**int**|Indique si l'événement s'est produit sur un processus système ou sur un processus utilisateur. 1 = système, 0 = utilisateur.|60|Oui|  
|LoginName|**nvarchar**|Nom de la connexion de l’utilisateur (soit la connexion de sécurité [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], soit les informations d’identification Windows au format *\<DOMAIN>\\<nom_utilisateur\>* ).|11|Oui|  
|LoginSid|**image**|Numéro d'identification de sécurité (SID) de l'utilisateur connecté. Vous pouvez trouver ces informations dans l'affichage catalogue sys.server_principals. Chaque connexion possède un SID unique au niveau du serveur.|41|Oui|  
|NTDomainName|**nvarchar**|Domaine Windows auquel appartient l'utilisateur.|7|Oui|  
|NTUserName|**nvarchar**|Nom d'utilisateur Windows.|6|Oui|  
|ObjectID|**int**|ID de nœud de la racine de l'équipe de hachage impliquée dans la répartition. Correspond à l'ID de nœud dans les plans d'exécution Showplan.|22|Oui|  
|RequestID|**int**|ID de la demande qui contient l'instruction.|49|Oui|  
|ServerName|**nvarchar**|Nom de l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tracée.|26||  
|SessionLoginName|**nvarchar**|Nom de connexion de l'utilisateur à l'origine de la session. Par exemple, si vous vous connectez à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en utilisant le nom Connexion1 et que vous exécutez une instruction en tant que Connexion2, SessionLoginName affiche Connexion1 et LoginName, Connexion2. Cette colonne affiche à la fois les connexions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et Windows.|64|Oui|  
|SPID|**int**|ID de la session au cours de laquelle l'événement s'est produit.|12|Oui|  
|StartTime|**datetime**|Heure à laquelle a débuté l'événement, si elle est connue.|14|Oui|  
|TransactionID|**bigint**|ID affecté par le système à la transaction.|4|Oui|  
|XactSequence|**bigint**|Jeton qui décrit la transaction en cours.|50|Oui|  
  
## <a name="see-also"></a>Voir aussi  
 [sp_trace_setevent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)    
 [Joins](../../relational-databases/performance/joins.md)    
  
  
