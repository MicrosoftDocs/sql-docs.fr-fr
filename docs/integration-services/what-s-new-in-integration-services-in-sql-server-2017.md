---
description: Nouveautés d’Integration Services dans SQL Server 2017
title: Nouveauté d’Integration Services dans SQL Server 2017 | Microsoft Docs
ms.custom: ''
ms.date: 09/28/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: e26d7884-e772-46fa-bfdc-38567fe976a1
author: chugugrace
ms.author: chugu
ms.openlocfilehash: b955aa61a4e196fc4c419a9b34e186590006df42
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345675"
---
# <a name="what39s-new-in-integration-services-in-sql-server-2017"></a>Nouveautés d’Integration Services dans SQL Server 2017

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]


Cette rubrique décrit les fonctionnalités qui ont été ajoutées ou mises à jour dans [!INCLUDE[ssSQLv14_md](../includes/sssql17-md.md)] [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)].

> [!NOTE]
> SQL Server 2017 inclut également les fonctionnalités de SQL Server 2016 et les fonctionnalités ajoutées dans les mises à jour de SQL Server 2016. Pour plus d’informations sur les nouvelles fonctionnalités SSIS dans SQL Server 2016, consultez [Nouveautés d’Integration Services dans SQL Server 2016](../integration-services/what-s-new-in-integration-services-in-sql-server-2016.md).

## <a name="highlights-of-this-release"></a>Principales caractéristiques de cette version

Voici les nouvelles fonctionnalités les plus importantes d’Integration Services dans SQL Server 2017.

-   **Scale-out**. Distribuez plus facilement l’exécution de package SSIS sur plusieurs ordinateurs de travail, et gérez les exécutions et les threads de travail à partir d’un seul ordinateur maître. Pour plus d’informations, consultez [Integration Services Scale Out](../integration-services/scale-out/integration-services-ssis-scale-out.md).

-   **Integration Services sur Linux**. Exécutez des packages SSIS sur des ordinateurs Linux. Pour plus d’informations, consultez [Extraire, transformer et charger des données sur Linux avec SSIS](../linux/sql-server-linux-migrate-ssis.md).

-   **Améliorations de la connectivité**. Connectez-vous aux flux OData de Microsoft Dynamics AX Online et Microsoft Dynamics CRM Online avec les composants OData mis à jour. 

## <a name="new-in-azure-data-factory"></a>Nouveautés dans Azure Data Factory

Avec la préversion publique d’Azure Data Factory version 2 de septembre 2017, vous pouvez désormais effectuer les opérations suivantes :
-   Déployer des packages dans la base de données du catalogue SSIS (SSISDB) sur Azure SQL Database.
-   Exécuter des packages déployés sur Azure sur le runtime d’intégration Azure-SSIS, composant d’Azure Data Factory version 2.

Pour plus d’informations, consultez [Effectuer un « lift-and-shift » des charges de travail SQL Server Integration Services vers le cloud](lift-shift/ssis-azure-lift-shift-ssis-packages-overview.md).

Ces nouvelles fonctionnalités nécessitent SQL Server Data Tools (SSDT) version 17.2 ou ultérieure, mais ne nécessitent pas SQL Server 2017 ou SQL Server 2016. Quand vous déployez des packages sur Azure, l’Assistant Déploiement de package met toujours à niveau les packages avec le format de package le plus récent.

## <a name="new-in-the-azure-feature-pack"></a>Nouveautés dans le Feature Pack Azure

