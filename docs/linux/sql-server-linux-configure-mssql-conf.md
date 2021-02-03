---
title: Configurer les paramètres SQL Server sur Linux
description: Cet article explique comment utiliser l'outil mssql-conf pour configurer les paramètres SQL Server sur Linux.
author: VanMSFT
ms.author: vanto
ms.date: 08/12/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 06798dff-65c7-43e0-9ab3-ffb23374b322
ms.openlocfilehash: 72d5179d64cda06a95e531d8cbc847b33ce9b92f
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237244"
---
# <a name="configure-sql-server-on-linux-with-the-mssql-conf-tool"></a>Configurer SQL Server sur Linux avec l'outil mssql-conf

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

<!--SQL Server 2017 on Linux-->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

**mssql-conf** est un script de configuration qui s’installe avec SQL Server 2017 pour Red Hat Enterprise Linux, SUSE Linux Enterprise Server et Ubuntu. Il modifie le [**fichier mssql.conf**](#mssql-conf-format) dans lequel les valeurs de configuration sont stockées. Vous pouvez utiliser l’utilitaire **mssql-conf** pour définir les paramètres suivants :

|Paramètre|Description|
|---|---|
| [Agent](#agent) | Activer l’agent SQL Server. |
| [Classement](#collation) | Définir un nouveau classement pour SQL Server sur Linux. |
| [Feedback des clients](#customerfeedback) | Choisir si SQL Server envoie ou non un feedback à Microsoft. |
| [Profil de messagerie de base de données](#dbmail) | Définir le profil de messagerie de base de données par défaut pour SQL Server sur Linux. |
| [Répertoire par défaut des données](#datadir) | Modifier le répertoire par défaut des nouveaux fichiers de données de la base de données SQL Server (.mdf). |
| [Répertoire par défaut des journaux](#datadir) | Modifier le répertoire par défaut des nouveaux fichiers journaux de la base de données SQL Server (.mdf). |
| [Répertoire par défaut de la base de données master](#masterdatabasedir) | Modifier le répertoire par défaut de la base de données master et des fichiers journaux.|
| [Nom par défaut du fichier de la base de données master](#masterdatabasename) | Modifier le nom des fichiers de la base de données master. |
| [Répertoire par défaut des images mémoire](#dumpdir) | Modifier le répertoire par défaut des nouvelles images mémoire et autres fichiers de dépannage. |
| [Répertoire par défaut des journaux d’erreurs](#errorlogdir) | Modifier le répertoire par défaut des nouveaux fichiers SQL Server ErrorLog, Default Profiler Trace, System Health Session XE et Hekaton Session XE. |
| [Répertoire par défaut des sauvegardes](#backupdir) | Modifier le répertoire par défaut des nouveaux fichiers de sauvegarde. |
| [Type d’image mémoire](#coredump) | Choisir le type de fichier image mémoire à collecter. |
| [Haute disponibilité](#hadr) | Activer les groupes de disponibilité. |
| [Répertoire d’audit local](#localaudit) | Définir un répertoire pour ajouter des fichiers d’audit locaux. |
| [Paramètres régionaux](#lcid) | Définir les paramètres régionaux que SQL Server doit utiliser. |
| [Limite de mémoire](#memorylimit) | Définir la limite de mémoire pour SQL Server. |
| [Paramètres réseau](#network) | Paramètres réseau supplémentaires pour SQL Server. |
| [Microsoft Distributed Transaction Coordinator](#msdtc) | Configurer et dépanner MSDTC sur Linux. |
| [Port TCP](#tcpport) | Modifier le port sur lequel SQL Server écoute les connexions. |
| [TLS](#tls) | Configurer Transport Level Security. |
| [Indicateurs de trace](#traceflags) | Définir les indicateurs de trace que le service va utiliser. |

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

**mssql-conf** est un script de configuration qui s’installe avec [!INCLUDE[SQL Server 2019](../includes/sssql19-md.md)] pour Red Hat Enterprise Linux, SUSE Linux Enterprise Server et Ubuntu. Vous pouvez utiliser cet utilitaire pour définir les paramètres suivants :

|Paramètre|Description|
|---|---|
| [Agent](#agent) | Activer SQL Server Agent |
| [Classement](#collation) | Définir un nouveau classement pour SQL Server sur Linux. |
| [Feedback des clients](#customerfeedback) | Choisir si SQL Server envoie ou non un feedback à Microsoft. |
| [Profil de messagerie de base de données](#dbmail) | Définir le profil de messagerie de base de données par défaut pour SQL Server sur Linux. |
| [Répertoire par défaut des données](#datadir) | Modifier le répertoire par défaut des nouveaux fichiers de données de la base de données SQL Server (.mdf). |
| [Répertoire par défaut des journaux](#datadir) | Modifier le répertoire par défaut des nouveaux fichiers journaux de la base de données SQL Server (.mdf). |
| [Répertoire par défaut des fichiers de la base de données master](#masterdatabasedir) | Modifier le répertoire par défaut des fichiers de la base de données master sur l’installation SQL existante.|
| [Nom par défaut du fichier de la base de données master](#masterdatabasename) | Modifier le nom des fichiers de la base de données master. |
| [Répertoire par défaut des images mémoire](#dumpdir) | Modifier le répertoire par défaut des nouvelles images mémoire et autres fichiers de dépannage. |
| [Répertoire par défaut des journaux d’erreurs](#errorlogdir) | Modifier le répertoire par défaut des nouveaux fichiers SQL Server ErrorLog, Default Profiler Trace, System Health Session XE et Hekaton Session XE. |
| [Répertoire par défaut des sauvegardes](#backupdir) | Modifier le répertoire par défaut des nouveaux fichiers de sauvegarde. |
| [Type d’image mémoire](#coredump) | Choisir le type de fichier image mémoire à collecter. |
| [Haute disponibilité](#hadr) | Activer les groupes de disponibilité. |
| [Répertoire d’audit local](#localaudit) | Définir un répertoire pour ajouter des fichiers d’audit locaux. |
| [Paramètres régionaux](#lcid) | Définir les paramètres régionaux que SQL Server doit utiliser. |
| [Limite de mémoire](#memorylimit) | Définir la limite de mémoire pour SQL Server. |
| [Microsoft Distributed Transaction Coordinator](#msdtc) | Configurer et dépanner MSDTC sur Linux. |
| [CLUF MLServices](#mlservices-eula) | Accepter les CLUF R et Python pour les packages mlservices. S'applique à SQL Server 2019 uniquement.|
| [Paramètres réseau](#network) | Paramètres réseau supplémentaires pour SQL Server. |
| [outboundnetworkaccess](#mlservices-outbound-access) |Activer l'accès réseau sortant pour les extensions [mlservices](sql-server-linux-setup-machine-learning.md) R, Python et Java.|
| [Port TCP](#tcpport) | Modifier le port sur lequel SQL Server écoute les connexions. |
| [TLS](#tls) | Configurer Transport Level Security. |
| [Indicateurs de trace](#traceflags) | Définir les indicateurs de trace que le service va utiliser. |

::: moniker-end

> [!TIP]
> Certains de ces paramètres peuvent également être configurés avec des variables d'environnement. Pour plus d’informations, voir [Configurer des paramètres SQL Server à l’aide de variables d’environnement](sql-server-linux-configure-environment-variables.md).

## <a name="usage-tips"></a>Conseils d’utilisation

* Pour les groupes de disponibilité Always On et les clusters de disques partagés, effectuez toujours les mêmes modifications de configuration sur chaque nœud.

* Pour le scénario de cluster de disques partagés, n'essayez pas de redémarrer le service **mssql-server** pour appliquer les modifications. SQL Server fonctionne comme une application. Mettez plutôt la ressource hors ligne, puis de nouveau en ligne.

* Ces exemples exécutent mssql-conf en spécifiant le chemin complet : **/opt/mssql/bin/mssql-conf**. Si vous choisissez d’accéder plutôt à ce chemin, exécutez mssql-conf dans le contexte du répertoire actuel : **./mssql-conf**.

## <a name="enable-sql-server-agent"></a><a id="agent"></a> Activer l’agent SQL Server

Le paramètre **sqlagent.enabled** active [SQL Server Agent](sql-server-linux-run-sql-server-agent-job.md). Par défaut, SQL Server Agent est désactivé. Si **sqlagent.enabled** n'est pas présent dans le fichier de configuration mssql.conf, alors SQL Server suppose en interne que SQL Server Agent est désactivé.

Pour modifier ce paramètre, procédez comme suit :

1. Activez SQL Server Agent :

   ```bash
   sudo /opt/mssql/bin/mssql-conf set sqlagent.enabled true 
   ```

2. Redémarrez le service SQL Server :

   ```bash
   sudo systemctl restart mssql-server
   ```

### <a name="set-the-default-database-mail-profile-for-sql-server-on-linux"></a><a id="dbmail"></a> Définir le profil de messagerie de base de données par défaut pour SQL Server sur Linux

L’option **sqlpagent.databasemailprofile** vous permet de définir le profil par défaut de DB Mail pour les alertes par e-mail.

```bash
sudo /opt/mssql/bin/mssql-conf set sqlagent.databasemailprofile <profile_name>
```

### <a name="sql-agent-error-logs"></a><a id="agenterrorlog"></a> Journaux d’erreurs SQL Agent

Les paramètres **sqlpagent.errorlogfile** et **sqlpagent.errorlogginglevel** vous permettent de définir le chemin d’accès au fichier journal SQL Agent et le niveau de journalisation, respectivement. 

```bash
sudo /opt/mssql/bin/mssql-conf set sqlagent.errorfile <path>
```

Les niveaux de journalisation SQL Agent correspondent aux valeurs de masque de bits suivantes :

- 1 = Erreurs
- 2 = Avertissements
- 4 = Info

Si vous souhaitez capturer tous les niveaux, utilisez `7` comme valeur.

```bash
sudo /opt/mssql/bin/mssql-conf set sqlagent.errorlogginglevel <level>
```

## <a name="change-the-sql-server-collation"></a><a id="collation"></a> Modifier le classement SQL Server

L'option **set-collation** modifie la valeur de classement en appliquant l’un des classements pris en charge.

1. Tout d'abord, [sauvegardez toutes les bases de données utilisateur](sql-server-linux-backup-and-restore-database.md) sur votre serveur.

1. Utilisez ensuite la procédure stockée [sp_detach_db](../relational-databases/system-stored-procedures/sp-detach-db-transact-sql.md) pour détacher les bases de données utilisateur.

1. Exécutez l'option **set-collation** et suivez les invites :

   ```bash
   sudo /opt/mssql/bin/mssql-conf set-collation
   ```

1. L'utilitaire mssql-conf tentera d’appliquer la valeur de classement spécifiée et de redémarrer le service. En cas d’erreur, le classement retourne à la valeur précédente.

1. Restaurez vos sauvegardes de bases de données utilisateur.

Pour obtenir une liste des classements pris en charge, exécutez la fonction [sys.fn_helpcollations](../relational-databases/system-functions/sys-fn-helpcollations-transact-sql.md) : `SELECT Name from sys.fn_helpcollations()`.

## <a name="configure-customer-feedback"></a><a id="customerfeedback"></a> Configurer le feedback des clients

Le paramètre **telemetry.customerfeedback** change si SQL Server envoie ou non un feedback à Microsoft. Par défaut, cette valeur est définie sur **true** pour toutes les éditions. Pour modifier la valeur, exécutez les commandes suivantes :

> [!IMPORTANT]
> Vous ne pouvez pas désactiver le feedback des clients pour les éditions gratuites de SQL Server, Express et Développeur.

1. Exécutez le script mssql-conf en tant que root avec la commande **set** pour **telemetry.customerfeedback**. L'exemple suivant désactive le feedback des clients client en spécifiant **false**.

   ```bash
   sudo /opt/mssql/bin/mssql-conf set telemetry.customerfeedback false
   ```

1. Redémarrez le service SQL Server :

   ```bash
   sudo systemctl restart mssql-server
   ```

Pour plus d'informations, voir [Commentaires client pour SQL Server sur Linux](./usage-and-diagnostic-data-configuration-for-sql-server-linux.md) et la [Déclaration de confidentialité SQL Server](../sql-server/sql-server-privacy.md).

## <a name="change-the-default-data-or-log-directory-location"></a><a id="datadir"></a> Modifier l'emplacement par défaut du répertoire des données ou des journaux

Les paramètres **filelocation.defaultdatadir** et **filelocation.defaultlogdir** modifient l'emplacement où sont créés les nouveaux fichiers des bases de données et des journaux. Par défaut, cet emplacement est /var/opt/mssql/data. Pour modifier ces paramètres, procédez comme suit :

1. Créez le répertoire cible des nouveaux fichiers de données de base de données et fichiers journaux. L’exemple suivant crée un répertoire **/tmp/data** :

   ```bash
   sudo mkdir /tmp/data
   ```

1. Remplacez le propriétaire et le groupe du répertoire par l’utilisateur **mssql** :

   ```bash
   sudo chown mssql /tmp/data
   sudo chgrp mssql /tmp/data
   ```

1. Utilisez mssql-conf pour modifier le répertoire par défaut des données avec la commande **set** :

   ```bash
   sudo /opt/mssql/bin/mssql-conf set filelocation.defaultdatadir /tmp/data
   ```

1. Redémarrez le service SQL Server :

   ```bash
   sudo systemctl restart mssql-server
   ```

1. Maintenant, tous les fichiers de base de données des nouvelles bases de données créées seront stockés dans ce nouvel emplacement. Si vous souhaitez changer l'emplacement des fichiers journaux (.ldf) des nouvelles bases de données, vous pouvez utiliser la commande « set » suivante :

   ```bash
   sudo /opt/mssql/bin/mssql-conf set filelocation.defaultlogdir /tmp/log
   ```

1. Cette commande suppose également qu'un répertoire /tmp/log existe, et qu'il se trouve sous le paramètre **mssql** de l’utilisateur et du groupe.


## <a name="change-the-default-master-database-file-directory-location"></a><a id="masterdatabasedir"></a> Modifier l’emplacement du répertoire par défaut des fichiers de la base de données master

Les paramètres **filelocation.masterdatafile** et **filelocation.masterlogfile** modifient l'emplacement où le moteur SQL Server recherche les fichiers de la base de données master. Par défaut, cet emplacement est /var/opt/mssql/data.

Pour modifier ces paramètres, procédez comme suit :

1. Créez le répertoire cible des nouveaux fichiers journaux d’erreurs. L’exemple suivant crée un répertoire **/tmp/masterdatabasedir** :

   ```bash
   sudo mkdir /tmp/masterdatabasedir
   ```

1. Remplacez le propriétaire et le groupe du répertoire par l’utilisateur **mssql** :

   ```bash
   sudo chown mssql /tmp/masterdatabasedir
   sudo chgrp mssql /tmp/masterdatabasedir
   ```

1. Utilisez mssql-conf pour modifier le répertoire par défaut de la base de données pour les données master et les fichiers journaux avec la commande **set** :

   ```bash
   sudo /opt/mssql/bin/mssql-conf set filelocation.masterdatafile /tmp/masterdatabasedir/master.mdf
   sudo /opt/mssql/bin/mssql-conf set filelocation.masterlogfile /tmp/masterdatabasedir/mastlog.ldf
   ```

   > [!NOTE]
   > En plus de déplacer les données de la base et les fichiers journaux, cette commande déplace également l'emplacement par défaut de toutes les autres bases de données du système.

1. Arrêtez le service SQL Server :

   ```bash
   sudo systemctl stop mssql-server
   ```

1. Déplacez les fichiers master.mdf et masterlog.ldf :

   ```bash
   sudo mv /var/opt/mssql/data/master.mdf /tmp/masterdatabasedir/master.mdf 
   sudo mv /var/opt/mssql/data/mastlog.ldf /tmp/masterdatabasedir/mastlog.ldf
   ```

1. Démarrez le service SQL Server :

   ```bash
   sudo systemctl start mssql-server
   ```

   > [!NOTE]
   > Si SQL Server ne trouve pas les fichiers master.mdf et mastlog.ldf dans le répertoire spécifié, une copie modèle des bases de données système sera automatiquement créée dans le répertoire spécifié, et SQL Server démarrera avec succès. Toutefois, les métadonnées telles que les bases de données utilisateurs, les connexions au serveur, les certificats de serveur, les clés de chiffrement, les tâches de l’agent SQL ou l'ancien mot de passe de connexion AS ne seront pas mis à jour dans la nouvelle base de données master. Vous devrez arrêter SQL Server et déplacer vos anciens fichiers master.mdf et mastlog.ldf vers le nouvel emplacement spécifié, puis démarrer SQL Server pour continuer à utiliser les métadonnées existantes.
 
## <a name="change-the-name-of-master-database-files"></a><a id="masterdatabasename"></a> Modifier le nom des fichiers de la base de données master

Les paramètres **filelocation.masterdatafile** et **filelocation.masterlogfile** modifient l'emplacement où le moteur SQL Server recherche les fichiers de la base de données master. Ils permettent également de modifier le nom de la base de données master et des fichiers journaux. 

Pour modifier ces paramètres, procédez comme suit :

1. Arrêtez le service SQL Server :

   ```bash
   sudo systemctl stop mssql-server
   ```

1. Utilisez mssql-conf pour modifier les noms attendus de la base de données pour les données master et les fichiers journaux avec la commande **set** :

   ```bash
   sudo /opt/mssql/bin/mssql-conf set filelocation.masterdatafile /var/opt/mssql/data/masternew.mdf
   sudo /opt/mssql/bin/mssql-conf set filelocation.mastlogfile /var/opt/mssql/data/mastlognew.ldf
   ```

   > [!IMPORTANT]
   > Vous ne pouvez modifier le nom de la base de données master et des fichiers journaux qu'après le démarrage réussi de SQL Server. Avant l'exécution initiale, SQL Server s'attend à ce que les fichiers soient nommés master.mdf et mastlog.ldf.

1. Modifier le nom des données de la base de données de base et des fichiers journaux 

   ```bash
   sudo mv /var/opt/mssql/data/master.mdf /var/opt/mssql/data/masternew.mdf
   sudo mv /var/opt/mssql/data/mastlog.ldf /var/opt/mssql/data/mastlognew.ldf
   ```

1. Démarrez le service SQL Server :

   ```bash
   sudo systemctl start mssql-server
   ```

## <a name="change-the-default-dump-directory-location"></a><a id="dumpdir"></a> Modifier l'emplacement par défaut du répertoire des images mémoire

Le paramètre **filelocation.defaultdumpdir** change l'emplacement par défaut où la mémoire et les images mémoire SQL sont générés chaque fois qu'un incident se produit. Par défaut, ces fichiers sont générés dans /var/opt/mssql/log.

Pour configurer ce nouvel emplacement, utilisez les commandes suivantes :

1. Créez le répertoire cible pour les nouveaux fichiers image mémoire. L’exemple suivant crée un répertoire **/tmp/dump** :

   ```bash
   sudo mkdir /tmp/dump
   ```

1. Remplacez le propriétaire et le groupe du répertoire par l’utilisateur **mssql** :

   ```bash
   sudo chown mssql /tmp/dump
   sudo chgrp mssql /tmp/dump
   ```

1. Utilisez mssql-conf pour modifier le répertoire par défaut des données avec la commande **set** :

   ```bash
   sudo /opt/mssql/bin/mssql-conf set filelocation.defaultdumpdir /tmp/dump
   ```

1. Redémarrez le service SQL Server :

   ```bash
   sudo systemctl restart mssql-server
   ```

## <a name="change-the-default-error-log-file-directory-location"></a><a id="errorlogdir"></a> Modifier l'emplacement par défaut des fichiers journaux d’erreurs

Le paramètre **filelocation.errorlogfile** modifie l'emplacement où sont créés les nouveaux fichiers journaux d'erreurs, les fichiers de suivi du profileur par défaut, les fichiers XE de session d’intégrité système, et les fichiers XE de session Hekaton. Par défaut, cet emplacement est /var/opt/mssql/log. Le répertoire dans lequel le fichier journal d'erreurs SQL est défini devient le répertoire par défaut des autres journaux.

Pour modifier ces paramètres :

1. Créez le répertoire cible des nouveaux fichiers journaux d’erreurs. L’exemple suivant crée un répertoire **/tmp/logs** :

   ```bash
   sudo mkdir /tmp/logs
   ```

1. Remplacez le propriétaire et le groupe du répertoire par l’utilisateur **mssql** :

   ```bash
   sudo chown mssql /tmp/logs
   sudo chgrp mssql /tmp/logs
   ```

1. Utilisez mssql-conf pour modifier le nom de fichier par défaut du journal d’erreurs avec la commande **set** :

   ```bash
   sudo /opt/mssql/bin/mssql-conf set filelocation.errorlogfile /tmp/logs/errorlog
   ```

1. Redémarrez le service SQL Server :

   ```bash
   sudo systemctl restart mssql-server
   ```

Le paramètre `errorlog.numerrorlogs` vous permet de spécifier le nombre de journaux d’erreurs maintenus avant le cycle du journal.

## <a name="change-the-default-backup-directory-location"></a><a id="backupdir"></a> Modifier l’emplacement par défaut du répertoire de sauvegarde

Le paramètre **filelocation.defaultbackupdir** modifie l'emplacement par défaut où les fichiers de sauvegarde sont générés. Par défaut, ces fichiers sont générés dans /var/opt/mssql/data.

Pour configurer ce nouvel emplacement, utilisez les commandes suivantes :

1. Créez le répertoire cible pour les nouveaux fichiers de sauvegarde. L’exemple suivant crée un répertoire **/tmp/backup** :

   ```bash
   sudo mkdir /tmp/backup
   ```

1. Remplacez le propriétaire et le groupe du répertoire par l’utilisateur **mssql** :

   ```bash
   sudo chown mssql /tmp/backup
   sudo chgrp mssql /tmp/backup
   ```

1. Utilisez mssql-conf pour modifier le répertoire de sauvegarde par défaut avec la commande « set » :

   ```bash
   sudo /opt/mssql/bin/mssql-conf set filelocation.defaultbackupdir /tmp/backup
   ```

1. Redémarrez le service SQL Server :

   ```bash
   sudo systemctl restart mssql-server
   ```

## <a name="specify-core-dump-settings"></a><a id="coredump"></a> Spécifier les paramètres de vidage principal

Si une exception se produit dans l'un des processus SQL Server, SQL Server crée une image mémoire.

Il existe deux options pour contrôler le type d’image mémoire que SQL Server collecte : **coredump.coredumptype** et **coredump.captureminiandfull**. Elles se rapportent aux deux phases de la capture d’image mémoire principale. 

La première phase de capture est contrôlée par le paramètre **coredump.coredumptype**, qui détermine le type de fichier image mémoire généré pendant une exception. La seconde phase est déclenchée lorsque le paramètre **coredump.captureminiandfull** est activé. Si **coredump.captureminiandfull** est défini sur true, le fichier image mémoire spécifié par **coredump.coredumptype** est généré, et un second vidage minimal est également généré. Définir **coredump.captureminiandfull** sur false désactive la seconde tentative de capture.

1. Décidez si vous devez capturer les vidages minimaux et complets avec le paramètre **coredump.captureminiandfull**.

    ```bash
    sudo /opt/mssql/bin/mssql-conf set coredump.captureminiandfull <true or false>
    ```

    Valeur par défaut : **false**

1. Spécifiez le type de fichier image mémoire à l’aide du paramètre **coredump.coredumptype**.

    ```bash
    sudo /opt/mssql/bin/mssql-conf set coredump.coredumptype <dump_type>
    ```

    Valeur par défaut : **miniplus**

    Le tableau suivant répertorie les valeurs possibles pour **coredump.coredumptype**.

    | Type | Description |
    |-----|-----|
    | **mini** | La valeur Mini est le plus petit type de fichier image mémoire. Il utilise les informations du système Linux pour déterminer les threads et les modules du processus. L’image mémoire ne contient que les piles de threads et les modules de l'environnement hôte. Il ne contient pas de références ou de variables globales indirectes de mémoire. |
    | **miniplus** | La valeur MiniPlus est similaire à Mini, mais inclut plus de mémoire. Elle comprend les mécanismes internes de SQLPAL et de l'environnement hôte, en ajoutant les régions de mémoire suivantes au fichier mémoire :</br></br> - Diverses variables globales</br> - Toutes les mémoires de plus de 64 To</br> - Toutes les régions nommées se trouvent dans **/proc/$pid/maps**</br> - Mémoire indirecte des threads et des piles</br> - Informations sur le thread</br> - Associé à Teb et Peb</br> - Informations sur le module</br> - Arborescence VMM et VAD |
    | **filtered** | Filtered utilise une conception basée sur la soustraction où toute la mémoire du processus est incluse, sauf si elle est spécifiquement exclue. La conception comprend les mécanismes internes de SQLPAL et de l'environnement hôte, en soustrayant les régions de mémoire suivantes de l’image mémoire.
    | **full** | Full correspond à un vidage de processus complet qui comprend toutes les régions situées dans **/proc/$pid/maps**. Cette opération n'est pas contrôlée par le paramètre **coredump.captureminiandfull**. |

## <a name="high-availability"></a><a id="hadr"></a> Haute disponibilité

L'option **hadr.hadrenabled** active les groupes de disponibilité sur votre instance SQL Server. La commande suivante active les groupes de disponibilité en définissant **hadr.hadrenabled** sur 1. Vous devez redémarrer SQL Server pour appliquer le paramètre.

```bash
sudo /opt/mssql/bin/mssql-conf set hadr.hadrenabled  1
sudo systemctl restart mssql-server
```

Pour plus d'informations sur son utilisation avec des groupes de disponibilité, reportez-vous aux deux rubriques suivantes.

- [Configurer un groupe de disponibilité Always On pour SQL Server sur Linux](sql-server-linux-availability-group-configure-ha.md)
- [Configurer un groupe de disponibilité d’échelle lecture pour SQL Server sur Linux](sql-server-linux-availability-group-configure-rs.md)


## <a name="set-local-audit-directory"></a><a id="localaudit"></a> Définir le répertoire d'audit local

L’option **telemetry.userrequestedlocalauditdirectory** active l’audit local et vous permet de définir le répertoire dans lequel les journaux d’audit locaux sont créés.

1. Créez un répertoire cible pour les nouveaux journaux d’audit local. L’exemple suivant crée un répertoire **/tmp/audit** :

   ```bash
   sudo mkdir /tmp/audit
   ```

1. Remplacez le propriétaire et le groupe du répertoire par l’utilisateur **mssql** :

   ```bash
   sudo chown mssql /tmp/audit
   sudo chgrp mssql /tmp/audit
   ```

1. Exécutez le script mssql-conf en tant que root avec la commande **set** pour **telemetry.userrequestedlocalauditdirectory** :

   ```bash
   sudo /opt/mssql/bin/mssql-conf set telemetry.userrequestedlocalauditdirectory /tmp/audit
   ```

1. Redémarrez le service SQL Server :

   ```bash
   sudo systemctl restart mssql-server
   ```

Pour plus d'informations, voir [Commentaires client pour SQL Server sur Linux](./usage-and-diagnostic-data-configuration-for-sql-server-linux.md).

## <a name="change-the-sql-server-locale"></a><a id="lcid"></a> Modifier les paramètres régionaux SQL Server

Le paramètre **language.lcid** remplace les paramètres régionaux de SQL Server par un identificateur de langue (LCID) pris en charge. 

1. L'exemple suivant sélectionne la langue Français (1036) :

   ```bash
   sudo /opt/mssql/bin/mssql-conf set language.lcid 1036
   ```

1. Redémarrez le service SQL Server pour appliquer les modifications :

   ```bash
   sudo systemctl restart mssql-server
   ```

## <a name="set-the-memory-limit"></a><a id="memorylimit"></a> Définir la limite de mémoire

Le paramètre **memory.memorylimitmb** contrôle la quantité de mémoire physique (en Mo) disponible pour SQL Server. La valeur par défaut est 80 % de la mémoire physique.

1. Exécutez le script mssql-conf en tant que root avec la commande **set** pour **memory.memorylimitmb**. L'exemple suivant définit la mémoire disponible pour SQL Server sur 3,25 Go (3 328 Mo).

   ```bash
   sudo /opt/mssql/bin/mssql-conf set memory.memorylimitmb 3328
   ```

1. Redémarrez le service SQL Server pour appliquer les modifications :

   ```bash
   sudo systemctl restart mssql-server
   ```

### <a name="additional-memory-settings"></a>Paramètres de mémoire supplémentaires

Les options suivantes sont disponibles pour les paramètres de mémoire.

|Option |Description |
|--- |--- |
| memory.disablememorypressure | Désactiver la sollicitation de mémoire sur SQL Server. Les valeurs peuvent être `true` ou `false`. |
| memory.memory_optimized | Activez ou désactivez les fonctionnalités SQL Server à mémoire optimisée : état d’éveil du fichier de mémoire persistante et protection de la mémoire. Les valeurs peuvent être `true` ou `false`. |

## <a name="configure-msdtc"></a><a id="msdtc"></a> Configurer MSDTC

Les paramètres **network.rpcport** et **distributedtransaction.servertcpport** servent à configurer MSDTC (Microsoft Distributed Transaction Coordinator). Pour modifier ces paramètres, exécutez les commandes suivantes :

1. Exécutez le script mssql-conf en tant que root avec la commande **set** pour « network.rpcport » :

   ```bash
   sudo /opt/mssql/bin/mssql-conf set network.rpcport <rcp_port>
   ```

2. Définissez ensuite le paramètre « distributedtransaction.servertcpport » :

   ```bash
   sudo /opt/mssql/bin/mssql-conf set distributedtransaction.servertcpport <servertcpport_port>
   ```

En plus de définir ces valeurs, vous devez également configurer le routage et mettre à jour le pare-feu pour le port 135. Pour plus d'informations sur ces procédures, voir [Comment configurer MSDTC sur Linux](sql-server-linux-configure-msdtc.md).

Il existe plusieurs autres paramètres pour mssql-conf que vous pouvez utiliser pour surveiller et dépanner MSDTC. Le tableau suivant décrit brièvement ces paramètres. Pour plus d'informations sur leur utilisation, consultez l'article de support Windows, [How to enable diagnostic tracing for MS DTC (Comment activer le suivi de diagnostic pour MS DTC)](https://support.microsoft.com/help/926099/how-to-enable-diagnostic-tracing-for-ms-dtc-on-a-windows-based-compute).

| paramaètre mssql-conf | Description |
|---|---|
| distributedtransaction.allowonlysecurerpccalls | Configurer les appels sécurisés RPC pour les transactions distribuées |
| distributedtransaction.fallbacktounsecurerpcifnecessary | Configurer les appels de sécurité RPC pour les transactions distribuées |
| distributedtransaction.maxlogsize | Taille du fichier journal des transactions DTC en Mo. La valeur par défaut est 64 Mo |
| distributedtransaction.memorybuffersize | Taille de la mémoire tampon circulaire dans laquelle les traces sont stockées. Cette taille est en Mo et sa valeur par défaut est de 10 Mo |
| distributedtransaction.servertcpport | Port du serveur rpc MSDTC |
| distributedtransaction.trace_cm | Traces dans le gestionnaire de connexions |
| distributedtransaction.trace_contact | Effectue le suivi du pool de contacts et des contacts |
| distributedtransaction.trace_gateway | Effectue le suivi de la source de la passerelle |
| distributedtransaction.trace_log | Suivi du journal |
| distributedtransaction.trace_misc | Traces qui ne peuvent pas être classées dans les autres catégories |
| distributedtransaction.trace_proxy | Traces générées dans le proxy MSDTC |
| distributedtransaction.trace_svc | Effectue le suivi du service et du démarrage du fichier .exe |
| distributedtransaction.trace_trace | L’infrastructure de suivi elle-même |
| distributedtransaction.trace_util | Effectue le suivi des routines de l’utilitaire appelées à partir de plusieurs emplacements |
| distributedtransaction.trace_xa | Source de suivi du gestionnaire de transactions XA (XATM) |
| distributedtransaction.tracefilepath | Dossier dans lequel les fichiers de trace doivent être stockés |
| distributedtransaction.turnoffrpcsecurity | Activer ou désactiver la sécurité RPC pour les transactions distribuées |

::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

## <a name="accept-mlservices-eulas"></a><a id="mlservices-eula"></a> Accepter les CLUF MLServices

L'ajout de [packages Machine Learning R ou Python](sql-server-linux-setup-machine-learning.md) au moteur de base de données nécessite l’acceptation des termes de licence pour les distributions open source de R et Python. Le tableau suivant énumère toutes les commandes ou options disponibles relatives aux CLUF mlservices. Le même paramètre CLUF est utilisé pour R et Python, selon ce que vous avez installé.

```bash
# For all packages: database engine and mlservices
# Setup prompts for mlservices EULAs, which you need to accept
sudo /opt/mssql/bin/mssql-conf setup

# Add R or Python to an existing installation
sudo /opt/mssql/bin/mssql-conf setup accept-eula-ml

# Alternative valid syntax
# Adds the EULA section to the INI and sets acceptulam to yes
sudo /opt/mssql/bin/mssql-conf set EULA accepteulaml Y

# Rescind EULA acceptance and removes the setting
sudo /opt/mssql/bin/mssql-conf unset EULA accepteulaml
```

Vous pouvez également ajouter l'acceptation du CLUF directement dans le fichier [mssql.conf](#mssql-conf-format) :

```ini
[EULA]
accepteula = Y
accepteulaml = Y
```
:::moniker-end
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

## <a name="enable-outbound-network-access"></a><a id="mlservices-outbound-access"></a> Activer l'accès réseau sortant

L'accès réseau sortant pour les extensions R, Python et Java dans la fonctionnalité [SQL Server Machine Learning Services](sql-server-linux-setup-machine-learning.md) est désactivé par défaut. Pour activer les requêtes sortantes, définissez la propriété booléenne « outboundnetworkaccess » à l’aide de mssql-conf.

Après avoir défini la propriété, redémarrez le service SQL Server Launchpad pour lire les valeurs mises à jour dans le fichier INI. Un message de redémarrage vous rappelle chaque fois qu’un paramètre relatif à l’extensibilité est modifié.

```bash
# Adds the extensibility section and property.
# Sets "outboundnetworkaccess" to true.
# This setting is required if you want to access data or operations off the server.
sudo /opt/mssql/bin/mssql-conf set extensibility outboundnetworkaccess 1

# Turns off network access but preserves the setting
sudo /opt/mssql/bin/mssql-conf set extensibility outboundnetworkaccess 0

# Removes the setting and rescinds network access
sudo /opt/mssql/bin/mssql-conf unset extensibility.outboundnetworkaccess
```

Vous pouvez également ajouter « outboundnetnetaccess » directement au fichier [mssql.conf](#mssql-conf-format) :

```ini
[extensibility]
outboundnetworkaccess = 1
```
:::moniker-end

## <a name="change-the-tcp-port"></a><a id="tcpport"></a> Modifier le port TCP

Le paramètre **network.tcpport** modifie le port TCP sur lequel SQL Server écoute les connexions. Par défaut, ce port est défini sur 1433. Pour modifier le port, exécutez les commandes suivantes :

1. Exécutez le script mssql-conf en tant que root avec la commande « set » pour « network.tcpport » :

   ```bash
   sudo /opt/mssql/bin/mssql-conf set network.tcpport <new_tcp_port>
   ```

2. Redémarrez le service SQL Server :

   ```bash
   sudo systemctl restart mssql-server
   ```

3. Lorsque vous vous connectez maintenant à SQL Server, vous devez spécifier le port personnalisé en ajoutant une virgule (,) après le nom d'hôte ou l'adresse IP. Par exemple, pour vous connecter avec SQLCMD, vous devez utiliser la commande suivante :

   ```bash
   sqlcmd -S localhost,<new_tcp_port> -U test -P test
   ```

## <a name="specify-tls-settings"></a><a id="tls"></a> Spécifier les paramètres TLS

Les options suivantes configurent TLS pour une instance SQL Server exécutée sur Linux.

|Option |Description |
|--- |--- |
|**network.forceencryption** |Si 1, alors [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] force le chiffrement de toutes les connexions. Par défaut, cette option est 0. |
|**network.tlscert** |Le chemin absolu vers le fichier de certificat que [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] utilise pour TLS. Exemple :   `/etc/ssl/certs/mssql.pem`  Le fichier de certificat doit être accessible par le compte mssql. Microsoft recommande de restreindre l'accès au fichier en utilisant `chown mssql:mssql <file>; chmod 400 <file>`. |
|**network.tlskey** |Le chemin absolu vers le fichier de clé privée que [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] utilise pour TLS. Exemple :  `/etc/ssl/private/mssql.key`  Le fichier de certificat doit être accessible par le compte mssql. Microsoft recommande de restreindre l'accès au fichier en utilisant `chown mssql:mssql <file>; chmod 400 <file>`. |
|**network.tlsprotocols** |Une liste séparée par des virgules dont les protocoles TLS sont autorisés par SQL Server. [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] tente toujours de négocier le protocole le plus strict possible. Si un client n’accepte aucun protocole autorisé , [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] rejette la tentative de connexion.  Pour des raisons de compatibilité, tous les protocoles pris en charge sont autorisés par défaut (1.2, 1.1, 1.0).  Si vos clients prennent en charge TLS 1.2, Microsoft recommande d'autoriser uniquement TLS 1.2. |
|**network.tlsciphers** |Spécifie quels chiffrements sont autorisés par [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] pour TLS. Cette chaîne doit être mise en forme selon [le format de liste de chiffrement OpenSSL](https://www.openssl.org/docs/man1.0.2/apps/ciphers.html). En général, vous n’aurez pas besoin de modifier cette option. <br /> Par défaut, les chiffrements suivants sont autorisés : <br /> `ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES128-SHA256:ECDHE-ECDSA-AES256-SHA384:ECDHE-RSA-AES128-SHA256:ECDHE-RSA-AES256-SHA384:ECDHE-ECDSA-AES256-SHA:ECDHE-ECDSA-AES128-SHA:ECDHE-RSA-AES256-SHA:ECDHE-RSA-AES128-SHA:AES256-GCM-SHA384:AES128-GCM-SHA256:AES256-SHA256:AES128-SHA256:AES256-SHA:AES128-SHA` |
| **network.kerberoskeytabfile** |Chemin d'accès au fichier keytab Kerberos |

Pour un exemple d'utilisation des paramètres TLS, voir [Chiffrement des connexions à SQL Server sur Linux](sql-server-linux-encrypted-connections.md).

## <a name="network-settings"></a><a id="network"></a> Paramètres réseau

Voir le [tutoriel : Utilisez l’authentification Active Directory avec SQL Server sur Linux](sql-server-linux-active-directory-authentication.md) pour obtenir des informations complètes sur l’utilisation de l’authentification AD avec SQL Server sur Linux.

Les options suivantes sont des paramètres réseau supplémentaires configurables à l’aide de `mssql-conf`.

|Option |Description |
|--- |--- |
| network.disablesssd | Désactiver l'interrogation de SSSD pour obtenir les informations du compte AD et utiliser par défaut des appels LDAP. Les valeurs peuvent être `true` ou `false`. |
| network.enablekdcfromkrb5conf | Activer la recherche d'informations KDC à partir de krb5.conf. Les valeurs peuvent être `true` ou `false`. |
| network.forcesecureldap | Forcer l'utilisation de LDAPS pour contacter le contrôleur de domaine. Les valeurs peuvent être `true` ou `false`. |
| network.ipaddress | Adresse IP pour les connexions entrantes. |
| network.kerberoscredupdatefrequency | Temps en secondes entre les vérifications des informations d'identification Kerberos qui doivent être mises à jour. La valeur est un entier.|
| network.privilegedadaccount | Utilisateur AD privilégié à utiliser pour l'authentification AD. La valeur est `<username>`. Pour plus d’informations, consultez [Didacticiel : Utiliser l’authentification Active Directory avec SQL Server sur Linux](sql-server-linux-active-directory-authentication.md#spn)|
| uncmapping | Mappe le chemin UNC à un chemin local. Par exemple : `sudo /opt/mssql/bin/mssql-conf set uncmapping //servername/sharename /tmp/folder`. |

## <a name="enabledisable-traceflags"></a><a id="traceflags"></a> Activer/désactiver les indicateurs de trace

L’option **traceflag** active ou désactive les indicateurs de trace pour le démarrage du service SQL Server. Pour activer/désactiver un indicateur de trace, utilisez les commandes suivantes :

1. Activez un indicateur de trace à l'aide de la commande suivante. Par exemple, pour l’indicateur de trace 1234 :

   ```bash
   sudo /opt/mssql/bin/mssql-conf traceflag 1234 on
   ```

1. Vous pouvez activer plusieurs indicateurs de trace en les spécifiant séparément :

   ```bash
   sudo /opt/mssql/bin/mssql-conf traceflag 2345 3456 on
   ```

1. De la même manière, vous pouvez désactiver un ou plusieurs indicateurs de trace activés en les spécifiant et en ajoutant le paramètre **off** :

   ```bash
   sudo /opt/mssql/bin/mssql-conf traceflag 1234 2345 3456 off
   ```

1. Redémarrez le service SQL Server pour appliquer les modifications :

   ```bash
   sudo systemctl restart mssql-server
   ```

## <a name="remove-a-setting"></a>Supprimer un paramètre

Pour annuler tout réglage effectué avec `mssql-conf set`, appelez **mssql-conf** avec l'option `unset` et le nom du réglage. Le réglage est effacé et la valeur par défaut est rétablie.

1. L'exemple suivant efface l'option **network.tcpport**.

   ```bash
   sudo /opt/mssql/bin/mssql-conf unset network.tcpport
   ```

1. Redémarrez le service SQL Server.

   ```bash
   sudo systemctl restart mssql-server
   ```

## <a name="view-current-settings"></a>Afficher les paramètres actuels

Pour afficher les paramètres configurés, exécutez la commande suivante afin d’afficher le contenu du fichier **mssql.conf** :

```bash
sudo cat /var/opt/mssql/mssql.conf
```

Tous les paramètres non affichés dans ce fichier utilisent leurs valeurs par défaut. La section suivante fournit un exemple de fichier **mssql.conf**.


## <a name="mssqlconf-format"></a><a id="mssql-conf-format"></a> Format mssql.conf

Le fichier **/var/opt/mssql/mssql.conf** suivant fournit un exemple pour chaque paramètre. Vous pouvez utiliser ce format pour modifier manuellement le fichier **mssql.conf** si nécessaire. Si vous modifiez manuellement le fichier, vous devez redémarrer SQL Server pour appliquer les modifications. Pour utiliser le fichier **mssql.conf** avec Docker, Docker doit [conserver vos données](./sql-server-linux-docker-container-deployment.md). Ajoutez d'abord un fichier **mssql.conf** complet à votre répertoire hôte, puis exécutez le conteneur. Vous trouverez un exemple dans la section [Feedback des clients](./usage-and-diagnostic-data-configuration-for-sql-server-linux.md).

<!--SQL Server 2017 on Linux-->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

```ini
[EULA]
accepteula = Y

[coredump]
captureminiandfull = true
coredumptype = full

[filelocation]
defaultbackupdir = /var/opt/mssql/data/
defaultdatadir = /var/opt/mssql/data/
defaultdumpdir = /var/opt/mssql/data/
defaultlogdir = /var/opt/mssql/data/

[hadr]
hadrenabled = 0

[language]
lcid = 1033

[memory]
memorylimitmb = 4096

[network]
forceencryption = 0
ipaddress = 10.192.0.0
kerberoskeytabfile = /var/opt/mssql/secrets/mssql.keytab
tcpport = 1401
tlscert = /etc/ssl/certs/mssql.pem
tlsciphers = ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES128-SHA256:ECDHE-ECDSA-AES256-SHA384:ECDHE-RSA-AES128-SHA256:ECDHE-RSA-AES256-SHA384:ECDHE-ECDSA-AES256-SHA:ECDHE-ECDSA-AES128-SHA:ECDHE-RSA-AES256-SHA:ECDHE-RSA-AES128-SHA:AES256-GCM-SHA384:AES128-GCM-SHA256:AES256-SHA256:AES128-SHA256:AES256-SHA:AES128-SHA
tlskey = /etc/ssl/private/mssql.key
tlsprotocols = 1.2,1.1,1.0

[sqlagent]
databasemailprofile = default
errorlogfile = /var/opt/mssql/log/sqlagentlog.log
errorlogginglevel = 7

[telemetry]
customerfeedback = true
userrequestedlocalauditdirectory = /tmp/audit

[traceflag]
traceflag0 = 1204
traceflag1 = 2345
traceflag = 3456
```

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

```ini
[EULA]
accepteula = Y
accepteulaml = Y

[coredump]
captureminiandfull = true
coredumptype = full

[distributedtransaction]
servertcpport = 51999

[filelocation]
defaultbackupdir = /var/opt/mssql/data/
defaultdatadir = /var/opt/mssql/data/
defaultdumpdir = /var/opt/mssql/data/
defaultlogdir = /var/opt/mssql/data/

[hadr]
hadrenabled = 0

[language]
lcid = 1033

[memory]
memorylimitmb = 4096

[network]
forceencryption = 0
ipaddress = 10.192.0.0
kerberoskeytabfile = /var/opt/mssql/secrets/mssql.keytab
rpcport = 13500
tcpport = 1401
tlscert = /etc/ssl/certs/mssql.pem
tlsciphers = ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES128-SHA256:ECDHE-ECDSA-AES256-SHA384:ECDHE-RSA-AES128-SHA256:ECDHE-RSA-AES256-SHA384:ECDHE-ECDSA-AES256-SHA:ECDHE-ECDSA-AES128-SHA:ECDHE-RSA-AES256-SHA:ECDHE-RSA-AES128-SHA:AES256-GCM-SHA384:AES128-GCM-SHA256:AES256-SHA256:AES128-SHA256:AES256-SHA:AES128-SHA
tlskey = /etc/ssl/private/mssql.key
tlsprotocols = 1.2,1.1,1.0

[sqlagent]
databasemailprofile = default
errorlogfile = /var/opt/mssql/log/sqlagentlog.log
errorlogginglevel = 7

[telemetry]
customerfeedback = true
userrequestedlocalauditdirectory = /tmp/audit

[traceflag]
traceflag0 = 1204
traceflag1 = 2345
traceflag = 3456
```

::: moniker-end

## <a name="next-steps"></a>Étapes suivantes

Pour utiliser plutôt des variables d'environnement afin d’effectuer certains de ces changements de configuration, voir [Configurer des paramètres SQL Server à l’aide de variables d’environnement](sql-server-linux-configure-environment-variables.md).

Pour d'autres outils et scénarios de gestion, voir [Gérer SQL Server sur Linux](sql-server-linux-management-overview.md).