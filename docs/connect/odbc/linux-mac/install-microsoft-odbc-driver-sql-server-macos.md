---
title: Installer Microsoft ODBC Driver for SQL Server (macOS)
description: Découvrez comment installer Microsoft ODBC Driver for SQL Server sur des clients macOS pour permettre la connectivité de base de données.
ms.date: 02/12/2021
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- driver, installing
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 2f07e613a6bd5ce8416c4b2c015749a1e631ea3f
ms.sourcegitcommit: c83c17e44b5e1e3e2a3b5933c2a1c4afb98eb772
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/15/2021
ms.locfileid: "100525143"
---
# <a name="install-the-microsoft-odbc-driver-for-sql-server-macos"></a>Installer Microsoft ODBC Driver for SQL Server (macOS)

Cet article explique comment installer Microsoft ODBC Driver for SQL Server sur macOS. Il contient également des instructions pour les outils en ligne de commande facultatifs pour SQL Server (`bcp` et `sqlcmd`) et les en-têtes de développement unixODBC.

Cet article fournit des commandes pour installer le pilote ODBC à partir de l’interpréteur de commandes bash. Si vous souhaitez télécharger les packages directement, consultez [Télécharger ODBC Driver for SQL Server](../download-odbc-driver-for-sql-server.md).

> [!Note]
> Le pilote Microsoft ODBC pour SQL Server sur macOS est pris en charge uniquement sur l’architecture x64. Apple M1 n’est pas pris en charge.

## <a name="microsoft-odbc-17"></a>Microsoft ODBC 17

Pour installer le pilote Microsoft ODBC 17 pour SQL Server sur macOS, exécutez les commandes suivantes :

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
brew tap microsoft/mssql-release https://github.com/Microsoft/homebrew-mssql-release
brew update
HOMEBREW_NO_ENV_FILTERING=1 ACCEPT_EULA=Y brew install msodbcsql17 mssql-tools
```

> [!IMPORTANT]
> Si vous avez installé le package `msodbcsql` v17 qui n’a pas été disponible longtemps, vous devez le supprimer avant d’installer le package `msodbcsql17`. Cette opération évitera les conflits. Le package `msodbcsql17` peut être installé côte à côte avec le package `msodbcsql` v13.

## <a name="previous-versions"></a>Versions précédentes

Les sections suivantes fournissent des instructions pour l’installation des versions précédentes du pilote Microsoft ODBC sur macOS.

## <a name="odbc-131"></a><a id="13.1"></a> ODBC 13.1

Utilisez les commandes suivantes pour installer le pilote Microsoft ODBC 13.1 pour SQL Server sur OS X 10.11 (El Capitan) et macOS 10.12 (Sierra) :

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
brew tap microsoft/mssql-release https://github.com/Microsoft/homebrew-mssql-release
brew update
brew install msodbcsql@13.1.9.2 mssql-tools@14.0.6.0
```

## <a name="driver-files"></a>Fichiers de pilote

Le pilote ODBC sur macOS est constitué des composants suivants :

|Composant|Description|  
|---------------|-----------------|  
|libmsodbcsql.17.dylib ou libmsodbcsql.13.dylib|Fichier de bibliothèque dynamique (`dylib`) contenant l’ensemble des fonctionnalités du pilote. Ce fichier est installé dans `/usr/local/lib/`.|  
|`msodbcsqlr17.rll` ou `msodbcsqlr13.rll`|Fichier de ressources qui accompagne la bibliothèque du pilote. Ce fichier est installé dans `[driver .dylib directory]../share/msodbcsql17/resources/en_US/` pour Driver 17 et dans `[driver .dylib directory]../share/msodbcsql/resources/en_US/` pour Driver 13. | 
|msodbcsql.h|Fichier d’en-tête qui contient toutes les nouvelles définitions nécessaires à l’utilisation du pilote.<br /><br /> **Remarque :**  Vous ne pouvez pas référencer msodbcsql.h et odbcss.h dans le même programme.<br /><br /> msodbcsql.h est installé dans `/usr/local/include/msodbcsql17/` pour Driver 17 et dans `/usr/local/include/msodbcsql/` pour Driver 13. |
|LICENSE.txt|Fichier texte qui contient les termes du contrat de licence utilisateur final. Ce fichier est placé dans `/usr/local/share/doc/msodbcsql17/` pour Driver 17 et dans `/usr/local/share/doc/msodbcsql/` pour Driver 13. |
|RELEASE_NOTES|Fichier texte qui contient les notes de publication. Ce fichier est placé dans `/usr/local/share/doc/msodbcsql17/` pour Driver 17 et dans `/usr/local/share/doc/msodbcsql/` pour Driver 13. |

## <a name="resource-file-loading"></a>Chargement du fichier de ressources

Le pilote doit charger le fichier de ressources pour pouvoir fonctionner. Ce fichier est appelé `msodbcsqlr17.rll` ou `msodbcsqlr13.rll` selon la version du pilote. L’emplacement du fichier `.rll` est relatif à l’emplacement du pilote lui-même (`so` ou `dylib`), comme indiqué dans le tableau ci-dessus. Depuis la version 17.1, le pilote peut aussi essayer de charger le fichier `.rll` à partir du répertoire par défaut si le chargement échoue à partir du chemin d’accès relatif. Le chemin du fichier de ressources par défaut sur macOS est `/usr/local/share/msodbcsql17/resources/en_US/`.

## <a name="troubleshooting"></a>Dépannage

Certains utilisateurs rencontrent un problème lors de la tentative de connexion après l’installation du pilote ODBC et reçoivent une erreur semblable à la suivante : `"[01000] [unixODBC][Driver Manager]Can't open lib 'ODBC Driver 17 for SQL Server' : file not found (0) (SQLDriverConnect)"`. Cela peut être le cas si unixODBC n’est pas configuré correctement pour trouver les pilotes inscrits. Dans ce cas, la création de liens symboliques peut résoudre le problème.

```bash
sudo ln -s /usr/local/etc/odbcinst.ini /etc/odbcinst.ini
sudo ln -s /usr/local/etc/odbc.ini /etc/odbc.ini
```

Si vous ne parvenez pas à établir de connexion à SQL Server à l’aide du pilote ODBC, consultez l’article sur la [résolution des problèmes de connexion](known-issues-in-this-version-of-the-driver.md#connectivity).

## <a name="next-steps"></a>Étapes suivantes

Après avoir installé le pilote, vous pouvez essayer l’[exemple d’application ODBC C++](../../odbc/cpp-code-example-app-connect-access-sql-db.md). Pour plus d’informations sur le développement d’applications ODBC, consultez [Développement d’applications](../../../odbc/reference/develop-app/developing-applications.md).

Pour plus d’informations, consultez les [notes de publication](release-notes-odbc-sql-server-linux-mac.md) et la [configuration système requise](system-requirements.md) du pilote ODBC.
