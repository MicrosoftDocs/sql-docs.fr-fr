---
description: MSSQL_ENG021798
title: MSSQL_ENG021798 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
helpviewer_keywords:
- MSSQL_ENG021798 error
ms.assetid: 596f5092-75ab-4a19-8582-588687c7b089
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: eeb2f515ceab6c2256f7c0f75c0847c1a55c8f18
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201510"
---
# <a name="mssql_eng021798"></a>MSSQL_ENG021798
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
    
## <a name="message-details"></a>Détails du message  
  
|Attribut|Valeur|  
|-|-|  
|Nom du produit|SQL Server|  
|ID de l’événement|21798|  
|Source de l’événement|MSSQLSERVER|  
|Composant|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|  
|Nom symbolique||  
|Texte du message|Le travail de l'Agent « %1!s! » doit être ajouté à l'aide de « %2!s! » avant de continuer. Consultez la documentation de '%s'.|  
  
## <a name="explanation"></a>Explication  
 Pour créer une publication, vous devez être membre du rôle de serveur fixe **sysadmin** sur le serveur de publication, ou membre du rôle de base de données fixe **db_owner** dans la base de données de publication. Si vous êtes membre du rôle **db_owner** , cette erreur est émise si :  
  
-   Vous exécutez des scripts à partir de [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)]. Le modèle de sécurité a changé dans [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]et ces scripts doivent être mis à jour.  
  
-   La procédure stockée **sp_addpublication** est exécutée avant l’exécution de [sp_addlogreader_agent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlogreader-agent-transact-sql.md). Ceci s'applique à toutes les publications transactionnelles.  
  
-   La procédure stockée **sp_addpublication** est exécutée avant l’exécution de [sp_addqreader_agent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addqreader-agent-transact-sql.md). Ceci s’applique aux publications transactionnelles qui sont activées pour les abonnements de mise à jour en attente (valeur TRUE pour le paramètre `@allow_queued_tran` de **sp_addpublication**).  
  
 Les procédures stockées **sp_addlogreader_agent** et **sp_addqreader_agent** créent chacune un travail d'Agent et vous permettent de spécifier le compte [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows sous lequel l'Agent s'exécute. Pour les utilisateurs membres du rôle **sysadmin** , les travaux d'Agents sont créés implicitement si **sp_addlogreader_agent** et **sp_addqreader_agent** ne sont pas exécutées ; les Agents s'exécutent dans le contexte du compte de service de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur le serveur de distribution. Bien que **sp_addlogreader_agent** et **sp_addqreader_agent** ne soient pas nécessaires pour les utilisateurs membres du rôle **sysadmin** , il est recommandé par mesure de sécurité de spécifier un compte distinct pour les Agents. Pour plus d’informations, voir [Replication Agent Security Model](../../relational-databases/replication/security/replication-agent-security-model.md).  
  
## <a name="user-action"></a>Action de l'utilisateur  
 Veillez à exécuter les procédures dans le bon ordre. Pour plus d’informations, voir [Create a Publication](../../relational-databases/replication/publish/create-a-publication.md). Si vous avez des scripts de réplication provenant de versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], mettez-les à jour pour qu'ils incluent les procédures stockées et les paramètres requis par [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] et les versions ultérieures. Pour plus d’informations, consultez [Mettre à niveau les scripts de réplication &#40;programmation Transact-SQL de la réplication&#41;](../../relational-databases/replication/administration/upgrade-replication-scripts-replication-transact-sql-programming.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Guide de référence des erreurs et des événements &#40;réplication&#41;](../../relational-databases/replication/errors-and-events-reference-replication.md)  
  
  
