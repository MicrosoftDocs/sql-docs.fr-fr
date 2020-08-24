---
title: Installation sans assistance de SQL Server sur SUSE Linux Enterprise Server
titleSuffix: SQL Server
description: Utilisez un exemple de script Bash pour installer SQL Server 2017 sur SUSE Linux Enterprise Server (SLES) version 12 SP2 sans intervention de l’utilisateur.
author: VanMSFT
ms.author: vanto
ms.date: 11/04/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: 6562d01d615b4aac929b01d1e28985b5adddea42
ms.sourcegitcommit: 3ea082c778f6771b17d90fb597680ed334d3e0ec
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/11/2020
ms.locfileid: "88089009"
---
# <a name="sample-unattended-sql-server-installation-script-for-suse-linux-enterprise-server"></a>Exemple : Script d'installation de SQL Server sans assistance pour SUSE Linux Enterprise Server

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Cet exemple de script Bash installe SQL Server 2017 sur SUSE Linux Enterprise Server (SLES) version 12 SP2 sans intervention de l’utilisateur. Il fournit des exemples d'installation du moteur de base de données, des outils en ligne de commande SQL Server, SQL Server Agent et exécute les étapes de post-installation. Vous pouvez également installer la recherche en texte intégral et créer un utilisateur administratif.

> [!TIP]
> Si vous n'avez pas besoin d'un script d'installation sans assistance, le moyen le plus rapide d'installer SQL Server consiste à suivre le [démarrage rapide pour SLES](quickstart-install-connect-suse.md). Pour d'autres informations d'installation, voir [Conseils d’installation pour SQL Server sur Linux](sql-server-linux-setup.md).

## <a name="prerequisites"></a>Prérequis

