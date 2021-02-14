---
title: Installer des packages avec des outils Python
description: Découvrez comment utiliser des outils Python standard pour installer de nouveaux packages Python sur une instance de Machine Learning Services de SQL Server.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 01/21/2020
ms.topic: how-to
author: garyericson
ms.author: garye
monikerRange: =sql-server-2017
ms.openlocfilehash: 37d7f95f4140e0cab778ac4864757fbe75b1ff0c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100082630"
---
# <a name="install-packages-with-python-tools-on-sql-server"></a>Installer des packages avec des outils Python sur SQL Server
[!INCLUDE [SQL Server 2017 only](../../includes/applies-to-version/sqlserver2017-only.md)]

Cet article décrit comment utiliser des outils Python standard pour installer de nouveaux packages Python sur une instance de Machine Learning Services de SQL Server. En général, le processus d’installation de nouveaux packages est semblable à celui dans un environnement Python standard. Toutefois, certaines étapes supplémentaires sont requises si le serveur n’a pas de connexion Internet.

Pour plus d’informations sur l’emplacement des packages et les chemins d’installation, consultez [Obtenir des informations sur les packages Python](python-package-information.md).

## <a name="prerequisites"></a>Conditions préalables requises

+ [SQL Server Machine Learning Services](../install/sql-machine-learning-services-windows-install.md) doit être installé avec l’option de langage Python.

### <a name="other-considerations"></a>Autres considérations

+ Les packages doivent être conformes à Python 3.5 et s’exécuter sur Windows.

