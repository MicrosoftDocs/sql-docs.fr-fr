---
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 02/08/2021
ms.topic: include
author: dphansen
ms.author: davidph
ms.openlocfilehash: 9f58ddf6b58e32d40b2962b47255aba2e553acca
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100072959"
---
## <a name="prerequisites"></a>Prérequis

Avant d’installer un runtime Python personnalisé, installez ce qui suit :

+ Si vous utilisez une instance existante de SQL Server, installez la [mise à jour cumulative (CU) 3 ou ultérieure](../../../database-engine/install-windows/latest-updates-for-microsoft-sql-server.md) pour SQL Server 2019.

## <a name="install-language-extensions"></a>Installer les extensions de langage

> [!NOTE]
> Si vous avez [Machine Learning Services](../../sql-server-machine-learning-services.md) installé sur SQL Server 2019, les extensions de langage sont déjà installées et vous pouvez ignorer cette étape.

Suivez les étapes ci-dessous pour installer les [Extensions de langage SQL Server](../../../language-extensions/language-extensions-overview.md), qui sont utilisées pour le runtime personnalisé Python.

1. Démarrez l’Assistant Installation de SQL Server 2019.
  
1. Sous l’onglet **Installation**, sélectionnez **Nouvelle installation autonome de SQL Server ou ajout de fonctionnalités à une installation existante**.

1. Dans la page **Sélection de fonctionnalités** , sélectionnez les options suivantes :
  
    + **Services Moteur de base de données**
  
        Pour utiliser les extensions de langage avec SQL Server, vous devez installer une instance du moteur de base de données. Vous pouvez utiliser une instance nouvelle ou existante.
  
    + **Machine Learning Services et extensions de langage**

        Sélectionnez **Machine Learning Services et extensions de langage**. Ne sélectionnez pas Python, car vous installerez le runtime python personnalisé ultérieurement.

        :::image type="content" source="../media/2019-setup-language-extensions.png" alt-text="Installation des extensions de langage SQL Server 2019.":::

1. Dans la page **Prêt pour l’installation**, vérifiez que ces sélections sont incluses, puis choisissez **Installer**.
  
    + Services Moteur de base de données
    + Machine Learning Services et extensions de langage

1. Une fois l’installation terminée, redémarrez la machine si vous y êtes invité.

> [!IMPORTANT]
> Si vous installez une nouvelle instance de SQL Server 2019 avec les extensions de langage, installez la [mise à jour cumulative (CU) 3 ou une version ultérieure](../../../database-engine/install-windows/latest-updates-for-microsoft-sql-server.md) avant de passer à l’étape suivante.

## <a name="install-python"></a>Installer Python

