---
description: Classe d'événements QN:Subscription
title: QN:Subscription, classe d’événements | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- event classes [SQL Server], QN:Subscription
ms.assetid: 4916167e-8541-43b4-900e-ec8e6adcbc34
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 1efb259b1cd895c3c7b7978bde78121d0e042d54
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99212151"
---
# <a name="qnsubscription-event-class"></a>Classe d'événements QN:Subscription
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
  L'événement QN:Subscription fournit des informations sur les abonnements aux notifications.  
  
## <a name="qnsubscription-event-class-data-columns"></a>Colonnes de données de la classe d'événements QN:Subscription  
  
|Colonne de données|Type|Description|Numéro de colonne|Filtrable|  
|-----------------|----------|-----------------|-------------------|----------------|  
|ApplicationName|**nvarchar**|Nom de l'application cliente qui a créé la connexion à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Cette colonne est remplie avec les valeurs passées par l'application plutôt que par le nom affiché du programme.|10|Oui|  
|ClientProcessID|**int**|ID affecté par l'ordinateur hôte au processus dans lequel s'exécute l'application cliente. Cette colonne de données est remplie si l'ID du processus du client est fourni par le client.|9|Oui|  
|DatabaseID|**int**|ID de la base de données spécifiée par l’instruction USE *database* ou celui de la base de données par défaut si aucune instruction USE *database* n’a été spécifiée pour une instance donnée. [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] affiche le nom de la base de données si la colonne de données ServerName est capturée dans la trace et que le serveur est disponible. Déterminez la valeur pour une base de données à l'aide de la fonction DB_ID.|3|Oui|  
|nom_base_de_données|**nvarchar**|Nom de la base de données dans laquelle l'instruction de l'utilisateur est exécutée.|35|Oui|  
|EventClass|**int**|Type d’événement = 199.|27|Non|  
|EventSequence|**int**|Numéro de séquence de cet événement.|51|Non|  
|EventSubClass|**nvarchar**|Type de sous-classe d’événements, qui fournit des informations complémentaires concernant chaque classe d’événements. Cette colonne peut contenir les valeurs suivantes :<br /><br /> **Subscription registered**: indique quand l’abonnement aux notifications de requêtes a été enregistré dans la base de données.<br /><br /> **Subscription rewound**: indique quand le [!INCLUDE[ssDE](../../includes/ssde-md.md)] reçoit une demande d’abonnement correspondant précisément à un abonnement existant. Dans ce cas, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] définit la valeur de délai de l’abonnement existant sur le délai spécifié dans la nouvelle demande d’abonnement.<br /><br /> **Subscription fired**: indique quand un abonnement aux notifications produit un message de notification.<br /><br /> **Firing failed with broker error**: indique quand un message de notification échoue en raison d’une erreur [!INCLUDE[ssSB](../../includes/sssb-md.md)] .<br /><br /> **Firing failed without broker error**: indique quand un message de notification échoue mais n’est pas dû à une erreur [!INCLUDE[ssSB](../../includes/sssb-md.md)] .<br /><br /> **Broker error intercepted**: indique que le [!INCLUDE[ssSB](../../includes/sssb-md.md)] a généré une erreur dans la conversation que la notification de requête utilise.<br /><br /> **Subscription deletion attempt**: indique que le [!INCLUDE[ssDE](../../includes/ssde-md.md)] a tenté de supprimer un abonnement ayant expiré pour libérer des ressources.<br /><br /> **Subscription deletion failed**: indique que la tentative de suppression d’un abonnement ayant expiré a échoué. Le [!INCLUDE[ssDE](../../includes/ssde-md.md)] replanifiera automatiquement la suppression de l’abonnement pour libérer des ressources.<br /><br /> **Subscription destroyed**: indique que le [!INCLUDE[ssDE](../../includes/ssde-md.md)] a supprimé avec succès un abonnement ayant expiré.|21|Oui|  
|GroupID|**int**|ID du groupe de charges de travail où l'événement Trace SQL se déclenche.|66|Oui|  
|HostName|**nvarchar**|Nom de l'ordinateur sur lequel s'exécute le client. Cette colonne de données est remplie si le nom de l'hôte est fourni par le client. Pour déterminer le nom de l'hôte, utilisez la fonction HOST_NAME.|8|Oui|  
|IsSystem|**int**|Indique si l'événement s'est produit sur un processus système ou sur un processus utilisateur.<br /><br /> 0 = utilisateur<br /><br /> 1 = système|60|Non|  
|LoginName|**nvarchar**|Nom de la connexion de l'utilisateur (soit la connexion de sécurité [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , soit les informations d'identification de connexion Windows au format DOMAINE\nom_utilisateur).|11|Non|  
|LoginSID|**image**|Numéro d'identification de sécurité (SID) de l'utilisateur connecté. Vous pouvez trouver ces informations dans l'affichage catalogue sys.server_principals. Chaque connexion possède un SID unique au niveau du serveur.|41|Oui|  
|NTDomainName|**nvarchar**|Domaine Windows auquel appartient l'utilisateur.|7|Oui|  
|NTUserName|**nvarchar**|Nom de l'utilisateur propriétaire de la connexion ayant généré l'événement.|6|Oui|  
|RequestID|**int**|Identificateur de la demande contenant l'instruction.|49|Oui|  
|ServerName|**nvarchar**|Nom de l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tracée.|26|Non|  
|SessionLoginName|**nvarchar**|Nom de connexion de l'utilisateur à l'origine de la session. Par exemple, si une application se connecte à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] au moyen de Login1 et qu'elle exécute une commande en tant que Connexion2, SessionLoginName affiche « Connexion1 » et LoginName affiche « Connexion2 ». Cette colonne affiche à la fois les connexions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et Windows.|64|Oui|  
|SPID|**int**|ID de la session au cours de laquelle l'événement s'est produit.|12|Oui|  
|StartTime|**datetime**|Heure à laquelle a débuté l'événement, si elle est connue.|14|Oui|  
|TextData|**ntext**|Retourne un document XML contenant des informations spécifiques à cet événement. Ce document est conforme au schéma XML disponible sur la page [SQL Server Query Notification Profiler Event Schema](https://go.microsoft.com/fwlink/?LinkId=63331) (en anglais).|1|Oui|  
  
  
