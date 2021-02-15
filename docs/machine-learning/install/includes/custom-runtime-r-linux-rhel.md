---
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 02/08/2021
ms.topic: include
author: dphansen
ms.author: davidph
ms.openlocfilehash: 588fbd33a0fb65f3de1c2bee54ce3d927960661a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100072740"
---
## <a name="install-language-extensions"></a>Installer les extensions de langage

> [!NOTE]
> Si vous avez [Machine Learning Services](../../sql-server-machine-learning-services.md) installé sur SQL Server 2019, le package **mssql-server-extensibility** pour les extensions de langage est déjà installée et vous pouvez ignorer cette étape.

Exécutez la commande ci-dessous pour installer les [extensions de langage SQL Server](../../../language-extensions/language-extensions-overview.md) sur RHEL (Red Hat Enterprise Linux), qui sont utilisées pour le runtime R personnalisé.

```bash
# Install as root or sudo
sudo yum install mssql-server-extensibility
```

## <a name="install-r"></a>Installation de R

1. Si [Machine Learning Services](../../sql-server-machine-learning-services.md) est installé, R est déjà installé dans `/opt/microsoft/ropen/3.5.2/lib64/R`. Si vous souhaitez continuer à utiliser ce chemin en tant que R_HOME, vous pouvez ignorer cette étape.

    Si vous souhaitez utiliser un autre CLR de R, vous devez d’abord supprimer `microsoft-r-open-mro` avant de poursuivre l’installation d’une nouvelle version.

    ```bash
    sudo yum erase microsoft-r-open-mro-3.5.2
    ```

1. Installez [R (3.3 ou ultérieur)](https://www.r-project.org/) pour RHEL (Red Hat Enterprise Linux). Par défaut, R est installé dans **/usr/lib/R**. Ce chemin d’accès correspond à votre **R_HOME**. Si vous installez R dans un autre emplacement, notez ce chemin qui est votre **R_HOME**.
