---
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 02/08/2021
ms.topic: include
author: dphansen
ms.author: davidph
ms.openlocfilehash: 1303df98c60212c13a233d1e386c60109308e981
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100072871"
---
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

Définissez l’option `datadirectories` de la section extensibility du fichier `/var/opt/mssql/mssql.conf` avec l’installation personnalisée de Python.

```bash
sudo /opt/mssql/bin/mssql-conf set extensibility.datadirectories /path/to/installation/of/python3.7
```

### <a name="restart-mssql-launchpadd"></a>Redémarrez mssql-launchpadd

Exécutez la commande suivante pour redémarrer **mssql-launchpadd**.

```bash
sudo systemctl restart mssql-launchpadd
```

<a name="register-language-extension-linux"></a>

## <a name="register-language-extension"></a>Inscrivez l’extension de langage

Procédez comme suit pour télécharger et inscrire l’extension de langage Python, qui est utilisée pour le runtime personnalisé Python.

1. Téléchargez le fichier **python-lang-extension-linux-release.zip** à partir du [dépôt GitHub d’extensions de langage SQL Server](https://github.com/microsoft/sql-server-language-extensions/releases).

    Vous pouvez également utiliser la version debug (**python-lang-extension-linux-debug.zip**) dans un environnement de développement ou de test. La version debug fournit des informations de journalisation détaillées pour examiner les erreurs, et n’est pas recommandée pour les environnements de production.

1. Utilisez [Azure Data Studio](../../../azure-data-studio/what-is-azure-data-studio.md) pour vous connecter à votre instance SQL Server et exécutez la commande T-SQL suivante pour inscrire l’extension de langage Python avec [CREATE EXTERNAL LANGUAGE](../../../t-sql/statements/create-external-language-transact-sql.md). 

    Modifiez le chemin dans cette instruction pour refléter l’emplacement du fichier zip de l’extension de langage téléchargé (**python-lang-extension-linux-release.zip**).

    ```sql
    CREATE EXTERNAL LANGUAGE [myPython]
    FROM (CONTENT = N'/path/to/python-lang-extension-linux-release.zip', FILE_NAME = 'libPythonExtension.so.1.1');
    GO
    ```

    Exécutez l’instruction pour chaque base de données dans laquelle vous souhaitez utiliser l’extension de langage Python.

    > [!NOTE]
    > **Python** est un mot réservé et ne peut pas être utilisé comme nom pour un nouveau nom de langue externe. Utilisez un autre nom à la place. Par exemple, l’instruction ci-dessus utilise **myPython**.
