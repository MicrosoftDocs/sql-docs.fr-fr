---
title: Configurer la collecte des données d’utilisation et de diagnostic pour SQL Server sur Linux
description: Décrit comment les données de diagnostic et d’utilisation des clients SQL Server sont collectées et configurées sur Linux.
ms.custom: seo-lt-2019
author: VanMSFT
ms.author: vanto
ms.date: 11/04/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: 31f391360636bb27bf346ee76614c409738347e2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347324"
---
# <a name="configure-usage--diagnostic-data-collection-for-sql-server-on-linux"></a>Configurer la collecte des données d’utilisation et de diagnostic pour SQL Server sur Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Par défaut, Microsoft SQL Server collecte des informations sur la façon dont ses clients utilisent l’application. Plus précisément, SQL Server recueille des données sur l’expérience d’installation, l’utilisation et les performances. Elles aident Microsoft à améliorer le produit pour mieux répondre aux besoins des clients. Par exemple, Microsoft collecte des informations sur les types de codes d’erreur rencontrés par les utilisateurs afin que nous puissions corriger les bogues associés, améliorer notre documentation sur l’utilisation de SQL Server et déterminer s’il faudrait ajouter des fonctionnalités au produit pour mieux servir des clients.

Ce document fournit des informations sur les types d’informations collectées et sur la configuration de Microsoft SQL Server sur Linux pour envoyer les informations collectées à Microsoft. SQL Server 2017 inclut une déclaration de confidentialité qui explique quelles sont les informations que nous collectons ou ne collectons pas auprès des utilisateurs. Pour plus d’informations, consultez la [déclaration de confidentialité](../sql-server/sql-server-privacy.md).

En particulier, Microsoft n’envoie par ce mécanisme aucune information de ces types :

- valeurs des tables utilisateur ;
- identifiants d’ouverture de session ou autres informations d’authentification ;
- Informations d’identification personnelle (PII)

SQL Server 2017 collecte et envoie toujours des informations sur l’expérience d’installation à partir du processus d’installation afin de nous permettre de trouver et de résoudre rapidement les problèmes d’installation que rencontre le client. SQL Server 2017 peut être configuré de façon à ne pas envoyer d’informations (pour une instance de serveur donnée) à Microsoft par le biais de **mssql-conf**. mssql-conf est un script de configuration qui s’installe avec SQL Server 2017 pour Red Hat Enterprise Linux, SUSE Linux Enterprise Server et Ubuntu.

> [!NOTE]
> Vous ne pouvez désactiver l’envoi d’informations à Microsoft que dans les versions payantes de SQL Server.

## <a name="disable-usage-and-diagnostic-data-collection"></a>Désactiver la collecte des données d’utilisation et de diagnostic

Cette option vous permet d’indiquer si SQL Server envoie une collecte de données d’utilisation et de diagnostic à Microsoft. La valeur par défaut est true. Pour modifier la valeur, exécutez les commandes suivantes :

> [!IMPORTANT]
> Vous ne pouvez pas désactiver la collecte des données d’utilisation et de diagnostic pour les éditions gratuites de SQL Server, Express et Développeur.

### <a name="on-red-hat-suse-and-ubuntu"></a>Sur Red Hat, SUSE et Ubuntu

1. Exécutez le script mssql-conf en tant que root avec la commande **set** pour **telemetry.customerfeedback**. L’exemple suivant désactive la collecte des données d’utilisation et de diagnostic en spécifiant **false**.

   ```bash
   sudo /opt/mssql/bin/mssql-conf set telemetry.customerfeedback false
   ```

1. Redémarrez le service SQL Server :

   ```bash
   sudo systemctl restart mssql-server
   ```
   
### <a name="on-docker"></a>Sur Docker
Pour désactiver la collecte des données d’utilisation et de diagnostic sur Docker, vous devez demander à Docker de [rendre vos données persistantes](./sql-server-linux-docker-container-deployment.md). 

<!--SQL Server 2017 on Linux -->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

1. Ajoutez un fichier `mssql.conf` contenant les lignes `[telemetry]` et `customerfeedback = false` dans le répertoire de l’hôte :
 
   ```bash
   echo '[telemetry]' >> <host directory>/mssql.conf
   ```

   ```bash
   echo 'customerfeedback = false' >> <host directory>/mssql.conf
   ```

2. Exécuter l’image conteneur

   ```bash
   docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -p 1433:1433 -v <host directory>:/var/opt/mssql -d mcr.microsoft.com/mssql/server:2017-latest
   ```

   ```PowerShell
   docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1433:1433 -v <host directory>:/var/opt/mssql -d mcr.microsoft.com/mssql/server:2017-latest
   ```

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

1. Ajoutez un fichier `mssql.conf` contenant les lignes `[telemetry]` et `customerfeedback = false` dans le répertoire de l’hôte :

   ```bash
   echo '[telemetry]' >> <host directory>/mssql.conf
   ```

   ```bash
   echo 'customerfeedback = false' >> <host directory>/mssql.conf
   ```

