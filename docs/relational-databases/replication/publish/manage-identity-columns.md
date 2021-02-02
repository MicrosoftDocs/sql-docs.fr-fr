---
description: Gérer des colonnes d'identité
title: Gérer des colonnes d’identité | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- identity values [SQL Server replication]
- merge replication [SQL Server replication], identity range management
- publishing [SQL Server replication], identity columns
- transactional replication, identity range management
- identity columns [SQL Server], replication
ms.assetid: 98892836-cf63-494a-bd5d-6577d9810ddf
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: aac4d9dd50a329f4395a6b7aa9efb116b5efc098
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076677"
---
# <a name="manage-identity-columns"></a>Gérer des colonnes d'identité
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  Cette rubrique explique comment gérer des colonnes d'identité dans [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] ou [!INCLUDE[tsql](../../../includes/tsql-md.md)]. Lorsque les insertions de l'Abonné sont répliquées sur le serveur de publication, les colonnes d'identité doivent être gérées afin d'éviter d'affecter la même valeur d'identité sur l'Abonné et sur le serveur de publication. La réplication peut gérer automatiquement les plages d'identité ou vous pouvez choisir de les gérer manuellement.  Pour plus d’informations sur les options de gestion des plages d’identité fournies par la réplication, consultez [Répliquer des colonnes d’identité](../../../relational-databases/replication/publish/replicate-identity-columns.md).  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Recommandations](#Recommendations)  
  
-   **Pour gérer des colonnes d'identité à l'aide de :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Recommandations  
  
-   Lors de la publication d'une table dans plusieurs publications, vous devez spécifier les mêmes options de gestion des plages d'identité pour les différentes publications. Pour plus d’informations, consultez « Publication de tables dans plusieurs publications » dans la rubrique [Publier des données et des objets de base de données](../../../relational-databases/replication/publish/publish-data-and-database-objects.md).  
  
-   Pour créer un numéro à incrémentation automatique qui peut être utilisé dans plusieurs tables ou être appelé par des applications sans faire référence à une table, consultez [Numéros de séquence](../../../relational-databases/sequence-numbers/sequence-numbers.md).  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
 Spécifiez une option de gestion des colonnes d’identité sous l’onglet **Propriétés** de la boîte de dialogue **Propriétés de l’article - \<Article>** de l’Assistant Nouvelle publication. Pour plus d’informations sur l’utilisation de cet Assistant, consultez [Créer une publication](../../../relational-databases/replication/publish/create-a-publication.md). Dans l'Assistant Nouvelle publication :  
  
-   Si vous sélectionnez **Publication de fusion** ou **Publication transactionnelle avec abonnements pouvant être mis à jour** dans la page **Type de publication** , sélectionnez une gestion des plages d'identité automatique ou manuelle (l'option par défaut, automatique, est recommandée). Après la publication de la table, la propriété ne peut plus être modifiée mais d'autres propriétés liées peuvent l'être.  
  
-   Si vous sélectionnez d'autres types de publication, vous devez définir une gestion manuelle des plages d'identité.  
  
 Modifiez les seuils et les plages d’identité sous l’onglet **Propriétés** de la page **Propriétés de l’article - \<Article>** , qui se trouve dans la boîte de dialogue **Propriétés de la publication - \<Publication>** . Pour plus d'informations sur l'accès à cette boîte de dialogue, consultez [Afficher et modifier les propriétés d’un serveur de publication](../../../relational-databases/replication/publish/view-and-modify-publication-properties.md).  
  
#### <a name="to-specify-an-identity-column-management-option"></a>Pour spécifier une option de gestion de colonnes d'identité  
  
1.  Si le serveur de publication exécute une version de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] antérieure à [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)], dans la page **Type de publication** de l'Assistant Nouvelle publication, sélectionnez **Publication de fusion** ou **Publication transactionnelle avec abonnements mis à jour**.  
  
2.  Dans la page **Articles** , sélectionnez une table avec une colonne d'identité.  
  
3.  Cliquez sur **Propriétés de l'article** puis sur **Définir les propriétés de l'article de la table en surbrillance**.  
  
