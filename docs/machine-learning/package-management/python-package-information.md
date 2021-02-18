---
title: Obtenir des informations sur les packages Python
description: Découvrez comment obtenir des informations sur les packages Python ayant été installés, y compris les versions et les emplacements d’installation, sur SQL Server Machine Learning Services.
ms.custom: ''
ms.prod: sql
ms.technology: machine-learning
ms.date: 06/03/2020
ms.topic: how-to
author: garyericson
ms.author: garye
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 29ed535b25d364ffa552aba20b10b446e0561cf9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352525"
---
# <a name="get-python-package-information"></a>Obtenir des informations sur les packages Python

[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
Cet article explique comment obtenir des informations sur les packages Python installés, notamment les versions et les emplacements d’installation, sur [Machine Learning Services sur SQL Server](../sql-server-machine-learning-services.md) et sur [Clusters Big Data](../../big-data-cluster/machine-learning-services.md). Les exemples de scripts Python vous montrent comment répertorier des informations de package telles que le chemin d’installation et la version.
::: moniker-end
::: moniker range="=sql-server-2017"
Cet article explique comment obtenir des informations sur les packages Python installés, notamment les versions et les emplacements d’installation, sur [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md). Les exemples de scripts Python vous montrent comment répertorier des informations de package telles que le chemin d’installation et la version.
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
Cet article explique comment obtenir des informations sur les packages Python installés, notamment les versions et les emplacements d’installation, sur [Azure SQL Managed Instance Machine Learning Services](/azure/azure-sql/managed-instance/machine-learning-services-overview). Les exemples de scripts Python vous montrent comment répertorier des informations de package telles que le chemin d’installation et la version.
::: moniker-end

## <a name="default-python-library-location"></a>Emplacement par défaut de la bibliothèque Python

Lorsque vous installez Machine Learning avec SQL Server, une seule bibliothèque de packages est créée au niveau de l’instance pour chaque langage que vous installez. La bibliothèque d’instances est un dossier sécurisé enregistré avec SQL Server.

Tous les scripts ou les codes qui s’exécutent dans la base de données sur SQL Server doivent charger les fonctions à partir de la bibliothèque d’instances. SQL Server ne peut pas accéder aux packages installés dans d’autres bibliothèques. Cela s’applique également aux clients distants : tout code Python qui s’exécute dans le contexte de calcul du serveur peut uniquement utiliser des packages installés dans la bibliothèque d’instances.
Pour protéger les ressources du serveur, la bibliothèque d’instances par défaut ne peut être modifiée que par un administrateur de l’ordinateur.

::: moniker range="=sql-server-2017"
Le chemin d’accès par défaut aux éléments binaires pour Python est :

`C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\PYTHON_SERVICES`

Cela nécessite d’utiliser l’instance SQL par défaut, MSSQLSERVER. Si SQL Server est installé en tant qu’instance nommée définie par l’utilisateur, le nom donné est utilisé à la place.
::: moniker-end

::: moniker range=">=sql-server-ver15"
Le chemin d’accès par défaut aux éléments binaires pour Python est :

`C:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\PYTHON_SERVICES`

Cela nécessite d’utiliser l’instance SQL par défaut, MSSQLSERVER. Si SQL Server est installé en tant qu’instance nommée définie par l’utilisateur, le nom donné est utilisé à la place.
::: moniker-end

Activez les scripts externes en exécutant les commandes SQL suivantes :

```sql
sp_configure 'external scripts enabled', 1;
RECONFIGURE WITH override;
```

::: moniker range="=azuresqldb-mi-current"
> [!IMPORTANT]
> Sur Azure SQL Managed Instance, l’exécution des commandes sp_configure et RECONFIGURE déclenche un redémarrage de SQL Server pour que les paramètres RG prennent effet. Cela peut entraîner quelques secondes d’indisponibilité.
::: moniker-end

Exécutez l’instruction SQL suivante si vous souhaitez vérifier la bibliothèque par défaut de l’instance actuelle. Cet exemple renvoie la liste des dossiers inclus dans la variable `sys.path` Python. La liste comprend le répertoire actuel et le chemin d’accès de la bibliothèque standard.

```sql
EXECUTE sp_execute_external_script
  @language =N'Python',
  @script=N'import sys; print("\n".join(sys.path))'
```

Pour en savoir plus sur la variable `sys.path` et sur la façon dont elle est utilisée pour définir le chemin de recherche de l’interpréteur pour les modules, consultez la section [Chemin de recherche du module](https://docs.python.org/2/tutorial/modules.html#the-module-search-path).

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15||=azuresqldb-mi-current"
> [!NOTE]
> N’essayez pas d’installer directement des packages Python dans la bibliothèque de packages SQL à l’aide de **pip** ou de méthodes similaires. Utilisez plutôt **sqlmlutils** pour installer des packages dans une instance SQL. Pour plus d’informations, consultez [Installation de packages Python avec sqlmlutils](install-additional-python-packages-on-sql-server.md).
::: moniker-end

## <a name="default-microsoft-python-packages"></a>Packages Microsoft Python par défaut

Les packages Microsoft Python suivants sont installés avec SQL Server Machine Learning Services lorsque vous sélectionnez la fonctionnalité Python au moment de l’installation.

| . | Version |  Description |
| ---------|---------|--------------|
| [revoscalepy](/machine-learning-server/python-reference/revoscalepy/revoscalepy-package) | 9.4.7 | Utilisé pour les contextes de calcul distants, la diffusion en continu, l’exécution parallèle des fonctions rx pour l’importation et la transformation des données, la modélisation, la visualisation et l’analyse. |
| [microsoftml](/machine-learning-server/python-reference/microsoftml/microsoftml-package) | 9.4.7 | Ajoute des algorithmes d’apprentissage automatique dans Python. |

Pour plus d’informations sur la version de Python incluse, consultez les [Versions de Python et de R](../sql-server-machine-learning-services.md#versions).

### <a name="component-upgrades"></a>Mise à niveau des composants

Par défaut, les packages Python sont actualisés grâce aux Service Packs et aux mises à jour cumulatives. Les packages supplémentaires et les mises à niveau de version complètes des composants de base de Python ne sont possibles que par le biais de mises à niveau de produit ou par liaison de la prise en charge de Python avec Microsoft Machine Learning Server.

Pour plus d’informations, consultez la section [Mettre à niveau les composants R et Python dans SQL Server](../install/upgrade-r-and-python.md).

## <a name="default-open-source-python-packages"></a>Packages Python open source par défaut

Lorsque vous sélectionnez l’option de langage Python au cours de l’installation, la distribution Anaconda 4.2 (sur Python 3.5) est installée. En plus des bibliothèques de code Python, l’installation standard comprend des exemples de données, des tests unitaires et des exemples de scripts.

> [!IMPORTANT]
> Ne remplacez jamais manuellement la version de Python installée par le programme de SQL Server par des versions plus récentes obtenues sur le Web. Les packages Microsoft Python sont basés sur des versions spécifiques d’Anaconda. Vous risquez de les déstabiliser si vous modifiez votre installation.

## <a name="list-all-installed-python-packages"></a>Répertorier tous les packages Python ayant été installés

L’exemple de script suivant affiche une liste de tous les packages Python installés dans l’instance SQL Server.

```sql
EXECUTE sp_execute_external_script
  @language = N'Python',
  @script = N'
import pkg_resources
import pandas
OutputDataSet = pandas.DataFrame(sorted([(i.key, i.version) for i in pkg_resources.working_set]))'
WITH result sets((Package NVARCHAR(128), Version NVARCHAR(128)));
```

## <a name="find-a-single-python-package"></a>Rechercher un seul package Python

Si vous avez installé un package Python et que vous souhaitez vous assurer qu’il est disponible pour une instance SQL Server donnée, vous pouvez exécuter une procédure stockée pour rechercher le package et renvoyer des messages.

Par exemple, le code suivant recherche le package `scikit-learn`.
Si le package est trouvé, le code imprime la version du package.

```sql
EXECUTE sp_execute_external_script
  @language = N'Python',
  @script = N'
import pkg_resources
pkg_name = "scikit-learn"
try:
    version = pkg_resources.get_distribution(pkg_name).version
    print("Package " + pkg_name + " is version " + version)
except:
    print("Package " + pkg_name + " not found")
'
```

Résultat :

```text
STDOUT message(s) from external script: Package scikit-learn is version 0.20.2
```

<a name="bkmk_SQLPythonVersion"></a>
## <a name="view-the-version-of-python"></a>Affichage de la version de Python

L’exemple de code suivant retourne la version de Python installée sur l’instance de SQL Server.

```sql
EXECUTE sp_execute_external_script
  @language = N'Python',
  @script = N'
import sys
print(sys.version)
'
```

## <a name="next-steps"></a>Étapes suivantes

::: moniker range="=sql-server-2017"
+ [Installer des packages avec des outils Python](install-python-packages-standard-tools.md)
::: moniker-end
::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15||=azuresqldb-mi-current"
+ [Installer de nouveaux packages Python avec sqlmlutils](install-additional-r-packages-on-sql-server.md)
::: moniker-end