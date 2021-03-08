---
title: Meilleures pratiques en matière de performances pour SQL Server sur Linux
description: Cet article présente les bonnes pratiques en matière de performances et les instructions d’exécution de SQL Server sur Linux.
author: tejasaks
ms.author: tejasaks
ms.reviewer: vanto
ms.date: 01/19/2021
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: 42638520dd0d7391a10217dc7f7fdd2ce708189f
ms.sourcegitcommit: 0bcda4ce24de716f158a3b652c9c84c8f801677a
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2021
ms.locfileid: "102247498"
---
# <a name="performance-best-practices-and-configuration-guidelines-for-sql-server-on-linux"></a>Meilleures pratiques en matière de performances et lignes directrices de configuration pour SQL Server sur Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Cet article présente les meilleures pratiques et suggestions pour optimiser les performances des applications de base de données qui se connectent à SQL Server sur Linux. Ces suggestions sont spécifiques à l’exécution sur la plateforme Linux. Toutes les suggestions de SQL Server normales, telles que la conception d’index, s’appliquent toujours.

Les directives suivantes contiennent des suggestions de configuration de SQL Server et du système d’exploitation Linux.

## <a name="linux-os-configuration"></a>Configuration du système d'exploitation Linux

Envisagez d’utiliser les paramètres de configuration suivants du système d’exploitation Linux pour bénéficier des meilleures performances pour une installation SQL Server.

### <a name="storage-configuration-recommendation"></a>Configuration de stockage recommandée

#### <a name="use-storage-subsystem-with-appropriate-iops-throughput-and-redundancy"></a>Utiliser le sous-système de stockage avec les E/S par seconde, le débit et la redondance appropriés

Le sous-système de stockage qui héberge des données, des journaux de transactions et d’autres fichiers associés (tels que les fichiers de point de contrôle pour OLTP en mémoire) doit être capable de gérer de manière appropriée des charges de travail moyennes et maximales. Normalement, dans des environnements locaux, le fournisseur de stockage prend en charge la configuration RAID matérielle appropriée avec agrégation par bandes sur plusieurs disques pour garantir des E/S par seconde, un débit et une redondance appropriés. Toutefois, cela peut varier selon les fournisseurs de stockage et les offres de stockage aux architectures variées.

Pour un déploiement de SQL Server sur Linux sur des machines virtuelles Azure, envisagez d’utiliser un RAID logiciel pour répondre aux besoins d’E/S par seconde et de débit. Reportez-vous à l’article suivant lors de la configuration de SQL Server sur des machines virtuelles Azure pour des considérations de stockage similaires : [Configuration du stockage pour les machines virtuelles SQL Server](/azure/azure-sql/virtual-machines/windows/storage-configuration)

Un exemple de création d’un RAID logiciel dans Linux sur des machines virtuelles Azure est présenté ci-dessous. Toutefois, vous devez utiliser le nombre approprié de disques de données pour le débit et les E/S par seconde requis pour les volumes, selon les exigences relatives aux données, aux journaux de transactions et aux E/S de tempdb. Dans cet exemple, huit disques de données ont été attachés à la machine virtuelle Azure : 4 pour héberger les fichiers de données, 2 pour les journaux des transactions et 2 pour la charge de travail tempdb.

```bash
# To locate the devices (for example /dev/sdc) for RAID creation, use the lsblk command
# For Data volume, using 4 devices, in RAID 5 configuration with 8KB stripes
mdadm --create --verbose /dev/md0 --level=raid5 --chunk=8K --raid-devices=4 /dev/sdc /dev/sdd /dev/sde /dev/sdf

# For Log volume, using 2 devices in RAID 10 configuration with 64KB stripes
mdadm --create --verbose /dev/md1 --level=raid10 --chunk=64K --raid-devices=2 /dev/sdg /dev/sdh

# For tempdb volume, using 2 devices in RAID 0 configuration with 64KB stripes
mdadm --create --verbose /dev/md2 --level=raid0 --chunk=64K --raid-devices=2 /dev/sdi /dev/sdj
```

#### <a name="disk-partitioning-and-configuration-recommendations"></a>Recommandations relatives à la configuration et au partitionnement de disque

