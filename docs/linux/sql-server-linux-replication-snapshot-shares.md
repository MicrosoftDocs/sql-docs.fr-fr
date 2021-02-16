---
title: Configurer les partages de dossiers de captures instantanées
titleSuffix: SQL Server on Linux
description: Apprenez à configurer les partages de dossiers de captures instantanées (réplication SQL Server sur Linux).
ms.custom: seo-lt-2019
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 09/24/2018
ms.topic: article
ms.prod: sql
ms.technology: linux
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15'
ms.openlocfilehash: fdd43008abb9ba49ab3a5e3a2859a6366a3a6542
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346472"
---
# <a name="configure-replication-snapshot-folder-with-shares"></a>Configurer un dossier de captures instantanées de réplication avec des partages

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Le dossier de captures instantanées correspond à un répertoire que vous définissez sous la forme d'un partage ; les agents qui lisent et écrivent dans le dossier doivent disposer des autorisations suffisantes pour pouvoir y accéder.

![diagramme de réplication][1]

### <a name="replication-snapshot-folder-share-explained"></a>Partage de dossiers de captures instantanées de réplication expliqué

Avant les exemples, voyons comment SQL Server utilise des partages Samba pour la réplication. Vous trouverez ci-dessous un exemple de base de ce fonctionnement.

1. Les partages Samba sont configurés de façon à ce que les fichiers écrits sur `/local/path1` par les agents de réplication sur le serveur de publication puissent être vus par l’abonné
2. SQL Server est configuré pour utiliser des chemins d’accès de partage lors de la configuration du serveur publication sur le serveur de distribution, de sorte que toutes les instances examinent le `//share/path`
3. SQL Server recherche le chemin d’accès local du `//share/path` pour savoir où rechercher les fichiers
4. SQL Server lit/écrit dans les chemins d’accès locaux sauvegardés par un partage Samba


## <a name="configure-a-samba-share-for-the-snapshot-folder"></a>Configurer un partage Samba pour le dossier de captures instantanées 

Les agents de réplication auront besoin d’un répertoire partagé entre les hôtes de réplication pour accéder aux dossiers de captures instantanées sur d’autres machines. Par exemple, dans la réplication par réception transactionnelle, l’agent de distribution réside sur l’abonné et nécessite l’accès au serveur de distribution pour obtenir des articles. Dans cette section, nous allons voir un exemple de configuration d’un partage Samba sur deux hôtes de réplication.


## <a name="steps"></a>Étapes

Par exemple, nous allons configurer un dossier de captures instantanées sur l’hôte 1 (le serveur de distribution) à partager avec l’hôte 2 (l’abonné) à l’aide de Samba. 

### <a name="install-and-start-samba-on-both-machines"></a>Installer et démarrer Samba sur les deux machines 

Sur Ubuntu :

```bash
sudo apt-get -y install samba
sudo service smbd restart
```

Sur RHEL :

```bash
sudo yum install samba
sudo service smb start
sudo service smb status
```

### <a name="on-host-1-distributor-set-up-the-samba-share"></a>Sur l’hôte 1 (serveur de distribution), configurez le partage Samba 

1. Configurer un utilisateur et un mot de passe pour Samba :

  ```bash
  sudo smbpasswd -a mssql 
  ```

1. Modifiez `/etc/samba/smb.conf` pour inclure l’entrée suivante et renseigner les champs *share_name* et *chemin d'accès*
 ```bash
  <[share_name]>
  path = </local/path/on/host/1>
  writable = yes
  create mask = 770
  directory mask 
  valid users = mssql 
  ```

  **Exemple**

  ```bash
  [mssql_data]    <- Name of the shared directory
  path = /var/opt/mssql/repldata <- location of directory we wish to share
  writable = yes  <- determines if the share is writable from other hosts
  create mask = 770  <- Linux permissions for files created 
  directory mask = 770 <- Linux permissions for directories created
  valid users = mssql   <- list of users who can login to this share
  ```

### <a name="on-host-2-subscriber--mount-the-samba-share"></a>Sur l’hôte 2 (abonné), montez le partage Samba

Modifiez la commande avec les chemins d’accès corrects et exécutez la commande suivante sur machine2 :

  ```bash
  sudo mount //<name_of_host_1>/<share_name> </local/path/on/host/2> -o user=mssql,uid=mssql,gid=mssql
  ```

  **Exemple**

  ```bash
  mount //host1/mssql_data /var/opt/mssql/repldata_shared -o user=mssql,uid=mssql,gid=mssql

  user=mssql <- sets the login name for samba
  uid=mssql   <- makes the mssql user as the owner of the mounted directory
  gid=mssql   <- sets the mssql group as the owner of the mounted directory
  ```

### <a name="on-both-hosts--configure-sql-server-on-linux-instances-to-use-snapshot-share"></a>Sur les deux hôtes, configurez les instances sur Linux pour utiliser le partage de fichiers de captures instantanées

Ajoutez la section suivante à `mssql.conf` sur les deux machines. Utiliser où que se trouve le partage Samba pour le //Share/Path. Dans cet exemple, il s’agit de `//host1/mssql_data`

  ```bash
  [uncmapping]
  //share/path = /local/path/on/hosts/
  ```

  **Exemple**

  Sur hôte1 :

  ```bash
  [uncmapping]
  //host1/mssql_data = /local/path/on/hosts/1
  ```

  Sur hôte2 :
  
  ```bash
  [uncmapping]
  //host1/mssql_data = /local/path/on/hosts/2
  ```

### <a name="configuring-publisher-with-shared-paths"></a>Configuration du serveur de publication avec des chemins d’accès partagés

* Lors de la configuration de la réplication, utilisez le chemin d’accès de partage (exemple `//host1/mssql_data`
* Mappez `//host1/mssql_data` à un répertoire local et au mappage ajouté à `mssql.conf`.

## <a name="next-steps"></a>Étapes suivantes

[Concepts : Réplication SQL Server sur Linux](sql-server-linux-replication.md)

[Procédures stockées de réplication](../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md).

[1]: ./media/sql-server-linux-replication-snapshot-shares/image1.png
