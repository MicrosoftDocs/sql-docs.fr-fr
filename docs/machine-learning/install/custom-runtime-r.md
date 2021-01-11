---
title: Installer le CLR personnalisé R
description: Découvrez comment installer un CLR personnalisé R pour SQL Server.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 09/20/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15'
ms.openlocfilehash: 9029dd87712ee7128504a9b495b699c9c58e000d
ms.sourcegitcommit: 5b2c47ce88f7e56552fd415c32b319009d043b56
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/29/2020
ms.locfileid: "97804340"
---
# <a name="install-an-r-custom-runtime-for-sql-server"></a>Installer un CLR personnalisé R pour SQL Server

[!INCLUDE [SQL Server 2019 and later](../../includes/applies-to-version/sqlserver2019.md)]

Cet article explique comment installer un CLR personnalisé pour l’exécution de scripts R avec SQL Server. Le runtime personnalisé utilise une technologie d’extension de langage reposant sur un framework d’extensibilité pour l’exécution de code externe. Le CLR personnalisé pour R peut être utilisé dans les scénarios suivants :

+ Une installation de SQL Server avec l’infrastructure d’extensibilité.

+ Une installation de Machine Learning Services avec SQL Server 2019. L’extension de langage peut être utilisée avec [Machine Learning Services SQL Server](../sql-server-machine-learning-services.md) après avoir effectué des étapes de configuration supplémentaires.

::: moniker range=">=sql-server-ver15"

> [!NOTE]
> Cet article explique comment installer un CLR personnalisé pour l’exécution de scripts R sur Windows. Pour installer sur Linux, consultez [installer un CLR personnalisé R pour SQL Server sur Linux](custom-runtime-r.md?view=sql-server-linux-ver15&preserve-view=true)

## <a name="pre-install-checklist"></a>Liste de contrôle avant l’installation

Avant d’installer un CLR personnalisé R, installez les éléments suivants :

+ [SQL Server 2019 pour Windows (avec la mise à jour cumulative 3 ou version ultérieure)](../../database-engine/install-windows/install-sql-server.md).

+ [Extensions de langage SQL Server sur Windows avec l’infrastructure d’extensibilité](../../language-extensions/install/windows-java.md).

