---
description: Catalogue SSIS
title: Catalogue SSIS | Microsoft Docs
ms.custom: ''
ms.date: 09/17/2020
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.ssis.ssms.iscreatecatalog.f1
- sql13.ssis.ssms.iscatalogprop.general.f1
- sql13.ssis.dbupgradewizard.f1
ms.assetid: 24bd987e-164a-48fd-b4f2-cbe16a3cd95e
author: chugugrace
ms.author: chugu
ms.openlocfilehash: a8ed14b14198bb95db838e76ca07bffe5875ce69
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97642586"
---
# <a name="ssis-catalog"></a>Catalogue SSIS

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Le catalogue **SSISDB** est l’élément central pour l’utilisation des projets [!INCLUDE[ssISnoversion_md](../../includes/ssisnoversion-md.md)] (SSIS) que vous avez déployés sur le serveur [!INCLUDE[ssISnoversion_md](../../includes/ssisnoversion-md.md)]. Ainsi, c'est dans ce catalogue que vous définissez les paramètres de projet et de package, configurez les environnements pour spécifier des valeurs d'exécution pour les packages, exécutez et résolvez les problèmes relatifs aux packages, et gérez les opérations du serveur [!INCLUDE[ssISnoversion_md](../../includes/ssisnoversion-md.md)] .  
 
> [!NOTE]
> Cet article décrit le catalogue SSIS de manière générale et le catalogue SSIS en cours d’exécution au niveau local. Vous pouvez également créer le catalogue SSIS dans Azure SQL Database, et déployer et exécuter des packages SSIS dans Azure. Pour plus d’informations, consultez [Effectuer un « lift-and-shift » des charges de travail SQL Server Integration Services vers le cloud](../lift-shift/ssis-azure-lift-shift-ssis-packages-overview.md).
>
> Bien que vous puissiez également exécuter des packages SSIS sur Linux, le catalogue SSIS n’est pas pris en charge sur Linux. Pour plus d’informations, consultez [Extraire, transformer et charger des données sur Linux avec SSIS](../../linux/sql-server-linux-migrate-ssis.md).
 
 Les objets stockés dans le catalogue **SSISDB** sont les projets, les packages, les paramètres, les environnements et l'historique opérationnel.  
  
 Vous inspectez les objets, les paramètres et les données opérationnelles stockés dans le catalogue **SSISDB** en interrogeant les vues de la base de données **SSISDB** . Vous gérez des objets en appelant des procédures stockées situées dans la base de données **SSISDB** ou à l'aide de l'interface utilisateur du catalogue **SSISDB** . Dans de nombreux cas, la même tâche peut être effectuée dans l'interface utilisateur ou en appelant une procédure stockée.  
  
 Pour maintenir la base de données **SSISDB** , il est recommandé d'appliquer des stratégies d'entreprise standard pour la gestion des bases de données utilisateur. Pour plus d'informations sur la création de plans de maintenance, consultez [Maintenance Plans](../../relational-databases/maintenance-plans/maintenance-plans.md).  
  
 Le catalogue **SSISDB** et la base de données **SSISDB** prennent en charge Windows PowerShell. Pour plus d'informations sur l'utilisation de SQL Server avec Windows PowerShell, consultez [SQL Server PowerShell](../../powershell/sql-server-powershell.md). Pour des exemples d'utilisation de Windows PowerShell pour exécuter des tâches telles que le déploiement d'un projet, consultez l'entrée de blog [SSIS et PowerShell dans SQL Server 2012](https://techcommunity.microsoft.com/t5/sql-server-integration-services/ssis-and-powershell-in-sql-server-2012/ba-p/388015), sur blogs.msdn.com.  
  
 Pour plus d’informations sur l’affichage des données opérationnelles, consultez [Surveiller les packages en cours d’exécution et autres opérations](../../integration-services/performance/monitor-running-packages-and-other-operations.md).  
  
 Vous accédez au catalogue **SSISDB** de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] en vous connectant au moteur de base de données de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , puis en développant le nœud **Catalogues Integration Services** dans l'Explorateur d'objets. Vous accédez à la base de données **SSISDB** de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] en développant le nœud Bases de données dans l'Explorateur d'objets.  
  
> [!NOTE]
> Vous ne pouvez pas renommer la base de données **SSISDB** .  
  
> [!NOTE]
> Si l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à laque la base de données **SSISDB** est rattachée s'arrête ou ne répond pas, le processus ISServerExec.exe prend fin. Un message est écrit dans un journal des événements Windows.  
>   
>  Si les ressources [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] basculent dans le cadre d’un basculement de cluster, les packages en cours d’exécution ne redémarrent pas. Vous pouvez utiliser les points de contrôle pour redémarrer les packages. Pour plus d'informations, consultez [Redémarrer des packages à l'aide de points de contrôle](../../integration-services/packages/restart-packages-by-using-checkpoints.md).  
  
## <a name="features-and-capabilities"></a>Fonctionnalités et capacités  
  
