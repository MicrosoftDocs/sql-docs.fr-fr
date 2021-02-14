---
title: Importer des données à partir de tables
description: Importez des données à partir de tables et apportez des modifications de données à un modèle en bloc. Utilisez cette procédure pour ajouter, mettre à jour et supprimer des données dans la base de données Master Data Services.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: ad5b83b1-8e40-4ef8-9ba8-4ea17a58b672
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: e9faf7de8d3e7517d47c61ba3a9b6e00cd2e2b09
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347731"
---
# <a name="import-data-from-tables-master-data-services"></a>Importer des données à partir de tables (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Vous pouvez ajouter des données et apporter des modifications de données à un modèle en bloc dans [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)].  
  
 **Conditions préalables**  
  
-   Vous devez avoir l’autorisation d’insérer des données dans le STG. \<name> _Leaf, STG. \<name> _Consolidated, STG. \<name> _Relationship table dans la [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] base de données.  
  
-   Vous devez disposer des autorisations nécessaires pour exécuter l’stg.udp_ \<name> _Leaf, STG. udp \_ \<name> _Consolidated ou la \_ \<name> procédure stockée _Relationship STG. UDP dans la [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] base de données.  
  
-   L'état du modèle ne doit pas être **Activé**.  
  
 **Pour ajouter, mettre à jour et supprimer des données dans la [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] base de données**  
  
1.  Préparez les membres à importer dans la table de mise en lots appropriée de la base de données [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] , en fournissant aussi les valeurs pour les champs obligatoires. Pour plus d’informations sur les tables de mise en lots, consultez [Vue d’ensemble : importation de données à partir de tables &#40;Master Data Services&#41;](../master-data-services/overview-importing-data-from-tables-master-data-services.md).  
  
    -   Pour les membres feuille, la table est STG. \<name> _Leaf, où \<name> fait référence à l’entité correspondante. Pour plus d’informations sur les champs requis, consultez [Table de mise en lots des membres feuille &#40;Master Data Services&#41;](../master-data-services/leaf-member-staging-table-master-data-services.md).  
  
    -   Pour les membres consolidés, la table est STG. \<name> _Consolidated. Pour plus d’informations sur les champs requis, consultez [Table de mise en lots des membres consolidés &#40;Master Data Services&#41;](../master-data-services/consolidated-member-staging-table-master-data-services.md).  
  
    -   Pour déplacer l’emplacement des membres dans les hiérarchies explicites, la table est STG. \<name> _Relationship. Pour plus d’informations sur les champs requis, consultez [Table de mise en lots des relations &#40;Master Data Services&#41;](../master-data-services/relationship-staging-table-master-data-services.md).  
  
         Pour plus d’informations sur le déplacement de membres dans les hiérarchies explicites, consultez [Vue d’ensemble : importation de données à partir de tables &#40;Master Data Services&#41;](../master-data-services/overview-importing-data-from-tables-master-data-services.md).  
  
    -   Utilisez la valeur du champ **ImportType** pour spécifier que vous créez des membres, que vous désactivez des membres ou que vous supprimez des membres. Pour plus d’informations sur les valeurs, consultez [Table de mise en lots des membres feuille &#40;Master Data Services&#41;](../master-data-services/leaf-member-staging-table-master-data-services.md) et [Table de mise en lots des membres consolidés &#40;Master Data Services&#41;](../master-data-services/consolidated-member-staging-table-master-data-services.md).  
  
         Pour plus d’informations sur la désactivation et la suppression de membres, consultez [Vue d’ensemble : importation de données à partir de tables &#40;Master Data Services&#41;](../master-data-services/overview-importing-data-from-tables-master-data-services.md).  
  
2.  Ouvrez [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] et connectez-vous à l'instance du moteur de base de données pour votre base de données [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] .  
  
     Pour plus d’informations, consultez [SQL Server Management Studio](../ssms/sql-server-management-studio-ssms.md).  
  
3.  Importez les données dans les tables de mise en lots à l'aide de l'Assistant Importation et exportation de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] .  
  
     Pour plus d'informations, consultez [SQL Server Import and Export Wizard](~/integration-services/import-export-data/welcome-to-sql-server-import-and-export-wizard.md).  
  
4.  Chargez les données depuis les tables de mise en lots dans les tables [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] , en effectuant une des opérations suivantes :  
  
    -   Exécutez la procédure stockée de mise en lots qui correspond à la table de mise en lots vers laquelle vous voulez déplacer des données.  
  
         Pour plus d’informations sur les procédures stockées de mise en lots et les tables de mise en lots, consultez [Vue d’ensemble : importation de données à partir de tables &#40;Master Data Services&#41;](../master-data-services/overview-importing-data-from-tables-master-data-services.md). Pour plus d’informations sur les paramètres des procédures stockées de mise en lots et un exemple de code, consultez [Procédure stockée de mise en lots &#40;Master Data Services&#41;](../master-data-services/staging-stored-procedure-master-data-services.md).  
  
    -   Utilisez la zone de la fonction **Gestion de l'intégration** de la gestion des données de référence.  
  
         Dans la page **Lots intermédiaires** , sélectionnez le modèle auquel vous ajoutez des données dans la liste déroulante, puis cliquez sur **Démarrer les lots**. L'état du traitement par lots est indiqué dans le champ **État** . Pour plus d’informations sur les états, consultez [États d’importation &#40;Master Data Services&#41;](../master-data-services/import-statuses-master-data-services.md).  
  
         ![Page Lots intermédiaires dans le Gestionnaire des données de référence](../master-data-services/media/mds-stagingbatchespage.png "Page Lots intermédiaires dans le Gestionnaire des données de référence")  
  
         Le processus de site est démarré à des intervalles déterminés par le paramètre **Intervalle de lot intermédiaire** dans [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)]. Pour plus d’informations, consultez [Paramètres système &#40;Master Data Services&#41;](../master-data-services/system-settings-master-data-services.md).  
  
5.  Afficher les erreurs rencontrées lors de la mise en lots. Pour plus d’informations, consultez [Afficher les erreurs rencontrées lors de la mise en lots &#40;Master Data Services&#41;](../master-data-services/view-errors-that-occur-during-staging-master-data-services.md) et [Erreurs du processus de mise en lots &#40;Master Data Services&#41;](../master-data-services/staging-process-errors-master-data-services.md).  
  
6.  Validation de données par rapport aux règles d'entreprise  
  
     Dans Master Data Manager, accédez à la zone de la fonction **Explorer** pour votre modèle, puis appliquez des règles d'entreprise pour valider les données. Pour plus d’informations, consultez [Valider des membres spécifiques par rapport aux règles d’entreprise &#40;Master Data Services&#41;](../master-data-services/validate-specific-members-against-business-rules-master-data-services.md). Vous pouvez également utiliser une procédure stockée pour valider les données. Pour plus d’informations, consultez [Procédure stockée de validation &#40;Master Data Services&#41;](../master-data-services/validation-stored-procedure-master-data-services.md).  
  
     Quand vous chargez des données à partir des tables de mise en lots, elles ne sont pas validées automatiquement par rapport aux règles d'entreprise. Pour plus d’informations sur la validation et quand elle a lieu, consultez [Validation &#40;Master Data Services&#41;](../master-data-services/validation-master-data-services.md).  
  
