---
description: Classe d'événements QN:Parameter Table
title: Classe d’événements QN:Parameter Table | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- event classes [SQL Server], QN:Parameter Table
ms.assetid: 292da1ed-4c7e-4bd2-9b84-b9ee09917724
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 9bfa37ec3e59c1e67b3909eaad7926e443f5d5d6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99178164"
---
# <a name="qnparameter-table-event-class"></a>Classe d'événements QN:Parameter Table
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
  L'événement QN:Parameter Table fournit des informations sur les opérations requises pour créer et supprimer les tables internes chargées de stocker les informations de paramètre et maintenir les nombres de références de ces tables. Il signale également l'activité interne pour réinitialiser le nombre d'utilisations d'une table de paramètres.  
  
## <a name="qnparameter-table-event-class-data-columns"></a>Colonnes de données de la classe d'événements QN:Parameter Table  
  
|Colonne de données|Type|Description|Numéro de colonne|Filtrable|  
|-----------------|----------|-----------------|-------------------|----------------|  
|ApplicationName|**nvarchar**|Nom de l'application cliente qui a créé la connexion à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Cette colonne est remplie avec les valeurs passées par l'application plutôt que par le nom affiché du programme.|10|Oui|  
|ClientProcessID|**int**|ID affecté par l'ordinateur hôte au processus dans lequel s'exécute l'application cliente. Cette colonne de données est remplie si l'ID du processus du client est fourni par le client.|9|Oui|  
|DatabaseID|**int**|ID de la base de données spécifiée par l’instruction USE *database* ou celui de la base de données par défaut si aucune instruction USE *database* n’a été spécifiée pour une instance donnée. [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] affiche le nom de la base de données si la colonne de données ServerName est capturée dans la trace et que le serveur est disponible. Déterminez la valeur pour une base de données à l'aide de la fonction DB_ID.|3|Oui|  
|nom_base_de_données|**nvarchar**|Nom de la base de données dans laquelle l'instruction de l'utilisateur est exécutée.|35|Oui|  
|EventClass|**Int**|Type d’événement = 200.|27|Non|  
|EventSequence|**int**|Numéro de séquence de cet événement.|51|Non|  
|EventSubClass|**nvarchar**|Type de sous-classe d’événements, qui fournit des informations complémentaires concernant chaque classe d’événements. Cette colonne peut contenir les valeurs suivantes :<br /><br /> **Table created**: indique qu’une table de paramètres a été créée dans la base de données.<br /><br /> **Table drop attempt**: indique que la base de données a tenté de supprimer automatiquement une table de paramètres inutilisée afin de libérer des ressources.<br /><br /> **Table drop attempt failed**: indique que la base de données a tenté de supprimer une table de paramètres inutilisée mais a échoué. Le [!INCLUDE[ssDE](../../includes/ssde-md.md)] replanifie alors automatiquement la suppression de la table de paramètres afin de libérer des ressources.<br /><br /> **Table dropped**: indique que la base de données a correctement supprimé une table de paramètres.<br /><br /> **Table pinned**: indique que la table de paramètres est marquée en cours d’utilisation dans le cadre d’un traitement interne.<br /><br /> **Table unpinned**: indique que la table de paramètres est libre. Le traitement interne de la table est terminé.<br /><br /> **Number of users incremented**: indique que le nombre d’abonnements aux notifications de requêtes référençant une table de paramètres a augmenté.<br /><br /> **Number of users decremented**: indique que le nombre d’abonnements aux notifications de requêtes référençant une table de paramètres a diminué.<br /><br /> **LRU counter reset**: indique que le nombre d’utilisations de la table de paramètres a été réinitialisé.<br /><br /> **Cleanup task started**: indique à quel moment le nettoyage de tous les abonnements au sein de la table de paramètres a débuté. Ceci se produit au démarrage de la base de données ou lorsqu'une table sous-jacente des abonnements de la table de paramètres est supprimée.<br /><br /> **Cleanup task finished**: indique à quel moment le nettoyage de tous les abonnements au sein de la table de paramètres s’est terminé.|21|Oui|  
|GroupID|**int**|ID du groupe de charges de travail où l'événement Trace SQL se déclenche.|66|Oui|  
|HostName|**nvarchar**|Nom de l'ordinateur sur lequel s'exécute le client. Cette colonne de données est remplie si le nom de l'hôte est fourni par le client. Pour déterminer le nom de l'hôte, utilisez la fonction HOST_NAME.|8|Oui|  
|IsSystem|**int**|Indique si l'événement s'est produit sur un processus système ou sur un processus utilisateur.<br /><br /> 0 = utilisateur<br /><br /> 1 = système|60|Non|  
|LoginName|**nvarchar**|Nom de connexion de l’utilisateur (connexion de sécurité [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou informations d’identification de connexion Windows sous la forme *DOMAINE*\\*nom_utilisateur*).|11|Non|  
|LoginSID|**image**|Numéro d'identification de sécurité (SID) de l'utilisateur connecté. Vous pouvez trouver ces informations dans l'affichage catalogue sys.server_principals. Chaque connexion possède un SID unique au niveau du serveur.|41|Oui|  
|NTDomainName|**nvarchar**|Domaine Windows auquel appartient l'utilisateur.|7|Oui|  
|NTUserName|**nvarchar**|Nom de l'utilisateur propriétaire de la connexion ayant généré l'événement.|6|Oui|  
|RequestID|**int**|Identificateur de la demande contenant l'instruction.|49|Oui|  
|ServerName|**nvarchar**|Nom de l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tracée.|26|Non|  
|SessionLoginName|**nvarchar**|Nom de connexion de l'utilisateur à l'origine de la session. Par exemple, si une application se connecte à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] au moyen de Login1 et qu'elle exécute une commande en tant que Connexion2, SessionLoginName affiche « Connexion1 » et LoginName affiche « Connexion2 ». Cette colonne affiche à la fois les connexions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et Windows.|64|Oui|  
|SPID|**int**|ID de la session au cours de laquelle l'événement s'est produit.|12|Oui|  
|StartTime|**datetime**|Heure à laquelle a débuté l'événement, si elle est connue.|14|Oui|  
|TextData|**ntext**|Retourne un document XML contenant des informations spécifiques à cet événement. Ce document est conforme au schéma XML disponible sur la page [SQL Server Query Notification Profiler Event Schema](https://go.microsoft.com/fwlink/?LinkId=63331) (en anglais).|1|Oui|  
  
  
