---
title: Exporter et importer une base de données sur Linux
description: Cet article explique comment utiliser SQL Server Management Studio et SqlPackage.exe pour exporter et importer une base de données sur SQL Server sur Linux.
author: VanMSFT
ms.author: vanto
ms.date: 10/02/2017
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 2210cfc3-c23a-4025-a551-625890d6845f
ms.openlocfilehash: 40db493908ab0163a973fb46e885d113195b4da5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339159"
---
# <a name="export-and-import-a-database-on-linux-with-ssms-or-sqlpackageexe-on-windows"></a>Exporter et importer une base de données sur Linux avec SSMS ou SqlPackage.exe sur Windows

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Cet article explique comment utiliser [SQL Server Management Studio (SSMS)](../ssms/download-sql-server-management-studio-ssms.md) et [SqlPackage.exe](../tools/sqlpackage/sqlpackage.md) pour exporter et importer une base de données sur SQL Server sur Linux. SSMS et SqlPackage.exe sont des applications Windows. Utilisez donc cette technique quand vous disposez d’une machine Windows capable de se connecter à une instance distante sur Linux.

Vous devez toujours installer et utiliser la version la plus récente de SQL Server Management Studio (SSMS), comme décrit dans [Utiliser SSMS sur Windows pour se connecter à SQL Server sur Linux](sql-server-linux-manage-ssms.md)

> [!NOTE]
> Si vous migrez une base de données d’une instance vers une autre, il est recommandé d’utiliser [Sauvegarder et restaurer](sql-server-linux-migrate-restore-database.md).

## <a name="export-a-database-with-ssms"></a>Exporter une base de données avec SSMS

1. Démarrez SSMS en tapant **Microsoft SQL Server Management Studio** dans la zone de recherche Windows, puis cliquez sur l’application de bureau.

    ![SQL Server Management Studio](./media/sql-server-linux-manage-ssms/ssms.png) 

2. Connectez-vous à votre base de données source dans l’Explorateur d’objets. La base de données source peut se trouver dans Microsoft SQL Server en local ou dans le cloud, sur Linux, Windows ou Docker, ainsi que dans Azure SQL Database ou Azure Synapse Analytics.

3. Cliquez avec le bouton de droite sur la base de données source dans l'Explorateur d’objets, pointez sur **Tâches**, puis cliquez sur **Exporter une application de la couche Données...**

4. Dans l’Assistant exportation, cliquez sur **Suivant**, puis dans l'onglet **Paramètres**, configurez l’exportation pour enregistrer le fichier BACPAC dans un emplacement de disque local ou dans un blob Azure.

5. Par défaut, tous les objets de la base de données sont exportés. Cliquez sur **l’onglet Avancé** et sélectionnez les objets de base de données que vous souhaitez exporter.

6. Cliquez sur **Suivant** , puis sur **Terminer**.

Le fichier *.BACPAC a été créé avec succès à l’emplacement que vous avez choisi et vous êtes prêt à l’importer dans une base de données cible.

## <a name="import-a-database-with-ssms"></a>Importer une base de données avec SSMS

1. Démarrez SSMS en tapant **Microsoft SQL Server Management Studio** dans la zone de recherche Windows, puis cliquez sur l’application de bureau.

    ![SQL Server Management Studio](./media/sql-server-linux-manage-ssms/ssms.png) 

2. Connectez-vous à votre serveur cible dans l’Explorateur d’objets. La serveur cible peut être Microsoft SQL Server en local ou dans le cloud, sur Linux, Windows ou Docker, ainsi qu’Azure SQL Database ou Azure Synapse Analytics.

3. Cliquez avec le bouton de droite sur le dossier **Bases de données** dans l'Explorateur d’objets, puis cliquez sur **Importer une application de la couche Données...**

4. Pour créer la base de données sur votre serveur cible, spécifiez un fichier BACPAC à partir de votre disque local ou sélectionnez le compte de stockage Azure et le conteneur dans lequel vous avez téléchargé votre fichier BACPAC.

5. Fournissez un nouveau nom pour la base de données. Si vous importez une base de données sur Azure SQL Database, définissez l’édition de Microsoft Azure SQL Database (niveau de service), la taille maximale de la base de données et l’objectif de service (niveau de performance).

6. Cliquez sur **Suivant**, puis sur **Terminer** pour importer le fichier BACPAC dans une nouvelle base de données sur votre serveur cible.

Le fichier *.BACPAC est importé pour créer une nouvelle base de données dans le serveur cible que vous avez spécifié.

## <a name="sqlpackage-command-line-option"></a><a id="sqlpackage"></a> Option de ligne de commande SqlPackage

Il est également possible d’utiliser l’outil en ligne de commande SQL Server Data Tools (SSDT), [SqlPackage.exe](../tools/sqlpackage/sqlpackage.md), pour exporter et importer des fichiers BACPAC.

L’exemple de commande suivant exporte un fichier BACPAC :

```bash
SqlPackage.exe /a:Export /ssn:tcp:<your_server> /sdn:<your_database> /su:<username> /sp:<password> /tf:<path_to_bacpac>
```

Utilisez la commande suivante pour importer le schéma de base de données et les données utilisateur à partir d’un fichier .BACPAC :

```bash
SqlPackage.exe /a:Import /tsn:tcp:<your_server> /tdn:<your_database> /tu:<username> /tp:<password> /sf:<path_to_bacpac>

```

## <a name="see-also"></a>Voir aussi
Pour plus d’informations sur l’utilisation de SSMS, consultez [Utiliser SQL Server Management Studio](../ssms/sql-server-management-studio-ssms.md). Pour plus d’informations sur SqlPackage.exe, consultez la [documentation de référence SqlPackage](../tools/sqlpackage/sqlpackage.md).