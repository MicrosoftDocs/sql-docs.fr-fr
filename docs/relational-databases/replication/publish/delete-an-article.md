---
description: Supprimer un article
title: Supprimer un article | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
dev_langs:
- TSQL
helpviewer_keywords:
- articles [SQL Server replication], dropping
- sp_droparticle
- sp_dropmergearticle
- deleting articles
- removing articles
- dropping articles
ms.assetid: 185b58fc-38c0-4abe-822e-6ec20066c863
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 12ec412078e893c8c527f977d35eecebf06f8f93
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076707"
---
# <a name="delete-an-article"></a>Supprimer un article
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  Cette rubrique explique comment supprimer un article dans [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[tsql](../../../includes/tsql-md.md)] ou des objets RMO (Replication Management Objects). Pour plus d’informations sur les conditions dans lesquelles un article peut être supprimé et pour savoir si la suppression d’un article requiert un nouvel instantané ou la réinitialisation des abonnements, consultez [Ajouter et supprimer des articles de publications existantes](../../../relational-databases/replication/publish/add-articles-to-and-drop-articles-from-existing-publications.md).  
  
 **Dans cette rubrique**  
  
-   **Pour supprimer un article à l'aide de :**  
  
     [Transact-SQL](#TsqlProcedure)  
  
     [Objets RMO (Replication Management Objects)](#RMOProcedure)  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
 Les articles peuvent être supprimés par programme en utilisant des procédures stockées de réplication. Les procédures stockées utilisées dépendent du type de publication auquel l'article appartient.  
  
#### <a name="to-delete-an-article-from-a-snapshot-or-transactional-publication"></a>Pour supprimer un article d'une publication transactionnelle ou d'instantané  
  
1.  Exécutez [sp_droparticle &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-droparticle-transact-sql.md) pour supprimer un article spécifié par **\@article** d’une publication spécifiée par **\@publication**. Affectez la valeur **1** à **\@force_invalidate_snapshot**.  
  
2.  (Facultatif) Pour supprimer entièrement l'objet publié de la base de données, exécutez la commande `DROP <objectname>` au niveau du serveur de publication dans la base de données de publication.  

#### <a name="to-delete-an-article-from-a-merge-publication"></a>Pour supprimer un article d'une publication de fusion  
  
1.  Exécutez [sp_dropmergearticle &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-dropmergearticle-transact-sql.md) pour supprimer un article spécifié par **\@article** d’une publication spécifiée par **\@publication**. Si nécessaire, affectez la valeur **1** à **\@force_invalidate_snapshot** et la valeur **1** à **\@force_reinit_subscription**.  
  
2.  (Facultatif) Pour supprimer entièrement l'objet publié de la base de données, exécutez la commande `DROP <objectname>` au niveau du serveur de publication dans la base de données de publication.  
  
###  <a name="examples-transact-sql"></a><a name="TsqlExample"></a> Exemples (Transact-SQL)  
 L'exemple suivant supprime un article d'une publication transactionnelle. Dans la mesure où cette modification invalide l’instantané existant, la valeur **1** est affectée au paramètre **\@force_invalidate_snapshot**.  
  
```  
DECLARE @publication AS sysname;  
DECLARE @article AS sysname;  
SET @publication = N'AdvWorksProductTran';   
SET @article = N'Product';   
  
-- Drop the transactional article.  
USE [AdventureWorks]  
EXEC sp_droparticle   
  @publication = @publication,   
  @article = @article,  
  @force_invalidate_snapshot = 1;  
GO  
```  
  
 L'exemple suivant supprime deux articles d'une publication de fusion. Dans la mesure où ces modifications invalident l’instantané existant, la valeur **1** est affectée au paramètre **\@force_invalidate_snapshot** spécifié.  
  
```  
DECLARE @publication AS sysname;  
DECLARE @article1 AS sysname;  
DECLARE @article2 AS sysname;  
SET @publication = N'AdvWorksSalesOrdersMerge';  
SET @article1 = N'SalesOrderDetail';   
SET @article2 = N'SalesOrderHeader';   
  
-- Remove articles from a merge publication.  
USE [AdventureWorks]  
EXEC sp_dropmergearticle   
  @publication = @publication,   
  @article = @article1,  
  @force_invalidate_snapshot = 1;  
EXEC sp_dropmergearticle   
  @publication = @publication,   
  @article = @article2,  
  @force_invalidate_snapshot = 1;  
GO  
```  
  
##  <a name="using-replication-management-objects-rmo"></a><a name="RMOProcedure"></a> Utilisation d'objets RMO (Replication Management Objects)  
 Vous pouvez supprimer des articles par programme à l'aide des objets RMO (Replication Management Objects). Les classes RMO à utiliser pour supprimer un article dépendent du type de publication auquel l'article appartient.  
  
#### <a name="to-delete-an-article-that-belongs-to-a-snapshot-or-transactional-publication"></a>Pour supprimer un article qui appartient à une publication transactionnelle ou d'instantané.  
  
1.  Créez une connexion au serveur de publication en utilisant la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Créez une instance de la classe <xref:Microsoft.SqlServer.Replication.TransArticle>.  
  
3.  Définissez les propriétés <xref:Microsoft.SqlServer.Replication.Article.Name%2A>, <xref:Microsoft.SqlServer.Replication.Article.PublicationName%2A>et <xref:Microsoft.SqlServer.Replication.Article.DatabaseName%2A> .  
  
4.  Définissez la connexion créée à l'étape 1 pour la propriété <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> .  
  
5.  Vérifiez la propriété <xref:Microsoft.SqlServer.Replication.ReplicationObject.IsExistingObject%2A> pour vérifier que l'article existe. Si la valeur de cette propriété est **false**, soit les propriétés de l'article ont été définies de manière incorrecte à l'étape 3, soit l'article n'existe pas.  
  
6.  Appelez la méthode <xref:Microsoft.SqlServer.Replication.Article.Remove%2A> .  
  
7.  Fermez toutes les connexions.  
  
#### <a name="to-delete-an-article-that-belongs-to-a-merge-publication"></a>Pour supprimer un article qui appartient à une publication de fusion  
  
1.  Créez une connexion au serveur de publication en utilisant la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Créez une instance de la classe <xref:Microsoft.SqlServer.Replication.MergeArticle>.  
  
3.  Définissez les propriétés <xref:Microsoft.SqlServer.Replication.Article.Name%2A>, <xref:Microsoft.SqlServer.Replication.Article.PublicationName%2A>et <xref:Microsoft.SqlServer.Replication.Article.DatabaseName%2A> .  
  
4.  Définissez la connexion créée à l'étape 1 pour la propriété <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> .  
  
5.  Vérifiez la propriété <xref:Microsoft.SqlServer.Replication.ReplicationObject.IsExistingObject%2A> pour vérifier que l'article existe. Si la valeur de cette propriété est **false**, soit les propriétés de l'article ont été définies de manière incorrecte à l'étape 3, soit l'article n'existe pas.  
  
6.  Appelez la méthode <xref:Microsoft.SqlServer.Replication.Article.Remove%2A> .  
  
7.  Fermez toutes les connexions.  
  
## <a name="see-also"></a>Voir aussi  
 [Ajouter et supprimer des articles de publications existantes](../../../relational-databases/replication/publish/add-articles-to-and-drop-articles-from-existing-publications.md)   
 [Replication System Stored Procedures Concepts](../../../relational-databases/replication/concepts/replication-system-stored-procedures-concepts.md)  
  
  
