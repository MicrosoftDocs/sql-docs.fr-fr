---
title: Répliquer les modifications de schéma | Microsoft Docs
description: Découvrez comment répliquer des changements de schéma dans SQL Server à l’aide de SQL Server Management Studio ou de Transact-SQL.
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- replication [SQL Server], schema changes
- schemas [SQL Server replication], replicating changes
ms.assetid: c09007f0-9374-4f60-956b-8a87670cd043
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: f552791a089aec66975d4cb950176ed2219a727d
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076697"
---
# <a name="replicate-schema-changes"></a>Répliquer les modifications de schéma
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  Cette rubrique explique comment répliquer les modification de schéma dans [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../../includes/tsql-md.md)].  
  
 Si vous apportez les modifications de schéma suivantes à un article publié, elles sont propagées, par défaut, aux Abonnés [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] :  
  
-   ALTER TABLE  
  
-   ALTER VIEW  
  
-   ALTER PROCEDURE  
  
-   ALTER FUNCTION  
  
-   ALTER TRIGGER  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Limitations et restrictions](#Restrictions)  
  
-   **Pour répliquer les modifications de schéma à l'aide de :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitations et restrictions  
  
-   L’instruction ALTER TABLE ... DROP COLUMN est toujours répliquée vers tous les Abonnés dont l’abonnement contient les colonnes à supprimer, même si vous désactivez la réplication des modifications de schéma.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
 Si vous ne voulez pas répliquer des modifications de schéma pour une publication, désactivez la réplication des modifications de schéma dans la boîte de dialogue **Propriétés de la publication - \<Publication>** . Pour plus d'informations sur l'accès à cette boîte de dialogue, consultez [Afficher et modifier les propriétés d’un serveur de publication](../../../relational-databases/replication/publish/view-and-modify-publication-properties.md).  
  
#### <a name="to-disable-replication-of-schema-changes"></a>Pour désactiver la réplication des modifications de schéma  
  
1.  Dans la page **Options d’abonnement** de la boîte de dialogue **Propriétés de la publication - \<Publication>** , définissez la propriété **Répliquer les modifications de schéma** sur **False**.  
  
2.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  

     Pour propager seulement des modifications de schéma spécifiques, définissez la propriété à **True** avant une modification de schéma, puis définissez-la à **False** après avoir effectué la modification. Inversement, pour propager la plupart des modifications de schéma mais pas une modification spécifique, définissez la propriété à **False** avant la modification de schéma, puis définissez-la à **True** après avoir effectué la modification.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
 Vous pouvez utiliser les procédures stockées de réplication pour spécifier si ces modifications de schéma sont répliquées. La procédure stockée que vous utilisez dépend du type de publication.  
  
#### <a name="to-create-a-snapshot-or-transactional-publication-that-does-not-replicate-schema-changes"></a>Pour créer une publication transactionnelle ou d'instantané qui ne réplique pas les modifications du schéma  
  
1.  Dans la base de données de publication sur le serveur de publication, exécutez [sp_addpublication &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addpublication-transact-sql.md), en spécifiant la valeur `0` pour `@replicate_ddl`. Pour plus d’informations, voir [Create a Publication](../../../relational-databases/replication/publish/create-a-publication.md).  
  
#### <a name="to-create-a-merge-publication-that-does-not-replicate-schema-changes"></a>Pour créer une publication de fusion qui ne réplique pas les modifications du schéma  
  
1.  Dans la base de données de publication sur le serveur de publication, exécutez [sp_addmergepublication &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addmergepublication-transact-sql.md), en spécifiant la valeur `0` pour `@replicate_ddl`. Pour plus d’informations, voir [Create a Publication](../../../relational-databases/replication/publish/create-a-publication.md).  
  
#### <a name="to-temporarily-disable-replicating-schema-changes-for-a-snapshot-or-transactional-publication"></a>Pour désactiver temporairement la réplication des modifications du schéma pour une publication transactionnelle ou d'instantané  
  
1.  Pour une publication avec réplication des modifications du schéma, exécutez [sp_changepublication &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-changepublication-transact-sql.md), en spécifiant la valeur `replicate_ddl` pour `@property` et la valeur `0` pour `@value`.  
  
2.  Exécutez la commande DDL sur l'objet publié.  
  
3.  (Facultatif) Réactivez la réplication des modifications du schéma en exécutant [sp_changepublication &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-changepublication-transact-sql.md), en spécifiant la valeur `replicate_ddl` pour `@property` et la valeur `1` pour `@value`.  
  
#### <a name="to-temporarily-disable-replicating-schema-changes-for-a-merge-publication"></a>Pour désactiver temporairement la réplication des modifications du schéma pour une publication de fusion  
  
1.  Pour une publication avec réplication des modifications du schéma, exécutez [sp_changemergepublication &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-changemergepublication-transact-sql.md), en spécifiant la valeur `replicate_ddl` pour `@property` et la valeur `0` pour `@value`.  
  
2.  Exécutez la commande DDL sur l'objet publié.  
  
3.  (Facultatif) Réactivez la réplication des modifications du schéma en exécutant [sp_changemergepublication &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-changemergepublication-transact-sql.md), en spécifiant la valeur `replicate_ddl` pour `@property` et la valeur `1` pour `@value`.  
  
## <a name="see-also"></a>Voir aussi  
 [Modifier le schéma dans les bases de données de publication](../../../relational-databases/replication/publish/make-schema-changes-on-publication-databases.md)   
 [Modifier le schéma dans les bases de données de publication](../../../relational-databases/replication/publish/make-schema-changes-on-publication-databases.md)  
  
  
