---
title: Capturer une trace pour les mises à niveau de SQL Server
description: Utilisez Assistant Expérimentation de base de données (DEA) pour créer un fichier de trace contenant un journal des événements de serveur capturés.
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
ms.openlocfilehash: 9bc1dafd5242dc4be8a9a2947465ed48a01fcb8d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100042419"
---
# <a name="capture-a-trace-in-database-experimentation-assistant"></a>Capturer une trace dans Assistant Expérimentation de base de données

Vous pouvez utiliser Assistant Expérimentation de base de données (DEA) pour créer un fichier de trace contenant un journal des événements de serveur capturés. Un événement de serveur capturé est un événement qui se produit sur un serveur spécifique pendant une période de temps spécifique. Une capture de trace doit être exécutée une fois par serveur.

Avant de démarrer une capture de trace, assurez-vous de sauvegarder toutes les bases de données cibles.

La mise en cache des requêtes dans SQL Server peut affecter les résultats de l’évaluation. Nous vous recommandons de redémarrer le service de SQL Server (MSSQLSERVER) dans l’application des services pour améliorer la cohérence des résultats de l’évaluation.

## <a name="configure-a-trace-capture"></a>Configurer une capture de trace

1. Dans la barre de navigation de gauche, sélectionnez l’icône de l’appareil photo, puis dans la page **toutes les captures** , sélectionnez **nouvelle capture**.

    ![Créer une capture dans la DEA](./media/database-experimentation-assistant-capture-trace/dea-initiate-capture.png)

2. Dans la page **nouvelle capture** , sous **Détails** de la capture, entrez ou sélectionnez les informations suivantes :

    - **Nom** de la capture : entrez un nom pour le fichier de trace de votre capture.
    - **Format**: spécifiez le format (trace ou XEvents) pour la capture.
    - **Durée**: sélectionnez la durée (en minutes) pendant laquelle vous souhaitez que la capture de trace s’exécute.
    - **Emplacement de capture**: sélectionnez le chemin d’accès de destination pour le fichier de trace.

        > [!NOTE]
        > Le chemin d’accès au fichier de trace doit se trouver sur l’ordinateur qui exécute SQL Server. Si le service SQL Server n’est pas défini pour un compte spécifique, le service peut avoir besoin d’autorisations en écriture sur le dossier spécifié pour le fichier de trace à écrire.

3. Vérifiez que vous avez effectué une sauvegarde en sélectionnant **Oui, j’ai manuellement suivi la sauvegarde...** .

4. Sous **Détails** de la capture, entrez ou sélectionnez les informations suivantes :

    - **Type de serveur**: spécifiez le type de SQL Server **(SqlServer**, **AzureSqlDb**, **AzureSqlManagedInstance**).
    - **Nom du serveur**: spécifiez le nom du serveur ou l’adresse IP de votre SQL Server.
    - **Type d’authentification**: pour le type d’authentification, sélectionnez **Windows**.
    - **Nom de la base de données**: entrez le nom d’une base de données sur laquelle démarrer une trace de base de données. Si vous ne spécifiez pas de base de données, la trace est capturée sur toutes les bases de données sur le serveur.

5. Activez ou désactivez les cases à cocher **chiffrer la connexion** et **approuver le certificat du serveur** en fonction de votre scénario.

    ![Nouvelle page de capture](./media/database-experimentation-assistant-capture-trace/dea-new-capture.png)

## <a name="start-the-trace-capture"></a>Démarrer la capture de trace

1. Une fois que vous avez entré ou sélectionné les informations requises, sélectionnez **Démarrer** pour lancer la capture de trace.

    Si les informations que vous avez entrées sont valides, le processus de capture de trace commence. Sinon, les zones de texte avec des entrées non valides sont mises en surbrillance en rouge. Si vous rencontrez des erreurs, corrigez les entrées nécessaires, puis sélectionnez **Démarrer** à nouveau.

    Pendant l’exécution de la capture de trace, sous détails de la **capture**, l’État et la progression du processus de capture de trace s’affichent.

    ![Surveiller la progression de la capture](./media/database-experimentation-assistant-capture-trace/dea-capture-running.png)