2. Exécuter l’image conteneur

   ```bash
   docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -p 1433:1433 -v <host directory>:/var/opt/mssql -d mcr.microsoft.com/mssql/server:2019-GA-ubuntu-16.04
   ```

   ```PowerShell
   docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1433:1433 -v <host directory>:/var/opt/mssql -d mcr.microsoft.com/mssql/server:2019-GA-ubuntu-16.04
   ```

::: moniker-end

## <a name="local-audit-for-sql-server-on-linux-usage-and-diagnostic-data-collection"></a>Audit local pour la collecte des données d’utilisation et de diagnostic SQL Server sur Linux

Microsoft SQL Server 2017 contient des fonctionnalités Internet susceptibles de collecter et d’envoyer des informations sur votre ordinateur ou appareil (« informations standard sur l’ordinateur ») à Microsoft. Le composant d’audit local de la collecte de données d’utilisation et de diagnostic de SQL Server peut écrire les données collectées par le service vers un dossier désigné, représentant les données (journaux) qui peuvent être envoyées à Microsoft. L’objectif de l’audit local est d’autoriser les clients à visualiser toutes les données collectées par Microsoft avec cette fonctionnalité, pour des raisons de conformité, de réglementation ou de validation de la confidentialité.

Dans SQL Server sur Linux, l’audit local est configurable au niveau de l’instance pour le Moteur de base de données SQL Server. Les autres composants et outils SQL Server ne disposent pas d’une fonctionnalité d’audit local pour la collecte des données d’utilisation et de diagnostic.

### <a name="enable-local-audit"></a>Activer l’audit local

Cette option active l’audit local et vous permet de définir le répertoire dans lequel les journaux d’audit locaux sont créés.

1. Créez un répertoire cible pour les nouveaux journaux d’audit local. L’exemple suivant crée un répertoire **/tmp/audit** :

   ```bash
   sudo mkdir /tmp/audit
   ```

2. Remplacez le propriétaire et le groupe du répertoire par l’utilisateur **mssql** :

   ```bash
   sudo chown mssql /tmp/audit
   sudo chgrp mssql /tmp/audit
   ```

3. Exécutez le script mssql-conf en tant que root avec la commande **set** pour **telemetry.userrequestedlocalauditdirectory** :

   ```bash
   sudo /opt/mssql/bin/mssql-conf set telemetry.userrequestedlocalauditdirectory /tmp/audit
   ```

4. Redémarrez le service SQL Server :

   ```bash
   sudo systemctl restart mssql-server
   ```
   
### <a name="on-docker"></a>Sur Docker
Pour activer l’audit local sur Docker, vous devez faire en sorte que Docker [rende vos données persistantes](./sql-server-linux-docker-container-deployment.md). 

<!--SQL Server 2017 on Linux -->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

1. Le répertoire cible pour les nouveaux journaux d’audit local est situé dans le conteneur. Créez un répertoire cible pour les nouveaux journaux d’audit local dans le répertoire hôte sur votre ordinateur. L’exemple suivant crée un répertoire **/audit** :

   ```bash
   sudo mkdir <host directory>/audit
   ```

1. Ajoutez un fichier `mssql.conf` contenant les lignes `[telemetry]` et `userrequestedlocalauditdirectory = <host directory>/audit` dans le répertoire de l’hôte :
 
   ```bash
   echo '[telemetry]' >> <host directory>/mssql.conf
   ```

   ```bash
   echo 'userrequestedlocalauditdirectory = <host directory>/audit' >> <host directory>/mssql.conf
   ```

1. Exécuter l’image conteneur

   ```bash
   docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -p 1433:1433 -v <host directory>:/var/opt/mssql -d mcr.microsoft.com/mssql/server:2017-latest
   ```

   ```PowerShell
   docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1433:1433 -v <host directory>:/var/opt/mssql -d mcr.microsoft.com/mssql/server:2017-latest
   ```

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

1. Le répertoire cible pour les nouveaux journaux d’audit local est situé dans le conteneur. Créez un répertoire cible pour les nouveaux journaux d’audit local dans le répertoire hôte sur votre ordinateur. L’exemple suivant crée un répertoire **/audit** :

   ```bash
   sudo mkdir <host directory>/audit
   ```

1. Ajoutez un fichier `mssql.conf` contenant les lignes `[telemetry]` et `userrequestedlocalauditdirectory = <host directory>/audit` dans le répertoire de l’hôte :
 
   ```bash
   echo '[telemetry]' >> <host directory>/mssql.conf
   ```

   ```bash
   echo 'userrequestedlocalauditdirectory = <host directory>/audit' >> <host directory>/mssql.conf
   ```

1. Exécuter l’image conteneur

   ```bash
   docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -p 1433:1433 -v <host directory>:/var/opt/mssql -d mcr.microsoft.com/mssql/server:2019-GA-ubuntu-16.04
   ```

   ```PowerShell
   docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1433:1433 -v <host directory>:/var/opt/mssql -d mcr.microsoft.com/mssql/server:2019-GA-ubuntu-16.04
   ```

::: moniker-end

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur SQL Server sur Linux, consultez [Vue d’ensemble de SQL Server sur Linux](sql-server-linux-overview.md).