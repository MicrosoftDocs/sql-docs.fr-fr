---
title: Configurer l’authentification Active Directory avec des conteneurs basés sur SQL Server sur Linux à l’aide d’adutil
description: Guide étape par étape pour la configuration de l’authentification Active Directory avec des conteneurs SQL Server sur Linux à l’aide d’adutil
author: amvin87
ms.author: amitkh
ms.reviewer: vanto
ms.date: 12/10/2020
ms.topic: tutorial
ms.prod: sql
ms.technology: linux
moniker: '>= sql-server-linux-2017 || >= sql-server-2017 || =sqlallproducts-allversions'
ms.openlocfilehash: 61f03a3d1175e5d38952c3e9e578a025c474188a
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/12/2021
ms.locfileid: "100273110"
---
# <a name="tutorial-configure-active-directory-authentication-with-sql-server-on-linux--containers"></a>Tutoriel : Configurer l’authentification Active Directory avec des conteneurs SQL Server sur Linux

> [!NOTE]
> **adutil** est actuellement en **préversion publique**.

Ce tutoriel explique comment configurer des conteneurs SQL Server sur Linux pour prendre en charge l’authentification Active Directory (AD), également appelée authentification intégrée. Pour une vue d’ensemble, consultez [Authentification Active Directory avec SQL Server sur Linux](sql-server-linux-active-directory-auth-overview.md).

Ce didacticiel contient les tâches suivantes :

> [!div class="checklist"]
> - Installer adutil-preview
> - Joindre l’hôte Linux au domaine AD
> - Créer un utilisateur AD pour SQL Server et définir ServicePrincipalName (SPN) à l’aide de l’outil adutil
> - Créer le fichier keytab du service SQL Server
> - Créer les fichiers mssql.conf et krb5.conf que le conteneur SQL Server utilisera
> - Monter les fichiers de configuration et déployer le conteneur SQL Server
> - Créer des connexions SQL Server basées sur AD à l’aide de Transact-SQL
> - Se connecter à SQL Server à l’aide de l’authentification AD

## <a name="prerequisites"></a>Prérequis

Les éléments suivants sont nécessaires avant de configurer l’authentification AD :

