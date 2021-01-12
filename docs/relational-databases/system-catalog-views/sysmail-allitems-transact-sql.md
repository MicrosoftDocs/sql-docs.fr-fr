---
description: sysmail_allitems (Transact-SQL)
title: sysmail_allitems (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sysmail_allitems_TSQL
- sysmail_allitems
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_allitems database mail view
ms.assetid: 21fb8432-7677-4435-902f-64a58bba4cbb
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 6bc9b58549bbc20fe66caaf2adc61edd4345acd7
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98096654"
---
# <a name="sysmail_allitems-transact-sql"></a>sysmail_allitems (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Contient une ligne pour chaque message traité par la messagerie de base de données. Utilisez cette vue lorsque vous voulez voir l'état de tous les messages.  
  
 Pour afficher uniquement les messages dont l’État a échoué, utilisez [sysmail_faileditems &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sysmail-faileditems-transact-sql.md). Pour afficher uniquement les messages non envoyés, utilisez [sysmail_unsentitems &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sysmail-unsentitems-transact-sql.md). Pour afficher uniquement les messages qui ont été envoyés, utilisez [sysmail_sentitems &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sysmail-sentitems-transact-sql.md).  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**mailitem_id**|**int**|Identificateur de l'élément de messagerie dans la file d'attente des messages.|  
|**profile_id**|**int**|Identificateur du profil utilisé pour envoyer le message.|  
|**destinataire**|**varchar(max)**|Adresses de messagerie des destinataires du message.|  
|**copy_recipients**|**varchar(max)**|Adresses de messagerie des personnes qui reçoivent une copie du message.|  
|**blind_copy_recipients**|**varchar(max)**|Adresses de messagerie des personnes qui reçoivent une copie du message mais dont le nom n'apparaît pas dans l'en-tête du message.|  
|**subject**|**nvarchar (510)**|Ligne d'objet du message.|  
|**body**|**varchar(max)**|le corps du message.|  
|**body_format**|**varchar (20)**|Format du corps du message. Les valeurs possibles sont TEXT et HTML.|  
|**importance**|**varchar (6)**|Paramètre d' **importance** du message.|  
|**sensibilité**|**varchar (12)**|Paramètre de **sensibilité** du message.|  
|**file_attachments**|**varchar(max)**|Liste des noms de fichiers joints au message électronique (délimitée par des points-virgules).|  
|**attachment_encoding**|**varchar (20)**|Type de pièce jointe.|  
|**query**|**varchar(max)**|Requête exécutée par le programme de messagerie.|  
|**execute_query_database**|**sysname**|Contexte de base de données dans lequel le programme de messagerie a exécuté la requête.|  
|**attach_query_result_as_file**|**bit**|Lorsque la valeur est 0, les résultats de la requête ont été inclus dans le corps du message électronique, après le contenu du corps. Lorsque la valeur est 1, les résultats ont été renvoyés sous forme de pièce jointe.|  
|**query_result_header**|**bit**|Lorsque la valeur est 1, cela signifie que les résultats de la requête contenaient des en-têtes de colonne. Lorsque la valeur est 0, cela signifie que les résultats de la requête ne contenaient pas d'en-têtes de colonne.|  
|**query_result_width**|**int**|Paramètre **query_result_width** du message.|  
|**query_result_separator**|**char(1)**|Caractère utilisé pour séparer les colonnes dans la sortie de la requête.|  
|**exclude_query_output**|**bit**|Paramètre **exclude_query_output** du message. Pour plus d’informations, consultez [sp_send_dbmail &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-send-dbmail-transact-sql.md).|  
|**append_query_error**|**bit**|Paramètre **append_query_error** du message. La valeur 0 indique que la messagerie de base de données ne doit pas envoyer le message électronique s'il existe une erreur dans la requête.|  
|**send_request_date**|**datetime**|Date et heure d'arrivée du message dans la file d'attente des messages.|  
|**send_request_user**|**sysname**|Utilisateur qui a envoyé le message. Il s'agit du contexte utilisateur de la procédure de la messagerie de base de données, et non du champ De : du message.|  
|**sent_account_id**|**int**|Identificateur du compte de messagerie de base de données utilisé pour envoyer le message.|  
|**sent_status**|**varchar (8)**|État du message. Les valeurs possibles sont les suivantes :<br /><br /> **sent** : le message a été envoyé.<br /><br /> non **envoyé** : la messagerie de base de données tente toujours d’envoyer le message.<br /><br /> **nouvelle tentative** -Database mail n’a pas pu envoyer le message, mais tente de l’envoyer à nouveau.<br /><br /> **échec** : la messagerie de base de données n’a pas pu envoyer le message.|  
|**sent_date**|**datetime**|Date et heure d'envoi du message.|  
|**last_mod_date**|**datetime**|Date et heure de la dernière modification de la ligne.|  
|**last_mod_user**|**sysname**|Dernier utilisateur qui a modifié la ligne.|  
  
## <a name="remarks"></a>Notes  
 Utilisez la vue **sysmail_allitems** pour afficher l’état de tous les messages traités par Database mail. En cas de dépannage de la messagerie de base de données, cette vue peut vous aider à identifier la nature du problème en vous permettant de comparer les attributs des messages qui ont été envoyés aux attributs des messages qui n'ont pas été envoyés.  
  
 Les tables système exposées par cette vue contiennent tous les messages et peuvent entraîner la croissance de la base de données **msdb** . Supprimez périodiquement les anciens messages de la vue afin de réduire la taille de ces tables. Pour plus d’informations, consultez [créer un travail de SQL Server Agent pour archiver les Messages Database mail et les journaux des événements](../../relational-databases/database-mail/create-a-sql-server-agent-job-to-archive-database-mail-messages-and-event-logs.md).  
  
## <a name="permissions"></a>Autorisations  
 Accordé au rôle serveur fixe **sysadmin** et au rôle de base de données **DatabaseMailUserRole** . Lorsqu’elle est exécutée par un membre du rôle serveur fixe **sysadmin** , cette vue affiche tous les messages. Les autres utilisateurs voient uniquement les messages qu'ils ont essayé d'envoyer.  
  
  
