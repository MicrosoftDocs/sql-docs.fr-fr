---
title: Configurer l’authentification Active Directory avec SQL Server sur Linux à l’aide d’adutil
description: Guide étape par étape pour configurer l’authentification Active Directory avec SQL Server sur Linux à l’aide d’adutil
author: amvin87
ms.author: amitkh
ms.reviewer: vanto
ms.date: 12/10/2020
ms.topic: tutorial
ms.prod: sql
ms.technology: linux
moniker: '>= sql-server-linux-2017 || >= sql-server-2017 || =sqlallproducts-allversions'
ms.openlocfilehash: 6b68cb2b2ff3712502ef4e0862fe71ad78705d7c
ms.sourcegitcommit: c52a6aeb6fa6d7c3a86b3e84449361f4a0949ad0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/06/2021
ms.locfileid: "99623759"
---
# <a name="tutorial-configure-active-directory-authentication-with-sql-server-on-linux-using-adutil"></a>Tutoriel : Configurer l’authentification Active Directory avec SQL Server sur Linux à l’aide d’adutil

> [!NOTE]
> **adutil** est actuellement en **préversion publique**.

Ce tutoriel explique comment configurer l’authentification Active Directory (AD) pour SQL Server sur Linux à l’aide d’adutil. Pour une autre méthode de configuration de l’authentification AD à l’aide de ktpass, consultez [Tutoriel : Utiliser l’authentification Active Directory avec SQL Server sur Linux](sql-server-linux-active-directory-authentication.md).

Ce didacticiel contient les tâches suivantes :

> [!div class="checklist"]
> - Installer adutil-preview
> - Joindre une machine Linux à votre domaine AD
> - Créer un utilisateur AD pour SQL Server et définir ServicePrincipalName (SPN) à l’aide de l’outil adutil
> - Créer le fichier keytab du service SQL Server
> - Configurer SQL Server pour utiliser le fichier keytab
> - Créer des connexions SQL Server basées sur AD à l’aide de Transact-SQL
> - Se connecter à SQL Server à l’aide de l’authentification AD

## <a name="prerequisites"></a>Prérequis

Les éléments suivants sont nécessaires avant de configurer l’authentification AD :

- Disposez d’un contrôleur de domaine AD (Windows) dans votre réseau.
- Installez l’outil adutil-preview sur un ordinateur hôte Linux. Suivez les instructions de la section ci-dessous, selon la distribution Linux que vous exécutez, pour installer adutil-preview.

## <a name="install-adutil-preview"></a>Installer adutil-preview

Sur l’ordinateur hôte Linux, utilisez les commandes suivantes pour installer adutil-preview.

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

1. Exécutez les commandes suivantes pour installer adutil-preview. `ACCEPT_EULA=Y` accepte la préversion du CLUF pour adutil. Le CLUF est placé dans le dossier « /usr/share/adutil/ ».

    ```bash
    sudo ACCEPT_EULA=Y yum install -y adutil-preview
    ```

### <a name="ubuntu"></a>Ubuntu

1. Enregistrez le référentiel Microsoft Ubuntu.

    ```bash
    sudo curl https://packages.microsoft.com/config/ubuntu/18.04/prod.list | sudo tee /etc/apt/sources.list.d/msprod.list
    ```

1. Si vous aviez une version précédente d’adutil installée, supprimez tous les anciens packages adutil à l’aide des commandes ci-dessous.

    ```bash
    sudo apt-get remove adutil
    ```

1. Exécutez la commande suivante pour installer adutil-preview. `ACCEPT_EULA=Y` accepte la préversion du CLUF pour adutil. Le CLUF est placé dans le dossier « /usr/share/adutil/ ».

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

1. Exécutez la commande suivante pour installer adutil-preview. `ACCEPT_EULA=Y` accepte la préversion du CLUF pour adutil. Le CLUF est placé dans le dossier « /usr/share/adutil/ ».

    ```bash
    sudo ACCEPT_EULA=Y zypper install -y adutil-preview
    ```

## <a name="domain-machine-preparation"></a>Préparation de l’ordinateur de domaine

Assurez-vous qu’une entrée d’hôte de transfert (A) est ajoutée dans Active Directory pour l’adresse IP de l’hôte Linux. Dans ce tutoriel, l’adresse IP de l’ordinateur hôte `myubuntu` est `10.0.0.10`. Nous ajoutons l’entrée de l’hôte de transfert dans Active Directory comme indiqué ci-dessous. Lorsque les utilisateurs se connectent à myubuntu.contoso.com, cette entrée garantit que l’hôte approprié est atteint.

