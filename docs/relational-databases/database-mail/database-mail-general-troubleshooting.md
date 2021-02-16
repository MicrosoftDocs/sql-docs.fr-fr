---
description: Étapes de résolution des problèmes généraux liés à Database Mail
title: Résolution des problèmes généraux liés à Database Mail
ms.date: 06/03/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- architecture [SQL Server], Database Mail
- Database Mail [SQL Server], architecture
- Database Mail [SQL Server], components
author: MashaMSFT
ms.author: mathoma
ms.custom: seo-dt-2019
ms.openlocfilehash: 2e9b7f5b5aec4f1b111983f893783e46ad14e021
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352949"
---
# <a name="general-database-mail-troubleshooting-steps"></a>Étapes de résolution des problèmes généraux liés à Database Mail 
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

Le dépannage de la messagerie de base de données implique de vérifier les aspects généraux du système de messagerie de base de données décrits ci-dessous. Ces procédures sont présentées dans un ordre logique, mais elles peuvent être évaluées dans n'importe quel ordre.

## <a name="permissions"></a>Autorisations

Vous devez être membre du rôle serveur fixe sysadmin pour pouvoir dépanner tous les aspects de Database Mail. Les utilisateurs qui ne sont pas membres du rôle serveur fixe sysadmin peuvent uniquement obtenir des informations sur les e-mails qu’ils ont essayé d’envoyer, mais pas sur les e-mails envoyés par d’autres utilisateurs.

## <a name="is-database-mail-enabled"></a>Database Mail est-il activé ?

1. Dans SQL Server Management Studio, connectez-vous à une instance de SQL Server à l’aide d’une fenêtre d’éditeur de requête, puis exécutez le code suivant :

    ```sql
    sp_configure 'show advanced', 1; 
    GO
    RECONFIGURE;
    GO
    sp_configure;
    GO
    ```

   Dans le volet de résultats, vérifiez que run_value pour [Database Mail XPs](../../database-engine/configure-windows/database-mail-xps-server-configuration-option.md) est défini sur 1.
   Si run_value n’est pas 1, Database Mail n’est pas activé. La messagerie de base de données n'est activée automatiquement, et ce, afin de réduire le nombre de fonctionnalités disponibles en cas d'attaque par un utilisateur malveillant. Pour plus d’informations, consultez [Présentation de la configuration de la surface d’exposition](../security/surface-area-configuration.md).

1. Si vous décidez que la messagerie de base de données peut être activée, exécutez le code suivant :

    ```sql
    sp_configure 'Database Mail XPs', 1; 
    GO
    RECONFIGURE;
    GO
    ```

1. Pour rétablir la procédure sp_configure à son état par défaut, qui ne présente pas d’options avancées, exécutez le code suivant :

    ```sql 
    sp_configure 'show advanced', 0; 
    GO
    RECONFIGURE;
    GO
    ```

## <a name="are-users-properly-configured-to-send-mail"></a>Les utilisateurs sont-ils correctement configurés pour envoyer du courrier ?

1. Pour envoyer du courrier avec Database Mail, vous devez être membre du rôle de base de données DatabaseMailUserRole dans la base de données msdb. Les membres du rôle serveur fixe sysadmin et du rôle msdbdb_owner sont automatiquement membres du rôle DatabaseMailUserRole. Pour afficher tous les autres membres du rôle DatabaseMailUserRole, exécutez l’instruction suivante :

    ```sql
    EXEC msdb.sys.sp_helprolemember 'DatabaseMailUserRole';
    ```

1. Pour ajouter des utilisateurs au rôle DatabaseMailUserRole, utilisez l’instruction suivante :

    ```sql
    USE msdb;
    GO
    
    sp_addrolemember @rolename = 'DatabaseMailUserRole'
    ,@membername = '<database user>';
    ```

1. Pour pouvoir envoyer des messages à l'aide de la messagerie de base de données, les utilisateurs doivent avoir accès à au moins un profil de messagerie de base de données. Pour afficher les utilisateurs (principaux) et les profils auxquels ils ont accès, exécutez l'instruction suivante :

    ```sql
    EXEC msdb.dbo.sysmail_help_principalprofile_sp;
    ```

1. Utilisez l'Assistant Configuration de la messagerie de base de données pour créer des profils et pour accorder l'accès des profils aux utilisateurs.
 
## <a name="is-database-mail-started"></a>Database Mail est-il démarré ?

1. Le [programme externe Database Mail](database-mail-external-program.md) est activé quand il y a des e-mails à traiter. S'il n'y a eu aucun message à envoyer pendant le délai d'attente spécifié, le programme est fermé. Pour confirmer que l’activation de Database Mail est démarrée, exécutez l’instruction suivante :

    ```sql
    EXEC msdb.dbo.sysmail_help_status_sp;
    ```
1. Si l'activation de la messagerie de base de données n'est pas démarrée, exécutez l'instruction suivante pour la démarrer :

    ```sql
    EXEC msdb.dbo.sysmail_start_sp;
    ```

