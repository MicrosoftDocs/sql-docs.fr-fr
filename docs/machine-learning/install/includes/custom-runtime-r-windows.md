---
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 02/08/2021
ms.topic: include
author: dphansen
ms.author: davidph
ms.openlocfilehash: 2a156ab122f0eadce71da9f4fcaf9e99584c8e32
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100072738"
---
## <a name="prerequisites"></a>Prérequis

Avant d’installer un CLR personnalisé R, installez les éléments suivants :

+ Si vous utilisez une instance existante de SQL Server, installez la [mise à jour cumulative (CU) 3 ou ultérieure](../../../database-engine/install-windows/latest-updates-for-microsoft-sql-server.md) pour SQL Server 2019.

## <a name="install-language-extensions"></a>Installer les extensions de langage

> [!NOTE]
> Si vous avez [Machine Learning Services](../../sql-server-machine-learning-services.md) installé sur SQL Server 2019, les extensions de langage sont déjà installées et vous pouvez ignorer cette étape.

Effectuez les étapes ci-dessous pour installer les [extensions de langage SQL Server](../../../language-extensions/language-extensions-overview.md), qui sont utilisées pour le runtime R personnalisé.

1. Démarrez l’Assistant Installation de SQL Server 2019.
  
1. Sous l’onglet **Installation**, sélectionnez **Nouvelle installation autonome de SQL Server ou ajout de fonctionnalités à une installation existante**.

1. Dans la page **Sélection de fonctionnalités** , sélectionnez les options suivantes :
  
    + **Services Moteur de base de données**
  
        Pour utiliser les extensions de langage avec SQL Server, vous devez installer une instance du moteur de base de données. Vous pouvez utiliser une instance nouvelle ou existante.
  
    + **Machine Learning Services et extensions de langage**

        Sélectionnez **Machine Learning Services et extensions de langage**. Ne sélectionnez pas R, car vous installerez le runtime R personnalisé plus tard.

        :::image type="content" source="../media/2019-setup-language-extensions.png" alt-text="Installation des extensions de langage SQL Server 2019.":::

1. Dans la page **Prêt pour l’installation**, vérifiez que ces sélections sont incluses, puis choisissez **Installer**.
  
    + Services Moteur de base de données
    + Machine Learning Services et extensions de langage

1. Une fois l’installation terminée, redémarrez la machine si vous y êtes invité.

> [!IMPORTANT]
> Si vous installez une nouvelle instance de SQL Server 2019 avec les extensions de langage, installez la [mise à jour cumulative (CU) 3 ou une version ultérieure](../../../database-engine/install-windows/latest-updates-for-microsoft-sql-server.md) avant de passer à l’étape suivante.

## <a name="install-r"></a>Installation de R

Téléchargez et installez la version de R que vous allez utiliser comme runtime personnalisé. Les versions 3.3 et ultérieures de R sont prises en charge.

