---
description: sp_mergecleanupmetadata (Transact-SQL)
title: sp_mergecleanupmetadata (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_mergecleanupmetadata_TSQL
- sp_mergecleanupmetadata
helpviewer_keywords:
- sp_mergecleanupmetadata
ms.assetid: 892f8628-4cbe-4cc3-b959-ed45ffc24064
author: markingmyname
ms.author: maghan
ms.openlocfilehash: f6fa369de984a81e42185ddedfb874c672c3b541
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185299"
---
# <a name="sp_mergecleanupmetadata-transact-sql"></a>sp_mergecleanupmetadata (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Doit être utilisé uniquement dans les topologies de réplication qui incluent des serveurs exécutant des versions de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] antérieures au [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] Service Pack 1.**sp_mergecleanupmetadata** permet aux administrateurs de nettoyer les métadonnées dans les tables système **MSmerge_genhistory**, **MSmerge_contents** et **MSmerge_tombstone** . Cette procédure stockée est exécutée sur le serveur de publication dans la base de données de publication.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_mergecleanupmetadata [ [ @publication = ] 'publication' ]  
    [ , [ @reinitialize_subscriber = ] 'reinitialize_subscriber' ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @publication = ] 'publication'` Nom de la publication. *publication* est de **type sysname**, avec la valeur par défaut **%** , qui nettoie les métadonnées de toutes les publications. La publication doit déjà exister si elle est spécifiée de manière explicite.  
  
`[ @reinitialize_subscriber = ] 'subscriber'` Spécifie s’il faut réinitialiser l’abonné. *Subscriber* est de type **nvarchar (5)**, peut avoir la valeur **true** ou **false**, avec **true** comme valeur par défaut. Si la **valeur est true**, les abonnements sont marqués pour la réinitialisation. Si la **valeur est false**, les abonnements ne sont pas marqués pour la réinitialisation.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_mergecleanupmetadata** doit être utilisé uniquement dans les topologies de réplication qui incluent des serveurs exécutant des versions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] antérieures au [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] Service Pack 1. Les topologies qui incluent uniquement [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] Service Pack 1 ou une version ultérieure doivent utiliser la rétention automatique basée sur le nettoyage des métadonnées. Lorsque vous exécutez cette procédure stockée, soyez conscient de l'importance de la croissance nécessaire et potentielle du fichier journal sur l'ordinateur sur lequel la procédure stockée est exécutée.  
  
> [!CAUTION]
>  Après l’exécution de **sp_mergecleanupmetadata** , par défaut, tous les abonnements sur les abonnés des publications qui ont des métadonnées stockées dans **MSmerge_genhistory**, **MSmerge_contents** et **MSmerge_tombstone** sont marqués pour la réinitialisation, les modifications en attente sur l’abonné sont perdues et l’instantané actuel est marqué comme obsolète.  
> 
> [!NOTE]
>  S’il existe plusieurs publications dans une base de données, et que l’une de ces publications utilise une période de rétention de publication infinie (**\@ rétention** = **0**), l’exécution de **sp_mergecleanupmetadata** ne nettoie pas les métadonnées de suivi des modifications de réplication de fusion pour la base de données. C'est pour cette raison qu'il faut utiliser la période de rétention infinie avec prudence.  
  
 Lors de l’exécution de cette procédure stockée, vous pouvez choisir de réinitialiser les abonnés en affectant la **valeur true** au paramètre **\@ reinitialize_subscriber** (valeur par défaut) ou **false**. Si **sp_mergecleanupmetadata** est exécutée avec le paramètre **\@ Reinitialize_subscriber** ayant la valeur **true**, un instantané est réappliqué sur l’abonné même si l’abonnement a été créé sans instantané initial (par exemple, si les données et le schéma de l’instantané ont été appliqués manuellement ou existent déjà sur l’abonné). L’affectation de la **valeur false** au paramètre doit être utilisée avec précaution, car si la publication n’est pas réinitialisée, vous devez vous assurer que les données sur le serveur de publication et l’abonné sont synchronisées.  
  
 Quelle que soit la valeur de **\@ reinitialize_subscriber**, **sp_mergecleanupmetadata** échoue s’il existe des processus de fusion en cours qui tentent de charger des modifications sur un serveur de publication ou un abonné de republication au moment de l’appel de la procédure stockée.  
  
 **Exécution de sp_mergecleanupmetadata avec \@ reinitialize_subscriber = true :**  
  
1.  Bien qu'il ne soit pas nécessaire d'arrêter toutes les mises à jour des bases de données d'abonnement et de publication, cette opération est recommandée. Si les mises à jour se poursuivent, toute mise à jour réalisée sur un abonné depuis la dernière fusion est perdue lorsque la publication est réinitialisée, mais les données de convergence sont conservées.  
  
2.  Exécutez une fusion à l'aide de l'Agent de fusion. Nous vous recommandons d’utiliser l’option **de ligne de commande-Validate** agent sur chaque abonné lorsque vous exécutez l’agent de fusion. Si vous exécutez des fusions en mode continu, consultez *considérations particulières sur les fusions en mode continu* plus loin dans cette section.  
  
3.  Une fois toutes les fusions terminées, exécutez **sp_mergecleanupmetadata**.  
  
4.  Exécutez **sp_reinitmergepullsubscription** sur tous les abonnés à l’aide d’un abonnement par extraction nommé ou anonyme pour garantir la convergence des données.  
  
5.  Si vous exécutez des fusions en mode continu, consultez *considérations particulières sur les fusions en mode continu* plus loin dans cette section.  
  
6.  Régénérez les fichiers d'instantané de toutes les publications de fusion impliquées à tous les niveaux. Si vous tentez d'effectuer la fusion sans régénérer l'instantané au préalable, vous recevez un message vous invitant à la régénérer.  
  
7.  Sauvegardez la base de données de publication. car sans cela, une opération de fusion peut échouer après la restauration de la base de données de publication.  
  
 **Exécution de sp_mergecleanupmetadata avec \@ reinitialize_subscriber = false :**  
  
1.  Arrêtez **toutes les** mises à jour des bases de données de publication et d’abonnement.  
  
2.  Exécutez une fusion à l'aide de l'Agent de fusion. Nous vous recommandons d’utiliser l’option **de ligne de commande-Validate** agent sur chaque abonné lorsque vous exécutez l’agent de fusion. Si vous exécutez des fusions en mode continu, consultez *considérations particulières sur les fusions en mode continu* plus loin dans cette section.  
  
3.  Une fois toutes les fusions terminées, exécutez **sp_mergecleanupmetadata**.  
  
4.  Si vous exécutez des fusions en mode continu, consultez *considérations particulières sur les fusions en mode continu* plus loin dans cette section.  
  
5.  Régénérez les fichiers d'instantané de toutes les publications de fusion impliquées à tous les niveaux. Si vous tentez d'effectuer la fusion sans régénérer l'instantané au préalable, vous recevez un message vous invitant à la régénérer.  
  
6.  Sauvegardez la base de données de publication. car sans cela, une opération de fusion peut échouer après la restauration de la base de données de publication.  

 **Remarques importantes concernant les fusions en mode continu**  
  
 Si vous exécutez des fusions en mode continu, vous devez :  
  
-   Arrêtez le Agent de fusion, puis effectuez une autre fusion sans le paramètre **-Continuous** spécifié.  
  
-   Désactivez la publication avec **sp_changemergepublication** pour vous assurer que les fusions en mode continu qui interrogent l’état de la publication échouent.  
  
    ```  
    EXEC central..sp_changemergepublication @publication = 'dynpart_pubn', @property = 'status', @value = 'inactive'  
    ```  
  
 Lorsque vous avez terminé l’étape 3 de l’exécution de **sp_mergecleanupmetadata**, reprenez les fusions en mode continu en fonction de la façon dont vous les avez arrêtées. Un des deux éléments suivants :  
  
-   Rajoutez le paramètre **-Continuous** pour le agent de fusion.  
  
-   Réactivez la publication avec **sp_changemergepublication.**  
  
    ```  
    EXEC central..sp_changemergepublication @publication = 'dynpart_pubn', @property = 'status', @value = 'active'  
    ```  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** ou du rôle de base de données fixe **db_owner** peuvent exécuter **sp_mergecleanupmetadata**.  
  
 Pour utiliser cette procédure stockée, le serveur de publication doit exécuter [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)], Les abonnés doivent exécuter [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] ou [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 7,0, Service Pack 2.  
  
## <a name="see-also"></a>Voir aussi  
 [MSmerge_genhistory &#40;Transact-SQL&#41;](../../relational-databases/system-tables/msmerge-genhistory-transact-sql.md)   
 [MSmerge_contents &#40;Transact-SQL&#41;](../../relational-databases/system-tables/msmerge-contents-transact-sql.md)   
 [MSmerge_tombstone &#40;Transact-SQL&#41;](../../relational-databases/system-tables/msmerge-tombstone-transact-sql.md)  
  
  