Outre les améliorations de connectivité dans SQL Server, le Feature Pack Integration Services pour Azure a ajouté la prise en charge d’Azure Data Lake Store. Pour plus d’informations, consultez le billet de blog intitulé [New Azure Feature Pack Release Strengthening ADLS Connectivity (Publication du nouveau Feature Pack Azure renforçant la connectivité ADLS)](https://blogs.msdn.microsoft.com/ssis/2017/08/29/new-azure-feature-pack-release-strengthening-adls-connectivity/). Voir également [Feature Pack Azure pour Integration Services (SSIS)](azure-feature-pack-for-integration-services-ssis.md).

## <a name="new-in-sql-server-data-tools-ssdt"></a>Nouveautés de SQL Server Data Tools (SSDT)

Vous pouvez maintenant développer des projets et des packages SSIS qui ciblent les versions de SQL Server de 2012 à 2017 dans Visual Studio 2017 ou Visual Studio 2015. Pour plus d’informations, consultez [Télécharger SSDT (SQL Server Data Tools)](../ssdt/download-sql-server-data-tools-ssdt.md).

## <a name="new-in-ssis-in-sql-server-2017-rc1"></a>Nouveautés de SSIS dans SQL Server 2017 RC1

### <a name="new-and-changed-features-in-scale-out-for-ssis"></a>Fonctionnalités nouvelles et modifiées dans Scale Out pour SSIS

-   Scale Out Master prend désormais en charge la haute disponibilité. Vous pouvez activer AlwaysOn pour SSISDB et configurer le clustering de basculement Windows Server pour le serveur qui héberge le service Scale Out Master. En appliquant cette modification à Scale Out Master, vous évitez un point de défaillance unique et bénéficiez d’une haute disponibilité pour l’ensemble du déploiement Scale Out.
-   La gestion de basculement des journaux d’exécution des Scale Out Workers est améliorée. Les journaux d’exécution sont conservés sur le disque local au cas où le Scale Out Worker s’arrêterait de manière inattendue. Plus tard, quand le worker redémarre, il recharge les journaux persistants et continue à les enregistrer dans SSISDB.
-   Le paramètre *runincluster* de la procédure stockée **[catalog].[create_execution]** est renommé en *runinscaleout* pour des raisons de cohérence et de lisibilité. Ce changement de nom de paramètre a l’impact suivant :
    -   Si vous avez des scripts existants pour exécuter des packages dans Scale Out, vous devez remplacer le nom du paramètre *runincluster* par *runinscaleout* pour que les scripts fonctionnent dans la version RC1.
    -   SQL Server Management Studio (SSMS) 17.1 et versions antérieures ne peuvent pas déclencher l’exécution du package dans Scale Out dans la version RC1. Le message d’erreur est : « *@runincluster* n’est pas un paramètre valide pour la procédure **create_execution** ». Ce problème est résolu dans la prochaine version de SSMS, la version 17.2. Les versions 17.2 et ultérieures de SSMS prennent en charge le nouveau nom de paramètre et l’exécution de package dans Scale Out. Jusqu’à ce que SSMS version 17.2 soit disponible, en guise de solution de contournement vous pouvez utiliser votre version existante de SSMS pour générer le script d’exécution du package, remplacer le nom du paramètre *runincluster* par *runinscaleout* dans le script, puis exécuter le script.
-   Le catalogue SSIS a une nouvelle propriété globale afin de spécifier le mode par défaut pour l’exécution de packages SSIS. Cette nouvelle propriété s’applique quand vous appelez la procédure stockée **[catalog].[create_execution]** avec le paramètre *runinscaleout* défini sur null. Ce mode s’applique également aux travaux de l’Agent SQL de SSIS. Vous pouvez définir la nouvelle propriété globale dans la boîte de dialogue Propriétés du nœud SSISDB dans SSMS, ou avec la commande suivante :
    ```sql
    EXEC [catalog].[configure_catalog] @property_name=N'DEFAULT_EXECUTION_MODE', @property_value=1
    ```

## <a name="new-in-ssis-in-sql-server-2017-ctp-21"></a>Nouveautés de SSIS dans SQL Server 2017 CTP 2.1

### <a name="new-and-changed-features-in-scale-out-for-ssis"></a>Fonctionnalités nouvelles et modifiées dans Scale Out pour SSIS

-   Vous pouvez maintenant utiliser le paramètre **Use32BitRuntime** quand vous déclenchez l’exécution dans Scale Out.
-   Les performances de journalisation dans SSISDB pour les exécutions de package dans Scale Out ont été améliorées. Les journaux Message d’événement et Contexte de message sont désormais écrits dans SSISDB en mode batch plutôt qu’un par un. Voici quelques remarques supplémentaires sur cette amélioration :        
    - Certains rapports dans la version actuelle de SQL Server Management Studio (SSMS) n’affichent pas ces journaux pour les exécutions dans Scale Out. Nous estimons qu’ils seront pris en charge dans la prochaine version de SSMS. Les rapports affectés incluent le rapport *Toutes les connexions*, le rapport *Contexte de l’erreur* et la section *Informations de connexion* dans le tableau de bord Integration Services.
    - Une nouvelle colonne **event_message_guid** a été ajoutée. Utilisez cette colonne pour joindre les vues [catalog].[event_message_context] et [catalog].[event_messages] au lieu d’utiliser **event_message_id** quand vous interrogez ces journaux d’exécutions dans Scale Out.
-   Pour obtenir l’application de gestion pour SSIS Scale Out, [téléchargez SQL Server Management Studio (SSMS)](../ssms/download-sql-server-management-studio-ssms.md) 17.1 ou version ultérieure.

## <a name="new-in-ssis-in-sql-server-2017-ctp-20"></a>Nouveautés de SSIS dans SQL Server 2017 CTP 2.0

Il n’y a pas de nouvelles fonctionnalités SSIS dans SQL Server 2017 CTP 2.0.

## <a name="new-in-ssis-in-sql-server-2017-ctp-14"></a>Nouveautés de SSIS dans SQL Server 2017 CTP 1.4

Il n’y a pas de nouvelles fonctionnalités SSIS dans SQL Server 2017 CTP 1.4.

## <a name="new-in-ssis-in-sql-server-2017-ctp-13"></a>Nouveautés de SSIS dans SQL Server 2017 CTP 1.3

Il n’y a pas de nouvelles fonctionnalités SSIS dans SQL Server 2017 CTP 1.3.

## <a name="new-in-ssis-in-sql-server-2017-ctp-12"></a>Nouveautés de SSIS dans SQL Server 2017 CTP 1.2

Il n’y a pas de nouvelles fonctionnalités SSIS dans SQL Server 2017 CTP 1.2.

## <a name="new-in-ssis-in-sql-server-2017-ctp-11"></a>Nouveautés de SSIS dans SQL Server 2017 CTP 1.1

Il n’y a pas de nouvelles fonctionnalités SSIS dans SQL Server 2017 CTP 1.1.

## <a name="new-in-ssis-in-sql-server-2017-ctp-10"></a>Nouveautés de SSIS dans SQL Server 2017 CTP 1.0

### <a name="scale-out-for-ssis"></a>Scale Out pour SSIS

La fonctionnalité Scale Out facilite grandement l’exécution de [!INCLUDE[ssIS_md](../includes/ssis-md.md)] sur plusieurs ordinateurs. 
   
Après l’installation du Scale Out Master et des Scale Out Workers, le package peut être distribué pour s’exécuter automatiquement sur différents Workers. Si l’exécution est arrêtée inopinément, une nouvelle tentative est effectuée automatiquement. De plus, vous pouvez gérer toutes les exécutions et Workers de manière centralisée à l’aide du Master.
   
Pour plus d’informations, consultez [Integration Services Scale Out](../integration-services/scale-out/integration-services-ssis-scale-out.md).
   
### <a name="support-for-microsoft-dynamics-online-resources"></a>Prise en charge des ressources Microsoft Dynamics Online

Le Gestionnaire de connexions OData et de sources OData prennent désormais en charge la connexion aux flux OData de Microsoft Dynamics AX Online et Microsoft Dynamics CRM Online.