Pour SQL Server, il est recommandé d’utiliser des configurations RAID. L’unité de bande (sunit) et la largeur de bande du système de fichiers déployé doivent correspondre à la géométrie RAID. Voici un exemple basé sur le système de fichiers XFS pour un volume de journal.

```bash
# Creating a log volume, using 6 devices, in RAID 10 configuration with 64KB stripes
mdadm --create --verbose /dev/md3 --level=raid10 --chunk=64K --raid-devices=6 /dev/sda /dev/sdb /dev/sdc /dev/sdd /dev/sde /dev/sdf

mkfs.xfs /dev/sda1 -f -L log 
meta-data=/dev/sda1              isize=512    agcount=32, agsize=18287648 blks 
         =                       sectsz=4096  attr=2, projid32bit=1 
         =                       crc=1        finobt=1, sparse=1, rmapbt=0 
         =                       reflink=1 
data     =                       bsize=4096   blocks=585204384, imaxpct=5 
         =                       sunit=16     swidth=48 blks 
naming   =version 2              bsize=4096   ascii-ci=0, ftype=1 
log      =internal log           bsize=4096   blocks=285744, version=2 
         =                       sectsz=4096  sunit=1 blks, lazy-count=1 
realtime =none                   extsz=4096   blocks=0, rtextents=0 
```

Le tableau de journaux est une configuration RAID-10 à 6 lecteurs avec une bande de 64 Ko. Comme vous pouvez le voir :

- « sunit=16 blks », 16*4096 (taille de bloc) = 64 Ko, ce qui correspond à la taille de bande.
- « swidth = 48 blks », swidth/sunit = 3, ce qui correspond au nombre de lecteurs de données dans le tableau, à l’exception des lecteurs de parité.

#### <a name="file-system-configuration-recommendation"></a>Configuration recommandée du système de fichiers

SQL Server prend en charge les systèmes de fichiers EXT4 et XFS pour héberger la base de données, les journaux des transactions et des fichiers supplémentaires, tels que les fichiers de point de contrôle pour OLTP en mémoire dans SQL Server. Microsoft recommande d’utiliser le système de fichiers XFS pour héberger les fichiers de données et les fichiers journaux de transactions SQL Server.

```bash
# Formatting the volume with XFS filesystem
mkfs.xfs /dev/md0 -f -L datavolume
mkfs.xfs /dev/md1 -f -L logvolume
mkfs.xfs /dev/md2 -f -L tempdb
```

> [!NOTE]
> Il est possible de configurer le système de fichiers XFS pour qu’il ne prenne pas en compte la casse lors de la création et de la mise en forme du volume XFS. Ce n’est pas une configuration fréquemment utilisée dans l’écosystème Linux, mais elle peut être utilisée pour des raisons de compatibilité.
>
> Exemple : mkfs.xfs /dev/md0 -f -n version=ci -L datavolume
>
> Dans cet exemple, les paramètres `-n version=ci` configurent le système de fichiers XFS pour qu’il ne prenne pas en compte la casse.

##### <a name="persistent-memory-filesystem-recommendation"></a>Système de fichiers de mémoire persistante recommandé

