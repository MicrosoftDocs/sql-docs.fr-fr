---
title: Évaluer SQL Server préparation à migrer vers Azure SQL Database
titleSuffix: Data Migration Assistant
description: Découvrez comment utiliser Assistant Migration de données pour migrer un SQL Server de données pour la migration vers Azure SQL Database
ms.date: 12/19/2019
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
ms.openlocfilehash: 4c59fb5228ef46a693e343716d1e58e56c042a53
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100061554"
---
# <a name="assess-the-readiness-of-a-sql-server-data-estate-migrating-to-azure-sql-database-using-the-data-migration-assistant"></a>Évaluer la préparation d’un SQL Server de données qui migrent vers Azure SQL Database à l’aide de la Assistant Migration de données

La migration de centaines d’instances de SQL Server et de milliers de bases de données vers Azure SQL Database, notre offre PaaS (Platform as a service), est une tâche considérable. Pour simplifier autant que possible le processus, vous devez être certain de votre disponibilité relative pour la migration. Identifier les fruits à faible baisse, y compris les serveurs et les bases de données qui sont entièrement prêts ou qui nécessitent un minimum d’effort pour préparer la migration, facilite et accélère vos efforts.

Cet article fournit des instructions pas à pas pour tirer parti de la [Assistant Migration de données](./dma-overview.md) pour résumer les résultats de préparation et les faire apparaître sur le Hub [Azure Migrate](https://portal.azure.com/?feature.customPortal=false#blade/Microsoft_Azure_Migrate/AmhResourceMenuBlade/overview) .

>
> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/Data-Migration-Assistant/player?WT.mc_id=dataexposed-c9-niner]

## <a name="create-a-project-and-add-a-tool"></a>Créez un projet et ajouter un outil

Configurez un nouveau projet de Azure Migrate dans un abonnement Azure, puis ajoutez un outil.

Un projet de Azure Migrate est utilisé pour stocker les métadonnées de découverte, d’évaluation et de migration collectées à partir de l’environnement que vous évaluez ou migrez. Vous utilisez également un projet pour suivre les ressources découvertes et orchestrer l’évaluation et la migration.

1. Connectez-vous au Portail Azure, sélectionnez **tous les services**, puis recherchez Azure Migrate.
2. Sous **Services**, sélectionnez **Azure Migrate**.

   ![Azure Migrate-sélectionner un service](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-azure-migrate-services.png)

3. Sur la page **vue d’ensemble** , sélectionnez **évaluer et migrer des bases de données**.

   ![Azure Migrate-lancer l’évaluation](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-azure-migrate-hub-assess.png)

4. Dans **bases de données**, sous **mise** en route, sélectionnez **Ajouter un ou plusieurs outils**.

   ![Azure Migrate-ajouter des outils](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-azure-migrate-add-tools.png)

5. Sous l’onglet **migrer un projet** , sélectionnez votre abonnement Azure et votre groupe de ressources (si vous ne disposez pas déjà d’un groupe de ressources, créez-en un).
6. Sous **Détails du projet**, spécifiez le nom du projet et la géographie dans laquelle vous souhaitez créer le projet.

    ![Azure Migrate boîte de dialogue Ajouter un outil](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-azure-migrate-add-tool-dialog.png)

    Vous pouvez créer un projet Azure Migrate dans les zones géographiques suivantes.

    | **Zone géographique**  | **Région de l’emplacement de stockage** |
    | ------------- | ------------- |
    | Asia | Asie Sud-Est ou Asie Est |
    | Europe | Europe Sud ou Europe Ouest |
    | Royaume-Uni | Royaume-Uni Sud ou Royaume-Uni Ouest |
    | États-Unis | USA Centre ou USA Ouest 2 |

    La zone géographique spécifiée pour le projet est utilisée uniquement pour stocker les métadonnées collectées à partir des machines virtuelles locales. Vous pouvez sélectionner n’importe quelle région cible pour la migration réelle.

7. Sélectionnez **suivant**, puis ajouter un outil d’évaluation.

   > [!NOTE]
   > Lorsque vous créez un projet, vous devez ajouter au moins un outil d’évaluation ou de migration.

