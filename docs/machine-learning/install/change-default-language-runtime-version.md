---
title: Modifier la version du runtime de langage R ou Python par défaut
description: Découvrez comment modifier la version par défaut du runtime R ou Python utilisé par une instance SQL avec SQL Server 2017 Machine Learning Services ou SQL Server R Services.
ms.custom: ''
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 08/14/2020
ms.topic: how-to
author: garyericson
ms.author: garye
monikerRange: =sql-server-2016||=sql-server-2017
ms.openlocfilehash: eacae756f788a5cf42c19cae27a975f98111fb71
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100068928"
---
# <a name="change-the-default-r-or-python-language-runtime-version"></a>Modifier la version du runtime de langage R ou Python par défaut

[!INCLUDE[SQL Server 2016 and 2017 only](../../includes/applies-to-version/sqlserver2016-2017-only.md)]

Cet article explique comment modifier la version par défaut de R ou Python utilisée dans [SQL Server 2016 R Services](../r/sql-server-r-services.md) ou [SQL Server 2017 Machine Learning Services](../sql-server-machine-learning-services.md).

La liste suivante répertorie les versions de runtime R et Python incluses dans les différentes versions de SQL Server.

| Version de SQL Server | Service | Mise à jour cumulée | Versions du runtime R | Version du runtime Python |
|-|-|-|-|-|
| SQL Server 2016 | R Services | RTM - SP2 CU13 | 3.2.2 | Non disponible |
| SQL Server 2016 | R Services | SP2 CU14 et versions ultérieures | 3.2.2 et 3.5.2 | Non disponible |
| SQL Server 2017 | Machine Learning Services | RTM - CU21 | 3.3.3 | 3.5.2 |
| SQL Server 2017 | Machine Learning Services | CU22 et versions ultérieures | 3.3.3 et 3.5.2 | 3.5.2 et 3.7.2 |

## <a name="prerequisites"></a>Prérequis

Vous devez installer une mise à jour cumulative (CU) pour modifier la version par défaut du runtime du langage R ou Python :

- **SQL Server 2016 :** Service Pack (SP) 2 mise à jour cumulative (CU) 14 ou version ultérieure
- **SQL Server 2017 :** Mise à jour cumulative (CU) 22 ou version ultérieure

Pour télécharger la dernière mise à jour cumulative, consultez les [Dernières mises à jour pour Microsoft SQL Server](../../database-engine/install-windows/latest-updates-for-microsoft-sql-server.md).

> [!NOTE]
> Si vous intégrez la mise à jour cumulative à une nouvelle installation de SQL Server, seules les versions les plus récentes du runtime R et Python seront installées.

## <a name="change-r-runtime-version"></a>Modifier la version du runtime R

Si vous avez installé l’une des mises à jour cumulatives ci-dessus pour SQL Server 2016 ou 2017, vous pouvez avoir plusieurs versions de R dans une instance SQL. Chaque version se trouve dans un sous-dossier du dossier d’instance avec le nom `R_SERVICES.`*&lt;major&gt;* . *&lt;minor&gt;* (le dossier de l’installation d’origine ne possède peut-être pas de numéro de version ajouté au nom du dossier).

Si vous installez une CU contenant R 3.5, le nouveau dossier `R_SERVICES` est :

- SQL Server 2016 : `C:\Program Files\Microsoft SQL Server\MSSQL13.<INSTANCE_NAME>\R_SERVICES.3.5`
- SQL Server 2017 : `C:\Program Files\Microsoft SQL Server\MSSQL14.<INSTANCE_NAME>\R_SERVICES.3.5`

Chaque instance SQL utilise l’une de ces versions comme version par défaut de R. Vous pouvez modifier la version par défaut à l’aide de l’utilitaire de ligne de commande **RegisterRext.exe**. L’utilitaire se trouve sous le dossier R de chaque instance SQL :

*&lt;Chemin d’instance SQL&gt;* \R_SERVICES.n.n\library\RevoScaleR\rxLibs\x64\RegisterRext.exe

> [!Note]
> La fonctionnalité décrite dans cet article est disponible uniquement avec la copie de **RegisterRext.exe** incluse dans les CU SQL. N’utilisez pas la copie fournie avec l’installation SQL d’origine.

Pour modifier la version du runtime R, passez les arguments de ligne de commande suivants à **RegisterRext.exe** :

- `/configure` : obligatoire, spécifie que vous configurez la version R par défaut.

- `/instance:`*&lt;Nom de l’instance&gt;* : facultatif, l’instance que vous souhaitez configurer. S’il n’est pas spécifié, l’instance par défaut est configurée.

- `/rhome:`*&lt;chemin d’accès au dossier R_SERVICES[n.n]&gt;* : facultatif, chemin d’accès au dossier de la version du runtime que vous souhaitez définir comme version de R par défaut.

  Si vous ne spécifiez pas /rhome, le chemin d’accès configuré correspond au chemin sous lequel se trouve **RegisterRext.exe**.

### <a name="examples"></a>Exemples

Vous trouverez ci-dessous des exemples de modification de la version du runtime R dans SQL Server 2016 et 2017.

#### <a name="change-r-runtime-version-in-sql-server-2016"></a>Modifier la version du runtime R dans SQL Server 2016

Par exemple, pour configurer **R 3.5** comme version par défaut de R pour l’instance MSSQLSERVER01 sur SQL Server 2016 :

```cmd
cd "C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER01\R_SERVICES.3.5\library\RevoScaleR\rxLibs\x64"

.\RegisterRext.exe /configure /rhome:"C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER01\R_SERVICES.3.5" /instance:MSSQLSERVER01
```

