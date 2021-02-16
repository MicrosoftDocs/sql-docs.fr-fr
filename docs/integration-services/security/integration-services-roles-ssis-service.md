---
description: Rôles Integration Services (Service SSIS)
title: Rôles Integration Services (Service SSIS) | Microsoft Docs
ms.custom: security
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.dtsserver.packageroles.f1
helpviewer_keywords:
- security [Integration Services], roles
- db_ssisoperator role
- db_ssisadmin role
- fixed database roles [Integration Services]
- packages [Integration Services], security
- roles [Integration Services]
- db_ssisltduser role
ms.assetid: 9702e90c-fada-4978-a473-1b1423017d80
author: chugugrace
ms.author: chugu
ms.openlocfilehash: d8801c1a4f7387f1ff5bc815b45897d5a4f68fdb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340784"
---
# <a name="integration-services-roles-ssis-service"></a>Rôles Integration Services (Service SSIS)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] fournit certains rôles fixes de niveau base de données pour aider à sécuriser l’accès aux packages stockés dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Les rôles disponibles diffèrent selon que vous enregistrez les packages dans la base de données du catalogue SSIS (SSISDB) ou dans la base de données msdb.  
  
## <a name="roles-in-the-ssis-catalog-database-ssisdb"></a>Rôles dans la base de données du catalogue SSIS (SSISDB)  
 La base de données du catalogue SSIS (SSISDB) fournit les rôles fixes au niveau de la base de données décrits ci-après. Ces rôles contribuent à sécuriser l’accès aux packages et aux informations sur ces derniers.  
  
-   **ssis_admin**: ce rôle fournit un accès administratif complet à la base de données du catalogue SSIS.  
  
-   **ssis_logreader** : ce rôle fournit les autorisations d’accès à toutes les vues associées aux journaux des opérations SSISDB.  
  
     La liste de vues est la suivante : [catalog].[projects], [catalog].[packages], [catalog].[operations], [catalog].[extended_operation_info], [catalog].[operation_messages], [catalog].[event_messages], [catalog].[execution_data_statistics], [catalog].[execution_component_phases], [catalog].[execution_data_taps], [catalog].[event_message_context], [catalog].[executions], [catalog].[executables], [catalog].[executable_statistics], [catalog].[validations], [catalog].[execution_parameter_values] et [catalog].[execution_property_override_values].  
  
## <a name="roles-in-the-msdb-database"></a>Rôles dans la base de données msdb  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] inclut trois rôles fixes de niveau base de données **db_ssisadmin**, **db_ssisltduser** et **db_ssisoperator**. Ces rôles permettent de contrôler l’accès aux packages enregistrés dans la base de données **msdb**. Vous affectez des rôles à un package à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Les affectations de rôles sont enregistrées dans la base de données **ms** .  
  
