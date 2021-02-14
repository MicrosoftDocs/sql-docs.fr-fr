---
title: Gérer les packages avec l’extension Machine Learning
description: Découvrez comment gérer les packages Python ou R dans votre base de données à l’aide de l’extension Machine Learning pour Azure Data Studio.
ms.prod: azure-data-studio
ms.technology: machine-learning
ms.topic: conceptual
author: dphansen
ms.author: davidph
ms.reviewer: sstein
ms.custom: ''
ms.date: 05/19/2020
ms.openlocfilehash: c874317acf39e954a0e640c1954e3a0dac6e9f43
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100051960"
---
# <a name="manage-packages-in-database-with-machine-learning-extension-for-azure-data-studio-preview"></a>Gérer les packages dans une base de données à l’aide de l’extension Machine Learning pour Azure Data Studio (préversion)

Découvrez comment gérer les packages Python ou R dans votre base de données à l’aide de l’[extension Machine Learning pour Azure Data Studio](machine-learning-extension.md).

> [!IMPORTANT]
> La gestion des packages dans une base de données à l’aide de l’extension Machine Learning ne prend actuellement en charge que [Machine Learning Services](../../machine-learning/sql-server-machine-learning-services.md) sur SQL Server 2019.

## <a name="prerequisites"></a>Prérequis

- Installez et configurez l’[extension Machine Learning pour Azure Data Studio](machine-learning-extension.md). Vous devez spécifier les [chemins d’installation Python ou R dans les paramètres d’extension](machine-learning-extension.md#settings) pour que la gestion des packages fonctionne.

- Package **sqlmlutils**. L’extension Machine Learning vous invite à installer le package le cas échéant.

- L’authentification Windows n’est pas prise en charge pour SQL Server sur Linux. Par conséquent, vous devez utiliser l’authentification SQL pour vous connecter à votre serveur SQL Server sur Linux.

## <a name="manage-python-packages"></a>Gérer les packages Python

Vous pouvez installer et désinstaller les packages Python à l’aide de l’extension Machine Learning.

### <a name="install-new-python-package"></a>Installer un nouveau package Python

Pour installer des packages Python dans votre base de données, procédez comme suit.

1. Sélectionnez **Gérer les packages dans la base de données**.

1. Si vous êtes invité à installer **sqlmlutils**, sélectionnez **Oui**.

1. Sous l’onglet **Installé**, sélectionnez le **type de package** **Python**, puis votre base de données sous **Emplacement**.

1. Sélectionnez l’onglet **Ajouter nouveau**.

1. Entrez le package Python que vous souhaitez installer dans **Rechercher des packages Python**, puis sélectionnez **Rechercher**.

1. Vérifiez que le package apparaît sous **Nom du package** et que la version souhaitée apparaît sous **Version du package**.

1. Sélectionnez **Installer**.

1. Sélectionnez **Fermer** et vérifiez que le package a été correctement installé sous **Tâches**.

### <a name="uninstall-a-python-package"></a>Désinstaller un package Python

Pour désinstaller des packages Python dans votre base de données, procédez comme suit.

> [!NOTE]
> Vous ne pouvez désinstaller que les packages qui ont été installés dans votre base de données. Il n’est pas possible de désinstaller un package qui a été préinstallé à l’aide de SQL Server Machine Learning Services.

1. Sélectionnez **Gérer les packages dans la base de données**.

1. Si vous êtes invité à installer **sqlmlutils**, sélectionnez **Oui**.

1. Sous l’onglet **Installé**, sélectionnez le **type de package** **Python**, puis votre base de données sous **Emplacement**.

1. Sélectionnez le ou les packages que vous souhaitez désinstaller.

1. Sélectionnez sur **Désinstaller les packages sélectionnés**.

1. Sélectionnez **Fermer** et vérifiez que le package a été correctement installé sous **Tâches**.

## <a name="manage-r-packages"></a>Gérer les packages R

Vous pouvez installer et désinstaller les packages R à l’aide de l’extension Machine Learning.

### <a name="install-new-r-package"></a>Installer un nouveau package R

Pour installer des packages R dans votre base de données, procédez comme suit.

1. Sélectionnez **Gérer les packages dans la base de données**.

1. Si vous êtes invité à installer **sqlmlutils**, sélectionnez **Oui**.

1. Sous l’onglet **Installé**, sélectionnez le **type de package** **R**, puis votre base de données sous **Emplacement**.

1. Sélectionnez l’onglet **Ajouter nouveau**.

1. Entrez le package R que vous souhaitez installer dans **Rechercher des packages R**, puis sélectionnez **Rechercher**.

1. Vérifiez que le package apparaît sous **Nom du package** et que la version souhaitée apparaît sous **Version du package**.

1. Sélectionnez **Installer**.

1. Sélectionnez **Fermer** et vérifiez que le package a été correctement installé sous **Tâches**.

### <a name="uninstall-an-r-package"></a>Désinstaller un package R

Pour désinstaller des packages R dans votre base de données, procédez comme suit.

> [!NOTE]
> Vous ne pouvez désinstaller que les packages qui ont été installés dans votre base de données. Il n’est pas possible de désinstaller un package qui a été préinstallé à l’aide de SQL Server Machine Learning Services.

1. Sélectionnez **Gérer les packages dans la base de données**.

1. Si vous êtes invité à installer **sqlmlutils**, sélectionnez **Oui**.

1. Sous l’onglet **Installé**, sélectionnez le **type de package** **R**, puis votre base de données sous **Emplacement**.

1. Sélectionnez le ou les packages que vous souhaitez désinstaller.

1. Sélectionnez sur **Désinstaller les packages sélectionnés**.

1. Sélectionnez **Fermer** et vérifiez que le package a été correctement installé sous **Tâches**.

## <a name="next-steps"></a>Étapes suivantes

- [Extension Machine Learning dans Azure Data Studio](machine-learning-extension.md)
- [Effectuer des prédictions](machine-learning-extension-predictions.md)
- [Importer ou afficher des modèles](machine-learning-extension-import-view-models.md)
- [Notebooks dans Azure Data Studio](../notebooks/notebooks-guidance.md)
- [Documentation sur SQL Machine Learning](../../machine-learning/index.yml)