:::image type="content" source="media/sql-server-linux-ad-auth-adutil-tutorial/host-a-record.png" alt-text="ajouter un enregistrement d’hôte":::

Pour ce tutoriel, nous utilisons un environnement dans Azure avec trois machines virtuelles. Une machine virtuelle agit en tant que contrôleur de domaine (DC) Windows, avec le nom de domaine `contoso.com`. Le contrôleur de domaine est nommé `adVM.contoso.com`. Le deuxième ordinateur est un ordinateur Windows appelé `winbox`, doté de Windows 10 Desktop, qui est utilisé comme zone cliente et sur lequel SQL Server Management Studio (SSMS) est installé. Le troisième ordinateur est un ordinateur Ubuntu 18.04 LTS nommé `myubuntu`, qui héberge SQL Server.

## <a name="join-the-linux-host-machine-to-your-ad-domain"></a>Joindre l’ordinateur hôte Linux à votre domaine AD

Joignez votre hôte SQL Server Linux à un contrôleur de domaine Active Directory. Pour plus d’informations sur la façon de joindre un domaine Active Directory, consultez [Joindre SQL Server sur un hôte Linux à un domaine Active Directory](sql-server-linux-active-directory-join-domain.md).

## <a name="create-an-ad-user-for-sql-server-and-set-the-serviceprincipalname-spn-using-the-adutil-tool"></a>Créer un utilisateur AD pour SQL Server et définir ServicePrincipalName (SPN) à l’aide de l’outil adutil

1. Obtenez ou renouvelez le ticket Kerberos TGT (Ticket-Granting Ticket) à l’aide de la commande `kinit` . Utilisez un compte privilégié pour la commande `kinit`. Le compte doit avoir l’autorisation de se connecter au domaine et doit également être en mesure de créer des comptes et des noms de principaux de service dans le domaine.

    > [!IMPORTANT]
    > Avant d’exécuter cette commande, l’ordinateur hôte doit déjà faire partie du domaine comme indiqué à l’étape précédente.

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

3. Inscrivez les noms SPN auprès du principal créé ci-dessus. Utilisez le nom de domaine complet de l’ordinateur. Dans ce tutoriel, nous utilisons le port par défaut de SQL Server, 1433. Votre numéro de port peut être différent.

    ```bash
    adutil spn addauto -n sqluser -s MSSQLSvc -H myubuntu.contoso.com -p 1433
    ```

    > [!NOTE]
    >
    > - `addauto` créera automatiquement les noms SPN, à condition que des privilèges suffisants soient présents pour le compte kinit.
    > - `-n` : nom du compte auquel les noms SPN seront affectés.
    > - `-s` : nom du service à utiliser pour générer les noms SPN. Dans ce cas, il est destiné à un service SQL Server et le nom du service est `MSSQLSvc`.
    > - `-H` : nom d’hôte à utiliser pour générer les noms SPN. S’il n’est pas spécifié, le nom de domaine complet de l’hôte local est utilisé. Dans ce cas, le nom d’hôte est `myubuntu` et le nom de domaine complet est `myubuntu.contoso.com`.
    > - `-p` : port à utiliser pour générer les noms SPN. S’il n’est pas spécifié, les noms SPN sont générés sans port. Les connexions SQL fonctionnent dans ce cas seulement quand SQL Server écoute le port par défaut 1433.

## <a name="create-the-sql-server-service-keytab-file"></a>Créer le fichier keytab du service SQL Server

Créez le fichier keytab qui contient des entrées pour chacun des 4 noms SPN créés précédemment, et une entrée pour l’utilisateur.

```bash
adutil keytab createauto -k /var/opt/mssql/secrets/mssql.keytab -p 1433 -H myubuntu.contoso.com --password 'P@ssw0rd' -s MSSQLSvc 
```

> [!NOTE]
>
> - `-k` : chemin où vous souhaitez créer le fichier `mssql.keytab`. Dans l’exemple ci-dessus, le répertoire `/var/opt/mssql/secrets/` doit déjà exister sur l’hôte.
> - `-p` : port à utiliser pour générer les noms SPN. S’il n’est pas spécifié, les noms SPN sont générés sans port.
> - `-H` : nom d’hôte à utiliser pour générer les noms SPN. S’il n’est pas spécifié, le nom de domaine complet de l’hôte local est utilisé. Dans ce cas, le nom d’hôte est `myubuntu` et le nom de domaine complet est `myubuntu.contoso.com`.
> - `-s` : nom du service à utiliser pour générer les noms SPN. Dans ce cas, il est destiné à un service SQL Server et le nom du service est `MSSQLSvc`.
> - `--password` : Il s’agit du mot de passe du compte d’utilisateur AD privilégié qui a été créé précédemment.
> - `-e` ou `--enctype` : Types de chiffrement pour l’entrée keytab. Utilisez une liste de valeurs séparées par des virgules. S’il n’est pas spécifié, une invite interactive est présentée.

