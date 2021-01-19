---
description: Abonnés de réplication sur des tables optimisées en mémoire
title: Abonnés de réplication sur des tables optimisées en mémoire | Microsoft Docs
ms.custom: ''
ms.date: 11/21/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
ms.assetid: 1a8e6bc7-433e-471d-b646-092dc80a2d1a
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 71a245f6263b938cef7a3841b3570a3a9137a1db
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/13/2021
ms.locfileid: "98172201"
---
# <a name="replication-to-memory-optimized-table-subscribers"></a>Abonnés de réplication sur des tables optimisées en mémoire
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Les tables agissant comme des abonnés à la réplication de capture instantanée et à la réplication transactionnelle, à l’exclusion de la réplication transactionnelle d’égal à égal, peuvent être configurées en tant que tables mémoire optimisées. Les autres configurations de réplication ne sont pas compatibles avec les tables mémoire optimisées. Cette fonctionnalité est disponible à partir de la version [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)].  
  
## <a name="two-configurations-are-required"></a>Deux configurations requises  
  
-   **Configurer la base de données de l’abonné pour la prise en charge de la réplication vers les tables mémoire optimisées**  
  
     Affectez à la propriété **\@memory_optimized** la valeur **true** en utilisant [sp_addsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md) ou [sp_changesubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changesubscription-transact-sql.md).  
  
-   **Configurer l’article pour la prise en charge de la réplication vers les tables mémoire optimisées**  
  
     Définissez l’option `@schema_option = 0x40000000000` pour l’article en utilisant [sp_addarticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md) ou [sp_changearticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changearticle-transact-sql.md).  
  
#### <a name="to-configure-a-memory-optimized-table-as-a-subscriber"></a>Pour configurer une table mémoire optimisée en tant qu’abonné  
  
1.  Créez une publication transactionnelle. Pour plus d’informations, voir [Create a Publication](../../relational-databases/replication/publish/create-a-publication.md).  
  
2.  Ajoutez des articles à la publication. Pour plus d’informations, consultez [définir un Article](../../relational-databases/replication/publish/define-an-article.md).  
  
     Si vous effectuez la configuration en utilisant [!INCLUDE[tsql](../../includes/tsql-md.md)], affectez au paramètre **\@schema_option** de la procédure stockée **sp_addarticle** la valeur   
    **0x40000000000**.  
  
3.  Dans la fenêtre de propriétés d’article, définissez **Activer l’optimisation mémoire** sur **true**.  
  
4.  Démarrez le travail de l'Agent d'instantané pour générer l'instantané initial pour cette publication. Pour plus d’informations, voir [Create and Apply the Initial Snapshot](../../relational-databases/replication/create-and-apply-the-initial-snapshot.md).  
  
5.  Ensuite, créez un abonnement. Dans l’ **Assistant Nouvel abonnement** , définissez **Abonnement de mémoire optimisée** sur **true**.  

 Les tables mémoire optimisées doivent maintenant commencer à recevoir les mises à jour du serveur de publication.  
  
#### <a name="reconfigure-an-existing-transaction-replication"></a>Reconfigurer une réplication de transaction existante  
  
1.  Accédez aux propriétés d’abonnement dans [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] et définissez **Abonnement de mémoire optimisée** sur **true**. Les modifications ne sont appliquées qu’une fois l’abonnement réinitialisé.  
  
     Si vous effectuez la configuration en utilisant [!INCLUDE[tsql](../../includes/tsql-md.md)], affectez au nouveau paramètre **\@memory_optimized** de la procédure stockée **sp_addsubscription** la valeur true.  
  
2.  Accédez aux propriétés d’article d’une publication dans [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] , puis définissez **Activer l’optimisation mémoire** sur true.  
  
     Si vous effectuez la configuration en utilisant [!INCLUDE[tsql](../../includes/tsql-md.md)], affectez au paramètre **\@schema_option** de la procédure stockée **sp_addarticle** la valeur   
    **0x40000000000**.  
  
3.  Les tables mémoire optimisées ne prennent pas en charge les index cluster. Pour que la réplication gère ces types d’index en les convertissant en index non cluster sur la destination, définissez **Convertir l’index cluster en index non-cluster pour l’article optimisé en mémoire** sur true.  
  
     Si vous effectuez la configuration en utilisant [!INCLUDE[tsql](../../includes/tsql-md.md)], affectez au paramètre **\@schema_option** de la procédure stockée **sp_addarticle** la valeur **0x0000080000000000**.  
  
4.  Régénérez la capture instantanée.  
  
5.  Réinitialisez l’abonnement.  
  
## <a name="remarks-and-restrictions"></a>Remarques et restrictions  
 Seule la réplication transactionnelle monodirectionnelle est prise en charge. La réplication transactionnelle d'égal à égal n'est pas prise en charge.  
  
 Les tables mémoire optimisées ne peuvent pas être publiées.  
  
 Les tables de réplication sur le serveur de distribution ne peuvent pas être configurées comme des tables mémoire optimisées.  
  
 La réplication de fusion ne peut pas inclure des tables mémoire optimisées.  
  
 Sur l'abonné, les tables impliquées dans la réplication transactionnelle peuvent être configurées en tant que tables mémoire optimisées, mais les tables d'abonné doivent répondre aux exigences des tables mémoire optimisées. Cette fonction requiert les restrictions suivantes.  
 
-   Les tables répliquées en tables mémoire optimisées sur un abonné sont limitées aux types de données autorisés dans les tables mémoire optimisées. Pour plus d’informations, consultez [Types de données pris en charge pour l’OLTP en mémoire](../../relational-databases/in-memory-oltp/supported-data-types-for-in-memory-oltp.md).  
  
-   Toutes les fonctionnalités Transact-SQL ne sont pas prises en charge avec les tables optimisées en mémoire. Pour plus d’informations, consultez [Constructions Transact-SQL non prises en charge par OLTP en mémoire](../../relational-databases/in-memory-oltp/transact-sql-constructs-not-supported-by-in-memory-oltp.md).  
  
##  <a name="modifying-a-schema-file"></a><a name="Schema"></a> Modification d'un fichier de schéma  
  
-   Si vous utilisez l'option de table mémoire optimisée `DURABILITY = SCHEMA_AND_DATA` , la table doit avoir un index de clé primaire non cluster.  
  
-   ANSI_PADDING doit être activé.  
  
  
  
