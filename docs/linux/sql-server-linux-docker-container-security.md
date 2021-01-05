---
title: Sécuriser des conteneurs Docker SQL Server
description: Découvrir les différentes façons de sécuriser des conteneurs Docker SQL Server et comment exécuter des conteneurs en tant qu’utilisateur non-racine différent sur l’hôte
author: vin-yu
ms.author: vinsonyu
ms.reviewer: vanto
ms.custom: contperf-fy21q1
ms.date: 09/07/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
moniker: '>= sql-server-linux-2017 || >= sql-server-2017 '
ms.openlocfilehash: 88c179b1a17dd84dc33c2ae77979507e82b7a55a
ms.sourcegitcommit: cb8e2ce950d8199470ff1259c9430f0560f0dc1d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/05/2021
ms.locfileid: "97878751"
---
# <a name="secure-sql-server-docker-containers"></a>Sécuriser des conteneurs Docker SQL Server

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Les conteneurs SQL Server 2017 démarrent en tant qu’utilisateur racine par défaut, ce qui peut poser des problèmes de sécurité. Cet article traite des options de sécurité à votre disposition lors de l’exécution de conteneurs Docker SQL Server, puis explique comment créer un conteneur SQL Server en tant qu’utilisateur non-racine.

## <a name="build-and-run-non-root-sql-server-2017-containers"></a><a id="buildnonrootcontainer"></a> Générer et exécuter des conteneurs SQL Server 2017 non-root

Suivez les étapes ci-dessous pour générer un conteneur SQL Server 2017 qui démarre en tant qu’utilisateur `mssql` (non-root).

> [!NOTE]
> Les conteneurs SQL Server 2019 ne démarrant pas automatiquement en tant que root, les étapes suivantes s’appliquent uniquement aux conteneurs SQL Server 2017 qui démarrent en tant que root par défaut.

