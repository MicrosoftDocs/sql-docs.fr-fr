---
title: Conseils d’installation pour SQL Server sur Linux
titleSuffix: SQL Server
description: Installez, mettez à jour et désinstallez SQL Server sur Linux. Cet article aborde des scénarios en ligne, hors connexion et sans assistance.
author: VanMSFT
ms.author: vanto
ms.date: 06/22/2020
ms.topic: conceptual
ms.prod: sql
ms.custom: sqlfreshmay19
ms.technology: linux
ms.assetid: 565156c3-7256-4e63-aaf0-884522ef2a52
ms.openlocfilehash: eacd871674d15bfa96cd77dd290ff9640520b58a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346422"
---
# <a name="installation-guidance-for-sql-server-on-linux"></a>Conseils d’installation pour SQL Server sur Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Cet article fournit des conseils sur l’installation, la mise à jour et la désinstallation de SQL Server 2017 et de SQL Server 2019 sur Linux.

Pour d’autres scénarios de déploiement, consultez :

- [Windows](../database-engine/install-windows/install-sql-server.md)
- [Conteneurs Docker](./sql-server-linux-docker-container-deployment.md)
- [Kubernetes - Clusters Big Data](../big-data-cluster/deploy-get-started.md)

> [!TIP]
> Ce guide aborde plusieurs scénarios de déploiement. Si vous recherchez uniquement des instructions d’installation pas à pas, passez à l’un des guides de démarrage rapide :
> - [Démarrage rapide RHEL](quickstart-install-connect-red-hat.md)
> - [Démarrage rapide SLES](quickstart-install-connect-suse.md)
> - [Démarrage rapide Ubuntu](quickstart-install-connect-ubuntu.md)
> - [Démarrage rapide Docker](quickstart-install-connect-docker.md)

Pour obtenir des réponses aux questions fréquemment posées, consultez la [FAQ de SQL Server sur Linux](../linux/sql-server-linux-faq.md).

## <a name="supported-platforms"></a><a id="supportedplatforms"></a> Plateformes prises en charge

SQL Server est actuellement pris en charge sur Red Hat Enterprise Server (RHEL), SUSE Linux Enterprise Server (SLES) et Ubuntu. Il est également pris en charge en tant qu’image Docker, qui peut s’exécuter sur un moteur Docker sur Linux ou Docker pour Windows/Mac.

<!--SQL Server 2017 on Linux-->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

