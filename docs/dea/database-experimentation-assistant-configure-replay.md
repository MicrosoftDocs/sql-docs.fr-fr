---
title: Configurer la relecture pour les mises à niveau SQL Server
description: Utilisez Assistant Expérimentation de base de données (DEA) pour accéder aux outils de Distributed Replay. Utilisez les outils pour relire une trace capturée sur un environnement de test mis à niveau.
ms.custom: seo-lt-2019
ms.date: 01/24/2020
ms.prod: sql
ms.prod_service: dea
ms.suite: sql
ms.technology: dea
ms.tgt_pltfrm: ''
ms.topic: conceptual
author: pochiraju
ms.author: rajpo
ms.reviewer: mathoma
ms.openlocfilehash: 18428e87d1c769d10a4d1acbf0033c739db09d0d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100017059"
---
# <a name="configure-distributed-replay-for-database-experimentation-assistant"></a>Configurer Distributed Replay pour Assistant Expérimentation de base de données

Assistant Expérimentation de base de données (DEA) utilise les outils de Distributed Replay de l’installation SQL Server pour relire une trace capturée sur un environnement de test mis à niveau. Nous vous recommandons d’effectuer une série de tests à l’aide d’un petit fichier de trace avant d’effectuer une relecture complète afin de garantir la relecture correcte des requêtes.

## <a name="distributed-replay-requirements"></a>Configuration requise pour Distributed Replay

- 78% supplémentaires d’espace disque dur sont nécessaires pour créer des fichiers IRF sur l’ordinateur contrôleur Distributed Replay.
- 200 Mo ou 512 Mo correspond à la taille de survol de trace idéale pour capturer les traces de production ou de performances.
- La configuration minimale requise pour le processeur et la mémoire RAM pour le contrôleur Distributed Replay et les ordinateurs clients est un processeur à cœur unique avec 3,5 Go de RAM.
- Le temps de relecture prend environ 1,55 fois plus de temps que l’heure de la capture, car un contrôleur et quatre machines enfants sont utilisés pour relire la trace de production.
- Si vous utilisez nos versions « publiées » des fichiers de définition de trace de production et de performances et que la définition de trace de performances filtre les traces pour une base de données d’intérêt, l’analyse indique que la taille de la **trace de performances** est environ 15 fois supérieure à la taille de **trace de production** .

## <a name="set-up-a-virtual-network-or-domain"></a>Configurer un réseau virtuel ou un domaine

Distributed Replay vous oblige à utiliser des comptes communs entre les machines. En raison de cette exigence et, pour des raisons de sécurité, nous vous recommandons d’exécuter Distributed Replay sur un réseau virtuel ou sur un réseau contrôlé par domaine :

- Créez le contrôleur et les ordinateurs clients dans l’environnement.
- Assurez-vous que le contrôleur et les ordinateurs clients peuvent effectuer un test ping entre eux sur le réseau.
- Distributed Replay ordinateurs clients doivent disposer d’une connectivité à l’ordinateur cible de relecture exécutant SQL Server.

## <a name="set-up-the-controller-service"></a>Configurer le service de contrôleur

Pour configurer le service de contrôleur :

1. Installez le contrôleur Distributed Replay à l’aide du programme d’installation de SQL Server. Si vous avez ignoré l’étape de l’Assistant SQL Server installer qui configure le contrôleur de Distributed Replay, vous pouvez configurer le contrôleur par le biais du fichier de configuration. Dans une installation par défaut, le fichier de configuration se trouve dans C:\Program Files (x86) \Microsoft SQL Server \<version\>\Tools\DReplayController\DReplayController.config.
2. Distributed Replay journaux du contrôleur se trouvent dans C:\Program Files (x86) \Microsoft SQL Server \<version\> \Tools\DReplayController\Log.
3. Ouvrez services. msc et accédez au service de **contrôleur SQL Server Distributed Replay** .
4. Cliquez avec le bouton droit sur le service, puis sélectionnez **Propriétés**. Définissez le compte de service sur un compte commun au contrôleur et aux ordinateurs clients du réseau.
5. Sélectionnez **OK** pour fermer la fenêtre **Propriétés** .
6. Redémarrez le service de **contrôleur SQL Server Distributed Replay** à partir de services. msc. Vous pouvez également exécuter les commandes suivantes sur la ligne de commande pour redémarrer le service :

   `NET STOP "SQL Server Distributed Replay Controller"`</br>
   `NET START "SQL Server Distributed Replay Controller"`

Pour obtenir d’autres options de configuration, consultez [configurer Distributed Replay](../tools/distributed-replay/configure-distributed-replay.md).

## <a name="configure-dcom"></a>Configurer DCOM

Cette configuration n’est requise que sur l’ordinateur contrôleur.

1. Ouvrez dcomcnfg.exe.
2. Développez **services de composants**  >  **ordinateurs**  >  **poste de travail**  >  **configuration DCOM**.
3. Sous **configuration DCOM**, cliquez avec le bouton droit sur **DReplayController**, puis sélectionnez **Propriétés**.
4. Sélectionnez l'onglet **Sécurité** .
5. Sous **autorisations d’exécution et d’activation**, sélectionnez **personnaliser**, puis sélectionnez **modifier**.
6. Ajoutez l’utilisateur qui va démarrer la relecture. Accordez les autorisations de lancement local et d’activation locale à l’utilisateur. Si l’utilisateur envisage de lancer ou d’activer à distance, accordez à l’utilisateur un lancement à distance et des autorisations d’activation à distance.
7. Sélectionnez **OK** pour valider les modifications et revenir à l’onglet **sécurité** .
8. Sous **autorisations d’accès**, sélectionnez **personnaliser**, puis sélectionnez **modifier**.
9. Ajoutez l’utilisateur qui va démarrer la relecture. Accordez des autorisations d’accès local à l’utilisateur. Si l’utilisateur envisage d’accéder au service de contrôleur à distance, accordez à l’utilisateur des autorisations d’accès à distance.
10. Sélectionnez **OK** pour valider les modifications et revenir à l’onglet **sécurité** .
11. Sélectionnez **OK** pour valider les modifications.
12. Redémarrez le service de contrôleur SQL Server Distributed Replay à partir de services. msc. Vous pouvez également exécuter les commandes suivantes sur la ligne de commande pour redémarrer le service :

    `NET STOP "SQL Server Distributed Replay Controller"`</br>
    `NET START "SQL Server Distributed Replay Controller"`

