---
description: Classe d'événements Lock:Acquired
title: Lock:Acquired, classe d’événements | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- Acquired event class
ms.assetid: a6b1df2a-06ed-4fc3-8a84-f0becd5810d5
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 96a8a6f655456ffd6e2536439a8904c32b0b7fe2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199883"
---
# <a name="lockacquired-event-class"></a>Classe d'événements Lock:Acquired
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  La classe d’événements Lock:Acquired indique que l’acquisition d’un verrou sur une ressource (une page de données, par exemple) a été effectuée.  
  
 Les classes d'événements Lock:Acquired et Lock:Released permettent de contrôler à quel moment des objets sont verrouillés, le type des verrous établis et la durée de maintien des verrous. Les verrous maintenus pendant de longues périodes peuvent causer des contentions et doivent être examinés. Par exemple, une application peut acquérir des verrous sur les lignes d'une table puis attendre la saisie de l'utilisateur. La saisie de l'utilisateur pouvant prendre du temps pour se produire, les verrous risquent de bloquer d'autres utilisateurs. Dans ce cas, l'application doit être modifiée pour effectuer des demandes de verrous uniquement lorsque cela est nécessaire et ne pas demander une saisie de l'utilisateur après avoir acquis des verrous.  
  
## <a name="lockacquired-event-class-data-columns"></a>Colonnes de la classe d'événements Lock:Acquired  
  