2. Une fois l’exécution de la capture de trace terminée, le nouveau fichier de trace (. trc) est enregistré dans l' **emplacement de capture** que vous avez spécifique lors de la configuration initiale.

    ![Capture de trace terminée](./media/database-experimentation-assistant-capture-trace/dea-capture-complete.png)

    Le fichier de trace comprend les résultats de trace de l’activité d’une base de données SQL Server. les fichiers. trc sont conçus pour fournir plus d’informations sur les erreurs détectées et signalées par SQL Server.

## <a name="frequently-asked-questions-about-trace-capture"></a>Forum aux questions sur la capture de trace

Voici quelques questions fréquemment posées sur la capture de trace dans la DEA.

**Q : quels événements sont capturés lors de l’exécution d’une capture de trace sur une base de données de production ?**

Le tableau suivant répertorie les événements et les données de colonnes correspondantes que la DEA collecte pour les traces :
  
|Nom de l'événement|Données texte (1)|Données binaires (2)|ID de base de données (3)|Nom d’hôte (8)|Nom de l’application (10)|Nom de connexion (11)|SPID (12)|Heure de début (14)|Heure de fin (15)|Nom de la base de données (35)|Séquence d’événements (51)|IsSystem (60)|  
|---|---|---|---|---|---|---|---|---|---|---|---|---|  
|**RPC : terminé (10)**||*|*|*|*|*|*|*|*|*|*|*|  
|**RPC : démarrage (11)**||*|*|*|*|*|*|*||*|*|*|  
|**Paramètre de sortie RPC (100)**|*||*|*|*|*|*|*||*|*|*|  
|**SQL : BatchCompleted (12)**|*||*|*|*|*|*|*|*|*|*|*|  
|**SQL : BatchStarting (13)**|*||*|*|*|*|*|*||*|*|*|  
|**Auditer la connexion (14)**|*|*|*|*|*|*|*|*||*|*|*|  
|**Audit de déconnexion (15)**|*||*|*|*|*|*|*|*|*|*|*|  
|**ExistingConnection (17)**|*|*|*|*|*|*|*|*||*|*|*|  
|**CursorOpen (53)**|*||*|*|*|*|*|*||*|*|*|  
|**CursorPrepare (70)**|*||*|*|*|*|*|*||*|*|*|  
|**Préparer SQL (71)**|||*|*|*|*|*|*||*|*|*|  
|**Exec Prepared SQL (72)**|||*|*|*|*|*|*||*|*|*|  
|**CursorExecute (74)**|*||*|*|*|*|*|*||*|*|*|  
|**CursorUnprepare (77)**|*||*|*|*|*|*|*||*|*|*|  
|**CursorClose (78)**|*||*|*|*|*|*|*||*|*|*|  

**Q : y a-t-il un effet sur les performances sur mon serveur de production lorsque la capture de trace est en cours d’exécution ?**

Oui, il y a un impact minime sur les performances lors de la collecte des traces. Dans nos tests, nous avons constaté une sollicitation de mémoire de 3%.

**Q : Quels sont les types d’autorisations nécessaires pour capturer des suivis sur une charge de travail de production ?**

- L’utilisateur Windows qui exécute l’opération de suivi dans l’application de DEA doit disposer de droits d’administrateur système sur l’ordinateur qui exécute SQL Server.
- Le compte de service utilisé sur l’ordinateur exécutant SQL Server doit avoir un accès en écriture au chemin d’accès au fichier de trace spécifié.

**Q : puis-je capturer des traces pour l’ensemble du serveur ou uniquement sur une base de données unique ?**

Vous pouvez utiliser la DEA pour capturer les traces de toutes les bases de données du serveur ou d’une base de données unique.

**Q : J’ai un serveur lié configuré dans mon environnement de production. Ces requêtes apparaissent-elles dans les traces ?**

Si vous exécutez une capture de trace pour l’ensemble du serveur, la trace capture toutes les requêtes, y compris les requêtes de serveur lié. Pour exécuter une capture de trace pour l’ensemble du serveur, laissez la zone **nom de la base de données** vide sous **nouvelle capture** vide.

