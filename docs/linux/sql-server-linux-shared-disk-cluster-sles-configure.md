---
title: Configurer un cluster de disque partagé SLES pour SQL Server
description: Implémentez la haute disponibilité en configurant le cluster de disques partagés SUSE Linux Enterprise Server (SLES) pour SQL Server.
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 03/17/2017
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: e5ad1bdd-c054-4999-a5aa-00e74770b481
ms.openlocfilehash: 87a4fd9edf52c3590fb19781fc5bf76c9bb0168b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346263"
---
# <a name="configure-sles-shared-disk-cluster-for-sql-server"></a>Configurer un cluster de disque partagé SLES pour SQL Server

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Ce guide fournit des instructions pour créer un cluster à deux nœuds pour SQL Server sur SUSE Linux Enterprise Server (SLES). La couche de clustering est basée sur l’[Extension haute disponibilité (HAE)](https://www.suse.com/products/highavailability) de SUSE placée au-dessus de [Pacemaker](https://clusterlabs.org/). 

Pour plus d’informations sur la configuration du cluster, les options de l’agent de ressources, la gestion, les meilleures pratiques et les suggestions, consultez [Extension haute disponibilité SUSE Linux Enterprise 12 SP2](https://www.suse.com/documentation/sle-ha-12/index.html).

## <a name="prerequisites"></a>Prérequis

Pour effectuer le scénario de bout en bout suivant, vous avez besoin de deux machines pour déployer le cluster à deux nœuds et d’un autre serveur pour configurer le partage NFS. Les étapes ci-dessous décrivent comment ces serveurs seront configurés.

## <a name="setup-and-configure-the-operating-system-on-each-cluster-node"></a>Installer et configurer le système d’exploitation sur chaque nœud du cluster

La première étape consiste à configurer le système d'exploitation sur les nœuds de cluster. Pour ce guide, utilisez SLES avec un abonnement valide pour le module complémentaire de haute disponibilité.

## <a name="install-and-configure-sql-server-on-each-cluster-node"></a>Installer et configurer SQL Server sur chaque nœud du cluster

1. Installez et configurez SQL Server sur les deux nœuds. Pour obtenir des instructions détaillées, consultez [Installer SQL Server sur Linux](sql-server-linux-setup.md).
2. Désignez un nœud comme principal et l’autre comme secondaire, à des fins de configuration. Utilisez ces termes pour le présent guide. 
3. Sur le nœud secondaire, arrêtez et désactivez SQL Server. L’exemple suivant arrête et désactive SQL Server :

    ```bash
    sudo systemctl stop mssql-server
    sudo systemctl disable mssql-server
    ```

    > [!NOTE]
    > Au moment de la configuration, une clé principale de serveur est générée pour l’instance et placée à l’adresse `/var/opt/mssql/secrets/machine-key`. Sur Linux, SQL Server s’exécute toujours en tant que compte local appelé mssql. Étant donné qu’il s’agit d’un compte local, son identité n’est pas partagée entre les nœuds. Par conséquent, vous devez copier la clé de chiffrement du nœud principal sur chaque nœud secondaire afin que chaque compte mssql local puisse y accéder pour déchiffrer la clé principale du serveur.
4. Sur le nœud principal, créez une connexion SQL Server pour Pacemaker et octroyez l’autorisation de connexion pour exécuter `sp_server_diagnostics`. Pacemaker utilise ce compte pour vérifier le nœud en cours d’exécution SQL Server.

    ```bash
    sudo systemctl start mssql-server
    ```
    Connectez-vous à la base de données SQL Server avec le compte « sa » et exécutez la commande suivante :

    ```sql
    USE [master]
    GO
    CREATE LOGIN [<loginName>] with PASSWORD= N'<loginPassword>'
    GRANT VIEW SERVER STATE TO <loginName>
    ```
5. Sur le nœud principal, arrêtez et désactivez SQL Server.
6. Suivez les instructions [de la documentation SUSE](https://www.suse.com/documentation/sles11/book_sle_admin/data/sec_basicnet_yast.html) pour configurer et mettre à jour le fichier hôtes pour chaque nœud de cluster. Le fichier « hôtes » doit inclure l’adresse IP et le nom de chaque nœud de cluster.

    Pour vérifier l’adresse IP de l’exécution du nœud actuel :

    ```bash
    sudo ip addr show
    ```

    Définissez le nom de l’ordinateur sur chaque nœud. Donnez à chaque nœud un nom unique de 15 caractères ou moins. Définissez le nom de l’ordinateur en l'ajoutant à `/etc/hostname` à l’aide de [yast](https://www.suse.com/documentation/sles11/book_sle_admin/data/sec_basicnet_yast.html) ou [manuellement](https://www.suse.com/documentation/sled11/book_sle_admin/data/sec_basicnet_manconf.html).

    L’exemple suivant présente `/etc/hosts` avec des ajouts pour deux nœuds nommés `SLES1` et `SLES2`.

    ```
    127.0.0.1   localhost
    10.128.18.128 SLES1
    10.128.16.77 SLES2
    ```

    > [!NOTE]
    > Tous les nœuds de cluster doivent pouvoir accéder aux uns et aux autres via SSH. Certains outils, comme hb_report ou crm_report (pour le dépannage) et l’Explorateur d’historique de Hawk, exigent un accès SSH sans mot de passe entre les nœuds, sinon ils ne peuvent collecter que les données du nœud actif. Si vous utilisez un port SSH non standard, utilisez l’option -X ([consultez la page man](https://www.suse.com/documentation/sle_ha/book_sleha/data/sec_ha_requirements_other.html)). Par exemple, si votre port SSH est le 3479, appelez un crm_report avec :
    >
    >```bash
    >crm_report -X "-p 3479" [...]
    >```
    >Pour plus d’informations, consultez [le guide d’administration]. (https://www.suse.com/documentation/sle-ha-12/singlehtml/book_sleha/book_sleha.html#sec.ha.troubleshooting.misc)

Dans la section suivante, vous allez configurer le stockage partagé et déplacer vos fichiers de base de données vers ce stockage.  

## <a name="configure-shared-storage-and-move-database-files"></a>Configurer le stockage partagé et déplacer des fichiers de base de données

Il existe diverses solutions pour fournir un stockage partagé. Cette procédure pas à pas illustre la configuration du stockage partagé avec NFS. Nous vous recommandons de suivre les meilleures pratiques et d’utiliser Kerberos pour sécuriser NFS : 

- [Partage de systèmes de fichiers avec NFS](https://www.suse.com/documentation/sles-12/singlehtml/book_sle_admin/book_sle_admin.html#cha.nfs)

Si vous ne suivez pas ce guide, toute personne pouvant accéder à votre réseau et usurper l’adresse IP d’un nœud SQL pourra accéder à vos fichiers de données. Comme toujours, veillez à modéliser votre système de menaces avant de l’utiliser en production. 

Une autre option de stockage consiste à utiliser le partage de fichiers SMB :

- [Section Samba de la documentation SUSE](https://www.suse.com/documentation/sles-12/singlehtml/book_sle_admin/book_sle_admin.html#cha.samba)

### <a name="configure-an-nfs-server"></a>Configurer un serveur NFS

Pour configurer un serveur NFS, consultez les étapes suivantes dans la documentation SUSE : [Configuration du serveur NFS](https://www.suse.com/documentation/sles-12/singlehtml/book_sle_admin/book_sle_admin.html#sec.nfs.configuring-nfs-server).

### <a name="configure-all-cluster-nodes-to-connect-to-the-nfs-shared-storage"></a>Configurer tous les nœuds de cluster pour la connexion au stockage partagé NFS

Avant de configurer le client NFS pour monter le chemin d’accès aux fichiers de base de données SQL Server pour qu’il pointe vers l’emplacement de stockage partagé, veillez à enregistrer les fichiers de base de données dans un emplacement temporaire afin de pouvoir les copier ultérieurement sur le partage :

1. **Sur le nœud principal uniquement**, enregistrez les fichiers de base de données dans un emplacement temporaire. Le script suivant crée un nouveau répertoire temporaire, copie les fichiers de base de données dans le nouveau répertoire et supprime les anciens fichiers de base de données. Comme SQL Server s’exécute en tant qu’utilisateur local mssql, vous devez vous assurer qu’après le transfert des données vers le partage monté, l’utilisateur local dispose d’un accès en lecture-écriture au partage. 

    ```bash
    su mssql
    mkdir /var/opt/mssql/tmp
    cp /var/opt/mssql/data/* /var/opt/mssql/tmp
    rm /var/opt/mssql/data/*
    exit
    ```

    Configurez le client NFS sur tous les nœuds de cluster :

    - [Configuration des clients](https://www.suse.com/documentation/sles-12/singlehtml/book_sle_admin/book_sle_admin.html#sec.nfs.configuring-nfs-clients)

    > [!NOTE]
    > Il est recommandé de suivre les meilleures pratiques et les suggestions de SUSE concernant le stockage NFS à haute disponibilité : [Stockage NFS à haute disponibilité avec DRBD et Pacemaker](https://www.suse.com/documentation/sle-ha-12/book_sleha_techguides/data/art_ha_quick_nfs.html).

2. Vérifiez que SQL Server démarre correctement avec le nouveau chemin d’accès au fichier. Procédez de la sorte sur chaque nœud. À ce stade, un seul nœud doit exécuter SQL Server à la fois. Ils ne peuvent pas être exécutés en même temps, car ils essaient d’accéder simultanément aux fichiers de données (pour éviter de démarrer accidentellement SQL Server sur les deux nœuds, utilisez une ressource de cluster de système de fichiers pour vous assurer que le partage n’est pas monté deux fois par les différents nœuds). Les commandes suivantes démarrent SQL Server, vérifient l’état, puis, arrêtent SQL Server.

    ```bash
    sudo systemctl start mssql-server
    sudo systemctl status mssql-server
    sudo systemctl stop mssql-server
    ```

À ce stade, les deux instances de SQL Server sont configurées pour s’exécuter avec les fichiers de base de données sur le stockage partagé. L’étape suivante consiste à configurer SQL Server pour Pacemaker. 

## <a name="install-and-configure-pacemaker-on-each-cluster-node"></a>Installer et configurer Pacemaker sur chaque nœud de cluster

1. **Sur les deux nœuds du cluster, créez un fichier pour stocker le nom d’utilisateur et le mot de passe SQL Server du compte de connexion Pacemaker**. La commande suivante a pour effet de créer et remplir ce fichier :

    ```bash
    sudo touch /var/opt/mssql/secrets/passwd
    sudo echo '<loginName>' >> /var/opt/mssql/secrets/passwd
    sudo echo '<loginPassword>' >> /var/opt/mssql/secrets/passwd
    sudo chown root:root /var/opt/mssql/secrets/passwd 
    sudo chmod 600 /var/opt/mssql/secrets/passwd
    ```
2. **Tous les nœuds de cluster doivent pouvoir accéder aux uns et aux autres via SSH**. Certains outils, comme hb_report ou crm_report (pour le dépannage) et l’Explorateur d’historique de Hawk, exigent un accès SSH sans mot de passe entre les nœuds, sinon ils ne peuvent collecter que les données du nœud actif. Si vous utilisez un port SSH non standard, utilisez l’option -X (consultez le manuel). Par exemple, si votre port SSH est le 3479, appelez un hb_report avec :

    ```bash
    crm_report -X "-p 3479" [...]
    ```

    Pour plus d’informations, consultez la [configuration système requise et les recommandations dans la documentation de SUSE](https://www.suse.com/documentation/sle_ha/book_sleha/data/sec_ha_requirements_other.html).

3. **Installez l’extension High Availability**. Pour installer cette extension, suivez les étapes décrites dans la rubrique SUSE suivante :
    
    [Installation and Setup Quick Start](https://www.suse.com/documentation/sle-ha-12/singlehtml/install-quick/install-quick.html) (Guide rapide d’installation et de configuration)

4. **Installez l’agent de ressources FCI pour SQL Server**. Exécutez les commandes suivantes sur les deux nœuds :

    ```bash
    sudo zypper addrepo -fc https://packages.microsoft.com/config/sles/12/mssql-server-2017.repo
    sudo zypper --gpg-auto-import-keys refresh
    sudo zypper install mssql-server-ha
    ```

5. **Configurez automatiquement le premier nœud**. L’étape suivante consiste à configurer un cluster à un nœud en cours d’exécution en configurant le premier nœud, SLES1. Suivez les instructions de la rubrique SUSE, [Setting Up the First Node](https://www.suse.com/documentation/sle-ha-12/singlehtml/install-quick/install-quick.html#sec.ha.inst.quick.setup.1st-node) (Configuration du premier nœud).

    À la fin de la procédure, vérifiez l’état du cluster à l’aide de `crm status` :
    ```bash
    crm status
    ```

    Le résultat doit indiquer qu’un seul nœud est configuré : SLES1.

6. **Ajoutez des nœuds à un cluster existant**. Joignez ensuite le nœud SLES2 au cluster. Suivez les instructions de la rubrique SUSE, [Adding the Second Node](https://www.suse.com/documentation/sle-ha-12/singlehtml/install-quick/install-quick.html#sec.ha.inst.quick.setup.2nd-node) (Ajout du deuxième nœud).
    
    À la fin de la procédure, vérifiez l’état du cluster à l’aide de **crm status**. Si vous avez correctement ajouté le deuxième nœud, voici ce que vous obtenez en sortie :
        
    ```
    2 nodes configured
    1 resource configured
    Online: [ SLES1 SLES2 ]
    Full list of resources:
    admin_addr     (ocf::heartbeat:IPaddr2):       Started SLES1
    ```

    > [!NOTE]
    > **admin_addr** est la ressource de cluster IP virtuelle qui est configurée pendant l’installation initiale du cluster à un nœud.

7.  **Procédures de suppression**. Si vous devez supprimer un nœud du cluster, utilisez le script d’amorçage **ha-cluster-remove**. Pour plus d’informations, consultez [Overview of the Bootstrap Scripts](https://www.suse.com/documentation/sle-ha-12/singlehtml/install-quick/install-quick.html#sec.ha.inst.quick.bootstrap) (Vue d’ensemble des scripts d’amorçage).  

## <a name="configure-the-cluster-resources-for-sql-server"></a>Configurer les ressources de cluster pour SQL Server

Les étapes suivantes expliquent comment configurer la ressource de cluster pour SQL Server. Vous devez personnaliser deux paramètres.

- **Nom de ressources SQL Server** : Un nom de la ressource de SQL Server en cluster. 
- **Valeur de délai d’attente** : La valeur de délai d’attente correspond à la durée d’attente du cluster tandis qu’une ressources est mise en ligne. Pour SQL Server, il s’agit de la durée prévue pour que SQL Server mette la base de données `master` en ligne. 

Mettez à jour les valeurs du script suivant pour votre environnement. Exécutez sur un nœud pour configurer et démarrer le service en cluster.

```bash
sudo crm configure
primitive <sqlServerResourceName> ocf:mssql:fci op start timeout=<timeout_in_seconds>
colocation <constraintName> inf: <virtualIPResourceName> <sqlServerResourceName>
show
commit
exit
```

Par exemple, le script suivant crée une ressource SQL Server en cluster nommée mssqlha. 

```bash
sudo crm configure
primitive mssqlha ocf:mssql:fci op start timeout=60s
colocation admin_addr_mssqlha inf: admin_addr mssqlha
show
commit
exit
```

Une fois la configuration validée, SQL Server démarre sur le même nœud que la ressource d’adresse IP virtuelle. 

Pour plus d’informations, consultez [Configuration et gestion des Ressources de cluster (ligne de commande)](https://www.suse.com/documentation/sle-ha-12/singlehtml/book_sleha/book_sleha.html#cha.ha.manual_config). 

### <a name="verify-that-sql-server-is-started"></a>Vérifiez que SQL Server a démarré. 

Pour vérifier que SQL Server a démarré, exécutez la commande **état crm** :

```bash
crm status
```

Les exemples suivants affichent les résultats lorsque Pacemaker a démarré avec succès en tant que ressource de cluster. 
```
2 nodes configured
2 resources configured

Online: [ SLES1 SLES2 ]

Full list of resources:

 admin_addr     (ocf::heartbeat:IPaddr2):       Started SLES1
 mssqlha        (ocf::mssql:fci):       Started SLES1
```

## <a name="managing-cluster-resources"></a>Gestion des ressources de cluster

Pour gérer vos ressources de cluster, consultez la rubrique SUSE suivante : [Gestion des ressources de cluster](https://www.suse.com/documentation/sle-ha-12/singlehtml/book_sleha/book_sleha.html#sec.ha.config.crm )

### <a name="manual-failover"></a>Basculement manuel

Bien que les ressources soient configurées pour basculer (ou migrer) automatiquement vers d’autres nœuds du cluster, en cas de défaillance matérielle ou logicielle, vous pouvez également déplacer manuellement une ressource vers un autre nœud dans le cluster à l’aide de l’interface graphique utilisateur Pacemaker ou de la ligne de commande. 

Utilisez la commande Migrer pour cette tâche. Par exemple, pour migrer la ressource SQL vers des noms d’un nœud de cluster, SLES2 exécute : 

```bash
crm resource
migrate mssqlha SLES2
```

## <a name="additional-resources"></a>Ressources supplémentaires

[Extension de haute disponibilité SUSE Linux Enterprise - guide d’administration](https://www.suse.com/documentation/sle-ha-12/singlehtml/book_sleha/book_sleha.html) 