|Nom de la colonne de données|Type de données|Description|ID de la colonne|Filtrable|  
|----------------------|---------------|-----------------|---------------|----------------|  
|ApplicationName|**nvarchar**|Nom de l’application cliente qui a créé la connexion à une instance de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Cette colonne est remplie avec les valeurs passées par l'application plutôt que par le nom affiché du programme.|10|Oui|  
|BigintData1|**bigint**|ID de partition si la ressource Lock est partitionnée.|52|Oui|  
|BinaryData|**image**|Identificateur de ressource du verrou.|2|Oui|  
|ClientProcessID|**int**|ID affecté par l'ordinateur hôte au processus dans lequel s'exécute l'application cliente. La colonne de données est remplie si le client fournit l'ID du processus client.|9|Oui|  
|DatabaseID|**int**|ID de la base de données dans laquelle le verrou a été obtenu. [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] affiche le nom de la base de données si la colonne de données ServerName est capturée dans la trace et que le serveur est disponible. Déterminez la valeur pour une base de données à l'aide de la fonction DB_ID.|3|Oui|  
|Duration|**bigint**|Délai (en microsecondes) entre l'émission de la demande de verrou et l'acquisition du verrou.|13|Oui|  
|EndTime|**datetime**|Heure de fin de l'événement.|15|Oui|  
|EventClass|**int**|Type d’événement = 24.|27|Non|  
|EventSequence|**int**|Séquence d'un événement donné au sein de la demande.|51|Non|  
|GroupID|**int**|ID du groupe de charges de travail où l'événement Trace SQL se déclenche.|66|Oui|  
|HostName|**nvarchar**|Nom de l'ordinateur sur lequel le client est exécuté. La colonne de données est remplie si le client fournit le nom de l'hôte. Pour déterminer le nom de l'hôte, utilisez la fonction HOST_NAME.|8|Oui|  
|IntegerData2|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|55|Oui|  
|IsSystem|**int**|Indique si l'événement s'est produit sur un processus système ou sur un processus utilisateur. 1 = système, 0 = utilisateur.|60|Oui|  
|LoginName|**nvarchar**|Nom de la connexion de l'utilisateur (soit la connexion de sécurité [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , soit les informations d'identification Windows sous la forme DOMAINE\Nomutilisateur).|11|Oui|  
|LoginSid|**image**|Numéro d'identification de sécurité (SID) de l'utilisateur connecté. Vous pouvez trouver ces informations dans l'affichage catalogue sys.server_principals. Chaque connexion possède un SID unique au niveau du serveur.|41|Oui|  
|Mode|**int**|Mode obtenu après l'acquisition du verrou.<br /><br /> 0=NULL - Compatible avec tous les autres modes de verrouillage (LCK_M_NL)<br /><br /> 1=Verrou de stabilité de schéma (LCK_M_SCH_S)<br /><br /> 2 = verrou de modification de schéma (LCK_M_SCH_M)<br /><br /> 3 = verrou partagé (LCK_M_S)<br /><br /> 4 = verrou de mise à jour (LCK_M_U)<br /><br /> 5 = verrou exclusif (LCK_M_X)<br /><br /> 6 = verrou Intent partagé (LCK_M_IS)<br /><br /> 7 = verrou Intent de mise à jour (LCK_M_IU)<br /><br /> 8 = verrou Intent exclusif (LCK_M_IX)<br /><br /> 9 = verrou partagé avec mise à jour Intent (LCK_M_SIU)<br /><br /> 10 = verrou partagé avec Intent exclusif (LCK_M_SIX)<br /><br /> 11 = verrou mise à jour avec Intent exclusif (LCK_M_UIX)<br /><br /> 12 = verrou de mise à jour en bloc (LCK_M_BU)<br /><br /> 13 = verrou d'étendue de clés partagé/de ressources partagé (LCK_M_RS_S)<br /><br /> 14 = verrou d'étendue de clés partagé/de ressources partagé (LCK_M_RS_U)<br /><br /> 15 = verrou d'étendue de clés d'insertion de valeurs NULL (LCK_M_RI_NL)<br /><br /> 16 = verrou d'étendue de clés d'insertion partagé (LCK_M_RI_S)<br /><br /> 17 = verrou d'étendue de clés d'insertion de mise à jour (LCK_M_RI_U)<br /><br /> 18 = verrou d'étendue de clés d'insertion exclusif (LCK_M_RI_X)<br /><br /> 19 = verrou d'étendue de clés exclusif partagé (LCK_M_RX_S)<br /><br /> 20 = verrou d'étendue de clés exclusif de mise à jour (LCK_M_RX_U)<br /><br /> 21 = verrou d'étendue de clés exclusif/de ressources exclusif (LCK_M_RX_X)|32|Oui|  
|NTDomainName|**nvarchar**|Domaine Windows auquel appartient l'utilisateur.|7|Oui|  
|NTUserName|**nvarchar**|Nom d'utilisateur Windows.|6|Oui|  
|ObjectID|**int**|ID de l'objet sur lequel a été acquis le verrou (si disponible et si applicable).|22|Oui|  
|ObjectID2|**bigint**|ID de l'entité ou de l'objet associé, s'il est disponible et applicable.|56|Oui|  
|OwnerID|**int**|1=TRANSACTION<br /><br /> 2=CURSOR<br /><br /> 3=SESSION<br /><br /> 4=SHARED_TRANSACTION_WORKSPACE<br /><br /> 5=EXCLUSIVE_TRANSACTION_WORKSPACE|58|Oui|  
|RequestID|**int**|ID de la demande contenant l'instruction.|49|Oui|  
|ServerName|**nvarchar**|Nom de l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tracée.|26|Non|  
|SessionLoginName|**nvarchar**|Nom de connexion de l'utilisateur à l'origine de la session. Par exemple, si vous vous connectez à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en utilisant le nom Connexion1 et que vous exécutez une instruction en tant que Connexion2, SessionLoginName affiche Connexion1 et LoginName, Connexion2. Cette colonne affiche à la fois les connexions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et Windows.|64|Oui|  
|SPID|**int**|ID de la session au cours de laquelle l'événement s'est produit.|12|Oui|  
|StartTime|**datetime**|Heure à laquelle a débuté l'événement, si elle est connue.|14|Oui|  
|TextData|**ntext**|Valeur de texte dépendant du type de verrou acquis. Il s'agit de la même valeur que la colonne resource_description de sys.dm_tran_locks.|1|Oui|  
|TransactionID|**bigint**|ID affecté par le système à la transaction.|4|Oui|  
|Type|**int**|1=NULL_RESOURCE<br /><br /> 2=DATABASE<br /><br /> 3=FILE<br /><br /> 5=OBJECT<br /><br /> 6=PAGE<br /><br /> 7=KEY<br /><br /> 8=EXTENT<br /><br /> 9=RID<br /><br /> 10=APPLICATION<br /><br /> 11=METADATA<br /><br /> 12=AUTONAMEDB<br /><br /> 13=HOBT<br /><br /> 14=ALLOCATION_UNIT|57|Oui|  
  
## <a name="see-also"></a>Voir aussi  
 [Classe d'événements Lock:Released](../../relational-databases/event-classes/lock-released-event-class.md)   
 [sp_trace_setevent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)   
 [sys.dm_tran_locks &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md)  
  
  
