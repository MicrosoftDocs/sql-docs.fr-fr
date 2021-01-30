---
description: sysmail_unsentitems (Transact-SQL)
title: sysmail_unsentitems (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysmail_unsentitems_TSQL
- sysmail_unsentitems
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_unsentitems database mail view
ms.assetid: 993c12da-41e5-4e53-a188-0323feb70c67
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: c532febf9074456ec1806fdb9d7ad0eed9b9b26a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199689"
---
# <a name="sysmail_unsentitems-transact-sql"></a>sysmail_unsentitems (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Contient une ligne pour chaque message de Database Mail avec l’état non **envoyé** ou **nouvelle tentative** . Les messages dont l'état est 'unsent' ou 'retrying' se trouvent toujours dans la file d'attente des messages et peuvent être envoyés à tout moment. Les messages peuvent avoir l’état non **envoyé** pour les raisons suivantes :  
  
-   il s'agit d'un nouveau message et, bien qu'il ait été placé dans la file d'attente des messages, la messagerie de base de données est en train d'envoyer d'autres messages et n'a pas encore atteint ce message ;  
  
-   le programme externe de messagerie de base de données n'est pas en cours d'exécution et aucun message n'est envoyé.  
  
 Les messages peuvent avoir l’état **renouvel essai** pour les raisons suivantes :  
  
-   La messagerie de base de données a essayé d'envoyer le message mais n'a pas réussi à contacter le serveur de messagerie SMTP. La messagerie de base de données va continuer à essayer d'envoyer le message en utilisant d'autres comptes de messagerie de base de données attribués au profil qui a envoyé le message. Si aucun compte ne peut envoyer le message, Database Mail attend la durée configurée pour le paramètre **délai entre reprises de comptes** , puis tente à nouveau d’envoyer le message. Database Mail utilise le paramètre tentatives de reprises de **comptes** pour déterminer le nombre de tentatives d’envoi du message. Les messages conservent l’état **nouvelle tentative** tant que Database mail tente d’envoyer le message.  
  
 Utilisez cette vue lorsque vous voulez voir combien de messages se trouvent dans la file d'attente des messages et depuis combien de temps ils s'y trouvent. Normalement, le nombre de messages non **envoyés** sera faible. Procédez à un test d'évaluation au cours d'opérations normales pour déterminer un nombre approprié de messages en file d'attente pour vos opérations.  
  
 Pour afficher tous les messages traités par Database Mail, utilisez [sysmail_allitems &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sysmail-allitems-transact-sql.md). Pour afficher uniquement les messages dont l’État a échoué, utilisez [sysmail_faileditems &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sysmail-faileditems-transact-sql.md). Pour afficher uniquement les messages qui ont été envoyés, utilisez [sysmail_sentitems &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sysmail-sentitems-transact-sql.md).  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**mailitem_id**|**int**|Identificateur de l'élément de messagerie dans la file d'attente des messages.|  
|**profile_id**|**int**|Identificateur du profil utilisé pour envoyer le message.|  
|**destinataire**|**varchar(max)**|Adresses de messagerie des destinataires du message.|  
|**copy_recipients**|**varchar(max)**|Adresses de messagerie des personnes qui reçoivent une copie du message.|  
|**blind_copy_recipients**|**varchar(max)**|Adresses de messagerie des personnes qui reçoivent une copie du message mais dont le nom n'apparaît pas dans l'en-tête du message.|  
|**subject**|**nvarchar (510)**|Ligne d'objet du message.|  
|**body**|**varchar(max)**|le corps du message.|  
|**body_format**|**varchar (20)**|Format du corps du message. Les valeurs possibles sont **Text** et **HTML**.|  
|**importance**|**varchar (6)**|Paramètre d' **importance** du message.|  
|**sensibilité**|**varchar (12)**|Paramètre de **sensibilité** du message.|  
|**file_attachments**|**varchar(max)**|Liste des noms de fichiers joints au message électronique (délimitée par des points-virgules).|  
|**attachment_encoding**|**varchar (20)**|Type de pièce jointe.|  
|**requête**|**varchar(max)**|Requête exécutée par le programme de messagerie.|  
|**execute_query_database**|**sysname**|Contexte de base de données dans lequel le programme de messagerie a exécuté la requête.|  
|**attach_query_result_as_file**|**bit**|Lorsque la valeur est 0, les résultats de la requête ont été inclus dans le corps du message électronique, après le contenu du corps. Lorsque la valeur est 1, les résultats ont été renvoyés sous forme de pièce jointe.|  
|**query_result_header**|**bit**|Lorsque la valeur est 1, cela signifie que les résultats de la requête contenaient des en-têtes de colonne. Lorsque la valeur est 0, cela signifie que les résultats de la requête ne contenaient pas d'en-têtes de colonne.|  
|**query_result_width**|**int**|Paramètre **query_result_width** du message.|  
|**query_result_separator**|**char(1)**|Caractère utilisé pour séparer les colonnes dans la sortie de la requête.|  
|**exclude_query_output**|**bit**|Paramètre **exclude_query_output** du message. Pour plus d’informations, consultez [sp_send_dbmail &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-send-dbmail-transact-sql.md).|  
|**append_query_error**|**bit**|Paramètre **append_query_error** du message. La valeur 0 indique que la messagerie de base de données ne doit pas envoyer le message électronique s'il existe une erreur dans la requête.|  
|**send_request_date**|**datetime**|Date et heure d'arrivée du message dans la file d'attente des messages.|  
|**send_request_user**|**sysname**|Utilisateur qui a envoyé le message. Il s’agit du contexte utilisateur de la procédure de messagerie de base de données, et non **du champ de du message** .|  
|**sent_account_id**|**int**|Identificateur du compte de messagerie de base de données utilisé pour envoyer le message. La valeur est toujours NULL pour cette vue.|  
|**sent_status**|**varchar (8)**|Sera non **envoyé** si Database mail n’a pas tenté d’envoyer le message. Une nouvelle **tentative sera tentée** si Database mail n’a pas réussi à envoyer le message, mais qu’il réessaie.|  
|**sent_date**|**datetime**|Date et heure de la dernière tentative d'envoi du message par la messagerie de base de données. La valeur est NULL si la messagerie de base de données n'a pas essayé d'envoyer le message.|  
|**last_mod_date**|**datetime**|Date et heure de la dernière modification de la ligne.|  
|**last_mod_user**|**sysname**|Dernier utilisateur qui a modifié la ligne.|  
  
## <a name="remarks"></a>Notes  
 En cas de dépannage de la messagerie de base de données, cette vue peut vous aider à identifier la nature du problème en vous montrant le nombre de messages se trouvant en file d'attente et leur temps d'attente. Si aucun message n'est envoyé, soit le programme externe de messagerie de base de données n'est pas en cours d'exécution, soit un problème réseau empêche la messagerie de base de données de contacter les serveurs SMTP. Si la plupart des messages non envoyés ont le même **profile_id**, il peut y avoir un problème avec le serveur SMTP. Pensez à ajouter des comptes supplémentaires au profil. Si les messages sont envoyés mais qu'ils restent trop longtemps dans la file d'attente, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nécessite peut-être des ressources plus importantes pour traiter le volume de messages dont vous avez besoin.  
  
## <a name="permissions"></a>Autorisations  
 Accordé au rôle serveur fixe **sysadmin** et au rôle de base de données **DatabaseMailUserRole** . Lorsqu’elle est exécutée par un membre du rôle serveur fixe **sysadmin** , cette vue affiche tous les messages non **envoyés** ou à **nouvel essai** . Tous les autres utilisateurs voient uniquement les messages non **envoyés** ou de **nouvelle tentative** qu’ils ont envoyés.  
  
  
