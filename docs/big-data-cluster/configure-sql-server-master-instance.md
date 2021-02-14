---
title: Configurer l’instance maître SQL Server
titleSuffix: Configure SQL Server master instance of Big Data Cluster
description: Configurer l’instance maître d’un cluster Big Data SQL Server
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.date: 11/04/2019
ms.topic: overview
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: de9bba220f985522926d610b36418d9607c8a568
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100039054"
---
# <a name="configure-master-instance-of-big-data-clusters-2019"></a>Configurer l’instance maître des [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Configurez l’instance maître des [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)].

Impossible de configurer les paramètres de configuration du serveur pour l’instance maître SQL Server au moment du déploiement. Cet article décrit une solution de contournement temporaire sur la manière de configurer des paramètres tels que l’édition SQL Server, d’activer ou de désactiver SQL Server Agent, d’activer des indicateurs de trace spécifiques ou d’activer/de désactiver les commentaires des clients.

Pour modifier l’un de ces paramètres, procédez comme suit :

1. Créez un fichier `mssql-custom.conf` personnalisé qui comprend les paramètres ciblés. L’exemple suivant active le service SQL Agent, la télémétrie, définit un PID pour Édition Entreprise et active l’indicateur de trace 1204. :

   ```
   [sqlagent]
   enabled=true
   
   [telemetry]
   customerfeedback=true
   userRequestedLocalAuditDirectory = /tmp/audit

   [DEFAULT]
   pid = Enterprise

   [traceflag]
   traceflag0 = 1204
   ```

1. Copiez le fichier `mssql-custom.conf` dans `/var/opt/mssql` dans le conteneur `mssql-server` du pod `master-0`. Remplacez `<namespaceName>` par le nom du cluster Big Data.

   ```bash
   kubectl cp mssql-custom.conf master-0:/var/opt/mssql/mssql-custom.conf -c mssql-server -n <namespaceName>
   ```

1. Redémarrez l’instance SQL Server.  Remplacez `<namespaceName>` par le nom du cluster Big Data.

   ```bash
   kubectl exec -it master-0  -c mssql-server -n <namespaceName> -- /bin/bash
   supervisorctl restart mssql-server
   exit
   ```

> [!IMPORTANT]
> Si l’instance maître SQL Server est dans une configuration de groupes de disponibilité, copiez le fichier `mssql-custom.conf` dans toutes les pods `master`. Notez que chaque redémarrage entraînant un basculement, vous devez veiller à planifier cette activité pendant les périodes d’inactivité.

## <a name="known-limitations"></a>Limitations connues

- Les étapes ci-dessus nécessitent des autorisations d’administrateur de cluster Kubernetes
- Vous ne pouvez pas modifier le classement du serveur pour l’instance maître SQL Server du cluster Big Data après le déploiement.

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur le déploiement des clusters Big Data SQL Server, consultez [Bien démarrer avec les clusters Big Data SQL Server](deploy-get-started.md).