---
title: Relire une trace pour les mises à niveau de SQL Server
description: Découvrez comment relire une trace capturée avec Assistant Expérimentation de base de données pour les mises à niveau SQL Server.
ms.custom: seo-lt-2019
ms.date: 12/12/2019
ms.prod: sql
ms.prod_service: dea
ms.suite: sql
ms.technology: dea
ms.tgt_pltfrm: ''
ms.topic: conceptual
author: pochiraju
ms.author: rajpo
ms.reviewer: mathoma
ms.openlocfilehash: 105f471afc10e03c8d6371aa22cce1f6e74ca8c1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100065914"
---
# <a name="replay-a-trace-in-database-experimentation-assistant"></a>Relire une trace dans Assistant Expérimentation de base de données

Dans Assistant Expérimentation de base de données (DEA), vous pouvez relire un fichier de trace capturé sur un environnement de test mis à niveau. Par exemple, considérez une charge de travail de production qui s’exécute sur SQL Server 2008 R2. Le fichier de trace de la charge de travail doit être relu deux fois : une fois dans un environnement avec la même version de SQL Server qui s’exécute en production et une deuxième fois sur un environnement dont la version de la cible de mise à niveau SQL Server, par exemple SQL Server 2016.

> [!NOTE]
> La relecture d’une trace requiert la configuration manuelle des machines virtuelles ou des ordinateurs physiques pour exécuter Distributed Replay traces. Pour plus d’informations, consultez [configurer Distributed Replay pour Assistant expérimentation de base de données](database-experimentation-assistant-configure-replay.md).
>

## <a name="configure-a-trace-replay-for-target-1"></a>Configurer une relecture de trace pour la cible 1

Tout d’abord, vous devez effectuer une relecture de trace sur la cible 1, qui représente votre environnement de production existant.

1. Dans la barre de navigation de gauche de la page de navigation, sélectionnez l’icône représentant une flèche, puis, dans la page **toutes les relectures** , sélectionnez **nouvelle relecture**.

    ![Créer une relecture dans la DEA](./media/database-experimentation-assistant-replay-trace/dea-create-replay.png)

    > [!NOTE]
    > L’ordinateur contrôleur Distributed Replay requiert des autorisations sur le compte d’utilisateur que vous utilisez pour vous connecter à distance.

2. Dans la page **nouvelle relecture** , sous **Détails de la relecture**, entrez ou sélectionnez les informations suivantes :

    - **Nom de la relecture**: entrez un nom pour la relecture de la trace.
    - **Format de trace source**: spécifiez le format (trace ou XEvents) du fichier de trace source.
    - **Chemin d’accès complet au fichier source**: spécifiez le chemin d’accès complet au fichier de trace source. Si vous utilisez DReplay, le fichier doit exister sur l’ordinateur servant de contrôleur DReplay, et le compte d’utilisateur doit accéder au fichier et au dossier.
    - **Outil de relecture**: spécifiez l’outil de relecture (DReplay ou ingénéré).
    - **Nom de l’ordinateur contrôleur**: spécifiez le nom de l’ordinateur servant de contrôleur de Distributed Replay.
    - **Emplacement de la trace de relecture**: spécifiez le chemin d’accès pour stocker les fichiers de trace/XEvents associés à la relecture de la trace.

        > [!NOTE]
        > Pour une Azure SQL Database ou un Managed Instance SQL Azure, vous devez fournir l’URI SAS du compte de stockage d’objets BLOB Azure.

3. Vérifiez que vous avez restauré la ou les bases de données en activant la case à cocher **Oui, j’ai restauré manuellement la ou les bases de données** .

4. Sous **SQL Server les détails** de la connexion, entrez ou sélectionnez les informations suivantes :

    - **Type de serveur**: spécifiez le type de SQL Server **(SqlServer**, **AzureSqlDb**, **AzureSqlManagedInstance**).
    - **Nom du serveur**: spécifiez le nom du serveur ou l’adresse IP de votre SQL Server.
    - **Type d’authentification**: pour le type d’authentification, sélectionnez **Windows**.
    - **Nom de la base de données**: entrez le nom d’une base de données sur laquelle démarrer une trace côté serveur. Si vous ne spécifiez pas de base de données, la trace est capturée sur toutes les bases de données sur le serveur.

5. Activez ou désactivez les cases à cocher **chiffrer la connexion** et **approuver le certificat du serveur** en fonction de votre scénario.

    ![Nouvelle page de relecture](./media/database-experimentation-assistant-replay-trace/dea-new-replay.png)

## <a name="start-the-trace-replay-on-target-1"></a>Démarrer la relecture de la trace sur la cible 1

