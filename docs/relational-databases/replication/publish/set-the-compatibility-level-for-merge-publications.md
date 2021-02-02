---
title: Définir le niveau de compatibilité pour les publications de fusion
description: Découvrez comment définir le niveau de compatibilité pour les publications de fusion à l’aide de SQL Server Management Studio (SSMS) ou de Transact-SQL (T-SQL).
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- compatibility [SQL Server], replication
- backward compatibility [SQL Server], replication
- publications [SQL Server replication], backward compatibility
ms.assetid: db47ac73-948b-4d77-b272-bb3565135ea5
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 7dd5765ed93648b0bdd1e6c7108f8c77a47c3143
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076627"
---
# <a name="set-the-compatibility-level-for-merge-publications"></a>Définir le niveau de compatibilité pour les publications de fusion
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Cette rubrique explique comment définir le niveau de compatibilité pour les publications de fusion dans [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../../includes/tsql-md.md)]. La réplication de fusion utilise le niveau de compatibilité de publication pour déterminer les fonctionnalités qui peuvent être utilisées par des publications dans une base de données particulière.  
  
 **Dans cette rubrique**  
  
-   **Pour définir le niveau de compatibilité des publications de fusion à l'aide de :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
 Définir le niveau de compatibilité sur la page **Types d'abonnés** de l'Assistant Nouvelle publication. Pour plus d'informations sur l'accès à cet Assistant, consultez [Create a Publication](../../../relational-databases/replication/publish/create-a-publication.md). Après la création d'un instantané de publication, le niveau de compatibilité peut être augmenté, mais il ne peut pas être diminué. Augmentez le niveau de compatibilité dans la page **Général** de la boîte de dialogue **Propriétés de la publication - \<Publication>** . Pour plus d'informations sur l'accès à cette boîte de dialogue, consultez [Afficher et modifier les propriétés d’un serveur de publication](../../../relational-databases/replication/publish/view-and-modify-publication-properties.md). Si vous augmentez le niveau de compatibilité de la publication, tous les abonnements existants sur les serveurs exécutant des versions antérieures au niveau de compatibilité ne peuvent plus se synchroniser.  
  
> [!NOTE]  
>  Le niveau de compatibilité ayant des implications sur d'autres propriétés de la publication et sur la détermination de la validité des propriétés des articles, ne modifiez pas le niveau de compatibilité ni d'autres propriétés dans la boîte de dialogue. L'instantané pour la publication doit être régénéré après la modification de la propriété.  
  
#### <a name="to-set-the-publication-compatibility-level"></a>Pour définir le niveau de compatibilité de la publication  
  
-   Sur la page **Types d'abonnés** de l'Assistant Nouvelle publication, sélectionnez les types d'abonnés que la publication doit prendre en charge.  
  
#### <a name="to-increase-the-publication-compatibility-level"></a>Pour augmenter le niveau de compatibilité de la publication  
  
-   Dans la page **Général** de la boîte de dialogue **Propriétés de la publication - \<Publication>** , sélectionnez **Niveau de compatibilité**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
 Le niveau de compatibilité d'une publication de fusion peut être défini par programme au moment de la création d'une publication ou être modifié par programme ultérieurement. Vous pouvez utiliser des procédures stockées de réplication pour définir ou modifier cette propriété de publication.  
  
#### <a name="to-set-the-publication-compatibility-level-for-a-merge-publication"></a>Pour définir le niveau de compatibilité d'une publication de fusion  
  
1.  Sur le serveur de publication, exécutez [sp_addmergepublication &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addmergepublication-transact-sql.md), en spécifiant une valeur pour `@publication_compatibility_level` afin de rendre la publication compatible avec des versions antérieures de [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Pour plus d’informations, voir [Create a Publication](../../../relational-databases/replication/publish/create-a-publication.md).  

#### <a name="to-change-the-publication-compatibility-level-of-a-merge-publication"></a>Pour modifier le niveau de compatibilité d'une publication de fusion  
  
1.  Exécutez [sp_changemergepublication &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-changemergepublication-transact-sql.md), en spécifiant **publication_compatibility_level** pour `@property` et le niveau de compatibilité de publication approprié pour `@value`.  
  
#### <a name="to-determine-the-publication-compatibility-level-of-a-merge-publication"></a>Pour déterminer le niveau de compatibilité d'une publication de fusion  
  
1.  Exécutez [sp_helpmergepublication &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-helpmergepublication-transact-sql.md), en spécifiant la publication souhaitée.  
  
2.  Recherchez le niveau de compatibilité de la publication dans la colonne **backward_comp_level** du jeu de résultats.  
  
###  <a name="examples-transact-sql"></a><a name="TsqlExample"></a> Exemples (Transact-SQL)  
 Cet exemple crée une publication de fusion et définit son niveau de compatibilité.  
  
```sql  
-- To avoid storing the login and password in the script file, the values   
-- are passed into SQLCMD as scripting variables. For information about   
-- how to use scripting variables on the command line and in SQL Server  
-- Management Studio, see the "Executing Replication Scripts" section in  
-- the topic "Programming Replication Using System Stored Procedures".  
  
--Add a new merge publication.  
DECLARE @publicationDB AS sysname;  
DECLARE @publication AS sysname;  
DECLARE @login AS sysname;  
DECLARE @password AS sysname;  
SET @publicationDB = N'AdventureWorks2012';   
SET @publication = N'AdvWorksSalesOrdersMerge'   
SET @login = $(Login);  
SET @password = $(Password);  
  
-- Create a new merge publication.   
USE [AdventureWorks2012]  
EXEC sp_addmergepublication   
@publication = @publication,   
-- Set the compatibility level to SQL Server 2014.  
@publication_compatibility_level = '120RTM';   
  
-- Create the snapshot job for the publication.  
EXEC sp_addpublication_snapshot   
@publication = @publication,  
@job_login = @login,  
@job_password = @password;  
GO  
```  
  
 Cet exemple modifie le niveau de compatibilité d'une publication de fusion  
  
> [!NOTE]  
>  La modification du niveau de compatibilité de la publication peut être interdite si la publication utilise une fonctionnalité qui requiert un niveau de compatibilité particulier. Pour plus d’informations, consultez [Compatibilité descendante de la réplication](../../../relational-databases/replication/replication-backward-compatibility.md).  
  
```sql  
DECLARE @publication AS sysname;  
SET @publication = N'AdvWorksSalesOrdersMerge' ;  
  
-- Change the publication compatibility level to   
-- SQL Server 2008 or later.  
EXEC sp_changemergepublication   
@publication = @publication,   
@property = N'publication_compatibility_level',   
@value = N'100RTM';  
GO  
  
```  
  
 Cet exemple retourne le niveau de compatibilité actuel de la publication de fusion  
  
```sql  
DECLARE @publication AS sysname;  
SET @publication = N'AdvWorksSalesOrdersMerge' ;  
EXEC sp_helpmergepublication   
@publication = @publication;  
GO  
  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Créer une publication](../../../relational-databases/replication/publish/create-a-publication.md)  
  
  
