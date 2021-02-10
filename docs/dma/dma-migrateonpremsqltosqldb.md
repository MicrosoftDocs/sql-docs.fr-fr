---
title: Migrer SQL Server vers Azure SQL Database à l’aide de la Assistant Migration de données
description: Découvrez comment utiliser Assistant Migration de données pour migrer un SQL Server local vers Azure SQL Database
ms.date: 06/29/2020
ms.prod: sql
ms.prod_service: dma
ms.reviewer: ''
ms.technology: dma
ms.topic: conceptual
keywords: ''
helpviewer_keywords:
- Data Migration Assistant, on-premises SQL Server
ms.assetid: ''
author: rajeshsetlem
ms.author: rajpo
ms.custom: seo-lt-2019
ms.openlocfilehash: 7bd846f4c4abd2a6b9fbcdc434964b789f32d047
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100030637"
---
# <a name="migrate-on-premises-sql-server-or-sql-server-on-azure-vms-to-azure-sql-database-using-the-data-migration-assistant"></a>Migrer des SQL Server locaux ou des SQL Server sur des machines virtuelles Azure vers Azure SQL Database à l’aide de la Assistant Migration de données

Le Assistant Migration de données fournit des évaluations transcohérentes de SQL Server locales et des mises à niveau vers des versions ultérieures de SQL Server ou des migrations vers SQL Server sur des machines virtuelles Azure ou des Azure SQL Database.

Cet article fournit des instructions pas à pas pour migrer SQL Server site local vers Azure SQL Database à l’aide de l’Assistant Migration de données.

## <a name="create-a-new-migration-project"></a>Créer un nouveau projet de migration

1. Dans le volet gauche, sélectionnez **nouveau** (+), puis sélectionnez le type de projet **migration** .

2. Définissez le type de source sur **SQL Server** et le type de serveur cible sur **Azure SQL Database**.

3. Sélectionnez **Create** (Créer).

   ![Créer un projet de migration](../dma/media/NewCreate1.png)

## <a name="specify-the-source-server-and-database"></a>Spécifier la base de données et le serveur sources

1. Pour la source, sous **se connecter au serveur source**, dans la zone de texte **nom du serveur** , entrez le nom de l’instance de SQL Server source.

2. Sélectionnez le **Type d’authentification** pris en charge par l’instance SQL Server source.

   > [!NOTE]
   > Il est recommandé de chiffrer la connexion en activant la case à cocher **chiffrer la connexion** sous **connexion poperties**.

    ![Sélectionner le serveur source](../dma/media/select-source-server.png)

3. Sélectionnez **Connecter**.

4. Sélectionnez une base de données source unique à migrer vers Azure SQL Database.

   > [!NOTE]
   > Si vous souhaitez évaluer la base de données et afficher et appliquer les corrections recommandées avant la migration, activez la case à cocher **évaluer la base de données avant la migration ?** .

    ![Sélectionner la base de données source](../dma/media/select-source-database.png)

5. Sélectionnez **Suivant**.

## <a name="specify-the-target-server-and-database"></a>Spécifier la base de données et le serveur cibles

1. Pour la cible, sous **se connecter au serveur cible**, dans la zone de texte **nom du serveur** , entrez le nom de l’instance de Azure SQL Database. 

2. Sélectionnez le **type d’authentification** pris en charge par l’instance de Azure SQL Database cible.

   > [!NOTE]
   > Il est recommandé de chiffrer la connexion en activant la case à cocher **chiffrer la connexion** sous **connexion poperties**.

     ![Sélectionner le serveur cible](../dma/media/select-target-server.png)

3. Sélectionnez **Connecter**.

4. Sélectionnez une base de données cible unique vers laquelle effectuer la migration.

   > [!NOTE]
   > Si vous envisagez de migrer des utilisateurs Windows, dans la zone de texte nom de domaine de l' **utilisateur externe cible** , assurez-vous que le nom de domaine de l’utilisateur externe cible est correctement spécifié.

    ![Sélectionner la base de données cible](../dma/media/select-target-database.png)

5. Sélectionnez **Suivant**.

## <a name="select-schema-objects"></a>Sélectionner les objets de schéma

1. Sélectionnez dans la base de données source les objets de schéma que vous souhaitez migrer vers Azure SQL Database.

    ![Sélectionner les objets de schéma](../dma/media/select-schema-objects.png)

    > [!NOTE]
    > Certains des objets qui ne peuvent pas être convertis tels quels sont présentés avec des opportunités de correctif automatique. Un clic sur ces objets dans le volet gauche permet d’afficher les suggestions de correctif dans le volet droit. Passez en revue les correctifs et choisissez d’appliquer ou d’ignorer toutes les modifications, objet par objet. Notez que le fait d’appliquer ou d’ignorer toutes les modifications pour un objet n’affecte pas les modifications apportées aux autres objets de base de données. Les instructions qui ne peuvent pas être converties ni corrigées automatiquement sont reproduites dans la base de données cible et commentées.

    ![Correction suggérée](../dma/media/suggested-fix.png)

2. Sélectionnez **script SQL général**.

3. Examinez le script généré.

    ![Script généré](../dma/media/generated-script.png)

## <a name="deploy-schema"></a>Déployer le schéma

1. Sélectionnez **déployer le schéma**.

2. Passez en revue les résultats du déploiement de schéma.

    ![Résultats du déploiement du schéma](../dma/media/schema-deployment-results.png)

3. Sélectionnez **migrer des données** pour lancer le processus de migration des données.

4. Sélectionnez les tables contenant les données que vous souhaitez migrer.

    ![Sélectionner les tables à migrer](../dma/media/select-tables-to-migrate.png) 

5. Sélectionnez **Démarrer la migration des données**.

Le dernier écran montre l’état global.

   ![état de la migration](../dma/media/migration-status.png) 

## <a name="see-also"></a>Voir également

* [Assistant Migration de données (DMA)](../dma/dma-overview.md)
* [Assistant Migration de données : paramètres de configuration](../dma/dma-configurationsettings.md)
* [Assistant Migration de données : meilleures pratiques](../dma/dma-bestpractices.md)
