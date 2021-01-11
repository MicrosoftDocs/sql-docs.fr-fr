---
title: Installer le CLR personnalisé Python
description: Découvrez comment installer un runtime personnalisé Python pour SQL Server à l’aide des extensions de langage. Le runtime personnalisé Python peut être utilisé pour l’apprentissage automatique.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 11/30/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
zone_pivot_groups: python-custom-runtime-platform
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15'
ms.openlocfilehash: 91aace4333b4496338b782344e64cdfea2b886bd
ms.sourcegitcommit: 5b2c47ce88f7e56552fd415c32b319009d043b56
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/29/2020
ms.locfileid: "97804329"
---
# <a name="install-a-python-custom-runtime-for-sql-server"></a>Installer un CLR personnalisé Python pour SQL Server
[!INCLUDE [SQL Server 2019 and later](../../includes/applies-to-version/sqlserver2019.md)]

Cet article explique comment installer un runtime personnalisé Python pour l’exécution de scripts Python externes avec SQL Server. Le runtime personnalisé utilise les extensions de langage [SQL Server](../../language-extensions/language-extensions-overview.md) et peut être utilisé pour l’exécution de scripts d’apprentissage automatique.

Le runtime personnalisé Python vous permet d’utiliser votre propre version du runtime Python avec SQL Server, au lieu de la version du Runtime par défaut installée avec [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md).

::: zone pivot="python-custom-runtime-windows"

## <a name="prerequisites"></a>Prérequis

Avant d’installer un CLR personnalisé Python, installez les éléments suivants :

+ Installez la [mise à jour cumulative (CU) 3 ou version ultérieure](../../database-engine/install-windows/latest-updates-for-microsoft-sql-server.md) pour SQL Server 2019.