Pour la configuration du système de fichiers sur les appareils à mémoire persistante, l’allocation de blocs pour le système de fichiers sous-jacent doit être de 2 Mo. Pour plus d’informations à ce sujet, consultez l’article [Considérations techniques](sql-server-linux-configure-pmem.md#technical-considerations).

#### <a name="disable-last-accessed-datetime-on-file-systems-for-sql-server-data-and-log-files"></a>Désactiver la date/heure du dernier accès aux systèmes de fichiers pour les données et les fichiers journaux SQL Server

Pour vous assurer que le ou les lecteurs attachés au système sont remontés automatiquement après un redémarrage, ils doivent être ajoutés au fichier `/etc/fstab`. Il est également vivement recommandé d’utiliser l’UUID (identificateur unique universel) dans `/etc/fstab` pour faire référence au lecteur plutôt que d’utiliser uniquement le nom de l’appareil (tel que `/dev/sdc1`).

Il est fortement recommandé d’utiliser l’attribut **noatime** avec tout système de fichiers servant à stocker des données et des fichiers journaux SQL Server. Reportez-vous à la documentation Linux pour savoir comment définir cet attribut. Vous trouverez ci-dessous un exemple illustrant comment activer l’option **noatime** pour un volume monté dans une machine virtuelle Azure.

L’entrée du point de montage dans ***/etc/fstab***

```bash
UUID="xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx" /data1 xfs,rw,attr2,noatime 0 0
```

Dans l’exemple ci-dessus, l’UUID représente l’appareil que vous pouvez trouver à l’aide de la commande ***blkid***.

#### <a name="sql-server-and-forced-unit-access-fua-io-subsystem-capability"></a>Fonctionnalité du sous-système d’E/S SQL Server et FUA (Forced Unit Access)

Certaines versions des distributions Linux prises en charge fournissent une prise en charge permettant à la fonctionnalité du sous-système d’E/S FUA d’assurer la durabilité des données. SQL Server utilise la fonctionnalité FUA pour fournir des E/S hautement efficaces et fiables pour la charge de travail SQL Server. Pour plus d’informations sur la prise en charge de FUA par la distribution Linux et son impact pour SQL Server, consultez le blog suivant : [SQL Server On Linux: Forced Unit Access (FUA) Internals](https://bobsql.com/sql-server-on-linux-forced-unit-access-fua-internals/)

SUSE Linux Enterprise Server 12 SP5 et Red Hat Enterprise Linux 8.0 (et versions ultérieures) prennent en charge la fonctionnalité FUA dans le sous-système d’E/S. Si vous utilisez SQL Server 2017 CU6 et versions ultérieures ou SQL Server 2019, vous devez utiliser la configuration suivante pour assurer l’implémentation d’E/S efficace et performante avec FUA par SQL Server.

Utilisez la configuration recommandée listée ci-dessous si les conditions suivantes sont remplies.

- Utilisation de SQL Server 2017 CU6 ou version ultérieure, ou de SQL Server 2019
- Utilisation d’une distribution Linux et d’une version prenant en charge la fonctionnalité FUA (Red Hat Enterprise Linux 8.0 ou version ultérieure, ou SUSE Linux Enterprise Server 12 SP5)
- Sous-système de stockage et/ou matériel prenant en charge la fonctionnalité FUA et configuré pour celle-ci

Configuration recommandée :

1. Activez l’indicateur de trace 3979 en tant que paramètre de démarrage.
2. Utilisez **mssql-conf** pour configurer `control.writethrough = 1` et `control.alternatewritethrough = 0`.

Pour presque toutes les autres configurations qui ne respectent pas les conditions précédentes, la configuration recommandée est la suivante :

1. Activez l’indicateur de trace 3982 en tant que paramètre de démarrage (par défaut pour SQL Server dans l’écosystème Linux), tout en veillant à ce que l’indicateur de trace 3979 ne soit pas activé en tant que paramètre de démarrage.
2. Utilisez **mssql-conf** pour configurer `control.writethrough = 1` et `control.alternatewritethrough = 1`.

### <a name="kernel-and-cpu-settings-for-high-performance"></a>Paramètres de noyau et de processeur pour une haute performance

La section suivante décrit les paramètres de système d’exploitation Linux recommandés en rapport avec la haute performance et le débit d’une installation SQL Server. Reportez-vous à la documentation sur le système d’exploitation Linux pour consulter le processus de configuration de ces paramètres. L’utilisation de [***Tuned***](https://tuned-project.org) telle qu’elle est décrite permet de configurer de nombreux processeurs et d’effectuer les configurations de noyau décrites ci-dessous.

#### <a name="using-tuned-to-configure-kernel-settings"></a>Utilisation de ***Tuned*** pour configurer les paramètres de noyau

Pour les utilisateurs de Red Hat Enterprise Linux (RHEL), le profil débit-performance [Tuned](https://tuned-project.org) configure automatiquement certains paramètres de noyau et de processeur (à l’exception des états C). À compter de RHEL 8.0, un profil ***Tuned** _ nommé _ *mssql** a été codéveloppé avec Red Hat et offre des optimisations des performances Linux plus précises pour les charges de travail SQL Server. Ce profil inclut le profil débit-performance RHEL. Vous trouverez ses définitions ci-après ainsi que d’autres distributions Linux et versions de RHEL sans ce profil.

Pour SUSE Linux Enterprise Server 12 SP5, Ubuntu 18.04 et Red Hat Enterprise Linux 7.x, le package ***Tuned** _ peut être installé manuellement. Il peut être utilisé pour créer et configurer le profil _ *mssql** comme décrit ci-dessous.

##### <a name="proposed-linux-settings-using-a-tuned-mssql-profile"></a>Paramètres Linux proposés avec un profil mssql Tuned

```bash
#
# A Tuned configuration for SQL Server on Linux
#

[main]
summary=Optimize for Microsoft SQL Server
include=throughput-performance

[cpu]
force_latency=5

[sysctl]
vm.swappiness = 1
vm.dirty_background_ratio = 3
vm.dirty_ratio = 80
vm.dirty_expire_centisecs = 500
vm.dirty_writeback_centisecs = 100
vm.transparent_hugepages=always
# For multi-instance SQL deployments, use
# vm.transparent_hugepages=madvise
vm.max_map_count=1600000
net.core.rmem_default = 262144
net.core.rmem_max = 4194304
net.core.wmem_default = 262144
net.core.wmem_max = 1048576
kernel.numa_balancing=0
kernel.sched_latency_ns = 60000000
kernel.sched_migration_cost_ns = 500000
kernel.sched_min_granularity_ns = 15000000
kernel.sched_wakeup_granularity_ns = 2000000
```

Pour activer ce profil Tuned, enregistrez ces définitions dans un fichier **tuned.conf**, dans un dossier `/usr/lib/tuned/mssql`, et activez le profil en utilisant les commandes suivantes :

```bash
chmod +x /usr/lib/tuned/mssql/tuned.conf
tuned-adm profile mssql
```

Vérifiez qu’il est activé à l’aide de la commande suivante :

```bash
tuned-adm active
```

ou

```bash
tuned-adm list
```

#### <a name="cpu-settings-recommendation"></a>Recommandation relative aux paramètres de processeur

La table suivante fournit des suggestions pour les paramètres de l’UC :

| Paramètre | Valeur | Informations complémentaires |
|---|---|---|
| Gouverneur de fréquence de l’UC | performances | Consultez la commande **cpupower** |
| ENERGY_PERF_BIAS | performances | Consultez la commande **x86_energy_perf_policy** |
| min_perf_pct | 100 | Consultez votre documentation sur l’état P Intel |
| États C | C1 uniquement | Consultez la documentation Linux ou de votre système pour savoir comment garantir que les états C sont définis sur C1 uniquement |

L’utilisation de ***Tuned** _ comme décrit précédemment configure automatiquement les paramètres du gouverneur de fréquence de processeur, ENERGY_PERF_BIAS et min_perf_pct, de façon appropriée, car le profil débit-performance est utilisé comme base pour le profil _ *mssql**. Le paramètre des états C doit être configuré manuellement conformément à la documentation fournie par Linux ou le distributeur du système.

#### <a name="disk-settings-recommendations"></a>Recommandations relatives aux paramètres de disque

La table suivante fournit des suggestions pour les paramètres du disque :

| Paramètre | Valeur | Informations complémentaires |
|---|---|---|
| disk `readahead` | 4096 | Consultez la commande `blockdev` |
| paramètres sysctl | kernel.sched_min_granularity_ns = 15000000<br/>kernel.sched_wakeup_granularity_ns = 2000000<br/>vm.dirty_ratio = 80<br/>vm.dirty_background_ratio = 3<br/>vm.swappiness = 1 | Consultez la commande **sysctl** |

**Description :**

- **vm.swappiness** : Ce paramètre contrôle le poids relatif donné à l’échange de la mémoire du processus de runtime par rapport au cache du système de fichiers. La valeur par défaut de ce paramètre est 60, ce qui signifie que le ratio de l’échange des pages de mémoire du processus de runtime par rapport à la suppression des pages de cache du système de fichiers est de 60:140. La définition de la valeur 1 indique une préférence forte pour la conservation de la mémoire du processus de runtime dans la mémoire physique au détriment du cache du système de fichiers. Étant donné que SQL Server utilise un pool de mémoires tampons comme cache de pages de données et qu’il préfère fortement écrire sur du matériel physique, contournant ainsi le cache du système de fichiers pour garantir une récupération fiable, une configuration d’échange agressive peut être bénéfique pour un serveur SQL Server dédié et hautement performant.
Vous trouverez des informations supplémentaires dans [Documentation pour /proc/sys/vm/ - #swappiness](https://www.kernel.org/doc/html/latest/admin-guide/sysctl/vm.html#swappiness)

- **vm.dirty_\***  : Les accès en écriture aux fichiers SQL Server ne sont pas mis en cache pour répondre aux exigences d’intégrité des données. Ces paramètres permettent de bonnes performances d’écriture asynchrone et réduisent l’impact des E/S de stockage des écritures de mise en cache Linux en permettant une mise en cache suffisamment importante pendant la limitation du vidage.

- **kernel.sched_\***  : Ces valeurs de paramètre représentent la recommandation actuelle pour la modification de l’algorithme CFS (Completely Fair Scheduling) dans le noyau Linux, afin d’améliorer le débit des appels d’E/S de réseau et de stockage en ce qui concerne la préemption entre processus et la reprise des threads.

L’utilisation du profil **mssql** **_Tuned_ *_ configure les paramètres _* vm.swappiness**, **vm.dirty_\* *_ et _* kernel.sched_\*** . La configuration de disk `readahead` à l’aide de la commande `blockdev` s’effectue appareil par appareil et doit être effectuée manuellement.

#### <a name="kernel-setting-auto-numa-balancing-for-multi-node-numa-systems"></a>Paramètre de noyau d’équilibrage NUMA automatique pour les systèmes NUMA à plusieurs nœuds

Si vous installez SQL Server sur un système **NUMA** à plusieurs nœuds, le paramètre de noyau **kernel.numa_balancing** suivant est activé par défaut. Pour permettre à SQL Server de fonctionner avec un niveau d'efficacité maximal sur un système **NUMA**, désactivez l’équilibrage NUMA automatique sur un système NUMA à plusieurs nœuds :

```bash
sysctl -w kernel.numa_balancing=0
```

L’utilisation du profil **mssql** **_Tuned_ *_ configure l’option _* kernel.numa_balancing**.

#### <a name="kernel-settings-for-virtual-address-space"></a>Paramètres de noyau pour l’espace d’adressage virtuel

La valeur par défaut de **vm. max _map_count** (65536) peut ne pas être suffisamment élevée pour une installation SQL Server. Par conséquent, remplacez la valeur de **vm.max_map_count** par au moins 262144 pour un déploiement SQL Server et reportez-vous à la section [Paramètres Linux proposés avec un profil mssql Tuned](#proposed-linux-settings-using-a-tuned-mssql-profile) pour affiner encore ces paramètres de noyau. La valeur maximale pour vm.max_map_count est 2147483647.

```bash
sysctl -w vm.max_map_count=1600000
```

L’utilisation du profil **mssql** **_Tuned_ *_ configure l’option _* vm.max_map_count**.

#### <a name="leave-transparent-huge-pages-thp-enabled"></a>Laissez les pages volumineuses transparentes (THP) activées

Cette option doit être activée par défaut pour la plupart des installations Linux. Nous vous recommandons d’utiliser les performances les plus cohérentes pour garder cette option de configuration activée. Toutefois, en cas d’activité de pagination sollicitant une grande quantité de mémoire (par exemple, dans le cadre de déploiements SQL Server avec plusieurs instances ou d’une exécution de SQL Server avec d’autres applications nécessitant une grande quantité de mémoire sur le serveur), nous vous suggérons de tester les performances de vos applications après l’exécution de la commande suivante :

```bash
echo madvise > /sys/kernel/mm/transparent_hugepage/enabled
```

Vous pouvez aussi modifier le profil **mssql** **_Tuned_** avec la ligne :

```bash
vm.transparent_hugepages=madvise
```

Et rendre le profil **mssql** actif après la modification :

```bash
tuned-adm off
tuned-adm profile mssql
```

L’utilisation du profil **mssql** **_Tuned_ *_ configure l’option _* transparent_hugepage**.

#### <a name="network-setting-recommendations"></a>Recommandations relatives aux paramètres réseau

À l’instar des recommandations en matière de stockage et de processeur, il existe des recommandations spécifiques liées au réseau. Celles-ci sont listées ci-dessous pour référence. Les paramètres mentionnés ci-dessous ne sont pas disponibles sur toutes les cartes réseau. Pour obtenir des conseils sur chacune de ces options, contactez les fournisseurs de cartes réseau. Testez et configurez ces paramètres sur des environnements de développement avant de les appliquer à des environnements de production. Les options mentionnées ci-dessous sont expliquées avec des exemples, et les commandes utilisées sont propres à un type et à un fournisseur de carte réseau.

1. Configuration de la taille de la mémoire tampon du port réseau : Dans l’exemple ci-dessous, la carte d’interface réseau est nommée « eth0 » (carte réseau Intel). Pour les cartes réseau Intel, la taille de la mémoire tampon recommandée est de 4 Ko (4096). Vérifiez les valeurs maximales prédéfinies, puis effectuez la configuration à l’aide des exemples de commandes ci-dessous :

   ```bash
            #To check the pre-set maximums please run the command, example NIC name used here is:"eth0"
            ethtool -g eth0
            #command to set both the rx(recieve) and tx (transmit) buffer size to 4 KB. 
            ethtool -G eth0 rx 4096 tx 4096
            #command to check the value is properly configured is:
            ethtool -g eth0
   ```

2. Activer les trames Jumbo : Avant d’activer les trames Jumbo, vérifiez que tous les commutateurs réseau, routeurs et autres éléments essentiels dans le chemin du paquet réseau entre les clients et SQL Server prennent en charge les trames Jumbo. Ce n’est que dans ce cas que l’activation des trames Jumbo peut améliorer les performances. Une fois les trames Jumbo activées, connectez-vous à SQL Server et remplacez la taille du paquet réseau par 8060 à l’aide de `sp_configure`, comme indiqué ci-dessous :

   ```bash
            #command to set jumbo frame to 9014 for a Intel NIC named eth0 is
            ifconfig eth0 mtu 9014
            #verify the setting using the command:
            ip addr | grep 9014
   ```

   ```sql
            sp_configure 'network packet size' , '8060'
            go
            reconfigure with override
            go
   ```

3. Par défaut, nous vous recommandons de définir le port pour la fusion des IRQ RX/TX adaptatives, ce qui signifie que la remise des interruptions sera ajustée pour améliorer la latence quand le taux de paquets est faible et améliorer le débit quand le taux de paquets est élevé. Notez que ce paramètre peut ne pas être disponible dans l’ensemble de l’infrastructure réseau. Vérifiez donc l’infrastructure réseau existante et confirmez qu’elle est prise en charge. L’exemple ci-dessous concerne la carte d’interface réseau nommée « eth0 » (carte réseau Intel) :

   ```bash
            #command to set the port for adaptive RX/TX IRQ coalescing
            echtool -C eth0 adaptive-rx on
            echtool -C eth0 adaptive-tx on
            #confirm the setting using the command:
            ethtool -c eth0
   ```

   > [!NOTE]
   > Pour obtenir un comportement prévisible dans les environnements hautes performances, comme ceux utilisés comme points de référence, désactivez la fusion des IRQ RX/TX adaptatives, puis définissez spécifiquement la fusion des interruptions RX/TX. Consultez les exemples de commandes pour désactiver la fusion des IRQ RX/TX, puis définissez spécifiquement les valeurs :

   ```bash
            #commands to disable adaptive RX/TX IRQ coalescing
            echtool -C eth0 adaptive-rx off
            echtool -C eth0 adaptive-tx off
            #confirm the setting using the command:
            ethtool -c eth0
            #Let us set the rx-usecs parameter which specify how many microseconds after at least 1 packet is received before generating an interrupt, and the [irq] parameters are the corresponding delays in updating the #status when the interrupt is disabled. For Intel bases NICs below are good values to start with:
            ethtool -C eth0 rx-usecs 100 tx-frames-irq 512
            #confirm the setting using the command:
            ethtool -c eth0
   ```

4. Nous vous recommandons également d’activer RSS (Receive-Side Scaling) et, par défaut, de combiner les côtés RX et TX des files d’attente RSS. Dans certains scénarios spécifiques impliquant le Support Microsoft, la désactivation de RSS a également permis d’améliorer les performances. Testez ce paramètre dans les environnements de test avant de l’appliquer à des environnements de production. L’exemple de commande illustré concerne les cartes réseau Intel.

   ```bash
            #command to get pre-set maximums
            ethtool -l eth0 
            #note the pre-set "Combined" maximum value. let's consider for this example, it is 8.
            #command to combine the queues with the value reported in the pre-set "Combined" maximum value:
            ethtool -L eth0 combined 8
            #you can verify the setting using the command below
            ethtool -l eth0
   ```

5. Utilisation de l’affinité d’IRQ des ports de la carte réseau. Pour obtenir les performances attendues en modifiant l’affinité d’IRQ, vous devez tenir compte de quelques paramètres importants tels que la gestion par Linux de la topologie du serveur, la pile de pilotes de la carte réseau, les paramètres par défaut et le paramètre irqbalance. Pour optimiser les paramètres d’affinité d’IRQ des ports de la carte réseau, vous devez connaître la topologie du serveur, désactiver irqbalance et utiliser des paramètres spécifiques au fournisseur de la carte réseau. Voici un exemple d’infrastructure réseau spécifique à Mellanox pour vous aider à comprendre la configuration. Notez que les commandes varient en fonction de l’environnement. Pour plus d’informations, contactez le fournisseur de la carte réseau :

   ```bash
            #disable irqbalance or get a snapshot of the IRQ settings and force the daemon to exit
            systemctl disable irqbalance.service
            #or
            irqbalance --oneshot

            #download the Mellanox mlnx_tuning_scripts tarball, https://www.mellanox.com/sites/default/files/downloads/tools/mlnx_tuning_scripts.tar.gz and extract it
            tar -xvf mlnx_tuning_scripts.tar.gz
            # be sure, common_irq_affinity.sh is executable. if not, 
            # chmod +x common_irq_affinity.sh       

            #display IRQ affinity for Mellanox NIC port; e.g eth0
            ./show_irq_affinity.sh eth0

            #optimize for best throughput performance
            ./mlnx_tune -p HIGH_THROUGHPUT

            #set hardware affinity to the NUMA node hosting physically the NIC and its port
            ./set_irq_affinity_bynode.sh `\cat /sys/class/net/eth0/device/numa_node` eth0

            #verify IRQ affinity
            ./show_irq_affinity.sh eth0

            #add IRQ coalescing optimizations
            ethtool -C eth0 adaptive-rx off
            ethtool -C eth0 adaptive-tx off
            ethtool -C eth0  rx-usecs 750 tx-frames-irq 2048

            #verify the settings
            ethtool -c eth0
   ```

6. Une fois les modifications ci-dessus effectuées, vérifiez que la vitesse de la carte réseau correspond aux attentes à l’aide de la commande suivante :

   ```bash
            ethtool eth0 | grep -i Speed
   ```

#### <a name="additional-advanced-kernelos-configuration"></a>Configuration avancée supplémentaire du noyau/système d’exploitation

- Pour des performances optimales d’E/S de stockage, il est recommandé d’utiliser la planification Linux à plusieurs files d’attente pour les appareils de traitement par blocs. Cela permet de mettre à l’échelle correctement les performances de la couche de traitement par blocs avec des disques SSD (Solid-State Drive) et des systèmes multicœurs rapides. Consultez la documentation si elle est activée par défaut dans vos distributions Linux. Dans la plupart des autres cas, l’amorçage du noyau avec **scsi_mod.use_blk_mq=y** l’active, même si la documentation de la distribution Linux utilisée peut fournir des instructions supplémentaires à ce sujet. Celle-ci est cohérente avec le noyau Linux en amont.

- Comme MPIO (Multipath I/O) est souvent utilisé pour les déploiements SQL Server, la cible multivoie du mappeur d’appareil (DM) doit également être configurée pour utiliser l’infrastructure `blk-mq` en activant l’option d’amorçage du noyau **dm_mod.use_blk_mq=y**. La valeur par défaut est `n` (désactivé). Ce paramètre, lorsque les appareils SCSI sous-jacents utilisent `blk-mq`, réduit la surcharge de verrouillage au niveau de la couche DM. Reportez-vous à la documentation de la distribution Linux utilisée pour obtenir des conseils supplémentaires sur la façon de le configurer.

#### <a name="configure-swapfile"></a>Configurer un fichier d’échange

Vérifiez que vous disposez d’un fichier d’échange correctement configuré pour éviter les problèmes de mémoire insuffisante. Consultez la documentation Linux pour savoir comment créer et dimensionner correctement un fichier d’échange.

#### <a name="virtual-machines-and-dynamic-memory"></a>Machines virtuelles et mémoire dynamique

Si vous exécutez SQL Server sur Linux sur une machine virtuelle, assurez-vous de sélectionner des options pour corriger la quantité de mémoire réservée pour la machine virtuelle. N’utilisez pas de fonctionnalités comme Mémoire dynamique Hyper-V.

## <a name="sql-server-configuration"></a>Configuration de SQL Server

Il est recommandé d’effectuer les tâches de configuration suivantes après avoir installé SQL Server sur Linux pour obtenir les meilleures performances pour votre application.

### <a name="best-practices"></a>Meilleures pratiques

- **Utiliser PROCESS AFFINITY pour le nœud et/ou les UC**

   Il est recommandé d’utiliser `ALTER SERVER CONFIGURATION` pour définir `PROCESS AFFINITY` pour tous les nœuds **NUMANODE** et/ou les processeurs que vous utilisez pour SQL Server (généralement pour tous les nœuds et tous les processeurs) sur un système d’exploitation Linux. L’affinité du processus permet de conserver un comportement de planification Linux et SQL efficace. L’utilisation de l’option **NUMANODE** est la méthode la plus simple. Utilisez **PROCESS AFFINITY** même si vous n’avez qu’un seul nœud NUMA sur votre ordinateur. Pour plus d’informations sur la manière de définir **PROCESS AFFINITY**, consultez l’article [ALTER SERVER CONFIGURATION](../t-sql/statements/alter-server-configuration-transact-sql.md).

- **Configurer plusieurs fichiers de données tempdb**

   Étant donné qu’une installation SQL Server sur Linux ne propose pas d’option de configuration de plusieurs fichiers tempdb, nous vous recommandons de considérer la création de plusieurs fichiers de données tempdb après l’installation. Pour plus d’informations, reportez-vous aux instructions de l’article [Suggestions pour réduire la contention d’allocation dans la base de données tempdb SQL Server](https://support.microsoft.com/help/2154845/recommendations-to-reduce-allocation-contention-in-sql-server-tempdb-d).

### <a name="advanced-configuration"></a>Configuration avancée

Les suggestions suivantes sont des paramètres de configuration facultatifs que vous pouvez choisir d’effectuer après l’installation de SQL Server sur Linux. Ces choix sont basés sur les exigences de votre charge de travail et la configuration de votre système d’exploitation Linux.

- **Définir une limite de mémoire avec mssql-conf**

   Pour garantir qu’il y a suffisamment de mémoire physique disponible pour le système d’exploitation Linux, le processus SQL Server utilise uniquement 80 % de la mémoire RAM physique par défaut. Pour certains systèmes qui ont une grande quantité de mémoire RAM physique, 20 % peut être un nombre significatif. Par exemple, sur un système avec 1 To de RAM, le paramètre par défaut laisse environ 200 Go de RAM inutilisée. Dans ce cas, vous souhaiterez peut-être configurer la limite de la mémoire sur une valeur plus élevée. Consultez la documentation sur l'outil **mssql-conf** et le paramètre [memory.memorylimitmb](sql-server-linux-configure-mssql-conf.md#memorylimit) qui contrôle la mémoire visible pour SQL Server (en mégaoctets).

   Lorsque vous modifiez ce paramètre, veillez à ne pas définir cette valeur trop élevée. Si vous ne laissez pas assez de mémoire, vous pouvez rencontrer des problèmes avec le système d’exploitation Linux et d’autres applications Linux.

## <a name="next-steps"></a>Étapes suivantes

Pour en savoir plus sur les fonctionnalités de SQL Server qui améliorent les performances, consultez [Prise en main des fonctionnalités de performances](sql-server-linux-performance-get-started.md).

Pour plus d’informations sur SQL Server sur Linux, consultez [Vue d’ensemble de SQL Server sur Linux](sql-server-linux-overview.md).