Quand vous avez le choix entre les types de chiffrement, vous pouvez en choisir plusieurs. Pour cet exemple, nous avons choisi `aes256-cts-hmac-sha1-96` et `arcfour-hmac`. Veillez à choisir le type de chiffrement qui est pris en charge par l’hôte et le domaine.

Si vous souhaitez choisir de façon non interactive le type de chiffrement, vous pouvez spécifier votre choix de type de chiffrement avec l’argument -e dans la commande ci-dessus. Pour obtenir une aide supplémentaire sur les commandes adutil, exécutez la commande ci-dessous.

```bash
adutil keytab createauto --help
```

> [!NOTE]
> `arcfour-hmac` est un chiffrement faible et n’est pas un type de chiffrement recommandé à utiliser dans un environnement de production.

Ajoutez une entrée dans le fichier keytab pour le nom de principal et son mot de passe qui sera utilisé par SQL Server pour se connecter à AD :

```bash
adutil keytab create -k /var/opt/mssql/secrets/mssql.keytab -p sqluser --password 'P@ssw0rd!'
```

> [!NOTE]
>
> - `-k` : chemin où vous souhaitez créer le fichier `mssql.keytab`.
> - `-p` : principal à ajouter au fichier keytab.

La création/création automatique du fichier keytab adutil ne remplace pas les fichiers précédents. Elle s’ajoute simplement au fichier, s’il est déjà présent.

Assurez-vous que le fichier keytab créé est détenu par l’utilisateur `mssql` et que seul l’utilisateur `mssql` dispose d’un accès en lecture/écriture au fichier. Vous pouvez exécuter les commandes `chown` et `chmod` comme indiqué ci-dessous :

```bash
chown mssql. /var/opt/mssql/secrets/mssql.keytab
chmod 440 /var/opt/mssql/secrets/mssql.keytab
```

## <a name="configure-sql-server-to-use-the-keytab"></a>Configurer SQL Server pour utiliser le fichier keytab

Exécutez les commandes ci-dessous pour configurer SQL Server afin d’utiliser le fichier keytab créé à l’étape précédente, et pour définir l’utilisateur créé ci-dessus en tant que compte AD privilégié. Dans notre exemple, le nom d’utilisateur est `sqluser`.

```bash
/opt/mssql/bin/mssql-conf set network.kerberoskeytabfile /var/opt/mssql/secrets/mssql.keytab
/opt/mssql/bin/mssql-conf set network.privilegedadaccount sqluser
```

## <a name="restart-sql-server"></a>Redémarrer SQL Server

Exécutez la commande ci-dessous pour redémarrer le service SQL Server :

```bash
sudo systemctl restart mssql-server
```

## <a name="create-ad-based-sql-server-logins-in-transact-sql"></a>Créer des connexions SQL Server basées sur AD dans Transact-SQL

Connectez-vous à SQL Server et exécutez les commandes suivantes pour créer la connexion, puis vérifiez qu’elle est listée.

```sql
create login [contoso\amvin] From Windows
SELECT name FROM sys.server_principals;
```

## <a name="connect-to-sql-server-using-ad-authentication"></a>Se connecter à SQL Server à l’aide de l’authentification AD.

Pour vous connecter en utilisant [SSMS](../ssms/download-sql-server-management-studio-ssms.md) ou [Azure Data Studio](../azure-data-studio/download-azure-data-studio.md), connectez-vous à SQL Server avec vos informations d’identification Windows.

Vous pouvez également utiliser un outil comme [sqlcmd](../tools/sqlcmd-utility.md) pour vous connecter à SQL Server avec l’authentification Windows.

```bash
sqlcmd -E -S 'myubuntu.contoso.com'
```

## <a name="next-steps"></a>Étapes suivantes

- [Joindre SQL Server sur un hôte Linux à un domaine Active Directory](sql-server-linux-active-directory-auth-overview.md)
- Si vous voulez savoir comment configurer l’authentification AD avec des conteneurs SQL Server sur Linux, consultez [Configurer l’authentification Active Directory avec des conteneurs SQL Server sur Linux](sql-server-linux-containers-ad-auth-adutil-tutorial.md).