+ Installez [Python 3.7](https://www.python.org/downloads/) sur le serveur.

    L’extension de langage Python utilisée pour le runtime python personnalisé prend actuellement en charge Python 3.7 uniquement. Si vous souhaitez utiliser une autre version de Python, suivez les instructions du [référentiel GitHub de l’extension de langage Python](https://github.com/microsoft/sql-server-language-extensions/tree/master/language-extensions/python) pour modifier et régénérer l’extension.

    > [!IMPORTANT]
    > Pendant l’installation de Python, consultez **Ajouter Python 3.7 à PATH**.

## <a name="install-language-extensions"></a>Installer les extensions de langage

> [!NOTE]
> Si vous avez [Machine Learning Services](../sql-server-machine-learning-services.md) installé sur SQL Server 2019, les extensions de langage sont déjà installées et vous pouvez ignorer cette étape.

Suivez les étapes ci-dessous pour installer les [Extensions de langage SQL Server](../../language-extensions/language-extensions-overview.md), qui sont utilisées pour le runtime personnalisé Python.

1. Démarrez l’Assistant Installation de SQL Server 2019.
  
1. Sous l’onglet **Installation**, sélectionnez **Nouvelle installation autonome de SQL Server ou ajout de fonctionnalités à une installation existante**.

1. Dans la page **Sélection de fonctionnalités** , sélectionnez les options suivantes :
  
    + **Services Moteur de base de données**
  
        Pour utiliser les extensions de langage avec SQL Server, vous devez installer une instance du moteur de base de données. Vous pouvez utiliser une instance nouvelle ou existante.
  
    + **Machine Learning Services et extensions de langage**

        Sélectionnez **Machine Learning Services et extensions de langage**. Ne sélectionnez pas Python, car vous installerez le runtime python personnalisé ultérieurement.

        :::image type="content" source="media/2019-setup-language-extensions.png" alt-text="Installation des extensions de langage SQL Server 2019.":::

1. Dans la page **Prêt pour l’installation**, vérifiez que ces sélections sont incluses, puis choisissez **Installer**.
  
    + Services Moteur de base de données
    + Machine Learning Services et extensions de langage

1. Une fois l’installation terminée, redémarrez la machine si vous y êtes invité.

> [!IMPORTANT]
> Si vous installez une nouvelle instance de SQL Server 2019 avec les extensions de langage, installez la [mise à jour cumulative (CU) 3 ou une version ultérieure](../../database-engine/install-windows/latest-updates-for-microsoft-sql-server.md) avant de passer à l’étape suivante.

## <a name="install-pandas"></a>Installer pandas

Installez le package [pandas](https://pandas.pydata.org/) pour Python à partir d’une invite de commandes *élevée* :

```bash
python.exe -m pip install pandas
```

## <a name="add-environment-variable"></a>Ajouter une variable d’environnement

Ajoutez ou modifiez la variable d’environnement système **PYTHONHOME**.

1. Dans la zone de recherche Windows, tapez *environnement*, puis sélectionnez **Modifier les variables d’environnement système**.
1. Sous l’onglet **Avancé**, sélectionnez **Variables d’environnement**.
1. Sous **Variables système**, sélectionnez **Nouveau** pour créer **PYTHONHOME** afin de pointer vers votre emplacement d’installation de Python 3.7. Si PYTHONHOME existe déjà, sélectionnez **Modifier** pour le pointer vers l’emplacement d’installation Python 3.7.
1. Sélectionnez **OK** pour fermer toutes les fenêtres.

    :::image type="content" source="media/pythonhome-env-variable.png" alt-text="Variable d’environnement PYTHONHOME.":::

## <a name="grant-access-to-python-folder"></a>Accorder l’accès au dossier Python

Exécutez les commandes **icacls** suivantes à partir d’une nouvelle invite de commandes *élevée* pour accorder l’accès **READ & EXECUTE** à **PYTHONHOME** au **Service SQL Server Launchpad** et SID **S-1-15-2-1** (**ALL_APPLICATION_PACKAGES**).

1. Accordez des autorisations pour le **nom d’utilisateur du service SQL Server Launchpad**.

    ```cmd
    icacls "%PYTHONHOME%" /grant "NT Service\MSSQLLAUNCHPAD":(OI)(CI)RX /T
    ```

    Pour une instance nommée, la commande est `icacls "%PYTHONHOME%" /grant "NT Service\MSSQLLAUNCHPAD$SQL01":(OI)(CI)RX /T` pour une instance appelée **SQL01**.

2. Accordez des autorisations à **SID S-1-15-2-1**.

    ```cmd
    icacls "%PYTHONHOME%" /grant *S-1-15-2-1:(OI)(CI)RX /T
    ```

    La commande précédente accorde des autorisations au SID de l’ordinateur **S-1-15-2-1**, ce qui équivaut à **ALL APPLICATION PACKAGES** sur une version en anglais de Windows. Vous pouvez également utiliser `icacls "%R_HOME%" /grant "ALL APPLICATION PACKAGES":(OI)(CI)RX /T` sur une version en anglais de Windows.

## <a name="restart-sql-server-launchpad"></a>Redémarrer SQL Server Launchpad

Procédez comme suit pour redémarrer le service SQL Server Launchpad.

1. Ouvrez le [Gestionnaire de configuration SQL Server](../../relational-databases/sql-server-configuration-manager.md).

1. Sous **Services SQL Server**, cliquez avec le bouton droit sur **SQL Server Launchpad (MSSQLSERVER)** et sélectionnez **Redémarrer**. Si vous utilisez une instance nommée, le nom de l’instance s’affiche à la place de **(MSSQLSERVER)** .

## <a name="register-language-extension"></a>Inscrire l’extension de langage

Procédez comme suit pour télécharger et inscrire l’extension de langage Python, qui est utilisée pour le runtime personnalisé Python.

1. Téléchargez le fichier **python-lang-extension-windows.zip** à partir du [référentiel GitHub d’extensions de langage SQL Server](https://github.com/microsoft/sql-server-language-extensions/releases).

    Vous pouvez également utiliser la version debug (**python-lang-extension-windows-debug.zip**) dans un environnement de développement ou de test. La version debug fournit des informations de journalisation détaillées pour examiner les erreurs, et n’est pas recommandée pour les environnements de production.

1. Utilisez [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md) pour vous connecter à votre instance SQL Server et exécutez la commande T-SQL suivante pour inscrire l’extension de langage Python avec [CREATE EXTERNAL LANGUAGE](../../t-sql/statements/create-external-language-transact-sql.md). 

    Modifiez le chemin d’accès dans cette instruction pour refléter l’emplacement du fichier zip de l’extension de langage téléchargé (**python-lang-extension-windows.zip**).

    ```sql
    CREATE EXTERNAL LANGUAGE [myPython]
    FROM (CONTENT = N'/path/to/python-lang-extension-windows.zip', FILE_NAME = 'pythonextension.dll');
    GO
    ```

    Exécutez l’instruction pour chaque base de données dans laquelle vous souhaitez utiliser l’extension de langage Python.

    > [!NOTE]
    > **Python** est un mot réservé et ne peut pas être utilisé comme nom pour un nouveau nom de langue externe. Utilisez un autre nom à la place. Par exemple, l’instruction ci-dessus utilise **myPython**.

::: zone-end

::: zone pivot="python-custom-runtime-linux"

## <a name="prerequisites"></a>Prérequis

Avant d’installer un CLR personnalisé Python, installez les éléments suivants :

+ Installez SQL Server 2019 pour Linux. Vous pouvez installer SQL Server sur Red Hat Enterprise Linux (RHEL), SUSE Linux Enterprise Server (SLES) et Ubuntu. Pour plus d’informations, voir [Conseils d’installation pour SQL Server sur Linux](../../linux/sql-server-linux-setup.md).

+ Effectuez une mise à niveau vers la mise à jour cumulative (CU) 3 ou version ultérieure pour SQL Server 2019. Effectuez les étapes suivantes :
    1. Configurez les référentiels pour les mises à jour cumulatives. Pour plus d’informations, consultez [Configurer les référentiels pour l’installation et la mise à niveau de SQL Server sur Linux](../../linux/sql-server-linux-change-repo.md).

    1. Mettez à jour le package **mssql-server** vers la dernière mise à jour cumulative. Pour plus d’informations, consultez [la section Mettre à jour ou mettre à niveau SQL Server dans le Guide d’installation de SQL Server sur Linux](../../linux/sql-server-linux-setup.md#upgrade).

+ Installez [Python 3.7](https://www.python.org/downloads/) sur le serveur.

    L’extension de langage Python utilisée pour le runtime python personnalisé prend actuellement en charge Python 3.7 uniquement. Si vous souhaitez utiliser une autre version de Python, suivez les instructions du [référentiel GitHub de l’extension de langage Python](https://github.com/microsoft/sql-server-language-extensions/tree/master/language-extensions/python) pour modifier et régénérer l’extension.

## <a name="install-language-extensions"></a>Installer les extensions de langage

> [!NOTE]
> Si vous avez [Machine Learning Services](../sql-server-machine-learning-services.md) installé sur SQL Server 2019, le package **mssql-server-extensibility** pour les extensions de langage est déjà installée et vous pouvez ignorer cette étape.

Suivez les instructions ci-dessous pour installer les [extensions de langage SQL Server](../../language-extensions/language-extensions-overview.md) sur Linux, qui sont utilisées pour le runtime personnalisé Python.

#### <a name="ubuntu"></a>[Ubuntu](#tab/ubuntu)

1. Si possible, exécutez cette commande pour actualiser les packages sur le système avant l’installation.

    ```bash
    # Install as root or sudo
    sudo apt-get update
    ```

1. Ubuntu n’a peut-être pas l’option de transport https apt. Pour l’installer, exécutez la commande suivante.

    ```bash
    # Install as root or sudo
    apt-get install apt-transport-https
    ```

1. Installez **mssql-server-extensibility** à l’aide de cette commande.

    ```bash
    # Install as root or sudo
    sudo apt-get install mssql-server-extensibility
    ```

#### <a name="red-hat-enterprise-linux-rhel"></a>[Red Hat Enterprise Linux (RHEL)](#tab/rhel)

```bash
# Install as root or sudo
sudo yum install mssql-server-extensibility
```

#### <a name="suse-linux-enterprise-server-sles"></a>[SUSE Linux Enterprise Server (SLES)](#tab/sles)

```bash
# Install as root or sudo
sudo zypper install mssql-server-extensibility
```

---

## <a name="install-python-37-and-pandas"></a>Installez Python 3.7 et pandas

Installez Python 3,7, la bibliothèque libpython 3.7 et le package pandas. 

Voici des exemples de commandes pour Ubuntu :

```bash
# Install python3.7 and the corresponding library:
sudo add-apt-repository ppa:deadsnakes/ppa
sudo apt-get update
sudo apt-get install python3.7 python3-pip libpython3.7

# Install pandas to /usr/lib:
sudo python3.7 -m pip install pandas -t /usr/lib/python3.7/dist-packages
```

## <a name="custom-installation-of-python"></a>Installation personnalisée de Python

> [!NOTE]
> Si vous avez installé Python 3.7 à l’emplacement par défaut de `/usr/lib/python3.7`, vous pouvez ignorer cette section et passer à la section [Inscrire l’extension de langage](#register-language-extension-linux).

Si vous avez créé votre propre version de Python 3.7, utilisez les commandes suivantes pour faire savoir à SQL Server votre installation personnalisée.

### <a name="add-environment-variable"></a>Ajouter une variable d’environnement

Modifiez d’abord le service **mssql-launchpad** pour ajouter la variable d’environnement **PYTHONHOME** au fichier `/etc/systemd/system/mssql-launchpadd.service.d/override.conf`

1. Ouvrez le fichier avec systemctl

    ```bash
    sudo systemctl edit mssql-launchpadd
    ```

1. Insérez le texte suivant dans le fichier `/etc/systemd/system/mssql-launchpadd.service.d/override.conf` qui s’ouvre. Définissez la valeur de **PYTHONHOME** sur le chemin d’installation de Python personnalisé.

    ```
    [Service]
    Environment="PYTHONHOME=/path/to/installation/of/python3.7"
    ```

1. Enregistrez le fichier et fermez l’éditeur.

Ensuite, assurez-vous que `libpython3.7m.so.1.0` peut être chargé.

1. Créez un fichier custom-python.conf dans `/etc/ld.so.conf.d`.

    ```bash
    sudo vi /etc/ld.so.conf.d/custom-python.conf
    ```

1. Dans le fichier qui s’ouvre, ajoutez le chemin d’accès à **libpython3.7m.so.1.0** de l’installation Python personnalisée.

    ```
    /path/to/installation/of/python3.7/lib
    ```

1. Enregistrez le nouveau fichier et fermez l’éditeur.

1. Exécutez `ldconfig` et vérifiez que `libpython3.7m.so.1.0` peut être chargé en exécutant la commande suivante et en vérifiant que toutes les bibliothèques dépendantes peuvent être trouvées.

    ```bash
    sudo ldconfig
    ldd /path/to/installation/of/python3.7/lib/libpython3.7m.so.1.0
    ```

### <a name="grant-access-to-python-folder"></a>Accordez l’accès au dossier Python

Définissez l’option `datadirectories` de la section extensibilité du fichier /var/opt/mssql/mssql.conf sur l’installation personnalisée de Python.

```bash
sudo /opt/mssql/bin/mssql-conf set extensibility.datadirectories /path/to/installation/of/python3.7
```

### <a name="restart-mssql-launchpadd"></a>Redémarrez mssql-launchpadd

```bash
sudo systemctl restart mssql-launchpadd
```

<a name="register-language-extension-linux"></a>

## <a name="register-language-extension"></a>Inscrivez l’extension de langage

Procédez comme suit pour télécharger et inscrire l’extension de langage Python, qui est utilisée pour le runtime personnalisé Python.

1. Téléchargez le fichier **python-lang-extension-linux.zip** à partir du [référentiel GitHub d’extensions de langage SQL Server](https://github.com/microsoft/sql-server-language-extensions/releases).

    Vous pouvez également utiliser la version debug (**python-lang-extension-linux-debug.zip**) dans un environnement de développement ou de test. La version debug fournit des informations de journalisation détaillées pour examiner les erreurs, et n’est pas recommandée pour les environnements de production.

1. Utilisez [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md) pour vous connecter à votre instance SQL Server et exécutez la commande T-SQL suivante pour inscrire l’extension de langage Python avec [CREATE EXTERNAL LANGUAGE](../../t-sql/statements/create-external-language-transact-sql.md). 

    Modifiez le chemin d’accès dans cette instruction pour refléter l’emplacement du fichier zip de l’extension de langage téléchargé (**python-lang-extension-linux.zip**).

    ```sql
    CREATE EXTERNAL LANGUAGE [myPython]
    FROM (CONTENT = N'/path/to/python-lang-extension-linux.zip', FILE_NAME = 'libPythonExtension.so.1.0');
    GO
    ```

    Exécutez l’instruction pour chaque base de données dans laquelle vous souhaitez utiliser l’extension de langage Python.

    > [!NOTE]
    > **Python** est un mot réservé et ne peut pas être utilisé comme nom pour un nouveau nom de langue externe. Utilisez un autre nom à la place. Par exemple, l’instruction ci-dessus utilise **myPython**.

::: zone-end

## <a name="enable-external-script"></a>Activer le script externe

Vous pouvez exécuter un script externe Python avec la procédure stockée [sp_execute_external script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).

Pour activer les scripts externes, utilisez [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md) pour exécuter l’instruction ci-dessous.

```sql
sp_configure 'external scripts enabled', 1;
RECONFIGURE WITH OVERRIDE;  
```

## <a name="verify-installation"></a>Vérifier l'installation

Utilisez le script SQL suivant pour vérifier l’installation et les fonctionnalités du runtime personnalisé Python.

```sql
EXEC sp_execute_external_script
@language =N'myPython',
@script=N'
import sys
print(sys.path)
print(sys.version)
print(sys.executable)'
```

## <a name="next-steps"></a>Étapes suivantes

+ [Installer un CLR personnalisé R pour SQL Server](custom-runtime-r.md)
+ [Framework d’extensibilité dans SQL Server](../concepts/extensibility-framework.md)
+ [Vue d’ensemble des extensions de langage](../../language-extensions/language-extensions-overview.md)
