---
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 02/08/2021
ms.topic: include
author: dphansen
ms.author: davidph
ms.openlocfilehash: 83b39be5be128ba4fbda17765a7b67deead2c80c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100072741"
---
## <a name="install-language-extensions"></a>Installer les extensions de langage

> [!NOTE]
> Si vous avez [Machine Learning Services](../../sql-server-machine-learning-services.md) installé sur SQL Server 2019, le package **mssql-server-extensibility** pour les extensions de langage est déjà installée et vous pouvez ignorer cette étape.

Exécutez les commandes ci-dessous pour installer les [extensions de langage SQL Server](../../../language-extensions/language-extensions-overview.md) sur Ubuntu Linux, qui sont utilisées pour le runtime R personnalisé.

1. Si possible, exécutez cette commande pour actualiser les packages sur le système avant l’installation.

    ```bash
    # Install as root or sudo
    sudo apt-get update
    ```

1. Installez **mssql-server-extensibility** à l’aide de cette commande.

    ```bash
    # Install as root or sudo
    sudo apt-get install mssql-server-extensibility
    ```

## <a name="install-r"></a>Installation de R

1. Si [Machine Learning Services](../../sql-server-machine-learning-services.md) est installé, R est déjà installé dans `/opt/microsoft/ropen/3.5.2/lib64/R`. Si vous souhaitez continuer à utiliser ce chemin en tant que R_HOME, vous pouvez ignorer cette étape.

    Si vous souhaitez utiliser un autre CLR de R, vous devez d’abord supprimer `microsoft-r-open-mro` avant de poursuivre l’installation d’une nouvelle version.

    ```bash
    sudo apt remove microsoft-r-open-mro-3.5.2
    ```

1. Installez [R (3.3 ou ultérieur)](https://www.r-project.org/) pour Ubuntu. Par défaut, R est installé dans **/usr/lib/R**. Ce chemin d’accès correspond à votre **R_HOME**. Si vous installez R dans un autre emplacement, notez ce chemin qui est votre **R_HOME**.

    Vous trouverez ci-dessous des exemples d’instructions pour Ubuntu. Modifiez l’URL de dépôt ci-dessous pour votre version de R.

    ```bash
    export DEBIAN_FRONTEND=noninteractive
    sudo apt-get update
    sudo apt-get --no-install-recommends -y install curl zip unzip apt-transport-https libstdc++6
    
    # Add R CRAN repository. This repository works for R 4.0.x.
    #
    sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys E298A3A825C0D65DFD57CBB651716619E084DAB9
    sudo add-apt-repository 'deb https://cloud.r-project.org/bin/linux/ubuntu xenial-cran40/'
    sudo apt-get update
    
    # Install R runtime.
    #
    sudo apt-get -y install r-base-core
    ```
