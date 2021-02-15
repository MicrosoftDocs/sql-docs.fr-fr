---
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 02/08/2021
ms.topic: include
author: dphansen
ms.author: davidph
ms.openlocfilehash: a4f854d13b794644b7491db52d204c3375cc8f0c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100072873"
---
## <a name="install-language-extensions"></a>Installer les extensions de langage

> [!NOTE]
> Si vous avez [Machine Learning Services](../../sql-server-machine-learning-services.md) installé sur SQL Server 2019, le package **mssql-server-extensibility** pour les extensions de langage est déjà installée et vous pouvez ignorer cette étape.

Exécutez la commande ci-dessous pour installer les [extensions de langage SQL Server](../../../language-extensions/language-extensions-overview.md) sur SLES (SUSE Linux Enterprise Server), qui sont utilisées pour le runtime Python personnalisé.

```bash
# Install as root or sudo
sudo zypper install mssql-server-extensibility
```

## <a name="install-python-37-and-pandas"></a>Installez Python 3.7 et pandas

L’extension de langage Python utilisée pour le runtime Python personnalisé prend uniquement en charge [Python 3.7](https://www.python.org/) pour l’instant. Si vous souhaitez utiliser une autre version de Python, suivez les instructions du [référentiel GitHub de l’extension de langage Python](https://github.com/microsoft/sql-server-language-extensions/tree/master/language-extensions/python) pour modifier et régénérer l’extension.

1. Installez [Python 3.7](https://www.python.org/) sur le serveur.

1. Exécutez la commande ci-dessous pour installer le package pandas.

    ```bash
    # Install pandas to /usr/lib:
    sudo python3.7 -m pip install pandas -t /usr/lib/python3.7/dist-packages
    ```