4.  Sous l’onglet **Propriétés** de la boîte de dialogue **Propriétés de l’article - \<Article>** , dans la section **Gestion des plages d’identité**, définissez la propriété **Gérer automatiquement les plages d’identité** sur **Automatique** ou **Manuelle** (pour les serveurs de publication exécutant [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] ou plus), ou sur **True** ou **False** (pour les serveurs de publication exécutant une version de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] antérieure à [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)]).  
  
5.  Si vous avez sélectionné la valeur **Automatique** ou **Vrai** dans l'étape 4, entrez des valeurs pour les options du tableau suivant. Pour plus d’informations sur l’utilisation de ces paramètres, consultez la section « Affectation de plages d’identité » de [Répliquer des colonnes d’identité](../../../relational-databases/replication/publish/replicate-identity-columns.md).  
  
    |Option|Valeur|Description|  
    |------------|-----------|-----------------|  
    |**Taille de la plage sur le serveur de publication**|Valeur entière représentant la taille de la plage (par exemple, 20000).|Consultez la section « Affectation de plages d’identité » de [Répliquer des colonnes d’identité](../../../relational-databases/replication/publish/replicate-identity-columns.md).|  
    |**Taille de la plage sur l'Abonné**|Valeur entière représentant la taille de la plage (par exemple, 10000).|Consultez la section « Affectation de plages d’identité » de [Répliquer des colonnes d’identité](../../../relational-databases/replication/publish/replicate-identity-columns.md).|  
    |**Pourcentage du seuil de plage**|Valeur entière représentant le pourcentage du seuil (par exemple, 90 signifie 90 %)|Pourcentage représentant le nombre total de valeurs d'identité utilisées sur un nœud avant d'affecter une nouvelle plage d'identité.<br /><br /> <br /><br /> Remarque : Cette valeur doit être spécifiée mais elle est uniquement utilisée par : les Abonnés utilisant des abonnements mis à jour en attente et les Abonnés aux publications de fusion qui exécutent [!INCLUDE[ssEW](../../../includes/ssew-md.md)] ou des versions antérieures d’autres éditions de SQL Server. Pour plus d’informations, consultez la section « Affectation de plages d’identité » de [Répliquer des colonnes d’identité](../../../relational-databases/replication/publish/replicate-identity-columns.md).|  
    |**Valeur de départ de la prochaine plage**|Valeur de type entier. Lecture seule.|Valeur de départ de la prochaine plage. Si, par exemple, la plage actuelle va de 5001 à 6000, il s'agira de la valeur 6001.|  
    |**Valeur d'identité maximale**|Valeur de type entier. Lecture seule.|Valeur maximale de la colonne identité. Déterminée par le type de données de base de la colonne.|  
    |**Incrément**|Valeur de type entier. Lecture seule.|Valeur utilisée pour augmenter ou diminuer le nombre de la colonne d'identité à chaque insertion : en règle générale définie à 1.|  
  
6.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
#### <a name="to-modify-identity-ranges-and-thresholds-after-a-table-is-published"></a>Pour modifier les seuils et les plages d'identité après la publication d'une table  
  
1.  Dans la page **Articles** de la boîte de dialogue **Propriétés de la publication - \<Publication>** , sélectionnez une table avec une colonne d’identité.  
  
2.  Cliquez sur **Propriétés de l'article** puis sur **Définir les propriétés de l'article de la table en surbrillance**.  
  
3.  Dans l’onglet **Propriétés**, dans la boîte de dialogue **Propriétés de l’article - \<Article>** au sein de la section **Gestion des plages d’identité**, entrez des valeurs pour une ou plusieurs des propriétés suivantes : **Taille de la plage sur le serveur de publication**, **Taille de la plage sur l’Abonné** et **Pourcentage du seuil de plage**.  
  
4.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
5.  Cliquez sur **OK** dans la boîte de dialogue **Propriétés de la publication - \<Publication>** .  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
 Vous pouvez utiliser des procédures stockées de réplication pour spécifier les options de gestion des plages d'identité lors de la création d'un article.  
  