8. Dans l’onglet **Sélectionner l’outil d’évaluation** , **Azure Migrate : l’évaluation de la base de données** s’affiche comme outil d’évaluation à ajouter. Si vous n’avez pas besoin d’un outil d’évaluation, activez la case à cocher **ignorer l’ajout d’un outil d’évaluation pour l’instant** . Sélectionnez **Suivant**.

    ![Azure Migrate-sélectionner l’onglet outil d’évaluation](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-azure-migrate-select-assessment-tool.png)

9. Dans l’onglet **Sélectionner l’outil de migration** , **Azure Migrate : la migration de base de données** apparaît en tant qu’outil de migration à ajouter. Si vous n’avez pas besoin d’un outil de migration, sélectionnez l’option **ignorer l’ajout d’un outil de migration pour l’instant**. Sélectionnez **Suivant**.

    ![Azure Migrate-sélectionner l’onglet outil de migration](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-azure-migrate-select-migration-tool.png)

10. Dans **examiner + ajouter des outils**, passez en revue les paramètres, puis sélectionnez **Ajouter des outils**.

    ![Onglet Azure Migrate-passer en revue + ajouter un ou plusieurs outils](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-azure-migrate-review-tools.png)

    Après avoir créé le projet, vous pouvez sélectionner des outils supplémentaires pour l’évaluation et la migration des serveurs, des charges de travail, des bases de données et des applications web.

## <a name="assess-and-upload-assessment-results"></a>Évaluer et télécharger les résultats de l’évaluation

Une fois que vous avez créé un projet de migration, sous **Outils d’évaluation**, dans la zone **Azure Migrate : évaluation de la base de données** , vous trouverez des instructions pour télécharger et utiliser l’outil Assistant Migration de données.

   ![Outil d’évaluation de Azure Migrate ajouté](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-azure-migrate-assessment-tool-added.png)

1. Téléchargez Assistant Migration de données à l’aide du lien fourni, puis installez-le sur un ordinateur ayant accès à vos instances de SQL Server sources.
2. Démarrez l’Assistant Migration de données.

### <a name="create-an-assessment"></a>Créer une évaluation

1. Sur la gauche, sélectionnez l' **+** icône, puis sélectionnez le type de **projet** évaluation.
2. Spécifiez le nom du projet, puis sélectionnez les types serveur source et serveur cible.

    Si vous effectuez la mise à niveau de votre instance locale de SQL Server vers une version ultérieure de SQL Server ou vers SQL Server hébergé sur une machine virtuelle Azure, définissez le type de serveur source et cible sur **SQL Server**. Définissez le type de serveur cible sur **Azure SQL Managed instance** pour une évaluation de la disponibilité cible du Azure SQL Database (PaaS).

3. Sélectionnez **Create** (Créer).

   ![Interface Azure Migrate Assistant Migration de données](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-dma-interface.png)

### <a name="choose-assessment-options"></a>Choisir les options d’évaluation

1. Sélectionnez le type de rapport.

    Vous pouvez choisir l’un des types de rapports suivants ou les deux :
    * Vérifier la compatibilité de la base de données
    * Vérifier la parité de fonctionnalité

   ![Écran des options d’évaluation de l’Assistant Migration de données Azure Migrate](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-dma-options-screen.png)

2. Sélectionnez **Suivant**.

### <a name="add-databases-to-assess"></a>Ajouter des bases de données à évaluer

1. Sélectionnez **Ajouter des sources** pour ouvrir le menu de la connexion.
2. Entrez le nom de l’instance SQL Server, choisissez le type d’authentification, définissez les propriétés de connexion appropriées, puis sélectionnez **se connecter**.
3. Sélectionnez les bases de données à évaluer, puis sélectionnez **Ajouter**.

   > [!NOTE]
   > Vous pouvez supprimer plusieurs bases de données en les sélectionnant tout en maintenant la touche Maj ou CTRL enfoncée, puis en cliquant sur supprimer les sources. Vous pouvez également ajouter des bases de données à partir de plusieurs instances de SQL Server à l’aide du bouton Ajouter des sources.

