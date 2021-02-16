---
title: Basculer manuellement une instance de cluster de basculement - SQL Server sur Linux
description: Découvrez comment effectuer un basculement manuel d’une instance de cluster de basculement (FCI) sur SQL Server sur Linux, en particulier Red Hat Linux Enterprise, Ubuntu et SUSE Linux Enterprise Server.
ms.custom: seo-lt-2019
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 12/06/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: ''
ms.openlocfilehash: 93bff6d536a6c1c5ad4eb8d8805fefb706377594
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346293"
---
# <a name="operate-failover-cluster-instance---sql-server-on-linux"></a>Utiliser une instance de cluster de basculement - SQL Server sur Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Cet article explique comment opérer une instance de cluster de basculement SQL Server sur Linux. Si vous n’avez pas créé d’instance de cluster de basculement SQL Server sur Linux, consultez [Configurer une instance de cluster de basculement - SQL Server sur Linux](sql-server-linux-shared-disk-cluster-configure.md). 

## <a name="failover"></a>Basculement

Le basculement pour les instances de cluster de basculement est similaire à un cluster de basculement Windows Server (WSFC). Si le nœud de cluster qui héberge l’instance de cluster de basculement subit une défaillance, l’instance de cluster de basculement doit automatiquement basculer vers un autre nœud. Contrairement à un WSFC, il n’existe aucun moyen de définir des propriétaires préférés. Par conséquent, Pacemaker sélectionne le nœud qui sera le nouvel hôte de l’instance de cluster de basculement.

Il peut arriver que vous souhaitiez basculer manuellement l’instance de cluster de basculement vers un autre nœud. Le processus n’est pas le même qu’avec les instances de cluster de basculement sur un WSFC. Sur un WSFC, vous basculez des ressources au niveau du rôle. Dans Pacemaker, vous choisissez une ressource à déplacer et en supposant que toutes les contraintes sont correctes, tout le reste sera également déplacé. 

Le mode de basculement dépend de la distribution Linux. Suivez les instructions pour votre distribution Linux.

- [RHEL ou Ubuntu](#manual-failover-rhel-or-ubuntu)
- [SLES](#manual-failover-sles)

## <a name="manual-failover-rhel-or-ubuntu"></a>Basculement manuel (RHEL ou Ubuntu)

Pour effectuer un basculement manuel sur des serveurs Red Hat Enterprise Linux (RHEL) ou Ubuntu, exécutez les étapes suivantes.
1.  Émettez les commandes suivantes : 

   ```bash
   sudo pcs resource move <FCIResourceName> <NewHostNode> 
   ```

   \<FCIResourceName> est le nom de ressource Pacemaker de l’interface de cluster de basculement SQL Server.

   \<NewHostNode> est le nom du nœud de cluster sur lequel vous souhaitez héberger l’instance de cluster de basculement. 

   Vous n’obtiendrez pas d’accusé de réception.

2.  Pendant un basculement manuel, Pacemaker crée une contrainte d’emplacement sur la ressource qui a été choisie pour le déplacement manuel. Pour afficher cette contrainte, exécutez `sudo pcs constraint`.

3.  Une fois le basculement terminé, supprimez la contrainte en émettant `sudo pcs resource clear <FCIResourceName>`. 

\<FCIResourceName> est le nom de ressource Pacemaker de l’interface de cluster de basculement. 

## <a name="manual-failover-sles"></a>Basculement manuel (SLES)


Dans SUSE Linux Enterprise Server (SLES), utilisez la commande `migrate` pour basculer manuellement une interface de cluster de basculement SQL Server. Par exemple :

```bash
crm resource migrate <FCIResourceName> <NewHostNode>
```

\<FCIResourceName> est le nom de la ressource de l’instance de cluster de basculement. 

\<NewHostNode> est le nom du nouvel hôte de destination. 


<!---

|Distribution |Topic 
|----- |-----
|**Red Hat Enterprise Linux with HA add-on** |[Configure](sql-server-linux-shared-disk-cluster-red-hat-7-configure.md)<br/>[Operate](sql-server-linux-shared-disk-cluster-red-hat-7-operate.md)
|**SUSE Linux Enterprise Server with HA add-on** |[Configure](sql-server-linux-shared-disk-cluster-sles-configure.md)

--->

## <a name="next-steps"></a>Étapes suivantes

- [Configurer l’instance de cluster de basculement - SQL Server sur Linux](sql-server-linux-shared-disk-cluster-configure.md)

<!--Image references-->