- Une fois que vous avez entré ou sélectionné les informations requises, sélectionnez **Démarrer** pour lancer la relecture de la trace.

  Si les informations que vous avez entrées sont valides, le processus de Distributed Replay démarre. Dans le cas contraire, les zones de texte qui contiennent des informations incorrectes sont mises en surbrillance en rouge. Assurez-vous que les valeurs entrées sont correctes, puis sélectionnez **Démarrer**.

  ![Relire la progression par rapport à la cible 1](./media/database-experimentation-assistant-replay-trace/dea-run-replay-target1.png)

  Vous pouvez surveiller le processus en fonction des besoins. Une fois l’exécution de la relecture terminée, la DEA stocke les résultats dans un fichier à l’emplacement que vous avez spécifié.

  ![Relecture sur la cible 1 terminée](./media/database-experimentation-assistant-replay-trace/dea-replay-target1-complete.png)

## <a name="perform-the-trace-replay-against-target-2"></a>Effectuer la relecture de trace sur la cible 2

Une fois que vous avez terminé l’exécution de la relecture de trace sur la cible 1, vous devez faire de même avec votre deuxième cible, qui représente l’environnement de mise à niveau prévu.

1. Configurez une relecture de trace en utilisant cette fois les détails associés à votre environnement cible 2.
2. Démarrer la relecture de la trace sur la cible 2.

   Vous pouvez surveiller le processus en fonction des besoins. Une fois l’exécution de la relecture terminée, la DEA stocke les résultats dans un fichier à l’emplacement que vous avez spécifié.

## <a name="frequently-asked-questions-about-trace-replay"></a>Forum aux questions sur la relecture de trace

**Q : Quelles sont les autorisations de sécurité nécessaires pour démarrer une capture de relecture sur mon serveur cible ?**

- L’utilisateur Windows qui exécute l’opération de suivi dans l’application de DEA doit disposer de droits d’administrateur système sur l’ordinateur cible qui exécute SQL Server. Ces droits d’utilisateur sont nécessaires pour démarrer une trace.
- Le compte de service sous lequel l’ordinateur cible exécutant SQL Server s’exécute doit disposer d’un accès en écriture au chemin d’accès au fichier de trace spécifié.
- Le compte de service sous lequel s’exécutent les services du client Distributed Replay doit disposer de droits d’utilisateur pour se connecter à l’ordinateur cible exécutant SQL Server et pour exécuter des requêtes.

**Q : puis-je démarrer plusieurs relectures au cours d’une même session ?**

Oui, vous pouvez démarrer plusieurs relectures et effectuer leur suivi jusqu’à la fin de la même session.

**Q : puis-je démarrer plusieurs relectures en parallèle ?**

Oui, mais pas avec le même ensemble d’ordinateurs sélectionnés dans le **contrôleur et les clients**. Le contrôleur et les clients seront occupés. Configurez un ensemble distinct d’ordinateurs sous le **contrôleur et le client** pour démarrer une relecture parallèle.

**Q : combien de temps une relecture prend-elle généralement pour se terminer ?**

Une relecture prend généralement le même temps que la trace source, plus la durée nécessaire pour prétraiter la trace source. Toutefois, si les ordinateurs clients inscrits auprès du contrôleur ne sont pas suffisants pour gérer la charge produite à partir de la relecture, la relecture peut prendre plus de temps. Vous pouvez inscrire jusqu’à 16 ordinateurs clients auprès du contrôleur.

**Q : quelle est la taille des fichiers de trace cibles ?**

La taille des fichiers de trace cibles peut être comprise entre 5 et 15 fois la taille de la trace source. La taille du fichier est basée sur le nombre de requêtes exécutées. Par exemple, les objets BLOB de plan de requête peuvent être volumineux. Si les statistiques de ces requêtes changent souvent, davantage d’événements sont capturés.

**Q : Pourquoi dois-je restaurer les bases de données ?**

SQL Server est un système de gestion de base de données relationnelle avec état. Pour exécuter correctement un test A/B, l’état de la base de données doit être conservé à tout moment. Dans le cas contraire, vous risquez de voir des erreurs dans les requêtes pendant la relecture qui n’apparaîtront pas en production. Pour éviter ces erreurs, nous vous recommandons d’effectuer une sauvegarde juste avant la capture source. De même, la restauration de la sauvegarde sur l’ordinateur cible qui exécute SQL Server est nécessaire pour éviter les erreurs lors de la relecture.

**Q : que signifie « Pass% » sur la page de relecture ?**

**Pass%** signifie que seul un pourcentage de requêtes a réussi. Vous pouvez diagnostiquer si le nombre d’erreurs est attendu. Les erreurs peuvent être attendues, ou les erreurs peuvent se produire parce que la base de données a perdu son intégrité. Si la valeur de la **passe%** n’est pas celle attendue, vous pouvez arrêter la trace et examiner le fichier de trace dans le générateur de profils SQL pour voir les requêtes qui n’ont pas réussi.

**Q : Comment puis-je examiner les événements de trace qui ont été collectés lors de la relecture ?**

Ouvrez un fichier de trace cible et affichez-le dans le générateur de profils SQL. Ou, si vous souhaitez apporter des modifications à la capture de relecture, tous les scripts de SQL Server sont disponibles dans C : \\ Program Files (x86) \\ Microsoft Corporation \\ Assistant expérimentation de base de données \\ scripts \\ StartReplayCapture. Sql.