4. Sélectionnez **Suivant** pour démarrer l’évaluation.

   ![Écran de sélection des sources Azure Migrate Assistant Migration de données](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-dma-select-sources-screen.png)

5. Une fois l’évaluation terminée, sélectionnez **charger pour Azure Migrate**.

   ![Capture d’écran montrant le Assistant Migration de données avec l’option Télécharger vers Azure Migrate appelée out.](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-dma-review-results-screen.png)

6. Connectez-vous au portail Azure.

   ![Capture d’écran de la Assistant Migration de données montrant la fenêtre de connexion Portail Azure.](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-azure-migrate-portal-signin.png)

7. Sélectionnez l’abonnement et le projet Azure Migrate dans lequel vous souhaitez télécharger les résultats de l’évaluation, puis sélectionnez **Télécharger**.

   Attendez la confirmation du téléchargement de l’évaluation.

## <a name="view-target-readiness-assessment-results"></a>Afficher les résultats de l’évaluation de la disponibilité cible

1. Connectez-vous au Portail Azure, recherchez Azure Migrate, puis sélectionnez **Azure Migrate**.

   ![Azure Migrate-Portail Azure-Service Search](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-azure-migrate-portal-search.png)

2. Sélectionnez **évaluer et migrer les bases de données** pour accéder aux résultats de l’évaluation.

   ![Azure Migrate-examiner les résultats de l’évaluation](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-azure-migrate-hub-assess.png)

    Vous pouvez afficher le résumé de préparation de l’SQL Server, vous assurer que vous êtes dans le projet de migration approprié, sinon, utilisez l’option modifier pour sélectionner un autre projet de migration.

    Chaque fois que vous mettez à jour les résultats de l’évaluation dans Azure Migrate Project, Azure Migrate Hub consolider tous les résultats et fournir le rapport de synthèse.  Vous pouvez exécuter plusieurs évaluations de Assistant Migration de données en parallèle et charger les résultats dans le projet de migration unique pour obtenir le rapport de disponibilité consolidé.

   ![Azure Migrate-vérifier les résultats de la préparation](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-azure-migrate-review-readiness.png)

    **Instances de base de données évaluées**: nombre d’instances de SQL Server évaluées jusqu’à présent.
    **Bases de données évaluées**: nombre total de bases de données évaluées sur une ou plusieurs instances SQL Server **les bases de données préparées pour SQL Database**: nombre de bases de données prêtes à migrer vers Azure SQL Database (PaaS).
    **Bases de données prêtes pour la machine virtuelle SQL Azure**: le nombre de bases de données est constitué d’un ou de plusieurs bloqueurs de migration vers Azure SQL Database (PaaS), mais prêt à migrer vers des machines virtuelles Azure SQL Server.

3. Sélectionnez **instances de base de données évaluées** pour accéder à SQL Server affichage au niveau de l’instance.

   ![Azure Migrate-vérifier la préparation des instances](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-azure-migrate-assessed-instances.png)

    Vous pouvez trouver l’état de disponibilité en pourcentage de chaque SQL Server instance migrant vers Azure SQL Database (PaaS).

4. Sélectionnez une instance spécifique pour accéder à la vue de préparation de la base de données.

   ![Azure Migrate-vérifier la préparation de la base de données](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-azure-migrate-assessed-databases.png)

    Vous pouvez voir le nombre de bloqueurs de migration pour chaque base de données, la cible recommandée pour chaque base de données dans l’affichage ci-dessus.

5. Passez en revue les résultats de l’évaluation DMA pour obtenir plus de détails sur les bloqueurs de migration.

   ![Azure Migrate-examiner les bloqueurs de migration](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-azure-migrate-migration-blockers.png)

## <a name="see-also"></a>Voir également

* [Assistant Migration de données (DMA)](../dma/dma-overview.md)
* [Assistant Migration de données : paramètres de configuration](../dma/dma-configurationsettings.md)
* [Assistant Migration de données : meilleures pratiques](../dma/dma-bestpractices.md)