| Plateforme | Version(s) prise(s) en charge | Obtenir
|-----|-----|-----
| **Red Hat Enterprise Linux** | 7.3, 7.4, 7.5, 7.6, 8  | [Obtenir RHEL 7.6](https://access.redhat.com/products/red-hat-enterprise-linux/evaluation)
| **SUSE Linux Enterprise Server** | v12 SP2 | [Obtenir SLES v12 SP2](https://www.suse.com/products/server)
| **Ubuntu** | 16.04 | [Obtenir Ubuntu 16.04](http://releases.ubuntu.com/xenial/)
| **Moteur Docker** | 1.8+ | [Obtenir Docker](https://www.docker.com/get-started)

::: moniker-end

<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

[!INCLUDE [linux-supported-platfoms-2019](../includes/linux-supported-platfoms-2019.md)]

::: moniker-end

Microsoft prend également en charge le déploiement et la gestion des conteneurs SQL Server à l’aide d’OpenShift et de Kubernetes.

> [!NOTE]
> SQL Server est testé et pris en charge sur Linux pour les distributions précédemment répertoriées. Toutefois, si vous choisissez d’installer SQL Server sur un système d’exploitation non pris en charge, consultez la section **Stratégie de support** de la [Stratégie de support technique pour Microsoft SQL Server](https://support.microsoft.com/help/4047326/support-policy-for-microsoft-sql-server) pour comprendre les implications du support.

## <a name="system-requirements"></a><a id="system"></a>Configuration requise

SQL Server présente la configuration requise suivante pour Linux :

|||
|-----|-----|
| **Mémoire** | 2 Go |
| **Système de fichiers** | **XFS** ou **EXT4** (les autres systèmes de fichiers, tels que **BTRFS** ne sont pas pris en charge) |
| **Espace disque** | 6 Go |
| **Vitesse du processeur** | 2 GHz |
| **Cœurs du processeur** | 2 cœurs |
| **Type de processeur** | compatible x64 uniquement |

Si vous utilisez des partages distants **NFS (Network File System)** en production, notez les exigences de support suivantes :

- Utilisez la version **4.2 ou ultérieure** de NFS. Les versions antérieures de NFS ne prennent pas en charge les fonctionnalités requises, telles que la création de fichiers alloués et partiellement alloués, communs aux systèmes de fichiers modernes.
- Localisez uniquement les répertoires **/var/opt/mssql** sur le montage NFS. D’autres fichiers, tels que les binaires du système SQL Server, ne sont pas pris en charge.
- Vérifiez que les clients NFS utilisent l’option « nolock » lors du montage du partage distant.

## <a name="configure-source-repositories"></a><a id="repositories"></a> Configurer les référentiels sources

Lorsque vous installez ou mettez à niveau SQL Server, vous recevez la dernière version de SQL Server à partir de votre référentiel Microsoft configuré. Les guides de démarrage rapide utilisent le référentiel **CU** de mise à jour cumulative pour SQL Server. Toutefois, vous pouvez configurer un référentiel **GDR**. Pour plus d’informations sur les référentiels ou sur leur configuration, consultez [Configurer les référentiels pour SQL Server sur Linux](sql-server-linux-change-repo.md).

## <a name="install-sql-server"></a><a id="platforms"></a> Installer SQL Server

Vous pouvez installer SQL Server 2017 ou SQL Server 2019 sur Linux à partir de la ligne de commande. Pour obtenir des instructions pas à pas, consultez une des rubriques de démarrage rapide suivantes :

| Plateforme | Guides de démarrage rapide pour l’installation |
|---|---|
| Red Hat Enterprise Linux (RHEL) | [2017](quickstart-install-connect-red-hat.md?view=sql-server-2017&preserve-view=true) \| [2019](quickstart-install-connect-red-hat.md?view=sql-server-linux-ver15&preserve-view=true) |
| SLES (SUSE Linux Enterprise Server) | [2017](quickstart-install-connect-suse.md?view=sql-server-2017&preserve-view=true) \| [2019](quickstart-install-connect-suse.md?view=sql-server-linux-ver15&preserve-view=true) |
| Ubuntu | [2017](quickstart-install-connect-ubuntu.md?view=sql-server-2017&preserve-view=true) \| [2019](quickstart-install-connect-ubuntu.md?view=sql-server-linux-ver15&preserve-view=true) |
| Docker | [2017](quickstart-install-connect-docker.md?view=sql-server-2017&preserve-view=true) \| [2019](quickstart-install-connect-docker.md?view=sql-server-linux-ver15&preserve-view=true) |

Vous pouvez également exécuter SQL Server sur Linux dans une machine virtuelle Azure. Pour plus d’informations, consultez [Provisionner une machine virtuelle SQL dans Azure](/azure/virtual-machines/linux/sql/provision-sql-server-linux-virtual-machine?toc=/sql/toc/toc.json).

Après l’installation, envisagez d’apporter des modifications de configuration supplémentaires pour des performances optimales. Pour plus d'informations, consultez [Meilleures pratiques relatives aux performances et lignes directrices de configuration pour SQL Server sur Linux](sql-server-linux-performance-best-practices.md).

## <a name="update-or-upgrade-sql-server"></a><a id="upgrade"></a> Mettre à jour ou mettre à niveau SQL Server

Pour mettre à jour le package **mssql-server** vers la version la plus récente, utilisez l’une des commandes suivantes en fonction de votre plateforme :

| Plateforme | Commande(s) de mise à jour de package |
|-----|-----|
| RHEL | `sudo yum update mssql-server` |
| SLES | `sudo zypper update mssql-server` |
| Ubuntu | `sudo apt-get update`<br/>`sudo apt-get install mssql-server` |

Ces commandes téléchargent le package le plus récent et remplacent les fichiers binaires situés sous `/opt/mssql/`. Les bases de données système et les bases de données générées par l’utilisateur ne sont pas affectées par cette opération.

Pour mettre à niveau SQL Server, commencez par [passer au référentiel configuré](sql-server-linux-change-repo.md) correspondant à la version souhaitée de SQL Server. Utilisez ensuite la même commande **update** pour mettre à niveau votre version de SQL Server. Cela n’est possible que si le chemin de mise à niveau est pris en charge entre les deux référentiels.

## <a name="rollback-sql-server"></a><a id="rollback"></a> Restauration de SQL Server

Pour restaurer ou passer SQL Server à une version antérieure, procédez comme suit :

1. Identifiez le numéro de version du package SQL Server que vous souhaitez passer à une version antérieure. Pour obtenir la liste des numéros de packages, consultez les [Notes de publication](../linux/sql-server-linux-release-notes.md).

1. Passer à une version antérieure de SQL Server. Dans les commandes suivantes, remplacez `<version_number>` par le numéro de version SQL Server que vous avez identifié à l’étape 1.

   | Plateforme | Commande(s) de mise à jour de package |
   |-----|-----|
   | RHEL | `sudo yum downgrade mssql-server-<version_number>.x86_64` |
   | SLES | `sudo zypper install --oldpackage mssql-server=<version_number>` |
   | Ubuntu | `sudo apt-get install mssql-server=<version_number>`<br/>`sudo systemctl start mssql-server` |

> [!NOTE]
> Le passage à une version antérieure est uniquement pris en charge au sein de la même version principale, par exemple SQL Server 2019.

## <a name="check-installed-sql-server-version"></a><a id="versioncheck"></a> Vérifier la version installée SQL Server

Pour vérifier la version et l’édition actuelles de votre SQL Server sur Linux, procédez comme suit :

1. S’il n’est pas déjà installé, installez les [outils en ligne de commande SQL Server](sql-server-linux-setup-tools.md).

1. Utilisez **sqlcmd** pour exécuter une commande Transact-SQL qui affiche la version et l’édition de votre SQL Server.

   ```bash
   sqlcmd -S localhost -U SA -Q 'select @@VERSION'
   ```

## <a name="uninstall-sql-server"></a><a id="uninstall"></a> Désinstaller SQL Server

Pour supprimer le package **mssql-server** sur Linux, utilisez une des commandes suivantes en fonction de votre plateforme :

| Plateforme | Commande(s) de suppression de package |
|-----|-----|
| RHEL | `sudo yum remove mssql-server` |
| SLES | `sudo zypper remove mssql-server` |
| Ubuntu | `sudo apt-get remove mssql-server` |

La suppression du package ne supprime pas les fichiers de base de données générés. Si vous souhaitez supprimer les fichiers de bases de données, utilisez la commande suivante :

```bash
sudo rm -rf /var/opt/mssql/
```

## <a name="unattended-install"></a><a id="unattended"></a> Installation sans assistance

Vous pouvez effectuer une installation sans assistance de la façon suivante :

- Suivez les étapes initiales des [démarrages rapides](#platforms) pour inscrire les référentiels et installer SQL Server.
- Lorsque vous exécutez `mssql-conf setup`, définissez des [variables d’environnement](sql-server-linux-configure-environment-variables.md) et utilisez l’option (aucune invite) `-n`.

L’exemple suivant configure l’édition Développeur de SQL Server avec la variable d’environnement **MSSQL_PID**. Il accepte également le CLUF (**ACCEPT_EULA**) et définit le mot de passe de l'utilisateur SA (**MSSQL_SA_PASSWORD**). Le paramètre `-n` effectue une installation non demandée dans laquelle les valeurs de configuration sont extraites des variables d’environnement.

```bash
sudo MSSQL_PID=Developer ACCEPT_EULA=Y MSSQL_SA_PASSWORD='<YourStrong!Passw0rd>' /opt/mssql/bin/mssql-conf -n setup
```

Vous pouvez également créer un script qui effectue d’autres actions. Par exemple, vous pouvez installer d’autres packages de SQL Server.

Pour obtenir un exemple de script plus détaillé, consultez les exemples suivants :

- [Script d’installation sans assistance de Red Hat](sample-unattended-install-redhat.md)
- [Script d’installation sans assistance de SUSE](sample-unattended-install-suse.md)
- [Script d’installation sans assistance de Ubuntu](sample-unattended-install-ubuntu.md)

## <a name="offline-install"></a><a id="offline"></a> Installation hors connexion

Si votre machine Linux n’a pas d’accès aux référentiels en ligne utilisés dans les [démarrages rapides](#platforms), vous pouvez télécharger directement les fichiers du package. Ces packages se trouvent dans le référentiel Microsoft, [https://packages.microsoft.com](https://packages.microsoft.com).

> [!TIP]
> Si vous avez réussi à installer en suivant les étapes des démarrages rapides, vous n’avez pas besoin de télécharger ni d’installer manuellement le ou les packages SQL Server. Cette section concerne seulement le scénario hors connexion.

1. **Téléchargez le package du moteur de base de données pour votre plateforme**. Recherchez les liens de téléchargement de packages dans la section Détails du package des [Notes de publication](../linux/sql-server-linux-release-notes.md).

1. **Déplacez le package téléchargé sur votre machine Linux**. Si vous avez utilisé une autre machine pour télécharger les packages, vous pouvez déplacer les packages vers votre machine Linux à l’aide de la commande **scp**.

1. **Installez le package du moteur de base de données**. Utilisez une des commandes suivantes en fonction de votre plateforme. Remplacez le nom du fichier de package dans cet exemple par le nom exact que vous avez téléchargé.

   | Plateforme | Commande d’installation de package |
   |-----|-----|
   | RHEL | `sudo yum localinstall mssql-server_versionnumber.x86_64.rpm` |
   | SLES | `sudo zypper install mssql-server_versionnumber.x86_64.rpm` |
   | Ubuntu | `sudo dpkg -i mssql-server_versionnumber_amd64.deb` |

    > [!NOTE]
    > Vous pouvez également installer les packages RPM (RHEL et SLES) à l'aide de la commande `rpm -ivh`, mais les commandes du tableau précédent installent automatiquement les dépendances si elles sont disponibles à partir de référentiels approuvés.

1. **Résoudre des dépendances manquantes** : Vous avez peut-être des dépendances manquantes à ce stade. Si ce n’est pas le cas, vous pouvez ignorer cette étape. Sur Ubuntu, si vous avez accès à des référentiels approuvés contenant ces dépendances, la solution la plus simple consiste à utiliser la commande `apt-get -f install`. Cette commande termine également l’installation de SQL Server. Pour inspecter manuellement les dépendances, utilisez les commandes suivantes :

   | Plateforme | Répertorier la commande des dépendances |
   |-----|-----|
   | RHEL | `rpm -qpR mssql-server_versionnumber.x86_64.rpm` |
   | SLES | `rpm -qpR mssql-server_versionnumber.x86_64.rpm` |
   | Ubuntu | `dpkg -I mssql-server_versionnumber_amd64.deb` |

   Après avoir résolu les dépendances manquantes, essayez de réinstaller le package mssql-server.

1. **Terminer la configuration de SQL Server**. Utilisez **mssql-conf** pour terminer la configuration de SQL Server :

   ```bash
   sudo /opt/mssql/bin/mssql-conf setup
   ```

## <a name="licensing-and-pricing"></a>Licences et tarifs

SQL Server est sous licence pour Linux et Windows. Pour plus d’informations sur les licences et les tarifs de SQL Server, consultez la rubrique [Comment faire pour obtenir une licence SQL Server](https://www.microsoft.com/sql-server/sql-server-2017-pricing).

## <a name="optional-sql-server-features"></a>Fonctionnalités SQL Server facultatives

Après l’installation, vous pouvez également installer ou activer des fonctionnalités de SQL Server facultatives.

- [Outils en ligne de commande SQL Server](sql-server-linux-setup-tools.md)
- [SQL Server Agent](sql-server-linux-setup-sql-agent.md)
- [Recherche en texte intégral SQL Server](sql-server-linux-setup-full-text-search.md)
- [Machine Learning Services (R, Python)](sql-server-linux-setup-machine-learning.md)
- [SQL Server Integration Services](sql-server-linux-setup-ssis.md)

[!INCLUDE[Get Help Options](../includes/paragraph-content/get-help-options.md)]

> [!TIP]
> Pour obtenir des réponses aux questions fréquemment posées, consultez la [FAQ de SQL Server sur Linux](sql-server-linux-faq.md).
