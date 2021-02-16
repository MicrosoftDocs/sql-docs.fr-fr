---
title: Installer SQL Server Integration Services sur Linux
description: Cet article explique comment installer SQL Server Integration Services (SSIS) sur Linux. Vous pouvez installer SSIS sur Ubuntu 16.04 et Red Hat Enterprise Linux.
author: lrtoyou1223
ms.author: lle
ms.reviewer: maghan
ms.date: 01/09/2018
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: 311b8fec7114c4d63680d131ac0988ad4f4a8b15
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346360"
---
# <a name="install-sql-server-integration-services-ssis-on-linux"></a>Installer SQL Server Integration Services (SSIS) sur Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Procédez comme indiqué dans cet article pour installer SQL Server Integration Services (**mssql-server-is**) sur Linux. Pour plus d’informations sur les fonctionnalités qui sont prises en charge dans cette version d’Integration Services pour Linux, consultez les [Notes de publication](sql-server-linux-release-notes.md).

Vous pouvez installer SQL Server Integration Services sur ces plateformes :

- [Ubuntu 16.04](#ubuntu)
- [Red Hat Enterprise Linux](#RHEL)

## <a name="install-ssis-on-ubuntu"></a><a name="ubuntu"></a> Installer SSIS sur Ubuntu

Pour installer le package **mssql-server-is** sur Ubuntu, procédez comme suit :

<!--SQL Server 2017 on Linux-->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

1. Importez les clés GPG de référentiel public.

   ```bash
   curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
   ```

1. Enregistrez le référentiel SQL Server Ubuntu.

   ```bash
   sudo add-apt-repository "$(curl https://packages.microsoft.com/config/ubuntu/16.04/mssql-server-2017.list)"
   ```

1. Exécutez les commandes suivantes pour installer SQL Server Integration Services.

   ```bash
   sudo apt-get update
   sudo apt-get install -y mssql-server-is
   ```

1. Après l’installation d’Integration Services, exécutez **ssis-conf**. Pour plus d’informations, consultez [Configurer SSIS sur Linux avec ssis-conf](sql-server-linux-configure-ssis.md).

   ```bash
   sudo /opt/ssis/bin/ssis-conf setup
   ```

1. Une fois la configuration terminée, définissez la variable d’environnement PATH.

   ```bash
   export PATH=/opt/ssis/bin:$PATH
   ```

::: moniker-end

<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

1. Importez les clés GPG de référentiel public.

   ```bash
   curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
   ```

1. Enregistrez le référentiel SQL Server Ubuntu.

   ```bash
   sudo add-apt-repository "$(curl https://packages.microsoft.com/config/ubuntu/16.04/mssql-server-2019.list)"
   ```

1. Exécutez les commandes suivantes pour installer SQL Server Integration Services.

   ```bash
   sudo apt-get update
   sudo apt-get install -y mssql-server-is
   ```

1. Après l’installation d’Integration Services, exécutez **ssis-conf**. Pour plus d’informations, consultez [Configurer SSIS sur Linux avec ssis-conf](sql-server-linux-configure-ssis.md).

   ```bash
   sudo /opt/ssis/bin/ssis-conf setup
   ```

1. Une fois la configuration terminée, définissez la variable d’environnement PATH.

   ```bash
   export PATH=/opt/ssis/bin:$PATH
   ```

::: moniker-end

### <a name="update-ssis"></a>Mettre à jour SSIS

Si vous avez déjà installé **mssql-server-is**, effectuez un mise à jour vers la dernière version avec la commande suivante :

```bash
sudo apt-get install mssql-server-is
```

### <a name="remove-ssis"></a>Supprimer SSIS

Pour supprimer **mssql-server-is**, exécutez la commande suivante :

```bash
sudo apt-get remove mssql-server-is
```

## <a name="install-ssis-on-rhel"></a><a name="RHEL"></a> Installer SSIS sur RHEL
Pour installer le package **mssql-server-is** sur RHEL, procédez comme suit :

<!--SQL Server 2017 on Linux-->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

1. Téléchargez le fichier config du référentiel SQL Server Red Hat.

   ```bash
   sudo curl -o /etc/yum.repos.d/mssql-server.repo https://packages.microsoft.com/config/rhel/7/mssql-server-2017.repo
   ```

1. Exécutez la commande suivante pour installer SQL Server Integration Services.

   ```bash
   sudo yum install -y mssql-server-is
   ```

1. Après l’installation, exécutez **ssis-conf**. Pour plus d’informations, consultez [Configurer SSIS sur Linux avec ssis-conf](sql-server-linux-configure-ssis.md).

   ```bash
   sudo /opt/ssis/bin/ssis-conf setup
   ```

1. Une fois la configuration terminée, définissez la variable d’environnement PATH.

   ```bash
   export PATH=/opt/ssis/bin:$PATH
   ```

::: moniker-end

<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

1. Téléchargez le fichier config du référentiel SQL Server Red Hat.

   ```bash
   sudo curl -o /etc/yum.repos.d/mssql-server.repo https://packages.microsoft.com/config/rhel/7/mssql-server-2019.repo
   ```

1. Exécutez la commande suivante pour installer SQL Server Integration Services.

   ```bash
   sudo yum install -y mssql-server-is
   ```

1. Après l’installation, exécutez **ssis-conf**. Pour plus d’informations, consultez [Configurer SSIS sur Linux avec ssis-conf](sql-server-linux-configure-ssis.md).

   ```bash
   sudo /opt/ssis/bin/ssis-conf setup
   ```

1. Une fois la configuration terminée, définissez la variable d’environnement PATH.

   ```bash
   export PATH=/opt/ssis/bin:$PATH
   ```

::: moniker-end

### <a name="update-ssis"></a>Mettre à jour SSIS

Si vous avez déjà installé **mssql-server-is**, effectuez une mise à jour vers la dernière version en utilisant la commande suivante :

```bash
sudo yum update mssql-server-is
```

### <a name="remove-ssis"></a>Supprimer SSIS
Pour supprimer **mssql-server-is**, exécutez la commande suivante :

```bash
sudo yum remove mssql-server-is
```

## <a name="unattended-installation"></a>Installation sans assistance

Pour exécuter **ssis-conf setup** en tant qu’installation sans assistance, procédez comme suit :

1. Spécifiez l’option **-n** (aucune invite).
1. Fournissez les valeurs requises en définissant les variables d’environnement.

L’exemple suivant effectue ces actions :

- Installe SSIS
- Spécifie l’édition Développeur en fournissant une valeur pour la variable d’environnement SSIS_PID
- Accepte les termes du contrat de licence logiciel Microsoft en fournissant une valeur pour la variable d’environnement ACCEPT_EULA
- Exécute une installation sans assistance en spécifiant l’option **-n** (aucune invite)

```
sudo SSIS_PID=Developer ACCEPT_EULA=Y /opt/ssis/bin/ssis-conf -n setup 
```

### <a name="environment-variables-for-unattended-installation"></a>Variables d’environnement pour une installation sans assistance

| Variable d’environnement | Description |
|---|---|
| ACCEPT_EULA | Accepte les termes du contrat de licence SQL Server lorsqu’il est défini sur n’importe quelle valeur comme « Y ».|
| SSIS_PID | Définit l’édition SQL Server ou la clé de produit (Product key). Les valeurs possibles sont les suivantes :<ul><li>Évaluation</li><li>Développeur</li><li>Express</li><li>Web</li><li>Standard</li><li>Enterprise</li><li>Une clé de produit (Product Key)</li></ul>Si vous spécifiez une clé de produit, celle-ci doit se présenter sous la forme *#####* - *#####* - *#####* - *#####* - *#####* , où *#* est une lettre ou un nombre.  |
| | |

## <a name="next-steps"></a>Étapes suivantes

Pour exécuter des packages SSIS sur Linux, consultez [Extraire, transformer et charger des données pour SQL Server sur Linux avec SSIS](sql-server-linux-migrate-ssis.md).

Pour configurer des paramètres SSIS supplémentaires sur Linux, consultez [Configurer SQL Server Integration Services sur Linux avec ssis-conf](sql-server-linux-configure-ssis.md).

## <a name="related-content-about-ssis-on-linux"></a>Contenu connexe relatif à SSIS sur Linux

- [Extraire, transformer et charger des données sur Linux avec SSIS](sql-server-linux-migrate-ssis.md)
- [Configurer SQL Server Integration Services sur Linux avec ssis-conf](sql-server-linux-configure-ssis.md)
- [Limitations et problèmes connus pour SSIS sur Linux](sql-server-linux-ssis-known-issues.md)
- [Planifier l’exécution du package SQL Server Integration Services sur Linux avec cron](sql-server-linux-schedule-ssis-packages.md)