#### <a name="change-r-runtime-version-in-sql-server-2017"></a>Modifier la version du runtime R dans SQL Server 2017

Par exemple, pour configurer **R 3.5** comme version par défaut de R pour l’instance MSSQLSERVER01 sur SQL Server 2017 :

```cmd
cd "C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER01\R_SERVICES.3.5\library\RevoScaleR\rxLibs\x64"

.\RegisterRext.exe /configure /rhome:"C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER01\R_SERVICES.3.5" /instance:MSSQLSERVER01
```

Dans ces exemples, vous n’avez pas besoin d’inclure l’argument `/rhome` dans la mesure où vous spécifiez le même dossier que celui où se trouve **RegisterRext.exe**.

## <a name="change-python-runtime-version"></a>Modifier la version du runtime Python

Si vous avez installé la CU22 ou une version ultérieure pour SQL Server 2017, vous pouvez avoir plusieurs versions de Python dans une instance SQL. Chaque version se trouve dans un sous-dossier du dossier d’instance avec le nom `PYTHON_SERVICES.`*&lt;major&gt;* . *&lt;minor&gt;* (le dossier de l’installation d’origine ne possède peut-être pas de numéro de version ajouté au nom du dossier).

Par exemple, si vous installez une CU contenant Python 3.7, un nouveau dossier `PYTHON_SERVICES` est créé :

`C:\Program Files\Microsoft SQL Server\MSSQL14.<INSTANCE_NAME>\PYTHON_SERVICES.3.7`  

Chaque instance SQL utilise une de ces versions comme version par défaut de Python. Vous pouvez modifier la version par défaut à l’aide de l’utilitaire de ligne de commande **RegisterRExt.exe**. L’utilitaire se trouve sous les dossiers Python de chaque instance SQL :

*&lt;Chemin de l’instance SQL&gt;* `\PYTHON_SERVICES.n.n\Lib\site-packages\revoscalepy\rxLibs\RegisterRExt.exe`

> [!Note]
> La fonctionnalité décrite dans cet article est disponible uniquement avec la copie de **RegisterRExt.exe** incluse dans les CU SQL. N’utilisez pas la copie fournie avec l’installation SQL d’origine.

Pour modifier la version du runtime Python, passez les arguments de ligne de commande suivants à **RegisterRext.exe** :

- `/configure` : obligatoire, spécifie que vous configurez la version Python par défaut.

- `/python` : spécifie que vous configurez la version Python par défaut. Facultatif si vous spécifiez `/pythonhome`.

- `/instance:`*&lt;Nom de l’instance&gt;* : facultatif, l’instance que vous souhaitez configurer. S’il n’est pas spécifié, l’instance par défaut est configurée.

- `/pythonhome:`*&lt;chemin d’accès au dossier PYTHON_SERVICES[n.n]&gt;* : facultatif, chemin d’accès au dossier de la version du runtime que vous souhaitez définir comme version de Python par défaut.

  Si vous ne spécifiez pas /pythonhome, le chemin d’accès configuré correspond au chemin sous lequel se trouve **RegisterRExt.exe**.

### <a name="example"></a> Exemple

Par exemple, pour configurer **Python 3.7** comme version par défaut de Python pour l’instance MSSQLSERVER01 sur SQL Server 2017 :

```cmd
cd "C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER01\PYTHON_SERVICES.3.7\Lib\site-packages\revoscalepy\rxLibs"

.\RegisterRext.exe /configure /pythonhome:"C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\PYTHON_SERVICES.3.7" /instance:MSSQLSERVER01
```

Dans cet exemple, vous n’avez pas besoin d’inclure l’argument `/pythonhome` dans la mesure où vous spécifiez le même dossier que celui où se trouve **RegisterRext.exe**.

## <a name="remove-a-runtime-version"></a>Supprimer une version du runtime

Pour supprimer une version de R ou Python, utilisez **RegisterRExt.exe** avec l’argument de ligne de commande `/cleanup`, en utilisant les mêmes arguments `/rhome`, `/pythonhome` et `/instance` que décrits précédemment.

Par exemple, pour supprimer le dossier **R 3.2** de l’instance MSSQLSERVER01 :

```cmd
.\RegisterRext.exe /cleanup /rhome:"C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER01\R_SERVICES" /instance:MSSQLSERVER01
```

Par exemple, pour supprimer le dossier **Python 3.7** de l’instance MSSQLSERVER01 :

```cmd
.\RegisterRExt.exe /cleanup /python /pythonhome:"C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER01\PYTHON_SERVICES.3.7" /instance:MSSQLSERVER01
```

**RegisterRext.exe** vous demande de confirmer le nettoyage du runtime R spécifié :

> *Voulez-vous vraiment définitivement supprimer le runtime donné en même temps que tous les packages installés sur celui-ci ? \[Oui (Y)/Non (N)/Par défaut (Oui)\] :*

Pour confirmer, répondez `Y` ou appuyez sur Entrée. Vous pouvez également ignorer cette invite en passant `/y` ou `/Yes` avec l’option `/cleanup`.

> [!NOTE]
> Vous ne pouvez supprimer une version que si elle n’est pas configurée en tant que version par défaut et qu’elle n’est pas utilisée actuellement pour exécuter **RegisterRext.exe**.

## <a name="next-steps"></a>Étapes suivantes

- [Obtenir des informations sur les packages R](../package-management/r-package-information.md)
- [Obtenir des informations sur les packages Python](../package-management/python-package-information.md)
- [Installer des packages avec les outils R](../package-management/install-r-packages-standard-tools.md)
- [Installer des packages avec des outils Python](../package-management/install-python-packages-standard-tools.md)
