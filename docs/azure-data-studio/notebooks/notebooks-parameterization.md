---
title: Paramétrage de notebooks dans Azure Data Studio
description: Ce tutoriel vous montre comment créer un notebook paramétrable dans ADS.
ms.topic: how-to
ms.prod: azure-data-studio
ms.technology: azure-data-studio
author: vasubhog
ms.author: vabhog
ms.reviewer: mikeray, alayu, maghan
ms.custom: ''
ms.date: 01/25/2021
ms.openlocfilehash: 25e8ea8c4f10ccdb7ee2901dced68f4a66d57000
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/04/2021
ms.locfileid: "101836076"
---
# <a name="create-a-parameterized-notebook"></a>Créer un notebook paramétrable

Le **paramétrage** correspond à la capacité à exécuter le même notebook avec différents paramètres.

Cet article vous montre comment créer et exécuter un notebook paramétrable dans Azure Data Studio à l’aide du noyau Python.

## <a name="prerequisites"></a>Prérequis

- [Azure Data Studio](../download-azure-data-studio.md)
- [Python](https://www.python.org/downloads/)

## <a name="install-and-set-up-papermill-in-azure-data-studio"></a>Installer et configurer Papermill dans Azure Data Studio

Les étapes de cette section s’exécutent toutes dans un notebook Azure Data Studio.

1. Créez un notebook et choisissez **Python 3** comme *Noyau*.

   ![Nouveau notebook](media/notebooks-kqlmagic/install-new-notebook.png)

2. Vous pouvez être invité à mettre à niveau vos packages Python quand vos packages doivent être mis à jour.

   ![Oui](media/notebooks-kqlmagic/install-python-yes.png)

3. Installez Papermill :

   ```python
   import sys
   !{sys.executable} -m pip install papermill --no-cache-dir --upgrade
   ```

   Vérifiez qu’il est installé :

   ```python
   import sys
   !{sys.executable} -m pip list
   ```

   :::image type="content" source="media/notebooks-parameterization/install-list-papermill.png" alt-text="Liste":::

5. Vous pouvez tester si Papermill est chargé correctement en vérifiant sa version.

   ```python
   import papermill
   papermill
   ```

   :::image type="content" source="media/notebooks-parameterization/install-validation-papermill.png" alt-text="Validation":::

## <a name="set-up-a-parameterized-notebook"></a>Configurer un notebook paramétrable

1. Vérifiez que **Python 3** est sélectionné comme *Noyau*.

   ![Modification du noyau](media/notebooks-kqlmagic/change-kernel.png)

2. Créez une cellule de code et étiquetez-la **Cellule de paramètres**.

   ```python
   x = 2.0
   y = 5.0
   ```

   :::image type="content" source="media/notebooks-parameterization/make-parameter-cell.png" alt-text="Notebook de cellule de paramètres":::

3. Ajoutez d’autres cellules pour tester différents paramètres.

   ```python
   addition = x + y
   multiply = x * y
   ```

   ```python
   print("Addition: " + str(addition))
   print("Multiplication: " + str(multiply))
   ```

   Cellules dans l’exemple de notebook d’entrée : :::image type="content" source="media/notebooks-parameterization/test-cells.png" alt-text="Cellules de notebook d’entrée supplémentaires":::

4. Enregistrez le notebook sous **Input.ipynb**.
   :::image type="content" source="media/notebooks-parameterization/save-notebook.png" alt-text="Enregistrer le notebook":::

## <a name="how-to-execute-papermill-notebook"></a>Comment exécuter le notebook Papermill

Papermill peut s’exécuter de deux façons :

- Interface de ligne de commande (CLI)
- API Python

### <a name="parameterized-cli-execution"></a>Exécution de l’interface CLI paramétrable

Pour exécuter un notebook à l’aide de l’interface CLI, entrez la commande Papermill dans le terminal avec le notebook d’entrée, l’emplacement du notebook de sortie et les options.

> [!Note]
   > Vous trouverez la documentation de l’interface de ligne de commande Papermill [ici](https://papermill.readthedocs.io/en/latest/usage-execute.html#execute-via-cli).

1. Exécutez le notebook d’entrée avec les nouveaux paramètres.

   ```shell
   papermill Input.ipynb Output.ipynb -p x 10 -p y 20
   ```

   Le notebook d’entrée est ainsi exécuté avec de nouvelles valeurs pour les paramètres **x** et **y**.

2. Après l’exécution, affichez le nouveau notebook paramétrable de sortie.
   Vous pouvez remarquer qu’il existe une nouvelle cellule étiquetée **# Injected-Parameters** qui contient les nouvelles valeurs de paramètre passées par le biais de l’interface CLI.

   :::image type="content" source="media/notebooks-parameterization/output-notebook.png" alt-text="Notebook de sortie":::

### <a name="parameterized-python-api-execution"></a>Exécution de l’API Python paramétrable

> [!Note]
   > La documentation de l’API Python Papermill est disponible [ici](https://papermill.readthedocs.io/en/latest/usage-execute.html#execute-via-the-python-api).

1. Créez un notebook et choisissez **Python 3** comme *Noyau*.
   ![Nouveau notebook](media/notebooks-kqlmagic/install-new-notebook.png)

2. Ajoutez une nouvelle cellule de code et utilisez Papermill pour utiliser la méthode execute.

   ```python
   import papermill as pm

   pm.execute_notebook(
   '/Users/vasubhog/GitProjects/AzureDataStudio-Notebooks/Demo_Parameterization/Input.ipynb',
   '/Users/vasubhog/GitProjects/AzureDataStudio-Notebooks/Demo_Parameterization/Output.ipynb',
   parameters = dict(x = 10, y = 20)
   )
   ```

   ![Exécution de l’API Python Papermill](media/notebooks-parameterization/python-api-execute.png)

3. Après l’exécution, affichez le nouveau notebook de paramétrage de sortie.

   Vous pouvez remarquer qu’il existe une nouvelle cellule étiquetée **# Injected-Parameters** qui contient les nouvelles valeurs de paramètre passées par le biais de l’interface CLI.

   :::image type="content" source="media/notebooks-parameterization/output-notebook.png" alt-text="Notebook de sortie":::

## <a name="next-steps"></a>Étapes suivantes

Découvrez-en plus sur les notebooks et le paramétrage :

- [Guide pratique pour utiliser des notebooks dans Azure Data Studio](./notebooks-guidance.md)
- [Documentation sur le paramétrage de Papermill](https://papermill.readthedocs.io/en/latest/index.html)