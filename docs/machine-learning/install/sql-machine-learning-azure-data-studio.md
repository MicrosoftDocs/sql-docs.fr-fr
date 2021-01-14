---
title: Notebooks Azure Data Studio (Python, R)
description: Découvrez comment exécuter des scripts Python et R dans un notebook dans Azure Data Studio avec SQL Server Machine Learning Services.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 03/09/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15'
ms.openlocfilehash: b1e3d81e0229d028b9fab6a4521ee11cb20ce1b8
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091545"
---
# <a name="run-python-and-r-scripts-in-azure-data-studio-notebooks-with-sql-server-machine-learning-services"></a>Exécuter des scripts Python et R dans des notebooks Azure Data Studio avec SQL Server Machine Learning Services
[!INCLUDE [SQL Server 2017 and later](../../includes/applies-to-version/sqlserver2017.md)]

Découvrez comment exécuter des scripts Python et R dans des notebooks [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md) avec [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md). Azure Data Studio est un outil de base de données multiplateforme.

## <a name="prerequisites"></a>Prérequis

- [Téléchargez et installez Azure Data Studio](../../azure-data-studio/download-azure-data-studio.md) sur votre poste de travail. Azure Data Studio est multiplateforme et s’exécute sur Windows, macOS et Linux.

- Un serveur avec SQL Server Machine Learning Services installé et activé. Vous pouvez utiliser Machine Learning Services sur des clusters Windows, Linux ou Big Data :

  - [Installez SQL Server Machine Learning Services sur Windows](sql-machine-learning-services-windows-install.md).
  - [Installez SQL Server Machine Learning Services sur Linux](../../linux/sql-server-linux-setup-machine-learning.md).
  - [Exécutez des scripts Python et R avec Machine Learning Services sur des clusters Big Data SQL Server](../../big-data-cluster/machine-learning-services.md).

## <a name="create-a-sql-notebook"></a>Créer un notebook SQL

> [!IMPORTANT]
> Machine Learning Services s’exécute dans le cadre de SQL Server. Par conséquent, vous devez utiliser un noyau SQL et non pas un noyau Python.

Vous pouvez utiliser Machine Learning Services dans Azure Data Studio avec un notebook SQL. Pour créer un notebook, effectuez ces étapes :

1. Cliquez sur **Fichier**, puis sur **Nouveau notebook** pour créer un notebook. Par défaut, le notebook utilise le **noyau SQL**.

1. Cliquez sur **Attacher à**, puis sur **Changer la connexion**. 

    > [!div class="mx-imgBorder"]
    > ![Azure Data Studio - Notebook SQL - Changer la connexion](media/ads-attach-to-connection.png)
    
1. Connectez-vous à un serveur SQL Server existant ou nouveau. Vous pouvez :

    1. Choisissez une connexion existante sous **Connexions récentes** ou sous **Connexions enregistrées**.

    1. Créez une connexion sous **Détails de la connexion**. Renseignez les informations de connexion à votre serveur et à votre base de données SQL Server.

    > [!div class="mx-imgBorder"]
    > ![Azure Data Studio - Notebook SQL - Détails de la connexion](media/ads-connection-details.png)  

## <a name="run-python-or-r-scripts"></a>Exécuter des scripts Python ou R

Les notebooks SQL sont constitués de cellules de code et de texte. Les cellules de code sont utilisées pour exécuter des scripts Python ou R via la procédure stockée [sp_execute_external_scripts](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md). Vous pouvez utiliser des cellules de texte pour documenter votre code dans le notebook.

### <a name="run-a-python-script"></a>Exécuter un script Python

Effectuez ces étapes pour exécuter un script Python :

1. Cliquez sur **+ Code** pour ajouter une cellule de code.

    > [!div class="mx-imgBorder"]
    > ![Azure Data Studio - Notebooks SQL - Ajouter un bloc de code](media/ads-add-code.png)  

1. Entrez le script suivant dans la cellule de code :

    ```sql
    EXECUTE sp_execute_external_script @language = N'Python'
        , @script = N'
    a = 1
    b = 2
    c = a/b
    d = a*b
    print(c, d)
    '
    ```

1. Cliquez sur **Exécuter la cellule** (la flèche noire arrondie) ou appuyez sur **F5** pour exécuter seulement cette cellule.

    > [!div class="mx-imgBorder"]
    > ![Azure Data Studio - Notebooks SQL - Exécuter du code Python](media/ads-run-python.png)  

1. Le résultat s’affiche sous la cellule de code.

    > [!div class="mx-imgBorder"]
    > ![Azure Data Studio - Notebook SQL - Résultat du code Python](media/ads-run-python-output.png)  

### <a name="run-an-r-script"></a>Exécuter un script R

Effectuez ces étapes pour exécuter un script R :

1. Cliquez sur **+ Code** pour ajouter une cellule de code.

    > [!div class="mx-imgBorder"]
    > ![Azure Data Studio - Notebooks SQL - Ajouter un bloc de code](media/ads-add-code.png)  

1. Entrez le script suivant dans la cellule de code :

    ```sql
    EXECUTE sp_execute_external_script @language = N'R'
        , @script = N'
    a <- 1
    b <- 2
    c <- a/b
    d <- a*b
    print(c(c, d))
    '
    ```

1. Cliquez sur **Exécuter la cellule** (la flèche noire arrondie) ou appuyez sur **F5** pour exécuter seulement cette cellule.

    > [!div class="mx-imgBorder"]
    > ![Azure Data Studio - Notebooks SQL - Exécuter du code R](media/ads-run-r.png)  

1. Le résultat s’affiche sous la cellule de code.

    > [!div class="mx-imgBorder"]
    > ![Azure Data Studio - Notebooks SQL - Résultat du code R](media/ads-run-r-output.png)  

## <a name="next-steps"></a>Étapes suivantes

- [Guide pratique pour utiliser les notebooks dans Azure Data Studio](../../azure-data-studio/notebooks/notebooks-guidance.md)
- [Créer et exécuter un notebook SQL Server](../../azure-data-studio/notebooks/notebooks-sql-kernel.md)
- [Démarrage rapide : Exécuter des scripts Python simples avec SQL Server Machine Learning Services](../tutorials/quickstart-python-create-script.md)
- [Démarrage rapide : Exécuter des scripts R simples avec SQL Server Machine Learning Services](../tutorials/quickstart-r-create-script.md)