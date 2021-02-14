---
description: Opérations de service Web par catégorie (Master Data Services)
title: Opérations de service Web par catégorie
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: reference
ms.assetid: e3f346b5-7e26-481d-9821-1846e2e91289
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 65656aa902b5500d1dcc57dde914b3a0b2265008
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350129"
---
# <a name="categorized-web-service-operations-master-data-services"></a>Opérations de service Web par catégorie (Master Data Services)

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Le service Web [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] contient un jeu complet d'événements qui vous permettent d'écrire du code pour contrôler toutes les fonctionnalités que [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] traite via son interface utilisateur. Les opérations de service Web sont définies par le biais de l'interface <xref:Microsoft.MasterDataServices.IService> et sont implémentées en tant que méthodes de la classe <xref:Microsoft.MasterDataServices.ServiceClient>. Cette rubrique regroupe les opérations de service Web en catégories conceptuelles pour vous aider à comprendre comment utiliser l'API du service Web.  
  
## <a name="model-operations"></a>Opérations de modèle  
 Ces opérations sont utilisées pour créer, mettre à jour et supprimer de modèles, ainsi que pour traiter tout le contenu d'un modèle, comme des entités, des hiérarchies et des versions. Pour plus d’informations, consultez [Modèles &#40;Master Data Services&#41;](../../master-data-services/models-master-data-services.md).  
  
- <xref:Microsoft.MasterDataServices.ServiceClient.MetadataClone%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.MetadataCreate%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.MetadataDelete%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.MetadataGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.MetadataUpdate%2A>
  
## <a name="entity-operations"></a>Opérations d'entité  
 Ces opérations sont utilisées pour créer, mettre à jour, et supprimer les membres d'une entité unique. Pour plus d’informations, consultez [Entités &#40;Master Data Services&#41;](../../master-data-services/entities-master-data-services.md) et [Membres &#40;Master Data Services&#41;](../../master-data-services/members-master-data-services.md).  
  
- <xref:Microsoft.MasterDataServices.ServiceClient.EntityMemberKeyLookup%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.EntityMembersCopy%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.EntityMembersCreate%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.EntityMembersDelete%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.EntityMembersGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.EntityMembersMerge%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.EntityMembersUpdate%2A>
  
## <a name="member-operations"></a>Opérations de membres  
 Ces opérations sont utilisées pour obtenir, mettre à jour et supprimer des membres. Le jeu de membres objet de l'opération peut contenir des membres provenant de plusieurs entités. Pour plus d’informations, consultez [Membres &#40;Master Data Services&#41;](../../master-data-services/members-master-data-services.md).  
  
- <xref:Microsoft.MasterDataServices.ServiceClient.ModelMembersBulkDelete%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.ModelMembersBulkMerge%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.ModelMembersBulkUpdate%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.ModelMembersGet%2A>
  
## <a name="attribute-and-hierarchy-operations"></a>Opérations d'attributs et de hiérarchies  
 Ces opérations sont utilisées pour obtenir les informations relatives aux attributs et aux hiérarchies. Les attributs et les hiérarchies peuvent également être modifiés à l'aide des opérations de modèle, telles que <xref:Microsoft.MasterDataServices.ServiceClient.MetadataUpdate%2A>. Pour plus d’informations, consultez [Attributs &#40;Master Data Services&#41;](../../master-data-services/attributes-master-data-services.md) et [Hiérarchies &#40;Master Data Services&#41;](../../master-data-services/hierarchies-master-data-services.md).  
  
- <xref:Microsoft.MasterDataServices.ServiceClient.EntityMemberAttributesGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.HierarchyMembersGet%2A>
  
## <a name="business-rule-operations"></a>Opérations de règle d'entreprise  
 Ces opérations sont utilisées pour créer, mettre à jour, supprimer, puis publier des règles d'entreprise. Pour plus d’informations, consultez [Règles d’entreprise &#40;Master Data Services&#41;](../../master-data-services/business-rules-master-data-services.md).  
  
- <xref:Microsoft.MasterDataServices.ServiceClient.BusinessRulesClone%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.BusinessRulesCreate%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.BusinessRulesDelete%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.BusinessRulesGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.BusinessRulesPaletteGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.BusinessRulesPublish%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.BusinessRulesUpdate%2A>
  
