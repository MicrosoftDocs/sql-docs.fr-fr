---
title: Installer PolyBase sur Linux
titlesuffix: SQL Server
description: Découvrez comment installer PolyBase SQL Server sur Linux. PolyBase vous permet d’exécuter des requêtes externes par rapport à des sources de données distantes.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: dakryze
ms.date: 8/18/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
monikerRange: '>= sql-server-linux-ver15 || >= sql-server-ver15'
ms.openlocfilehash: 85beab1eb4d256c0433678ca34a8ac2627ebfb52
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100062244"
---
# <a name="install-polybase-on-linux"></a>Installer PolyBase sur Linux

[!INCLUDE [sqlserver2019-linux](../../includes/applies-to-version/sqlserver2019-linux.md)]

Les étapes suivantes permettent d’installer [PolyBase](../../relational-databases/polybase/polybase-guide.md) (`mssql-server-polybase` et `mssql-server-polybase-hadoop`) sur Linux. PolyBase vous permet d’exécuter des requêtes externes par rapport à des sources de données distantes.

>[!NOTE]
> Avant d’installer Polybase, commencez par [installer SQL Server 2019](../../linux/sql-server-linux-setup.md#platforms). Cela permet de configurer les clés et les référentiels que vous utilisez pendant l’installation du package `mssql-server-polybase` et `mssql-server-polybase-hadoop`.

>[!NOTE]
>
> - PolyBase n’est pas pris en charge par SQL Server 2017 pour Linux.
> - Le scale-out pour PolyBase sur Linux est actuellement indisponible.

Installation de PolyBase pour votre système d’exploitation :

- [Red Hat Enterprise Linux](#RHEL)
- [Ubuntu](#ubuntu)
- [SUSE Linux Enterprise Server](#SLES)

## <a name="install-on-rhel"></a><a name="RHEL"></a>Installer sur RHEL

Utilisez la commande suivante pour installer `mssql-server-polybase` sur Red Hat Enterprise Linux. 

```bash
sudo yum install -y mssql-server-polybase
```

Vous devrez redémarrer l’instance de SQL Server. Pour ce faire, exécutez la commande suivante.

```bash
sudo systemctl restart mssql-server
```

>[!NOTE]
>Après l’installation, vous devez [activer la fonctionnalité PolyBase](#enable).

Utilisez la commande suivante pour installer `mssql-server-polybase-hadoop`. 

```bash
sudo yum install -y mssql-server-polybase-hadoop
```

Le package Polybase Hadoop présente des dépendances vis-à-vis des packages suivants :
- `mssql-server`
- `mssql-server-polybase`
- `mssql-server-extensibility`
- `mssql-zulu-jre-11`. 

L’installation invite à redémarrer `launchpadd`. Pour ce faire, exécutez la commande suivante.

```bash
sudo systemctl restart mssql-launchpadd
```

>[!NOTE]
>Après l’installation, vous devez [définir le niveau de connectivité de Hadoop](../../database-engine/configure-windows/polybase-connectivity-configuration-transact-sql.md#c-set-hadoop-connectivity).

Si vous avez besoin d’une installation hors connexion, recherchez le téléchargement du package PolyBase dans les [Notes de publication](../../linux/sql-server-linux-release-notes.md). Puis utilisez les mêmes étapes d’installation hors connexion décrites dans l’article [Installer SQL Server](../../linux/sql-server-linux-setup.md#offline).

## <a name="install-on-ubuntu"></a><a name="ubuntu"></a>Installer sur Ubuntu

Utilisez la commande suivante pour installer `mssql-server-polybase` sur Ubuntu. 

```bash
sudo apt-get install mssql-server-polybase
```

Vous devrez redémarrer l’instance de SQL Server. Pour ce faire, exécutez la commande suivante.

```bash
sudo systemctl restart mssql-server
```

>[!NOTE]
>Après l’installation, vous devez [activer la fonctionnalité PolyBase](#enable).

Si vous avez besoin d’une installation hors connexion, recherchez le téléchargement du package PolyBase dans les [Notes de publication](../../linux/sql-server-linux-release-notes.md). Puis utilisez les mêmes étapes d’installation hors connexion décrites dans l’article [Installer SQL Server](../../linux/sql-server-linux-setup.md#offline).

Utilisez la commande suivante pour installer `mssql-server-polybase-hadoop`. 

```bash
sudo apt-get install mssql-server-polybase-hadoop
```

Le package Polybase Hadoop présente des dépendances vis-à-vis des packages suivants :
- `mssql-server`
- `mssql-server-polybase`
- `mssql-server-extensibility`
- `mssql-zulu-jre-11`. 

L’installation invite à redémarrer `launchpadd`. Pour ce faire, exécutez la commande suivante.

```bash
sudo systemctl restart mssql-launchpadd
```

>[!NOTE]
>Après l’installation, vous devez [définir le niveau de connectivité de Hadoop](../../relational-databases/polybase/polybase-configure-hadoop.md#configure-hadoop-connectivity).

## <a name="install-on-sles"></a><a name="SLES"></a>Installer sur SLES

Utilisez la commande suivante pour installer `mssql-server-polybase` sur SLES. 

```bash
sudo zypper install mssql-server-polybase
```

Vous devrez redémarrer l’instance de SQL Server. Pour ce faire, exécutez la commande suivante.

```bash
sudo systemctl restart mssql-server
```

>[!NOTE]
>Après l’installation, vous devez [activer la fonctionnalité PolyBase](#enable).

Si vous avez besoin d’une installation hors connexion, recherchez le téléchargement du package PolyBase dans les [Notes de publication](../../linux/sql-server-linux-release-notes.md). Puis utilisez les mêmes étapes d’installation hors connexion décrites dans l’article [Installer SQL Server](../../linux/sql-server-linux-setup.md#offline).


## <a name="enable-polybase"></a><a name="enable"></a> Activer PolyBase

Après l’installation, vous devez activer PolyBase pour accéder à ses fonctionnalités. Connectez-vous à l’instance de SQL Server installée et utilisez la commande Transact-SQL suivante pour l’activation.

```sql
exec sp_configure @configname = 'polybase enabled', @configvalue = 1;
RECONFIGURE WITH OVERRIDE;
```

## <a name="update-polybase"></a>Mettre à jour PolyBase

Si vous avez déjà installé `mssql-server-polybase`, vous pouvez mettre à jour vers la dernière version avec les commandes suivantes :

### <a name="rhel"></a>RHEL

```bash
sudo yum remove -y mssql-server-polybase-hadoop
sudo yum remove -y mssql-server-polybase
sudo yum check-update
sudo yum install -y mssql-server-polybase
sudo yum install -y mssql-server-polybase-hadoop
```

Vous devrez redémarrer l’instance de SQL Server. Pour ce faire, exécutez la commande suivante.

```
sudo systemctl restart mssql-server
```

### <a name="ubuntu"></a>Ubuntu

```bash
sudo apt-get remove mssql-server-polybase-hadoop
sudo apt-get remove mssql-server-polybase
sudo apt-get update 
sudo apt-get install mssql-server-polybase
sudo apt-get remove mssql-server-polybase-hadoop
```

Vous devrez redémarrer l’instance de SQL Server. Pour ce faire, exécutez la commande suivante.

```
sudo systemctl restart mssql-server
```

### <a name="sles"></a>SLES

```bash
sudo zypper remove mssql-server-polybase
sudo zypper refresh
sudo zypper install mssql-server-polybase
```

Vous devrez redémarrer l’instance de SQL Server. Pour ce faire, exécutez la commande suivante.

```
sudo systemctl restart mssql-server
```

>[!NOTE]
>Après l’installation, vous devez [activer la fonctionnalité PolyBase](#enable).

## <a name="next-steps"></a>Étapes suivantes

PolyBase sur Linux peut accéder aux sources de données suivantes. Suivez les liens pour plus d’informations sur la façon dont la création d’une table externe à partir de ces sources sur PolyBase est permise. 

- [SQL Server, SQL Database, Azure Synapse Analytics)](../../relational-databases/polybase/polybase-configure-sql-server.md)
- [Hadoop](../../relational-databases/polybase/polybase-configure-hadoop.md)
- [Stockage Blob Azure](../../relational-databases/polybase/polybase-configure-azure-blob-storage.md)
- [Oracle](../../relational-databases/polybase/polybase-configure-oracle.md)
- [Teradata](../../relational-databases/polybase/polybase-configure-teradata.md)
- [MongoDB (& Cosmos DB)](../../relational-databases/polybase/polybase-configure-mongodb.md)

Pour plus d’informations sur l’utilisation de ces paramètres, consultez l’article de référence sur Transact-SQL pour [CREATE EXTERNAL TABLE](../../t-sql/statements/create-external-table-transact-sql.md).