## <a name="set-up-the-client-service"></a>Configurer le service client

Avant de configurer le service client, utilisez des outils de mise en réseau tels que ping pour vérifier que le contrôleur et les ordinateurs clients peuvent communiquer.

1. Installez le client Distributed Replay à l’aide du programme d’installation de SQL Server.
2. Ouvrez services. msc et accédez au service client SQL Server Distributed Replay.
3. Cliquez avec le bouton droit sur le service, puis sélectionnez **Propriétés**. Définissez le compte de service sur un compte commun au contrôleur et aux ordinateurs clients du réseau.
4. Sélectionnez **OK** pour fermer la fenêtre **Propriétés** . Si vous avez ignoré l’étape SQL Server Assistant Installation pour configurer le client Distributed Replay, vous pouvez le configurer à l’aide du fichier de configuration. Dans une installation par défaut, le fichier de configuration se trouve dans C:\Program Files (x86) \Microsoft SQL Server \<version\>\Tools\DReplayClient\DReplayClient.config.
5. Assurez-vous que le fichier DReplayClient.config contient le nom de l’ordinateur contrôleur en tant que contrôleur pour l’inscription.
6. Redémarrez le service client SQL Server Distributed Replay à partir de services. msc. Vous pouvez également exécuter les commandes suivantes à partir de la ligne de commande pour redémarrer le service :

    `NET STOP "SQL Server Distributed Replay Client"`</br>
    `NET START "SQL Server Distributed Replay Client"`

    Distributed Replay journaux du contrôleur se trouvent dans C:\Program Files (x86) \Microsoft SQL Server \<version\> \Tools\DReplayClient\Log. Les journaux indiquent si le client peut s’inscrire auprès du contrôleur.

    Si la configuration est réussie, le journal affiche le message **enregistré avec le contrôleur <le \> nom du contrôleur**.

Pour obtenir d’autres options de configuration, consultez [configurer Distributed Replay](../tools/distributed-replay/configure-distributed-replay.md).

## <a name="set-up-distributed-replay-administration-tools"></a>Configurer les outils d’administration Distributed Replay

Vous pouvez utiliser des outils d’administration Distributed Replay pour tester rapidement si Distributed Replay fonctionne correctement dans l’environnement. Le test de la configuration peut être particulièrement utile dans un environnement dans lequel plusieurs ordinateurs clients sont inscrits auprès d’un contrôleur. Vous devrez peut-être installer SQL Server Management Studio (SSMS) pour accéder aux outils d’administration.

1. Accédez à l’emplacement d’installation de SSMS et recherchez l’outil d’administration Distributed Replay dreplay.exe et ses composants dépendants. Actuellement, [SSMS 17](../ssms/release-notes-ssms.md#1791) est la dernière version de SSMS à inclure dreplay.exe.
2. À l’invite de commandes, exécutez `dreplay.exe status -f 1` .

Si les étapes précédentes ont réussi, la sortie de la console indique que le contrôleur peut voir ses clients dans un `READY` État.

## <a name="configure-the-firewall-for-remote-distributed-replay-access"></a>Configurer le pare-feu pour l’accès Distributed Replay à distance

L’accès à distance à Distributed Replay nécessite l’ouverture de ports visibles dans le domaine ou le réseau virtuel.

1. Ouvrez **pare-feu Windows** avec **fonctions avancées de sécurité**.
2. Accédez à **règles de trafic entrant**.
3. Créez une règle de pare-feu entrante pour le programme C:\Program Files (x86) \Microsoft SQL Server \<version\>\Tools\DReplayController\DReplayController.exe.
4. Autorisez l’accès au niveau du domaine à tous les ports pour DReplayController.exe être en mesure de communiquer avec le service de contrôleur à distance.
5. Enregistrez la règle.

## <a name="set-up-target-computers"></a>Configurer des ordinateurs cibles

Deux relectures sont nécessaires pour exécuter un test A/B ou une expérience. Autrement dit, vous aurez peut-être besoin de deux instances distinctes de SQL Server installations pour un scénario de migration.

Vous pouvez également installer les deux versions de SQL Server instances sur le même ordinateur. L’inconvénient est de s’assurer que les instances sont isolées lorsqu’une relecture est en cours.

Les étapes suivantes doivent être effectuées pour chaque relecture :

1. Restaurez la sauvegarde de la base de données.
2. Accordez des autorisations à l’utilisateur du compte de service client pour accéder aux bases de données sous l’instance SQL Server. Des autorisations sont requises pour que les requêtes soient exécutées sur l’instance SQL Server.
3. Démarrer la relecture.

## <a name="see-also"></a>Voir également

- Pour savoir comment relire une trace capturée dans un environnement de test mis à niveau, consultez [relire une trace dans Assistant expérimentation de base de données](database-experimentation-assistant-replay-trace.md).