**Q : quels événements de trace sont collectés par la DEA pendant la relecture ?**

La DEA capture les événements de trace qui contiennent des informations relatives aux performances. La configuration de capture se trouve dans le script StartReplayCaptureTrace. Sql. Ces événements sont typiques SQL Server événements de trace répertoriés dans la [documentation de référence sp_trace_setevent (Transact-SQL)](../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md).

## <a name="troubleshoot-trace-replay"></a>Résoudre les problèmes de relecture de trace

**Q : pourquoi ne puis-je pas me connecter à l’ordinateur qui exécute SQL Server ?**

- Confirmez que le nom de l’ordinateur exécutant SQL Server est valide. Pour confirmer, essayez de vous connecter au serveur à l’aide de SQL Server Management Studio (SSMS).
- Vérifiez que la configuration du pare-feu ne bloque pas les connexions à l’ordinateur exécutant SQL Server.
- Confirmez que l’utilisateur dispose des droits d’utilisateur requis.
- Vérifiez que le compte de service du client Distributed Replay a accès à l’ordinateur exécutant SQL Server.

Vous pouvez obtenir plus de détails dans les journaux de% temp% \\ DEA. Si le problème persiste, contactez l’équipe du produit.

**Q : pourquoi ne puis-je pas me connecter au contrôleur Distributed Replay ?**

- Vérifiez que le service du contrôleur de Distributed Replay est en cours d’exécution sur l’ordinateur contrôleur. Pour vérifier, utilisez les outils de gestion Distributed Replay (exécutez la commande `dreplay.exe status -f 1` ).
- Si la relecture est démarrée à distance :
  - Vérifiez que l’ordinateur qui exécute la DEA peut effectuer un test ping sur le contrôleur. Vérifiez que les paramètres du pare-feu autorisent les connexions en suivant les instructions de la page **configurer l’environnement de relecture** . Pour plus d’informations, consultez l’article [SQL Server Distributed Replay](../tools/distributed-replay/sql-server-distributed-replay.md).
  - Assurez-vous que le lancement à distance DCOM et l’activation à distance sont autorisés pour l’utilisateur du contrôleur de Distributed Replay.
  - Assurez-vous que les droits d’utilisateur de l’accès à distance DCOM sont autorisés pour l’utilisateur du contrôleur de Distributed Replay.

**Q : le chemin d’accès au fichier de trace existe sur mon ordinateur. Pourquoi Distributed Replay contrôleur ne peut-il pas le Rechercher ?**

Distributed Replay pouvez accéder uniquement aux ressources de disque local. Vous devez copier les fichiers de trace source sur l’ordinateur du contrôleur de Distributed Replay avant de démarrer la relecture. Vous devez également fournir le chemin d’accès sur la page **nouvelle relecture** de la DEA.

Les chemins d’accès UNC ne sont pas compatibles avec Distributed Replay. Distributed Replay chemins d’accès doivent être des chemins d’accès locaux et absolus au premier fichier de trace source, y compris l’extension.

**Q : pourquoi ne puis-je pas Rechercher des fichiers sur la nouvelle page de relecture ?**

Étant donné que nous ne pouvons pas parcourir les dossiers sur un ordinateur distant, l’exploration des fichiers n’est pas utile. Il est plus efficace de copier et coller les chemins absolus.

**Q : J’ai commencé à relire avec une trace, mais Distributed Replay n’a pas relu les événements. Pourquoi?**

Ce problème peut se produire si le fichier de trace n’a pas d’événements pouvant être relus ou si contient des informations sur la façon de relire les événements. Vérifiez si le chemin d’accès au fichier de suivi fourni pointe vers un fichier de trace source. Le fichier de trace source est créé à l’aide de la configuration fournie dans le script StartCaptureTrace. Sql.

**Q : le message « une erreur inattendue s’est produite ! » s’affiche lorsque j’essaie de prétraiter mes fichiers de trace à l’aide du contrôleur SQL Server 2017 Distributed Replay. Pourquoi?**

Ce problème est connu dans la version RTM de SQL Server 2017. Pour plus d’informations, consultez [erreur inattendue lorsque vous utilisez la fonctionnalité DReplay pour relire une trace capturée dans SQL Server 2017](https://support.microsoft.com/help/4045678/fix-unexpected-error-when-you-use-the-dreplay-feature-to-replay-a).  
  
Le problème a été résolu dans la dernière mise à jour cumulative 1 pour SQL Server 2017. Téléchargez la dernière version de la [mise à jour cumulative 1 pour SQL Server 2017](https://support.microsoft.com/help/4038634/cumulative-update-1-for-sql-server-2017).

## <a name="see-also"></a>Voir également

- Pour créer un rapport d’analyse qui vous aide à obtenir des informations sur les modifications proposées, consultez [créer des rapports](database-experimentation-assistant-create-report.md).