- Disposez d’un contrôleur de domaine AD (Windows) dans votre réseau.
- Installez l’outil adutil-preview sur un ordinateur hôte Linux joint à un domaine. Suivez les instructions de la section [Installer adutil-preview](#install-adutil-preview) ci-dessous, selon la distribution Linux que vous exécutez, pour installer l’outil adutil-preview.

## <a name="container-deployment-and-preparation"></a>Déploiement et préparation du conteneur

Pour configurer votre conteneur, vous devez savoir à l’avance quel port sera utilisé par le conteneur sur l’hôte. Le port par défaut, 1433, peut être mappé différemment sur votre hôte de conteneur. Pour ce tutoriel, le port 5433 sur l’hôte sera mappé au port 1433 du conteneur. Pour plus d’informations, consultez notre guide de démarrage rapide [Exécuter des images de conteneur SQL Server avec Docker](quickstart-install-connect-docker.md).

Lors de l’inscription des noms des principaux du service (SPN), vous pouvez utiliser le nom d’hôte de l’ordinateur ou le nom du conteneur, mais vous devez le configurer en fonction de ce que vous souhaitez voir quand vous vous connectez en externe au conteneur.

Assurez-vous qu’une entrée d’hôte de transfert (A) est ajoutée dans Active Directory pour l’adresse IP de l’hôte Linux, mappée au conteneur SQL Server. Dans ce tutoriel, l’adresse IP de l’ordinateur hôte `myubuntu` est `10.0.0.10`, et le nom du conteneur SQL Server est `sql1`. Nous ajoutons l’entrée de l’hôte de transfert dans Active Directory comme indiqué ci-dessous. Lorsque les utilisateurs se connectent à sql1.contoso.com, cette entrée garantit que l’hôte approprié est atteint.

:::image type="content" source="media/sql-server-linux-containers-ad-auth-adutil-tutorial/host-a-record.png" alt-text="ajouter un enregistrement d’hôte":::

Pour ce tutoriel, nous utilisons un environnement dans Azure avec trois machines virtuelles. Une machine virtuelle agit en tant que contrôleur de domaine (DC) Windows, avec le nom de domaine `contoso.com`. Le contrôleur de domaine est nommé `adVM.contoso.com`. Le deuxième ordinateur est un ordinateur Windows appelé `winbox`, doté de Windows 10 Desktop, qui est utilisé comme zone cliente et sur lequel SQL Server Management Studio (SSMS) est installé. Le troisième ordinateur est un ordinateur Ubuntu 18.04 LTS nommé `myubuntu`, qui héberge les conteneurs SQL Server. Tous les ordinateurs ont été joints au domaine `contoso.com`. Pour plus d’informations, consultez [Joindre SQL Server sur un hôte Linux à un domaine Active Directory](sql-server-linux-active-directory-join-domain.md).

> [!NOTE]
> Il n’est pas obligatoire de joindre l’ordinateur conteneur hôte au domaine, comme vous le verrez ultérieurement dans cet article.

## <a name="install-adutil-preview"></a>Installer adutil-preview

Sur l’ordinateur hôte Linux, utilisez les commandes suivantes pour installer adutil-preview en fonction de la distribution Linux.

> [!NOTE]
> Pour cette préversion, nous sommes conscients que sur certaines distributions Linux, si l’installation d’adutil est tentée sans le paramètre `ACCEPT_EULA`, l’expérience d’installation est entravée. Notre recommandation ci-dessous est d’installer l’outil adutil-preview en définissant `ACCEPT_EULA=Y`. Vous pouvez lire le [CLUF](https://go.microsoft.com/fwlink/?linkid=2151376) en aperçu avant l’installation. Nous travaillons activement à ce problème qui devrait être résolu pour la version GA.

### <a name="rhel"></a>RHEL

1. Téléchargez le fichier config du référentiel Microsoft Red Hat.

    ```bash
    sudo curl -o /etc/yum.repos.d/msprod.repo https://packages.microsoft.com/config/rhel/8/prod.repo
    ```

1. Si vous aviez une version précédente d’adutil installée, supprimez tous les anciens packages adutil.

    ```bash
    sudo yum remove adutil
    ```

1. Exécutez les commandes suivantes pour installer adutil-preview. `ACCEPT_EULA=Y` accepte la préversion du CLUF pour adutil. Le CLUF est placé dans le dossier `/usr/share/adutil/`.

    ```bash
    sudo ACCEPT_EULA=Y yum install -y adutil-preview
    ```

### <a name="ubuntu"></a>Ubuntu

1. Importez les clés GPG du dépôt public, puis inscrivez le dépôt Microsoft Ubuntu.

    ```bash
    curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
    sudo curl https://packages.microsoft.com/config/ubuntu/18.04/prod.list | sudo tee /etc/apt/sources.list.d/msprod.list
    ```

1. Si vous aviez une version précédente d’adutil installée, supprimez tous les anciens packages adutil à l’aide des commandes ci-dessous.

    ```bash
    sudo apt-get remove adutil
    ```

1. Exécutez la commande suivante pour installer adutil-preview. `ACCEPT_EULA=Y` accepte la préversion du CLUF pour adutil. Le CLUF est placé dans le dossier `/usr/share/adutil/`.

    ```bash
    sudo apt-get update
    sudo ACCEPT_EULA=Y apt-get install -y adutil-preview
    ```

### <a name="sles"></a>SLES

1. Ajoutez le référentiel Microsoft SQL Server à Zypper.

    ```bash
    sudo zypper addrepo -fc https://packages.microsoft.com/config/sles/12/prod.repo
    ```

1. Si vous aviez une version précédente d’adutil installée, supprimez tous les anciens packages adutil.

    ```bash
    sudo zypper remove adutil
    ```

1. Exécutez la commande suivante pour installer adutil-preview. `ACCEPT_EULA=Y` accepte la préversion du CLUF pour adutil. Le CLUF est placé dans le dossier `/usr/share/adutil/`.

    ```bash
    sudo ACCEPT_EULA=Y zypper install -y adutil-preview
    ```

## <a name="creating-the-ad-user-spns-and-sql-server-service-keytab"></a>Création de l’utilisateur AD, des noms des principaux du service et du fichier keytab du service SQL Server

Si vous ne souhaitez pas que l’hôte conteneur SQL Server sur Linux fasse partie du domaine et que vous n’avez pas suivi les étapes pour joindre l’ordinateur au domaine, suivez alors les étapes ci-dessous sur un autre ordinateur Linux qui fait déjà partie du domaine AD :

 1. Créez un utilisateur AD pour SQL Server et définissez le nom de principal du service à l’aide de l’outil adutil.

 2. Créez et configurez le fichier keytab du service SQL Server.

Copiez le fichier mssql.keytab qui a été créé sur l’ordinateur hôte qui exécutera le conteneur SQL Server, et configurez le conteneur afin d’utiliser le fichier mssql.keytab copié. Si vous le souhaitez, vous pouvez également joindre votre hôte Linux qui exécutera le conteneur SQL Server au domaine AD et suivre les étapes ci-dessous sur le même ordinateur.

### <a name="create-an-ad-user-for-sql-server-and-set-the-serviceprincipalname-using-the-adutil-tool"></a>Créer un utilisateur AD pour SQL Server et définir ServicePrincipalName à l’aide de l’outil adutil

L’activation de l’authentification AD sur les conteneurs SQL Server sur Linux requiert l’exécution des étapes 1-3 mentionnées ci-dessous sur une machine Linux qui fait partie du domaine AD.

1. Obtenez ou renouvelez le ticket Kerberos TGT (Ticket-Granting Ticket) à l’aide de la commande `kinit` . Utilisez un compte privilégié pour la commande `kinit`. Le compte doit avoir l’autorisation de se connecter au domaine et doit également être en mesure de créer des comptes et des noms de principaux de service dans le domaine.

    > [!IMPORTANT]
    > Avant d’exécuter cette commande, l’hôte doit déjà faire partie du domaine comme indiqué à l’étape précédente.

    ```bash
    kinit privilegeduser@DOMAIN.COM
    ```

    Exemple : pour l’environnement décrit ci-dessus, mon compte privilégié est `amvin@CONTOSO.COM`.

    ```bash
    kinit amvin@CONTOSO.COM
    ```

2. À l’aide de l’outil adutil, créez le nouvel utilisateur qui sera utilisé comme compte AD privilégié par SQL Server.

   ```bash
   adutil user create --name sqluser -distname CN=sqluser,CN=Users,DC=CONTOSO,DC=COM --password 'P@ssw0rd'
   ```

    > [!NOTE]
    > Les mots de passe peuvent être spécifiés de l’une des trois façons suivantes :
    >
    > - Indicateur de mot de passe : --password \<password\>
    > - Variables d’environnement – `ADUTIL_ACCOUNT_PWD`
    > - Entrée interactive
    >
    > La priorité des méthodes d’entrée de mot de passe suit l’ordre des options ci-dessus. Les options recommandées sont de fournir le mot de passe à l’aide des variables d’environnement ou d’une entrée interactive, car elles sont plus sécurisées que l’indicateur de mot de passe.

    Vous pouvez spécifier le nom du compte à l’aide du nom unique (`-distname`) comme indiqué ci-dessus, ou vous pouvez utiliser le nom de l’unité d’organisation (UO). Le nom de l’unité d’organisation (`--ou`) a priorité sur le nom unique si vous spécifiez les deux. Vous pouvez exécuter la commande ci-dessous pour plus d’informations :

    ```bash
    adutil user create --help
    ```

3. Inscrivez les noms SPN auprès de l’utilisateur créé ci-dessus. Vous pouvez utiliser le nom de l’ordinateur hôte à la place du nom du conteneur, si vous le souhaitez, en fonction de la façon dont vous souhaitez que la connexion apparaisse en externe. Dans ce tutoriel, le port 5433 est utilisé à la place de 1433. Il s’agit du mappage de port pour le conteneur. Votre numéro de port peut être différent.

    ```bash
    adutil spn addauto -n sqluser -s MSSQLSvc -H sql1.contoso.com -p 5433
    ```

    > [!NOTE]
    >
    > - `addauto` créera automatiquement les noms SPN, à condition que des privilèges suffisants soient présents pour le compte kinit.
    > - `-n` : nom du compte auquel les noms SPN seront affectés.
    > - `-s` : nom du service à utiliser pour générer les noms SPN. Dans ce cas, il est destiné à un service SQL Server et le nom du service est MSSQLSvc.
    > - `-H` : nom d’hôte à utiliser pour générer les noms SPN. S’il n’est pas spécifié, le nom de domaine complet de l’hôte local est utilisé. Spécifiez également le nom de domaine complet pour le nom du conteneur. Dans ce cas, le nom du conteneur est `sql1` et le nom de domaine complet est `sql1.contoso.com`.
    > - `-p` : port à utiliser pour générer les noms SPN. S’il n’est pas spécifié, les noms SPN sont générés sans port. Les connexions SQL fonctionnent dans ce cas seulement quand SQL Server écoute le port par défaut 1433.

### <a name="create-the-sql-server-service-keytab-file"></a>Créer le fichier keytab du service SQL Server

Créez le fichier keytab qui contient des entrées pour chacun des 4 noms SPN créés précédemment, et une entrée pour l’utilisateur. Le fichier keytab sera monté sur le conteneur, de sorte qu’il peut être créé à n’importe quel emplacement sur l’hôte. Vous pouvez modifier ce chemin en toute sécurité, tant que le fichier keytab obtenu est monté correctement lors de l’utilisation de docker/podman pour déployer le conteneur.

Pour créer le fichier keytab pour tous les noms SPN, nous pouvons utiliser l’option `createauto` :

```bash
adutil keytab createauto -k /container/sql1/secrets/mssql.keytab -p 5433 -H sql1.contoso.com --password 'P@ssw0rd' -s MSSQLSvc
```

> [!NOTE]
>
> - `-k` : chemin où vous souhaitez créer le fichier `mssql.keytab`. Dans l’exemple ci-dessus, le répertoire « /container/sql1/secrets » doit déjà exister sur l’hôte.
> - `-p` : port à utiliser pour générer les noms SPN. S’il n’est pas spécifié, les noms SPN sont générés sans port.
> - `-H` : nom d’hôte à utiliser pour générer les noms SPN. S’il n’est pas spécifié, le nom de domaine complet de l’hôte local est utilisé. Spécifiez également le nom de domaine complet pour le nom du conteneur. Dans ce cas, le nom du conteneur est `sql1` et le nom de domaine complet est `sql1.contoso.com`.
> - `-s` : nom du service à utiliser pour générer les noms SPN. Dans ce cas, il est destiné à un service SQL Server et le nom du service est MSSQLSvc.
> - `--password` : Il s’agit du mot de passe du compte d’utilisateur AD privilégié qui a été créé précédemment.
> - `-e` ou `--enctype` : Types de chiffrement pour l’entrée keytab. Utilisez une liste de valeurs séparées par des virgules. S’il n’est pas spécifié, une invite interactive est présentée.

Quand vous avez le choix entre les types de chiffrement, vous pouvez en choisir plusieurs. Pour cet exemple, nous avons choisi `aes256-cts-hmac-sha1-96` et `arcfour-hmac`. Veillez à choisir le type de chiffrement qui est pris en charge par l’hôte et le domaine.

Si vous souhaitez choisir de façon non interactive le type de chiffrement, vous pouvez spécifier votre choix de type de chiffrement avec l’argument -e dans la commande ci-dessus. Pour obtenir une aide supplémentaire sur les commandes adutil, exécutez la commande ci-dessous.

```bash
adutil keytab createauto --help
```

> [!NOTE]
> `arcfour-hmac` est un chiffrement faible et n’est pas un type de chiffrement recommandé à utiliser dans un environnement de production.

Pour créer le fichier keytab pour l’utilisateur, la commande est la suivante :

```bash
adutil keytab create -k /container/sql1/secrets/mssql.keytab -p sqluser --password 'P@ssw0rd!'
```

> [!NOTE]
>
> - `-k` : chemin où vous souhaitez créer le fichier `mssql.keytab`. Dans l’exemple ci-dessus, le répertoire « /container/sql1/secrets » doit déjà exister sur l’hôte.
> - `-p` : principal à ajouter au fichier keytab.

La création/création automatique du fichier keytab adutil ne remplace pas les fichiers précédents. Elle s’ajoute simplement au fichier, s’il est déjà présent.

Vérifiez que le fichier keytab créé dispose des autorisations appropriées lors du déploiement du conteneur.

```bash
chmod 440 /container/sql1/secrets/mssql.keytab
```

> [!NOTE]
> À ce stade, vous pouvez copier le fichier mssql.keytab de l’hôte Linux actuel vers l’hôte Linux où vous voulez déployer le conteneur SQL Server, puis suivez les étapes restantes sur l’hôte Linux qui exécutera le conteneur SQL Server. Si les étapes ci-dessus ont été effectuées sur l’hôte Linux où les conteneurs SQL seront déployés, suivez également les étapes ci-dessous sur cet hôte.

## <a name="create-the-config-files-to-be-used-by-the-sql-server-container"></a>Créer les fichiers de configuration que le conteneur SQL Server utilisera

1. Créez un fichier `mssql.conf` avec les paramètres pour AD. Ce fichier peut être créé n’importe où sur l’hôte et doit être monté correctement au cours de la commande docker run. Dans cet exemple, nous avons placé ce fichier `mssql.conf` sous `/container/sql1 `, qui est notre répertoire de conteneur. Le contenu de `mssql.conf` est comme indiqué ci-dessous :

    ```output
    [network]
    privilegedadaccount = sqluser
    kerberoskeytabfile = /var/opt/mssql/secrets/mssql.keytab
    ```

    > [!NOTE]
    >
    > - `privilagedadaccount` : Utilisateur AD privilégié à utiliser pour l'authentification AD.
    > - `kerberoskeytabfile` : Chemin dans le conteneur où le fichier mssql.keytab se trouvera.

1. Créez un fichier krb5.conf. Voici un exemple illustré ci-dessous. La casse est prise en compte pour ces fichiers.

    ```output
    [libdefaults]
    default_realm = DOMAIN.COM

    [realms]
     CONTOSO.COM = {
         kdc = adVM.contoso.com
         admin_server = adVM.contoso.com
         default_domain = CONTOSO.COM
     }

    [domain_realm]
     .contoso.com = CONTOSO.COM
     contoso.com = CONTOSO.COM


1. Copy all files, `mssql.conf`, `krb5.conf`, `mssql.keytab` to a location that will be mounted to the SQL Server container. In this example, these files are placed on the host at the following locations: `mssql.conf` and `krb5.conf` at `/container/sql1/`. `mssql.keytab` is placed at the location `/container/sql1/secrets/`.

1. Make sure there's enough permission on these folders for the user running the docker/podman command. When the container starts, the user needs access to the folder path created. In this example, we provided the below permissions given to the folder path:

    ```bash
    sudo chmod 755 /container/sql1/
    ```

## <a name="mount-the-config-files-and-deploy-the-sql-server-container"></a>Monter les fichiers de configuration et déployer le conteneur SQL Server

Exécutez votre conteneur SQL Server et montez les fichiers de configuration AD appropriés qui ont été créés précédemment, comme indiqué ci-dessous :

```bash
sudo docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=\<YourStrong@Passw0rd\>" \
-p 5433:1433 --name sql1 \
-v /container/sql1:/var/opt/mssql \
-v /container/sql1/krb5.conf:/etc/krb5.conf \
-d mcr.microsoft.com/mssql/server:2019-latest
```

> [!NOTE]
> Quand vous exécutez le conteneur sur le module de sécurité Linux (LSM) comme les hôtes activés pour SELinux, vous devez monter les volumes à l’aide de l’option `Z`, qui indique à Docker d’étiqueter le contenu avec une étiquette privée non partagée. Pour plus d’informations, reportez-vous à la [configuration de l’étiquette selinux](https://docs.docker.com/storage/bind-mounts/#configure-the-selinux-label).

Notre exemple contient les commandes suivantes :

```bash
sudo docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=P@ssw0rd" -p 5433:1433 --name sql1 \
-v /container/sql1:/var/opt/mssql/ \
-v /container/sql1/krb5.conf:/etc/krb5.conf \
--dns-search contoso.com \
--dns 10.0.0.4 \
--add-host adVM.contoso.com:10.0.0.4 \
--add-host contoso.com:10.0.0.4 \
--add-host contoso:10.0.0.4 \
-d mcr.microsoft.com/mssql/server:2019-latest
```

> [!NOTE]
>
> - Les fichiers `mssql.conf` et `krb5.conf` se trouvent dans le chemin d’accès de fichier hôte `/container/sql1`.
> - Le fichier `mssql.keytab` qui a été créé se trouve dans le chemin d’accès de fichier hôte `/container/sql1/secrets`.
> - Comme notre ordinateur hôte se trouve sur Azure, les détails AD doivent être ajoutés dans le même ordre à la commande docker run. Dans notre exemple, le contrôleur de domaine `adVM` se trouve dans le domaine `contoso.com`, avec l’adresse IP `10.0.0.4`. Le contrôleur de domaine exécute DNS et KDC.

## <a name="create-ad-based-sql-server-logins-in-transact-sql"></a>Créer des connexions SQL Server basées sur AD dans Transact-SQL

Connectez-vous au conteneur SQL et exécutez les commandes suivantes pour créer la connexion, puis vérifiez qu’elle est listée. Vous pouvez exécuter cette commande à partir d’un ordinateur client (Windows ou Linux) qui exécute SSMS, Azure Data Studio (ADS) ou n’importe quel autre outil d’interface de ligne de commande (CLI).

```sql
create login [contoso\amvin] From Windows
SELECT name FROM sys.server_principals;
```

## <a name="connect-to-sql-server-using-ad-authentication"></a>Se connecter à SQL Server à l’aide de l’authentification AD.

Pour vous connecter en utilisant [SSMS](../ssms/download-sql-server-management-studio-ssms.md) ou [ADS](../azure-data-studio/download-azure-data-studio.md), connectez-vous à SQL Server avec les informations d’identification Windows en utilisant le numéro de port et le nom SQL Server (le nom peut être le nom de conteneur ou le nom d’hôte). Dans notre exemple, le nom du serveur est `sql1.contoso.com, 5433`.

Vous pouvez également utiliser un outil comme [sqlcmd](../tools/sqlcmd-utility.md) pour vous connecter à SQL Server dans votre conteneur.

```bash
sqlcmd -E -S 'sql1.contoso.com, 5433'
```

## <a name="next-steps"></a>Étapes suivantes

- [Démarrage rapide : Exécution d’images conteneur SQL Server avec Docker](quickstart-install-connect-docker.md)
- [Joindre SQL Server sur un hôte Linux à un domaine Active Directory](sql-server-linux-active-directory-auth-overview.md)
