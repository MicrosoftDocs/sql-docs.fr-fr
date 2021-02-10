---
title: Migrer le schéma de ressources humaines Oracle vers SQL Server sur Linux | Microsoft Docs
description: Convertir un exemple de schéma Oracle en SQL Server sur Linux
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.date: 10/02/2017
ms.topic: conceptual
ms.prod: sql
ms.custom: ''
ms.technology: ssma
ms.openlocfilehash: fb21fdc7da82d6dae2390ddcf0884ea1e8ac8a58
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100044389"
---
# <a name="migrate-an-oracle-schema-to-sql-server-2017-on-linux-with-the-sql-server-migration-assistant"></a>Migrer un schéma Oracle vers SQL Server 2017 sur Linux avec le Assistant Migration SQL Server

Ce didacticiel utilise Assistant Migration SQL Server (SSMA) pour Oracle sur Windows pour convertir le schéma des **RH** des exemples oracle en [SQL Server 2017 sur Linux](../../linux/sql-server-linux-overview.md).

> [!div class="checklist"]
> * Télécharger et installer SSMA sur Windows
> * Créer un projet SSMA pour gérer la migration
> * Connexion à Oracle
> * Exécuter un rapport de migration
> * Convertir l’exemple de schéma RH
> * Migrer les données

## <a name="prerequisites"></a>Prérequis

- Une instance d’Oracle 12C (12.2.0.1.0) avec le schéma **HR** installé
- Une instance de travail de SQL Server sur Linux

> [!NOTE]
> La même procédure peut être utilisée pour cibler SQL Server sur Windows, mais vous devez sélectionner Windows dans le paramètre **migrer vers** le projet.

## <a name="download-and-install-ssma-for-oracle"></a>Télécharger et installer SSMA pour Oracle

Plusieurs éditions de Assistant Migration SQL Server sont disponibles, en fonction de votre base de données source.  Téléchargez la version actuelle de [Assistant Migration SQL Server pour Oracle](https://aka.ms/ssmafororacle) et installez-la à l’aide des instructions figurant sur la page de téléchargement.

> [!NOTE]
> À ce stade, le **Pack d’extension SSMA pour Oracle** n’est pas pris en charge sur Linux, mais il n’est pas nécessaire pour ce didacticiel.

## <a name="create-and-set-up-project"></a>Créer et configurer un projet

Pour créer un nouveau projet SSMA, procédez comme suit :

1. Ouvrez SSMA pour Oracle et choisissez **nouveau projet** dans le menu **fichier** .

1. Nommez le projet.

1. Choisissez « SQL Server 2017 (Linux)-Preview » dans le champ **migrer vers** .

SSMA pour Oracle n’utilise pas les exemples de schémas Oracle par défaut. Pour activer le schéma HR, procédez comme suit :

1. Dans SSMA, sélectionnez le menu **Outils** .

1. Sélectionnez **paramètres du projet par défaut**, puis choisissez **chargement des objets système**.

1. Assurez-vous que **HR** est coché, puis cliquez sur **OK**.

## <a name="connect-to-oracle"></a>Connexion à Oracle

Ensuite, connectez SSMA à Oracle.

1. Dans la barre d’outils, cliquez sur **se connecter à Oracle**.

1. Entrez le nom du serveur, le port, le SID Oracle, le nom d’utilisateur et le mot de passe.

   ![Connexion à Oracle](./media/sql-server-linux-convert-from-oracle/ConnectToOracle.png)

1. Puis, cliquez sur **Se connecter**. Dans quelques instants, SSMA pour Oracle se connecte à votre base de données et lit ses métadonnées.

## <a name="create-a-report"></a>Créer un rapport

Pour générer un rapport de migration, procédez comme suit.

1. Dans l **'Explorateur de métadonnées Oracle**, développez le nœud de votre serveur.

1. Développez **schémas**, cliquez avec le bouton droit sur **RH**, puis sélectionnez **créer un rapport**.

   ![Créer un rapport dans l’Explorateur de métadonnées Oracle](./media/sql-server-linux-convert-from-oracle/CreateReport.png)

1. Une nouvelle fenêtre de navigateur s’ouvre avec un rapport qui répertorie tous les avertissements et erreurs associés à la conversion.

   > [!NOTE]
   > Vous n’avez rien à faire avec cette liste pour ce didacticiel. Si vous effectuez ces étapes pour votre propre base de données Oracle, vous devez examiner le rapport pour résoudre les problèmes de conversion importants de votre base de données.

   ![Exemple de rapport de migration](./media/sql-server-linux-convert-from-oracle/SSMAReport.png)

## <a name="connect-to-sql-server"></a>Se connecter à SQL Server

Choisissez ensuite **se connecter à SQL Server** et entrez les informations de connexion appropriées.  Si vous utilisez un nom de base de données qui n’existe pas encore, SSMA pour Oracle le crée pour vous.

![Se connecter à SQL Server](./media/sql-server-linux-convert-from-oracle/ConnectToSQLServer.png)

## <a name="convert-schema"></a>Convertir le schéma

Cliquez avec le bouton droit sur **HR** dans l' **Explorateur de métadonnées Oracle**, puis choisissez convertir le schéma.

![Convertir le schéma](./media/sql-server-linux-convert-from-oracle/ConvertSchema.png)

## <a name="synchronize-database"></a>Synchroniser la base de données

Ensuite, synchronisez votre base de données.

1. Une fois la conversion terminée, utilisez l' **Explorateur de métadonnées SQL Server** pour accéder à la base de données que vous avez créée à l’étape précédente.

1. Cliquez avec le bouton droit sur votre base de données, sélectionnez **synchroniser avec la base de données**, puis cliquez sur OK.

   ![Synchroniser avec la base de données](./media/sql-server-linux-convert-from-oracle/SynchronizeWithDatabase.png)

## <a name="migrate-data"></a>Migration des données

La dernière étape consiste à migrer vos données.

1. Dans l **'Explorateur de métadonnées Oracle**, cliquez avec le bouton droit sur **HR**, puis sélectionnez **migrer des données**.

1. L’étape de migration des données vous oblige à entrer à nouveau vos informations d’identification Oracle et SQL Server.

1. Lorsque vous avez terminé, examinez le rapport de migration de données, qui doit ressembler à la capture d’écran suivante :

   ![Rapport de migration des données](./media/sql-server-linux-convert-from-oracle/DataMigrationReport.png)

## <a name="next-steps"></a>Étapes suivantes

Pour un schéma Orcale plus complexe, le processus de conversion implique plus de temps, de tests et de modifications possibles pour les applications clientes. L’objectif de ce didacticiel est de montrer comment vous pouvez utiliser SSMA pour Oracle dans le cadre de votre processus de migration global.

Dans ce didacticiel, vous avez appris à :
> [!div class="checklist"]
> * Installer SSMA sur Windows
> * Créer un nouveau projet SSMA
> * Évaluer et exécuter une migration à partir d’Oracle

Explorez ensuite d’autres façons d’utiliser SSMA :

> [!div class="nextstepaction"]
>[Documentation Assistant Migration SQL Server](../sql-server-migration-assistant.md)
