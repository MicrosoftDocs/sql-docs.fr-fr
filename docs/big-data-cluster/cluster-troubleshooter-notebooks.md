---
title: Résolution des problèmes liés aux Clusters Big Data (BDC) avec des notebooks Jupyter et Azure Data Studio
titleSuffix: SQL Server Big Data Clusters
description: Résolution des problèmes liés au BDC avec des notebooks Jupyter et Azure Data Studio sur le cluster Big Data SQL Server 2019.
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.metadata: seo-lt-2019
ms.date: 09/22/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 550c56a7ceb82b1215e6e921adb68a1438e87464
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048239"
---
# <a name="troubleshooting-big-data-clusters-bdc-with-notebooks"></a>Résolution des problèmes liés aux Clusters Big Data (BDC) avec des notebooks

Cette page est un index des notebooks pour les Clusters Big Data SQL Server. Ces notebooks exécutables (.ipynb) sont conçus pour SQL Server 2019 afin de faciliter la résolution des problèmes liés aux clusters Big Data.

Chaque notebook est conçu pour vérifier ses propres dépendances. Une commande **Exécuter toutes les cellules** s’effectue correctement ou produit une exception avec un conseil en lien hypertexte vers un autre notebook qui va résoudre la dépendance manquante. Suivez le lien hypertexte vers le notebook suivant, appuyez sur **Exécuter toutes les cellules** puis, après une exécution réussie, revenez au notebook d’origine, et appuyez à nouveau sur **Exécuter toutes les cellules**.

Une fois toutes les dépendances installées, mais après l’échec de la commande **Exécuter toutes les cellules**, chaque notebook analyse les résultats et, dans la mesure du possible, produit un conseil de lien hypertexte vers un autre notebook pour faciliter la résolution du problème.


## <a name="troubleshooting-big-data-cluster-bdc"></a>Résolution des problèmes liés au cluster Big Data (BDC)

Cette section contient un ensemble de notebooks permettant d’obtenir des journaux à partir d’un cluster Big Data (BDC) SQL Server.

|Nom |Description |
|---|---|---|---|
|TSG100 - Utilitaire de résolution des problèmes d’un cluster Big Data|Vue d’ensemble de tous les notebooks disponibles sur la résolution des problèmes liés au cluster Big Data (BDC) et le moment de leur utilisation  |
|TSG101 - Utilitaire de résolution des problèmes de SQL Server|Vue d’ensemble de tous les notebooks disponibles sur la résolution des problèmes SQL Server et le moment de leur utilisation  |
|TSG102 - Utilitaire de résolution des problèmes de HDFS|Vue d’ensemble de tous les notebooks disponibles sur la résolution des problèmes HDFS et le moment de leur utilisation  |
|TSG103 - Utilitaire de résolution des problèmes liés à Spark|Vue d’ensemble de tous les notebooks disponibles sur la résolution des problèmes Spark et le moment de leur utilisation  |
|TSG104 - Utilitaire de résolution des problèmes de contrôle|Vue d’ensemble de tous les notebooks disponibles sur la résolution des problèmes du contrôleur et le moment de leur utilisation  |
|TSG105 - Utilitaire de résolution des problèmes de la passerelle|Vue d’ensemble de tous les notebooks disponibles sur la résolution des problèmes de la passerelle Knox et le moment de leur utilisation  |
|TSG106 - Utilitaire de résolution des problèmes d’application|Vue d’ensemble de tous les notebooks disponibles sur la résolution des problèmes de déploiement d’applications et le moment de leur utilisation  |



## <a name="diagnose-issues-from-big-data-clusters-bdc"></a>Diagnostiquer les problèmes à partir de clusters de Big Data (BDC)

Un ensemble de notebooks permettant de diagnostiquer des situations et des états liés à un cluster Big Data.