**Q : quelle est la durée minimale recommandée pour les traces de la charge de travail de production ?**

Nous vous recommandons de choisir une heure qui représente le mieux l’intégralité de votre charge de travail. De cette façon, l’analyse s’exécute sur toutes les requêtes dans votre charge de travail.

**Q : quelle est la procédure à suivre pour effectuer une sauvegarde de base de données juste avant de démarrer une capture de trace ?**

Avant de démarrer une capture de trace, assurez-vous de sauvegarder toutes vos bases de données cibles. La trace capturée dans cible 1 et cible 2 est relue. Si l’état de la base de données n’est pas le même, les résultats de l’expérimentation sont faussés.

**Q : puis-je collecter des XEvents au lieu de traces, puis-je relire XEvents ?**

Oui. La DEA prend en charge XEvents. Téléchargez la dernière version de DEA et essayez-la.

## <a name="troubleshoot-trace-captures"></a>Résoudre les problèmes de captures de trace

Si une erreur s’affiche lorsque vous exécutez une capture de trace, vérifiez que :

- Le nom de l’ordinateur exécutant SQL Server est valide. Pour confirmer, essayez de vous connecter à l’ordinateur qui exécute SQL Server à l’aide de SQL Server Management Studio (SSMS).
- La configuration de votre pare-feu ne bloque pas les connexions à l’ordinateur exécutant SQL Server.
- L’utilisateur dispose des autorisations répertoriées dans le [Forum aux questions sur la relecture](./database-experimentation-assistant-replay-trace.md#frequently-asked-questions-about-trace-replay).
- Le nom de la trace ne suit pas la Convention de substitution standard (capture \_ 1). Essayez plutôt les noms de trace comme capture \_ 1a ou Capture1.

Voici quelques-unes des erreurs possibles et des solutions pour les résoudre :

|Erreurs possibles|Solution|  
|---|---|  
|Impossible de démarrer la trace sur le SQL Server cible, de vérifier si vous disposez des autorisations requises et que le compte SQL Server dispose d’un accès en écriture au chemin d’accès du fichier de trace spécifié code d’erreur SQL (53)|L’utilisateur exécutant l’outil DEA doit avoir accès à l’ordinateur exécutant SQL Server. Le rôle sysadmin doit être attribué à l’utilisateur.|  
|Impossible de démarrer la trace sur le SQL Server cible, de vérifier si vous disposez des autorisations requises et que le compte SQL Server dispose d’un accès en écriture au chemin d’accès du fichier de trace spécifié code d’erreur SQL (19062)|Le chemin d’accès de suivi spécifié n’existe peut-être pas ou le dossier ne dispose pas des autorisations d’écriture pour le compte sous lequel les services SQL Server sont en cours d’exécution (par exemple, SERVICE réseau). Le chemin d’accès doit exister et doit avoir les autorisations nécessaires pour démarrer le suivi.|  
|Une trace de DEA est actuellement en cours d’exécution sur le serveur cible.|Une trace active est déjà en cours d’exécution sur le serveur cible. Vous ne pouvez pas démarrer une nouvelle trace lorsqu’une trace au niveau du serveur est déjà en cours d’exécution.|  
|Impossible d’ouvrir la base de données demandée pour la capture de la trace. Cette erreur peut être due à un nom de base de données incorrect.|La base de données spécifiée n’existe pas ou n’est pas accessible à l’utilisateur actuel. Utilisez le nom correct de la base de données.|  

Si vous voyez d’autres erreurs libellées *code d’erreur SQL*, consultez [moteur de base de données des erreurs](../relational-databases/errors-events/database-engine-events-and-errors.md) pour obtenir des descriptions détaillées.

## <a name="see-also"></a>Voir également

- Pour savoir comment configurer les outils de Distributed Replay dans SQL Server avant de relire une trace capturée, consultez [configurer Distributed Replay pour Assistant expérimentation de base de données](database-experimentation-assistant-configure-replay.md).