+ [R Version 3.3 ou ultérieure](https://cran.r-project.org/).

## <a name="add-sql-server-language-extensions-for-windows"></a>Ajouter Extensions de langage SQL Server pour Windows

> [!NOTE]
> Si vous avez Machine Learning Services installé sur SQL Server 2019, l’infrastructure d’extensibilité pour les extensions de langage avec le service Launchpad est déjà installée et vous pouvez ignorer cette étape.

Les extensions de langage utilisent l’infrastructure d’extensibilité pour exécuter du code externe. L’exécution du code est isolée des processus du moteur de base, mais entièrement intégrée à l’exécution des requêtes SQL Server.

1. Démarrez l’Assistant Installation de SQL Server 2019.

1. Sous l’onglet **Installation**, sélectionnez **Nouvelle installation autonome de SQL Server ou ajout de fonctionnalités à une installation existante**.

    ![Installation de SQL Server 2019 CU3 ou version ultérieure](../install/media/2019setup-installation-page-mlsvcs.png)

1. Dans la page **Sélection de fonctionnalités** , sélectionnez les options suivantes :

    - **Services Moteur de base de données**

        Pour utiliser les extensions de langage avec SQL Server, vous devez installer une instance du moteur de base de données. Vous pouvez utiliser l’instance par défaut ou une instance nommée.

    - **Machine Learning Services et extensions de langage**

        Sélectionnez **Machine Learning Services et extensions de langage**. Il n’est pas nécessaire de sélectionner R.

        :::image type="content" source="media/2019-setup-language-extensions.png" alt-text="Installation des extensions de langage SQL Server 2019.":::

1. Dans la page **Prêt pour l’installation**, vérifiez que ces sélections sont incluses, puis choisissez **Installer**.

    + Services Moteur de base de données
    + Machine Learning Services et extensions de langage

1. Si vous êtes invité à redémarrer l’ordinateur après l’installation, faites-le dès à présent. Il est important de lire le message affiché par l'Assistant Installation à la fin de l'installation. Pour plus d'informations, consultez [View and Read SQL Server Setup Log Files](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md).

## <a name="install-r"></a>Installation de R

> [!NOTE]
>Pour SQL Machine Learning Services, R est déjà installé dans le dossier **R_SERVICES** de votre instance SQL Server. Exemple : C:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\R_SERVICES Si vous souhaitez continuer à utiliser ce chemin d’accès comme R_HOME, passez à l’étape suivante de la procédure d’installation de Rcpp. Dans le cas contraire, si vous souhaitez utiliser un autre CLR de R, continuez ici pour l’installer.

Installez [R (3.3 ou version ultérieure)](https://cran.r-project.org/bin/windows/base/) et notez le chemin où il est installé. Ce chemin d’accès correspond à votre **R_HOME**. Par exemple, comme indiqué ici, R_HOME est « C:\Program Files\R\R-4.0.2 »

![Installer R personnalisé](../install/media/custom-r-installation.png)

> [!NOTE]
>Dans les instructions suivantes, %R_HOME% est le chemin d’accès à votre installation de R et doit être remplacé par cette valeur.

## <a name="install-rcpp-package"></a>Installer le package Rcpp

+ Recherchez l’exécutable R dans %R_HOME%\bin. Par défaut, il se trouve dans `C:\Program Files\R\R-4.0.2\bin`.

+ À partir d’une invite de commandes avec *élévation* de privilèges, exécutez R :

```CMD
%R_HOME%\bin\R.exe
```

Dans cette invite R *élevée* (% R_HOME%\bin\R.exe), exécutez le script suivant pour installer le package rcpp dans le dossier %R_HOME%\library.

```R
install.packages("Rcpp", lib="%R_HOME%/library");
```

## <a name="update-the-system-environment-variables"></a>Mettez à jour les variables d'environnement système

1. Ajoutez ou modifiez **R_HOME** en tant que variable d’environnement système.
    + Dans la zone de recherche Windows, tapez « environnement », puis sélectionnez **Modifier les variables d’environnement système**.
    + Sous l’onglet **Avancé**, sélectionnez **Variables d’environnement**.

    + Sous **Variables système**, sélectionnez **Nouvelle** pour créer R_HOME.
    Pour modifier, sélectionnez **Modifier** pour le changer. Modifiez sa valeur pour qu’elle pointe vers le chemin d’installation de R personnalisé.

    ![Créez une variable d’environnement système R_HOME.](../install/media/sys-env-r-home.png)

2. Mettez à jour la variable d’environnement **PATH**.
    Ajoutez le chemin d’accès à **R.dll** dans la variable d’environnement **PATH** du système. Sélectionnez **PATH** puis **Modifier** et ajoutez `%R_HOME%\bin\x64` à la liste des chemins d’accès.

    ![Ajoutez à une variable d’environnement système PATH.](../install/media/sys-env-path-r.png)

3. Sélectionnez **OK** pour fermer les fenêtres restantes.

    Pour définir ces variables d’environnement à partir d’une invite de commandes *élevée*, exécutez les commandes suivantes. Veillez à utiliser le chemin d’installation de R personnalisé.

```CMD
setx /m R_HOME "path\to\installation\of\R"
setx /m PATH "path\to\installation\of\R\bin\x64;%PATH%"
```

## <a name="grant-access-to-the-custom-r-installation-folder"></a>Accorder l’accès au dossier d’installation de R personnalisé

> [!NOTE]
>Si vous avez installé R à l’emplacement par défaut de **C:\Program Files\R\R-version**, vous pouvez ignorer cette étape.

Exécutez les commandes **icacls** à partir d’une nouvelle invite de commandes *élevée* pour accorder l’accès READ & EXECUTE au **nom d'utilisateur du service SQL Server Launchpad** et SID **S-1-15-2-1** (**ALL APPLICATION PACKAGES**). Le nom d’utilisateur du service Launchpad se présente sous la forme `NT Service\MSSQLLAUNCHPAD$INSTANCENAME` où `INSTANCENAME` est le nom de l’instance de votre serveur SQL Server.

Les commandes accordent l’accès de manière récursive à tous les fichiers et dossiers sous le chemin d’un répertoire donné.

Ajoutez le nom de l’instance à `MSSQLLAUNCHPAD` (`MSSQLLAUNCHPAD$INSTANCENAME`). Dans cet exemple, `INSTANCENAME` est l’instance par défaut `MSSQLSERVER`.

1. Accordez des autorisations pour le **nom d’utilisateur du service SQL Server Launchpad**

    ```cmd
    icacls "%R_HOME%" /grant "NT Service\MSSQLLAUNCHPAD$MSSQLSERVER":(OI)(CI)RX /T
    ```
2. Accordez des autorisations à **SID S-1-15-2-1**

    ```cmd
    icacls "%R_HOME%" /grant *S-1-15-2-1:(OI)(CI)RX /T

>[!NOTE]
>The preceding command grants permissions to the computer **SID S-1-15-2-1**, which is equivalent to ALL APPLICATION PACKAGES on an English version of Windows. Alternatively, you can use `icacls "%R_HOME%" /grant "ALL APPLICATION PACKAGES":(OI)(CI)RX /T` on an English version of Windows.

## Restart SQL Server Launchpad service

From an *elevated* command prompt, run the following commands. Replace "MSSQLSERVER" with the name of your SQL Server instance.


```CMD
net stop MSSQLLAUNCHPAD$MSSQLSERVER
net start MSSQLLAUNCHPAD$MSSQLSERVER
```

Vous pouvez également cliquer avec le bouton droit sur le service de SQL Server Launchpad dans l’application **Services** du système, puis sélectionner la commande **Redémarrer**. Ou utilisez le [Gestionnaire de configuration SQL Server](../../relational-databases/sql-server-configuration-manager.md) pour redémarrer le service.

## <a name="download-r-language-extension"></a>Téléchargez l’extension pour langage R

Téléchargez le [fichier zip contenant l’extension de langage R pour Windows](https://github.com/microsoft/sql-server-language-extensions/releases). Il est recommandé d’utiliser la version de production. Optez pour la version de débogage en développement ou en test, car elle fournit des informations de journalisation détaillées permettant d’examiner les erreurs.

## <a name="register-external-language"></a>Inscrire le langage externe

Pour chaque base de données dans laquelle vous souhaitez utiliser des extensions de langage, vous devez inscrire l’extension de langage avec [CREATE EXTERNAL LANGUAGE](../../t-sql/statements/create-external-language-transact-sql.md). Utilisez [Azure Data Studio](https://docs.microsoft.com/sql/azure-data-studio/download-azure-data-studio) pour se connecter à SQL Server et exécuter la commande T-SQL suivante.
Modifiez le chemin d’accès dans cette instruction pour refléter l’emplacement du fichier zip de l’extension de langage téléchargé (R-lang-extension.zip).

> [!NOTE]
>**R** est un mot réservé. Utilisez un nom différent pour la langue externe, par exemple « myR ».

```sql
CREATE EXTERNAL LANGUAGE [myR]
FROM (CONTENT = N'/path/to/R-lang-extension.zip', FILE_NAME = 'libRExtension.dll');
GO
```

::: moniker-end

::: moniker range=">=sql-server-linux-ver15"

Vous pouvez installer SQL Server sur Red Hat Enterprise Linux (RHEL), SUSE Linux Enterprise Server (SLES) et Ubuntu. Pour plus d’informations, consultez la [section Plateformes prises en charge dans Conseils d’installation pour SQL Server sur Linux](../../linux/sql-server-linux-setup.md#supportedplatforms).

> [!NOTE]
> Cet article explique comment installer un CLR personnalisé pour l’exécution de scripts R sur Linux. Pour installer sur Windows, consultez [Installer un R personnalisé pour SQL Server sur Windows](custom-runtime-r.md?view=sql-server-ver15&preserve-view=true)

## <a name="pre-install-checklist"></a>Liste de contrôle avant l’installation

Avant d’installer un CLR personnalisé R, installez les éléments suivants :

+ [SQL Server 2019 pour Linux (avec la mise à jour cumulative 3 ou version ultérieure)](../../linux/sql-server-linux-setup.md).
Avant d’installer SQL Server sur Linux, vous devez configurer un référentiel Microsoft. Pour plus d’informations, consultez [Configuration des référentiels](../../linux/sql-server-linux-change-repo.md) (configuring repositories).

+ [Extensions de langage SQL Server sur Linux avec l’infrastructure d’extensibilité](../../linux/sql-server-linux-setup-language-extensions-java.md).

+ [R Version 3.3 ou ultérieure](https://cran.r-project.org/).

## <a name="add-sql-server-language-extensions-for-linux"></a>Ajouter des Extensions de langage SQL Server pour Linux

> [!NOTE]
> Si vous avez Machine Learning Services installé sur SQL Server 2019, le package **mssql-server-extensibility** pour les extensions de langage est déjà installée et vous pouvez ignorer cette étape.

Les extensions de langage utilisent l’infrastructure d’extensibilité pour exécuter du code externe. L’exécution du code est isolée des processus du moteur de base, mais entièrement intégrée à l’exécution des requêtes SQL Server.

Utilisez les commandes suivantes pour installer les extensions de langage, selon votre version de Linux.

### <a name="ubuntu"></a>Ubuntu
> [!Tip]
> Si possible, `sudo apt-get update` pour actualiser les packages sur le système avant l’installation. Ubuntu n’a peut-être pas l’option de transport https apt. Pour l’installer, utilisez `sudo apt-get install apt-transport-https`.

```bash
# Install as root or sudo
sudo apt-get install mssql-server-extensibility
```

### <a name="red-hat"></a>Red Hat
```bash
# Install as root or sudo
sudo yum install mssql-server-extensibility
```

### <a name="suse"></a>SUSE
```bash
# Install as root or sudo
sudo zypper install mssql-server-extensibility
```

## <a name="install-r"></a>Installation de R

>[!NOTE]
>Pour SQL Machine Learning Services, R est déjà installé dans `/opt/microsoft/ropen/3.5.2/lib64/R`. Si vous souhaitez continuer à utiliser ce chemin d’accès comme R_HOME, passez à l’étape suivante de la procédure d’**installation de rcpp**. 

Si vous souhaitez utiliser un autre CLR de R, vous devez d’abord supprimer `microsoft-r-open-mro` avant de poursuivre l’installation d’une nouvelle version. Exemple pour Ubuntu :

```bash
sudo apt remove microsoft-r-open-mro-3.5.2
```

Suivez les [instructions](https://cran.r-project.org/bin/linux/) pour terminer l’installation de R (3.3 ou version ultérieure) pour votre plateforme Linux respective. Par défaut, R est installé dans **/usr/lib/R**. Ce chemin d’accès correspond à votre **R_HOME**. Si vous installez R à un autre emplacement, prenez note de ce chemin d’accès en tant que R_HOME.

Exemples d’instructions pour Ubuntu. Modifiez l’URL de dépôt ci-dessous pour votre version de R.

```bash
export DEBIAN_FRONTEND=noninteractive
sudo apt-get update
sudo apt-get --no-install-recommends -y install curl zip unzip apt-transport-https libstdc++6

# Add R CRAN repository. This repository works for R 4.0.x.
#
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys E298A3A825C0D65DFD57CBB651716619E084DAB9
sudo add-apt-repository 'deb https://cloud.r-project.org/bin/linux/ubuntu xenial-cran40/'
sudo apt-get update

# Install R runtime.
#
sudo apt-get -y install r-base-core
```

## <a name="install-rcpp-package"></a>Installer le package Rcpp

Dans les instructions suivantes, ${R_HOME} est le chemin d’accès à votre installation de R. 

+ Localisez le fichier binaire R dans ${R_HOME}/bin. Par défaut, il se trouve dans **/usr/lib/R/bin**.

+ Démarrer R

  ```bash
  sudo ${R_HOME}/bin/R
  ```

+ Dans cette invite R *élevée* (${R_HOME}/bin/R), exécutez le script suivant pour installer le package **Rcpp** dans le dossier ${R_HOME}/library.

  ```R
  install.packages("Rcpp", lib = "${R_HOME}/library");
  ```

## <a name="using-a-custom-installation-of-r"></a>Utilisation d’une installation personnalisée de R

> [!NOTE]
>Si vous avez installé R à l’emplacement par défaut de **/usr/lib/R**, vous pouvez ignorer cette étape.

### <a name="update-the-environment-variables"></a>Mettez à jour les variables d’environnement

1. Modifiez le service mssql-launchpad pour ajouter la variable d’environnement R_HOME au fichier. `/etc/systemd/system/mssql-launchpadd.service.d/override.conf`

    ```bash
    sudo systemctl edit mssql-launchpadd
    ```

    + Insérez le texte suivant dans le fichier `/etc/systemd/system/mssql-launchpadd.service.d/override.conf` qui s’ouvre. Définissez la valeur de R_HOME sur le chemin d’installation de R personnalisé.

    ```text
    [Service]
    Environment="R_HOME=/path/to/installation/of/R"
    ```

    + Enregistrez et fermez.

2. Assurez-vous que **libR.so** peut être chargé.

    + Créez un fichier custom-r.conf dans **/etc/ld.so.conf.d**.

    ```bash
    sudo vi /etc/ld.so.conf.d/custom-r.conf
    ```

    + Dans le fichier qui s’ouvre, ajoutez path à **libR.so** à partir de l’installation personnalisée de R.

    ```vi editor
    /path/to/installation/of/R/lib
    ```

    + Enregistrez et fermez le nouveau fichier.

    + Exécutez `ldconfig` et vérifiez que **libR.so** peuvent être chargés en exécutant la commande suivante et en vérifiant que toutes les bibliothèques dépendantes peuvent être trouvées.

    ```bash
    sudo ldconfig
    ldd /path/to/installation/of/R/lib/libR.so
    ```

### <a name="grant-access-to-the-custom-r-installation-folder"></a>Accorder l’accès au dossier d’installation de R personnalisé

Définissez l’option `datadirectories` de la section extensibilité du fichier /var/opt/mssql/mssql.conf sur l’installation personnalisée de R.

```bash
sudo /opt/mssql/bin/mssql-conf set extensibility.datadirectories /path/to/installation/of/R
```

### <a name="restart-mssql-launchpadd-service"></a>Redémarrer le service mssql-Launchpad

> [!NOTE]
>Si vous avez installé R à l’emplacement par défaut de **/usr/lib/R**, vous pouvez ignorer cette étape.

```bash
sudo systemctl restart mssql-launchpadd
```

## <a name="download-r-language-extension"></a>Téléchargez l’extension pour langage R

Téléchargez le [fichier zip contenant l’extension de langage R pour Linux](https://github.com/microsoft/sql-server-language-extensions/releases). Il est recommandé d’utiliser la version de production. Optez pour la version de débogage en développement ou en test, car elle fournit des informations de journalisation détaillées permettant d’examiner les erreurs.

## <a name="register-external-language"></a>Inscrire le langage externe

Pour chaque base de données dans laquelle vous souhaitez utiliser des extensions de langage, vous devez inscrire l’extension de langage avec [CREATE EXTERNAL LANGUAGE](../../t-sql/statements/create-external-language-transact-sql.md). Utilisez [Azure Data Studio](https://docs.microsoft.com/sql/azure-data-studio/download-azure-data-studio) pour se connecter à SQL Server et exécuter la commande T-SQL suivante. 
Modifiez le chemin d’accès dans cette instruction pour refléter l’emplacement du fichier zip de l’extension de langage téléchargé (R-lang-extension.zip).


> [!NOTE]
>**R** est un mot réservé. Utilisez un nom différent pour la langue externe, par exemple « myR ».

```sql
CREATE EXTERNAL LANGUAGE [myR]
FROM (CONTENT = N'/path/to/R-lang-extension.zip', FILE_NAME = 'libRExtension.so.1.0');
GO
```

::: moniker-end

## <a name="enable-external-script-execution-in-sql-server"></a>Activer l’exécution de scripts externes dans SQL Server

Un script externe dans R peut être exécuté à l’aide de la procédure stockée [sp_execute_external script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) exécuté sur SQL Server. 

Pour activer les scripts externes, exécutez les commandes SQL suivantes à l’aide de [Azure Data Studio](https://docs.microsoft.com/sql/azure-data-studio/download-azure-data-studio), connecté à SQL Server.

```sql
sp_configure 'external scripts enabled', 1;
RECONFIGURE WITH OVERRIDE;
```

## <a name="verify-language-extension-installation"></a>Vérifiez l’installation de l’extension de langage

Ce script SQL vérifie la réussite de l’installation de l’extension de langage R personnalisée. La sortie de ce script doit afficher le R_HOME, le chemin d’accès à R et la version du CLR R personnalisé. Il confirme que le script utilise votre CLR personnalisé.

```sql
EXEC sp_execute_external_script
    @language =N'myR',
    @script=N'
print(R.home());
print(file.path(R.home("bin"), "R"));
print(R.version);
print("Hello RExtension!");'
```

## <a name="verify-parameters-and-datasets-of-different-data-types"></a>Vérifiez les paramètres et les jeux de données de différents types de données

Ce script teste différents types de données pour les paramètres et les jeux de données d’entrée/sortie.

```sql
DECLARE @sumVal INT = 12;
DECLARE @charVal VARCHAR(30) = N'Hello';
EXEC sp_execute_external_script
    @language =N'myR',
    @script=N'
print(sumVal);
print(charVal);
sumVal <- sumVal + 300;
OutputDataSet <- InputDataSet;'
    ,@input_data_1 = N'select 1, cast(1.4 as real), ''Hi'', cast(''1'' as bit)'
    ,@params = N'@sumVal int OUTPUT, @charVal VARCHAR(30)'
    ,@sumVal = @sumVal OUTPUT
    ,@charVal =  @charVal
WITH RESULT SETS ((intCol INT, doubleCol REAL, charCol CHAR(2), logicalCol BIT));
PRINT @sumVal;
```

## <a name="see-also"></a>Voir aussi

+ [Framework d’extensibilité dans SQL Server](../concepts/extensibility-framework.md)
+ [Vue d’ensemble des extensions de langage](../../language-extensions/language-extensions-overview.md)