|Nom |Description |
|---|---|---|---|
|TSG002 - CrashLoopBackoff|Ce TSG se connecte à chaque conteneur dont la dernière tentative d’atteindre l’état « en cours d’exécution » a échoué et qui obtient les journaux de conteneur actuels et précédents. Cela est utile pour déboguer les problèmes CrashLoopBackOff signalés dans les pods get kubectl.|
|TSG025 - Navigateur FSM - État FSM du contrôleur de requêtes|Utilisez ce notebook pour vous connecter à la base de données du contrôleur et parcourir l’état FSM (Finite State Machine). Utilisez ce notebook pour lister les machines à états actifs et identifier les workflows bloqués.|
|TSG026 - Se connecter au nœud du pool de données (pour exécuter T-SQL)|Utiliser ce notebook pour se connecter au nœud du pool de données (pour exécuter T-SQL)|
|TSG027 - Observer le déploiement du cluster|Utilisez ce notebook pour observer le déploiement des clusters, il donne des indications sur la résolution des problèmes de création d’un cluster Big Data SQL Server, les commandes suivantes peuvent être utiles pour identifier les causes sous-jacentes. |
|TSG029 - Rechercher des dumps dans le cluster|Utilisez ce notebook pour observer des coredumps et des minidumps à partir de processus tels que SQL Server ou un contrôleur dans un cluster Big Data.|
|TSG032 - Utilisation du processeur et de la mémoire pour tous les conteneurs|Utilisez ce notebook pour vérifier l’utilisation du processeur et de la mémoire pour tous les conteneurs.|
|TSG037 - Déterminer le réplica principal hébergeant le pod du pool principal|Utilisez ce notebook pour déterminer le réplica principal hébergeant le pod du pool principal pour le Cluster Big Data lorsque la haute disponibilité du pool principal est activée.|
|TSG044 - Exécuter sqlcmd dans le conteneur de pool maître|Utilisez ce notebook pour vous connecter directement à un nœud de pool principal avec T-SQL.|
|TSG055 - Roulement de temps pour Sparkhead|Utilisez ce notebook pour diagnostiquer les étapes permettant de comprendre le temps de réponse du Roulement du pod contrôleur au pod sparkhead.|
|TSG060 - Espace disque du volume persistant pour tous les PVC du BDC|Utilisez ce notebook pour vous connecter à chaque conteneur et obtenir l’espace disque utilisé/disponible pour chaque volume persistant (PV) mappé à chaque revendication de volume persistant (PVC) d’un cluster Big Data (BDC).|
|TSG078 - Le cluster est-il sain ?|Utilisez ce Notebook pour vérifier si votre Cluster Big Data (BDC) est sain.|
|TSG079 - Générer une copie de sauvegarde de base du contrôleur|Utilisez ce notebook pour générer une copie de sauvegarde de base du contrôleur.|
|TSG086 - Exécuter le niveau supérieur de tous les conteneurs|Utilisez ce notebook pour exécuter le niveau supérieur de tous les conteneurs.|
|TSG087 - Utiliser l’interface CLI hadoop fs sur le pod namenode|Utilisez ce notebook pour utiliser l’interface CLI hadoop fs sur le pod namenode.|
|TSG108 - Afficher le ConfigMap de mise à niveau du contrôleur|Utilisez ce notebook pour résoudre la défaillance survenue lors de l’exécution d’une mise à niveau du Cluster Big Data à l’aide de la **mise à niveau azdata BDC**.|
|TSG112 - Vérification de pré-déploiement Active Directory|Utilisez ce notebook pour valider une configuration de Cluster Big Data (BDC) valide pour un déploiement Active Directory (AD).|
|TSG115 - Translator de journaux de sécurité SQL Server sur Linux|Utilisez ce Notebook pour analyser les journaux générés par les enregistreurs d'événements secuirty.ldap et security.kerberos pour SQL Server sur Linux. Pour activer ces enregistreurs d'événements, placez les lignes ci-dessous dans /var/opt/mssql/logger.ini sur l’ordinateur exécutant SQL Server sur Linux. Remarque : ce fichier respecte la casse.|
|TSG116 - Translator de journaux de support de sécurité BDC SQL|Utilisez ce notebook pour analyser les journaux générés par le service de support de sécurité dans le BDC SQL. Pour récupérer les journaux, nous allons copier les journaux de débogage à partir du cluster et les extraire. Suivez les étapes ci-dessous : exécutez « azdata bdc debug copy-logs -n <namespace>  *». Cela créera plusieurs fichiers .tar.gz : extrayez le contenu de debuglogs-* <namespace>-<date>-<time>.tar.gz ; recherchez le journal de support de sécurité stocké à l’emplacement ./<namespace>/control-<... >/security-support/supervisol/log/secsupp-stderr---<... >.log.|
|TSG119 - Vérification de pré-déploiement Active Directory|Ce notebook est conçu pour valider votre configuration BDC après un déploiement AD. Il vérifie l’existence d’entrées DNS pour tous les points de terminaison avec un attribut dnsName et ces entrées DNS doivent être des enregistrements hôtes et non des alias (c’est-à-dire des enregistrements A non CNAME). L’existence de comptes AD connus et leur activation ou non et l’existence des SPN attendus|






## <a name="repair-issues-from-big-data-clusters-bdc"></a>Réparer les problèmes à partir de Clusters Big Data (BDC)

Un ensemble de notebooks destiné à la réparation de situations et d’états connus d’un Cluster Big Data SQL Server.

