---
description: 'Database Mail : Message en file d’attente, non remis'
title: 'Database Mail : Message en file d’attente, non remis | Microsoft Docs'
ms.custom: ''
ms.date: 04/22/2019
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- architecture [SQL Server], Database Mail
- Database Mail [SQL Server], architecture
- Database Mail [SQL Server], components
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: ebf8096c4b679cc6d5ac01b164bb9ae4bb014e6e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352944"
---
# <a name="database-mail-mail-queued-not-delivered"></a>Database Mail : Message en file d’attente, non remis 
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

Cette rubrique explique comment remédier à la situation où les messages électroniques sont mis en attente avec succès, mais non remis.

## <a name="diagnose-the-problem"></a>Diagnostiquer le problème 

Le programme externe Database Mail consigne l’activité de la messagerie dans la base de données **msdb**.

En premier lieu, pour confirmer que Database Mail est activé, utilisez l’[option Database Mail XPs](../../database-engine/configure-windows/database-mail-xps-server-configuration-option.md) de la procédure stockée système **sp_configure**, comme dans l’exemple suivant :

```sql 
EXEC sp_configure 'show advanced', 1;  
RECONFIGURE; 
EXEC sp_configure; 
GO
```

Exécutez, ensuite l’instruction suivante dans la base de données **msdb** pour vérifier l’état de la file d’attente de messagerie :

```sql
sysmail_help_queue_sp @queue_type = 'Mail' ;
```

Pour obtenir une explication détaillée des colonnes, consultez [sysmail_help_queue_sp (Transact-SQL)](../system-stored-procedures/sysmail-help-queue-sp-transact-sql.md#result-set).

Vérifiez l’activité dans la vue **sysmail_event_log**. Cette vue doit contenir une entrée établissant que le programme externe de messagerie de base de données a été démarré. S’il n’existe aucune entrée dans la vue **sysmail_event_log**, consultez le symptôme [Messages en file d’attente, aucune entrée](database-mail-common-errors.md#database-mail-queued-no-entries-in-sysmail_event_log-or-windows-application-event-log) dans **sysmail_event_log**. S’il existe des erreurs dans la vue **sysmail_event_log**, résolvez l’erreur spécifique.

S’il existe des entrées dans la vue **sysmail_event_log**, vérifiez l’état des messages dans la vue **sysmail_allitems**.

## <a name="message-status-unsent"></a>État de message = Non envoyé 

Un état **non envoyé** indique que le [programme externe Database Mail](database-mail-external-program.md) n’a pas encore traité l’e-mail. Le programme externe de messagerie de base de données peut avoir pris du retard dans le traitement des messages ; la vitesse à laquelle le programme externe traite les messages dépend des conditions du réseau, du délai d'attente avant une nouvelle tentative, du volume des messages et de la capacité du serveur SMTP. Si le problème persiste, envisagez d'utiliser plusieurs profils pour distribuer les messages entre plusieurs serveurs SMTP.

Vérifiez la date de modification la plus récente pour les messages remis. Si la dernière remise s’est produite il y a quelque temps, vérifiez dans la vue sysmail_event_log que le programme externe a bien été démarré par Service Broker. Si la dernière tentative n’a pas démarré le programme externe, vérifiez que le Programme externe Database Mail se trouve dans le répertoire correct et que le compte de service pour SQL Server dispose de l’autorisation d’exécuter l’exécutable.

   > [!NOTE]
   > Pour supprimer les anciens messages non envoyés, attendez que les messages non remis soient les messages les plus anciens dans la file d’attente, puis supprimez-les avec [sysmail_delete_mailitems_sp](../system-stored-procedures/sysmail-delete-mailitems-sp-transact-sql.md).

## <a name="message-status-retrying"></a>État de message = Nouvel tentative

Un état reprise indique que la messagerie de base de données a tenté de remettre le message au serveur SMTP, mais n'a pas pu. Ceci est généralement dû à une interruption du réseau, à une défaillance du serveur SMTP ou à une configuration incorrecte du compte de messagerie de base de données. Le message finira par parvenir à destination, ou il échouera et postera un message dans le journal d'événements.

## <a name="message-status-sent"></a>État de message = Envoyé

Un état **envoyé** indique que le programme externe Database Mail a remis l’e-mail au serveur SMTP. Si le message n'est pas arrivé à destination, le serveur SMTP a accepté le message de la messagerie de base de données, mais n'a pas pu le remettre au destinataire final. Vérifiez les journaux du serveur SMTP ou contactez l'administrateur du serveur SMTP. Vous pouvez aussi tester le serveur de messagerie SMTP au moyen d'un autre client tel qu'Outlook Express.

## <a name="message-status-failed"></a>État de message = Échec

Un état Échec indique que le programme externe Database Mail n’a pas pu remettre le message au serveur SMTP. Dans ce cas, la vue **sysmail_event_log** contient les informations détaillées du programme externe. Pour obtenir un exemple de requête joignant **sysmail_faileditems** et **sysmail_event_log** pour récupérer des messages d’erreur détaillés, consultez [Vérifier l’état des messages électroniques envoyés avec la messagerie de base de données](check-the-status-of-e-mail-messages-sent-with-database-mail.md). Les causes d'échec les plus courantes sont une adresse de destination incorrecte ou des problèmes de réseau qui empêchent le programme externe d'atteindre un ou plusieurs des comptes de basculement. Des problèmes sur le serveur SMTP peuvent également entraîner le rejet du courrier électronique. Avec l’Assistant Configuration de Database Mail, définissez le **Niveau de journalisation** sur **Commentaires** et envoyez un message de test pour rechercher le point d’échec.



##  <a name="see-also"></a><a name="RelatedContent"></a> Voir aussi
  
-  [Vue d’ensemble de Database Mail](database-mail.md)

  
  
