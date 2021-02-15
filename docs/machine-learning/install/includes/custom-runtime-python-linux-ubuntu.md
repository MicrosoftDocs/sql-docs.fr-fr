---
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 02/08/2021
ms.topic: include
author: dphansen
ms.author: davidph
ms.openlocfilehash: 0c0efdc599574748112c1d5909404603de24da2c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100072876"
---
## <a name="install-language-extensions"></a>Installer les extensions de langage

> [!NOTE]
> Si vous avez [Machine Learning Services](../../sql-server-machine-learning-services.md) installé sur SQL Server 2019, le package **mssql-server-extensibility** pour les extensions de langage est déjà installée et vous pouvez ignorer cette étape.

Exécutez les commandes ci-dessous pour installer les [extensions de langage SQL Server](../../../language-extensions/language-extensions-overview.md) sur Ubuntu Linux, qui sont utilisées pour le runtime Python personnalisé.

1. Si possible, exécutez cette commande pour actualiser les packages sur le système avant l’installation.

    ```bash
    # Install as root or sudo
    sudo apt-get update
    ```

1. Ubuntu n’a peut-être pas l’option de transport https apt. Pour l’installer, exécutez la commande suivante.

    ```bash
    # Install as root or sudo
    apt-get install apt-transport-https
    ```

1. Installez **mssql-server-extensibility** à l’aide de cette commande.

    ```bash
    # Install as root or sudo
    sudo apt-get install mssql-server-extensibility
    ```

## <a name="install-python-37-and-pandas"></a>Installez Python 3.7 et pandas

L’extension de langage Python utilisée pour le runtime Python personnalisé prend uniquement en charge [Python 3.7](https://www.python.org/) pour l’instant. Si vous souhaitez utiliser une autre version de Python, suivez les instructions du [référentiel GitHub de l’extension de langage Python](https://github.com/microsoft/sql-server-language-extensions/tree/master/language-extensions/python) pour modifier et régénérer l’extension.

1. Exécutez les commandes ci-dessous pour installer Python 3.7.

    ```bash
    # Install python3.7 and the corresponding library:
    sudo add-apt-repository ppa:deadsnakes/ppa
    sudo apt-get update
    sudo apt-get install python3.7 python3-pip libpython3.7
    ```

1. Exécutez la commande ci-dessous pour installer le package pandas.

    ```bash
    # Install pandas to /usr/lib:
    sudo python3.7 -m pip install pandas -t /usr/lib/python3.7/dist-packages
    ```
