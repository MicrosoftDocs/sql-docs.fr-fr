---
title: Insertion de données à partir d’une table SQL dans une trame de données Python Pandas
titleSuffix: SQL machine learning
description: Découvrez comment lire des données à partir d’une table SQL et les insérer dans une trame de données Pandas avec Python.
author: dphansen
ms.author: davidph
ms.date: 07/23/2020
ms.topic: how-to
ms.prod: sql
ms.technology: machine-learning
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current||=azuresqldb-current'
ms.openlocfilehash: 3acd5153599c6237117e52b483247033ebf983ec
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091626"
---
# <a name="insert-data-from-a-sql-table-into-a-python-pandas-dataframe"></a>Insertion de données à partir d’une table SQL dans une trame de données Python Pandas
[!INCLUDE[SQL Server SQL DB SQL MI](../../includes/applies-to-version/sql-asdb-asdbmi.md)]

Cet article explique comment insérer des données SQL dans une trame de données [Pandas](https://pandas.pydata.org/) à l’aide du package [pyodbc](../../connect/python/pyodbc/python-sql-driver-pyodbc.md) en Python. Les lignes et les colonnes de données contenues dans la trame de données peuvent être utilisées pour une exploration plus approfondie des données.

## <a name="prerequisites"></a>Prérequis

::: moniker range=">=sql-server-2017||>=sql-server-linux-ver15"
* [SQL Server pour Windows](../../database-engine/install-windows/install-sql-server.md) ou [pour Linux](../../linux/sql-server-linux-overview.md).
::: moniker-end

::: moniker range="=azuresqldb-current"
* [Azure SQL Database](/azure/sql-database/sql-database-get-started-portal)
::: moniker-end

::: moniker range="=azuresqldb-mi-current"
* [Azure SQL Managed Instance](/azure/azure-sql/managed-instance/instance-create-quickstart)

* [SQL Server Management Studio](../../ssms/download-sql-server-management-studio-ssms.md) pour restaurer l’exemple de base de données sur Azure SQL Managed Instance.
::: moniker-end

* Azure Data Studio. Pour l’installer, consultez [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md).

* [Exemple de restauration de base de données](../../samples/adventureworks-install-configure.md) pour obtenir les exemples de données utilisés dans cet article.

## <a name="verify-restored-database"></a>Vérification de la base de données restaurée

Vous pouvez vérifier que la base de données restaurée existe en interrogeant la table **Person.CountryRegion** :

```sql
USE AdventureWorks;
SELECT * FROM Person.CountryRegion;
```

## <a name="install-python-packages"></a>Installer des packages Python

[Téléchargez et installez Azure Data Studio](../../azure-data-studio/download-azure-data-studio.md).

Installez les packages Python suivants :
  * pyodbc
  * pandas

  Pour installer ces packages :

  1. Dans votre notebook Azure Data Studio, sélectionnez **Gérer les packages**.
  2. Dans le volet **Gérer les packages**, sélectionnez l’onglet **Ajouter nouveau**.
  3. Pour chacun des packages suivants, entrez le nom du package, cliquez sur **Rechercher**, puis cliquez sur **Installer**.

## <a name="insert-data"></a>Insertion des données

Utilisez le script suivant pour sélectionner des données de la table Person.CountryRegion et les insérer dans une trame de données. Modifiez les variables de chaîne de connexion « server », « database », « username » et « password » pour la connexion à SQL.

Pour créer un bloc-notes :

1. Dans Azure Data Studio, sélectionnez **Fichier**, puis **Nouveau notebook**.
2. Dans le notebook, sélectionnez le noyau **Python3**, puis **+code**.
3. Collez le code dans le notebook, puis sélectionnez **Tout exécuter**.

```python
import pyodbc
import pandas as pd
# Some other example server values are
# server = 'localhost\sqlexpress' # for a named instance
# server = 'myserver,port' # to specify an alternate port
server = 'servername' 
database = 'AdventureWorks' 
username = 'yourusername' 
password = 'databasename'  
cnxn = pyodbc.connect('DRIVER={SQL Server};SERVER='+server+';DATABASE='+database+';UID='+username+';PWD='+ password)
cursor = cnxn.cursor()
# select 26 rows from SQL table to insert in dataframe.
query = "SELECT [CountryRegionCode], [Name] FROM Person.CountryRegion;"
df = pd.read_sql(query, cnxn)
print(df.head(26))
```

**Sortie**

La commande `print` du script précédent affiche les lignes de données de la trame de données `pandas` `df`.

```text
CountryRegionCode                 Name
0                 AF          Afghanistan
1                 AL              Albania
2                 DZ              Algeria
3                 AS       American Samoa
4                 AD              Andorra
5                 AO               Angola
6                 AI             Anguilla
7                 AQ           Antarctica
8                 AG  Antigua and Barbuda
9                 AR            Argentina
10                AM              Armenia
11                AW                Aruba
12                AU            Australia
13                AT              Austria
14                AZ           Azerbaijan
15                BS         Bahamas, The
16                BH              Bahrain
17                BD           Bangladesh
18                BB             Barbados
19                BY              Belarus
20                BE              Belgium
21                BZ               Belize
22                BJ                Benin
23                BM              Bermuda
24                BT               Bhutan
25                BO              Bolivia
```

## <a name="next-steps"></a>Étapes suivantes

+ [Insertion d’une trame de données Python dans SQL](../data-exploration/python-dataframe-sql-server.md)