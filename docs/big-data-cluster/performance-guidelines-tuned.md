---
title: Meilleures pratiques en matière de performances pour Clusters Big Data SQL Server
description: Cet article donne les meilleures pratiques en matière de performances et des recommandations concernant l’exécution de Clusters Big Data SQL Server sur Kubernetes.
author: mihaelablendea
ms.author: mihaelab
ms.reviewer: mikeray
ms.date: 06/30/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 7b430e09a06523bc3ee369434911738f83ab5ee4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100044139"
---
# <a name="performance-best-practices-and-configuration-guidelines-for-sql-server-big-data-clusters"></a>Meilleures pratiques en matière de performances et recommandations de configuration pour Clusters Big Data SQL Server

[!INCLUDE [sqlserver2019](../includes/applies-to-version/sqlserver2019.md)]

Cet article donne les meilleures pratiques et des recommandations pour optimiser les performances des applications qui ciblent des services exécutés au sein d’un cluster Big Data.

Les recommandations suivantes se concentrent sur la configuration du système d’exploitation Linux qui héberge les nœuds Worker Kubernetes sur lesquels le cluster Big Data sera déployé. Il est recommandé de configurer le profil de paramétrage avant de déployer le cluster Big Data. Les paramètres inclus dans le profil de paramétrage proposé ont été validés au cours de l’étude de cas menée par Microsoft et Intel. Les résultats de l’étude sont publiés et téléchargeables dans ce [livre blanc](https://aka.ms/sql-bdc-spark-perf/).

> [!TIP]
> Pour connaître les configurations de paramétrage propres à SQL Server sur Linux, consultez [Meilleures pratiques en matière de performances et recommandations de configuration pour SQL Server sur Linux](../linux/sql-server-linux-performance-best-practices.md). Par ailleurs, d’autres meilleures pratiques, comme la conception d’index pour les bases de données SQL Server, continuent de s’appliquer.

## <a name="proposed-linux-settings-using-a-tuned-mssql-bdc-profile"></a>Paramètres Linux proposés avec un profil tuned `mssql-bdc`

Créez un profil de configuration **tuned.conf** comportant le contenu ci-dessous.

```bash
[main]
summary=Optimize for Microsoft SQL Server Big Data Clusters TPC-DS performance
include=throughput-performance
 
[sysctl]
#network tunings
net.ipv4.conf.default.rp_filter=1
net.ipv4.tcp_timestamps=0
net.ipv4.tcp_sack = 1
net.core.netdev_max_backlog = 25000
net.core.rmem_max = 2147483647
net.core.wmem_max = 2147483647
net.core.rmem_default = 33554431
net.core.wmem_default = 33554432
net.core.optmem_max = 33554432
net.ipv4.tcp_rmem =8192 33554432 2147483647
net.ipv4.tcp_wmem =8192 33554432 2147483647
net.ipv4.tcp_low_latency=1
net.ipv4.tcp_adv_win_scale=1
net.ipv6.conf.all.disable_ipv6 = 1
net.ipv6.conf.default.disable_ipv6 = 1
net.ipv4.conf.all.arp_filter=1
net.ipv4.tcp_retries2=5
net.ipv6.conf.lo.disable_ipv6 = 1
net.core.somaxconn = 65535
 
#memory cache settings
vm.swappiness=1
vm.overcommit_memory=0
vm.dirty_background_ratio=1
 
#kernel NUMA
kernel.numa_balancing=0

#filesystem
fs.aio-max-nr=1048576
 
[vm]
#should be revisited for SQL large pages use in master/data/compute pods
transparent_hugepages=never
```

## <a name="install-tuned-utility-on-all-the-kubernetes-worker-nodes"></a>Installation de l’utilitaire **tuned** sur tous les nœuds Worker Kubernetes

Pour installer **tuned**, exécutez la commande suivante :

```bash
apt-get -y install tuned
```

## <a name="apply-tuning-settings-to-all-kubernetes-worker-nodes"></a>Application des paramètres à tous les nœuds Worker Kubernetes

Sur chacun des nœuds Worker ciblés, copiez le fichier **tuned.conf** créé ci-dessus :

```bash
cd /usr/lib/tuned
scp -r <sourcePath> ./mssql-bdc
```

Pour activer ce profil tuned **mssql-bdc**, enregistrez ces définitions dans un fichier **tuned.conf** dans un dossier `/usr/lib/tuned/mssql-bdc` sur tous les nœuds Worker Kubernetes et activez le profil à l’aide des commandes suivantes :

```bash
chmod +x /usr/lib/tuned/mssql-bdc/tuned.conf
tuned-adm profile mssql-bdc
```

Vérifiez qu’il est activé à l’aide de la commande suivante :

```bash
tuned-adm active
```

or

```bash
tuned-adm list
```

Si le profil est modifié de manière dynamique, vous devez redémarrer **tuned** sur tous les nœuds Worker impactés pour que les nouvelles modifications prennent effet :

```bash
systemctl restart tuned
```
 
Vous trouverez les journaux du service **tuned** à l’adresse */var/log/tuned/tuned.log*.

Si vous le souhaitez, vous pouvez configurer le profil de paramétrage sur un nœud du cluster Kubernetes et utiliser le script ci-dessous pour le copier et le configurer sur les nœuds restants.

```bash
#!/bin/bash -e
# This script takes a list of servers (IPs like `cat ~administrator/workerhosts)) as input
# and update these servers with the local version of mssql-bdc tuned.conf.
 
