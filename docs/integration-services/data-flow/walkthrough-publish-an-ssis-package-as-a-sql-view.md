---
description: 'Procédure pas à pas : Publication d’un package SSIS sous la forme d’une vue SQL'
title: 'Procédure pas à pas : publier un package SSIS en tant que vue SQL | Microsoft Docs'
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.ssis.packagepublishwizard.f1
ms.assetid: d32d9761-93fb-4020-bf82-231439c6f3ac
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 0a9c522bf292147852203fb9ac740e7103195b35
ms.sourcegitcommit: 868c60aa3a76569faedd9b53187e6b3be4997cc9
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/08/2021
ms.locfileid: "99835990"
---
# <a name="walkthrough-publish-an-ssis-package-as-a-sql-view"></a>Procédure pas à pas : publier un package SSIS en tant que vue SQL

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Cette procédure pas à pas fournit des étapes détaillées sur la publication d’un package SSIS en tant que vue SQL dans une base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="prerequisites"></a>Conditions préalables requises  
 Pour pouvoir effectuer cette procédure pas à pas, vous devez disposer des logiciels suivants installés sur votre ordinateur.  
  
1.  [!INCLUDE[ssNoVersion ](../../includes/ssnoversion-md.md)] avec [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
2.  [SQL Server Data Tools](../../ssdt/download-sql-server-data-tools-ssdt.md).  
  
## <a name="step-1-build-and-deploy-ssis-project-to-the-ssis-catalog"></a>Étape 1 : créer et déployer le projet SSIS dans le catalogue SSIS  
 Dans cette étape, vous créez un package SSIS qui extrait les données d’une source de données SSIS prise en charge (dans cet exemple, nous utilisons une base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ) et qui produit en sortie des données en utilisant un composant Data Streaming Destination. Vous générez et déployez ensuite le projet SSIS dans le catalogue SSIS.  
  
1.  Lancez **SQL Server Data Tools**. Dans le menu **Démarrer** , pointez sur **Tous les programmes**, puis sur **Microsoft SQL Server** et cliquez sur **SQL Server Data Tools**.  
  
2.  Créez un projet [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
    1.  Dans la barre de menus, cliquez sur **Fichier** , pointez sur **Nouveau**, puis cliquez sur **Projet**.  
  
    2.  Développez **Business Intelligence** dans le volet gauche, puis cliquez sur **Integration Services** dans l’arborescence.  
  
    3.  Sélectionnez **Projet Integration Services** , si cette option n’est pas déjà sélectionnée.  
  
    4.  Spécifiez **SSISPackagePublishing** comme **nom de projet**.  
  
    5.  Spécifiez un emplacement pour le projet.  
  
    6.  Cliquez sur **OK** pour fermer la boîte de dialogue **Nouveau projet** .  
  
3.  Faites glisser le composant **Flux de données** de la **Boîte à outils SSIS** vers l’aire de conception de l’onglet **Flux de contrôle** .  
  
4.  Double-cliquez sur le composant **Flux de données** dans **Flux de contrôle** pour ouvrir le **Concepteur de flux de données**.  
  
5.  Faites glisser un **composant source** de la boîte à outils vers le **Concepteur de flux de données** , puis configurez-le pour extraire les données d’une source de données.  
  
    1.  Pour les besoins de la procédure pas à pas, créez une base de données de test : **TestDB** avec une table : **Employee**. Créez la table avec trois colonnes, **ID**, **FirstName** et **LastName**.  
  
    2.  Définissez **ID** en tant que clé primaire.  
  
    3.  Insérez deux enregistrements avec les données suivantes.  
  
        |id|FIRSTNAME|LASTNAME|  
        |--------|---------------|--------------|  
        |1|John|Doe|  
        |2|Jane|Doe|  
  
    4.  Faites glisser le composant **Source OLE DB** de la **Boîte à outils SSIS** vers le **Concepteur de flux de données**.  
  
    5.  Configurez le composant pour extraire les données de la table **Employee** dans la base de données **TestDB** . Sélectionnez **(local).TestDB** pour le **Gestionnaire de connexions OLE DB**, **Table ou vue** pour le **Mode d’accès aux données**, et **[dbo].[Employee]** pour **Nom de la table ou de la vue**.  
  
         ![Data Streaming Destination - Connexion OLE DB](../../integration-services/data-flow/media/dsd-oledbconnectionmanager.jpg "Data Streaming Destination - Connexion OLE DB")  
  
6.  À présent, faites glisser **Data Streaming Destination** de la boîte à outils vers le flux de données. Vous trouverez ce composant dans la section Commun de la boîte à outils.  
  
7.  Connectez le composant **Source OLE DB** du flux de données au composant **Data Streaming Destination** .  
  
8.  Créez et déployez le projet SSIS dans le catalogue SSIS.  
  
    1.  Dans la barre de menus, cliquez sur **Projet** , puis sur **Déployer**.  
  
    2.  Suivez les instructions de l’Assistant pour déployer le projet dans le catalogue SSIS sur le serveur de base de données local. L’exemple suivant utilise **Power BI** comme nom de dossier et **SSISPackagePublishing** comme nom de projet dans le catalogue SSIS.  
  
## <a name="step-2-use-the-ssis-data-feed-publishing-wizard-to-publish-ssis-package-as-a-sql-view"></a>Étape 2 : utiliser l’Assistant Publication de flux de données SSIS pour publier le package SSIS en tant que vue SQL  
 Au cours de cette étape, vous utilisez l’Assistant Publication de flux de données SQL Server Integration Services (SSIS) pour publier le package SSIS en tant que vue dans une base de données SQL Server. Les données de sortie du package peuvent être consommées par l’interrogation de cette vue.  
  
 L’Assistant Publication de flux de données SSIS crée un serveur lié à l’aide du fournisseur OLE DB pour SSIS (SSISOLEDB), puis crée une vue SQL qui se compose d’une requête sur le serveur lié. Cette requête inclut le nom du dossier, le nom du projet et le nom du package dans le catalogue SSIS.  
  
 Au moment de l’exécution, la vue envoie la requête au fournisseur OLE DB pour SSIS à l’aide du serveur lié que vous avez créé. Le fournisseur OLE DB pour SSIS exécute le package que vous avez spécifié dans la requête, puis retourne le jeu de résultats tabulaire à la requête.  
  
1.  Lancez l’Assistant **Publication de flux de données SSIS** en exécutant ISDataFeedPublishingWizard.exe depuis C:\Program Files\Microsoft SQL Server\130\DTS\Binn ou en cliquant sur Microsoft SQL Server 2016\Assistant Publication de flux de données de SQL Server 2016 sous Démarrer\Tous les programmes.  
  
2.  Cliquez sur **Suivant** dans la page **Introduction** .  
  
     ![Assistant Publication du flux de données - Page Introduction](../../integration-services/data-flow/media/dsd-feedpublishingwizard-introductionpage.jpg "Assistant Publication du flux de données - Page Introduction")  
  
3.  Dans la page **Paramètres du package** , effectuez les tâches suivantes :  
  
    1.  Tapez le **nom** de l’instance SQL Server qui contient le catalogue SSIS, ou cliquez sur **Parcourir** pour sélectionner le serveur.  
  
         ![Assistant Publication du flux de données - Page Paramètres de package](../../integration-services/data-flow/media/dsd-feedpublishingwizard-packagesettingspage.jpg "Assistant Publication du flux de données - Page Paramètres de package")  
  
    2.  Cliquez sur **Parcourir** en regard du champ Chemin, parcourez le catalogue SSIS, sélectionnez le package SSIS à publier (par exemple : **SSISDB**->**SSISPackagePublishing**->**Package.dtsx**), puis cliquez sur **OK**.  
  
         ![Assistant Publication du flux de données - Rechercher le package](../../integration-services/data-flow/media/dsd-feedpublishingwizard-browseforpackage.jpg "Assistant Publication du flux de données - Rechercher le package")  
  
    3.  À l’aide des onglets Paramètres du package, Paramètres du projet et Gestionnaires de connexions situés au bas de la page, entrez les valeurs appropriées pour les paramètres de package, les paramètres de projet ou les paramètres de gestionnaires de connexions du package. Vous pouvez également indiquer une référence d’environnement à utiliser pour l’exécution du package, et lier les paramètres de package/projet aux variables d’environnement.  
  
         Nous vous recommandons de lier les paramètres sensibles aux variables d’environnement. Cela vous permet de vous assurer que la valeur d’un paramètre sensible n’est pas stockée au format texte brut dans la vue SQL créée par l’Assistant.  
  
    4.  Cliquez sur **Suivant** pour passer à la page **Paramètres de publication** .  
  
4.  Dans la page **Paramètres de publication** , effectuez les tâches suivantes :  
  
    1.  Sélectionnez la **base de données** correspondant à la vue à créer.  
  
         ![Assistant Publication du flux de données - Page Paramètres de publication](../../integration-services/data-flow/media/dsd-feedpublishingwizard-publishsettingspage.jpg "Assistant Publication du flux de données - Page Paramètres de publication")  
  
    2.  Tapez un **nom** pour la **vue**. Vous pouvez également sélectionner une vue existante dans la liste déroulante.  
  
    3.  Dans la liste **Paramètres** , spécifiez le **nom** du **serveur lié** à associer à la vue. S’il n’existe pas déjà de serveur lié, l’Assistant le crée avant de créer la vue. Vous pouvez également définir ici des valeurs pour **User32BitRuntime** et **Timeout** .  
  
    4.  Cliquez sur le bouton **Avancé** . Vous devez voir la boîte de dialogue **Paramètres avancés** .  
  
    5.  Dans la boîte de dialogue **Paramètres avancés** , procédez comme suit :  
  
        1.  Spécifiez le schéma de la base de données dans lequel vous souhaitez créer la vue (champ Schéma).  
  
        2.  Spécifiez si les données doivent être chiffrées avant d’être envoyées sur le réseau (champ Chiffrer). Consultez la rubrique [Utilisation du chiffrement sans validation](../../relational-databases/native-client/features/using-encryption-without-validation.md) pour plus d’informations sur ce paramètre et sur le paramètre TrustServerCertificate.  
  
        3.  Spécifiez si un certificat de serveur auto-signé peut être utilisé quand le paramètre de chiffrement est activé (champ **TrustServerCertificate** ).  
  
        4.  Cliquez sur **OK** pour fermer la boîte de dialogue **Paramètres avancés** .  
  
    6.  Cliquez sur **Suivant** pour passer à la page **Validation** .  
  
5.  Dans la page **Validation** , passez en revue les résultats de la validation des valeurs pour tous les paramètres. Dans l’exemple suivant, vous voyez un **avertissement** concernant l’existence d’un serveur lié, car celui-ci n’existe pas sur l’instance SQL Server sélectionnée. Si vous voyez **Erreur** pour **Résultat**, pointez sur **Erreur** pour voir les détails de l’erreur. Par exemple, si vous n’avez pas activé l’option Autoriser inprocess pour le fournisseur SSISOLEDB, vous obtenez une erreur pour l’action de configuration du serveur lié.  
  
     ![Assistant Publication du flux de données - Page Validation](../../integration-services/data-flow/media/dsd-feedpublishingwizard-validationpage.jpg "Assistant Publication du flux de données - Page Validation")  
  
6.  Pour enregistrer ce rapport en tant que fichier XML, cliquez sur Enregistrer le rapport.  
  
7.  Cliquez sur **Suivant** dans la page **Validation** pour passer à la page **Résumé** .  
  
8.  Vérifiez votre sélection dans la page **Résumé** , puis cliquez sur **Publier** pour démarrer le processus de publication. Celui-ci permet de créer le serveur lié, s’il n’existe pas déjà, puis de créer la vue à l’aide du serveur lié.  
  
     ![Assistant Publication du flux de données - Page Résumé](../../integration-services/data-flow/media/dsd-feedpublishingwizard-summarypage.jpg "Assistant Publication du flux de données - Page Résumé")  
  
     Vous pouvez désormais interroger les données de sortie du package en exécutant l’instruction SQL suivante sur la base de données TestDB : SELECT * FROM [SSISPackageView].  
  
9. Pour enregistrer ce rapport en tant que fichier XML, cliquez sur **Enregistrer le rapport**.  
  
10. Passez en revue les résultats du processus de publication, puis cliquez sur **Terminer** pour fermer l’Assistant.  
  
    > [!NOTE]  
    >  Les types de données suivants ne sont pas pris en charge : text, ntext, image, nvarchar(max), varchar(max) et varbinary(max).  
  
## <a name="step-3-test-the-sql-view"></a>Étape 3 : tester la vue SQL  
 Au cours de cette étape, vous allez exécuter la vue SQL créée par l’Assistant Publication de flux de données SSIS.  
  
1.  Lancez SQL Server Management Studio.  
  
2.  Développez \<**machine name**>, **Bases de données**, \<**database you selected in the wizard**> et **Vues**.  
  
3.  Cliquez avec le bouton droit sur la \<**view created by the wizard**> créée par l’Assistant, puis cliquez sur **Sélectionner les 1000 premières lignes**.  
  
4.  Vérifiez que vous voyez bien les résultats du package SSIS.  
  
## <a name="step-4-verify-the-ssis-package-execution"></a>Étape 4 : vérifier l’exécution du Package SSIS  
 Au cours de cette étape, vous allez vérifier que le package SSIS s’est bien exécuté.  
  
1.  Dans SQL Server Management Studio, développez **Catalogues Integration Services**, développez **SSISDB**, développez le **dossier** où se trouve votre projet SSIS, développez **Projets**, développez votre nœud de projet, puis développez **Packages**.  
  
2.  Cliquez avec le bouton droit sur le package SSIS, pointez sur **Rapports**, pointez sur **Rapports standard**, puis cliquez sur **Toutes les exécutions**.  
  
3.  Vous devez voir l’exécution du package SSIS dans le rapport.  
  
    > [!NOTE]  
    >  Sur un ordinateur doté de Windows Vista Service Pack 2, vous pouvez éventuellement voir deux exécutions de package SSIS dans le rapport, l’une réussie, l’autre non. Ignorez l’échec d’exécution, car il est provoqué par un problème connu dans cette version.  
  
## <a name="more-info"></a>En savoir plus  
 L’Assistant Publication de flux de données effectue les étapes importantes suivantes :  
  
1.  Il crée un serveur lié et le configure pour utiliser le fournisseur OLE DB pour SSIS.  
  
2.  Il crée une vue SQL dans la base de données spécifiée, qui interroge le serveur lié sur les informations de catalogue du package sélectionné.  
  
 Cette section contient les procédures nécessaires pour la création d’un serveur lié et d’une vue SQL sans l’aide de l’Assistant Publication de flux de données. Elle contient également des informations supplémentaires sur l’utilisation de la fonction OPENQUERY avec le fournisseur OLE DB pour SSIS.  
  
### <a name="create-a-linked-server-using-the-ole-db-provider-for-ssis"></a>Créer un serveur lié à l’aide du fournisseur OLE DB pour SSIS  
 Créez un serveur lié à l’aide du fournisseur OLE DB pour SSIS (SSISOLEDB) en exécutant la requête suivante dans SQL Server Management Studio.  
  
```sql 
  
USE [master]  
GO  
  
EXEC sp_addlinkedserver  
@server = N'SSISFeedServer',  
@srvproduct = N'Microsoft',  
@provider = N'SSISOLEDB',  
@datasrc = N'.'  
GO  
  
```  
  
### <a name="create-a-view-using-linked-server-and-ssis-catalog-information"></a>Créer une vue à l’aide du serveur lié et des informations du catalogue SSIS  
 Au cours de cette étape, vous allez créer une vue SQL qui exécute une requête sur le serveur lié que vous avez créé dans la section précédente. La requête inclut le nom du dossier, le nom du projet et le nom du package dans le catalogue SSIS.  
  
 Au moment de l’exécution, quand la vue est exécutée, la requête de serveur lié définie dans la vue démarre le package SSIS spécifié dans la requête, puis reçoit la sortie du package sous forme de jeu de résultats tabulaire.  
  
1.  Avant de créer la vue, tapez et exécutez la requête suivante dans la nouvelle fenêtre de requête. OPENQUERY est une fonction d’ensemble de lignes prise en charge par SQL Server. Elle exécute la requête directe indiquée sur le serveur lié spécifié à l’aide du fournisseur OLE DB associé au serveur lié. Il est possible de référencer OPENQUERY dans la clause FROM d’une requête SELECT comme s’il s’agissait du nom d’une table. Consultez la [documentation OPENQUERY sur MSDN Library](../../t-sql/functions/openquery-transact-sql.md) pour plus d’informations.  
  
    ```sql
    SELECT * FROM OPENQUERY(SSISFeedServer,N'Folder=Eldorado;Project=SSISPackagePublishing;Package=Package.dtsx')   
    GO  
    ```  
  
    > [!IMPORTANT]  
    >  Mettez à jour le nom du dossier, le nom du projet et le nom du package, le cas échéant. En cas d’échec de la fonction OPENQUERY, dans **SQL Server Management Studio**, développez **Objets serveur**, développez **Serveurs liés**, développez **Fournisseurs**, double-cliquez sur le fournisseur **SSISOLEDB** , puis assurez-vous que l’option **Autoriser inprocess** est activée.  
  
2.  Créez une vue dans la base de données **TestDB** pour les besoins de cette procédure pas à pas en exécutant la requête suivante.  
  
    ```sql
  
    USE [TestDB]   
    GO   
  
    CREATE VIEW SSISPackageView AS   
    SELECT * FROM OPENQUERY(SSISFeedServer, 'Folder=Eldorado;Project=SSISPackagePublishing;Package=Package.dtsx')   
    GO  
  
    ```  
  
3.  Testez la vue en exécutant la requête suivante.  
  
    ```sql
    SELECT * FROM SSISPackageView  
    ```  
  
### <a name="openquery-function"></a>Fonction OPENQUERY  
 Syntaxe de la fonction OPENQUERY :  
  
```sql 
SELECT * FROM OPENQUERY(<LinkedServer Name>, N'Folder=<Folder Name from SSIS Catalog>; Project=<SSIS Project Name>; Package=<SSIS Package Name>; Use32BitRuntime=[True | False];Parameters="<parameter_name_1>=<value1>; parameter_name_2=<value2>";Timeout=<Number of Seconds>;')  
```  
  
 Les paramètres Folder, Project et Package sont obligatoires. Use32BitRuntime, Timeout et Parameters sont facultatifs.  
  
 Use32BitRuntime peut avoir la valeur 0, 1, true ou false. Il indique si le package doit être exécuté avec le runtime 32 bits (1 ou true) quand la plateforme de SQL Server est de type 64 bits.  
  
 Timeout indique le délai d’attente en secondes du fournisseur OLE DB pour SSIS avant l’arrivée de nouvelles données en provenance du package SSIS. Par défaut, le délai d’expiration est de 60 secondes. Vous pouvez spécifier une valeur entière pour indiquer un délai d’attente compris entre 20 et 32 000.  
  
 Parameters contient la valeur des paramètres du package et des paramètres du projet. Les règles des paramètres sont les mêmes que celles des paramètres dans [DTExec](../packages/dtexec-utility.md).  
  
 La liste suivante indique les caractères spéciaux autorisés dans la clause de requête :  
  
-   Guillemet simple (') - Pris en charge par la fonction OPENQUERY standard. Si vous souhaitez utiliser le guillemet simple dans la clause de requête, utilisez deux guillemets simples ('').  
  
-   Guillemet (") - La partie relative aux paramètres de la requête est placée entre guillemets. Si une valeur de paramètre contient elle-même un guillemet, utilisez le caractère d’échappement. Par exemple : \".  
  
-   Crochets gauche et droit ([ et ]) - Ces caractères sont utilisés pour indiquer les espaces de début/de fin. Par exemple, « [ des espaces ] » représente la chaîne « des espaces » avec un espace de début et un espace de fin. Si ces caractères sont eux-mêmes utilisés dans la clause de requête, ils doivent faire l’objet d’une séquence d’échappement. Par exemple, \\[ et \\].  
  
-   Barre oblique (\\) - Chaque caractère \ utilisé dans la clause de la requête doit être précédé du caractère d’échappement. Par exemple, \\\ est évalué comme \ dans la clause de la requête.  
  
## <a name="see-also"></a>Voir aussi  
 [Data Streaming Destination](../../integration-services/data-flow/data-streaming-destination.md)   
 [Configurer Data Streaming Destination](./data-streaming-destination.md)  
  
