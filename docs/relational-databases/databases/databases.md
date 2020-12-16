---
title: Bases de données | Microsoft Docs
description: En savoir plus sur les schémas de base de données, les tables, les groupes de fichiers, les connexions et les rôles. Découvrez comment vous pouvez avoir recours à l’outil SQL Server Management Studio pour travailler avec des bases de données.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- data warehouse [SQL Server]
- OLTP databases [SQL Server]
- databases [SQL Server], about databases
ms.assetid: 316eea58-81b8-4bf3-a1fc-801946740e94
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: d1550e59cfa9579cf2c7bf38cdbc8c688b447b69
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/14/2020
ms.locfileid: "97478370"
---
# <a name="databases"></a>Bases de données
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , une base de données est constituée d'une collection de tables qui renferment un ensemble spécifique de données structurées. Une table se compose de lignes, également appelées enregistrements ou tuples, et de colonnes, également appelées attributs. Chaque colonne d'une table est conçue pour stocker un certain type d'informations, par exemple, des données, des noms, des valeurs monétaires ou des nombres.  
  
## <a name="basic-information-about-databases"></a>Informations générales sur les bases de données  
 Une ou plusieurs instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peuvent être installées sur un ordinateur. Chaque instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut contenir une ou plusieurs bases de données.  Dans une base de données, il existe un ou plusieurs groupes d'appropriation d'objets appelés schémas. Chaque schéma contient divers objets de base de données tels que des tables, des vues et des procédures stockées. Certains objets, tels que les certificats et les clés asymétriques, sont présents dans une base de données, mais pas dans un schéma. Pour plus d’informations sur la création de tables, consultez [Tables](../../relational-databases/tables/tables.md).  
  
 Les bases de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont stockées dans des fichiers du système de fichiers. Les fichiers peuvent être regroupés en groupes de fichiers. Pour plus d’informations sur les fichiers et groupes de fichiers, consultez [Groupes de fichiers et fichiers de base de données](../../relational-databases/databases/database-files-and-filegroups.md).  
  
 Lorsque les utilisateurs accèdent à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , ils sont identifiés comme connexion. Lorsque les utilisateurs accèdent à une base de données, ils sont identifiés comme utilisateur de base de données. Un utilisateur de base de données peut être basé sur une connexion. Si les bases de données autonomes sont activées, il est possible de créer un utilisateur de base de données qui n'est pas basé sur une connexion. Pour plus d’informations sur les utilisateurs, consultez [CREATE USER &#40;Transact-SQL&#41;](../../t-sql/statements/create-user-transact-sql.md).  
  
 Un utilisateur qui a accès à une base de données peut recevoir l'autorisation d'accéder aux objets de la base de données. Même si les autorisations peuvent être accordées aux utilisateurs de façon individuelle, nous vous recommandons de créer des rôles de base de données, d'ajouter les utilisateurs de la base de données aux rôles, puis d'accorder l'autorisation d'accès aux rôles. L'octroi d'autorisations à des rôles plutôt qu'aux utilisateurs contribue à garantir que les autorisations restent cohérentes et compréhensibles à mesure que le nombre d'utilisateurs augmente et évolue. Pour plus d’informations sur les autorisations des rôles, consultez [CREATE ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-role-transact-sql.md) et [Principaux &#40;moteur de base de données&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md).  
  
## <a name="working-with-databases"></a>Utilisation des bases de données  
 La plupart des personnes qui utilisent des bases de données emploient l'outil [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] . L'outil [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] comporte une interface utilisateur graphique permettant de créer des bases de données et les objets à y inclure. [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] fournit également un éditeur de requête permettant de rédiger des instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] pour interagir avec les bases de données. [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] peut être installé à partir du disque d'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ou téléchargé à partir de MSDN. Pour plus d’informations sur l’outil [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], consultez la page [SQL Server Management Studio (SSMS)](../../ssms/sql-server-management-studio-ssms.md).
  
## <a name="in-this-section"></a>Dans cette section  

:::row:::
    :::column:::
        [Bases de données système](../../relational-databases/databases/system-databases.md)  
        [Bases de données autonomes](../../relational-databases/databases/contained-databases.md)  
        [Fichiers de données SQL Server dans Microsoft Azure](../../relational-databases/databases/sql-server-data-files-in-microsoft-azure.md)  
        [Groupes de fichiers et fichiers de base de données](../../relational-databases/databases/database-files-and-filegroups.md)  
        [États d’une base de données](../../relational-databases/databases/database-states.md)  
        [États des fichiers](../../relational-databases/databases/file-states.md)  
        [Estimer la taille d'une base de données](../../relational-databases/databases/estimate-the-size-of-a-database.md)  
        [Copier des bases de données sur d’autres serveurs](../../relational-databases/databases/copy-databases-to-other-servers.md)  
        [Attacher et détacher une base de données &#40;SQL Server&#41;](../../relational-databases/databases/database-detach-and-attach-sql-server.md)  
        [Ajouter des fichiers de données ou journaux à une base de données](../../relational-databases/databases/add-data-or-log-files-to-a-database.md)  
        [Modifier les paramètres de configuration d’une base de données](../../relational-databases/databases/change-the-configuration-settings-for-a-database.md)  
        [Créer une base de données](../../relational-databases/databases/create-a-database.md)  
        [Supprimer une base de données](../../relational-databases/databases/delete-a-database.md)  
    :::column-end:::
    :::column:::
        [Supprimer des fichiers de données ou des fichiers journaux d’une base de données](../../relational-databases/databases/delete-data-or-log-files-from-a-database.md)  
        [Afficher les informations sur l’espace occupé par les données et par le journal d’une base de données](../../relational-databases/databases/display-data-and-log-space-information-for-a-database.md)  
        [Augmenter la taille d’une base de données](../../relational-databases/databases/increase-the-size-of-a-database.md)  
        [Renommer une base de données](../../relational-databases/databases/rename-a-database.md)  
        [Définir une base de données en mode mono-utilisateur](../../relational-databases/databases/set-a-database-to-single-user-mode.md)  
        [Réduire une base de données](../../relational-databases/databases/shrink-a-database.md)  
        [Réduire un fichier](../../relational-databases/databases/shrink-a-file.md)  
        [Afficher ou modifier les propriétés d’une base de données](../../relational-databases/databases/view-or-change-the-properties-of-a-database.md)  
        [Afficher une liste des bases de données sur une instance de SQL Server](../../relational-databases/databases/view-a-list-of-databases-on-an-instance-of-sql-server.md)  
        [Afficher ou modifier le niveau de compatibilité d’une base de données](../../relational-databases/databases/view-or-change-the-compatibility-level-of-a-database.md)  
        [Utiliser l'Assistant Plan de maintenance](../../relational-databases/maintenance-plans/use-the-maintenance-plan-wizard.md)  
        [Créer un alias de type de données défini par l’utilisateur](../../relational-databases/databases/create-a-user-defined-data-type-alias.md)  
        [Instantanés de base de données &#40;SQL Server&#41;](../../relational-databases/databases/database-snapshots-sql-server.md)  
    :::column-end:::
:::row-end:::

## <a name="related-content"></a>Contenu associé  
 [Index](../../relational-databases/indexes/indexes.md)  
  
 [Views](../../relational-databases/views/views.md)  
  
 [Procédures stockées &#40;moteur de base de données &#41;](../../relational-databases/stored-procedures/stored-procedures-database-engine.md)  
