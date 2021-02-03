---
description: "Classe d'événements Progress Report: Online Index Operation"
title: "Classe d'événements Progress Report: Online Index Operation"
ms.date: 06/03/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 'Progress Report: Online Index Operation event class [SQL Server]'
ms.assetid: 491616c1-f666-4b16-a5ea-1192bf156692
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.custom: seo-lt-2019
ms.openlocfilehash: c612dfd308f20c56f1e216861c1f59e6e169107a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205656"
---
# <a name="progress-report-online-index-operation-event-class"></a>Classe d'événements Progress Report: Online Index Operation
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
  La classe d'événements Progress Report: Online Index Operation indique la progression d'une opération de construction d'index en ligne en cours.  
  
## <a name="progress-report-online-index-operation-event-class-data-columns"></a>Colonnes de données de la classe d'événements Progress Report: Online Index Operation  
  
|Nom de la colonne de données|Type de données|Description|ID de la colonne|Filtrable|  
|----------------------|---------------|-----------------|---------------|----------------|  
|ApplicationName|**nvarchar**|Nom de l'application cliente qui a créé la connexion à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Cette colonne est remplie avec les valeurs passées par l'application plutôt que par le nom affiché du programme.|10|Oui|  
|BigintData1|**bigint**|Nombre de lignes insérées.|52|Oui|  
|BigintData2|**bigint**|0 = plan en série ; sinon, ID du thread lors d'une exécution parallèle.|53|Oui|  
|ClientProcessID|**int**|ID affecté par l'ordinateur hôte au processus dans lequel s'exécute l'application cliente. La colonne de données est remplie si le client fournit l'ID du processus client.|9|Oui|  
|DatabaseID|**int**|ID de la base de données spécifiée par l'instruction USE *database* ou celui de la base de données par défaut si aucune instruction USE *database* n'a été spécifiée pour une instance donnée. [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] affiche le nom de la base de données si la colonne de données ServerName est capturée dans la trace et que le serveur est disponible. Déterminez la valeur pour une base de données à l'aide de la fonction DB_ID.|3|Oui|  
|nom_base_de_données|**nvarchar**|Nom de la base de données dans laquelle l'instruction de l'utilisateur est exécutée.|35|Oui|  
|Duration|**bigint**|Temps (en microsecondes) pris par l'événement.|13|Oui|  
|EndTime|**datetime**|Heure à laquelle l'opération d'index en ligne s'est achevée.|15|Oui|  
|EventClass|**int**|Type d’événement = 190.|27|Non|  
|EventSequence|**int**|Séquence d'un événement donné au sein de la demande.|51|Non|  
|EventSubClass|**int**|Type de sous-classe d'événements.<br /><br /> 1=Début<br /><br /> 2=Début d'exécution Étape 1<br /><br /> 3=Fin d'exécution Étape 1<br /><br /> 4=Début d'exécution Étape 2<br /><br /> 5=Fin d'exécution Étape 2<br /><br /> 6=Nombre de lignes insérées<br /><br /> 7=Fin<br /><br /> L’étape 1 référence l’objet de base (index cluster ou segment de mémoire), ou indique si l’opération d’index implique un index non-cluster uniquement. L’étape 2 est utilisée lorsqu’une opération de création d’index implique la reconstruction d’origine et des index non-cluster supplémentaires.  Par exemple, si un objet a un index cluster et plusieurs index non-cluster, l’option Regénérer tout permet de reconstruire tous les index. L’objet de base (index cluster) est reconstruit à l’étape 1, puis tous les index non-cluster sont reconstruits à l’étape 2.|21|Oui|  
|GroupID|**int**|ID du groupe de charges de travail où l'événement Trace SQL se déclenche.|66|Oui|  
|HostName|**nvarchar**|Nom de l'ordinateur sur lequel le client est exécuté. La colonne de données est remplie si le client fournit le nom de l'hôte. Pour déterminer le nom de l'hôte, utilisez la fonction HOST_NAME.|8|Oui|  
|IndexID|**int**|ID de l'index de l'objet affecté par l'événement.|24|Oui|  
|IsSystem|**int**|Indique si l'événement s'est produit sur un processus système ou sur un processus utilisateur. 1 = système, 0 = utilisateur.|60|Oui|  
|LoginName|**nvarchar**|Nom de la connexion de l'utilisateur (soit la connexion de sécurité [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , soit les informations d'identification de connexion [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows au format DOMAINE\nom_utilisateur).|11|Oui|  
|LoginSid|**image**|Numéro d'identification de sécurité (SID) de l'utilisateur connecté. Vous pouvez trouver ces informations dans l'affichage catalogue sys.server_principals. Chaque connexion possède un SID unique au niveau du serveur.|41|Oui|  
|NTDomainName|**nvarchar**|Domaine Windows auquel appartient l'utilisateur.|7|Oui|  
|NTUserName|**nvarchar**|Nom d'utilisateur Windows.|6|Oui|  
|ObjectID|**int**|ID affecté à l'objet par le système.|22|Oui|  
|ObjectName|**nvarchar**|Nom de l'objet référencé.|34|Oui|  
|PartitionId|**bigint**|ID de la partition en cours de génération.|65|Oui|  
|PartitionNumber|**int**|Numéro ordinaire de la partition en cours de génération.|25|Oui|  
|ServerName|**nvarchar**|Nom de l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tracée.|26|Non|  
|SessionLoginName|**nvarchar**|Nom de connexion de l'utilisateur à l'origine de la session. Par exemple, si vous vous connectez à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en utilisant le nom Connexion1 et que vous exécutez une instruction en tant que Connexion2, SessionLoginName affiche Connexion1 et LoginName, Connexion2. Cette colonne affiche à la fois les connexions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et Windows.|64|Oui|  
|SPID|**int**|ID de la session au cours de laquelle l'événement s'est produit.|12|Oui|  
|StartTime|**datetime**|Heure de début de l'événement.|14|Oui|  
|TransactionID|**bigint**|ID affecté par le système à la transaction.|4|Oui|  
  
## <a name="see-also"></a>Voir aussi  
 [sp_trace_setevent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)  
  
  
