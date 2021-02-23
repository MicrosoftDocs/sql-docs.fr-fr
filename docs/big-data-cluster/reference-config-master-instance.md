---
title: Propriétés de configuration de l’instance maître SQL Server
titleSuffix: SQL Server big data clusters
description: Article de référence pour les propriétés de configuration pour une instance maître SQL Server.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: rahul.ajmera
ms.date: 02/11/2021
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 2d986013374e7f69111288d2d0f50b09130a2d68
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/13/2021
ms.locfileid: "100343510"
---
# <a name="sql-server-master-instance-configuration-properties----pre-cu9-release"></a>Propriétés de configuration de l’instance maître de SQL Server - Version antérieure à CU9

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]
> [!NOTE]
> Les informations suivantes s’appliquent uniquement aux clusters dont la version est antérieure à CU9, qui ne sont pas configurables et qui nécessitent mssql-conf pour configurer l’instance maître de SQL Server. Les clusters CU9 et versions ultérieures tirent parti de la fonctionnalité de gestion de la configuration et n’ont donc plus besoin d’un fichier mssql-conf. Vous trouverez [ici](reference-config-bdc-overview.md) les configurations disponibles pour l’instance maître de SQL Server et d’autres composants de clusters Big Data.

## <a name="properties"></a>Propriétés

Vous pouvez configurer les options de SQL Server suivantes pour l’instance maître au moment du déploiement.

|Propriété|Options|
| --- | --- |
|`[sqlagent]`|`enabled = { true | false }` |
|`[telemetry]`|`customerfeedback = { true | false }` |
|`[telemetry]`|`userRequestedLocalAuditDirectory = </path/file>`|
|`[licensing]`| `pid = { Enterprise | Developer }`|
|`[traceflag]`|` traceflag<#> = <####>`|

### <a name="examples"></a>Exemples

L’exemple suivant active le service SQL Agent, la télémétrie, définit un PID pour Édition Entreprise et active l’indicateur de trace 1204. :

```
[sqlagent]
enabled=true

[telemetry]
customerfeedback=true
userRequestedLocalAuditDirectory = /tmp/audit

[licensing]
pid = Enterprise

[traceflag]
traceflag0 = 1204
```

Pour obtenir des instructions, consultez [Configurer l’instance maître de [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](configure-sql-server-master-instance.md).

## <a name="next-steps"></a>Étapes suivantes

[Configurer l’instance maître de [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](configure-sql-server-master-instance.md)
