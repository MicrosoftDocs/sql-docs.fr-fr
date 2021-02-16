---
title: Déployer un groupe de disponibilité avec HPE Serviceguard - SQL Server sur Linux
description: Utilisez HPE Serviceguard comme gestionnaire de cluster pour bénéficier d’une haute disponibilité avec un groupe de disponibilité sur SQL Server sur Linux
ms.date: 01/15/2021
ms.prod: sql
ms.technology: linux
ms.topic: tutorial
author: amvin87
ms.author: amitkh
ms.reviewer: vanto
ms.openlocfilehash: 528d9550f79eea50db554dec11ead53039348cdb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100051930"
---
# <a name="tutorial---setup-a-three-node-always-on-availability-group-with-hpe-serviceguard-for-linux"></a>Tutoriel : Configurer un groupe de disponibilité Always On à trois nœuds avec HPE Serviceguard pour Linux 

Ce tutoriel explique comment configurer un groupe de disponibilité Always On SQL Server avec HPE Serviceguard pour Linux exécuté sur des machines virtuelles locales.

Pour obtenir une vue d’ensemble des clusters HPE Serviceguard, consultez [Clusters HPE Serviceguard](https://h20195.www2.hpe.com/v2/GetPDF.aspx/c04154488.pdf).

> [!NOTE]
> Microsoft assure le support lié au déplacement des données, au groupe de disponibilité et aux composants SQL Server. Contactez HPE pour obtenir de l’aide relative à la documentation sur la gestion du quorum et du cluster HPE Serviceguard.

Ce didacticiel contient les tâches suivantes :

> [!div class="checklist"]
> * Installer SQL Server sur les trois machines virtuelles qui seront membres du groupe de disponibilité
> * Installer HPE Serviceguard sur les machines virtuelles
> * Créer le cluster HPE Serviceguard
> * Créer le groupe de disponibilité et y ajouter un exemple de base de données
> * Déployer la charge de travail SQL Server sur le groupe de disponibilité par le biais du gestionnaire de cluster Serviceguard
> * Effectuer un basculement automatique et joindre à nouveau le nœud au cluster

## <a name="prerequisites"></a>Prérequis

* Trois machines virtuelles dans un environnement local où HPE Serviceguard est exécuté sur une distribution Linux prise en charge.

   Pour obtenir un exemple de distribution prise en charge, consultez [HPE Serviceguard pour Linux](https://h20195.www2.hpe.com/v2/gethtml.aspx?docname=c04154488).

   Contactez HPE pour obtenir des informations sur la prise en charge des environnements de cloud public.

   Les instructions de ce tutoriel ont été validées pour HPE Serviceguard pour Linux. Une version d’essai peut être téléchargée à partir du site [HPE](https://www.hpe.com/us/en/resources/servers/serviceguard-linux-trial.html).

* Les fichiers de base de données SQL Server sur le montage de volume logique (LVM) pour les trois machines virtuelles. Consultez le [Guide de démarrage rapide de Serviceguard pour Linux (HPE)](https://support.hpe.com/hpesc/public/docDisplay?docId=a00107699en_us)

* Assurez-vous que le runtime Java OpenJDK est installé sur les machines virtuelles.

   > [!NOTE]
   > Le SDK Java IBM n’est pas pris en charge.

## <a name="install-sql-server"></a>Installer SQL Server

Sur les trois machines virtuelles, installez SQL Server et les outils nécessaires en effectuant l’une des étapes ci-dessous selon la distribution Linux choisie pour ce tutoriel.

### <a name="rhel"></a>RHEL

* [Installer SQL Server sur Red Hat](quickstart-install-connect-red-hat.md#install)
* [outils](quickstart-install-connect-red-hat.md#tools)

### <a name="sles"></a>SLES

* [Installer SQL Server sur SLES](quickstart-install-connect-suse.md#install)
* [outils](quickstart-install-connect-suse.md#tools)

Au terme de cette étape, le service SQL Server et les outils sont normalement installés sur les trois machines virtuelles qui seront membres du groupe de disponibilité.

## <a name="install-hpe-serviceguard-on-the-vms"></a>Installer HPE Serviceguard sur les machines virtuelles

Durant cette étape, vous installez HPE Serviceguard pour Linux sur les trois machines virtuelles. Le tableau suivant décrit le rôle de chaque serveur dans le cluster.

|Nombre d'ordinateurs virtuels | Rôle HPE Serviceguard | Rôle de réplica du groupe de disponibilité Microsoft SQL Server|
|--------------|-----------------|------------|
|1 | Nœuds du cluster HPE Serviceguard | Réplica principal |
|1 ou plus | Nœud du cluster HPE Serviceguard | Réplica secondaire |
|1 | Serveur de quorum HPE Serviceguard | Réplica de configuration uniquement |

Pour installer Serviceguard, utilisez la méthode `cminstaller`. Vous trouverez des instructions spécifiques en suivant les liens ci-dessous :

Cluster Serviceguard et serveur de quorum Serviceguard

* [Installez Serviceguard pour Linux sur deux nœuds](https://support.hpe.com/hpesc/public/docDisplay?docId=a00107699en_us#Install_serviceguard_using_cminstaller). Reportez-vous à la section **Install_serviceguard_using_cminstaller**.
* [Installez le serveur de quorum Serviceguard sur le troisième nœud](https://support.hpe.com/hpesc/public/docDisplay?docId=a00107699en_us#Install_QS_from_the_ISO). Reportez-vous à la section **Install_QS_from_the_ISO**.

Après avoir installé le cluster HPE Serviceguard, vous pouvez activer le portail de gestion du cluster sur le port TCP 5522 sur le nœud du réplica principal. Les étapes ci-dessous ajoutent une règle au pare-feu pour autoriser le port 5522. La commande exécutée ici s’applique à une distribution RHEL ; si vous utilisez une autre distribution, vous devez exécuter les commandes similaires appropriées :

```console
sudo firewall-cmd --zone=public --add-port=5522/tcp --permanent

sudo firewall-cmd --reload 
```

## <a name="create-hpe-serviceguard-cluster"></a>Créer le cluster HPE Serviceguard

Suivez les instructions ci-dessous pour créer et configurer le cluster HPE Serviceguard. Durant cette étape, vous configurez également le serveur de quorum.

1. [Configurez le serveur de quorum Serviceguard sur le troisième nœud](https://support.hpe.com/hpesc/public/docDisplay?docId=a00107699en_us#Configure_QS). Reportez-vous à la section **Configure_QS**.
2. [Créez et configurez le cluster Serviceguard sur les deux autres nœuds](https://support.hpe.com/hpesc/public/docDisplay?docId=a00107699en_us#Configure_and_create_cluster). Reportez-vous à la section **Configure_and_create_Cluster**.

## <a name="create-the-availability-group-and-add-a-sample-database"></a>Créer le groupe de disponibilité et y ajouter un exemple de base de données

Durant cette étape, vous créez un groupe de disponibilité avec deux (ou plus) réplicas synchrones et un réplica de configuration uniquement, ce qui assure la protection des données et contribue à offrir une haute disponibilité. Le diagramme suivant illustre cette architecture :

:::image type="content" source="media/sql-server-linux-availability-group-ha/2-configuration-only.png" alt-text="Le réplica principal synchronise les données utilisateur et les données de configuration avec le réplica secondaire. Le réplica principal synchronise uniquement les données de configuration avec le réplica de configuration uniquement. Le réplica de configuration uniquement n’a pas de réplicas de données utilisateur.":::

1. Réplication synchrone des données utilisateur vers le réplica secondaire. Il comprend également des métadonnées de configuration des groupes de disponibilité.

2. Réplication synchrone des métadonnées de configuration des groupes de disponibilité. Elle n’inclut pas les données utilisateur.

Pour plus de détails, consultez [Deux réplicas synchrones et un réplica de configuration uniquement](sql-server-linux-availability-group-ha.md).

Pour créer le groupe de disponibilité, effectuez les étapes suivantes :

1. [Activer les groupes de disponibilité Always On et exécuter restart mssql-server](#enable-always-on-availability-groups-and-restart-mssql-server) sur toutes les machines virtuelles, y compris le réplica de configuration uniquement.
2. [Activer une session d’événements `AlwaysOn_health` (facultatif)](#enable-an-alwayson_health-event-session---optional)
3. [Créer un certificat sur la machine virtuelle principale](#create-a-certificate-on-the-primary-vm)
4. [Créer le certificat sur les serveurs secondaires](#create-the-certificate-on-secondary-servers)
5. [Créer les points de terminaison de mise en miroir de bases de données sur les réplicas](#create-the-database-mirroring-endpoints-on-the-replicas)
6. [Créer le groupe de disponibilité](#create-availability-group)
7. [Joindre les réplicas secondaires](#join-the-secondary-replicas)
8. [Ajouter une base de données au groupe de disponibilité créé précédemment](#add-a-database-to-the-availability-group-created-above)

### <a name="enable-always-on-availability-groups-and-restart-mssql-server"></a>Activer les groupes de disponibilité AlwaysOn et redémarrer mssql-server

Activez la fonctionnalité Always On sur tous les nœuds qui hébergent une instance SQL Server. Ensuite, redémarrez mssql-server. Exécutez le script suivant sur les trois réplicas :

```console
sudo /opt/mssql/bin/mssql-conf
set hadr.hadrenabled 1 sudo systemctl restart mssql-serve
```

### <a name="enable-an-alwayson_health-event-session---optional"></a>Activer une session d’événements `AlwaysOn_health` (facultatif)

Activez facultativement les événements étendus des groupes de disponibilité Always On pour faciliter le diagnostic de la cause racine quand vous résolvez les problèmes rencontrés avec un groupe de disponibilité. Exécutez la commande suivante sur chaque instance de SQL Server :

```tsql
ALTER EVENT SESSION AlwaysOn_health ON SERVER WITH (STARTUP_STATE=ON);
GO
```

### <a name="create-a-certificate-on-the-primary-vm"></a>Créer un certificat sur la machine virtuelle principale

Le script Transact-SQL suivant crée une clé principale et un certificat. Il sauvegarde ensuite le certificat et sécurise le fichier avec une clé privée. Mettez à jour le script avec des mots de passe forts. Connectez-vous à l’instance principale de SQL Server et exécutez le script Transact-SQL suivant :

```tsql
CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<Master_Key_Password';

CREATE CERTIFICATE dbm_certificate WITH SUBJECT = 'dbm';

BACKUP CERTIFICATE dbm_certificate TO FILE = '/var/opt/mssql/data/dbm_certificate.cer'
WITH PRIVATE KEY 
    ( FILE = '/var/opt/mssql/data/dbm_certificate.pvk',
      ENCRYPTION BY PASSWORD = '<Private_Key_Password>' );

```

À ce stade, le réplica SQL Server principal a un certificat à l’emplacement `/var/opt/mssql/data/dbm_certificate.cer` et une clé privée à l’emplacement `var/opt/mssql/data/dbm_certificate.pvk`. Copiez ces deux fichiers au même emplacement sur tous les serveurs qui hébergeront les réplicas de disponibilité. Utilisez l’utilisateur mssql ou accordez à l’utilisateur mssql l’autorisation d’accéder à ces fichiers.

Par exemple, sur le serveur source, la commande suivante copie les fichiers sur la machine cible. Remplacez les valeurs `'node2'` par le nom de l’hôte qui exécute l’instance SQL Server secondaire. Copiez aussi le certificat sur le réplica de configuration uniquement et exécutez également les commandes ci-dessous sur ce nœud.

```console
cd /var/opt/mssql/data
scp dbm_certificate.* root@<node2>:/var/opt/mssql/data/
```

À présent, sur les machines virtuelles secondaires qui exécutent l’instance secondaire et le réplica de configuration uniquement de SQL Server, exécutez les commandes suivantes pour que l’utilisateur mssql puisse posséder le certificat copié :

```console
cd /var/opt/mssql/data

chown mssql:mssql dbm_certificate.*
```

### <a name="create-the-certificate-on-secondary-servers"></a>Créer le certificat sur les serveurs secondaires

Le script Transact-SQL suivant crée une clé principale et un certificat à partir de la sauvegarde que vous avez créée sur le réplica SQL Server principal. Mettez à jour le script avec des mots de passe forts. Le mot de passe de déchiffrement est le même mot de passe que celui que vous avez utilisé pour créer le fichier .pvk à une étape précédente. Pour créer le certificat, exécutez le script suivant sur tous les serveurs secondaires à l’exception du réplica de configuration uniquement :

```tsql
CREATE MASTER KEY ENCRYPTION BY PASSWORD =
'<Master_Key_Password>';

CREATE CERTIFICATE dbm_certificate FROM FILE =
'/var/opt/mssql/data/dbm_certificate.cer'
WITH PRIVATE KEY ( FILE = '/var/opt/mssql/data/dbm_certificate.pvk',
DECRYPTION BY PASSWORD = '<Private_Key_Password>' );
```

### <a name="create-the-database-mirroring-endpoints-on-the-replicas"></a>Créer les points de terminaison de mise en miroir de bases de données sur les réplicas

Sur le réplica principal et le réplica secondaire, exécutez les commandes ci-dessous pour créer les points de terminaison de mise en miroir de bases de données :

```tsql
CREATE ENDPOINT [hadr_endpoint] AS TCP (LISTENER_PORT = 5022)
    FOR DATABASE_MIRRORING 
        (
        ROLE = WITNESS,
        AUTHENTICATION = CERTIFICATE dbm_certificate,
        ENCRYPTION = REQUIRED ALGORITHM AES
        );

ALTER ENDPOINT [hadr_endpoint] STATE = STARTED;
```

> [!NOTE]
> Le port 5022 est le port standard utilisé pour le point de terminaison de mise en miroir de bases de données, mais vous pouvez choisir n’importe quel autre port disponible.

Sur le réplica de configuration uniquement, créez le point de terminaison de mise en miroir de bases de données à l’aide de la commande suivante. Notez que le rôle est défini ici sur la valeur `WITNESS`, comme cela est requis pour le réplica de configuration uniquement.

```tsql
CREATE ENDPOINT [hadr_endpoint] AS TCP (LISTENER_PORT = 5022)
    FOR DATABASE_MIRRORING (
        ROLE = WITNESS,
        AUTHENTICATION = CERTIFICATE dbm_certificate,
        ENCRYPTION = REQUIRED ALGORITHM AES
        );

ALTER ENDPOINT [hadr_endpoint] STATE = STARTED;
```

### <a name="create-availability-group"></a>Créer un groupe de disponibilité

Sur l’instance du réplica principal, exécutez les commandes ci-dessous. Ces commandes créent un groupe de disponibilité nommé **ag1** qui a un `cluster_type` **External** et accorde l’autorisation de création de base de données au groupe de disponibilité.

Avant d’exécuter les scripts suivants, remplacez les espaces réservés `<node1>`, `<node2>` et `<node3>` (réplica de configuration uniquement) par le nom des trois machines virtuelles que vous avez créées aux étapes précédentes.

```tsql
CREATE AVAILABILITY GROUP [ag1]
    WITH (CLUSTER_TYPE = EXTERNAL)
    FOR REPLICA ON
    N'<node1>' WITH (
        ENDPOINT_URL = N'tcp://<node1>:<5022>',
        AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,
        FAILOVER_MODE = EXTERNAL,
        SEEDING_MODE = AUTOMATIC
        ),

    N'<node2>' WITH (
        ENDPOINT_URL = N'tcp://<node2>:\<5022>',
        AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,
        FAILOVER_MODE = EXTERNAL,
        SEEDING_MODE = AUTOMATIC
        ),
    
    N'<node3>' WITH (
        ENDPOINT_URL = N'tcp://<node3>:<5022>',
        AVAILABILITY_MODE = CONFIGURATION_ONLY
        );
          
ALTER AVAILABILITY GROUP [ag1] GRANT CREATE ANY DATABASE;
```

### <a name="join-the-secondary-replicas"></a>Joindre les réplicas secondaires

Exécutez les commandes ci-dessous sur tous les réplicas secondaires. Ces commandes joignent les réplicas secondaires au groupe de disponibilité « ag1 » contenant le réplica principal et accordent l’autorisation de création de base de données au groupe de disponibilité ag1.

```tsql
ALTER AVAILABILITY GROUP [ag1] JOIN WITH (CLUSTER_TYPE = EXTERNAL);
GO
ALTER AVAILABILITY GROUP [ag1] GRANT CREATE ANY DATABASE;
GO
```

### <a name="add-a-database-to-the-availability-group-created-above"></a>Ajouter une base de données au groupe de disponibilité créé précédemment

Connectez-vous au réplica principal et exécutez les commandes T-SQL ci-dessous pour :

1. Créer un exemple de base de données nommé **db1** qui sera ajouté au groupe de disponibilité.
2. Définir le mode de récupération complet (full) pour la base de données. Toutes les bases de données dans un groupe de disponibilité doivent être configurées sur le mode de récupération complet.
3. Sauvegardez la base de données. Une base de données doit avoir fait l’objet d’une sauvegarde complète au moins avant d’être ajoutée à un groupe de disponibilité.

```tsql
CREATE DATABASE [db1]; -- creates a database named db1
GO
ALTER DATABASE [db1] SET RECOVERY FULL; -- set the database in full recovery mode
GO
BACKUP DATABASE [db1] -- backs up the database to disk TO DISK = N'/var/opt/mssql/data/db1.bak';
GO
ALTER AVAILABILITY GROUP [ag1] ADD DATABASE [db1]; -- adds the database db1 to the AG
GO
```

Quand vous avez terminé les étapes précédentes, vous voyez que le groupe de disponibilité **ag1** a été créé et que les trois machines virtuelles ont été ajoutées en tant que réplicas (un réplica principal, un réplica secondaire et un réplica de configuration uniquement). **ag1** contient une seule base de données.

## <a name="deploy-the-sql-server-availability-group-workload-hpe-cluster-manager"></a>Déployer la charge de travail du groupe de disponibilité SQL Server (gestionnaire de cluster HPE)

Dans HPE Serviceguard, déployez la charge de travail SQL Server sur le groupe de disponibilité par le biais de l’interface utilisateur du gestionnaire de cluster Serviceguard.
   
Déployez la charge de travail du groupe de disponibilité, et activez la haute disponibilité (HA) et la reprise après sinistre (DR) via le cluster Serviceguard à l’aide de l’[interface graphique de Serviceguard Manager](https://support.hpe.com/hpesc/public/docDisplay?docId=a00107699en_us#Protect_your_alwayson_availability_group). Reportez-vous à la section **Protection de Microsoft SQL Server sur Linux pour les groupes de disponibilité Always On**.

## <a name="perform-automatic-failover-and-join-the-node-back-to-cluster"></a>Effectuer un basculement automatique et joindre à nouveau le nœud au cluster

Pour effectuer le test de basculement automatique, vous pouvez arrêter le réplica principal (mettez-le hors tension), ce qui permet de répliquer l’indisponibilité soudaine du nœud principal. Le comportement attendu est le suivant :

1. Le gestionnaire de cluster promeut l’un des réplicas secondaires du groupe de disponibilité en réplica principal.
2. Le réplica principal à l’arrêt rejoint automatiquement le cluster après sa sauvegarde. Le gestionnaire de cluster le promeut en réplica secondaire.

Pour HPE Serviceguard, reportez-vous à la section [**Tester la configuration pour la disponibilité du basculement**](https://support.hpe.com/hpesc/public/docDisplay?docId=a00107699en_us#Test_the_setup_preparedness)

## <a name="next-steps"></a>Étapes suivantes

Découvrez-en plus sur les [groupes de disponibilité Always On sur Linux](sql-server-linux-availability-group-overview.md).