1. Téléchargez l’[exemple de fichier Dockerfile pour le conteneur SQL Server non racine](https://raw.githubusercontent.com/microsoft/mssql-docker/master/linux/preview/examples/mssql-server-linux-non-root/Dockerfile) et enregistrez-le comme `dockerfile`.

2. Exécutez la commande suivante dans le contexte du répertoire de fichier Dockerfile pour générer le conteneur SQL Server non racine :

    ```bash
    cd <path to dockerfile>
    docker build -t 2017-latest-non-root .
    ```

3. Démarrez le conteneur.

    ```bash
    docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=MyStrongPassword@" --cap-add SYS_PTRACE --name sql1 -p 1433:1433 -d 2017-latest-non-root
    ```

    > [!NOTE]
    > L’indicateur `--cap-add SYS_PTRACE` est requis pour les conteneurs SQL Server non racine afin de générer des vidages à des fins de dépannage.

4. Vérifiez que le conteneur s’exécute en tant qu’utilisateur non racine :

    ```bash
    docker exec -it sql1 bash
    ```

    Exécutez `whoami`, ce qui retourne l’utilisateur qui s’exécute dans le conteneur.
    
    ```bash
    whoami
    ```

## <a name="run-container-as-a-different-non-root-user-on-the-host"></a><a id="nonrootuser"></a> Exécuter le conteneur comme un autre utilisateur non racine sur l’hôte

Pour exécuter le conteneur SQL Server en tant qu’autre utilisateur non racine, ajoutez l’indicateur -u à la commande docker run. Le conteneur non-racine a comme restriction qu’il doit s’exécuter dans le cadre du groupe racine, sauf si un volume est monté sur `/var/opt/mssql` qui est accessible à l’utilisateur non-racine. Le groupe racine n’accorde pas d’autorisations racine supplémentaires à l’utilisateur non racine.

#### <a name="run-as-a-user-with-a-uid-4000"></a>Exécuter en tant qu’utilisateur avec un UID 4000

Vous pouvez démarrer SQL Server avec un UID personnalisé. Par exemple, la commande ci-dessous démarre SQL Server avec un UID 4000 :

```bash
docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=MyStrongPassword" --cap-add SYS_PTRACE -u 4000:0 -p 1433:1433 -d mcr.microsoft.com/mssql/server:2019-latest
```

> [!Warning]
> Vérifiez que le conteneur SQL Server a un utilisateur nommé, tel que « mssql » ou « racine », ou SQLCMD ne pourra pas s’exécuter dans le conteneur. Vous pouvez vérifier si le conteneur SQL Server s’exécute en tant qu’utilisateur nommé en exécutant `whoami` dans le conteneur.

#### <a name="run-the-non-root-container-as-the-root-user"></a>Exécuter le conteneur non racine en tant qu’utilisateur racine

Vous pouvez exécuter le conteneur non-racine en tant qu’utilisateur racine, si nécessaire. Cela permet également d’octroyer automatiquement toutes les autorisations de fichiers au conteneur, car il s’agit d’un privilège plus élevé.

```bash
docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=MyStrongPassword" -u 0:0 -p 1433:1433 -d mcr.microsoft.com/mssql/server:2019-latest
```

#### <a name="run-as-a-user-on-your-host-machine"></a>Exécuter en tant qu’utilisateur sur votre ordinateur hôte

Vous pouvez démarrer SQL Server avec un utilisateur existant sur l’ordinateur hôte à l’aide de la commande suivante :
```bash
docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=MyStrongPassword" --cap-add SYS_PTRACE -u $(id -u myusername):0 -p 1433:1433 -d mcr.microsoft.com/mssql/server:2019-latest
```

#### <a name="run-as-a-different-user-and-group"></a>Exécuter en tant qu’utilisateur ou groupe différent

Vous pouvez démarrer SQL Server avec un utilisateur ou groupe personnalisé. Dans cet exemple, le volume monté dispose des autorisations configurées pour l’utilisateur ou le groupe sur l’ordinateur hôte.

```bash
docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=MyStrongPassword" --cap-add SYS_PTRACE -u (id -u myusername):(id -g myusername) -v /path/to/mssql:/var/opt/mssql -p 1433:1433 -d mcr.microsoft.com/mssql/server:2019-latest
```

## <a name="configure-persistent-storage-permissions-for-non-root-containers"></a><a id="storagepermissions"></a> Configurer des autorisations de stockage persistant pour les conteneurs non racine

Pour permettre à l’utilisateur non-racine d’accéder aux fichiers de base de fichiers qui se trouvent sur des volumes montés, vérifiez que l’utilisateur ou le groupe sous lequel vous exécutez le conteneur peut accéder en lecture/écriture au stockage de fichiers persistant.  

Vous pouvez obtenir la propriété actuelle des fichiers de base de données à l’aide de cette commande.

```bash
ls -ll <database file dir>
```

Exécutez l’une des commandes suivantes si SQL Server n’a pas accès aux fichiers de base de données conservés.

#### <a name="grant-the-root-group-rw-access-to-the-db-files"></a>Accorder au groupe racine l’accès en lecture/écriture aux fichiers de base de données

Accordez au groupe racine des autorisations sur les répertoires suivants afin que le conteneur SQL Server non racine ait accès aux fichiers de base de données.

```bash
chgrp -R 0 <database file dir>
chmod -R g=u <database file dir>
```

#### <a name="set-the-non-root-user-as-the-owner-of-the-files"></a>Définir l’utilisateur non-racine comme propriétaire des fichiers

Il peut s’agir de l’utilisateur non racine par défaut ou de tout autre utilisateur non racine que vous souhaitez spécifier. Dans cet exemple, nous définissons UID 10001 en tant qu’utilisateur non racine.

```bash
chown -R 10001:0 <database file dir>
```
## <a name="encrypting-connections-to-sql-server-linux-containers"></a>Chiffrement des connexions à des conteneurs Linux SQL Server

Pour chiffrer les connexions aux conteneurs Linux SQL Server, vous avez besoin d’un certificat dont les spécifications sont documentées [ici].

Vous trouverez ci-dessous un exemple de chiffrement de la connexion à des conteneurs Linux SQL Server. Nous utilisons ici un certificat auto-signé, ce qu’il ne faut pas faire dans des scénarios de production pour de tels environnements : vous devez utiliser pour ceux-ci des certificats d’autorité de certification.

1. Créez un certificat auto-signé, qui convient seulement aux environnements de test et hors production.
  
      ```bash
      openssl req -x509 -nodes -newkey rsa:2048 -subj '/CN=sql1.contoso.com' -keyout /container/sql1/mssql.key -out /container/sql1/mssql.pem -days 365
      ```
     Où sql1 est le nom d’hôte du conteneur SQL : lors de la connexion à ce conteneur, le nom utilisé dans la chaîne de connexion sera donc \'sql1.contoso.com,port\'.

    > [!NOTE]
    > Vérifiez que le chemin du dossier /container/sql1/ existe déjà avant d’exécuter la commande ci-dessus.

2. Veillez à définir les autorisations appropriées sur les fichiers mssql.key et mssql.pem, afin d’éviter des erreurs lors du montage des fichiers sur le conteneur SQL :

    ```bash
    chmod 440 /container/sql1/mssql.pem
    chmod 440 /container/sql1/mssql.key
    ```

3. Créez maintenant un fichier mssql.conf avec le contenu ci-dessous pour activer le chiffrement lancé par le serveur. Pour le chiffrement lancé par le client, remplacez la dernière ligne par 'forceencryption = 0\'.

    ```bash
    [network]
    tlscert = /etc/ssl/certs/mssql.pem
    tlskey = /etc/ssl/private/mssql.key
    tlsprotocols = 1.2
    forceencryption = 1
    ```

    > [!NOTE]
    > Pour certaines distributions Linux, le chemin pour le stockage du certificat et de la clé peut également être : /etc/pki/tls/certs/ et /etc/pki/tls/private/, respectivement. Vérifiez le chemin avant de mettre à jour mssql.conf pour les conteneurs SQL. L’emplacement que vous définissez dans mssql.conf sera l’emplacement où l’instance SQL Server dans le conteneur va rechercher le certificat et sa clé. Dans le cas présent, cet emplacement est /etc/ssl/certs/ et /etc/ssl/private/.

    Le fichier mssql.conf est également créé sous le même emplacement de dossier /container/sql1/. Après avoir effectué les étapes ci-dessus, vous devez avoir trois fichiers : mssql.conf, mssql.key et mssql.pem dans le dossier sql1.

4. Déployez le conteneur SQL avec la commande illustrée ci-dessous :

    ```bash
    docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=P@ssw0rd" -p 5434:1433 --name sql1 -h sql1 -v /container/sql1/mssql.conf:/var/opt/mssql/mssql.conf -v   /container/sql1/mssql.pem:/etc/ssl/certs/mssql.pem -v /container/sql1/mssql.key:/etc/ssl/private/mssql.key -d mcr.microsoft.com/mssql/server:2019-latest
    ```

    Dans la commande ci-dessus, nous avons monté les fichiers mssql.conf, mssql.pem et mssql.key dans le conteneur et mappé le port 1433 (le port par défaut de SQL Server) dans le conteneur sur le port 5434 de l’hôte. 

    > [!NOTE]
    > Si vous utilisez RHEL 8 et ultérieur, vous pouvez également utiliser la commande \'podman run\' au lieu de \'docker run\'. 

Suivez les sections \"Inscrire le certificat sur votre machine cliente\" et \"Exemples de chaînes de connexion\" documentées [ici][1] pour démarrer le chiffrement des connexions à SQL Server sur des conteneurs Linux.

  [Encrypting connection to SQL Server Linux]: https://docs.microsoft.com/sql/linux/sql-server-linux-encrypted-connections?view=sql-server-ver15&preserve-view=true
  [ici]: https://docs.microsoft.com/sql/linux/sql-server-linux-encrypted-connections?view=sql-server-ver15&preserve-view=true#requirements-for-certificates
  [1]: https://docs.microsoft.com/sql/linux/sql-server-linux-encrypted-connections?view=sql-server-ver15&preserve-view=true#client-initiated-encryption

## <a name="next-steps"></a>Étapes suivantes

<!--SQL Server 2017 on Linux -->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

- Bien démarrer avec les images de conteneur SQL Server 2017 sur Docker en suivant le [démarrage rapide](quickstart-install-connect-docker.md?view=sql-server-2017&preserve-view=true)

::: moniker-end

<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

- Bien démarrer avec les images de conteneur SQL Server 2019 sur Docker en suivant le [démarrage rapide](quickstart-install-connect-docker.md)

::: moniker-end

- [Déployer des conteneurs Docker SQL Server et s’y connecter](sql-server-linux-docker-container-deployment.md)

- [Référencer une autre configuration et une personnalisation pour des conteneurs Docker](sql-server-linux-docker-container-configure.md)

- [Résolution des problèmes liés aux conteneurs Docker SQL Server](sql-server-linux-docker-container-troubleshooting.md)