L’extension de langage Python utilisée pour le runtime python personnalisé prend actuellement en charge Python 3.7 uniquement. Si vous souhaitez utiliser une autre version de Python, suivez les instructions du [référentiel GitHub de l’extension de langage Python](https://github.com/microsoft/sql-server-language-extensions/tree/master/language-extensions/python) pour modifier et régénérer l’extension.

1. Téléchargez [Python 3.7](https://www.python.org/downloads/windows/) pour Windows et exécutez le programme d’installation sur le serveur.

1. Sélectionnez **Add Python 3.7 to PATH** (Ajouter Python 3.7 à PATH), puis **Customize installation** (Personnaliser l’installation).

    :::image type="content" source="../media/python-install-add-to-path.png" alt-text="Installation de Python 3.7 - Ajouter Python 3.7 à PATH":::

1. Sous **Optional Features** (Fonctionnalités facultatives), laissez les valeurs par défaut et sélectionnez **Next** (Suivant).

1. Sélectionnez **Install for all users** (Installer pour tous les utilisateurs) et notez l’emplacement de l’installation.

    :::image type="content" source="../media/python-install-for-all-users.png" alt-text="Installation de Python 3.7 - Installer pour tous les utilisateurs":::

1. Sélectionnez **Installer**.

## <a name="install-pandas"></a>Installer pandas

Installez le package [pandas](https://pandas.pydata.org/) pour Python à partir d’une invite de commandes avec *élévation de privilèges* (Exécuter en tant qu’administrateur) :

```bash
python.exe -m pip install pandas
```

## <a name="grant-access-to-python-folder"></a>Accordez l’accès au dossier Python

Exécutez les commandes **icacls** suivantes à partir d’une nouvelle invite de commandes avec *élévation de privilèges* pour accorder l’accès **READ & EXECUTE** à l’emplacement d’installation de Python au **service SQL Server Launchpad** et au SID **S-1-15-2-1** (**ALL_APPLICATION_PACKAGES**).

Les exemples ci-dessous utilisent l’emplacement d’installation de Python suivant : `C:\Program Files\Python37`. Si votre emplacement est différent, modifiez-le dans la commande.

1. Accordez des autorisations pour le **nom d’utilisateur du service SQL Server Launchpad**.

    ```cmd
    icacls "C:\Program Files\Python37" /grant "NT Service\MSSQLLAUNCHPAD":(OI)(CI)RX /T
    ```

    Pour une instance nommée, la commande est `icacls "C:\Program Files\Python37" /grant "NT Service\MSSQLLAUNCHPAD$SQL01":(OI)(CI)RX /T` pour une instance appelée **SQL01**.

2. Accordez des autorisations à **SID S-1-15-2-1**.

    ```cmd
    icacls "C:\Program Files\Python37" /grant *S-1-15-2-1:(OI)(CI)RX /T
    ```

    La commande précédente accorde des autorisations au SID de l’ordinateur **S-1-15-2-1**, ce qui équivaut à **ALL APPLICATION PACKAGES** sur une version en anglais de Windows. Vous pouvez également utiliser `icacls "C:\Program Files\Python37" /grant "ALL APPLICATION PACKAGES":(OI)(CI)RX /T` sur une version en anglais de Windows.

## <a name="restart-sql-server-launchpad"></a>Redémarrer SQL Server Launchpad

Procédez comme suit pour redémarrer le service SQL Server Launchpad.

1. Ouvrez le [Gestionnaire de configuration SQL Server](../../../relational-databases/sql-server-configuration-manager.md).

1. Sous **Services SQL Server**, cliquez avec le bouton droit sur **SQL Server Launchpad (MSSQLSERVER)** et sélectionnez **Redémarrer**. Si vous utilisez une instance nommée, le nom de l’instance est indiqué à la place de **(MSSQLSERVER)** .

## <a name="register-language-extension"></a>Inscrivez l’extension de langage

Procédez comme suit pour télécharger et inscrire l’extension de langage Python, qui est utilisée pour le runtime personnalisé Python.

1. Téléchargez le fichier **python-lang-extension-windows-release.zip** à partir du [dépôt GitHub d’extensions de langage SQL Server](https://github.com/microsoft/sql-server-language-extensions/releases).

    Vous pouvez également utiliser la version debug (**python-lang-extension-windows-debug.zip**) dans un environnement de développement ou de test. La version debug fournit des informations de journalisation détaillées pour examiner les erreurs, et n’est pas recommandée pour les environnements de production.

1. Utilisez [Azure Data Studio](../../../azure-data-studio/what-is-azure-data-studio.md) pour vous connecter à votre instance SQL Server et exécutez la commande T-SQL suivante pour inscrire l’extension de langage Python avec [CREATE EXTERNAL LANGUAGE](../../../t-sql/statements/create-external-language-transact-sql.md).

    Modifiez le chemin dans cette instruction pour refléter l’emplacement du fichier zip de l’extension de langage téléchargé (**python-lang-extension-windows-release.zip**) et l’emplacement de votre installation de Python (`C:\\Program Files\\Python3.7`).

    ```sql
    CREATE EXTERNAL LANGUAGE [myPython]
    FROM (CONTENT = N'C:\path\to\python-lang-extension-windows-release.zip', 
        FILE_NAME = 'pythonextension.dll', 
        ENVIRONMENT_VARIABLES = N'{"PYTHONHOME": "C:\\Program Files\\Python3.7"}');
    GO
    ```

    Exécutez l’instruction pour chaque base de données dans laquelle vous souhaitez utiliser l’extension de langage Python.

    > [!NOTE]
    > **Python** est un mot réservé et ne peut pas être utilisé comme nom pour un nouveau nom de langue externe. Utilisez un autre nom à la place. Par exemple, l’instruction ci-dessus utilise **myPython**.
