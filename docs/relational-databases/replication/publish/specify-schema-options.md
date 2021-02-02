---
description: Spécifier les options de schéma pour la réplication SQL Server
title: Spécifier les options de schéma pour la réplication SQL Server | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- schemas [SQL Server replication], options
- articles [SQL Server replication], transactional replication options
- articles [SQL Server replication], merge replication options
- articles [SQL Server replication], schema options
ms.assetid: 1f85a479-bd6e-4023-abf7-7435a7e5b567
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 53c7c3b803a16fd7ee95f35f33ee2439deb86199
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076617"
---
# <a name="specify-schema-options-for-sql-server-replication"></a>Spécifier les options de schéma pour la réplication SQL Server
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  Cette rubrique explique comment spécifier les options de schéma dans [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../../includes/tsql-md.md)]. Lors de la publication d'une table ou d'une vue, vous pouvez contrôler les options de création d'objet qui sont répliquées pour l'objet publié. Vous pouvez définir ces options lorsque l'article est créé, et vous pouvez également les modifier ultérieurement. Si vous ne spécifiez pas explicitement ces options pour un article, un jeu d'options par défaut sera défini.  
  
> [!NOTE]  
>  Les options de schéma par défaut lors de l'utilisation de procédures stockées de réplication peuvent différer des options par défaut lorsque des articles sont ajoutés à l'aide de [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)].  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Limitations et restrictions](#Restrictions)  
  
     [Recommandations](#Recommendations)  
  
-   **Pour spécifier des options de schéma à l'aide de :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitations et restrictions  
  
-   Si vous modifiez des options de schéma après la création d'une publication, vous devez générer un nouvel instantané.  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Recommandations  
  
-   Pour obtenir la liste complète des options de schéma, consultez le paramètre `@schema_option` de [sp_addarticle &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md) et [sp_addmergearticle &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md).  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
 Spécifiez les options de schéma, comme la copie des contraintes et des déclencheurs sur les abonnés, sous l’onglet **Propriétés** de la boîte de dialogue **Propriétés de l’article - \<Article>** . Cet onglet est disponible dans l’Assistant Nouvelle publication et dans la boîte de dialogue **Propriétés de la publication - \<Publication>** . Pour plus d’informations sur l’utilisation de l’Assistant et sur l’accès à la boîte de dialogue, consultez [Créer une publication](../../../relational-databases/replication/publish/create-a-publication.md) et [Afficher et modifier les propriétés d’une publication](../../../relational-databases/replication/publish/view-and-modify-publication-properties.md).  
  
#### <a name="to-specify-schema-options"></a>Pour spécifier des options de schéma  
  
1.  Dans la page **Articles** de l’Assistant Nouvelle publication ou la boîte de dialogue **Propriétés de la publication - \<Publication>** , sélectionnez un article, puis cliquez sur **Propriétés de l’article**.  
  
2.  Sélectionnez les articles auxquels les modifications de l'option de schéma doivent s'appliquer :  
  
    -   Cliquez sur **Définir les propriétés de l’article de \<ObjectType> en surbrillance** pour ouvrir la boîte de dialogue **Propriétés de l’article - \<ObjectName>** . Les modifications de propriétés apportées dans cette boîte de dialogue sont appliquées uniquement à l’objet en surbrillance dans le volet Objet de la page **Articles**.  
  
    -   Cliquez sur **Définir les propriétés de tous les articles de \<ObjectType>** pour ouvrir la boîte de dialogue **Propriétés pour tous les articles de \<ObjectType>** . Les modifications de propriétés apportées dans cette boîte de dialogue sont appliquées à tous les objets de ce type dans le volet Objet de la page **Articles**, notamment ceux qui ne sont pas encore sélectionnés pour publication.  
  
        > [!NOTE]  
        >  Les modifications de propriétés apportées dans la boîte de dialogue **Propriétés pour tous les \<ObjectType> articles** remplacent celles apportées dans la boîte de dialogue **Propriétés de l’article - \<ObjectName>** . Ainsi, si vous voulez définir un certain nombre de valeurs par défaut applicables à tous les articles d'un type d'objet mais souhaitez également définir des propriétés pour des objets spécifiques, vous devez d'abord définir les valeurs par défaut pour tous les articles. Définissez ensuite les propriétés des objets individuels.  
  
3.  Dans les sections **Copier les objets et les paramètres sur l’Abonné** et **Objet de destination** de l’onglet **Propriétés** de la boîte de dialogue **Propriétés de l’article - \<Article>** , spécifiez des valeurs pour les options.  
  
4.  Modifiez les propriétés si nécessaire, puis cliquez sur **OK**.  
  
5.  Si vous êtes dans la boîte de dialogue **Propriétés de la publication - \<Publication>** , cliquez sur **OK** pour enregistrer et fermer la boîte de dialogue.  

##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
 Les options de schéma sont spécifiées sous la forme d'une valeur hexadécimale qui correspond au résultat [| (opération de bits OR)](../../../t-sql/language-elements/bitwise-or-transact-sql.md) d'une ou de plusieurs options. Pour plus d'informations, consultez [sp_addarticle](../../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md) et [sp_addmergearticle](../../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md).  
  
> [!NOTE]  
>  Vous devez convertir les valeurs des options de schéma du format **binaire** au format **int** avant d'effectuer une opération au niveau du bit. Pour plus d’informations, consultez [CAST et CONVERT &#40;Transact-SQL&#41;](../../../t-sql/functions/cast-and-convert-transact-sql.md).  
  
#### <a name="to-specify-schema-options-when-defining-an-article-for-a-snapshot-or-transactional-publication"></a>Pour spécifier des options de schéma lors de la définition d'un article pour une publication transactionnelle ou d'instantané  
  
1.  Exécutez [sp_addarticle](../../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md)sur la base de données de publication du serveur de publication. Spécifiez le nom de la publication à laquelle l’article appartient pour `@publication`, le nom de l’article pour `@article`, l’objet de base de données qui est publié pour `@source_object`, le type d’objet de base de données pour `@type` et le résultat [| (opération de bits OR)](../../../t-sql/language-elements/bitwise-or-transact-sql.md) d’une ou plusieurs options de schéma pour `@schema_option`. Pour plus d’informations, consultez [définir un Article](../../../relational-databases/replication/publish/define-an-article.md).  
  
#### <a name="to-specify-schema-options-when-defining-an-article-for-a-merge-publication"></a>Pour spécifier des options de schéma lors de la définition d'un article pour une publication de fusion  
  
1.  Dans la base de données de publication sur le serveur de publication, exécutez [sp_addmergearticle](../../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md). Spécifiez le nom de la publication à laquelle l’article appartient pour `@publication`, le nom de l’article pour `@article`, l’objet de base de données qui est publié pour `@source_object` et le résultat [| (opération de bits OR)](../../../t-sql/language-elements/bitwise-or-transact-sql.md) d’une ou plusieurs options de schéma pour `@schema_option`. Pour plus d’informations, consultez [définir un Article](../../../relational-databases/replication/publish/define-an-article.md).  
  
#### <a name="to-change-schema-options-for-an-existing-article-in-a-snapshot-or-transactional-publication"></a>Pour modifier des options de schéma pour un article existant dans une publication transactionnelle ou d'instantané  
  
1.  Exécutez [sp_helparticle](../../../relational-databases/system-stored-procedures/sp-helparticle-transact-sql.md)dans la base de données de publication du serveur de publication. Spécifiez le nom de la publication à laquelle l’article appartient pour `@publication` et le nom de l’article pour `@article`. Notez la valeur de la colonne `schema_option` dans le jeu de résultats.  
  
2.  Exécutez une opération [& (Bitwise AND)](../../../t-sql/language-elements/bitwise-and-transact-sql.md) en utilisant la valeur de l’étape 1 et la valeur de l’option de schéma de votre choix pour déterminer si cette option est définie.  
  
    -   Si le résultat est **0**, l'option n'est pas définie.  
  
    -   Si le résultat correspond à la valeur de l'option, l'option est déjà définie.  
  
3.  Si l'option n'est pas définie, exécutez une opération [| (opération de bits OR)](../../../t-sql/language-elements/bitwise-or-transact-sql.md) en utilisant la valeur de l'étape 1 et la valeur de l'option de schéma de votre choix.  
  
4.  Exécutez [sp_changearticle](../../../relational-databases/system-stored-procedures/sp-changearticle-transact-sql.md)dans la base de données de publication du serveur de publication. Spécifiez le nom de la publication à laquelle l’article appartient pour `@publication`, le nom de l’article pour `@article`, la valeur `schema_option` pour `@property` et le résultat hexadécimal obtenu à l’étape 3 pour `@value`.  
  
5.  Exécutez l'Agent d'instantané afin de générer un nouvel instantané. Pour plus d’informations, voir [Create and Apply the Initial Snapshot](../../../relational-databases/replication/create-and-apply-the-initial-snapshot.md).  
  
#### <a name="to-change-schema-options-for-an-existing-article-in-a-merge-publication"></a>Pour modifier des options de schéma pour un article existant dans une publication de fusion  
  
1.  Exécutez [sp_helpmergearticle](../../../relational-databases/system-stored-procedures/sp-helpmergearticle-transact-sql.md)dans la base de données de publication du serveur de publication. Spécifiez le nom de la publication à laquelle l’article appartient pour `@publication` et le nom de l’article pour `@article`. Notez la valeur de la colonne **schema_option** dans le jeu de résultats.  
  
2.  Exécutez une opération [& (Bitwise AND)](../../../t-sql/language-elements/bitwise-and-transact-sql.md) en utilisant la valeur de l’étape 1 et la valeur de l’option de schéma de votre choix pour déterminer si cette option est définie.  
  
    -   Si le résultat est **0**, l'option n'est pas définie.  
  
    -   Si le résultat correspond à la valeur de l'option, l'option est déjà définie.  
  
3.  Si l'option n'est pas définie, exécutez une opération [| (opération de bits OR)](../../../t-sql/language-elements/bitwise-or-transact-sql.md) en utilisant la valeur de l'étape 1 et la valeur de l'option de schéma de votre choix.  
  
4.  Dans la base de données de publication sur le serveur de publication, exécutez [sp_changemergearticle](../../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md). Spécifiez le nom de la publication à laquelle l’article appartient pour `@publication`, le nom de l’article pour `@article`, la valeur `schema_option` pour `@property` et le résultat hexadécimal obtenu à l’étape 3 pour `@value`.  
  
5.  Exécutez l'Agent d'instantané afin de générer un nouvel instantané. Pour plus d’informations, voir [Create and Apply the Initial Snapshot](../../../relational-databases/replication/create-and-apply-the-initial-snapshot.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Publier des données et des objets de base de données](../../../relational-databases/replication/publish/publish-data-and-database-objects.md)   
 [Article Options for Transactional Replication](../../../relational-databases/replication/transactional/article-options-for-transactional-replication.md)  
  
  