+ La bibliothèque de packages Python se trouve dans le dossier Program Files de votre instance SQL Server et, par défaut, l’installation dans ce dossier requiert des autorisations d’administrateur. Pour plus d’informations, consultez [Emplacement de la bibliothèque de packages](../package-management/python-package-information.md#default-python-library-location).

+ L’installation du package se fait par instance. Si vous avez plusieurs instances de Machine Learning Services, vous devez ajouter le package à chacune d’entre elles.

+ Les serveurs de base de données sont souvent verrouillés. Dans de nombreux cas, l’accès à Internet est entièrement bloqué. Pour les packages présentant une longue liste de dépendances, vous devez identifier ces dépendances à l’avance et être prêt à les installer manuellement.

+ Avant d’ajouter un package, déterminez si le package est adapté à l’environnement SQL Server.

  + Nous vous recommandons d’utiliser Python dans la base de données pour les tâches qui bénéficient d’une intégration étroite avec le moteur de base de données, par exemple l’apprentissage automatique, plutôt que pour les tâches qui interrogent simplement la base de données.

  + Si vous ajoutez des packages qui placent trop de pression de calcul sur le serveur, les performances en seront affectées.

  + Dans un environnement SQL Server renforcé, vous souhaiterez peut-être éviter ce qui suit :
    + Les packages qui nécessitent un accès réseau
    + Les packages qui nécessitent un accès au système de fichiers élevé
    + Les packages utilisés pour le développement Web ou d’autres tâches qui ne bénéficient pas de l’exécution dans SQL Server

## <a name="add-a-python-package-on-sql-server"></a>Ajouter un package Python sur SQL Server

Pour installer un nouveau package Python qui peut être utilisé dans un script sur SQL Server, installez le package dans l’instance de Machine Learning Services. Si vous avez plusieurs instances de Machine Learning Services, vous devez ajouter le package à chacune d’entre elles.

Le package installé dans les exemples suivants est [CNTK](/cognitive-toolkit/), une infrastructure d’apprentissage approfondi de Microsoft qui prend en charge la personnalisation, la formation et le partage de différents types de réseaux neuraux.

### <a name="for-offline-install-download-the-python-package"></a>Pour une installation hors connexion, téléchargez le package Python

Si vous installez des packages Python sur un serveur sans accès à Internet, vous devez télécharger le fichier WHL à partir d’un ordinateur disposant d’un accès à Internet, puis copier le fichier sur le serveur.

Par exemple, sur un ordinateur connecté à Internet, vous pouvez télécharger le fichier `cntk-2.1-cp35-cp35m-win_amd64.whl` à partir du site [https://cntk.ai/PythonWheel/CPU-Only](https://cntk.ai/PythonWheel/CPU-Only/cntk-2.1-cp35-cp35m-win_amd64.whl), puis le copier dans un dossier local sur l’ordinateur SQL Server.

> [!IMPORTANT]
> Assurez-vous que vous disposez de la version Windows du package. Si le fichier se termine par .gz, il ne s’agit probablement pas de la bonne version.

Pour plus d’informations sur les téléchargements de l’infrastructure CNTK pour plusieurs plateformes et pour plusieurs versions de Python, consultez [Configurer CNTK sur votre ordinateur](/cognitive-toolkit/Setup-CNTK-on-your-machine).

### <a name="locate-the-python-library"></a>Localiser la bibliothèque Python

Localisez l’emplacement de la bibliothèque Python par défaut utilisé par SQL Server. Si vous avez installé plusieurs instances, recherchez le dossier `PYTHON_SERVICES` de l’instance pour laquelle vous souhaitez ajouter le package.

Par exemple, si Machine Learning Services a été installé à l’aide des valeurs par défaut et Machine Learning a été activé sur l’instance par défaut, le chemin d’accès est le suivant :

```console
cd "C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\PYTHON_SERVICES"
```

> [!TIP]
> Pour un débogage et des tests ultérieurs, vous souhaiterez peut-être configurer un environnement Python spécifique à la bibliothèque d’instances.

### <a name="install-the-package-using-pip"></a>Installer le package en utilisant PIP

Utilisez le programme d’installation **PIP** pour installer de nouveaux packages. Vous pouvez trouver `pip.exe` dans le sous-dossier `Scripts` du dossier `PYTHON_SERVICES`. La configuration SQL Server n’ajoute pas le sous-dossier `Scripts` au chemin d’accès système, vous devez donc spécifier le chemin d’accès complet ou ajouter le dossier Scripts à la variable PATH dans Windows.

> [!NOTE]
> Si vous utilisez Visual Studio 2017 ou Visual Studio 2015 avec les extensions Python, vous pouvez exécuter `pip install` à partir de la fenêtre des **Environnements Python**. Cliquez sur **Packages**, puis dans la zone de texte, indiquez le nom ou l’emplacement du package à installer. Vous n’avez pas besoin de saisir `pip install` ; il est renseigné automatiquement pour vous.

+ Si l’ordinateur a accès à Internet, indiquez le nom du package :

  ```console
  scripts\pip.exe install cntk
  ```
  Vous pouvez également spécifier l’URL d’un package et d’une version spécifiques, par exemple :

  ```console
  scripts\pip.exe install https://cntk.ai/PythonWheel/CPU-Only/cntk-2.1-cp35-cp35m-win_amd64.whl
  ```

+ Si l’ordinateur n’a pas accès à Internet, spécifiez le fichier WHL que vous avez téléchargé précédemment. Par exemple :

  ```console
  scripts\pip.exe install C:\Downloads\cntk-2.1-cp35-cp35m-win_amd64.whl
  ```

Vous pouvez être invité à élever les autorisations pour terminer l’installation.
À l’avancement de l’installation, vous pouvez voir les messages d’état dans la fenêtre d’invite de commandes.

### <a name="load-the-package-or-its-functions-as-part-of-your-script"></a>Charger le package ou ses fonctions dans le cadre de votre script

Une fois l’installation terminée, vous pouvez commencer immédiatement à utiliser le package dans les scripts Python dans SQL Server.

Pour utiliser les fonctions du package dans votre script, insérez l’instruction standard `import <package_name>` dans les lignes initiales du script :

```sql
EXECUTE sp_execute_external_script 
  @language = N'Python', 
  @script = N'
import cntk
# Python statements ...
'
```

## <a name="see-also"></a>Voir aussi

+ [Obtenir des informations sur les packages Python](python-package-information.md)
+ [Tutoriels Python pour SQL Server Machine Learning Services](../tutorials/python-tutorials.md)
+ [API Python pour CNTK](https://cntk.ai/pythondocs/tutorials.html).