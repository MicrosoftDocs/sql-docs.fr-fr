---
title: Insertion d’une trame de données Python dans une table SQL
titleSuffix: SQL machine learning
description: Guide pratique pour insérer les données d’une trame de données dans une table SQL.
author: dphansen
ms.author: davidph
ms.date: 07/23/2020
ms.topic: how-to
ms.prod: sql
ms.technology: machine-learning
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current||=azuresqldb-current'
ms.openlocfilehash: 9a8b30cc1fa5af3e22ce0294e8eff21cf413e2c6
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102044"
---
# <a name="insert-python-dataframe-into-sql-table"></a>Insertion d’une trame de données Python dans une table SQL
[!INCLUDE[SQL Server SQL DB SQL MI](../../includes/applies-to-version/sql-asdb-asdbmi.md)]

Cet article explique comment insérer une trame de données [Pandas](https://pandas.pydata.org/) dans une base de données SQL à l’aide du package [pyodbc](../../connect/python/pyodbc/python-sql-driver-pyodbc.md) en Python.

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

Vous pouvez vérifier que la base de données restaurée existe en interrogeant la table **HumanResources.Department** :

```sql
USE AdventureWorks;
SELECT * FROM HumanResources.Department;
```

## <a name="install-python-packages"></a>Installer des packages Python

* [Télécharger et installer Azure Data Studio](../../azure-data-studio/download-azure-data-studio.md)

Installez les packages Python suivants :
  * pyodbc
  * pandas

  Pour installer ces packages :

  1. Dans votre notebook Azure Data Studio, sélectionnez **Gérer les packages**.
  2. Dans le volet **Gérer les packages**, sélectionnez l’onglet **Ajouter nouveau**.
  3. Pour chacun des packages suivants, entrez le nom du package, cliquez sur **Rechercher**, puis cliquez sur **Installer**.

## <a name="connect-to-sql-server-using-azure-data-studio"></a>Connexion à SQL Server avec Azure Data Studio

[Connectez-vous avec Azure Data Studio](../../azure-data-studio/quickstart-sql-server.md).

1. Connectez-vous à la base de données AdventureWorks pour créer la nouvelle table SQL, HumanResources.DepartmentTest, qui sera utilisée pour l’insertion de la trame de données.

```sql
CREATE TABLE [HumanResources].[DepartmentTest](
[DepartmentID] [smallint] NOT NULL,
[Name] [dbo].[Name] NOT NULL,
[GroupName] [dbo].[Name] NOT NULL
)
GO
```

## <a name="create-csv-file"></a>Création d’un fichier CSV

Copiez le texte et enregistrez le fichier sous department.csv pour la trame de la données.

```text
DepartmentID,Name,GroupName,
1,Engineering,Research and Development,
2,Tool Design,Research and Development,
3,Sales,Sales and Marketing,
4,Marketing,Sales and Marketing,
5,Purchasing,Inventory Management,
6,Research and Development,Research and Development,
7,Production,Manufacturing,
8,Production Control,Manufacturing,
9,Human Resources,Executive General and Administration,
10,Finance,Executive General and Administration,
11,Information Services,Executive General and Administration,
12,Document Control,Quality Assurance,
13,Quality Assurance,Quality Assurance,
14,Facilities and Maintenance,Executive General and Administration,
15,Shipping and Receiving,Inventory Management,
16,Executive,Executive General and Administration
```

## <a name="connect-to-sql-using-python"></a>Connexion à SQL avec Python

1. Modifiez les variables de chaîne de connexion « server », « database », « username » et « password » pour la connexion à la base de données SQL.

2. Modifiez le chemin du fichier CSV.

## <a name="load-dataframe-from-csv-file"></a>Chargement de la trame de données à partir du fichier CSV

Utilisez le package Python `pandas` pour créer une trame de données et charger le fichier CSV. Connectez-vous à SQL pour charger la trame de données dans la nouvelle table SQL, HumanResources.DepartmentTest.

Pour créer un bloc-notes :

1. Dans Azure Data Studio, sélectionnez **Fichier**, puis **Nouveau notebook**.
2. Dans le notebook, sélectionnez le noyau **Python3**, puis **+code**.
3. Collez le code dans le notebook, puis sélectionnez **Tout exécuter**.

 ```Python
import pyodbc
import pandas as pd
# insert data from csv file into dataframe.
# working directory for csv file: type "pwd" in Azure Data Studio or Linux
# working directory in Windows c:\users\username
df = pd.read_csv("c:\\user\\username\department.csv")
# Some other example server values are
# server = 'localhost\sqlexpress' # for a named instance
# server = 'myserver,port' # to specify an alternate port
server = 'yourservername' 
database = 'AdventureWorks' 
username = 'username' 
password = 'yourpassword' 
cnxn = pyodbc.connect('DRIVER={SQL Server};SERVER='+server+';DATABASE='+database+';UID='+username+';PWD='+ password)
cursor = cnxn.cursor()
# Insert Dataframe into SQL Server:
for index, row in df.iterrows():
     cursor.execute("INSERT INTO HumanResources.DepartmentTest (DepartmentID,Name,GroupName) values(?,?,?)", row.DepartmentID, row.Name, row.GroupName)
cnxn.commit()
cursor.close()
```

## <a name="confirm-row-count-in-sql"></a>Confirmation du nombre de lignes en SQL

Exécutez l’instruction SQL pour confirmer que la table a bien été chargée avec les données de la trame de données.

```sql
SELECT count(*) from HumanResources.DepartmentTest;
```

Résultats

```bash
(No column name)
16
```

## <a name="next-steps"></a>Étapes suivantes

+ [Création d’un histogramme pour l’exploration de données avec Python](../data-exploration/python-plot-histogram.md)