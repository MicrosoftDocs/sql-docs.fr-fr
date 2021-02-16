---
title: Copie en bloc de données pour SQL Server sur Linux
description: Cet article décrit l’utilitaire bcp. Utilisez bcp pour importer un grand nombre de lignes dans des tables SQL Server ou pour exporter des données de tables SQL Server dans des fichiers de données.
author: VanMSFT
ms.author: vanto
ms.date: 01/30/2018
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 7b93d0d7-7946-4b78-b33a-57d6307cdfa9
ms.openlocfilehash: c27e4095854abd409336c97ac5b8458bf6f62894
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345614"
---
# <a name="bulk-copy-data-with-bcp-to-sql-server-on-linux"></a>Copie en bloc de données avec bcp pour SQL Server sur Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Cet article explique comment utiliser l'utilitaire de ligne de commande [bcp](../tools/bcp-utility.md) pour copier en bloc des données entre une instance de SQL Server sur Linux et un fichier de données dans un format spécifié par l’utilisateur.

Vous pouvez utiliser `bcp` pour importer un grand nombre de lignes dans des tables SQL Server ou pour exporter des données de tables SQL Server dans des fichiers de données. Sauf lorsqu’il est utilisé avec l’option queryout, `bcp` ne nécessite aucune connaissance de Transact-SQL. L'utilitaire en ligne de commande `bcp` fonctionne avec Microsoft SQL Server en local ou dans le cloud, sur Linux, Windows ou Docker, ainsi qu’avec Azure SQL Database et Azure Synapse Analytics.

Cet article vous montre comment :
- Importer des données dans une table à l’aide de la commande `bcp in`
- Importer des données à partir d’une table à l’aide de la commande `bcp out`

## <a name="install-the-sql-server-command-line-tools"></a>Installer les outils en ligne de commande SQL Server

`bcp` fait partie des outils en ligne de commande SQL Server qui ne sont pas installés automatiquement avec SQL Server sur Linux. Si vous n’avez pas encore installé les outils en ligne de commande SQL Server sur votre machine Linux, vous devez les installer. Pour plus d’informations sur l’installation des outils, sélectionnez votre distribution Linux dans la liste suivante :

