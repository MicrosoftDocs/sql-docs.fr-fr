---
title: Autorisations requises
description: En savoir plus sur les autorisations dont vous avez besoin pour les différentes tâches de SQL Server Data Tools, telles que la création d’une base de données, l’exécution de tests unitaires et la comparaison de schémas.
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: b27038c4-94ab-449c-90b7-29d87ce37a8b
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.openlocfilehash: 6e519a3786b45d87344f8f5f5355353fd5865d62
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100072453"
---
# <a name="required-permissions-for-sql-server-data-tools"></a>Autorisations nécessaires pour SQL Server Data Tools

Avant d'exécuter une action sur une base de données dans Visual Studio, vous devez ouvrir une session avec un compte disposant de certaines autorisations sur cette base de données. Les autorisations nécessaires varient selon l'action à exécuter. Les sections suivantes décrivent chaque action que vous pouvez éventuellement effectuer et les autorisations spécifiques nécessaires à cet effet.  
  
-   [Autorisations nécessaires pour créer ou déployer une base de données](#DatabaseCreationAndDeploymentPermissions)  
  
-   [Autorisations nécessaires pour refactoriser une base de données](#DatabaseRefactoringPermissions)  
  
-   [Autorisations nécessaires pour exécuter des tests unitaires sur une base de données SQL Server](#DatabaseUnitTestingPermissions)  
  
-   [Autorisations nécessaires pour générer des données](#DataGenerationPermissions)  
  
-   [Autorisations nécessaires pour comparer les schémas et les données](#SchemaAndDataComparePermissions)  
  
-   [Autorisations nécessaires pour exécuter l’éditeur Transact-SQL](#Transact-SQLEditorPermissions)  
  
-   [Autorisations nécessaires pour les projets SQL Server Common Language Runtime (SQL CLR)](#SQLCLRPermissions)  
  
## <a name="permissions-to-create-or-deploy-a-database"></a><a name="DatabaseCreationAndDeploymentPermissions"></a>Autorisations nécessaires pour créer ou déployer une base de données  
Vous devez disposer des autorisations suivantes pour créer ou déployer une base de données.  
  
|Actions|Autorisations requises|  
|-|-|  
|Importer les objets et paramètres de base de données|Vous devez pouvoir vous connecter à la base de données source.<br /><br />Si la base de données source est basée sur SQL Server 2005, vous devez également être propriétaire ou disposer de l’autorisation **VIEW DEFINITION** sur chaque objet.<br /><br />Si la base de données source est basée sur SQL Server 2008 ou sur une version ultérieure, vous devez également être propriétaire ou disposer de l’autorisation **VIEW DEFINITION** sur chaque objet. Votre connexion doit disposer de l’autorisation **VIEW SERVER STATE** (pour les clés de chiffrement de la base de données).|  
|Importer les objets et paramètres de serveur|Vous devez pouvoir vous connecter à la base de données master sur le serveur spécifié.<br /><br />Si le serveur exécute SQL Server 2005, vous devez disposer de l’autorisation **VIEW ANY DEFINITION** sur le serveur.<br /><br />Si la base de données source est basée sur SQL 2008 ou version ultérieure, vous devez disposer de l’autorisation **VIEW ANY DEFINITION** sur le serveur. Votre connexion doit disposer de l’autorisation **VIEW SERVER STATE** (pour les clés de chiffrement de la base de données).|  
|Créer ou mettre à jour un projet de base de données|Aucune autorisation n'est nécessaire pour créer ou modifier un projet de base de données.|  
|Déployer une nouvelle base de données ou déployer avec l’option **Toujours recréer la base de données** définie|Vous devez disposer de l’autorisation **CREATE DATABASE** ou être membre du rôle **dbcreator** sur le serveur cible.<br /><br />Lorsque vous créez une base de données, Visual Studio se connecte à la base de données model et copie son contenu. La connexion d’origine (par exemple, *yourLogin*) que vous utilisez pour vous connecter à la base de données cible doit disposer des autorisations **db_creator** et **CONNECT SQL**. Cette connexion doit avoir un mappage d'utilisateur sur la base de données model. Si vous disposez des autorisations **sysadmin**, vous pouvez créer le mappage en émettant les instructions Transact\-SQL suivantes :<br /><br />`USE [model] CREATE USER yourUser FROM LOGIN yourLogin`<br /><br />L’utilisateur (dans cet exemple, yourUser) doit disposer des autorisations **CONNECT** et **VIEW DEFINITION** sur la base de données model. Si vous disposez des autorisations **sysadmin**, vous pouvez octroyer ces autorisations en émettant les instructions Transact\-SQL suivantes :<br /><br />`USE [model] GRANT CONNECT to yourUser GRANT VIEW DEFINITION TO yourUser`<br /><br />Si vous déployez une base de données qui contient des contraintes sans nom alors que l’option **CheckNewContraints** est activée (par défaut), vous devez disposer des autorisations **db_owner** ou **sysadmin** pour pouvoir effectuer le déploiement. Cela ne concerne que les contraintes sans nom. Pour plus d’informations sur l’option **CheckNewConstraints**, consultez [Paramètres du projet de base de données](../ssdt/database-project-settings.md).|  
|Déployer des mises à jour dans une base de données existante|Vous devez être un utilisateur de base de données valide. Vous devez également être membre du rôle **db_ddladmin**, propriétaire du schéma ou propriétaire des objets que vous souhaitez créer ou modifier dans la base de données cible. Vous devez disposer d'autorisations supplémentaires pour utiliser des concepts plus avancés tels que les connexions ou les serveurs liés dans les scripts de prédéploiement ou de post-déploiement.<br /><br />**REMARQUE** : Si vous effectuez un déploiement sur la base de données master, vous devez également disposer de l’autorisation **VIEW ANY DEFINITION** sur le serveur sur lequel vous effectuez le déploiement.|  
|Utiliser un assembly avec l'option EXTERNAL_ACCESS dans un projet de base de données|Vous devez définir la propriété TRUSTWORTHY pour votre projet de base de données. Vous devez disposer de l'autorisation EXTERNAL ACCESS ASSEMBLY pour votre connexion SQL Server.|  
|Déployer des assemblys dans une base de données nouvelle ou existante|Vous devez être membre du rôle sysadmin sur le serveur de déploiement cible.|  
  
Pour plus d'informations, consultez la documentation en ligne de SQL Server.  
  
## <a name="permissions-to-refactor-a-database"></a><a name="DatabaseRefactoringPermissions"></a>Autorisations nécessaires pour refactoriser une base de données  
La *refactorisation de base de données* se produit uniquement dans le projet de base de données. Vous devez disposer des autorisations nécessaires pour utiliser le projet de base de données. Aucune autorisation n'est nécessaire sur une base de données cible jusqu'à ce que vous y déployiez des modifications.  
  
## <a name="permissions-to-perform-unit-testing-on-a-sql-server-database"></a><a name="DatabaseUnitTestingPermissions"></a>Autorisations nécessaires pour exécuter des tests unitaires sur une base de données SQL Server  
Vous devez disposer des autorisations suivantes pour exécuter des tests unitaires sur une base de données.  
  
|Actions|Autorisations requises|  
|-|-|   
|Exécuter une action de test|Vous devez utiliser la connexion de base de données dans le contexte d'exécution. Pour plus d’informations, consultez [Vue d’ensemble des chaînes de connexion et des autorisations](../ssdt/overview-of-connection-strings-and-permissions.md).|  
|Exécuter une action de prétest ou de post-test|Vous devez utiliser la connexion de base de données dans le contexte privilégié. Cette connexion de base de données dispose de davantage d'autorisations par rapport à celles du contexte d'exécution.|  
|Exécuter les scripts TestInitialize et TestCleanup|Vous devez utiliser la connexion de base de données dans le contexte privilégié.|  
|Déployer les modifications de la base de données avant d'exécuter des tests|Vous devez utiliser la connexion de base de données dans le contexte privilégié. Pour plus d’informations, consultez [Procédure : configurer l’exécution de test unitaire SQL Server](../ssdt/how-to-configure-sql-server-unit-test-execution.md).|  
|Générer des données avant d'exécuter des tests|Vous devez utiliser la connexion de base de données dans le contexte privilégié. Pour plus d’informations, consultez [Procédure : configurer l’exécution de test unitaire SQL Server](../ssdt/how-to-configure-sql-server-unit-test-execution.md).|  
  
## <a name="permissions-to-generate-data"></a><a name="DataGenerationPermissions"></a>Autorisations nécessaires pour générer des données  
Vous devez disposer des autorisations **INSERT** et **SELECT** sur les objets de la base de données cible pour générer les données de test en utilisant le générateur de données. Si vous supprimez des données avant de générer des données, vous devez également disposer des autorisations **DELETE** sur les objets de la base de données cible. Pour réinitialiser la colonne **IDENTITY** dans une table, vous devez être propriétaire de la table ou être membre du rôle db_owner ou db_ddladmin.  
  
## <a name="permissions-to-compare-schemas-and-data"></a><a name="SchemaAndDataComparePermissions"></a>Autorisations nécessaires pour comparer les schémas et les données  
Vous devez disposer des autorisations suivantes pour comparer des schémas ou des données.  
  
|Actions|Autorisations requises|  
|-|-|   
|Comparer les schémas de deux bases de données|Vous devez disposer des autorisations nécessaires pour importer des objets et des paramètres des bases de données, tel que le décrit la section [Autorisations nécessaires pour créer ou déployer une base de données](#DatabaseCreationAndDeploymentPermissions).|  
|Comparer les schémas d'une base de données et d'un projet de base de données|Vous devez disposer des autorisations nécessaires pour importer des objets et des paramètres de la base de données, tel que le décrit la section [Autorisations nécessaires pour créer ou déployer une base de données](#DatabaseCreationAndDeploymentPermissions). Le projet de base de données doit également être ouvert dans Visual Studio.|  
|Écrire des mises à jour dans une base de données cible|Vous devez disposer des autorisations nécessaires pour déployer des mises à jour dans la base de données cible, tel que le décrit la section [Autorisations nécessaires pour créer ou déployer une base de données](#DatabaseCreationAndDeploymentPermissions).|  
|Comparer les données de deux bases de données|Outre les autorisations nécessaires pour comparer les schémas de deux bases de données, vous devez également disposer de l’autorisation **SELECT** sur toutes les tables à comparer, ainsi que de l’autorisation **VIEW DATABASE STATE**.|  
  
Pour plus d'informations, consultez la documentation en ligne de SQL Server.  
  
## <a name="permissions-to-run-the-transact-sql-editor"></a><a name="Transact-SQLEditorPermissions"></a>Autorisations nécessaires pour exécuter l’éditeur Transact\-SQL  
Les opérations réalisables dans l'éditeur Transact\-SQL sont déterminées par le contexte d'exécution dans la base de données cible.  
  
## <a name="permissions-for-sql-server-common-language-run-time-projects"></a><a name="SQLCLRPermissions"></a>Autorisations nécessaires pour les projets SQL Server Common Language Runtime  
Le tableau suivant répertorie les autorisations nécessaires pour déployer ou déboguer des projets CLR :  
  
|Actions|Autorisations requises|  
|-----------|------------------------|  
|Déploiement (initial ou incrémentiel) d'un assembly de jeu d'autorisations SAFE|db_DDLAdmin : cette autorisation octroie les autorisations CREATE et ALTER pour les types d'assemblys et d'objets déployés.<br /><br />VIEW DEFINITION au niveau de la base de données : nécessaire pour le déploiement.<br /><br />CONNECT au niveau de la base de données : octroie la capacité de se connecter à la base de données.|  
|Déploiement d'un assembly de jeu d'autorisations external_access|db_DDLAdmin : cette autorisation octroie les autorisations CREATE et ALTER pour les types d'assemblys et d'objets déployés.<br /><br />VIEW DEFINITION au niveau de la base de données : nécessaire pour le déploiement.<br /><br />CONNECT au niveau de la base de données : octroie la capacité de se connecter à la base de données.<br /><br />En outre, vous devez également veillez à ce que :<br /><br />l'option de base de données TRUSTWORTHY soit définie sur ON (activée) ;<br /><br />la connexion utilisée pour le déploiement dispose de l'autorisation serveur External Access Assembly.|  
|Déploiement d'un assembly de jeu d'autorisations UNSAFE|db_DDLAdmin : cette autorisation octroie les autorisations CREATE et ALTER pour les types d'assemblys et d'objets déployés.<br /><br />VIEW DEFINITION au niveau de la base de données : nécessaire pour le déploiement.<br /><br />CONNECT au niveau de la base de données : octroie la capacité de se connecter à la base de données.<br /><br />En outre, vous devez également veillez à ce que :<br /><br />l'option de base de données TRUSTWORTHY soit définie sur ON (activée) ;<br /><br />la connexion utilisée pour le déploiement dispose de l'autorisation serveur Unsafe Assembly.|  
|Débogage distant d'un assembly SQL CLR|Vous devez disposer de l'autorisation de rôle fixe sysadmin.|  
  
> [!IMPORTANT]  
> Dans tous les cas, le propriétaire de l'assembly doit être l'utilisateur utilisé pour déployer l'assembly ou le propriétaire doit être un rôle dont l'utilisateur est membre. Cette condition s'applique à tous les assemblys référencés par l'assembly déployé.  
  
## <a name="see-also"></a>Voir aussi  
[Création et définition de tests unitaires SQL Server](../ssdt/creating-and-defining-sql-server-unit-tests.md)  
[SQL Server Data Tools](../ssdt/sql-server-data-tools.md)  
  
