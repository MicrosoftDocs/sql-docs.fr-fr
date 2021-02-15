---
title: Installer le CLR personnalisé Python
description: Découvrez comment installer un runtime personnalisé Python pour SQL Server à l’aide des extensions de langage. Le runtime Python personnalisé peut exécuter des scripts de Machine Learning.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 02/08/2021
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
zone_pivot_groups: sqlml-platforms
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15'
ms.openlocfilehash: 23f1bac7674002178602c0d8fa844531ea4f1a8b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100072874"
---
# <a name="install-a-python-custom-runtime-for-sql-server"></a>Installer un CLR personnalisé Python pour SQL Server
[!INCLUDE [SQL Server 2019 and later](../../includes/applies-to-version/sqlserver2019.md)]

Découvrez comment installer un runtime Python personnalisé pour l’exécution de scripts Python externes avec SQL Server sur :

+ Windows
+ Ubuntu Linux
+ Red Hat Enterprise Linux (RHEL)
+ SLES (SUSE Linux Enterprise Server)

Le runtime personnalisé peut exécuter des scripts de Machine Learning et utilise les [extensions de langage SQL Server](../../language-extensions/language-extensions-overview.md).

Utilisez votre propre version du runtime Python avec SQL Server, au lieu de celle par défaut installée avec [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md).

::: zone pivot="platform-windows"
[!INCLUDE [Python custom runtime - Windows](includes/custom-runtime-python-windows.md)]
::: zone-end

::: zone pivot="platform-linux-ubuntu"
[!INCLUDE [Python custom runtime - Linux - Prerequisites](includes/custom-runtime-python-linux-prerequisites.md)]

[!INCLUDE [Python custom runtime - Linux - Ubuntu specific steps](includes/custom-runtime-python-linux-ubuntu.md)]

[!INCLUDE [Python custom runtime on Linux - Common steps](includes/custom-runtime-python-linux-common.md)]
::: zone-end

::: zone pivot="platform-linux-rhel"
[!INCLUDE [Python custom runtime - Linux - Prerequisites](includes/custom-runtime-python-linux-prerequisites.md)]

[!INCLUDE [Python custom runtime - Linux - RHEL specific steps](includes/custom-runtime-python-linux-rhel.md)]

[!INCLUDE [Python custom runtime on Linux - Common steps](includes/custom-runtime-python-linux-common.md)]
::: zone-end

::: zone pivot="platform-linux-sles"
[!INCLUDE [Python custom runtime - Linux - Prerequisites](includes/custom-runtime-python-linux-prerequisites.md)]

[!INCLUDE [Python custom runtime - Linux - SLES specific steps](includes/custom-runtime-python-linux-sles.md)]

[!INCLUDE [Python custom runtime on Linux - Common steps](includes/custom-runtime-python-linux-common.md)]
::: zone-end

## <a name="enable-external-script"></a>Activer le script externe

Vous pouvez exécuter un script externe Python avec la procédure stockée [sp_execute_external script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).

Pour activer les scripts externes, utilisez [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md) pour exécuter l’instruction ci-dessous.

```sql
sp_configure 'external scripts enabled', 1;
RECONFIGURE WITH OVERRIDE;  
```

## <a name="verify-installation"></a>Vérifier l'installation

Utilisez le script SQL suivant pour vérifier l’installation et les fonctionnalités du runtime personnalisé Python.

```sql
EXEC sp_execute_external_script
@language =N'myPython',
@script=N'
import sys
print(sys.path)
print(sys.version)
print(sys.executable)'
```

## <a name="next-steps"></a>Étapes suivantes

+ [Installer un CLR personnalisé R pour SQL Server](custom-runtime-r.md)
+ [Framework d’extensibilité dans SQL Server](../concepts/extensibility-framework.md)
+ [Vue d’ensemble des extensions de langage](../../language-extensions/language-extensions-overview.md)