is_root() {
    local is_root_set=0
    if [ "$EUID" -ne 0 ]; then
        echo "Please run as root"
    else
        is_root_set=1
    fi
    return "${is_root_set}"
}
 
# function main
if is_root -eq 0; then
    exit 0
fi
 
while [ $# -gt 0 ]
do
    # sometimes, people add non-breaking space characters to their *host* files.
    WORKER_IP=$(echo "$1" | sed -e 's/\xC2\xA0//g')
    echo -e "updating mssql-bdc tuned on \e[42m${WORKER_IP}\e[0m"
    # the following commands assume tuned was not set up and active...
    # TODO: add the tuned install and status checks
    ssh "${WORKER_IP}" mkdir -p /usr/lib/tuned/mssql-bdc
    scp ~administrator/tuned.conf "${WORKER_IP}":/usr/lib/tuned/mssql-bdc/tuned.conf
    ssh "${WORKER_IP}" tuned-adm profile mssql-bdc
    ssh "${WORKER_IP}" systemctl restart tuned
    ssh "${WORKER_IP}" tuned-adm active
    shift
done

```

## <a name="next-steps"></a>Étapes suivantes

Pour plus de ressources, notamment les architectures de référence de Clusters Big Data SQL Server, consultez :

* [Étude de cas : Charges de travail SQL exécutées sur Apache Spark dans Clusters Big Data MS SQL Server 2019](https://aka.ms/sql-bdc-spark-perf/)

* [Architecture de référence HPE pour obtenir des informations sur toutes les données avec Clusters Big Data Microsoft SQL Server 2019](https://h20195.www2.hpe.com/V2/GetDocument.aspx?docname=a50001963enw)

* [Dell EMC PowerStore : Clusters Big Data Microsoft SQL Server 2019](https://www.dellemc.com/resources/en-us/asset/white-papers/products/storage/h18231-dell-emc-powerstore-sql-server-big-data-clusters.pdf)

* [Clusters Big Data Microsoft SQL Server 2019 : une solution Big Data qui utilise l’infrastructure Dell EMC](https://infohub.delltechnologies.com/t/microsoft-sql-server-2019-big-data-clusters-a-big-data-solution-using-dell-emc-infrastructure/)

* [Architecture de référence de Clusters Big Data Microsoft SQL Server 2019 sur Cisco UCS](https://www.cisco.com/c/en/us/solutions/collateral/data-center-virtualization/unified-computing/sql-server-on-big-data-cluster-on-ucs.html)