-   [Identificateurs d’objets de catalogue](../../integration-services/catalog/ssis-catalog.md#CatalogObjectIdentifiers)  
  
-   [Configuration du catalogue](../../integration-services/catalog/ssis-catalog.md#Configuration)  
  
-   [autorisations](../../integration-services/catalog/ssis-catalog.md#Permissions)  
  
-   [Dossiers](../../integration-services/catalog/ssis-catalog.md#Folders)  
  
-   [Projets et packages](../../integration-services/catalog/ssis-catalog.md#ProjectsAndPackages)  
  
-   [Paramètres](../../integration-services/catalog/ssis-catalog.md#Parameters)  
  
-   [Environnements serveur, variables de serveur et références d’environnement serveur](../../integration-services/catalog/ssis-catalog.md#ServerEnvironments)  
  
-   [Exécutions et validations](../../integration-services/catalog/ssis-catalog.md#Executions)  

##  <a name="catalog-object-identifiers"></a><a name="CatalogObjectIdentifiers"></a> Identificateurs d’objets de catalogue  
 Lorsque vous créez un objet dans le catalogue, attribuez-lui un nom. Ce nom constitue l'identificateur de l'objet. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] définit des règles quant aux caractères pouvant être utilisés dans un identificateur. Les noms des objets suivants doivent respecter les règles liées aux identificateurs.  
  
-   Dossier  
  
-   Projet  
  
-   Environnement  
  
-   Paramètre  
  
-   Variable d’environnement  
  
###  <a name="folder-project-environment"></a><a name="Folder"></a> Dossier, projet, environnement  
 Lorsque vous renommez un dossier, un projet ou un environnement, respectez les règles suivantes.  
  
-   Les caractères non valides sont les caractères ASCII/Unicode allant de 1 à 31, les guillemets ("), le signe inférieur à (\<), greater than (>), la barre verticale (|), le retour arrière (\b), Null (\0) et la tabulation (\t).  
  
-   Le nom ne peut pas contenir d'espaces de début ni de fin.  
  
-   \@ ne doit pas être utilisé comme premier caractère, mais il peut l’être par la suite \@.  
  
-   La longueur du nom doit être supérieure à 0 et inférieure ou égale à 128.  
  
###  <a name="parameter"></a><a name="Parameter"></a> Parameter  
 Lorsque vous affectez un nom à un paramètre, respectez les règles suivantes :  
  
-   Le premier caractère du nom doit être une lettre, ainsi que défini dans la norme Unicode 2.0, ou un trait de soulignement (_).  
  
-   Les caractères suivants peuvent être des lettres ou des nombres conformément à la norme Unicode 2.0, ou un trait de soulignement (_).  
  
###  <a name="environment-variable"></a><a name="EnvironmentVariable"></a> Variable d'environnement  
 Lorsque vous attribuez un nom à une variable d'environnement, respectez les règles suivantes :  
  
-   Les caractères non valides sont les caractères ASCII/Unicode allant de 1 à 31, les guillemets ("), le signe inférieur à (\<), greater than (>), la barre verticale (|), le retour arrière (\b), Null (\0) et la tabulation (\t).  
  
-   Le nom ne peut pas contenir d'espaces de début ni de fin.  
  
-   \@ ne doit pas être utilisé comme premier caractère, mais il peut l’être par la suite \@.  
  
-   La longueur du nom doit être supérieure à 0 et inférieure ou égale à 128.  
  
-   Le premier caractère du nom doit être une lettre, ainsi que défini dans la norme Unicode 2.0, ou un trait de soulignement (_).  
  
-   Les caractères suivants peuvent être des lettres ou des nombres conformément à la norme Unicode 2.0, ou un trait de soulignement (_).  
  
##  <a name="catalog-configuration"></a><a name="Configuration"></a> Configuration du catalogue  
 Pour définir précisément le comportement du catalogue, vous devez ajuster ses propriétés. Les propriétés du catalogue définissent la façon dont les données sensibles sont chiffrées, ainsi que la façon dont les opérations et les données du contrôle de version des projets sont conservées. Pour définir les propriétés du catalogue, utilisez la boîte de dialogue **Propriétés du catalogue** ou appelez la procédure stockée [catalog.configure_catalog &#40;base de données SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-configure-catalog-ssisdb-database.md). Pour afficher les propriétés, utilisez la boîte de dialogue ou interrogez [catalog.configure_catalog &#40;base de données SSISDB&#41;](../../integration-services/system-views/catalog-catalog-properties-ssisdb-database.md). Vous pouvez accéder à cette boîte de dialogue en cliquant avec le bouton droit sur **SSISDB** dans l’Explorateur d’objets.  
  
###  <a name="operations-and-project-version-cleanup"></a><a name="Cleanup"></a> Nettoyage des opérations et des versions de projet  
 Les données d'état de nombreuses opérations du catalogue sont stockées dans des tables de base de données internes. Ainsi, le catalogue effectue le suivi de l'état des exécutions de packages et des déploiements de projets. Pour limiter la taille des données opérationnelles, le **travail de maintenance de serveur SSIS** dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] est utilisé pour supprimer les anciennes données. Ce travail de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est créé lors de l'installation de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
 Vous pouvez mettre à jour ou redéployer un projet [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] en le déployant avec le même nom dans le même dossier du catalogue. Par défaut, chaque fois que vous redéployez un projet, le catalogue **SSISDB** en conserve la version précédente. Pour limiter la taille des données opérationnelles, le **travail de maintenance de serveur SSIS** est utilisé pour supprimer les anciennes versions des projets.  
 
Pour exécuter le **travail de maintenance du serveur SSIS**, SSIS crée la connexion SQL Server **##MS_SSISServerCleanupJobLogin##** . Cette connexion est réservée à un usage interne par SSIS.
  
 Les propriétés de catalogue **SSISDB** suivantes définissent le comportement de ce travail de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Vous pouvez afficher et modifier les propriétés à l’aide de la boîte de dialogue **Propriétés du catalogue** ou à l’aide de [catalog.catalog_properties &#40;base de données SSISDB&#41;](../../integration-services/system-views/catalog-catalog-properties-ssisdb-database.md) et [catalog.configure_catalog &#40;base de données SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-configure-catalog-ssisdb-database.md).  
  
 **Nettoyer les journaux régulièrement**  
 L’étape de travail de nettoyage des opérations s’exécute quand cette propriété a la valeur **True**.  
  
 **Période de rétention (jours)**  
 Définit l'âge maximal des données opérationnelles autorisées (en jours). Les données plus anciennes sont supprimées.  
  
 La valeur minimale est de un jour. La valeur maximale est limitée uniquement par la valeur maximale des données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **int**. Pour plus d’informations sur ce type de données, consultez [int, bigint, smallint et tinyint &#40;Transact-SQL&#41;](../../t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql.md).  
  
 **Supprimer régulièrement les anciennes versions**  
 L’étape de travail de nettoyage des versions de projet s’exécute quand cette propriété a la valeur **True**.  
  
 **Nombre maximal de versions par projet**  
 Définit le nombre de versions d’un projet stockées dans le catalogue. Les versions antérieures des projets sont supprimées.  
  
###  <a name="encryption-algorithm"></a><a name="Encryption"></a> Algorithme de chiffrement  
 La propriété **Algorithme de chiffrement** spécifie le type de chiffrement utilisé pour chiffrer les valeurs des paramètres sensibles. Vous pouvez faire votre choix parmi les types de chiffrement suivants.  
  
-   AES_256 (par défaut)  
  
-   AES_192  
  
-   AES_128  
  
-   DESX  
  
-   TRIPLE_DES_3KEY  
  
-   TRIPLE_DES  
  
-   DES  
  
 Lorsque vous déployez un projet [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] sur le serveur [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , le catalogue chiffre automatiquement les données du package et les valeurs sensibles. Le catalogue déchiffre automatiquement les données lorsque vous les récupérez. Le catalogue SSISDB utilise le niveau de protection **ServerStorage** . Pour plus d'informations, consultez [Access Control for Sensitive Data in Packages](../../integration-services/security/access-control-for-sensitive-data-in-packages.md).  
  
 La modification de l'algorithme de chiffrement est une opération qui prend du temps. Tout d'abord, le serveur doit utiliser l'algorithme précédemment spécifié pour déchiffrer toutes les valeurs de configuration. Le serveur doit ensuite utiliser le nouvel algorithme pour ré-chiffrer les valeurs. Pendant ce temps, aucune autre opération [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] ne peut être effectuée sur le serveur. Ainsi, pour permettre aux opérations [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] de continuer de façon ininterrompue, l’algorithme de chiffrement est une valeur en lecture seule dans la boîte de dialogue dans [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)].  
  
 Pour modifier le paramètre de la propriété **Algorithme de chiffrement** , définissez la base de données **SSISDB** en mode mono-utilisateur, puis appelez la procédure stockée catalog.configure_catalog. Utilisez ENCRYPTION_ALGORITHM pour l’argument *property_name* . Pour connaître les valeurs de propriétés prises en charge, consultez [catalog.catalog_properties &#40;base de données SSISDB&#41;](../../integration-services/system-views/catalog-catalog-properties-ssisdb-database.md). Pour plus d’informations sur la procédure stockée, consultez [catalog.configure_catalog &#40;base de données SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-configure-catalog-ssisdb-database.md).  
  
 Pour plus d’informations sur le mode mono-utilisateur, consultez [Définir une base de données en mode mono-utilisateur](../../relational-databases/databases/set-a-database-to-single-user-mode.md). Pour plus d’informations sur le chiffrement et les algorithmes de chiffrement dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez les rubriques de la section [Chiffrement SQL Server](../../relational-databases/security/encryption/sql-server-encryption.md).  
  
 Une clé principale de base de données est utilisée pour le chiffrement. La clé est générée lorsque vous créez le catalogue.  
  
 Le tableau suivant répertorie les noms des propriétés apparaissant dans la boîte de dialogue **Propriétés du catalogue** et les propriétés correspondantes en vue de base de données.  
  
|Nom de la propriété (boîte de dialogue **Propriétés du catalogue** )|Nom de la propriété (vue de base de données)|  
|---------------------------------------------------------|-------------------------------------|  
|Nom de l'algorithme de chiffrement|ENCRYPTION_ALGORITHM|  
|Nettoyer les journaux régulièrement|OPERATION_CLEANUP_ENABLEDâ€‹|  
|Période de rétention (jours)|RETENTION_WINDOW|  
|Supprimer régulièrement les anciennes versions|VERSION_CLEANUP_ENABLED|  
|Nombre maximal de versions par projet|MAX_PROJECT_VERSIONS|  
|Niveau d'enregistrement par défaut au niveau du serveur|SERVER_LOGGING_LEVEL|  
  
##  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Les projets, les environnements et les packages sont contenus dans des dossiers qui sont des objets sécurisables. Vous pouvez accorder des autorisations à un dossier, notamment l'autorisation de MANAGE_OBJECT_PERMISSIONS. MANAGE_OBJECT_PERMISSIONS vous permet de déléguer l'administration du contenu du dossier à un utilisateur sans avoir à accorder à celui-ci l'appartenance au rôle ssis_admin. Vous pouvez également accorder des autorisations relatives à des projets, des environnements et des opérations. Les opérations incluent l'initialisation de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], le déploiement de projets, la création et le démarrage d'exécutions, la validation de projets et de packages, ainsi que la configuration du catalogue **SSISDB** .  
  
 Pour plus d’informations sur les rôles de base de données, consultez [Rôles au niveau de la base de données](../../relational-databases/security/authentication-access/database-level-roles.md).  
  
 Le catalogue SSISDB utilise un déclencheur DDL, ddl_cleanup_object_permissions, pour appliquer l'intégrité des informations d'autorisations sur les éléments sécurisables SSIS. Le déclencheur est activé lorsqu'un principal de base de données, tel qu'un utilisateur de base de données, un rôle de base de données ou un rôle d'application de base de données, est supprimé de la base de données SSISDB.  
  
 Si le principal a accordé ou refusé des autorisations à d'autres principaux, révoquez les autorisations données par le fournisseur d'autorisations, avant que le principal puisse être supprimé. Sinon, un message d'erreur est retourné lorsque le système essaie de supprimer le principal. Le déclencheur supprime tous les enregistrements d'autorisation dans lesquels le principal de la base de données est un bénéficiaire.  
  
 Il est recommandé de ne pas désactiver le déclencheur car il garantit qu'il n'existe aucun enregistrement d'autorisation orphelin après la suppression d'un principal de la base de données **SSISDB** .  
  
### <a name="managing-permissions"></a>Gestion des autorisations  
 Vous pouvez gérer les autorisations à l’aide de l’interface utilisateur de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] , des procédures stockées et de l’espace de noms <xref:Microsoft.SqlServer.Management.IntegrationServices> .  
  
 Pour gérer les autorisations à l’aide de l’IU de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], servez-vous des boîtes de dialogue suivantes : 
  
-   Dans le cas d'un dossier, utilisez la page **Autorisations** de la [Folder Properties Dialog Box](../../integration-services/catalog/folder-properties-dialog-box.md).  
  
-   Dans le cas d'un projet, utilisez la page **Autorisations** de la [Project Properties Dialog Box](../../integration-services/catalog/project-properties-dialog-box.md).  

 Pour gérer les autorisations à l’aide de Transact-SQL, appelez [catalog.grant_permission &#40;base de données SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-grant-permission-ssisdb-database.md), [catalog.deny_permission &#40;base de données SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-deny-permission-ssisdb-database.md) et [catalog.revoke_permission &#40;base de données SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-revoke-permission-ssisdb-database.md). Pour afficher les autorisations effectives pour le principal actuel pour tous les objets, interrogez [catalog.effective_object_permissions &#40;base de données SSISDB&#41;](../../integration-services/system-views/catalog-effective-object-permissions-ssisdb-database.md). Cette rubrique fournit les descriptions des différents types d'autorisations. Pour afficher les autorisations affectées explicitement à l’utilisateur, interrogez [catalog.explicit_object_permissions &#40;base de données SSISDB&#41;](../../integration-services/system-views/catalog-explicit-object-permissions-ssisdb-database.md).  
  
##  <a name="folders"></a><a name="Folders"></a> Dossiers  
 Un dossier contient un ou plusieurs projets et environnements du catalogue **SSISDB** . Vous pouvez utiliser la vue [catalog.folders &#40;base de données SSISDB&#41;](../../integration-services/system-views/catalog-folders-ssisdb-database.md) pour accéder aux informations relatives aux dossiers du catalogue. Vous pouvez utiliser les procédures stockées suivantes pour gérer les dossiers :  
  
-   [catalog.create_folder &#40;Base de données SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-create-folder-ssisdb-database.md)  
  
-   [catalog.delete_folder &#40;Base de données SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-delete-folder-ssisdb-database.md)  
  
-   [catalog.rename_folder &#40;Base de données SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-rename-folder-ssisdb-database.md)  
  
-   [catalog.set_folder_description &#40;Base de données SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-set-folder-description-ssisdb-database.md)  
  
##  <a name="projects-and-packages"></a><a name="ProjectsAndPackages"></a> Projets et packages  
 Chaque projet peut contenir plusieurs packages. Les projets et les packages peuvent contenir des paramètres et des références aux environnements. Vous pouvez accéder aux paramètres et aux références d'environnement à l'aide de la [Configure Dialog Box](../../integration-services/catalog/configure-dialog-box.md).  
  
 Vous pouvez effectuer d’autres tâches de projet en appelant les procédures stockées suivantes : 
  
-   [catalog.delete_project &#40;Base de données SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-delete-project-ssisdb-database.md)  
  
-   [catalog.deploy_project &#40;Base de données SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-deploy-project-ssisdb-database.md)  
  
-   [catalog.get_project &#40;Base de données SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-get-project-ssisdb-database.md)  
  
-   [catalog.move_project &#40;&#40;Base de données SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-move-project-ssisdb-database.md)  
  
-   [catalog.restore_project &#40;Base de données SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-restore-project-ssisdb-database.md)  
  
 Ces vues fournissent des détails sur les packages, les projets et les versions des projets.  
  
-   [catalog.projects &#40;Base de données SSISDB&#41;](../../integration-services/system-views/catalog-projects-ssisdb-database.md)  
  
-   [catalog.packages &#40;Base de données SSISDB&#41;](../../integration-services/system-views/catalog-packages-ssisdb-database.md)  
  
-   [catalog.object_versions &#40;Base de données SSISDB&#41;](../../integration-services/system-views/catalog-object-versions-ssisdb-database.md)  
  
##  <a name="parameters"></a><a name="Parameters"></a> Paramètres  
 Vous utilisez des paramètres pour affecter des valeurs aux propriétés des packages au moment de l'exécution de ces packages. Pour définir la valeur d’un package ou d’un paramètres de projet et pour effacer la valeur, appelez [catalog.set_object_parameter_value &#40;base de données SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-set-object-parameter-value-ssisdb-database.md) et [catalog.clear_object_parameter_value &#40;base de données SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-clear-object-parameter-value-ssisdb-database.md). Pour définir la valeur d’un paramètre pour une instance d’exécution, appelez [catalog.set_execution_parameter_value &#40;base de données SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-set-execution-parameter-value-ssisdb-database.md). Vous pouvez récupérer les valeurs des paramètres par défaut en appelant [catalog.get_parameter_values &#40;base de données SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-get-parameter-values-ssisdb-database.md).  
  
 Ces vues affichent les paramètres de tous les packages et projets, ainsi que les valeurs de paramètre utilisées pour une instance d'exécution.  
  
-   [catalog.object_parameters &#40;Base de données SSISDB&#41;](../../integration-services/system-views/catalog-object-parameters-ssisdb-database.md)  
  
-   [catalog.execution_parameter_values &#40;Base de données SSISDB&#41;](../../integration-services/system-views/catalog-execution-parameter-values-ssisdb-database.md)  
  
##  <a name="server-environments-server-variables-and-server-environment-references"></a><a name="ServerEnvironments"></a> Environnements serveur, variables de serveur et références d’environnement serveur  
 Les environnements serveur contiennent des variables de serveur. Les valeurs des variables peuvent être utilisées lorsqu'un package est exécuté ou validé sur le serveur [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
 Les procédures stockées suivantes vous permettent d'exécuter de nombreuses autres tâches de gestion sur les environnements et les variables.  
  
-   [catalog.create_environment &#40;Base de données SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-create-environment-ssisdb-database.md)  
  
-   [catalog.delete_environment &#40;Base de données SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-delete-environment-ssisdb-database.md)  
  
-   [catalog.move_environment &#40;Base de données SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-move-environment-ssisdb-database.md)  
  
-   [catalog.rename_environment &#40;Base de données SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-rename-environment-ssisdb-database.md)  
  
-   [catalog.set_environment_property &#40;Base de données SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-set-environment-property-ssisdb-database.md)  
  
-   [catalog.create_environment_variable &#40;Base de données SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-create-environment-variable-ssisdb-database.md)  
  
-   [catalog.delete_environment_variable &#40;Base de données SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-delete-environment-variable-ssisdb-database.md)  
  
-   [catalog.set_environment_variable_property &#40;Base de données SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-set-environment-variable-property-ssisdb-database.md)  
  
-   [catalog.set_environment_variable_value &#40;Base de données SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-set-environment-variable-value-ssisdb-database.md)  
  
 En appelant la procédure stockée [catalog.set_environment_variable_protection &#40;base de données SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-set-environment-variable-protection-ssisdb-database.md), vous pouvez définir le bit de sensibilité d’une variable.  
  
 Pour utiliser la valeur d'une variable de serveur, spécifiez la référence entre le projet et l'environnement serveur. Vous pouvez utiliser les procédures stockées suivantes pour créer et supprimer des références. Vous pouvez également indiquer si l'environnement peut se trouver dans le même dossier que le projet ou dans un dossier différent.  
  
-   [catalog.create_environment_reference &#40;Base de données SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-create-environment-reference-ssisdb-database.md)  
  
-   [catalog.delete_environment_reference &#40;Base de données SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-delete-environment-reference-ssisdb-database.md)  
  
-   [catalog.set_environment_reference_type &#40;Base de données SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-set-environment-reference-type-ssisdb-database.md)  
  
 Pour plus de détails sur les environnements et les variables, interrogez ces vues.  
  
-   [catalog.environments &#40;Base de données SSISDB&#41;](../../integration-services/system-views/catalog-environments-ssisdb-database.md)  
  
-   [catalog.environment_variables &#40;Base de données SSISDB&#41;](../../integration-services/system-views/catalog-environment-variables-ssisdb-database.md)  
  
-   [catalog.environment_references &#40;Base de données SSISDB&#41;](../../integration-services/system-views/catalog-environment-references-ssisdb-database.md)  
  
##  <a name="executions-and-validations"></a><a name="Executions"></a> Exécutions et validations  
 Une exécution est une instance d'une exécution de package. Appelez [catalog.create_execution &#40;base de données SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-create-execution-ssisdb-database.md) et [catalog.start_execution &#40;base de données SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-start-execution-ssisdb-database.md) pour créer et démarrer une exécution. Pour arrêter une exécution ou une validation de package/projet, appelez [catalog.stop_operation &#40;base de données SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-stop-operation-ssisdb-database.md).  
  
 Pour suspendre un package en cours d'exécution et créer un fichier de vidage, appelez la procédure stockée catalog.create_execution_dump. Le fichier de vidage fournit des informations sur l'exécution d'un package, ce qui peut vous aider à résoudre les problèmes d'exécution. Pour plus d'informations sur la génération et la configuration de fichiers de vidage, consultez [Generating Dump Files for Package Execution](../../integration-services/troubleshooting/generating-dump-files-for-package-execution.md).  
  
 Pour plus de détails sur les exécutions, les validations et les messages enregistrés pendant les opérations, ainsi que pour obtenir des informations contextuelles sur les erreurs, interrogez ces vues.  
  
-   [catalog.executions &#40;Base de données SSISDB&#41;](../../integration-services/system-views/catalog-executions-ssisdb-database.md)  
  
-   [catalog.operations &#40;Base de données SSISDB&#41;](../../integration-services/system-views/catalog-operations-ssisdb-database.md)  
  
-   [catalog.operation_messages &#40;Base de données SSISDB&#41;](../../integration-services/system-views/catalog-operation-messages-ssisdb-database.md)  
  
-   [catalog.extended_operation_info &#40;Base de données SSISDB&#41;](../../integration-services/system-views/catalog-extended-operation-info-ssisdb-database.md)  
  
-   [catalog.event_messages](../../integration-services/system-views/catalog-event-messages.md)  
  
-   [catalog.event_message_context](../../integration-services/system-views/catalog-event-message-context.md)  
  
 Vous pouvez valider des projets et des packages en appelant les procédures stockées [catalog.validate_project &#40;base de données SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-validate-project-ssisdb-database.md) et [catalog.validate_package &#40;base de données SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-validate-package-ssisdb-database.md). La vue [catalog.validations &#40;base de données SSISDB&#41;](../../integration-services/system-views/catalog-validations-ssisdb-database.md) fournit des informations sur les validations, telles que les références d’environnement serveur prises en compte dans la validation, s’il s’agit d’une validation de dépendance ou d’une validation complète, et si le runtime 32 bits ou 64 bits est utilisé pour exécuter le package.  

## <a name="create-the-ssis-catalog"></a>Créer le catalogue SSIS
  Après avoir conçu et testé des packages dans [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)], vous pouvez déployer les projets qui contiennent les packages sur un serveur [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Avant cela, le serveur [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] doit contenir le catalogue **SSISDB** . Le programme d'installation de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] ne crée pas automatiquement le catalogue ; vous devez le créer manuellement à l'aide des instructions suivantes.  
  
 Vous pouvez créer le catalogue SSISDB dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Vous pouvez également créer le catalogue par programmation en utilisant Windows PowerShell.  
  
### <a name="to-create-the-ssisdb-catalog-in-sql-server-management-studio"></a>Pour créer le catalogue SSISDB dans SQL Server Management Studio  
  
1.  Ouvrez [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
2.  Connectez-vous au moteur de base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
3.  Dans l’Explorateur d’objets, développez le nœud du serveur, cliquez avec le bouton droit sur le nœud **Catalogues Integration Services** , puis cliquez sur **Créer un catalogue**.  
  
4.  Cliquez sur **Activer l'intégration du CLR**.  
  
     Le catalogue utilise des procédures stockées du CLR.  
  
5.  Cliquez sur **Activer l’exécution automatique des procédures stockées Integration Services au démarrage de SQL Server** pour permettre à la procédure stockée [catalog.startup](../../integration-services/system-stored-procedures/catalog-startup.md) de s’exécuter à chaque redémarrage de l’instance de serveur [!INCLUDE[ssIS](../../includes/ssis-md.md)] .  
  
     La procédure stockée effectue la maintenance de l'état des opérations pour le catalogue SSISDB. Elle corrige l’état des packages en cours d’exécution si l’instance de serveur [!INCLUDE[ssIS](../../includes/ssis-md.md)] s’arrête.  
  
6.  Entrez un mot de passe, puis cliquez sur **OK**.  
  
     Le mot de passe protège la clé principale de la base de données utilisée pour le chiffrement des données du catalogue. Enregistrez le mot de passe dans un emplacement sécurisé. Il est également recommandé de sauvegarder la clé principale de base de données. Pour plus d'informations, consultez [Back Up a Database Master Key](../../relational-databases/security/encryption/back-up-a-database-master-key.md).  
  
### <a name="to-create-the-ssisdb-catalog-programmatically"></a>Pour créer le catalogue SSISDB par programmation  
  
1.  Exécutez le script PowerShell suivant :  
  
    ```  
    # Load the IntegrationServices Assembly  
    [Reflection.Assembly]::LoadWithPartialName("Microsoft.SqlServer.Management.IntegrationServices")  
  
    # Store the IntegrationServices Assembly namespace to avoid typing it every time  
    $ISNamespace = "Microsoft.SqlServer.Management.IntegrationServices"  
  
    Write-Host "Connecting to server ..."  
  
    # Create a connection to the server  
    $sqlConnectionString = "Data Source=localhost;Initial Catalog=master;Integrated Security=SSPI;"  
    $sqlConnection = New-Object System.Data.SqlClient.SqlConnection $sqlConnectionString  
  
    # Create the Integration Services object  
    $integrationServices = New-Object $ISNamespace".IntegrationServices" $sqlConnection  
  
    # Provision a new SSIS Catalog  
    $catalog = New-Object $ISNamespace".Catalog" ($integrationServices, "SSISDB", "P@assword1")  
    $catalog.Create()  
  
    ```  
  
     Vous trouverez d’autres exemples d’utilisation de Windows PowerShell et de l’espace de noms <xref:Microsoft.SqlServer.Management.IntegrationServices> dans l’entrée de blog [SSIS et PowerShell dans SQL Server 2012](https://techcommunity.microsoft.com/t5/sql-server-integration-services/ssis-and-powershell-in-sql-server-2012/ba-p/388015), sur blogs.msdn.com. Pour obtenir une vue d'ensemble de l'espace de noms et des exemples de code, consultez l'entrée de blog, [A Glimpse of the SSIS Catalog Managed Object Model](https://techcommunity.microsoft.com/t5/sql-server-integration-services/a-glimpse-of-the-ssis-catalog-managed-object-model/ba-p/387892), sur blogs.msdn.com.  

## <a name="catalog-properties-dialog-box"></a>Boîte de dialogue Propriétés du catalogue
  Utilisez la boîte de dialogue Propriétés du catalogue pour configurer le catalogue SSISDB. Les propriétés de catalogue définissent la façon dont les données sensibles sont chiffrées, la façon dont les opérations et les données du contrôle de version du projet sont conservées et le délai d'attente des opérations de validation. Le catalogue SSISDB est une base de données qui représente le point d’administration et de stockage central pour les projets, les packages, les paramètres et les environnements [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
 Vous pouvez également consulter les propriétés de catalogue dans la vue `catalog.catalog_properties` et les définir à l’aide de la procédure stockée `catalog.configure_catalog`. Pour plus d’informations, consultez [catalog.catalog_properties &#40;Base de données SSISDB&#41;](../../integration-services/system-views/catalog-catalog-properties-ssisdb-database.md) et [catalog.configure_catalog &#40;Base de données SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-configure-catalog-ssisdb-database.md).  
  
 **Que voulez-vous faire ?**  
  
-   [Ouvrez la boîte de dialogue Propriétés du catalogue.](#open_dialog)  
  
-   [Configurer les options](#options)  
  
###  <a name="open-the-catalog-properties-dialog-box"></a><a name="open_dialog"></a> Ouvrez la boîte de dialogue Propriétés du catalogue.  
  
1.  Ouvrir [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)][!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)].  
  
2.  Connectez-vous au moteur de base de données Microsoft SQL Server.  
  
3.  Dans l’Explorateur d’objets, développez le nœud **Integration Services** , cliquez avec le bouton droit sur **SSISDB**, puis cliquez sur **Propriétés**.  
  
###  <a name="configure-the-options"></a><a name="options"></a> Configurer les options  
  
#### <a name="options"></a>Options  
 Le tableau décrit certaines propriétés de la boîte de dialogue et les propriétés correspondantes dans la vue `catalog.catalog_properties`.  
  
|Nom de la propriété (boîte de dialogue Propriétés du catalogue)|Nom de la propriété (vue catalog.catalog_properties)|Description|  
|-----------------------------------------------------|------------------------------------------------------|-----------------|  
|Nom de l'algorithme de chiffrement|ENCRYPTION_ALGORITHM|Spécifie le type de chiffrement utilisé pour chiffrer les valeurs des paramètres sensibles dans le catalogue. Les valeurs possibles sont les suivantes :<br /><br /> DES<br /><br /> TRIPLE_DES<br /><br /> TRIPLE_DES_3KEY<br /><br /> DESPX<br /><br /> AES_128<br /><br /> AES_192<br /><br /> AES_256 (par défaut)|  
|Nombre maximal de versions par projet|MAX_PROJECT_VERSIONS|Indiquez combien de versions d’un projet sont stockées dans le catalogue. Les versions antérieures des projets qui dépassent la limite maximale autorisée sont supprimées lors de l’exécution du travail de nettoyage des versions de projets.|  
|Nettoyer les journaux régulièrement|OPERATION_CLEANUP_ENABLED|Définissez la propriété sur True pour indiquer que le travail de l'Agent SQL Server, à savoir le nettoyage des opérations, doit être exécuté. Sinon, définissez la propriété sur False.|  
|Période de rétention (jours)|RETENTION_WINDOW|Spécifiez l'âge maximal des données opérationnelles autorisées (en jours). Les données plus anciennes que le nombre de jours spécifié sont supprimées par le travail de l’Agent SQL, à savoir le nettoyage des opérations.|  

## <a name="back-up-restore-and-move-the-ssis-catalog"></a>Sauvegarder, restaurer et déplacer le catalogue SSIS
[!INCLUDE[tsql-appliesto-ss2016-xxxx-xxxx-xxx_md](../../includes/tsql-appliesto-ss2016-xxxx-xxxx-xxx-md.md)]

  [!INCLUDE[ssISCurrent](../../includes/ssiscurrent-md.md)] comprend la base de données SSISDB. Interrogez les vues de la base de données SSISDB pour inspecter les objets, les paramètres et les données opérationnelles stockés dans le catalogue **SSISDB** . Cette rubrique fournit des instructions sur la sauvegarde et la restauration de la base de données.  
  
 Le catalogue **SSISDB** stocke les packages que vous avez déployés sur le serveur [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. Pour plus d’informations sur le catalogue, consultez [Catalogue SSIS](../../integration-services/catalog/ssis-catalog.md).  
  
###  <a name="to-back-up-the-ssis-database"></a><a name="backup"></a> Pour sauvegarder la base de données SSIS  
  
1.  Ouvrez [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] et connectez-vous à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
2.  Sauvegardez la clé principale de la base de données SSISDB, à l'aide de l'instruction Transact-SQL BACKUP MASTER KEY. La clé est stockée dans un fichier que vous spécifiez. Utilisez un mot de passe pour chiffrer la clé principale dans le fichier.  
  
     Pour plus d’informations sur l’instruction, consultez [BACKUP MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/backup-master-key-transact-sql.md).  
  
     Dans l’exemple suivant, la clé principale est exportée vers le fichier `c:\temp directory\RCTestInstKey` . Le mot de passe `LS2Setup!` est utilisé pour chiffrer la clé principale.  
  
    ```  
    backup master key to file = 'c:\temp\RCTestInstKey'  
           encryption by password = 'LS2Setup!'  
  
    ```  
  
3.  Sauvegardez la base de données SSISDB à l’aide de la boîte de dialogue **Sauvegarder la base de données** dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Pour plus d’informations, consultez [Procédure : sauvegarder une base de données (SQL Server Management Studio)](../../relational-databases/backup-restore/create-a-full-database-backup-sql-server.md).  
  
4.  Générez le script CREATE LOGIN pour ##MS_SSISServerCleanupJobLogin## en effectuant les actions suivantes. Pour plus d’informations, consultez [CREATE LOGIN &#40;Transact-SQL&#41;](../../t-sql/statements/create-login-transact-sql.md).  
  
    1.  Dans l’Explorateur d’objets de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], développez le nœud **Sécurité** , puis le nœud **Connexions** .  
  
    2.  Cliquez avec le bouton droit sur **##MS_SSISServerCleanupJobLogin##** , puis cliquez sur **Générer un script de la connexion en tant que** > **CREATE To** > **Nouvelle fenêtre d’éditeur de requête**.  
  
5.  Si vous restaurez la base de données SSISDB sur une instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] où le catalogue SSISDB n’a jamais été créé, générez le script CREATE PROCEDURE pour sp_ssis_startup en effectuant les actions suivantes. Pour plus d’informations, consultez [CREATE PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/create-procedure-transact-sql.md).  
  
    1.  Dans l’Explorateur d’objets, développez le nœud **Bases de données** , puis le nœud **Clé principale** > **Programmabilité** > **Procédures stockées** .  
  
    2.  Cliquez avec le bouton droit sur **dbo.sp_ssis_startup**, puis cliquez sur **Générer un script de la procédure stockée en tant que** > **CREATE To** > **Nouvelle fenêtre d’éditeur de requête**.  
  
6.  Assurez-vous que Le SQL Server Agent a démarré.  
  
7.  Si vous restaurez la base de données SSISDB sur une instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] où le catalogue SSISDB n’a jamais été créé, générez un script pour la tâche de maintenance de serveur SSIS en effectuant les opérations suivantes. Le script est créé automatiquement dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent quand le catalogue SSISDB est créé. Le travail permet de nettoyer les journaux d'opérations de nettoyage en dehors de la période de conservation et de supprimer les versions antérieures des projets.  
  
    1.  Dans l’Explorateur d’objets, développez le nœud **SQL Server Agent** , puis le nœud **Travaux** .  
  
    2.  Cliquez avec le bouton droit sur le travail de maintenance de serveur SSIS, puis cliquez sur **Générer un script du travail en tant que** > **CREATE To** > **Nouvelle fenêtre d’éditeur de requête**.  
  
### <a name="to-restore-the-ssis-database"></a>Pour restaurer la base de données SSIS  
  
1.  Si vous restaurez la base de données SSISDB sur une instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] où le catalogue SSISDB n’a jamais été créé, activez le CLR en exécutant la procédure stockée `sp_configure`. Pour plus d’informations, consultez [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) et [clr enabled (option de configuration de serveur)](../../database-engine/configure-windows/clr-enabled-server-configuration-option.md).  
  
    ```  
    use master   
           sp_configure 'clr enabled', 1  
           reconfigure  
  
    ```  
  
2.  Vous restaurez la base de données SSISDB sur une instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] où le catalogue SSISDB n'a jamais été créé, créez la clé asymétrique et la connexion à partir de la clé asymétrique et accordez l'autorisation UNSAFE à la connexion.  

    ```  
    Create Asymmetric Key MS_SQLEnableSystemAssemblyLoadingKey  
           FROM Executable File = 'C:\Program Files\Microsoft SQL Server\YourSQLServerDefaultCompatibilityLevel\DTS\Binn\Microsoft.SqlServer.IntegrationServices.Server.dll'  
    ```  

    Vous pouvez trouver la valeur de `YourSQLServerDefaultCompatibilityLevel` dans une [liste des niveaux de compatibilité par défaut de SQL Server](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md#arguments).
  
    [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] Les procédures stockées CLR exigent l’octroi d’autorisations UNSAFE à la connexion, car cette dernière nécessite un accès supplémentaire aux ressources restreintes, par exemple l’API Win32 de Microsoft. Pour plus d’informations sur l’autorisation de code UNSAFE, consultez [Création d’un assembly](../../relational-databases/clr-integration/assemblies/creating-an-assembly.md).  

    ```  
    Create Login ##MS_SQLEnableSystemAssemblyLoadingUser## FROM Asymmetric Key MS_SQLEnableSystemAssemblyLoadingKey   
    Grant Unsafe Assembly to ##MS_SQLEnableSystemAssemblyLoadingUser##    
    ```  
  
3.  Restaurez la base de données SSISDB à partir de la sauvegarde, à l’aide de la boîte de dialogue **Restaurer la base de données** dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Pour plus d'informations, voir les rubriques suivantes :  
  
    -   [Restaurer la base de données &#40;page Général&#41;](../../relational-databases/backup-restore/restore-database-general-page.md)  
  
    -   [Restaurer la base de données &#40;page Fichiers&#41;](../../relational-databases/backup-restore/restore-database-files-page.md)  
  
    -   [Restaurer la base de données &#40;page Options&#41;](../../relational-databases/backup-restore/restore-database-options-page.md)  
  
4.  Exécutez les scripts que vous avez créés dans la procédure [Pour sauvegarder la base de données SSIS](#backup) pour ##MS_SSISServerCleanupJobLogin##, sp_ssis_startup et le travail de maintenance de serveur SSIS. Assurez-vous que SQL Server Agent a démarré.  
  
5.  Exécutez l’instruction suivante pour définir la procédure sp_ssis_startup d’exécution automatique. Pour plus d’informations, consultez [sp_procoption &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-procoption-transact-sql.md).  
  
    ```  
    EXEC sp_procoption N'sp_ssis_startup','startup','on'  
    ```  
  
6.  Mappez l’utilisateur SSISDB ##MS_SSISServerCleanupJobUser## (base de données SSISDB) à ##MS_SSISServerCleanupJobLogin##, à l’aide de la boîte de dialogue **Propriétés de la connexion** dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
7.  Restaurez la clé principale à l'aide de l'une des méthodes suivantes. Pour plus d’informations sur le chiffrement, consultez [Hiérarchie de chiffrement](../../relational-databases/security/encryption/encryption-hierarchy.md).  
  
    -   **Méthode 1**  
  
         Utilisez cette méthode si vous avez déjà effectué une sauvegarde de la clé principale de base de données et si vous disposez du mot de passe de chiffrement de la clé principale.  
  
        ```  
               Restore master key from file = 'c:\temp\RCTestInstKey'  
               Decryption by password = 'LS2Setup!' -- 'Password used to encrypt the master key during SSISDB backup'  
               Encryption by password = 'LS3Setup!' -- 'New Password'  
               Force  
  
        ```  
  
        > [!NOTE]  
        >  Assurez-vous que le compte de service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dispose des autorisations nécessaires pour lire le fichier de clé de sauvegarde.  
  
        > [!NOTE]  
        >  Le message d’avertissement suivant s’affiche dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] si la clé principale de la base de données n’a pas encore été chiffrée par la clé principale du service. Ignorez le message d'avertissement.  
        >   
        >  **Impossible de déchiffrer la clé principale active. Cette erreur a été ignorée, car l’option FORCE a été spécifiée.**  
        >   
        >  L'argument FORCE spécifie que le processus de restauration doit continuer même si la clé principale de base de données actuelle n'est pas ouverte. Pour le catalogue SSISDB, comme la clé principale de la base de données n’a pas été ouverte sur l’instance où est restaurée la base de données, ce message s’affiche.  
  
    -   **Méthode 2**  
  
         Utilisez cette méthode si vous disposez du mot de passe d'origine utilisé pour créer SSISDB.  
  
        ```  
        open master key decryption by password = 'LS1Setup!' --'Password used when creating SSISDB'  
               Alter Master Key Add encryption by Service Master Key  
        ```  
  
8.  Déterminez si le schéma de catalogue SSISDB et les binaires [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] (assembly SQLCLR et ISServerExec) sont compatibles en exécutant [catalog.check_schema_version](../../integration-services/system-stored-procedures/catalog-check-schema-version.md).  
  
9. Pour vérifier que la base de données SSISDB a été restaurée correctement, effectuez des opérations sur le catalogue SSISDB, par exemple exécutez des packages déployés sur le serveur [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Pour plus d’informations, consultez la page [Exécuter des packages Integration Services (SSIS)](../../integration-services/packages/run-integration-services-ssis-packages.md).  
  
### <a name="to-move-the-ssis-database"></a>Pour déplacer la base de données SSIS  
  
-   Suivez les instructions pour déplacer les bases de données utilisateur. Pour plus d’informations, consultez [Déplacer des bases de données utilisateur](../../relational-databases/databases/move-user-databases.md).  
  
     Veillez à sauvegarder la clé principale de la base de données SSISDB et à protéger le fichier de sauvegarde. Pour plus d’informations, consultez [Pour sauvegarder la base de données SSIS](#backup).  
  
     Vérifiez que les objets Integration Services (SSIS) appropriés sont créés dans la nouvelle instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] où le catalogue SSISDB n’a pas encore été créé.  

## <a name="upgrade-the-ssis-catalog-ssisdb"></a>Mettre à niveau le catalogue SSIS (SSISDB)
  Exécutez l’Assistant Mise à niveau de SSISDB pour mettre à niveau la base de données du catalogue SSIS, SSISDB, quand celle-ci est plus ancienne que la version actuelle de l’instance SQL Server. La base de données peut être plus ancienne quand l’une des conditions suivantes est remplie.  
  
-   Vous avez restauré la base de données à partir d’une ancienne version de SQL Server.  
  
-   Vous n’avez pas supprimé la base de données d’un groupe de disponibilité Always On avant la mise à niveau de l’instance SQL Server. Cette condition empêche la mise à niveau automatique de la base de données. Pour plus d’informations, consultez [Upgrading SSISDB in an availability group](#Upgrade).  
  
 L’assistant peut uniquement mettre à niveau la base de données sur une instance de serveur local.  
  
### <a name="upgrade-the-ssis-catalog-ssisdb-by-running-the-ssisdb-upgrade-wizard"></a>Mettre à niveau le catalogue SSIS (SSISDB) en exécutant l’Assistant Mise à niveau de SSISDB  
  
1.  Sauvegardez la base de données de catalogues SSIS, SSISDB.  
  
2.  Dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], développez le serveur local puis développez **Catalogues Integration Services**.  
  
3.  Cliquez avec le bouton droit sur **SSISDB**, puis sélectionnez **Mise à niveau de base de données** pour lancer l’Assistant Mise à niveau de SSISDB. Vous pouvez aussi lancer l’Assistant Mise à niveau de SSISDB en exécutant `C:\Program Files\Microsoft SQL Server\140\DTS\Binn\ISDBUpgradeWizard.exe` avec des autorisations élevées sur le serveur local.
  
     ![Lancer l’Assistant Mise à niveau de SSISDB](../../integration-services/service/media/ssisdb-upgrade-wizard-1.png)

4.  Sur la page **Sélectionner une instance** , sélectionnez une instance de SQL Server sur le serveur local.  
  
    > [!IMPORTANT]  
    >  L’assistant peut uniquement mettre à niveau la base de données sur une instance de serveur local.  
  
     Sélectionnez la case à cocher pour indiquer que vous avez sauvegardé la base de données SSISDB avant d’exécuter l’assistant.  
  
     ![Sélectionner le serveur dans l’Assistant Mise à niveau de SSISDB](../../integration-services/service/media/ssisdb-upgrade-wizard-2.png "Sélectionner le serveur dans l’Assistant Mise à niveau de SSISDB")  
  
5.  Sélectionnez **Mettre à niveau** pour mettre à niveau la base de données du catalogue SSIS.  
  
6.  Sur la page **Résultat** , passez en revue les résultats.  
  
     ![Consulter les résultats dans l’Assistant Mise à niveau de SSISDB](../../integration-services/service/media/ssisdb-upgrade-wizard-3.png "Consulter les résultats dans l’Assistant Mise à niveau de SSISDB")  

## <a name="always-on-for-ssis-catalog-ssisdb"></a>Always On pour le catalogue SSIS (SSISDB)
  La fonctionnalité des groupes de disponibilité AlwaysOn est une solution de haute disponibilité et de récupération d’urgence qui offre une alternative au niveau de l’entreprise à la mise en miroir de bases de données. Un groupe de disponibilité prend en charge un environnement de basculement pour un ensemble discret de bases de données utilisateur, appelées bases de données de disponibilité, qui basculent de concert. Pour plus d’informations, consultez [Groupes de disponibilité AlwaysOn](../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md).  
  
 Pour assurer la haute disponibilité du catalogue SSIS (base de données SSISDB) et de son contenu (projets, packages, journaux d’exécution, etc.), vous pouvez ajouter la base de données SSISDB (de la même façon que toute autre base de données utilisateur) à un groupe de disponibilité AlwaysOn. Quand un basculement se produit, le nœud secondaire devient automatiquement le nouveau nœud primaire.  
 
 > [!IMPORTANT]
 > Quand un basculement se produit, les packages en cours d’exécution ne redémarrent pas ou ne reprennent pas. 
 
 **Dans cette section :**  
  
1.  [Composants requis](#prereq)  
  
2.  [Configurer la prise en charge de SSIS pour AlwaysOn](#Firsttime)  
  
3.  [Mise à niveau de la base de données SSISDB dans un groupe de disponibilité](#Upgrade)  
  
###  <a name="prerequisites"></a><a name="prereq"></a> Conditions préalables  
Avant d’activer la prise en charge d’Always On pour la base de données SSISDB, effectuez les étapes suivantes.  
  
1.  Configurez un cluster de basculement Windows. Pour obtenir des instructions, voir le billet de blog [Installing the Failover Cluster Feature and Tools for Windows Server 2012](https://techcommunity.microsoft.com/t5/failover-clustering/installing-the-failover-cluster-feature-and-tools-in-windows/ba-p/371733) (Installation de la fonctionnalité de cluster de basculement et des outils pour Windows Server 2012). Installez la fonctionnalité et les outils sur tous les nœuds de cluster.  
  
2.  Installez SQL Server 2016 avec la fonctionnalité Integration Services (SSIS) sur chaque nœud du cluster.  
  
3.  Activez les groupes de disponibilité Always On pour chaque instance SQL Server. Pour plus d’informations, consultez [Activer et désactiver les groupes de disponibilité AlwaysOn (SQL Server)](../../database-engine/availability-groups/windows/enable-and-disable-always-on-availability-groups-sql-server.md) .  
  
###  <a name="configure-ssis-support-for-always-on"></a><a name="Firsttime"></a> Configurer la prise en charge de SSIS pour AlwaysOn  
  
-   [Étape 1 : Créer un catalogue Integration Services](#Step1)  
  
-   [Étape 2 : Ajouter la base de données SSISDB à un groupe de disponibilité Always On](#Step2)  
  
-   [Étape 3 : Activer la prise en charge de SSIS pour Always On](#Step3)  
  
> [!IMPORTANT]  
> -   Vous devez exécuter les étapes suivantes sur le **nœud primaire** du groupe de disponibilité.
> -   Vous devez activer la **prise en charge d’Always On par SSIS** *après* avoir ajouté la base de données SSISDB à un groupe de disponibilité Always On.  

> [!NOTE]
> Pour plus d’informations sur cette procédure, consultez le guide pas à pas suivant, qui comprend d’autres captures d’écran de Marcos Freccia (MVP Data Platform) : [Ajouter une base de données SSISDB à un groupe de disponibilité pour SQL Server 2016](https://marcosfreccia.com/2017/04/28/adding-ssisdb-to-ag-for-sql-server-2016/).

####  <a name="step-1-create-integration-services-catalog"></a><a name="Step1"></a> Étape 1 : Créer un catalogue Integration Services  
  
1.  Lancez **SQL Server Management Studio** , puis connectez-vous à une instance SQL Server dans le cluster que vous voulez définir comme **nœud primaire** du groupe de disponibilité AlwaysOn pour la base de données SSISDB.  
  
2.  Dans l’Explorateur d’objets, développez le nœud du serveur, cliquez avec le bouton droit sur le nœud **Catalogues Integration Services** , puis cliquez sur **Créer un catalogue**.  
  
3.  Cliquez sur **Activer l'intégration du CLR**. Le catalogue utilise des procédures stockées du CLR.  
  
4.  Cliquez sur **Activer l’exécution automatique des procédures stockées Integration Services au démarrage de SQL Server** pour que la procédure stockée [catalog.startup](../system-stored-procedures/catalog-startup.md) soit exécutée à chaque redémarrage de l’instance de serveur SSIS. La procédure stockée effectue la maintenance de l'état des opérations pour le catalogue SSISDB. Elle résout l’état de tous les packages en cours d’exécution si et quand l’instance de serveur SSIS s’arrête.  
  
5.  Entrez un **mot de passe**, puis cliquez sur **OK**. Le mot de passe protège la clé principale de la base de données utilisée pour le chiffrement des données du catalogue. Enregistrez le mot de passe dans un emplacement sécurisé. Il est également recommandé de sauvegarder la clé principale de base de données. Pour plus d'informations, consultez [Back Up a Database Master Key](../../relational-databases/security/encryption/back-up-a-database-master-key.md).  
  
####  <a name="step-2-add-ssisdb-to-an-always-on-availability-group"></a><a name="Step2"></a> Étape 2 : Ajouter la base de données SSISDB à un groupe de disponibilité Always On  
La procédure à suivre pour ajouter la base de données SSISDB à un groupe de disponibilité AlwaysOn est presque identique à celle qui permet d’ajouter n’importe quelle autre base de données utilisateur à un groupe de disponibilité. Voir [Utiliser l’Assistant groupe de disponibilité](../../database-engine/availability-groups/windows/use-the-availability-group-wizard-sql-server-management-studio.md).  
  
Indiquez le mot de passe que vous avez spécifié durant la création du catalogue SSIS sur la page **Sélectionner les bases de données** de l’Assistant **Nouveau groupe de disponibilité**.

![Nouveau groupe de disponibilité](../../integration-services/service/media/ssis-newavailabilitygroup.png "Nouveau groupe de disponibilité")  
  
####  <a name="step-3-enable-ssis-support-for-always-on"></a><a name="Step3"></a> Étape 3 : Activer la prise en charge de SSIS pour Always On  
 Après avoir créé le catalogue Integration Services, cliquez avec le bouton droit sur le nœud **Catalogues Integration Services**, puis cliquez sur **Activer la prise en charge d’Always On**. La boîte de dialogue **Enable Support for AlwaysOn** (Activer la prise en charge d’AlwaysOn) doit s’afficher. Si cette option de menu est désactivée, vérifiez que vous disposez de tous les composants requis, puis cliquez sur **Actualiser**.  
  
 ![Activer la prise en charge d’Always On](../../integration-services/service/media/ssis-enablesupportforalwayson.png)  
  
> [!WARNING]  
>  Le basculement automatique de la base de données SSISDB n’est pas pris en charge tant que vous n’activez pas la prise en charge de SSIS pour AlwaysOn.  
  
 Les réplicas secondaires récemment ajoutés à partir du groupe de disponibilité Always On apparaissent dans le tableau. Cliquez sur le bouton de **connexion** pour chaque réplica figurant dans la liste, puis entrez les informations d’identification d’authentification pour la connexion au réplica. Le compte d’utilisateur doit être membre du groupe sysadmin sur chaque réplica pour pouvoir activer la prise en charge d’Always On par SSIS. Une fois connecté à chaque réplica, cliquez sur **OK** pour activer la prise en charge de SSIS pour AlwaysOn.  
 
Si l’option **Activer la prise en charge d’Always On** du menu contextuel semble désactivée une fois que vous avez rempli les autres prérequis, essayez d’effectuer les actions suivantes :
1.  Actualisez le menu contextuel en cliquant sur l’option **Actualiser**.
2.  Vérifiez que vous vous connectez au nœud principal. Vous devez activer la prise en charge d’Always On sur le nœud principal.
3.  Vérifiez que la version de SQL Server est au moins égale à 13.0. SSIS prend en charge Always On uniquement sur SQL Server 2016 et les versions ultérieures.

###  <a name="upgrading-ssisdb-in-an-availability-group"></a><a name="Upgrade"></a> Mise à niveau de la base de données SSISDB dans un groupe de disponibilité  
 Si vous mettez à niveau SQL Server à partir d’une version précédente et si la base de données SSISDB se trouve dans un groupe de disponibilité AlwaysOn, la mise à niveau peut être bloquée par la règle « Vérification : SSISDB est dans des groupes de disponibilité AlwaysOn ». Ce blocage se produit parce que la mise à niveau s’exécute en mode mono-utilisateur, alors qu’une base de données de disponibilité doit être une base de données multi-utilisateurs. Par conséquent, durant une mise à niveau ou une mise à jour corrective, toutes les bases de données de disponibilité, y compris la base de données SSISDB, sont mises hors connexion, et ne sont pas mises à niveau ou corrigées. Pour permettre la poursuite de la mise à niveau, supprimez la base de données SSISDB du groupe de disponibilité, mettez à niveau ou corrigez chaque nœud, puis rajoutez la base de données SSISDB au groupe de disponibilité.  
  
 Si la règle « Vérification : SSISDB dans un groupe de disponibilité Always On » vous bloque, mettez à niveau SQL Server en effectuant les étapes suivantes.  
  
1.  Supprimez la base de données SSISDB du groupe de disponibilité. Pour plus d’informations, consultez [Supprimer une base de données secondaire d’un groupe de disponibilité &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/remove-a-secondary-database-from-an-availability-group-sql-server.md) et [Supprimer une base de données primaire d’un groupe de disponibilité &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/remove-a-primary-database-from-an-availability-group-sql-server.md).  
  
2.  Cliquez sur **Réexécuter** dans l’Assistant Mise à niveau. La règle « Vérification : SSISDB dans un groupe de disponibilité Always On » ne bloque plus.  
  
3.  Cliquez sur **Suivant** pour continuer la mise à niveau.  
  
4.  Une fois tous les nœuds mis à niveau, ajoutez la base de données SSISDB au groupe de disponibilité AlwaysOn. Pour plus d’informations, consultez [Ajouter une base de données à un groupe de disponibilité &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/availability-group-add-a-database.md).  
  
 Si rien ne vous bloque pendant la mise à niveau de SQL Server et que la base de données SSISDB se trouve dans un groupe de disponibilité Always On, mettez à niveau la base de données SSISDB séparément après avoir mis à niveau le moteur de base de données SQL Server. Utilisez l’Assistant Mise à niveau de SSIS pour mettre à niveau la base de données SSISDB comme décrit dans la procédure suivante.  
  
1.  Déplacez la base de données SSISDB hors du groupe de disponibilité, ou supprimez le groupe de disponibilité si la base de données SSISDB est la seule base de données figurant dans le groupe de disponibilité. Pour effectuer cette tâche, lancez **SQL Server Management Studio** sur le **nœud principal** du groupe de disponibilité.  
  
2.  Supprimez la base de données SSISDB de tous les **nœuds de réplica**.  
  
3.  Mettez à niveau la base de données SSISDB sur le **nœud primaire**. Dans **l’Explorateur d’objets** de SQL Server Management Studio, développez **Catalogues Integration Services**, cliquez avec le bouton droit sur **SSISDB**, puis sélectionnez **Mise à niveau de la base de données**. Suivez les instructions de l’ **Assistant Mise à niveau de SSISDB** pour mettre à niveau la base de données. Lancez **l’Assistant Mise à niveau de SSIDB** localement sur le **nœud principal**.  
  
4.  Suivez les instructions de l’[étape 2 : Ajouter la base de données SSISDB à un groupe de disponibilité Always On](#Step2) pour rajouter la base de données SSISDB à un groupe de disponibilité.  
  
5.  Suivez les instructions de l’[étape 3 : Activer la prise en charge de SSIS pour Always On](#Step3).  


## <a name="ssisdb-catalog-and-delegation-in-double-hop-scenarios"></a>Catalogue et délégation SSISDB dans les scénarios à double tronçon

Par défaut, l’appel à distance des packages SSIS stockés dans le catalogue SSISDB ne prend pas en charge la délégation des informations d’identification, parfois appelée double tronçon. 

Imaginez un scénario dans lequel un utilisateur se connecte à la machine cliente A et lance SQL Server Management Studio (SSMS). À partir de SSMS, l’utilisateur se connecte à un serveur SQL Server hébergé sur la machine B, qui a le catalogue SSISDB. Le package SSIS est stocké sous ce catalogue SSISDB et le package se connecte à son tour à un service SQL Server qui s’exécute sur la machine C (le package peut également accéder à d’autres services). Quand l’utilisateur appelle l’exécution du package SSIS à partir de la machine A, SSMS passe d’abord les informations d’identification de l’utilisateur de la machine A à la machine B (où le processus de runtime SSIS exécute le package). Le processus de runtime d’exécution SSIS (ISServerExec.exe) est maintenant nécessaire pour déléguer les informations d’identification de l’utilisateur de la machine B à la machine C pour que l’exécution s’effectue correctement. Cependant, la délégation des informations d’identification n’est pas activée par défaut.

Un utilisateur peut activer la délégation des informations d’identification en octroyant le droit *Approuver cet utilisateur pour la délégation à tous les services (Kerberos uniquement)* au compte de service SQL Server (sur la machine B), qui lance ISServerExec.exe en tant que processus enfant. Ce processus est appelé « configuration d’une délégation non contrainte » ou « délégation ouverte » pour un compte de service SQL Server. Avant d’octroyer ce droit, demandez-vous s’il est conforme aux exigences de sécurité de votre organisation.

SSISDB ne prend pas en charge la délégation contrainte. Dans un environnement à double tronçon, si le compte de service du serveur SQL Server qui héberge le catalogue SSISDB (la machine B dans notre exemple) est configuré pour la délégation contrainte, ISServerExec.exe ne peut pas déléguer les informations d’identification à la troisième machine (la machine C). Ceci s’applique aux scénarios dans lesquels Windows Credential Guard est activé, qui nécessite impérativement la configuration de la délégation contrainte.

  
##  <a name="related-content"></a><a name="RelatedContent"></a> Contenu associé  
  
-   Entrée de blog, [SSIS and PowerShell in SQL Server 2012](https://techcommunity.microsoft.com/t5/sql-server-integration-services/ssis-and-powershell-in-sql-server-2012/ba-p/388015), sur blogs.msdn.com.  
  
-   Entrée de blog [Conseils pour le contrôle d'accès du catalogue SSIS](https://techcommunity.microsoft.com/t5/sql-server-integration-services/ssis-catalog-access-control-tips/ba-p/388057), sur blogs.msdn.com.  
  
-   Entrée de blog, [A Glimpse of the SSIS Catalog Managed Object Model](https://techcommunity.microsoft.com/t5/sql-server-integration-services/a-glimpse-of-the-ssis-catalog-managed-object-model/ba-p/387892), sur blogs.msdn.com.