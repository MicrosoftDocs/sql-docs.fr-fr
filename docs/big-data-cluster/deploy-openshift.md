---
title: Déploiement sur OpenShift
titleSuffix: SQL Server Big Data Cluster
description: Découvrez comment mettre à niveau des clusters Big Data SQL Server sur OpenShift.
author: mihaelablendea
ms.author: mihaelab
ms.reviewer: mikeray
ms.date: 06/22/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 92cca7d688f954fc8e832f4bc0e68f105c024202
ms.sourcegitcommit: ece104654ac14e10d32e59f45916fa944665f4df
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/09/2021
ms.locfileid: "102474883"
---
# <a name="deploy-big-data-clusters-2019-on-openshift-on-premises-and-azure-red-hat-openshift"></a>Déploiement de [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] sur OpenShift en local et sur Azure Red Hat OpenShift

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Cet article explique comment déployer un cluster Big Data SQL Server sur des environnements OpenShift en local et sur Azure Red Hat OpenShift (ARO).

> [!TIP]
> Pour amorcer rapidement un exemple d’environnement avec ARO, puis déployer Clusters Big Data sur cette plateforme, vous pouvez utiliser le script Python disponible [ici](quickstart-big-data-cluster-deploy-aro.md).


SQL Server 2019 CU5 introduit la prise en charge de Clusters Big Data SQL Server sur OpenShift. Vous pouvez déployer des clusters Big Data sur OpenShift en local ou sur Azure Red Hat OpenShift (ARO). Le déploiement exige au minimum la version 4.3 du cluster OpenShift. Même si le flux de travail de déploiement est similaire à celui d’autres plateformes basées sur Kubernetes ([kubeadm](deploy-with-kubeadm.md) et [AKS](deploy-on-aks.md)), il existe quelques différences. Les principales concernent l’exécution d’applications en tant qu’utilisateur non racine et le contexte de sécurité utilisé pour l’espace de noms dans lequel le cluster Big Data est déployé.

