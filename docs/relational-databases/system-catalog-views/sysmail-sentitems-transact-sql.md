---
description: sysmail_sentitems (Transact-SQL)
title: sysmail_sentitems (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysmail_sentitems_TSQL
- sysmail_sentitems
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_sentitems database mail view
ms.assetid: 16eb2a44-cebb-4cec-93ac-e2498c39989f
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: ef28d1d962fbdcfffd13dee4599b30617bbc36ad
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199709"
---
# <a name="sysmail_sentitems-transact-sql"></a>sysmail_sentitems (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Contient une ligne pour chaque message envoyé par la messagerie de base de données. Utilisez **sysmail_sentitems** lorsque vous souhaitez voir les messages qui ont été envoyés avec succès.  
  
 Pour afficher tous les messages traités par Database Mail, utilisez [sysmail_allitems &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sysmail-allitems-transact-sql.md). Pour afficher uniquement les messages dont l’État a échoué, utilisez [sysmail_faileditems &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sysmail-faileditems-transact-sql.md). Pour afficher uniquement les messages non envoyés ou à nouvel essai, utilisez [sysmail_unsentitems &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sysmail-unsentitems-transact-sql.md). Pour afficher les pièces jointes du courrier électronique, utilisez [sysmail_mailattachments &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sysmail-mailattachments-transact-sql.md).  
  
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
|**send_request_user**|**sysname**|Utilisateur qui a envoyé le message. Il s'agit du contexte utilisateur de la procédure de la messagerie de base de données, et non du champ De : du message.|  
|**sent_account_id**|**int**|Identificateur du compte de messagerie de base de données utilisé pour envoyer le message.|  
|**sent_status**|**varchar (8)**|État du message. Toujours **envoyé** pour cette vue.|  
|**sent_date**|**datetime**|Date et heure d'envoi du message.|  
|**last_mod_date**|**datetime**|Date et heure de la dernière modification de la ligne.|  
|**last_mod_user**|**sysname**|Dernier utilisateur qui a modifié la ligne.|  
  
## <a name="remarks"></a>Notes  
 En cas de dépannage de la messagerie de base de données, cette vue peut vous aider à identifier la nature du problème en vous montrant les attributs des messages qui ont été correctement envoyés. La messagerie de base de données marque les messages comme envoyés ('sent') lorsqu'ils sont soumis avec succès à un serveur de messagerie SMTP. En principe, le message est reçu en l'espace de quelques minutes, mais il peut être retardé en raison de problèmes avec le serveur SMTP. La messagerie de base de données marque le message comme envoyé lorsque celui-ci est accepté par le serveur de messagerie SMTP. Les erreurs qui se produisent sur le serveur de messagerie SMTP, par exemple lorsque le message ne peut pas être remis à l'adresse de messagerie du destinataire, ne sont pas renvoyées à la messagerie de base de données. Ces messages sont donc considérés comme envoyés, bien qu'ils n'aient pas été remis. Vous devez résoudre ce type d'erreur sur le serveur SMTP. Le serveur de messagerie SMTP peut également envoyer un avis de non remise à l'adresse de réponse d'un compte de messagerie de base de données.  
  
## <a name="permissions"></a>Autorisations  
 Accordé au rôle serveur fixe **sysadmin** et au rôle de base de données **DatabaseMailUserRole** . Lorsqu’elle est exécutée par un membre du rôle serveur fixe **sysadmin** , cette vue affiche tous les messages envoyés. Les autres utilisateurs voient uniquement les messages qu'ils ont envoyés.  
  
## <a name="see-also"></a>Voir aussi  
 [Objets de messagerie de base de données](../../relational-databases/database-mail/database-mail-messaging-objects.md)  
  
  