## <a name="annotation-operations"></a>Opérations d'annotation  
 Ces opérations sont utilisées pour créer, mettre à jour et supprimer des annotations. Pour plus d’informations, consultez [Annotations &#40;Master Data Services&#41;](../../master-data-services/annotations-master-data-services.md).  
  
- <xref:Microsoft.MasterDataServices.ServiceClient.AnnotationsDelete%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.AnnotationsUpdate%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.EntityMemberAnnotationsCreate%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.EntityMemberAnnotationsGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.TransactionAnnotationsCreate%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.TransactionAnnotationsGet%2A>
  
## <a name="transaction-operations"></a>Opérations de transaction  
 Ces opérations sont utilisées pour obtenir et inverser des transactions. Pour plus d’informations, consultez [Administrateurs &#40;Master Data Services&#41;](../../master-data-services/transactions-master-data-services.md).  
  
- <xref:Microsoft.MasterDataServices.ServiceClient.TransactionsGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.TransactionsReverse%2A>
  
## <a name="version-and-validation-operations"></a>Opérations de version et de validation  
 Ces opérations sont utilisées pour copier et valider des versions. Pour plus d’informations, consultez [Versions &#40;Master Data Services&#41;](../../master-data-services/versions-master-data-services.md) et [Validation &#40;Master Data Services&#41;](../../master-data-services/validation-master-data-services.md).  
  
- <xref:Microsoft.MasterDataServices.ServiceClient.VersionCopy%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.ValidationGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.ValidationProcess%2A>
  
## <a name="data-quality-operations"></a>Opérations de qualité des données  
 Ces opérations sont utilisées pour effectuer des tâches de qualité des données et vérifier les résultats.  
  
- <xref:Microsoft.MasterDataServices.ServiceClient.DataQualityCleansingOperationCreate%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.DataQualityMatchingOperationCreate%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.DataQualityInstalledState%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.DataQualityKnowledgeBasesGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.DataQualityOperationStart%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.DataQualityOperationResultsGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.DataQualityOperationStatus%2A>
  
## <a name="data-import-operations"></a>Opérations d'importation de données  
 Ces opérations sont utilisées pour importer des données dans une base de données [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] . Pour plus d’informations, consultez [vue d’ensemble : importation de données à partir de Tables &#40;Master Data Services&#41;](../../master-data-services/overview-importing-data-from-tables-master-data-services.md).  
  
- <xref:Microsoft.MasterDataServices.ServiceClient.EntityStagingClear%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.EntityStagingGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.EntityStagingLoad%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.EntityStagingProcess%2A>
  
 Les opérations suivantes sont utilisées pour importer des données à l'aide du processus de site inclus dans [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]. Elles doivent être utilisées uniquement pour prendre en charge les bases de données existantes. Pour la développement, utilisez les opérations précédemment répertoriées.  
  
- <xref:Microsoft.MasterDataServices.ServiceClient.StagingClear%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.StagingGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.StagingNameCheck%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.StagingProcess%2A>
  
## <a name="data-export-operations"></a>Opérations d'exportations de données  
 Ces opérations sont utilisées pour exporter des données via les vues d'abonnement. Pour plus d’informations, consultez [Vue d’ensemble : exportation de données &#40;Master Data Services&#41;](../../master-data-services/overview-exporting-data-master-data-services.md).  
  
- <xref:Microsoft.MasterDataServices.ServiceClient.ExportViewCreate%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.ExportViewDelete%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.ExportViewListGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.ExportViewUpdate%2A>
  
## <a name="security-operations"></a>Opérations de sécurité  
 Ces opérations sont utilisées pour modifier les paramètres de sécurité qui contrôlent l'accès à la base de données [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] . Pour plus d’informations, consultez [Importer des données à partir de tables &#40;Master Data Services&#41;](../../master-data-services/security-master-data-services.md).  
  
- <xref:Microsoft.MasterDataServices.ServiceClient.SecurityPrincipalsClone%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.SecurityPrincipalsCreate%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.SecurityPrincipalsDelete%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.SecurityPrincipalsGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.SecurityPrincipalsUpdate%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.SecurityPrivilegesClone%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.SecurityPrivilegesCreate%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.SecurityPrivilegesDelete%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.SecurityPrivilegesGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.SecurityPrivilegesUpdate%2A>
  
## <a name="system-operations"></a>Opérations du système  
 Ces opérations sont utilisées pour obtenir et mettre à jour les paramètres système et les préférences de l'utilisateur.  
  
- <xref:Microsoft.MasterDataServices.ServiceClient.ServiceCheck%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.ServiceVersionGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.SystemDomainListGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.SystemPropertiesGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.SystemSettingsGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.SystemSettingsUpdate%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.UserPreferencesDelete%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.UserPreferencesGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.UserPreferencesUpdate%2A>
  
  
