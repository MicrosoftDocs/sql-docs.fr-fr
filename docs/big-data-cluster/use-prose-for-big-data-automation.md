---
title: Générer du code pour les tâches de data wrangling
titleSuffix: Azure Data Studio
description: Cet article explique comment utiliser l’accélérateur de code PROSE dans Azure Data Studio pour générer automatiquement du code pour les tâches de data wrangling courantes.
author: dphansen
ms.author: davidph
ms.reviewer: mihaelab
ms.date: 10/12/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: machine-learning-bdc
ms.openlocfilehash: c8423d8a08264255df02d6a9a2ea544a89dbbad3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100043513"
---
# <a name="data-wrangling-using-prose-code-accelerator"></a>Data wrangling avec l’accélérateur de code PROSE

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

L’accélérateur de code PROSE génère du code Python lisible pour vos tâches de data wrangling. Vous pouvez mélanger le code généré au code écrit manuellement lorsque vous utilisez un notebook dans Azure Data Studio.

Cet article fournit une vue d’ensemble de la façon dont vous pouvez utiliser l’accélérateur de code.

 > [!NOTE]
 > Program Synthesis using Examples, ou PROSE, est une technologie Microsoft qui génère du code explicite à l’aide de l’intelligence artificielle. Pour ce faire, il analyse l’intention d’un utilisateur, ainsi que les données, génère plusieurs programmes candidats et choisit le meilleur programme à l’aide d’algorithmes de classement. Pour en savoir plus sur la technologie PROSE, visitez la [page d’accueil de PROSE](https://microsoft.github.io/prose/).

L’accélérateur de code est préinstallé avec Azure Data Studio. Vous pouvez l’importer comme tout autre package Python dans le notebook. Par convention, nous l’importons sous le diminutif cx.

```python
import prose.codeaccelerator as cx
```

Dans la version actuelle, l’accélérateur de code peut générer intelligemment du code Python pour les tâches suivantes :

- Lecture des fichiers de données dans un dataframe Pandas ou Pyspark.
- Correction des types de données dans un dataframe.
- Recherche d’expressions régulières représentant des modèles dans une liste de chaînes.

Pour obtenir une vue d’ensemble générale des méthodes de l’accélérateur de code, consultez la [documentation](/python/api/overview/azure/prose/intro).

## <a name="reading-data-from-a-file-to-a-dataframe"></a>Lecture de données à partir d’un fichier dans un dataframe

La lecture de fichiers dans un dataframe implique de consulter le contenu du fichier et d’identifier les bons paramètres à passer à une bibliothèque de chargement de données.

Selon la complexité du fichier, l’identification des paramètres corrects peut nécessiter plusieurs itérations.

L’accélérateur de code PROSE résout ce problème en analysant la structure du fichier de données et en générant automatiquement du code pour charger le fichier. En général, le code généré analyse correctement les données. Dans certains cas, vous devrez peut-être modifier le code pour répondre à vos besoins.

Prenons l’exemple suivant :

 ```python
import prose.codeaccelerator as cx

# Call the ReadCsvBuilder builder to analyze the file content and generate code to load it
builder = cx.ReadCsvBuilder(r'C:/911.txt')

#Set target to pyspark if generating code to use pyspark library
#builder.Target = "pyspark"

#Get the code generated to fix the data types
builder.learn().code()
 ```

Le bloc de code précédent imprime le code Python suivant pour lire un fichier délimité. Notez la façon dont PROSE détermine automatiquement le nombre de lignes à ignorer, les en-têtes, les éléments QuoteChar, les délimiteurs, etc.

 ```python
import pandas as pd

def read_file(file):
    names = ["lat",
             "lng",
             "desc",
             "zip",
             "title"]

    df = pd.read_csv(file,
        skiprows = 11,
        header = None,
        names = names,
        quotechar = "\"",
        delimiter = "|",
        index_col = False,
        dtype = str,
        na_values = [],
        keep_default_na = False,
        skipinitialspace = True)
    return df
 ```

L’accélérateur de code peut générer du code pour charger des fichiers délimités, JSON et de largeur fixe sur un dataframe. Pour la lecture de fichiers de largeur fixe, le `ReadFwfBuilder` utilise éventuellement un fichier de schéma explicite qu’il peut analyser pour obtenir les positions de colonne. Pour plus d’informations, consultez la [documentation](/python/api/overview/azure/prose/intro).

## <a name="fixing-data-types-in-a-dataframe"></a>Correction des types de données dans un dataframe

Il est courant d’avoir un dataframe Pandas ou Pyspark avec des types de données incorrects. Cela se produit en raison de la présence de valeurs non conformes dans une colonne. Par conséquent, les entiers sont lus en tant que valeurs Float ou chaînes, et les dates sont lues en tant que chaînes. L’effort requis pour corriger manuellement les types de données est proportionnel au nombre de colonnes.

Dans ces situations, vous pouvez utiliser le `DetectTypesBuilder`. Celui-ci analyse les données et génère du code pour corriger les types de données. Le code sert de point de départ. Vous pouvez l’examiner, l’utiliser ou le modifier en fonction des besoins.

```python
import prose.codeaccelerator as cx

builder = cx.DetectTypesBuilder(df)

#Set target to pyspark if working with pyspark
#builder.Target = "pyspark"

#Get the code generated to fix the data types
builder.learn().code()
```

Pour plus d’informations, consultez la [documentation](/python/api/overview/azure/prose/fixdatatypes).

## <a name="identifying-patterns-in-strings"></a>Identification des modèles dans les chaînes

p.


|Ligne|Nom                      |BirthDate      |
|--:|:-------------------------|:--------------|
| 0 |Bertram du Plessis        |1995           |
| 1 |Naiara Moravcikova        |Unknown        |
| 2 |Jihoo Spel                |2014           |
| 3 |Viachaslau Gordan Hilario |22-avr-67      |
| 4 |Maya de Villiers          |19-mar-60      |

En fonction du volume et de la diversité des données, l’écriture d’expressions régulières pour différents modèles dans la colonne peut être une tâche très longue. Le `FindPatternsBuilder` est un puissant outil d’accélération du code qui résout le problème ci-dessus en générant des expressions régulières pour une liste de chaînes.

```python
import prose.codeaccelerator as cx

builder = cx.FindPatternsBuilder(df['BirthDate'])

#Set target to pyspark if working with pyspark
#builder.Target = "pyspark"

builder.learn().regexes
```

Voici les expressions régulières générées par le `FindPatternsBuilder` pour les données ci-dessus.

```
^[0-9]{2}-[A-Z][a-z]+-[0-9]{2}$
^[0-9]{2}[\s][A-Z][a-z]+[\s][0-9]{4}$
^[0-9]{4}$
^Unknown$
```

En plus de la génération d’expressions régulières, le `FindPatternsBuilder` peut également générer du code pour le clustering des valeurs en fonction des regex générés. Il peut également déclarer que toutes les valeurs d’une colonne sont conformes aux expressions régulières générées. Pour en savoir plus et pour découvrir d’autres scénarios utiles, consultez la [documentation](/python/api/overview/azure/prose/findpatterns).