|Nom |Description |
|---|---|---|---|
|TSG005 - Boucle de transfert détectée|Utilisez ce notebook pour gérer la boucle de transfert détectée, étant donné que l’utilitaire dnsmasq peut placer un bouclage local dans resolv.conf, ce qui peut conduire les pods de contrôleur à entrer dans un CrashLoopBackOff durant le déploiement initial du cluster : https://askubuntu.com/questions/627899/nameserver-127-0-1-1-in-resolv-conf-wont-go-away|
|TSG011 - Redémarrer le serveur sparkhistory|Utilisez ce notebook pour redémarrer le serveur sparkhistory, car le processus Java sparkhistory peut arrêter de répondre pendant le démarrage. Redémarrez le serveur sparkhistory (supervisorctl restart sparkhistory) pour tenter de résoudre ce problème.|
|TSG018 - Tuer le processus sqlservr sur le pool maître| Utilisez ce notebook lorsque l’ARRÊT de T-SQL ne parvient pas à recycler le processus ./sqlservr. Utilisez ce notebook pour tuer le processus sqlservr principal, lequel sera automatiquement redémarré par le processus frontal ./sqlservr.|
|TSG024 - Namenode est en mode sans échec| Utilisez ce notebook quand HDFS s’obtient lui-même en mode sans échec. Par exemple, si trop de pods sont recyclés trop rapidement dans le pool de stockage, le mode sans échec peut être automatiquement activé.|
|TSG028 - Redémarrer le gestionnaire de nœuds sur tous les nœuds du pool de stockage| Utilisez ce notebook lorsqu’il faut redémarrer le Gestionnaire de nœuds sur tous les nœuds du pool de stockage.|
|TSG038 - Échecs de création du BDC en raison de l’absence de clé dans doc| Utilisez ce notebook lors des échecs de création du BDC en raison de l’absence de clé dans doc.|
|TSG039 - Nom d’objet « role_permissions » non valide| Utilisez ce notebook lorsque vous rencontrez un problème d’objet non valide en raison de l’autorisation de rôle dans gateway.log de Knox|
|TSG040 - Échec de l’obtention des noms de fichier à partir du contrôleur avec l’erreur| Utilisez ce notebook lorsque le délai d’expiration de la Passerelle 504 est dépassé lors de l’obtention des noms de fichiers du contrôleur.|
|TSG041 : Impossible de créer un contexte d’E/S asynchrone (augmentez sysctl fs.aio-max-nr)| Utilisez ce notebook lorsqu’il est impossible de créer un contexte d’E/S asynchrone (augmentez sysctl fs.aio-max-nr).|
|TSG045 - Nombre maximal de disques de données autorisés à être attachés à une machine virtuelle de cette taille (AKS)| Utilisez ce notebook lorsqu’un nombre maximal de disques de données autorisés sont attachés à une machine virtuelle de cette taille (AKS).|
|TSG047 - ConfigException - Un seul objet est attendu avec le nom| Utilisez ce notebook quand vous avez des ConfigException qui n’attendent qu’un seul objet avec le nom.|
|TSG048 - Déploiement bloqué « en attente de la création du pod du contrôleur »| Utilisez ce notebook lorsque le déploiement est bloqué sur « en attente de la création du pod du contrôleur ».|
|TSG050 - La création du cluster s’arrête avec le problème « expiration du délai d’attente de l’attachement ou du montage des volumes pour le pod »| Utilisez ce notebook lorsque la création du cluster s’arrête avec le problème « expiration du délai d’attente de l’attachement ou du montage des volumes pour le pod ».|
|TSG052 - Échec de la tentative d’obtention du DNS master-svc, et nouvelle tentative| Utilisez ce notebook lorsque la création du cluster s’arrête avec le problème « expiration du délai d’attente de l’attachement ou du montage des volumes pour le pod ».|
|TSG057 - Échec lors du démarrage du service de contrôleur .System.TimeoutException| Utilisez ce notebook lors du démarrage du service de contrôleur et de l’obtention de System.TimeoutException.|
|TSG067 - Échec de l’installation de la configuration Kube| Utilisez ce notebook lors de l’échec de Terminer l’installation de la configuration Kube.|
|TSG074 - Supprimer les déploiements d’applications| Utilisez ce notebook quand vous rencontrez un problème pour supprimer des applications dans un cluster BDC.|
|TSG075 - FailedCreatePodSandBox en raison de l’échec de configuration du pod par le CNI NetworkPlugin| Utilisez ce notebook lors de l’obtention d’une exception FailedCreatePodSandBox en raison de l’échec du pod de configuration par le cni NetworkPlugin.|
|TSG080 - Supprimer des sessions Spark avec azdata| Utilisez ce notebook lorsque vous rencontrez des problèmes lors de la suppression de sessions Spark.|
|TSG109 - Définir des délais d’attente de mise à niveau| Utilisez ce notebook lorsque vous rencontrez un problème de mise à niveau du BDC.|
|TSG110 - Azdata retourne une erreur ApiError| Utilisez ce notebook quand Azdata retourne ApiError.|

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur les clusters Big Data, consultez [Que sont les [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] ?](big-data-cluster-overview.md).
