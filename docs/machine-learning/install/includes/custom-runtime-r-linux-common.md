---
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 02/08/2021
ms.topic: include
author: dphansen
ms.author: davidph
ms.openlocfilehash: 07bd68eaee05893174813ed43dc5358104016e4b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100072739"
---
## <a name="custom-installation-of-r"></a>Installation personnalisée de R

> [!NOTE]
> Si vous avez installé R dans l’emplacement par défaut **/usr/lib/R**, vous pouvez ignorer cette section et passer à la section [Installer le package Rcpp](#install-rcpp-package-linux).

### <a name="update-the-environment-variables"></a>Mettez à jour les variables d’environnement

Tout d’abord, modifiez le service **mssql-launchpadd** pour ajouter la variable d’environnement **R_HOME** au fichier `/etc/systemd/system/mssql-launchpadd.service.d/override.conf`.

1. Ouvrez le fichier avec systemctl

    ```bash
    sudo systemctl edit mssql-launchpadd
    ```

1. Insérez le texte suivant dans le fichier `/etc/systemd/system/mssql-launchpadd.service.d/override.conf` qui s’ouvre. Définissez la valeur de **R_HOME** avec le chemin d’installation personnalisé de R.

    ```text
    [Service]
    Environment="R_HOME=/path/to/installation/of/R"
    ```

1. Enregistrez et fermez.

Ensuite, assurez-vous que `libR.so` peut être chargé.

1. Créez un fichier custom-r.conf dans **/etc/ld.so.conf.d**.

    ```bash
    sudo vi /etc/ld.so.conf.d/custom-r.conf
    ```

1. Dans le fichier qui s’ouvre, ajoutez path à **libR.so** à partir de l’installation personnalisée de R.

    ```
    /path/to/installation/of/R/lib
    ```

1. Enregistrez le nouveau fichier et fermez l’éditeur.

1. Exécutez `ldconfig` et vérifiez que **libR.so** peuvent être chargés en exécutant la commande suivante et en vérifiant que toutes les bibliothèques dépendantes peuvent être trouvées.

    ```bash
    sudo ldconfig
    ldd /path/to/installation/of/R/lib/libR.so
    ```

### <a name="grant-access-to-the-custom-r-installation-folder"></a>Accorder l’accès au dossier d’installation de R personnalisé

Définissez l’option `datadirectories` de la section extensibility du fichier `/var/opt/mssql/mssql.conf` avec l’installation personnalisée de R.

```bash
sudo /opt/mssql/bin/mssql-conf set extensibility.datadirectories /path/to/installation/of/R
```

### <a name="restart-mssql-launchpadd-service"></a>Redémarrer le service mssql-Launchpad

Exécutez la commande suivante pour redémarrer **mssql-launchpadd**.

```bash
sudo systemctl restart mssql-launchpadd
```

<a name="install-rcpp-package-linux"></a>

## <a name="install-rcpp-package"></a>Installer le package Rcpp

Effectuez ces étapes pour installer le package **Rcpp**.

1. Démarrez R à partir d’un interpréteur de commandes :

    ```bash
    sudo ${R_HOME}/bin/R
    ```

1. Exécutez le script suivant pour installer le package Rcpp dans le dossier ${R_HOME}\library.

  ```R
  install.packages("Rcpp", lib = "${R_HOME}/library");
  ```

## <a name="register-language-extension"></a>Inscrivez l’extension de langage

Effectuez ces étapes pour télécharger et inscrire l’extension de langage R, qui est utilisée pour le runtime R personnalisé.

1. Téléchargez le fichier **R-lang-extension-linux-release.zip** à partir du [dépôt GitHub d’extensions de langage SQL Server](https://github.com/microsoft/sql-server-language-extensions/releases).

    Vous pouvez également utiliser la version debug (**R-lang-extension-linux-debug.zip**) dans un environnement de développement ou de test. La version debug fournit des informations de journalisation détaillées pour examiner les erreurs, et n’est pas recommandée pour les environnements de production.

1. Utilisez [Azure Data Studio](../../../azure-data-studio/what-is-azure-data-studio.md) pour vous connecter à votre instance SQL Server et exécutez la commande T-SQL suivante pour inscrire l’extension de langage R avec [CREATE EXTERNAL LANGUAGE](../../../t-sql/statements/create-external-language-transact-sql.md). 

    Modifiez le chemin dans cette instruction pour refléter l’emplacement du fichier zip de l’extension de langage téléchargé (**R-lang-extension-linux-release.zip**).

    ```sql
    CREATE EXTERNAL LANGUAGE [myR]
    FROM (CONTENT = N'/path/to/R-lang-extension-linux-release.zip', FILE_NAME = 'libRExtension.so.1.0');
    GO
    ```

    Exécutez l’instruction pour chaque base de données dans laquelle vous souhaitez utiliser l’extension de langage R.

    > [!NOTE]
    > **R** est un mot réservé et ne peut pas être utilisé comme nom pour un nouveau nom de langue externe. Utilisez un autre nom à la place. Par exemple, l’instruction ci-dessus utilise **myR**.