Pour déployer le cluster OpenShift en local, consultez la [documentation Red Hat OpenShift](https://docs.openshift.com/container-platform/4.3/release_notes/ocp-4-3-release-notes.html#ocp-4-3-installation-and-upgrade). Pour les déploiements ARO, consultez [Azure Red Hat OpenShift](/azure/openshift/intro-openshift).

Cet article décrit la procédure de déploiement propre à la plateforme OpenShift et détaille les options disponibles pour accéder à l’environnement cible et à l’espace de noms servant à déployer le cluster Big Data.

## <a name="pre-requisites"></a>Conditions préalables

> [!IMPORTANT]
> Les prérequis suivants doivent être suivis par un administrateur de cluster OpenShift (rôle de cluster cluster-admin) disposant d’autorisations suffisantes pour créer ces objets au niveau du cluster. Pour plus d’informations sur les rôles de cluster dans OpenShift, consultez [Définition et application d’autorisations à l’aide du contrôle RBAC](https://docs.openshift.com/container-platform/4.4/authentication/using-rbac.html).

1. Veillez à ce que le paramètre `pidsLimit` sur OpenShift soit mis à jour pour prendre en charge les charges de travail SQL Server. La valeur par défaut dans OpenShift est trop faible pour les charges de travail de type production. Démarrez avec au moins `4096`, mais la valeur optimale dépend du paramètre `max worker threads` dans SQL Server et du nombre de processeurs UC présents sur le nœud hôte OpenShift. 
    - Pour savoir comment mettre à jour `pidsLimit` sur votre cluster OpenShift, suivez [ces instructions]( https://github.com/openshift/machine-config-operator/blob/master/docs/ContainerRuntimeConfigDesign.md). Notez que les versions de OpenShift antérieures à `4.3.5` comportaient un défaut à cause duquel la valeur mise à jour n’était pas prise en compte. Veillez à mettre à niveau OpenShift pour passer à la dernière version. 
    - Pour calculer la valeur optimale en fonction de votre environnement et de vos charges de travail SQL Server prévues, vous pouvez utiliser l’estimation et les exemples suivants :

    |Nombre de processeurs|Nombre maximal de threads de travail|Nombre de Workers par processeur par défaut|Valeur pidsLimit minimale|
    |--------------------|--------------------------|-----------------------------|-----------------------|
    |          64        |           512            |             16              | 512 + (64 × 16) = 1 536 |
    |         128        |           512            |             32              | 512 + (128 × 32) = 4 608 |

    > [!NOTE]
    > D’autres processus (par exemple, les sauvegardes, CLR, Fulltext et SQLAgent) introduisent également une certaine surcharge. Par conséquent, ajoutez une marge à la valeur estimée.

1. Téléchargez la contrainte de contexte de sécurité (SCC) personnalisée [`bdc-scc.yaml`](#bdc-sccyaml-file) :

    ```console
    curl https://raw.githubusercontent.com/microsoft/sql-server-samples/master/samples/features/sql-big-data-cluster/deployment/openshift/bdc-scc.yaml -o bdc-scc.yaml
    ```

1. Appliquez la SCC au cluster.

    ```console
    oc apply -f bdc-scc.yaml
    ```

    > [!NOTE]
    > La contrainte de contexte de sécurité personnalisée de Clusters Big Data s’appuie sur la contrainte `nonroot` intégrée dans OpenShift, à laquelle s’ajoutent des autorisations supplémentaires. Pour plus d’informations sur les contraintes de contexte de sécurité dans OpenShift, consultez [Gestion des contraintes de contexte de sécurité](https://docs.openshift.com/container-platform/4.3/authentication/managing-security-context-constraints.html). Pour des informations détaillées sur les autorisations nécessaires aux clusters Big Data en plus de la contrainte de contexte de sécurité `nonroot`, téléchargez le livre blanc [ici](https://aka.ms/sql-bdc-openshift-security).

3. Créez un espace de noms/projet :

   ```console
   oc new-project <namespaceName>
   ```

4. Affectez la contrainte de contexte de sécurité personnalisée aux comptes de service pour les utilisateurs de l’espace de noms dans lequel le cluster Big Data est déployé :

   ```console
  oc create rolebinding bdc-rbac --clusterrole=system:scc:bdc-scc --group=system:serviceaccounts:<namespace>
   ```

5. Attribuez l’autorisation appropriée à l’utilisateur qui déploie le cluster Big Data. Procédez de l'une des manières suivantes : 

   - Si l’utilisateur qui déploie le cluster Big Data possède un rôle d’administrateur de cluster, passez à [Déploiement du cluster Big Data](#deploy-big-data-cluster).

   - Si l’utilisateur qui déploie le cluster Big Data est administrateur d’espace de noms, attribuez-lui le rôle local d’administrateur de cluster pour l’espace de noms créé. Il s’agit de l’option recommandée pour que l’utilisateur qui déploie et gère le cluster Big Data dispose des autorisations d’administrateur au niveau de l’espace de noms.

   ```console
   oc adm policy add-role-to-user cluster-admin <deployingUser> -n <namespaceName>
   ```

   L’utilisateur qui déploie le cluster Big Data doit ensuite se connecter à la console OpenShift :

   ```console
   oc login -u <deployingUser> -p <password>
   ```

## <a name="deploy-big-data-cluster"></a>Déploiement d’un cluster Big Data

1. Installez la dernière version de [azdata](../azdata/install/deploy-install-azdata.md).

1. Clonez l’un des fichiers de configuration intégrés pour OpenShift, en fonction de votre environnement cible (OpenShift local ou ARO) et du scénario de déploiement. Pour connaître les paramètres propres à OpenShift dans les fichiers de configuration intégrés, consultez la section *Paramètres propres à OpenShift dans les fichiers de configuration de déploiement* ci-dessous. Pour plus d’informations sur les fichiers de configuration disponibles, consultez [Aide au déploiement](deployment-guidance.md).

   Listez tous les fichiers de configuration intégrés disponibles.

   ```console
   azdata bdc config list
   ```

   Pour cloner l’un des fichiers de configuration intégrés, exécutez la commande suivante (si vous le souhaitez, vous pouvez remplacer le profil en fonction de votre plateforme/scénario ciblé) :

   ```console
   azdata bdc config init --source openshift-dev-test --target custom-openshift
   ```

   Pour un déploiement sur ARO, démarrez avec l’un des profils `aro-`, qui comprend les valeurs par défaut pour `serviceType` et `storageClass` adaptées à cet environnement. Par exemple :

   ```console
   azdata bdc config init --source aro-dev-test --target custom-openshift
   ```

1. Personnalisez les fichiers de configuration control.json et bdc.json. Voici quelques ressources supplémentaires qui vous guideront à travers les personnalisations prises en charge pour différents cas d’usage :

   - [Stockage](concept-data-persistence.md)
   - [Paramètres liés à AD](active-directory-deploy.md)
   - [Autres personnalisations](deployment-custom-configuration.md)

   > [!NOTE]
   > L’intégration avec Azure Active Directory pour Clusters Big Data n’est pas prise en charge. Il n’est donc pas possible d’utiliser cette méthode d’authentification lors du déploiement sur ARO.

1. Définissez des [variables d’environnement](deployment-guidance.md#env).

1. Déploiement d’un cluster Big Data

   ```console
   azdata bdc create --config custom-openshift --accept-eula yes
   ```

1. Une fois le déploiement réussi, vous pouvez vous connecter et lister les points de terminaison du cluster externe :

   ```console
      azdata login -n mssql-cluster
      azdata bdc endpoint list
   ```

## <a name="openshift-specific-settings-in-the-deployment-configuration-files"></a>Paramètres propres à OpenShift dans les fichiers de configuration de déploiement

SQL Server 2019 CU5 introduit deux commutateurs de fonctionnalités pour contrôler la collecte de métriques sur les pods et les nœuds. Ces paramètres ont la valeur `false` par défaut dans les profils intégrés pour OpenShift, car la supervision des conteneurs exige un [contexte de sécurité privilégié](https://www.openshift.com/blog/managing-sccs-in-openshift), ce qui assouplit certaines contraintes de sécurité de l’espace de noms sur lequel le cluster Big Data est déployé.

```json
    "security": {
      "allowNodeMetricsCollection": false,
      "allowPodMetricsCollection": false
}
```

Le nom de la classe de stockage par défaut dans ARO est managed-premium (par opposition à AKS, où elle s’appelle default). Vous la trouverez dans le fichier `control.json` correspondant à `aro-dev-test` et à `aro-dev-test-ha` :

```json
    },
    "storage": {
      "data": {
        "className": "managed-premium",
        "accessMode": "ReadWriteOnce",
        "size": "15Gi"
      },
      "logs": {
        "className": "managed-premium",
        "accessMode": "ReadWriteOnce",
        "size": "10Gi"
      }
```

## <a name="bdc-sccyaml-file"></a>Fichier `bdc-scc.yaml`

Le fichier SCC pour ce déploiement est le suivant :

:::code language="yaml" source="../../sql-server-samples/samples/features/sql-big-data-cluster/deployment/openshift/bdc-scc.yaml":::

## <a name="next-steps"></a>Étapes suivantes

[Tutoriel : Chargement d’un exemple de données dans un cluster Big Data SQL Server](tutorial-load-sample-data.md)