1. Téléchargez [R version 3.3 ou ultérieure](https://cran.r-project.org/bin/windows/base/).

1. Exécutez le programme d’installation de R.

1. Notez le chemin d’installation de R. Par exemple, dans cet article, il s’agit de `C:\Program Files\R\R-4.0.3`.

    :::image type="content" source="../media/r-setup-path.png" alt-text="Installation de R":::

## <a name="update-system-environment-variable"></a>Mettre à jour les variables d’environnement système

Effectuez ces étapes pour modifier les variables d’environnement système **PATH**.

1. Dans la zone de recherche Windows, recherchez **Modifier les variables d’environnement système** et ouvrez le résultat.

1. Sous **Avancé**, sélectionnez **Variables d’environnement**.

1. Modifiez la variable d’environnement système **PATH**.

    Sélectionnez **PATH**, puis cliquez sur **Modifier**.

    Sélectionnez **Nouveau** et ajoutez le chemin au dossier `\bin\x64` dans le chemin d’installation de R. Par exemple : `C:\Program Files\R\R-4.0.3\bin\x64`.

## <a name="install-rcpp-package"></a>Installer le package Rcpp

Effectuez ces étapes pour installer le package **Rcpp**.

1. Démarrez une invite de commandes avec *élévation de privilèges* (Exécuter en tant qu’administrateur).

1. Exécutez R à partir de l’invite de commandes. Exécutez `\bin\R.exe` dans le dossier du chemin d’installation de R. Par exemple : `C:\Program Files\R\R-4.0.3\bin\R.exe`.

    ```CMD
    "C:\Program Files\R\R-4.0.3\bin\R.exe"
    ```

1. Exécutez le script suivant pour installer le package Rcpp dans le dossier `\library` du chemin d’installation de R. Par exemple : `C:\Program Files\R\R-4.0.3\library`.

    ```R
    install.packages("Rcpp", lib="C:\Program Files\R\R-4.0.3\library");
    ```

## <a name="grant-access-to-r-folder"></a>Accorder l’accès au dossier R

> [!NOTE]
> Si vous avez installé R dans l’emplacement par défaut `C:\Program Files\R\R-version` (par exemple, `C:\Program Files\R\R-4.0.3`), vous pouvez ignorer cette étape.

Exécutez les commandes **icacls** suivantes à partir d’une nouvelle invite de commandes *avec élévation de privilèges* pour accorder l’accès **READ & EXECUTE** au **nom d’utilisateur du service SQL Server Launchpad** et au SID **S-1-15-2-1** (**ALL APPLICATION PACKAGES**). Le nom d’utilisateur du service Launchpad se présente sous la forme `NT Service\MSSQLLAUNCHPAD$INSTANCENAME` où `INSTANCENAME` est le nom de l’instance de votre serveur SQL Server.

Les commandes accordent l’accès de manière récursive à tous les fichiers et dossiers sous le chemin d’un répertoire donné.

1. Accordez des autorisations au chemin d’installation de R au **nom d’utilisateur du service SQL Server Launchpad**. Par exemple : `C:\Program Files\R\R-4.0.3`.

    ```cmd
    icacls "C:\Program Files\R\R-4.0.3" /grant "NT Service\MSSQLLAUNCHPAD":(OI)(CI)RX /T
    ```

    Pour une instance nommée, la commande est `icacls "C:\Program Files\R\R-4.0.3" /grant "NT Service\MSSQLLAUNCHPAD$SQL01":(OI)(CI)RX /T` pour une instance appelée **SQL01**.

2. Accordez des autorisations au chemin d’installation de R au **SID S-1-15-2-1**. Par exemple : `C:\Program Files\R\R-4.0.3`.

    ```cmd
    icacls "C:\Program Files\R\R-4.0.3" /grant *S-1-15-2-1:(OI)(CI)RX /T
    ```

    La commande précédente accorde des autorisations au SID de l’ordinateur **S-1-15-2-1**, ce qui équivaut à **ALL APPLICATION PACKAGES** sur une version en anglais de Windows. Vous pouvez également utiliser `icacls "C:\Program Files\R\R-4.0.3" /grant "ALL APPLICATION PACKAGES":(OI)(CI)RX /T` sur une version en anglais de Windows.

## <a name="restart-sql-server-launchpad"></a>Redémarrer SQL Server Launchpad

Procédez comme suit pour redémarrer le service SQL Server Launchpad.

1. Ouvrez le [Gestionnaire de configuration SQL Server](../../../relational-databases/sql-server-configuration-manager.md).

1. Sous **Services SQL Server**, cliquez avec le bouton droit sur **SQL Server Launchpad (MSSQLSERVER)** et sélectionnez **Redémarrer**. Si vous utilisez une instance nommée, le nom de l’instance s’affiche à la place de **(MSSQLSERVER)** .

## <a name="register-language-extension"></a>Inscrire l’extension de langage

Effectuez ces étapes pour télécharger et inscrire l’extension de langage R, qui est utilisée pour le runtime R personnalisé.

1. Téléchargez le fichier **R-lang-extension-windows-release.zip** à partir du [dépôt GitHub d’extensions de langage SQL Server](https://github.com/microsoft/sql-server-language-extensions/releases).

    Vous pouvez également utiliser la version debug (**R-lang-extension-windows-debug.zip**) dans un environnement de développement ou de test. La version debug fournit des informations de journalisation détaillées pour examiner les erreurs, et n’est pas recommandée pour les environnements de production.

1. Utilisez [Azure Data Studio](../../../azure-data-studio/what-is-azure-data-studio.md) pour vous connecter à votre instance SQL Server et exécutez la commande T-SQL suivante pour inscrire l’extension de langage R avec [CREATE EXTERNAL LANGUAGE](../../../t-sql/statements/create-external-language-transact-sql.md).

    Modifiez le chemin dans cette instruction pour refléter l’emplacement du fichier zip de l’extension de langage téléchargé (**R-lang-extension-windows-release.zip**) et l’emplacement de votre installation de R (`C:\\Program Files\\R\\R-4.0.3`).

    ```sql
    CREATE EXTERNAL LANGUAGE [myR]
    FROM (CONTENT = N'C:\path\to\R-lang-extension-windows-release.zip', 
        FILE_NAME = 'libRExtension.dll',
        ENVIRONMENT_VARIABLES = N'{"R_HOME": "C:\\Program Files\\R\\R-4.0.3"}'););
    GO
    ```

    Exécutez l’instruction pour chaque base de données dans laquelle vous souhaitez utiliser l’extension de langage R.

    > [!NOTE]
    > **R** est un mot réservé et ne peut pas être utilisé comme nom pour un nouveau nom de langue externe. Utilisez un autre nom à la place. Par exemple, l’instruction ci-dessus utilise **myR**.