### <a name="read-and-write-actions"></a>Actions de lecture et d'écriture  
 Le tableau suivant décrit les actions de lecture et d’écriture de Windows et des rôles fixes de niveau base de données dans [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
|Role|Action de lecture|Action d'écriture|  
|----------|-----------------|------------------|  
|**db_ssisadmin**<br /><br /> ou<br /><br /> **sysadmin**|Énumérer ses packages.<br /><br /> Énumérer tous les packages.<br /><br /> Afficher ses packages.<br /><br /> Afficher tous les packages.<br /><br /> Exécuter ses packages.<br /><br /> Exécuter tous les packages.<br /><br /> Exporter ses packages.<br /><br /> Exporter tous les packages.<br /><br /> Exécuter tous les packages dans l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|Importer des packages.<br /><br /> Supprimer ses packages.<br /><br /> Supprimer tous les packages.<br /><br /> Modifier les rôles de ses packages.<br /><br /> Modifier tous les rôles de package.<br /><br /> <br /><br /> **\*\* Avertissement \*\*** Il se peut que les membres du rôle db_ssisadmin et du rôle dc_admin puissent élever leurs privilèges à sysadmin. Cette élévation de privilège peut se produire, car ces rôles peuvent modifier les packages [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] et les packages [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] peuvent être exécutés par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à l’aide du contexte de sécurité sysadmin de l’Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Pour vous prémunir contre cette élévation de privilège lors de l'exécution de plans de maintenance, de jeux d'éléments de collecte de données et d'autres packages [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , configurez des travaux de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui exécutent des packages pour l'utilisation d'un compte proxy doté de privilèges limités ou ajoutez uniquement des membres sysadmin aux rôles db_ssisadmin et dc_admin.|  
|**db_ssisltduser**|Énumérer ses packages.<br /><br /> Énumérer tous les packages.<br /><br /> Afficher ses packages.<br /><br /> Exécuter ses packages.<br /><br /> Exporter ses packages.|Importer des packages.<br /><br /> Supprimer ses packages.<br /><br /> Modifier les rôles de ses packages.|  
|**db_ssisoperator**|Énumérer tous les packages.<br /><br /> Afficher tous les packages.<br /><br /> Exécuter tous les packages.<br /><br /> Exporter tous les packages.<br /><br /> Exécuter tous les packages dans l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|Aucun|  
|**Administrateurs Windows**|Afficher les détails d'exécution de tous les packages en cours d'exécution.|Arrêter tous les packages en cours d'exécution.|  
  
### <a name="sysssispackages-table"></a>Table sysssispackages  
 La table **sysssispackages** dans la base de données **msdb** contient les packages enregistrés dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d’informations, consultez [sysssispackages &#40;Transact-SQL&#41;](../../relational-databases/system-tables/sysssispackages-transact-sql.md).  
  
 La table **sysssispackages** inclut des colonnes contenant des informations sur les rôles affectés aux packages.  
  
-   Le rôle **readerrole** spécifie le rôle qui bénéficie d'un accès en lecture au package.  
  
-   La colonne **writerrole** spécifie le rôle qui bénéficie d'un accès en écriture au package.  
  
-   La colonne **ownersid** contient l'identificateur de sécurité unique de l'utilisateur qui a créé le package. Cette colonne définit le propriétaire du package.  
  
### <a name="permissions"></a>Autorisations  
 Par défaut, les autorisations des rôles fixes de niveau base de données **db_ssisadmin** et **db_ssisoperator** et l’identificateur de sécurité unique de l’utilisateur qui a créé le package s’appliquent au rôle de lecteur pour les packages, tandis que les autorisations du rôle **db_ssisadmin** et l’identificateur de sécurité unique de l’utilisateur qui a créé le package s’appliquent au rôle de rédacteur. Un utilisateur doit être membre du rôle **db_ssisadmin**, **db_ssisltduser** ou **db_ssisoperator** pour bénéficier d’un accès en lecture au package. Un utilisateur doit être membre du rôle **db_ssisadmin** pour bénéficier d’un accès en écriture.  
  
### <a name="access-to-packages"></a>Accès aux packages  
 Les rôles fixes au niveau de la base de données fonctionnent conjointement avec les rôles définis par l'utilisateur. Les rôles définis par l’utilisateur sont les rôles que vous créez dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] , puis que vous utilisez pour attribuer des autorisations aux packages. Pour accéder à un package, un utilisateur doit être membre du rôle défini par l’utilisateur et du rôle fixe pertinent de niveau base de données [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Par exemple, si des utilisateurs sont membres du rôle défini par l’utilisateur **AuditUsers** affecté à un package, ils doivent aussi être membres du rôle **db_ssisadmin**, **db_ssisltduser** ou **db_ssisoperator** pour disposer d’un accès en lecture au package.  
  
 Si vous n'affectez pas des rôles définis par l'utilisateur aux packages, les rôles de base de données fixes déterminent l'accès aux packages.  
  
 Si vous voulez utiliser des rôles définis par l’utilisateur, vous devez les ajouter à la base de données **msdb** avant de pouvoir les affecter à des packages. Vous pouvez créer de nouveaux rôles de base de données dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
 Les rôles de niveau base de données [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] accordent des droits sur les tables système [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] de la base de données msdb.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (le service MSSQLSERVER) doit être démarré pour que vous puissiez vous connecter au moteur de base de données et accéder à la base de données **msdb** .  
  
 Pour affecter des rôles dans les packages, vous devez effectuer les tâches suivantes.  
  
-   **Ouvrir l'Explorateur d'objets et se connecter à Integration Services**  
  
     Avant de pouvoir affecter des rôles à des packages à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], vous devez ouvrir l'Explorateur d'objets dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] et vous connecter à [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
     Le service [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] doit être démarré pour que vous puissiez vous connecter à [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
-   **Affecter des rôles de lecture et d'écriture aux packages**  
  
     Vous pouvez affecter un rôle de lecture et d'écriture à chaque package.  

## <a name="assign-a-reader-and-writer-role-to-a-package"></a><a name="assign"></a> Affecter un rôle de lecture et d'écriture à un package
  Vous pouvez affecter un rôle de lecture et d'écriture à chaque package.  
  
### <a name="assign-a-reader-and-writer-role-to-a-package"></a>Affecter un rôle de lecture et d'écriture à un package  
  
1.  Dans l'Explorateur d'objets, localisez la connexion [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
2.  Développez le dossier Packages stockés, puis le sous-dossier contenant le package auquel vous voulez affecter des rôles.  
  
3.  Cliquez avec le bouton droit sur le package auquel vous voulez affecter des rôles.  
  
4.  Dans la boîte de dialogue **Rôles de package** , sélectionnez un rôle de lecture dans la liste **Rôle Lecteur** et un rôle d'écriture dans la liste **Rôle Rédacteur** .  
  
5.  Cliquez sur **OK**.

## <a name="create-a-user-defined-role"></a><a name="create"></a> Créer un rôle défini par l'utilisateur
    
### <a name="to-create-a-user-defined-role"></a>Pour créer un rôle défini par l'utilisateur  
  
1.  Ouvrez [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
2.  Cliquez sur **Explorateur d'objets** dans le menu **Affichage** .  
  
3.  Dans la barre d'outils de l'Explorateur d'objets, cliquez sur **Connecter**, puis sur **Moteur de base de données**.  
  
4.  Dans la boîte de dialogue **Se connecter au serveur** , entrez un nom de serveur et sélectionnez un mode d'authentification. Vous pouvez utiliser un point (.), (local) ou **localhost** pour indiquer le serveur local.  
  
5.  Cliquez sur **Connecter**.  
  
6.  Développez Bases de données, Bases de données système, msdb, Sécurité et Rôles.  
  
7.  Dans le nœud Rôles, cliquez avec le bouton droit sur Rôles de base de données, puis cliquez sur **Nouveau rôle de base de données**.  
  
8.  Sur la page Général, entrez un nom, et, si vous le souhaitez, spécifiez un propriétaire et des schémas appartenant à un rôle et ajoutez des membres de rôle.  
  
9. Vous pouvez également cliquer sur **Autorisations** pour configurer les autorisations pour les objets.  
  
10. Vous pouvez également cliquer sur **Propriétés étendues** pour configurer les éventuelles propriétés étendues.  
  
11. Cliquez sur **OK**.

## <a name="package-roles-dialog-box-ui-reference"></a><a name="roles_dialog"></a> Référence de l’IU de la boîte de dialogue Rôles de package
  Utilisez la boîte de dialogue **Rôles de package** , disponible dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], pour spécifier les rôles au niveau de la base de données qui possèdent un accès en lecture au package et les rôles au niveau de la base de données qui possèdent un accès en écriture au package. Les rôles au niveau de la base de données s’appliquent uniquement aux packages stockés dans la base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **msdb**.  
  
 Les rôles répertoriés dans la boîte de dialogue sont les rôles actuels de la base de données système **msdb** . Si aucun rôle n'est sélectionné, les rôles [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] par défaut s'appliquent. Par défaut, le rôle de lecteur inclut **db_ssisadmin**, **db_ssisoperator** et l’utilisateur qui a créé le package. Un utilisateur membre de l'un de ces rôles ou qui a créé les packages peut énumérer, consulter, exporter et exécuter des packages. Par défaut, le rôle de rédacteur inclut **db_ssisadmin** et l’utilisateur qui a créé le package. Un utilisateur membre de ce rôle et l'utilisateur qui a créé les packages peuvent importer, supprimer et modifier des packages.  
  
 La colonne **ownersid** de la table **sysssispackages** contient l'identificateur de sécurité unique de l'utilisateur qui a créé le package.  
  
### <a name="options"></a>Options  
 **Nom du package**  
 Spécifiez le nom du package.  
  
 **Rôle Lecteur**  
 Sélectionnez un rôle dans la liste.  
  
 **Rôle Rédacteur**  
 Sélectionnez un rôle dans la liste.  