- Vous avez besoin d'au moins 2 Go de mémoire pour exécuter SQL Server sur Linux.
- Le système de fichiers doit être **XFS** ou **EXT4**. Les autres systèmes de fichiers, tels que **BTRFS**, ne sont pas pris en charge.
- Pour les autres configurations système requises, voir [Configuration système requise pour SQL Server sur Linux](sql-server-linux-setup.md#system).

> [!IMPORTANT]
> SQL Server 2017 nécessite libsss_nss_nss_idmap0, qui n'est pas fourni par les référentiels SLES par défaut. Vous pouvez l’installer à partir du Kit de développement logiciel (SDK) SLES version 12 SP2.

## <a name="sample-script"></a>Exemple de script

```bash
#!/bin/bash -e

# Use the following variables to control your install:

# Password for the SA user (required)
MSSQL_SA_PASSWORD='<YourStrong!Passw0rd>'

# Product ID of the version of SQL server you're installing
# Must be evaluation, developer, express, web, standard, enterprise, or your 25 digit product key
# Defaults to developer
MSSQL_PID='evaluation'

# Install SQL Server Agent (recommended)
SQL_INSTALL_AGENT='y'

# Install SQL Server Full Text Search (optional)
# SQL_INSTALL_FULLTEXT='y'

# Create an additional user with sysadmin privileges (optional)
# SQL_INSTALL_USER='<Username>'
# SQL_INSTALL_USER_PASSWORD='<YourStrong!Passw0rd>'

if [ -z $MSSQL_SA_PASSWORD ]
then
  echo Environment variable MSSQL_SA_PASSWORD must be set for unattended install
  exit 1
fi

echo Adding Microsoft repositories...
sudo zypper addrepo -fc https://packages.microsoft.com/config/sles/12/mssql-server-2017.repo
sudo zypper addrepo -fc https://packages.microsoft.com/config/sles/12/prod.repo 
sudo zypper --gpg-auto-import-keys refresh

#Add the SLES v12 SP2 SDK to obtain libsss_nss_idmap0
sudo SUSEConnect -p sle-sdk/12.2/x86_64

echo Installing SQL Server...
sudo zypper install -y mssql-server

echo Running mssql-conf setup...
sudo MSSQL_SA_PASSWORD=$MSSQL_SA_PASSWORD \
     MSSQL_PID=$MSSQL_PID \
     /opt/mssql/bin/mssql-conf -n setup accept-eula

echo Installing mssql-tools and unixODBC developer...
sudo ACCEPT_EULA=Y zypper install -y mssql-tools unixODBC-devel

# Add SQL Server tools to the path by default:
echo Adding SQL Server tools to your path...
echo PATH="$PATH:/opt/mssql-tools/bin" >> ~/.bash_profile
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc

# Optional SQL Server Agent installation:
if [ ! -z $SQL_INSTALL_AGENT ]
then
  echo Installing SQL Server Agent...
  sudo zypper install -y mssql-server-agent
fi

# Optional SQL Server Full Text Search installation:
if [ ! -z $SQL_INSTALL_FULLTEXT ]
then
    echo Installing SQL Server Full-Text Search...
    sudo zypper install -y mssql-server-fts
fi

# Configure firewall to allow TCP port 1433:
echo Configuring SuSEfirewall2 to allow traffic on port 1433...
sudo SuSEfirewall2 open INT TCP 1433
sudo SuSEfirewall2 stop
sudo SuSEfirewall2 start

# Example of setting post-installation configuration options
# Set trace flags 1204 and 1222 for deadlock tracing:
# echo Setting trace flags...
# sudo /opt/mssql/bin/mssql-conf traceflag 1204 1222 on

# Restart SQL Server after making configuration changes:
echo Restarting SQL Server...
sudo systemctl restart mssql-server

# Connect to server and get the version:
counter=1
errstatus=1
while [ $counter -le 5 ] && [ $errstatus = 1 ]
do
  echo Waiting for SQL Server to start...
  sleep 5s
  /opt/mssql-tools/bin/sqlcmd \
    -S localhost \
    -U SA \
    -P $MSSQL_SA_PASSWORD \
    -Q "SELECT @@VERSION" 2>/dev/null
  errstatus=$?
  ((counter++))
done

# Display error if connection failed:
if [ $errstatus = 1 ]
then
  echo Cannot connect to SQL Server, installation aborted
  exit $errstatus
fi

# Optional new user creation:
if [ ! -z $SQL_INSTALL_USER ] && [ ! -z $SQL_INSTALL_USER_PASSWORD ]
then
  echo Creating user $SQL_INSTALL_USER
  /opt/mssql-tools/bin/sqlcmd \
    -S localhost \
    -U SA \
    -P $MSSQL_SA_PASSWORD \
    -Q "CREATE LOGIN [$SQL_INSTALL_USER] WITH PASSWORD=N'$SQL_INSTALL_USER_PASSWORD', DEFAULT_DATABASE=[master], CHECK_EXPIRATION=ON, CHECK_POLICY=ON; ALTER SERVER ROLE [sysadmin] ADD MEMBER [$SQL_INSTALL_USER]"
fi

echo Done!
```

### <a name="running-the-script"></a>Exécution du script

Pour exécuter le script

1. Collez l'exemple dans votre éditeur de texte préféré, puis enregistrez-le sous un nom facile à retenir, comme `install_sql.sh`.

1. Personnalisez `MSSQL_SA_PASSWORD`, `MSSQL_PID`, et toutes les autres variables que vous souhaitez modifier.

1. Marquer le script comme exécutable

   ```bash
   chmod +x install_sql.sh
   ```

1. Exécuter le script

   ```bash
   ./install_sql.sh
   ```

### <a name="understanding-the-script"></a>Fonctionnement du script
La première tâche du script Bash est de définir quelques variables. Il peut s'agir soit de variables de script, comme l’exemple, soit de variables d'environnement. La variable `MSSQL_SA_PASSWORD` est **requise** par l'installation de SQL Server, les autres sont des variables personnalisées créées pour le script. L’exemple de script effectue les étapes suivantes :

1. Importer les clés publiques Microsoft GPG.

1. Enregistrer les référentiels Microsoft pour SQL Server et les outils en ligne de commande.

1. Mettre à jour les référentiels locaux

1. Installer SQL Server

1. Configurez SQL Server avec ```MSSQL_SA_PASSWORD``` et acceptez automatiquement le Contrat de licence de l'utilisateur final.

1. Acceptez automatiquement le Contrat de licence de l'utilisateur final pour les outils en ligne de commande SQL Server, installez ces outils, puis installez le package unixodbc-dev.

1. Ajoutez les outils en ligne de commande SQL Server au chemin d'accès pour en faciliter l'utilisation.

1. Installez SQL Server Agent si la variable de script ```SQL_INSTALL_AGENT``` est activée par défaut.

1. Si la variable ```SQL_INSTALL_FULLTEXT``` est définie, vous pouvez installer la recherche en texte intégral SQL Server.

1. Débloquez le port 1433 pour TCP sur le pare-feu système pour permettre la connexion à SQL Server depuis un autre système.

1. Vous pouvez aussi définir des indicateurs de trace pour le traçage de l'interblocage. (Nécessite de supprimer les marques de commentaire)

1. SQL Server est maintenant installé. Pour le rendre opérationnel, redémarrez le processus.

1. Vérifiez que SQL Server est installé correctement, tout en masquant les messages d'erreur.

1. Créez un utilisateur administrateur du serveur si les valeurs ```SQL_INSTALL_USER``` et ```SQL_INSTALL_USER_PASSWORD``` sont toutes deux définies.

## <a name="next-steps"></a>Étapes suivantes

Simplifiez plusieurs installations sans assistance et créez un script Bash autonome qui définit les variables d'environnement appropriées. Vous pouvez supprimer n'importe laquelle des variables utilisées dans l'exemple de script et les placer dans leur propre script Bash.

```bash
#!/bin/bash
export MSSQL_SA_PASSWORD='<YourStrong!Passw0rd>'
export MSSQL_PID='evaluation'
export SQL_INSTALL_AGENT='y'
export SQL_INSTALL_USER='<Username>'
export SQL_INSTALL_USER_PASSWORD='<YourStrong!Passw0rd>'
export SQL_INSTALL_AGENT='y'
```

Lancez ensuite le script Bash comme suit :
```bash
. ./my_script_name.sh
```

Pour plus d’informations sur SQL Server sur Linux, consultez [Vue d’ensemble de SQL Server sur Linux](sql-server-linux-overview.md).