#### <a name="to-enable-automatic-identity-range-management-when-defining-articles-for-a-transactional-publication"></a>Pour activer la gestion automatique des plages d'identité lors de la définition d'articles pour une publication transactionnelle  
  
1.  Exécutez [sp_addarticle](../../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md)sur la base de données de publication du serveur de publication. Si la table source qui est publiée contient une colonne d’identité, affectez la valeur **auto** à **\@identityrangemanagementoption**, spécifiez la plage de valeurs d’identité affectée au serveur de publication pour **\@pub_identity_range**, la plage de valeurs d’identité affectée à chaque Abonné pour **\@identity_range** et le pourcentage des valeurs d’identité totales utilisé avant qu’une nouvelle plage d’identité soit affectée pour **\@threshold**. Pour plus d’informations sur la définition d’articles, consultez [Définir un article](../../../relational-databases/replication/publish/define-an-article.md).  
  
    > [!NOTE]  
    >  Vérifiez que le type de données de la colonne d'identité est suffisamment grand pour prendre en charge la plage totale des identités affectées à l'ensemble des Abonnés.  
  
#### <a name="to-disable-automatic-identity-range-management-when-defining-articles-for-a-transactional-publication"></a>Pour désactiver la gestion automatique des plages d'identité lors de la définition d'articles pour une publication transactionnelle  
  
1.  Exécutez [sp_addarticle](../../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md)sur la base de données de publication du serveur de publication. Affectez la valeur **manual** à **\@identityrangemanagementoption**. Pour plus d’informations sur la définition d’articles, consultez [Définir un article](../../../relational-databases/replication/publish/define-an-article.md).  
  
2.  Affectez des plages aux colonnes d'identité d'article sur l'Abonné pour éviter de générer des conflits lors de la mise à jour des Abonnés. Pour plus d’informations, consultez la section relative à l’affectation de plages pour la gestion manuelle de plages d’identité dans la rubrique [Répliquer des colonnes d’identité](../../../relational-databases/replication/publish/replicate-identity-columns.md).  
  
#### <a name="to-enable-automatic-identity-range-management-when-defining-articles-for-a-merge-publication"></a>Pour activer la gestion automatique des plages d'identité lors de la définition d'articles pour une publication de fusion  
  
1.  Dans la base de données de publication sur le serveur de publication, exécutez [sp_addmergearticle](../../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md). Si la table source qui est publiée contient une colonne d’identité, affectez la valeur **auto** à **\@identityrangemanagementoption**, spécifiez la plage de valeurs d’identité affectée à un abonnement serveur pour **\@pub_identity_range**, la plage de valeurs d’identité affectée au serveur de publication et à chaque abonnement client pour **\@identity_range** et le pourcentage des valeurs d’identité totales utilisé avant qu’une nouvelle plage d’identité soit affectée pour **\@threshold**. Pour plus d’informations sur l’affectation de nouvelles plages d’identité, consultez la section « Attribution de plages d’identité » de la rubrique [Répliquer des colonnes d’identité](../../../relational-databases/replication/publish/replicate-identity-columns.md). Pour plus d’informations sur la définition d’articles, consultez [Définir un article](../../../relational-databases/replication/publish/define-an-article.md).  
  
    > [!NOTE]  
    >  Vérifiez que le type de données de la colonne d'identité est suffisamment grand pour prendre en charge la plage totale des identités qui sont assignées à l'ensemble des Abonnés, en particulier pour les Abonnés avec des abonnements serveur.  
  
#### <a name="to-disable-automatic-identity-range-management-when-defining-articles-for-a-merge-publication"></a>Pour désactiver la gestion automatique des plages d'identité lors de la définition d'articles pour une publication de fusion  
  
1.  Dans la base de données de publication sur le serveur de publication, exécutez [sp_addmergearticle](../../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md). Affectez l’une des valeurs suivantes à **\@identityrangemanagementoption** :  
  
    -   **manuel** - les plages d'identité doivent être affectées manuellement pour la mise à jour des Abonnés.  
  
    -   **none** - les colonnes d'identité sur le serveur de publication ne seront pas définies en tant que colonnes d'identité sur l'Abonné.  
  
     Pour plus d’informations sur la définition d’articles, consultez [Définir un article](../../../relational-databases/replication/publish/define-an-article.md).  
  
