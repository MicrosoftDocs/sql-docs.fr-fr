---
description: sp_repladdcolumn (Transact-SQL)
title: sp_repladdcolumn (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_repladdcolumn_TSQL
- sp_repladdcolumn
helpviewer_keywords:
- sp_repladdcolumn
ms.assetid: d6220f9f-c738-4f9c-bcf8-419994e86c81
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 3579b68a229bd939be61bdb233e5d429dd0ca4c7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193533"
---
# <a name="sp_repladdcolumn-transact-sql"></a>sp_repladdcolumn (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Ajoute une colonne à un article de table existant qui a été publié. Permet d'ajouter la nouvelle colonne à tous les serveurs de publication, ou à une publication spécifique, qui publient cette table. Cette procédure stockée est exécutée sur le serveur de publication dans la base de données de publication.  
  
> [!IMPORTANT]
>  Cette procédure stockée est déconseillée ; elle est prise en charge pour des raisons de compatibilité descendante. Elle doit être utilisée uniquement avec les serveurs de publication [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] et les [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] abonnés de republication. Cette procédure ne doit pas être utilisée sur des colonnes avec des types de données qui ont été introduits dans [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] ou ultérieur.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_repladdcolumn [ @source_object = ] 'source_object', [ @column = ] 'column' ]  
    [ , [ @typetext = ] 'typetext' ]  
    [ , [ @publication_to_add = ] 'publication_to_add' ]  
    [ , [ @from_agent = ] from_agent ]  
    [ , [ @schema_change_script = ] 'schema_change_script' ]  
    [ , [ @force_invalidate_snapshot = ] force_invalidate_snapshot ]  
    [ , [ @force_reinit_subscription = ] force_reinit_subscription ]  
```  
  
## <a name="arguments"></a>Arguments  
 [ @source_object =] '*source_object*'  
 Nom de l'article de table contenant la nouvelle colonne à ajouter. *source_object* est de type **nvarchar (358**), sans valeur par défaut.  
  
 [ @column =] '*colonne*'  
 Nom de la colonne dans la table à ajouter pour la réplication. *Column* est de **type sysname**, sans valeur par défaut.  
  
 [ @typetext = ] '*TypeText*'  
 Définition de la colonne ajoutée. *TypeText* est de type **nvarchar (3000)**, sans valeur par défaut. Par exemple, si la colonne order_filled est ajoutée et qu’il s’agit d’un champ à un seul caractère, et non NULL, et que a une valeur par défaut de **N**, order_filled serait le paramètre de *colonne* , tandis que la définition de la colonne, **char (1) n’est pas une contrainte null constraint_name la valeur par défaut « N »** est la valeur du paramètre *TypeText* .  
  
 [ @publication_to_add =] '*publication_to_add*'  
 Nom de la publication à laquelle la nouvelle colonne est ajoutée. *publication_to_add* est de type **nvarchar (4000)**, avec **All** comme valeur par défaut. Si la **totalité** est, toutes les publications contenant cette table sont affectées. Si *publication_to_add* est spécifié, seule la nouvelle colonne est ajoutée à cette publication.  
  
 [ @from_agent =] *from_agent*  
 Indique si la procédure stockée est exécutée par un agent de réplication. *from_agent* est de **type int**, avec **0** comme valeur par défaut, où la valeur **1** est utilisée lorsque cette procédure stockée est exécutée par un agent de réplication, et dans tous les autres cas, la valeur par défaut **0** doit être utilisée.  
  
 [ @schema_change_script =] '*schema_change_script*'  
 Spécifie le nom et le chemin d'accès d'un script [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilisé pour modifier les procédures stockées personnalisées générées par le système. *schema_change_script* est de type **nvarchar (4000)**, avec NULL comme valeur par défaut. La réplication permet aux procédures stockées personnalisées définies par l'utilisateur de remplacer une ou plusieurs procédures par défaut utilisées dans la réplication transactionnelle. *schema_change_script* est exécutée après la modification d’un schéma d’un article de table répliqué à l’aide de sp_repladdcolumn, et peut être utilisé pour effectuer l’une des opérations suivantes :  
  
-   Si des procédures stockées personnalisées sont automatiquement régénérées, *schema_change_script* pouvez les utiliser pour supprimer ces procédures stockées personnalisées et les remplacer par des procédures stockées personnalisées définies par l’utilisateur qui prennent en charge le nouveau schéma.  
  
-   Si les procédures stockées personnalisées ne sont pas régénérées automatiquement, *schema_change_script* pouvez les utiliser pour régénérer ces procédures stockées ou pour créer des procédures stockées personnalisées définies par l’utilisateur.  
  
 [ @force_invalidate_snapshot =] *force_invalidate_snapshot*  
 Active ou désactive la possibilité d'invalider un instantané. *force_invalidate_snapshot* est un **bit**, avec **1** comme valeur par défaut.  
  
 **1** indique que les modifications apportées à l’article peuvent entraîner la non-validité de l’instantané. Si tel est le cas, la valeur **1** accorde l’autorisation de se produire pour le nouvel instantané.  
  
 **0** indique que les modifications apportées à l’article n’entraînent pas la non-validité de l’instantané.  
  
 [ @force_reinit_subscription =] *force_reinit_subscription*  
 Active ou désactive la possibilité de réinitialiser l'abonnement. *force_reinit_subscription* est un **bit** avec **0** comme valeur par défaut.  
  
 **0** indique que les modifications apportées à l’article n’entraînent pas la réinitialisation de l’abonnement.  
  
 **1** indique que les modifications apportées à l’article peuvent provoquer la réinitialisation de l’abonnement. Si tel est le cas, la valeur **1** accorde l’autorisation de réinitialisation de l’abonnement.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe sysadmin et du rôle de base de données fixe db_owner peuvent exécuter sp_repladdcolumn.  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctionnalités dépréciées dans Réplication SQL Server](../../relational-databases/replication/deprecated-features-in-sql-server-replication.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
