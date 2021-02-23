---
title: Notes de publication des clusters Big Data SQL Server
titleSuffix: SQL Server big data clusters
description: Cet article décrit les dernières mises à jour et les problèmes connus relatifs aux clusters Big Data SQL Server.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.date: 02/11/2021
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 1f3e1e0ed29121f0fb0ffcac54885ca80de3e63c
ms.sourcegitcommit: e8c0c04eb7009a50cbd3e649c9e1b4365e8994eb
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/14/2021
ms.locfileid: "100489303"
---
# <a name="sql-server-2019-big-data-clusters-release-notes"></a>Notes de publication des clusters Big Data SQL Server 2019

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Les notes de publication suivantes s’appliquent à [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]. Cet article est divisé en sections pour chaque mise en production. Chaque version a un lien vers un article de support décrivant les modifications CU, ainsi que des liens de téléchargement des packages Linux. L’article répertorie également les [problèmes connus](#known-issues) pour les versions les plus récentes de [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] (BDC).

## <a name="supported-platforms"></a>Plateformes prises en charge

Cette section décrit les plateformes qui sont prises en charge avec BDC.

### <a name="kubernetes-platforms"></a>Plateformes Kubernetes

|Plateforme|Versions prises en charge|
|---------|---------|
|Vanilla (amont) Kubernetes|Déployez BDC localement à l’aide d’une version minimale 1.13 du cluster Kubernetes. Voir [Stratégie de prise en charge des versions et des différences de version Kubernetes](https://kubernetes.io/docs/setup/release/version-skew-policy/).|
|Red Hat OpenShift|Déployez BDC localement à l’aide d’une version minimale 4.3 du cluster OpenShift. Voir [Stratégie du cycle de vie Red Hat OpenShift Container Platform](https://access.redhat.com/support/policy/updates/openshift).<br><br> Prise en charge introduite dans SQL Server 2019 CU5.|
|Azure Kubernetes Service (AKS)|Déployez BDC sur la version minimale 1.13 du cluster AKS.<br/>Consultez [Versions Kubernetes prises en charge dans AKS](/azure/aks/supported-kubernetes-versions) pour découvrir la stratégie de prise en charge des versions.|
|Azure Red Hat OpenShift (ARO)|Déployez BDC sur la version minimale 4.3 d’ARO. Voir [Azure Red Hat OpenShift](/azure/openshift/). <br><br> Prise en charge introduite dans SQL Server 2019 CU5.|

### <a name="host-os-for-kubernetes"></a>Système d’exploitation hôte pour Kubernetes

|Plateforme|Système d’exploitation hôte|Versions prises en charge|
|---------|---------|---------|
|Kubernetes|Ubuntu|16.04|
|Kubernetes|Red Hat Enterprise Linux|7.3, 7.4, 7.5, 7.6|
|OpenShift|Red Hat Enterprise Linux / CoreOS |Voir [Notes de publication OpenShift](https://docs.openshift.com/container-platform/4.3/release_notes/ocp-4-3-release-notes.html#ocp-4-3-about-this-release)|

### <a name="sql-server-editions"></a>Éditions de SQL Server

|Édition|Notes|
|---------|---------|
|Entreprise<br/>standard<br/>Développeur| L’édition du cluster Big Data est déterminée par l’édition de l’instance principale SQL Server. Au moment du déploiement, l’édition Développeur est déployée par défaut. Vous pouvez changer l’édition après le déploiement. Consultez [Configurer l’instance principale SQL Server](./configure-sql-server-master-instance.md). |

## <a name="tools"></a>Outils

|Plateforme|Versions prises en charge|
|---------|---------|
|[!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]|Nous recommandons d’utiliser la dernière version disponible. À compter de SQL Server 2019 CU5, [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] inclut une version sémantique indépendante du serveur. <br/><br/>Exécutez `azdata –-version` pour valider la version.<br/><br/>Pour connaître la dernière version, consultez l’[Historique des versions](#release-history).|
|Azure Data Studio|Procurez-vous la dernière version d’[Azure Data Studio](../azure-data-studio/download-azure-data-studio.md).|

Pour obtenir la liste complète, consultez [Quels sont les outils requis ?](deploy-big-data-tools.md#which-tools-are-required)

## <a name="release-history"></a>Historique des mises en production

La table suivante énumère l’historique des mises en production pour [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)].

| Version <sup>1</sup> | Version BDC | [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] version <sup>2</sup> | Date de publication |
|--|--|--|--|
| [CU9](#cu9) |  15.0.4102.2 | 20.3.0    | 2021-02-11 |
| [CU8-GDR](#cu8-gdr) | 15.0.4083.2  | 20.2.6    | 2021-01-12 |
| [CU8](#cu8)     | 15.0.4073.23 | 20.2.2    | 2020-10-19 |
| [CU6](#cu6)     | 15.0.4053.23 | 20.0.1    | 2020-08-04 |
| [CU5](#cu5)     | 15.0.4043.16 | 20.0.0    | 2020-06-22 |
| [CU4](#cu4)     | 15.0.4033.1  | 15.0.4033 | 31-03-2020 |
| [CU3](#cu3)     | 15.0.4023.6  | 15.0.4023 | 2020-03-12 |
| [CU2](#cu2)     | 15.0.4013.40 | 15.0.4013 | 2020-02-13 |
| [CU1](#cu1)     | 15.0.4003.23 | 15.0.4003 | 07-01-2020 |
| [GDR1](#rtm)    | 15.0.2070.34 | 15.0.2070 | 04-11-2019 |

<sup>1</sup> CU7 n'est pas disponible pour BDC.

<sup>2</sup> [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] version reflète la version de l’outil au moment de la mise en production CU. `azdata` peut également être libéré indépendamment de la version du serveur. par conséquent, vous pouvez recevoir des versions plus récentes lorsque vous installez les packages les plus récents. Les versions plus récentes sont compatibles avec les versions de CU précédemment publiées.

## <a name="how-to-install-updates"></a>Comment installer les mises à jour

Pour installer les mises à jour, consultez [Comment mettre à niveau [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](deployment-upgrade.md).

## <a name="cu9-february-2021"></a><a id="cu9"></a> CU9 (février 2021)

Mise à jour cumulative 9 (CU9) pour SQL Server 2019.

|Version du package | Balise d'image |
|-----|-----|
|15.0.4102.2|[2019-CU9-ubuntu-16.04]|

La mise à jour cumulative 9 (CU9) de SQL Server 2019 pour les clusters Big Data SQL Server comprend des fonctionnalités importantes :

- Prise en charge de la configuration de clusters Big Data après le déploiement et amélioration de la visibilité des paramètres système.

   Les clusters utilisant `mssql-conf` pour les configurations de l’instance maître de SQL Server nécessitent des étapes supplémentaires après la mise à niveau vers CU9. Suivez les instructions [ici](bdc-upgrade-configuration.md).

- Amélioration de l’expérience [!INCLUDE[azdata](../includes/azure-data-cli-azdata.md)] pour le chiffrement au repos.
- Possibilité d’installer de manière dynamique des packages Python Spark à l’aide d’environnements virtuels.
- Versions logicielles mises à niveau de la plupart de nos composants OSS (Grafana, Kibana, FluentBit, etc.) pour garantir que les images de clusters Big Data sont à jour avec les dernières améliorations et correctifs. Consultez [Informations de référence sur les logiciels Open Source](reference-open-source-software.md).
- Autres améliorations diverses et correctifs de bogues.

## <a name="cu8-gdrjanuary-2021"></a><a id="cu8-gdr"></a> CU8-GDR (janvier 2021)

Version de mise à jour cumulative 8 GDR (CU8-GDR) pour SQL Server 2019.

|Version du package | Balise d'image |
|-----|-----|
|15.0.4083.2 |[2019-CU8-GDR2-ubuntu-16.04]|

## <a name="cu8-september-2020"></a><a id="cu8"></a> CU8 (septembre 2020)

Version de mise à jour cumulative 8 (CU8) pour SQL Server 2019.

|Version du package | Balise d'image |
|-----|-----|
|15.0.4073.23 |[2019-CU8-ubuntu-16.04]

Cette version comprend plusieurs correctifs et quelques améliorations.

### <a name="added-capabilities"></a>Fonctionnalités ajoutées

- [Chiffrement au repos sur des Clusters Big Data SQL Server](encryption-at-rest-concepts-and-configuration.md) à l’aide de clés et de certificats managés par le système.
   > [!CAUTION]
   > Il s’agit de la version initiale du chiffrement au repos BDC SQL Server. Consultez les articles suivants : 
   > - [Concepts de sécurité pour [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](concept-security.md)
   > - [Guide de configuration et de concepts du chiffrement au repos](encryption-at-rest-concepts-and-configuration.md)
- Support de [l’utilisateur du proxy Oracle](tutorial-query-oracle.md) pour le scénario de virtualisation des données.

## <a name="cu6-july-2020"></a><a id="cu6"></a> CU6 (juillet 2020)

Mise à jour cumulative 6 (CU6) pour SQL Server 2019.

|Version du package | Balise d'image |
|-----|-----|
|15.0.4053.23 |[2019-CU6-ubuntu-16.04]

Cette version comprend des correctifs mineurs et des améliorations. Les articles suivants contiennent des informations relatives à ces mises à jour :

- [Gestion de l’accès à un cluster Big Data en mode Active Directory](manage-user-access.md)
- [Déploiement de [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] en mode Active Directory](active-directory-deploy.md)
- [Déploiement de [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] sur AKS en mode Active Directory](active-directory-deployment-aks.md)
- [Déployer des clusters Big Data (BDC) dans un cluster privé Azure Kubernetes Service (AKS)](private-deploy.md)
- [Restreindre le trafic de sortie des clusters Big Data (BDC) dans un cluster privé Azure Kubernetes Service (AKS)](private-restrict-egress-traffic.md)
- [Déployer un cluster Big Data SQL Server avec une haute disponibilité](deployment-high-availability.md)
- [Configurer un cluster Big Data SQL Server](configure-cluster.md)
- [Configurer Apache Spark et Apache Hadoop dans les clusters Big Data](configure-spark-hdfs.md)
- [Propriétés de configuration de l’instance maître SQL Server](reference-config-master-instance.md)
- [Propriétés de configuration d’Apache Spark et Apache Hadoop (HDFS)](reference-config-spark-hadoop.md)
- [Modèle RBAC Kubernetes et impact sur les utilisateurs et comptes de service qui gèrent des clusters Big Data](kubernetes-rbac.md)

## <a name="cu5-june-2020"></a><a id="cu5"></a> CU5 (juin 2020)

Mise à jour cumulative 5 (CU5) pour SQL Server 2019.

|Version du package | Balise d'image |
|-----|-----|
|15.0.4043.16 |[2019-CU5-ubuntu-16.04]

### <a name="added-capabilities"></a>Fonctionnalités ajoutées

- Prise en charge du déploiement de clusters Big Data sur Red Hat OpenShift. La prise en charge inclut au moins la version 4.3 de la plateforme de conteneur OpenShift déployée localement et Azure Red Hat OpenShift. Voir [Déployer des clusters SQL Server Big Data sur OpenShift](deploy-openshift.md)
- Le modèle de sécurité du déploiement BDC a été mis à jour afin que les conteneurs privilégiés déployés dans le cadre de BDC ne soient plus *requis*. Outre les utilisateurs sans privilège, les conteneurs s’exécutent en tant qu’utilisateur non racine par défaut pour tous les nouveaux déploiements avec SQL Server 2019 CU5. 
- Ajout de la prise en charge du déploiement de plusieurs clusters Big Data sur un domaine Active Directory.
- [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] possède sa propre version sémantique, indépendante du serveur. Toute dépendance entre le client et la version serveur d’azdata est supprimée. Nous vous recommandons d’utiliser la version la plus récente du client et du serveur pour vous assurer de bénéficier des dernières améliorations et des derniers correctifs.
- Deux nouvelles procédures stockées, sp_data_source_objects et sp_data_source_table_columns, ont été introduites pour prendre en charge l’introspection de certaines sources de données externes. Elles peuvent être utilisées par les clients directement via T-SQL pour découvrir le schéma et identifier les tables disponibles pour la virtualisation. Nous utilisons ces modifications dans l’Assistant Table externe de l’[extension de virtualisation de données](../azure-data-studio/extensions/data-virtualization-extension.md) pour Azure Data Studio, ce qui vous permet de créer des tables externes à partir de SQL Server, Oracle, MongoDB et Teradata.
- Ajout de la prise en charge de la persistance des personnalisations effectuées dans Grafana. Avant CU5, les clients constataient que les modifications apportées aux configurations Grafana étaient perdues en cas de redémarrage du pod `metricsui` (qui héberge le tableau de bord Grafana). Ce problème est résolu et toutes les configurations sont désormais conservées. 
- Correction d’un problème de sécurité lié à l’API utilisée pour collecter les métriques sur les pods et les nœuds à l’aide de Telegraf (hébergement dans les pods `metricsdc`). Suite à cette modification, Telegraf nécessite désormais un compte de service, un rôle de cluster et des liaisons de cluster afin d’obtenir les autorisations nécessaires pour collecter les métriques sur les pods et les nœuds. Pour plus d’informations, voir [Rôle de cluster nécessaire pour la collecte de métriques sur les pods et les nœuds](kubernetes-rbac.md#cluster-role-required-for-pods-and-nodes-metrics-collection).
- Deux commutateurs de fonctionnalités ont été ajoutés pour contrôler la collecte de métriques sur les pods et les nœuds. Si vous utilisez d’autres solutions pour superviser votre infrastructure Kubernetes, vous pouvez désactiver la collecte de métriques intégrée pour les pods et les nœuds hôtes en définissant *allowNodeMetricsCollection* et *allowPodMetricsCollection* sur false dans le fichier de configuration de déploiement control.json. Pour les environnements OpenShift, ces paramètres sont définis sur false par défaut dans les profils de déploiement intégrés, puisque la collecte des métriques sur les pods et les nœuds requiert des fonctionnalités privilégiées.

## <a name="cu4-april-2020"></a><a id="cu4"></a> CU4 (avril 2020)

Mise à jour cumulative 4 (CU4) pour SQL Server 2019. La version du moteur de base de données SQL Server pour cette version est 15.0.4033.1.

|Version du package | Balise d'image |
|-----|-----|
|15.0.4033.1 |[2019-CU4-ubuntu-16.04]

## <a name="cu3-march-2020"></a><a id="cu3"></a> CU3 (mars 2020)

Mise à jour cumulative 3 (CU3) pour SQL Server 2019. La version du moteur de base de données SQL Server pour cette version est 15.0.4023.6.

|Version du package | Balise d'image |
|-----|-----|
|15.0.4023.6 |[2019-CU3-ubuntu-16.04]

### <a name="resolved-issues"></a>Problèmes résolus

SQL Server 2019 CU3 résout les problèmes suivants des versions précédentes.

- [Déploiement avec référentiel privé](#deployment-with-private-repository)
- [La mise à niveau peut échouer en raison du délai d’expiration](#upgrade-may-fail-due-to-timeout)

## <a name="cu2-february-2020"></a><a id="cu2"></a> CU2 (février 2020)

Mise à jour cumulative 2 (CU2) pour SQL Server 2019. La version du moteur de base de données SQL Server pour cette version est 15.0.4013.40.

|Version du package | Balise d'image |
|-----|-----|
|15.0.4013.40 |[2019-CU2-ubuntu-16.04]

## <a name="cu1-january-2020"></a><a id="cu1"></a> CU1 (Janvier 2020)

Mise à jour cumulative 1 (CU1) pour SQL Server 2019. La version du moteur de base de données SQL Server pour cette version est 15.0.4003.23.

|Version du package | Balise d'image |
|-----|-----|
|15.0.4003.23|[2019-CU1-ubuntu-16.04]

## <a name="gdr1-november-2019"></a><a id="rtm"></a> GDR1 (novembre 2019)

SQL Server 2019 GDR1 (General Distribution Release 1) : offre une disponibilité générale pour [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-nover.md)]. La version du moteur de base de données SQL Server pour cette version est 15.0.2070.34.

|Version du package | Balise d'image |
|-----|-----|
|15.0.2070.34|[2019-GDR1-ubuntu-16.04]

[!INCLUDE [sql-server-servicing-updates-version-15](../includes/sql-server-servicing-updates-version-15.md)]

## <a name="known-issues"></a>Problèmes connus

### <a name="partial-loss-of-logs-collected-in-elasticsearch-upon-rollback"></a>Perte partielle des journaux collectés dans ElasticSearch lors de la restauration

- **Versions concernées** : Clusters existants quand une mise à niveau non réussie vers CU9 aboutit à une restauration ou quand un utilisateur passe à une version antérieure.

- **Problème et impact sur le client** : La version du logiciel utilisée pour ElasticSearch a été mise à niveau vers CU9 et la nouvelle version n’offre pas de compatibilité descendante avec le format ou les métadonnées des journaux précédents. Si le composant ElasticSearch est mis à niveau et qu’une restauration est déclenchée par la suite, les journaux collectés entre la mise à niveau d’ElasticSearch et la restauration sont définitivement perdus. Si vous passez à une version antérieure de BDC (ce qui n’est pas recommandé), les journaux stockés dans ElasticSearch sont perdus. Notez que si l’utilisateur réeffectue la mise à niveau vers CU9, les données sont restaurées.

- **Solution de contournement** : Si nécessaire, vous pouvez résoudre les problèmes à l’aide des journaux collectés avec la commande `azdata bdc debug copy-logs`.

### <a name="missing-pods-and-container-metrics"></a>Métriques sur les pods et les conteneurs manquantes

- **Versions concernées** : Clusters existants et nouveaux clusters lors de la mise à niveau vers CU9

- **Problème et impact sur le client** : À la suite de la mise à niveau de la version de Telegraf utilisée pour les composants de supervision BDC dans CU9, vous notez que les métriques sur les pods et les conteneurs ne sont pas collectées lors de la mise à niveau du cluster vers la version CU9. Cela est dû au fait qu’une ressource supplémentaire est nécessaire dans la définition du rôle de cluster utilisé pour Telegraf à la suite de la mise à niveau du logiciel. Si l’utilisateur qui déploie le cluster ou effectue la mise à niveau ne dispose pas d’autorisations suffisantes, le déploiement ou la mise à niveau continue avec un avertissement et aboutit, mais les métriques sur les pods et les nœuds ne sont pas collectées.

- **Solution de contournement** : Vous pouvez demander à un administrateur de créer ou de mettre à jour le rôle et le compte de service correspondant (avant ou après le déploiement ou la mise à niveau) afin que BDC utilise ces informations. [Cet article](kubernetes-rbac.md#cluster-role-required-for-pods-and-nodes-metrics-collection) explique comment créer les artefacts requis.

### <a name="issuing-azdata-bdc-copy-logs-does-not-result-in-logs-being-copied"></a>L’émission de `azdata bdc copy-logs` n’entraîne pas la copie des journaux

- **Versions concernées** : [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] version *20.0.0*

- **Problème et impact sur le client** : L’implémentation de la commande *copy-logs* suppose l’installation de l’outil client `kubectl` version 1.15 ou ultérieure sur l’ordinateur client à partir duquel la commande est émise. Si vous utilisez `kubectl` version 1.14 ou ultérieure, la commande *azdata bdc debug copy-logs* se termine sans échec, mais les journaux ne sont pas copiés. Si vous exécutez la commande avec l’indicateur *--debug*, cette erreur apparaît dans la sortie : *la source ‘.’ n’est pas valide*.

- **Solution de contournement** : Installez l’outil `kubectl` version 1.15 ou ultérieure sur le même ordinateur client et réexécutez la commande `azdata bdc copy-logs`. Vous trouverez [ici](deploy-big-data-tools.md) des instructions sur l’installation de `kubectl`.

### <a name="msdtc-capabilities-can-not-be-enabled-for-sql-server-master-instance-running-within-bdc"></a>Les fonctionnalités MSDTC ne peuvent pas être activées pour l’instance maître SQL Server s’exécutant dans BDC

- **Versions concernées** : Toutes les configurations de déploiement de cluster Big Data, indépendamment de la version.

- **Problème et impact sur le client** : Quand SQL Server est déployé dans BDC en tant qu’instance maître SQL Server, la fonctionnalité MSDTC ne peut pas être activée. Il n’existe aucune solution de contournement à ce problème.

### <a name="ha-sql-server-database-encryption-key-encryptor-rotation"></a>Rotation du chiffreur de clé de chiffrement de base de données SQL Server HA

- **Versions concernées** : Toutes les versions jusqu’à CU8. Résolu pour CU9.

- **Problème et impact sur le client** : Avec SQL Server déployé avec HA, la rotation de certificat pour la base de données chiffrée échoue. Lorsque la commande suivante est exécutée sur le pool principal, un message d’erreur s’affiche :
    ```
    ALTER DATABASE ENCRYPTION KEY
    ENCRYPTION BY SERVER
    CERTIFICATE <NewCertificateName>
    ```
    Il n’y a aucun impact, la commande échoue et le chiffrement de la base de données cible est conservé à l’aide du certificat précédent.

### <a name="enabling-hdfs-encryption-zones-support-on-cu8"></a>Activation de la prise en charge des zones de chiffrement HDFS sur CU8

- **Versions concernées** : Ce scénario apparaît lors de la mise à niveau vers CU8 à partir de la version CU6 ou d’une version précédente. Il ne se produira pas dans les nouveaux déploiements de CU8+ ou lors de la mise à niveau directe vers CU9.

- **Problème et impact sur le client** : La prise en charge des zones de chiffrement HDFS n’est pas activée par défaut dans ce scénario et doit être configurée à l’aide de la procédure décrite dans le [guide de configuration](encryption-at-rest-concepts-and-configuration.md).

### <a name="empty-livy-jobs-before-you-apply-cumulative-updates"></a>Tâches Livy vides avant d’appliquer les mises à jour cumulatives

- **Versions concernées** : Toutes les versions jusqu’à CU6. Résolu pour CU8.

- **Problème et impact sur le client** : Pendant une mise à niveau, `sparkhead` retourne l’erreur 404.

- **Solution de contournement** : Avant de mettre à niveau le BDC, assurez-vous qu’il n’existe aucune session Livy ou aucun travail de traitement par lots actif. Suivez les instructions de [Mise à niveau à partir d’une version prise en charge](deployment-upgrade.md#upgrade-from-supported-release) pour éviter cela. 

   Si Livy retourne une erreur 404 pendant le processus de mise à niveau, redémarrez le serveur Livy sur les deux nœuds `sparkhead`. Exemple :

   ```console
   kubectl -n <clustername> exec -it sparkhead-0/sparkhead-1 -c hadoop-livy-sparkhistory -- exec supervisorctl restart livy
   ```

### <a name="big-data-cluster-generated-service-accounts-passwords-expiration"></a>Expiration des mots de passe des comptes de service générés par le cluster Big Data

- **Versions concernées** : Tous les déploiements de cluster Big Data avec intégration Active Directory, quelle que soit la version

- **Problème et impact sur le client** : Lors du déploiement d’un cluster Big Data, le workflow génère un ensemble de [comptes de service](active-directory-objects.md). Selon la stratégie d’expiration de mot de passe définie dans le contrôleur de domaine, les mots de passe de ces comptes peuvent expirer (après 42 jours, par défaut). Pour l’instant, il n’existe aucun mécanisme permettant de faire pivoter les informations d’identification de tous les comptes dans le BDC, ainsi le cluster ne pourra plus fonctionner une fois la période d’expiration atteinte.

- **Solution de contournement** : Mettez à jour la stratégie d’expiration pour les comptes de service BDC sur « Le mot de passe n’expire jamais » dans le contrôleur de domaine. Pour obtenir la liste complète de ces comptes, consultez [Objets Active Directory générés automatiquement](active-directory-objects.md). Cette action peut être effectuée avant ou après l’heure d’expiration. Dans ce dernier cas, Active Directory réactivera les mots de passe arrivés à expiration.

### <a name="credentials-for-accessing-services-through-gateway-endpoint"></a>Informations d’identification pour l’accès aux services via le point de terminaison de passerelle

- **Versions concernées** : Nouveaux clusters déployés à partir de CU5.

- **Problème et impact sur le client** : Pour les nouveaux clusters Big Data déployés avec SQL Server 2019 CU5, le nom d’utilisateur de la passerelle n’est pas **racine**. Si l’application permettant de se connecter au point de terminaison de la passerelle utilise des informations d’identification incorrectes, une erreur d’authentification se produit. Ce changement résulte de l’exécution d’applications dans le cluster Big Data en tant qu’utilisateur non racine (un nouveau comportement par défaut à partir de SQL Server 2019 CU5, lorsque vous déployez un nouveau cluster Big Data à l’aide de CU5, le nom d’utilisateur du point de terminaison de la passerelle est basé sur la valeur transmise via la variable d'environnement **AZDATA_USERNAME**. Il s’agit du même nom d’utilisateur que pour le contrôleur et les points de terminaison SQL Server. Seuls les nouveaux déploiements sont affectés ; les clusters Big Data existants déployés avec les versions précédentes continueront d’utiliser **racine**. Il n’y a aucun impact sur les informations d’identification si le cluster est déployé de façon à appliquer l’authentification Active Directory. 

- **Solution de contournement** : Azure Data Studio gérera la modification des informations d’identification de manière transparente pour la connexion établie via la passerelle afin d’activer l’expérience de navigation HDFS dans l’Explorateur d’objets. Vous devez installer la [dernière version d’Azure Data Studio](../azure-data-studio/download-azure-data-studio.md) qui comprend les modifications nécessaires pour ce cas d’utilisation.
En ce qui concerne les autres scénarios dans lesquels vous devez fournir des informations d’identification pour accéder au service via la passerelle (par exemple, connexion avec [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] ou accès aux tableaux de bord web pour Spark), vous devez vérifier que les informations d’identification utilisées sont correctes. Si vous ciblez un cluster existant déployé avant CU5, continuez d’utiliser le nom d’utilisateur **root** pour vous connecter à la passerelle, même après la mise à niveau du cluster vers CU5. Si vous déployez un nouveau cluster à l’aide de la build CU5, connectez-vous en fournissant le nom d’utilisateur correspondant à la variable d’environnement **AZDATA_USERNAME**.

### <a name="pods-and-nodes-metrics-not-being-collected"></a>Métriques sur les pods et sur les nœuds non collectées

- **Versions concernées** : Clusters nouveaux et existants utilisant des images CU5

- **Problème et impact sur le client** : À la suite d’un correctif de sécurité lié à l’API utilisée par `telegraf` pour collecter les métriques sur les pods et sur les nœuds, les clients constataient parfois que les métriques n’étaient pas collectées. Cela est possible dans les déploiements nouveaux et existants de BDC (après la mise à niveau avec CU5). Grâce à ce correctif, Telegraf nécessite désormais un compte de service avec des autorisations de rôle pour l’ensemble du cluster. Le déploiement tente de créer le compte de service et le rôle de cluster nécessaires. Cependant, même si l’utilisateur qui déploie le cluster ou effectue la mise à niveau ne dispose pas d’autorisations suffisantes, le déploiement ou la mise à niveau continuera avec un avertissement et aboutira, mais les métriques sur les pods et les nœuds ne seront pas collectées.

- **Solution de contournement** : Vous pouvez demander à un administrateur créer le rôle et le compte de service (avant ou après le déploiement ou la mise à niveau) afin que BDC utilise ces informations. [Cet article](kubernetes-rbac.md#cluster-role-required-for-pods-and-nodes-metrics-collection) explique comment créer les artefacts requis.

### <a name="azdata-bdc-copy-logs-command-failure"></a>Échec de la commande `azdata bdc copy-logs`

- **Versions concernées** : [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] version *20.0.0*

- **Problème et impact sur le client** : L’implémentation de la commande *copy-logs* suppose l’installation de l’outil client `kubectl` sur l’ordinateur client à partir duquel la commande est émise. Si vous émettez la commande vers un cluster BDC installé sur OpenShift, à partir d’un client sur lequel seul l’outil `oc` est installé, vous obtiendrez un message d’erreur : *Une erreur s’est produite lors de la collecte des journaux : [WinError 2] Le système ne trouve pas le fichier spécifié*.

- **Solution de contournement** : Installez l’outil `kubectl` sur le même ordinateur client et réexécutez la commande `azdata bdc copy-logs`. Vous trouverez [ici](deploy-big-data-tools.md) des instructions sur l’installation de `kubectl`.

### <a name="deployment-with-private-repository"></a>Déploiement avec dépôt privé

- **Versions concernées** : GDR1, CU1, CU2. Résolu pour CU3.

- **Problème et impact sur le client** : La mise à niveau à partir d’un dépôt privé a des exigences spécifiques

- **Solution de contournement** : Si vous utilisez un dépôt privé pour pré-extraire les images pour le déploiement ou la mise à niveau de BDC, assurez-vous que les images de build actuelles et les images de build cibles se trouvent dans le dépôt privé. Cela permet une restauration réussie, si nécessaire. En outre, si vous avez modifié les informations d’identification du dépôt privé depuis le déploiement d’origine, mettez à jour le secret correspondant dans Kubernetes avant de procéder à la mise à niveau. [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] ne prend pas en charge la mise à jour des informations d’identification via les variables d’environnement `AZDATA_PASSWORD` et `AZDATA_USERNAME`. Mettez à jour le secret à l’aide de [`kubectl edit secrets`](https://kubernetes.io/docs/concepts/configuration/secret/#editing-a-secret). 

La mise à niveau à l’aide de dépôts différents pour les builds actuels et cibles n’est pas prise en charge.

### <a name="upgrade-may-fail-due-to-timeout"></a>La mise à niveau peut échouer en raison du délai d’expiration

- **Versions concernées** : GDR1, CU1, CU2. Résolu pour CU 3.

- **Problème et impact sur le client** : Une mise à niveau peut échouer en raison du délai d’expiration.

   Le code suivant montre à quoi peut ressembler l’échec :

   ```
   >azdata.EXE bdc upgrade --name <mssql-cluster>
   Upgrading cluster to version 15.0.4003

   NOTE: Cluster upgrade can take a significant amount of time depending on
   configuration, network speed, and the number of nodes in the cluster.

   Upgrading Control Plane.
   Control plane upgrade failed. Failed to upgrade controller.
   ```

   Cette erreur est plus susceptible de se produire lorsque vous mettez à niveau BDC dans Azure Kubernetes Service (AKS).

- **Solution de contournement** : Augmentez le délai d’expiration de la mise à niveau. 

   Pour augmenter les délais d’attente pour une mise à niveau, modifiez le mappage de configuration de mise à niveau. Pour modifier le mappage de configuration de mise à niveau :

   1. Exécutez la commande suivante :

      ```bash
      kubectl edit configmap controller-upgrade-configmap
      ```

   2. Modifiez les champs suivants :

       **`controllerUpgradeTimeoutInMinutes`** Spécifie le nombre de minutes à attendre avant la fin de la mise à niveau du contrôleur ou de la base de données du contrôleur. La valeur par défaut est 5. Mettez à jour vers au moins 20.

       **`totalUpgradeTimeoutInMinutes`**  : Désigne la durée combinée du contrôleur et de la base de données du contrôleur pour terminer la mise à niveau (mise à niveau `controller` + `controllerdb`). La valeur par défaut est 10. Mettez à jour vers au moins 40.

       **`componentUpgradeTimeoutInMinutes`**  : Désigne la durée d’exécution de chaque phase suivante de la mise à niveau. La valeur par défaut est 30. Mettez à jour vers 45.

   3. Enregistrez et quittez

   Le script Python ci-dessous est une autre façon de définir le délai d’expiration :

   ```python
   from kubernetes import client, config
   import json

   def set_upgrade_timeouts(namespace, controller_timeout=20, controller_total_timeout=40, component_timeout=45):
       """ Set the timeouts for upgrades

       The timeout settings are as follows

       controllerUpgradeTimeoutInMinutes: sets the max amount of time for the controller
           or controllerdb to finish upgrading

       totalUpgradeTimeoutInMinutes: sets the max amount of time to wait for both the
           controller and controllerdb to complete their upgrade

       componentUpgradeTimeoutInMinutes: sets the max amount of time allowed for
           subsequent phases of the upgrade to complete
       """
       config.load_kube_config()

       upgrade_config_map = client.CoreV1Api().read_namespaced_config_map("controller-upgrade-configmap", namespace)

       upgrade_config = json.loads(upgrade_config_map.data["controller-upgrade"])

       upgrade_config["controllerUpgradeTimeoutInMinutes"] = controller_timeout

       upgrade_config["totalUpgradeTimeoutInMinutes"] = controller_total_timeout

       upgrade_config["componentUpgradeTimeoutInMinutes"] = component_timeout

       upgrade_config_map.data["controller-upgrade"] = json.dumps(upgrade_config)

       client.CoreV1Api().patch_namespaced_config_map("controller-upgrade-configmap", namespace, upgrade_config_map)
   ```

### <a name="livy-job-submission-from-azure-data-studio-ads-or-curl-fail-with-500-error"></a>Échec de l’envoi de tâches Livy à partir d’Azure Data Studio (ADS) ou de curl avec l’erreur 500

- **Problème et impact sur le client** : Dans une configuration à haute disponibilité, les ressources partagées Spark `sparkhead` sont configurées avec plusieurs réplicas. Dans ce cas, vous pouvez rencontrer des échecs lors de l’envoi de tâches Livy à partir d’Azure Data Studio (ADS) ou de `curl`. Pour vérifier, une opération `curl` vers n’importe quel pod `sparkhead` entraîne un refus de connexion. Par exemple, `curl https://sparkhead-0:8998/` ou `curl https://sparkhead-1:8998` retourne l’erreur 500.

   Ceci est le cas dans les scénarios suivants :

   - Les pods ou les processus Zookeeper pour chaque instance Zookeeper sont redémarrés plusieurs fois.
   - Lorsque la connectivité réseau n’est pas fiable entre le pod `sparkhead` et les pods Zookeeper.

- **Solution de contournement** : Redémarrage des deux serveurs Livy.

   ```bash
   kubectl -n <clustername> exec sparkhead-0 -c hadoop-livy-sparkhistory supervisorctl restart livy
   ```

   ```bash
   kubectl -n <clustername> exec sparkhead-1 -c hadoop-livy-sparkhistory supervisorctl restart livy
   ```

### <a name="create-memory-optimized-table-when-master-instance-in-an-availability-group"></a>Créer une table à mémoire optimisée lorsque l’instance principale est dans un groupe de disponibilité

- **Problème et impact sur le client** : Vous ne pouvez pas utiliser le point de terminaison principal exposé pour la connexion aux bases de données du groupe de disponibilité (écouteur) pour créer des tables à mémoire optimisée.

- **Solution de contournement** : Pour créer des tables à mémoire optimisée lorsque l’instance principale SQL Server est une configuration de groupe de disponibilité, [connectez-vous à l’instance SQL Server](deployment-high-availability.md#instance-connect), exposez un point de terminaison, connectez-vous à la base de données SQL Server et créez les tables optimisées en mémoire dans le session créée avec la nouvelle connexion.

### <a name="insert-to-external-tables-active-directory-authentication-mode"></a>Insérer dans des tables externes en mode d’authentification Active Directory

- **Problème et impact sur le client** : Lorsque l’instance principale SQL Server est en mode d’authentification Active Directory, une requête qui sélectionne uniquement à partir de tables externes où au moins l’une des tables externes figure dans un pool de stockage, et insère dans une autre table externe, retourne :

   ```
   Msg 7320, Level 16, State 102, Line 1
   Cannot execute the query "Remote Query" against OLE DB provider "SQLNCLI11" for linked server "SQLNCLI11". Only domain logins can be used to query Kerberized storage pool.
   ```

- **Solution de contournement** : Modifiez la requête de l’une des manières suivantes. Joignez d’abord la table de pool de stockage à une table locale, ou insérez dans la table locale, puis lisez la table locale pour l’insérer dans le pool de données.

### <a name="transparent-data-encryption-capabilities-can-not-be-used-with-databases-that-are-part-of-the-availability-group-in-the-sql-server-master-instance"></a>Les fonctionnalités de Transparent Data Encryption ne peuvent pas être utilisées avec des bases de données qui font partie du groupe de disponibilité dans l’instance maître SQL Server

- **Problème et impact sur le client** : Dans une configuration à haute disponibilité, les bases de données pour lesquelles le chiffrement est activé ne peuvent pas être utilisées après un basculement, car la clé principale utilisée pour le chiffrement est différente sur chaque réplica. 

- **Solution de contournement** : Il n’existe aucune solution de contournement pour ce problème. Nous vous recommandons de ne pas activer le chiffrement dans cette configuration tant qu’un correctif n’est pas en place.

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur les [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)], consultez [Que sont les [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)] ?](big-data-cluster-overview.md)
