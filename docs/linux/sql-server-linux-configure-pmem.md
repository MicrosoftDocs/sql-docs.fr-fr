---
title: Configurer la mémoire persistante (PMEM)
description: Découvrez comment configurer la mémoire persistante (PMEM) pour SQL Server sur Linux et créer des espaces de noms pour les appareils PMEM.
ms.custom: seo-lt-2019
author: briancarrig
ms.author: brcarrig
ms.date: 10/31/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
monikerRange: '>= sql-server-linux-ver15  || >= sql-server-ver15'
ms.openlocfilehash: c11807faa2180d3f86036392d42c7f6ee6b9f473
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348201"
---
# <a name="configure-persistent-memory-pmem-for-sql-server-on-linux"></a>Configurer la mémoire persistante (PMEM) pour SQL Server sur Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Cet article décrit comment configurer la mémoire persistante (PMEM) pour [!INCLUDE[sqlv15](../includes/sssql19-md.md)] sur Linux.

## <a name="overview"></a>Vue d’ensemble

[!INCLUDE[sqlv15](../includes/sssql19-md.md)] dispose d’un certain nombre de fonctionnalités en mémoire qui utilisent la mémoire persistante. Ce document décrit les étapes nécessaires à la configuration de la mémoire persistante pour SQL Server sur Linux.

> [!NOTE]
> Le terme _enlightment_ (« illumination ») a été introduit pour décrire le concept d’utiliser un système de fichiers prenant en charge la mémoire persistante. L’accès direct au système de fichiers à partir d’applications d’espace utilisateur est facilité par l’utilisation du mappage de mémoire (`mmap()`). Lors de la création d’un mappage de mémoire pour un fichier, l’application peut émettre des instructions de chargement/stockage en contournant totalement la pile d’E/S. Il s’agit d’une méthode d’accès aux fichiers « enlightened » (ou illuminée) du point de vue de l’application d’extension hôte (qui est le code de boîte noire qui autorise SQLPAL à interagir avec le système d’exploitation Windows ou Linux).

## <a name="create-namespaces-for-pmem-devices"></a>Créer des espaces de noms pour les appareils PMEM

### <a name="configure-the-devices"></a>Configurer les appareils

Dans Linux, utilisez l’utilitaire `ndctl`.

- Installez `ndctl` pour configurer l’appareil PMEM. Vous pouvez le trouver [ici](https://docs.pmem.io/getting-started-guide/installing-ndctl).
- Utilisez `ndctl` pour créer un espace de noms. Les espaces de noms sont entrelacés sur les NVDIMM PMEM et peuvent fournir différents types d’accès d’espace utilisateur aux régions de la mémoire sur l’appareil. `fsdax` est le mode par défaut et souhaité pour SQL Server.

```bash 
ndctl create-namespace -f -e namespace0.0 --mode=fsdax* --map=dev
```

Notez que nous avons choisi le mode `fsdax` et que nous utilisons la mémoire système pour stocker les métadonnées par page. Nous vous recommandons d’utiliser `--map=dev`. Cela permet de stocker les métadonnées directement sur l’espace de noms. Le stockage de métadonnées en mémoire avec `--map=mem` est considéré expérimental pour l’instant.

Utilisez `ndctl` pour vérifier l’espace de noms. 
  
Un exemple est alors sorti :

```bash
# ndctl list -N
{
  "dev":"namespace0.0",
  "mode":"fsdax",
  "map":"dev",
  "size":4294967296,
  "sector_size":512,
  "blockdev":"pmem0",
  "numa_node":0
}
```

### <a name="create-and-mount-pmem-device"></a>Créez et montez un appareil PMEM

Par exemple, avec XFS

```bash
mkfs.xfs -f /dev/pmem0
mount -o dax,noatime /dev/pmem0 /mnt/dax
xfs_io -c "extsize 2m" /mnt/dax
```

Par exemple, avec EXT4

```bash
mkfs.ext4 -b 4096 -E stride=512 -F /dev/pmem0
mount -o dax,noatime /dev/pmem0 /mnt/dax
```

## <a name="technical-considerations"></a>Considérations techniques

- Bloquer l’allocation de 2 Mo pour XFS/EXT4, comme décrit ci-dessus
- Un mauvais alignement entre l’allocation de bloc et `mmap` entraîne un basculement silencieux sur 4 Ko
- Les tailles de fichier doivent être un multiple de 2 Mo (modulo 2 Mo)
- Ne pas désactiver les pages THP (Transparent huge page) (activées par défaut sur la plupart des distributions)

Une fois que l’appareil a été configuré avec `ndctl`, créé et monté, vous pouvez y placer des fichiers de base de données ou créer une nouvelle base de données.

Étant donné que les appareils PMEM sont O_DIRECT-safe (E/S directes), activez l’indicateur de trace 3979 pour désactiver le mécanisme de vidage forcé. Pour plus d’informations, consultez [Support FUA](https://support.microsoft.com/help/4131496/enable-forced-flush-mechanism-in-sql-server-2017-on-linux). Les éléments internes d’accès aux unités forcées sont décrits ici [FUA internals](/archive/blogs/bobsql/sql-server-on-linux-forced-unit-access-fua-internals).

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur SQL Server sur Linux, consultez [SQL Server sur Linux](sql-server-linux-overview.md).
Pour des bonnes pratiques de performance pour SQL Server sur Linux, consultez [Bonnes pratiques de performance](sql-server-linux-performance-best-practices.md).