2.  Affectez des plages aux colonnes d'identité d'article sur l'Abonné pour éviter de générer des conflits lors de la mise à jour des Abonnés.  
  
#### <a name="to-change-automatic-identity-range-management-settings-for-an-existing-article-in-a-snapshot-or-transactional-publication"></a>Pour modifier les paramètres de gestion automatique des plages d'identité pour un article existant dans une publication transactionnelle ou d'instantané  
  
1.  Dans la base de données de publication sur le serveur de publication, exécutez [sp_helparticle](../../../relational-databases/system-stored-procedures/sp-helparticle-transact-sql.md) et notez la valeur de **identityrangemanagementoption** dans le jeu de résultats. Si cette valeur est **0**, la gestion automatique des plages d'identité n'est pas activée.  
  
2.  Si la valeur de **identityrangemanagementoption** dans le jeu de résultats est **1**, modifiez les paramètres suivants :  
  
    -   Pour modifier les plages d'identité affectées, exécutez [sp_changearticle](../../../relational-databases/system-stored-procedures/sp-changearticle-transact-sql.md) au niveau du serveur de publication dans la base de données de publication. Affectez la valeur **identity_range** ou **pub_identity_range** à **\@property** et spécifiez la nouvelle valeur de la plage pour **\@value**.  
  
    -   Pour modifier le seuil d'affectation de nouvelles plages, exécutez [sp_changearticle](../../../relational-databases/system-stored-procedures/sp-changearticle-transact-sql.md) au niveau du serveur de publication dans la base de données de publication. Affectez la valeur **threshold** à **\@property** et spécifiez la nouvelle valeur de seuil pour **\@value**.  
  
#### <a name="to-change-automatic-identity-range-management-settings-for-an-existing-article-in-a-merge-publication"></a>Pour modifier les paramètres de gestion automatique des plages d'identité pour un article existant dans une publication de fusion  
  
1.  Dans la base de données de publication sur le serveur de publication, exécutez [sp_helpmergearticle](../../../relational-databases/system-stored-procedures/sp-helpmergearticle-transact-sql.md) et notez la valeur de **identity_support** dans le jeu de résultats. Si cette valeur est **0**, la gestion automatique des plages d'identité n'est pas activée.  
  
2.  Si la valeur de **identity_support** dans le jeu de résultats est **1**, modifiez les paramètres suivants :  
  
    -   Pour modifier les plages d'identité affectées, exécutez [sp_changemergearticle](../../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md) au niveau du serveur de publication dans la base de données de publication. Affectez la valeur **identity_range** ou **pub_identity_range** à **\@property** et spécifiez la nouvelle valeur de la plage pour **\@value**.  
  
    -   Pour modifier le seuil d'affectation de nouvelles plages, exécutez [sp_changemergearticle](../../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md) au niveau du serveur de publication dans la base de données de publication. Affectez la valeur **threshold** à **\@property** et spécifiez la nouvelle valeur de seuil pour **\@value**. Pour plus d’informations sur l’affectation de nouvelles plages d’identité, consultez la section « Attribution de plages d’identité » de la rubrique [Répliquer des colonnes d’identité](../../../relational-databases/replication/publish/replicate-identity-columns.md).  
  
    -   Pour désactiver la gestion automatique des plages d'identité, exécutez [sp_changemergearticle](../../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md) au niveau du serveur de publication dans la base de données de publication. Affectez la valeur **identityrangemanagementoption** à **\@property** et la valeur **manual** ou **none** à **\@value**.  
  
## <a name="see-also"></a>Voir aussi  
 [Peer-to-Peer Transactional Replication](../../../relational-databases/replication/transactional/peer-to-peer-transactional-replication.md)   
 [Concepts liés aux procédures stockées système de réplication](../../../relational-databases/replication/concepts/replication-system-stored-procedures-concepts.md)   
 [Répliquer des colonnes d’identité](../../../relational-databases/replication/publish/replicate-identity-columns.md)  
  
  