- [Red Hat Enterprise Linux (RHEL)](sql-server-linux-setup-tools.md#RHEL)
- [Ubuntu](sql-server-linux-setup-tools.md#ubuntu)
- [SUSE Linux Enterprise Server (SLES)](sql-server-linux-setup-tools.md#SLES)

## <a name="import-data-with-bcp"></a>Importer des données avec bcp

Dans ce didacticiel, vous allez créer un exemple de base de données et de table sur l'instance locale (**localhost**), puis utiliser `bcp` pour charger l’exemple de table à partir d’un fichier texte sur le disque.

### <a name="create-a-sample-database-and-table"></a>Créer un exemple de base de données et de table

Commençons par créer un exemple de base de données avec une table simple utilisée dans le reste de ce didacticiel.

1. Sur votre boîte Linux, ouvrez un terminal de commande.

2. Copiez et collez les commandes suivantes dans la fenêtre du terminal. Ces commandes utilisent l'utilitaire de ligne de commande **sqlcmd** pour créer un exemple de base de données (**BcpSampleDB**) et une table (**TestEmployees**) sur l’instance locale (**localhost**). N’oubliez pas de remplacer le `username` et le `<your_password>`, si nécessaire, avant d’exécuter les commandes.

Pour créer la base de données **BcpSampleDB** :
```bash 
sqlcmd -S localhost -U sa -P <your_password> -Q "CREATE DATABASE BcpSampleDB;"
```
Créez la table **TestEmployees** dans la base de données **BcpSampleDB** :
```bash 
sqlcmd -S localhost -U sa -P <your_password> -d BcpSampleDB -Q "CREATE TABLE TestEmployees (Id INT IDENTITY(1,1) NOT NULL PRIMARY KEY, Name NVARCHAR(50), Location NVARCHAR(50));"
```
### <a name="create-the-source-data-file"></a>Créer le fichier de données source
Copiez et collez la commande suivante dans la fenêtre du terminal. Nous utilisons la commande `cat` intégrée pour créer un exemple de fichier de données texte avec trois enregistrements et enregistrer le fichier dans votre répertoire de départ sous la forme **~/test_data.txt**. Les champs des enregistrements sont délimités par une virgule.

```bash
cat > ~/test_data.txt << EOF
1,Jared,Australia
2,Nikita,India
3,Tom,Germany
EOF
```

Vous pouvez vérifier que le fichier de données a été créé correctement en exécutant la commande suivante dans la fenêtre du terminal :
```bash 
cat ~/test_data.txt
```

Ce qui suit s’affiche dans la fenêtre du terminal :
```bash
1,Jared,Australia
2,Nikita,India
3,Tom,Germany
```

### <a name="import-data-from-the-source-data-file"></a>Importer des données à partir du fichier de données source
Copiez et collez les commandes suivantes dans la fenêtre du terminal. Cette commande utilise `bcp` pour se connecter à l’instance locale (**localhost**) et importer les données à partir du fichier de données ( **~/test_data.txt**) dans la table (**TestEmployees**) dans la base de données(**BcpSampleDB**). N’oubliez pas de remplacer le nom d’utilisateur et le `<your_password>`, si nécessaire, avant d’exécuter les commandes.

```bash 
bcp TestEmployees in ~/test_data.txt -S localhost -U sa -P <your_password> -d BcpSampleDB -c -t  ','
```

Voici une brève vue d’ensemble des paramètres de ligne de commande utilisés avec `bcp` dans cet exemple :
- `-S` : spécifie l’instance de SQL Server à laquelle se connecter
- `-U` : spécifie l'ID de connexion utilisé pour se connecter à SQL Server
- `-P` : spécifie le mot de passe de l’ID de connexion
- `-d` : spécifie la base de données à laquelle se connecter
- `-c` : effectue des opérations en utilisant un type de données caractères
- `-t` : spécifie la marque de fin de champ. Nous utilisons `comma` en tant que marque de fin de champ pour les enregistrements dans notre fichier de données

> [!NOTE]
> Dans cet exemple, nous ne spécifions pas de marque de fin de ligne personnalisé. Les lignes du fichier de données texte étaient correctement terminées par `newline` lorsque nous avons utilisé la commande `cat` pour créer le fichier de données précédemment.

Vous pouvez vérifier que les données ont été correctement importées en exécutant la commande suivante dans la fenêtre du terminal. N’oubliez pas de remplacer le `username` et le `<your_password>`, si nécessaire, avant d’exécuter la commande.
```bash 
sqlcmd -S localhost -d BcpSampleDB -U sa -P <your_password> -I -Q "SELECT * FROM TestEmployees;"
```

Les résultats suivants doivent s’afficher :
```bash
Id          Name                Location
----------- ------------------- -------------------
          1 Jared               Australia
          2 Nikita              India
          3 Tom                 Germany

(3 rows affected)
```

## <a name="export-data-with-bcp"></a>Exporter des données avec bcp

Dans ce didacticiel, vous allez utiliser `bcp` pour exporter des données à partir de la table d’exemple que nous avons créée précédemment dans un nouveau fichier de données.

Copiez et collez les commandes suivantes dans la fenêtre du terminal. Ces commandes utilisent l'utilitaire de ligne de commande `bcp` pour exporter des données à partir de la table **TestEmployees** dans la base de données **BcpSampleDB** vers un nouveau fichier de données appelé **~/test_export.txt**.  N’oubliez pas de remplacer le nom d’utilisateur et le `<your_password>`, si nécessaire, avant d’exécuter la commande.

```bash 
bcp TestEmployees out ~/test_export.txt -S localhost -U sa -P <your_password> -d BcpSampleDB -c -t ','
```

Vous pouvez vérifier que les données ont été exportées correctement en exécutant la commande suivante dans la fenêtre du terminal :
```bash 
cat ~/test_export.txt
```

Ce qui suit s’affiche dans la fenêtre du terminal :
```
1,Jared,Australia
2,Nikita,India
3,Tom,Germany
```

## <a name="see-also"></a>Voir aussi
- [Utilitaire bcp](../tools/bcp-utility.md)
- [Formats de données pour la compatibilité lors de l’utilisation de bcp](../relational-databases/import-export/specify-data-formats-for-compatibility-when-using-bcp-sql-server.md)
- [Importer des données en bloc à l’aide de BULK INSERT](../relational-databases/import-export/import-bulk-data-by-using-bulk-insert-or-openrowset-bulk-sql-server.md)
- [BULK INSERT (Transact-SQL)](../t-sql/statements/bulk-insert-transact-sql.md)
