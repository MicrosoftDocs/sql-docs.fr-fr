---
title: Installer des packages Python avec sqlmlutils
description: Découvrez comment utiliser le PIP Python pour installer de nouveaux packages Python sur une instance de SQL Server Machine Learning Services.
ms.prod: sql
ms.technology: machine-learning
ms.date: 08/26/2020
ms.topic: how-to
author: garyericson
ms.author: garye
ms.reviewer: davidph
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15||=azuresqldb-mi-current||=sqlallproducts-allversions'
ms.openlocfilehash: a387e10afc9210fd90248fb0240e3b77c37b2afd
ms.sourcegitcommit: 9be0047805ff14e26710cfbc6e10d6d6809e8b2c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89042526"
---
# <a name="install-python-packages-with-sqlmlutils"></a>Installer des packages Python avec sqlmlutils

[!INCLUDE [SQL Server 2019 SQL MI](../../includes/applies-to-version/sqlserver2019-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15||=sqlallproducts-allversions"
Cet article explique comment utiliser les fonctions du package [**sqlmlutils**](https://github.com/Microsoft/sqlmlutils) pour installer de nouveaux packages Python sur une instance de [Machine Learning Services sur SQL Server](../sql-server-machine-learning-services.md) et sur [Clusters Big Data](../../big-data-cluster/machine-learning-services.md). Les packages que vous installez peuvent être utilisés dans des scripts Python exécutés dans la base de données à l’aide de l’instruction T-SQL [sp_execute_external_script](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql).
::: moniker-end

::: moniker range="=azuresqldb-mi-current||=sqlallproducts-allversions"
Cet article explique comment utiliser les fonctions du package [**sqlmlutils**](https://github.com/Microsoft/sqlmlutils) pour installer de nouveaux packages Python sur une instance [d’Azure SQL Managed Instance Machine Learning Services](/azure/azure-sql/managed-instance/machine-learning-services-overview). Les packages que vous installez peuvent être utilisés dans des scripts Python exécutés dans la base de données à l’aide de l’instruction T-SQL [sp_execute_external_script](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql).
::: moniker-end

Pour plus d’informations sur l’emplacement des packages et les chemins d’installation, consultez [Obtenir des informations sur les packages Python](../package-management/python-package-information.md).

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15||=sqlallproducts-allversions"
> [!NOTE]
> Le package **sqlmlutils** décrit dans cet article est utilisé pour ajouter des packages Python à SQL Server 2019 ou ultérieur. Pour SQL Server 2017 et antérieur, consultez [Installer des packages avec des outils Python](https://docs.microsoft.com/sql/machine-learning/package-management/install-python-packages-standard-tools?view=sql-server-2017).
::: moniker-end

## <a name="prerequisites"></a>Prérequis

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15||=sqlallproducts-allversions"
+ [SQL Server Machine Learning Services](../install/sql-machine-learning-services-windows-install.md) doit être installé avec l’option de langage Python.
::: moniker-end

+ Installez [Azure Data Studio](https://docs.microsoft.com/sql/azure-data-studio/what-is) sur l’ordinateur client que vous utilisez pour vous connecter à SQL Server. Vous pouvez utiliser d’autres outils de gestion de base de données ou de requête, mais cet article part du principe que vous utilisez Azure Data Studio.

+ Installez le noyau Python dans Azure Data Studio. Vous pouvez également installer et utiliser Python en ligne de commande. Il peut par ailleurs être judicieux de disposer d’un environnement de développement Python, par exemple, [Visual Studio Code](https://code.visualstudio.com/download) avec [l’extension Python](https://marketplace.visualstudio.com/items?itemName=ms-python.python).

### <a name="other-considerations"></a>Autres considérations

+ Les packages doivent être conformes à la version de Python que vous possédez. Par ailleurs, la version de Python sur le serveur doit correspondre à celle de l’ordinateur client. Pour plus d’informations sur la version de Python incluse avec chaque version de SQL Server, consultez les [Versions de Python et de R](../sql-server-machine-learning-services.md#versions). Pour confirmer la version de Python dans une instance SQL en particulier, consultez [Afficher la version de Python](python-package-information.md#bkmk_SQLPythonVersion).

+ La bibliothèque de packages Python se trouve dans le dossier Program Files de votre instance SQL Server et, par défaut, l’installation dans ce dossier requiert des autorisations d’administrateur. Pour plus d’informations, consultez [Emplacement de la bibliothèque de packages](../package-management/python-package-information.md#default-python-library-location).

+ L’installation du package est spécifique à l’instance SQL, à la base de données et à l’utilisateur que vous spécifiez dans les informations de connexion que vous fournissez à **sqlmlutils**. Pour utiliser le package dans plusieurs instances ou bases de données SQL, ou pour différents utilisateurs, vous devez installer le package pour chacun d’entre eux. L’exception est que si le package est installé par un membre de `dbo`, le package est *public* et est partagé avec tous les utilisateurs. Si un utilisateur installe une version plus récente d’un package public, le package public n’est pas affecté, mais cet utilisateur aura accès à la version plus récente.

+ Avant d’ajouter un package, déterminez si le package est adapté à l’environnement SQL Server.

  + Nous vous recommandons d’utiliser Python dans la base de données pour les tâches qui bénéficient d’une intégration étroite avec le moteur de base de données, par exemple l’apprentissage automatique, plutôt que pour les tâches qui interrogent simplement la base de données.

  + Si vous ajoutez des packages qui placent trop de pression de calcul sur le serveur, les performances en seront affectées.

  + Dans un environnement SQL Server renforcé, vous souhaiterez peut-être éviter ce qui suit :
    + Les packages qui nécessitent un accès réseau
    + Les packages qui nécessitent un accès au système de fichiers élevé
    + Les packages utilisés pour le développement Web ou d’autres tâches qui ne bénéficient pas de l’exécution dans SQL Server

  + Il n’est pas possible d’installer le package Python **TensorFlow** à l’aide de sqlmlutils. Pour plus d’informations ainsi qu’une solution de contournement, consultez [Problèmes connus dans SQL Server Machine Learning Services](../troubleshooting/known-issues-for-sql-server-machine-learning-services.md#9-cannot-install-tensorflow-package-using-sqlmlutils).

## <a name="install-sqlmlutils-on-the-client-computer"></a>Installer sqlmlutils sur l’ordinateur client

Pour utiliser **sqlmlutils**, vous devez d’abord l’installer sur l’ordinateur client que vous utilisez pour vous connecter à SQL Server.

### <a name="in-azure-data-studio"></a>Dans Azure Data Studio

Si vous utilisez **sqlmlutils** dans Azure Data Studio, vous pouvez l’installer à l’aide de la fonctionnalité Gérer les packages dans un notebook de noyau Python.

1. Dans un [notebook de noyau Python dans Azure Data Studio](../../azure-data-studio/notebooks-tutorial-python-kernel.md), cliquez sur **Gérer les packages**.
1. Cliquez sur **Ajouter nouveau**.
1. Entrez « sqlmlutils » dans le champ **Recherche de packages Pip**, puis cliquez sur **Rechercher**.
1. Sélectionnez la **Version du package** que vous voulez installer (la dernière version est recommandée).
1. Cliquez sur **Installer**, puis sur **Fermer**.

### <a name="from-python-command-line"></a>En ligne de commande Python

Si vous utilisez **sqlmlutils** à partir d’une invite de commandes Python ou d’un environnement de développement intégré, vous pouvez installer sqlmlutils avec une simple commande **pip** :

```console
pip install sqlmlutils
```

Il est également possible d’installer **sqlmlutils** à partir d’un fichier zip :

1. Vérifiez que **pip** est installé. Pour plus d’informations, consultez [Installation de pip](https://pip.pypa.io/en/stable/installing/).
1. Téléchargez le dernier fichier zip **sqlmlutils** à partir de https://github.com/Microsoft/sqlmlutils/tree/master/Python/dist sur l’ordinateur client. Ne décompressez pas le fichier.
1. Ouvrez une **invite de commandes** et exécutez les commandes suivantes pour installer le package **sqlmlutils**. Remplacez le chemin d’accès complet par le fichier zip **sqlmlutils** que vous avez téléchargé (cet exemple suppose que le fichier téléchargé est `c:\temp\sqlmlutils-1.0.0.zip`).
   ```console
   pip install --upgrade --upgrade-strategy only-if-needed c:\temp\sqlmlutils-1.0.0.zip
   ```

## <a name="add-a-python-package-on-sql-server"></a>Ajouter un package Python sur SQL Server

**sqlmlutils** permet d’ajouter des packages Python à une instance SQL. Vous pouvez ensuite les utiliser dans votre code Python qui s’exécute dans l’instance SQL. **sqlmlutils** utilise [CREATE EXTERNAL LIBRARY](../../t-sql/statements/create-external-library-transact-sql.md) pour installer le package et chacune de ses dépendances.

Dans l’exemple suivant, vous allez ajouter le package [text-tools](https://pypi.org/project/text-tools/) à SQL Server.

### <a name="add-the-package-online"></a>Ajouter le package en ligne

Si l’ordinateur client que vous utilisez pour vous connecter à SQL Server a accès à Internet, vous pouvez utiliser **sqlmlutils** pour rechercher le package **text-tools** et toutes les dépendances sur Internet, puis installer le package sur une instance SQL Server distante.

::: moniker range=">=sql-server-ver15||=sqlallproducts-allversions"

1. Sur l’ordinateur client, ouvrez **Python** ou un environnement Python.

1. Utilisez les commandes suivantes pour installer le package **text-tools**. Remplacez vos propres informations de connexion de base de données SQL Server (si vous utilisez l’authentification Windows, vous n’avez pas besoin des paramètres `uid` et `pwd`).

::: moniker-end

::: moniker range=">=sql-server-linux-ver15||=azuresqldb-mi-current||=sqlallproducts-allversions"

1. Sur l’ordinateur client, ouvrez **Python** ou un environnement Python.

1. Utilisez les commandes suivantes pour installer le package **text-tools**. Remplacez les valeurs existantes par vos propres informations de connexion à la base de données SQL Server.

::: moniker-end

   ```python
   import sqlmlutils
   connection = sqlmlutils.ConnectionInfo(server="server", database="database", uid="username", pwd="password")
   sqlmlutils.SQLPackageManager(connection).install("text-tools")
   ```

### <a name="add-the-package-offline"></a>Ajouter le package hors connexion

Si l’ordinateur client que vous utilisez pour vous connecter à SQL Server n’est pas connecté à Internet, vous pouvez utiliser **pip** sur un ordinateur disposant d’un accès à Internet pour télécharger le package et tous les packages dépendants dans un dossier local. Vous copiez ensuite le dossier sur l’ordinateur client sur lequel vous pouvez installer le package hors connexion.

#### <a name="on-a-computer-with-internet-access"></a>Sur un ordinateur connecté à Internet

1. Ouvrez une **invite de commandes** et exécutez la commande suivante pour créer un dossier local qui contient le package **text-tools**. Cet exemple crée le dossier `c:\temp\text-tools`.

   ```console
   pip download text-tools -d c:\temp\text-tools
   ```

1. Copiez le dossier `text-tools` sur l’ordinateur client. L’exemple suivant suppose que vous l’avez copié dans `c:\temp\packages\text-tools`.

#### <a name="on-the-client-computer"></a>Sur l’ordinateur client

Utilisez **sqlmlutils** pour installer chaque package (fichier WHL) que vous trouvez dans le dossier local créé par **pip**. Peu importe l’ordre dans lequel vous installez les packages.

Dans cet exemple, **text-tools** n’a pas de dépendances. Il n’y a donc qu’un seul fichier du dossier `text-tools` que vous pouvez installer. En revanche, un package tel que **scikit-plot** a 11 dépendances. Par conséquent, vous trouverez 12 fichiers dans le dossier (le package **scikit-plot** et les 11 packages dépendants) et vous devez tous les installer.

::: moniker range=">=sql-server-ver15||=sqlallproducts-allversions"

Exécutez le script Python suivant. Remplacez le chemin du fichier, le nom du package et vos propres informations de connexion de base de données SQL Server (si vous utilisez l’authentification Windows, vous n’avez pas besoin des paramètres `uid` et `pwd`). Répétez l’instruction `sqlmlutils.SQLPackageManager` pour chaque fichier de package dans le dossier.

::: moniker-end

::: moniker range=">=sql-server-linux-ver15||=sqlallproducts-allversions"

Exécutez le script Python suivant. Remplacez le chemin du fichier, le nom du package et vos propres informations de connexion de base de données SQL Server. Répétez l’instruction `sqlmlutils.SQLPackageManager` pour chaque fichier de package dans le dossier.

::: moniker-end

```python
import sqlmlutils
connection = sqlmlutils.ConnectionInfo(server="yourserver", database="yourdatabase", uid="username", pwd="password"))
sqlmlutils.SQLPackageManager(connection).install("text_tools-1.0.0-py3-none-any.whl")
```

## <a name="use-the-package"></a>Utiliser le package

Vous pouvez maintenant utiliser le package dans un script Python dans SQL Server. Par exemple :

```sql
EXECUTE sp_execute_external_script
  @language = N'Python',
  @script = N'
from text_tools.finders import find_best_string
corpus = "Lorem Ipsum text"
query = "Ipsum"
first_match = find_best_string(query, corpus)
print(first_match)
  '
```

## <a name="remove-the-package-from-sql-server"></a>Supprimer le package de SQL Server

Si vous souhaitez supprimer le package **text-tools**, utilisez la commande Python suivante sur l’ordinateur client, en utilisant la même variable de connexion que celle que vous avez définie précédemment.

```python
sqlmlutils.SQLPackageManager(connection).uninstall("text-tools")
```

## <a name="next-steps"></a>Étapes suivantes

+ Pour des informations sur les packages Python installés dans SQL Server Machine Learning Services, consultez [Obtenir des informations sur les packages Python](../package-management/python-package-information.md).

+ Pour plus d’informations sur l’installation de packages R dans SQL Server Machine Learning Services, consultez [Installer de nouveaux packages R sur SQL Server](install-additional-r-packages-on-sql-server.md).
