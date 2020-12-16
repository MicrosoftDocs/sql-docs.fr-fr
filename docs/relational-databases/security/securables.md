---
title: Éléments sécurisables | Microsoft Docs
description: Découvrez les étendues sécurisables, que le système d'autorisation du moteur de base de données SQL Server utilise pour réguler l'accès aux sécurisables.
ms.custom: ''
ms.date: 10/18/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
f1_keywords:
- sql13.swb.roleproperties.selectobject.f1
helpviewer_keywords:
- securables [SQL Server]
- schemas [SQL Server], securables
- database securables [SQL Server]
- hierarchies [SQL Server], securables
- server securables [SQL Server]
ms.assetid: bfa748f0-70b0-453c-870a-04b7b205b9ff
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 8d780aa878779bf25a3854dbd0c7c392800f7039
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/14/2020
ms.locfileid: "97432216"
---
# <a name="securables"></a>Éléments sécurisables
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Les éléments sécurisables sont les ressources auxquelles le système d'autorisation du moteur de base de données [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] régule l'accès. Par exemple, une table est un élément sécurisable. Certains éléments sécurisables peuvent être contenus dans d'autres, de façon à créer des hiérarchies imbriquées appelées « étendues », pouvant elles-mêmes être sécurisées. L'étendue de ces éléments sécurisables est constituée par le **serveur**, la **base de données** et le **schéma**.  
  
## <a name="securable-scope-server"></a>Étendue sécurisable : Serveur  
 Les éléments sécurisables du **serveur** sont les suivants :  
  
-   Groupe de disponibilité  
  
-   Point de terminaison  
  
-   Connexion  
  
-   Rôle du serveur  
  
-   Base de données  
  
## <a name="securable-scope-database"></a>Étendue sécurisable : Base de données  
 Les éléments sécurisables de la **base de données** sont les suivants :  
  
-   Rôle d'application  
  
-   Assembly  
  
-   Clé asymétrique  
  
-   Certificat  
  
-   Contrat  
  
-   Catalogue FullText  
  
-   Liste de mots vides  
  
-   type de message  
  
-   Liaisons de service distant  
  
-   Rôle (de base de données)  
  
-   Routage  
  
-   schéma  
  
-   Liste des propriétés de recherche  
  
-   Service  
  
-   Clé symétrique  
  
-   Utilisateur  
  
## <a name="securable-scope-schema"></a>Étendue sécurisable : schéma  
 Les éléments sécurisables du **schéma** sont les suivants :  
  
-   Type  
  
-   Collection de schémas XML  
  
-   Objet - La classe d’objet comprend les membres suivants :  
  
    -   Agrégat  
  
    -   Fonction  
  
    -   Procédure  
  
    -   File d'attente  
  
    -   Synonyme  
  
    -   Table de charge de travail  
  
    -   Affichage 
    
    -   Table externe 
  
## <a name="controlling-access-to-a-securable"></a>Contrôle de l'accès à un élément sécurisable  
 L'entité qui reçoit l'autorisation d'accéder à un élément sécurisable est appelée « principal ». Les principaux les plus courants sont les connexions et les utilisateurs de base de données. Vous pouvez contrôler l’accès aux éléments sécurisables en accordant ou en refusant des autorisations, ou en ajoutant des connexions et des utilisateurs aux rôles auxquels ils ont accès. Pour plus d’informations sur le contrôle des autorisations, consultez [GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-transact-sql.md), [REVOKE &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-transact-sql.md), [DENY &#40;Transact-SQL&#41;](../../t-sql/statements/deny-transact-sql.md), [sp_addrolemember &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addrolemember-transact-sql.md) et [sp_droprolemember &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-droprolemember-transact-sql.md).  
  
> [!CAUTION]  
>  Les autorisations par défaut accordées aux objets système au moment de l'installation sont évaluées avec soin par rapport aux menaces potentielles et ne doivent pas être modifiées dans le cadre du renforcement de l'installation [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Les modifications apportées aux autorisations sur les objets système peuvent limiter ou rompre le fonctionnement et pourraient potentiellement laisser votre installation [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans un état non pris en charge.  
  
## <a name="related-content"></a>Contenu associé  
 [Prise en main des autorisations du moteur de base de données](../../relational-databases/security/authentication-access/getting-started-with-database-engine-permissions.md)  
  
 [Sécurisation de SQL Server](../../relational-databases/security/securing-sql-server.md)  
  
 [sys.database_principals &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md)  
  
 [sys.database_role_members &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-role-members-transact-sql.md)  
  
 [sys.server_principals &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md)  
  
 [sys.server_role_members &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-role-members-transact-sql.md)  
  
 [sys.sql_logins &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-sql-logins-transact-sql.md)  
  
  
