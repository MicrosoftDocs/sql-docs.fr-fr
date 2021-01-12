---
description: sysmail_event_log (Transact-SQL)
title: sysmail_event_log (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sysmail_event_log
- sysmail_event_log_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_event_log database mail view
ms.assetid: 440bc409-1188-4175-afc4-c68e31e44fed
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 58c8e734ed9acdef41b22772c8d3955b825a46de
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98096634"
---
# <a name="sysmail_event_log-transact-sql"></a>sysmail_event_log (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contient une ligne pour chaque message Windows ou [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] retourné par le système de messagerie de base de données (Le message dans ce contexte fait référence à un message, tel qu’un message d’erreur, et non un message électronique.) Configurez le paramètre de **niveau de journalisation** à l’aide de la boîte de dialogue **configurer les paramètres système** de l’Assistant Configuration de Database mail ou de la procédure stockée [sysmail_configure_sp](../../relational-databases/system-stored-procedures/sysmail-configure-sp-transact-sql.md) pour déterminer quels messages sont renvoyés.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**Log_id**|**int**|Identificateur d'éléments du journal.|  
|**event_type**|**varchar (11)**|Type d'avis inséré dans le journal. Les valeurs possibles sont les suivantes : erreurs, avertissements, messages d'information, messages de succès et messages internes supplémentaires.|  
|**log_date**|**datetime**|Date et l'heure de création de l'entrée du journal.|  
|**description**|**nvarchar(max)**|Texte du message en cours d'enregistrement.|  
|**process_id**|**int**|L'ID de processus du programme externe de messagerie de base de données. Cette valeur change en principe à chaque démarrage du programme externe de messagerie de base de données.|  
|**mailitem_id**|**int**|Identificateur de l'élément de messagerie dans la file d'attente des messages. La valeur est NULL si le message n'est pas associé à un élément de courrier électronique spécifique.|  
|**account_id**|**int**|**Account_id** du compte associé à l’événement. La valeur est NULL si le message n'est pas associé à un compte spécifique.|  
|**last_mod_date**|**datetime**|Date et heure de la dernière modification de la ligne.|  
|**last_mod_user**|**sysname**|Dernier utilisateur qui a modifié la ligne. Pour les messages électroniques, il s'agit de l'utilisateur qui a envoyé le message. Pour les messages générés par le programme externe de messagerie de base de données, il s'agit du contexte utilisateur du programme.|  
  
## <a name="remarks"></a>Notes  
 Lors de la résolution des problèmes Database Mail, recherchez dans la vue **sysmail_event_log** des événements liés aux échecs de courrier électronique. Certains messages, comme ceux signalant l'échec du programme externe de messagerie de base de données, ne sont pas associés à des messages électroniques spécifiques. Pour rechercher des erreurs liées à des messages électroniques spécifiques, recherchez la **mailitem_id** du message électronique ayant échoué dans la vue **sysmail_faileditems** puis recherchez dans le **sysmail_event_log** les messages relatifs à ce **mailitem_id**. Lorsqu’une erreur est retournée à partir de **sp_send_dbmail**, le message électronique n’est pas envoyé au système Database mail et l’erreur ne s’affiche pas dans cette vue.  
  
 Lorsque les tentatives de remise d'un compte spécifique échouent, la messagerie de base de données conserve les messages d'erreur pendant les tentatives de reprises de comptes jusqu'à ce que la remise de l'élément de messagerie aboutisse ou échoue. En cas de réussite ultime, toutes les erreurs accumulées sont consignées en tant qu’avertissements séparés, y compris les **account_id**. Il se peut alors que des avertissements s'affichent, bien que le message électronique ait été envoyé. En cas d’échec de remise ultime, tous les avertissements précédents sont consignés sous la forme d’un message d’erreur sans **account_id**, car tous les comptes ont échoué.  
  
## <a name="permissions"></a>Autorisations  
 Pour accéder à cette vue, vous devez être membre du rôle serveur fixe **sysadmin** ou du rôle de base de données **DatabaseMailUserRole** . Les membres de **DatabaseMailUserRole** qui ne sont pas membres du rôle **sysadmin** peuvent uniquement voir les événements pour les courriers électroniques qu’ils envoient.  
  
## <a name="see-also"></a>Voir aussi  
 [sysmail_faileditems &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sysmail-faileditems-transact-sql.md)   
 [Programme externe de la messagerie de base de données](../../relational-databases/database-mail/database-mail-external-program.md)  
  
  