1. Si le programme externe de messagerie de base de données est démarré, vérifiez l'état de la file d'attente des messages à l'aide de l'instruction suivante :

    ```sql
    EXEC msdb.dbo.sysmail_help_queue_sp @queue_type = 'mail';
    ```
  
   La file d’attente des messages doit être à l’état RECEIVES_OCCURRING. La file d'attente des états peut varier d'un moment à l'autre. Si l’état de la file d’attente des messages n’est pas RECEIVES_OCCURRING, essayez de redémarrer la file d’attente. Arrêtez la file d’attente à l’aide de l’instruction suivante :
   
```sql
EXEC msdb.dbo.sysmail_stop_sp;
```

Ensuite, démarrez la file d’attente à l’aide de l’instruction suivante :

```sql
EXEC msdb.dbo.sysmail_start_sp;
```

  > [!NOTE]
  >  Utilisez la colonne length dans le jeu de résultats de la procédure sysmail_help_queue_sp pour déterminer le nombre d’e-mails se trouvant dans la file d’attente des messages.

## <a name="do-problems-affect-some-or-all-accounts"></a>Les problèmes affectent-ils tous les comptes ou seulement certains d’entre eux ?

1. Si vous avez déterminé que certains profils, mais pas tous, peuvent envoyer des messages, il se peut que les problèmes viennent des comptes de messagerie de base de données utilisés par les profils affectés par les problèmes. Pour déterminer quels comptes parviennent à envoyer des messages, exécutez l'instruction suivante :

    ```sql
    SELECT sent_account_id, sent_date FROM msdb.dbo.sysmail_sentitems;
    ```

1. Si un profil défaillant n’utilise aucun des comptes listés, il est possible qu’aucun des comptes disponibles pour ce profil ne fonctionne correctement. Pour tester des comptes individuels, utilisez l’Assistant Configuration de Database Mail pour créer un nouveau profil associé à un compte unique, puis utilisez la boîte de dialogue Envoyer un message électronique de test pour envoyer un message à l’aide du nouveau compte. 
1. Pour afficher les messages d'erreur renvoyés par la messagerie de base de données, exécutez l'instruction suivante :

    ```sql
    SELECT * FROM msdb.dbo.sysmail_event_log;
    ```

   > [!NOTE]
   > Database Mail considère qu’un message a été envoyé quand il est remis à un serveur de messagerie SMTP. Les erreurs qui se produisent après, par exemple à cause d'une adresse de destinataire non valide, peuvent empêcher la remise du message, mais elles n'apparaissent pas dans le journal de la messagerie de base de données.

## <a name="retry-mail-delivery"></a>Nouvelles tentatives de remise de courrier

1. Si vous avez déterminé que la messagerie de base de données échoue parce que le serveur SMTP ne peut pas être contacté de façon fiable, vous pourrez peut-être augmenter le taux de réussite de la remise des messages en augmentant le nombre de tentatives d'envoi de chaque message. Démarrez l’Assistant Configuration de Database Mail et sélectionnez l’option Afficher ou modifier les paramètres du système. Pour résoudre ce problème, vous pouvez également associer des comptes supplémentaires au profil ; ainsi, en cas de basculement du compte principal, la messagerie de base de données utilisera le compte de remplacement pour envoyer les messages.
1. Dans la page Configurer les paramètres du système, le nombre de Tentatives de reprises de comptes est de 60 par défaut, et le Délai entre reprises de comptes est de cinq secondes par défaut, ce qui signifie que la remise des messages échouera si le serveur SMTP ne peut pas être atteint dans un délai de cinq minutes. Augmentez ces paramètres pour accroître le délai imparti avant l’échec de la remise du courrier.

    > [!NOTE]
    > Lorsqu'un grand nombre de messages est envoyé, l'utilisation de valeurs par défaut élevées peut améliorer la fiabilité, mais elle entraîne également une augmentation considérable de l'utilisation des ressources pour permettre à la messagerie de base de données d'envoyer à plusieurs reprises ces nombreux messages. Attaquez-vous au problème de base en essayant de résoudre le problème de réseau ou de serveur SMTP qui empêche la messagerie de base de données de contacter rapidement le serveur SMTP.



##  <a name="see-also"></a><a name="RelatedContent"></a> Voir aussi
  
-   [Objets de configuration de la messagerie de base de données](../../relational-databases/database-mail/database-mail-configuration-objects.md)  
  
-   [Objets de messagerie de base de données](../../relational-databases/database-mail/database-mail-messaging-objects.md)  
  
-   [Programme externe de la messagerie de base de données](../../relational-databases/database-mail/database-mail-external-program.md)  
  
-   [Journal et audits de la messagerie de base de données](../../relational-databases/database-mail/database-mail-log-and-audits.md)  
  
-   [Configurer la messagerie SQL Server Agent en vue d’utiliser la messagerie de base de données](../../relational-databases/database-mail/configure-sql-server-agent-mail-to-use-database-mail.md)  
  
  
  
