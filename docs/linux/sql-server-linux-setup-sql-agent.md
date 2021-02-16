---
title: Configurer SQL Server Agent sur Linux
description: Découvrez comment activer ou installer SQL Server Agent sur Linux. À partir de SQL Server 2017 CU4, SQL Server Agent est inclus avec le package mssql-server.
author: VanMSFT
ms.author: vanto
ms.date: 12/05/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 77f16adc-e6cb-4a57-82f3-7b9780369868
ms.openlocfilehash: 8ca8de4b922ad2108ad40996713cf47501b29dca
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346389"
---
# <a name="install-sql-server-agent-on-linux"></a>Installer SQL Server Agent sur Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Cet article explique comment activer ou installer SQL Server Agent sur Linux.

[SQL Server Agent](../ssms/agent/sql-server-agent.md) exécute des tâches SQL Server planifiées. En commençant par SQL Server 2017 CU4, SQL Server Agent est inclus avec le package **mssql-server** et est désactivé par défaut. Pour plus d’informations sur les fonctionnalités prises en charge pour cette version de SQL Server Agent, ainsi que les informations sur la version, consultez les [Notes de publication](sql-server-linux-release-notes.md).

## <a name="instructions"></a>Instructions

Avant d’utiliser SQL Server Agent sur Linux, effectuez les étapes suivantes pour l’activer ou l’installer.

1. Ajoutez votre nom d’hôte (avec et sans domaine) dans les fichiers `/etc/hosts`. Les lignes suivantes illustrent le format utilisé pour ces entrées :

   ```bash
   "IP Address" "hostname"
   "IP Address" "hostname.domain.com"
   ```

1. Suivez les instructions de l’une des sections suivantes selon votre version de SQL Server :

   | Versions | Instructions |
   |---|---|
   | SQL Server 2017 CU4 et versions ultérieures</br>SQL Server 2019 | [Activer SQL Server Agent](#EnableAgentAfterCU4) |
   | SQL Server 2017 CU3 et versions antérieures | [Installer SQL Server Agent](#InstallAgentBelowCU4) |

## <a name="enable-the-sql-server-agent"></a><a id="EnableAgentAfterCU4"></a>Activer SQL Server Agent

Pour SQL Server 2019 et SQL Server 2017 CU4 (et versions ultérieures), vous avez besoin d’activer SQL Server Agent uniquement. Vous n’avez pas besoin d’installer un package distinct.

Pour activer SQL Server Agent, procédez comme suit.

```bash
sudo /opt/mssql/bin/mssql-conf set sqlagent.enabled true 
sudo systemctl restart mssql-server
```

> [!NOTE]
> Si vous effectuez une mise à niveau à partir de 2017 CU3 ou version antérieure avec l’agent installé, SQL Server Agent est activé automatiquement et les packages d’agent précédents sont désinstallés.  

## <a name="install-the-sql-server-agent"></a><a name="InstallAgentBelowCU4"></a>Installer SQL Server Agent

Pour SQL Server 2017 CU3 (et versions antérieures), vous devez installer le package SQL Server Agent.

> [!NOTE]
> Les instructions d’installation ci-dessous s’appliquent à SQL Server versions 2017 CU3 et antérieures. Avant d’installer SQL Server Agent, commencez par [installer SQL Server](sql-server-linux-setup.md#platforms). Cela permet de configurer les clés et les référentiels que vous utilisez pour installer le package **mssql-server-agent**.

Installer SQL Server Agent pour votre plateforme :
- [Red Hat Enterprise Linux](#RHEL)
- [Ubuntu](#ubuntu)
- [SUSE Linux Enterprise Server](#SLES)

### <a name=""></a><a name="RHEL">Installer sur RHEL</a>

Utilisez les commandes suivantes pour installer **mssql-server-agent** sur Red Hat Enterprise Linux. 

```bash
sudo yum install mssql-server-agent
sudo systemctl restart mssql-server
```

Si vous avez déjà installé **mssql-server-agent**, vous pouvez mettre à jour vers la dernière version avec les commandes suivantes :

```bash
sudo yum check-update
sudo yum update mssql-server-agent
sudo systemctl restart mssql-server
```

Si vous avez besoin d’une installation hors connexion, recherchez le téléchargement du package SQL Server Agent dans les [Notes de publication](sql-server-linux-release-notes.md). Puis utilisez les mêmes étapes d’installation hors connexion décrites dans l’article [Installer SQL Server](sql-server-linux-setup.md#offline).

### <a name=""></a><a name="ubuntu">Installer sur Ubuntu</a>

Utilisez les commandes suivantes pour installer **mssql-server-agent** sur Ubuntu. 

```bash
sudo apt-get update 
sudo apt-get install mssql-server-agent
sudo systemctl restart mssql-server
```

Si vous avez déjà installé **mssql-server-agent**, vous pouvez mettre à jour vers la dernière version avec les commandes suivantes :

```bash
sudo apt-get update 
sudo apt-get install mssql-server-agent
sudo systemctl restart mssql-server
```

Si vous avez besoin d’une installation hors connexion, recherchez le téléchargement du package SQL Server Agent dans les [Notes de publication](sql-server-linux-release-notes.md). Puis utilisez les mêmes étapes d’installation hors connexion décrites dans l’article [Installer SQL Server](sql-server-linux-setup.md#offline).

### <a name=""></a><a name="SLES">Installer sur SLES</a>

Utilisez les étapes suivantes pour installer **mssql-server-agent** sur SUSE Linux Enterprise Server. 

Installer **mssql-server-agent** 

```bash
sudo zypper install mssql-server-agent
sudo systemctl restart mssql-server
```

Si vous avez déjà installé **mssql-server-agent**, vous pouvez mettre à jour vers la dernière version avec les commandes suivantes :

```bash
sudo zypper refresh
sudo zypper update mssql-server-agent
sudo systemctl restart mssql-server
```

Si vous avez besoin d’une installation hors connexion, recherchez le téléchargement du package SQL Server Agent dans les [Notes de publication](sql-server-linux-release-notes.md). Puis utilisez les mêmes étapes d’installation hors connexion décrites dans l’article [Installer SQL Server](sql-server-linux-setup.md#offline).

## <a name="next-steps"></a>Étapes suivantes
Pour plus d’informations sur l’utilisation de SQL Server Agent pour créer, planifier et exécuter des tâches, consultez [Exécuter une tâche SQL Server Agent sur Linux](sql-server-linux-run-sql-server-agent-job.md).