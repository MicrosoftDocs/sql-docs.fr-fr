---
description: sp_addscriptexec (Transact-SQL)
title: sp_addscriptexec (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_addscriptexec
- sp_addscriptexec_TSQL
helpviewer_keywords:
- sp_addscriptexec
ms.assetid: 1627db41-6a80-45b6-b0b9-c0b7f9a1c886
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 6349479d4f3ce3097771516640b0eec258e32f5b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207157"
---
# <a name="sp_addscriptexec-transact-sql"></a>sp_addscriptexec (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Publie un script SQL (fichier .sql) sur tous les Abonnés d'une publication. Cette procédure stockée est exécutée sur le serveur de publication dans la base de données de publication.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_addscriptexec [ @publication = ] publication  
    [ , [ @scriptfile = ] 'scriptfile' ]  
    [ , [ @skiperror = ] 'skiperror' ]  
    [ , [ @publisher = ] 'publisher' ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @publication = ] 'publication'` Nom de la publication. *publication* est de **type sysname**, sans valeur par défaut.  
  
`[ @scriptfile = ] 'scriptfile'` Est le chemin d’accès complet au fichier de script SQL. *scriptfile* est de type **nvarchar (4000)**, sans valeur par défaut.  
  
`[ @skiperror = ] 'skiperror'` Indique si le Agent de distribution ou Agent de fusion doit s’arrêter lorsqu’une erreur se produit pendant le traitement du script. *SkipError* est de valeur de **bit**, avec 0 comme valeur par défaut.  
  
 **0** = l’agent va s’arrêter.  
  
 **1** = l’agent continue le script et ignore l’erreur.  
  
`[ @publisher = ] 'publisher'` Spécifie un serveur de publication non- [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . *Publisher* est de **type sysname**, avec NULL comme valeur par défaut.  
  
> [!NOTE]  
>  l' *éditeur* ne doit pas être utilisé lors de la publication à partir d’un serveur de publication [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_addscriptexec** est utilisé dans la réplication transactionnelle et la réplication de fusion.  
  
 **sp_addscriptexec** n’est pas utilisé pour la réplication d’instantané.  
  
 Pour utiliser **sp_addscriptexec**, le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] compte de service doit disposer d’autorisations en lecture et en écriture sur l’emplacement de l’instantané, ainsi que des autorisations de lecture sur l’emplacement où sont stockés les scripts.  
  
 L' [utilitaire sqlcmd](../../tools/sqlcmd-utility.md) est utilisé pour exécuter le script sur l’abonné, et le script est exécuté dans le contexte de sécurité utilisé par le Agent de distribution ou agent de fusion lors de la connexion à la base de données d’abonnement. Lorsque l’agent est exécuté sur une version précédente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , l' [utilitaire osql](../../tools/osql-utility.md) est utilisé à la place de [sqlcmd](../../tools/sqlcmd-utility.md).  
  
 **sp_addscriptexec** est utile pour appliquer des scripts aux abonnés et utilise [sqlcmd](../../tools/sqlcmd-utility.md) pour appliquer le contenu du script à l’abonné. Toutefois, dans la mesure où les configurations des Abonnés peuvent varier, des scripts testés avant la publication sur le serveur de publication peuvent toujours provoquer des erreurs sur un Abonné. *SkipError* offre la possibilité de faire en sorte que les Agent de distribution ou agent de fusion ignorent les erreurs et continuent sur. Utilisez [sqlcmd](../../tools/sqlcmd-utility.md) pour tester les scripts avant d’exécuter **sp_addscriptexec**.  
  
> [!NOTE]  
>  Les erreurs ignorées sont toujours consignées dans l'historique de l'Agent à titre de référence.  
  
 L’utilisation de **sp_addscriptexec** pour poster un fichier de script pour les publications à l’aide de FTP pour la remise d’instantané est prise en charge uniquement pour les [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] abonnés.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** ou du rôle de base de données fixe **db_owner** peuvent exécuter **sp_addscriptexec**.  
  
## <a name="see-also"></a>Voir aussi  
 [Exécuter des scripts pendant la synchronisation &#40;la programmation Transact-SQL de la réplication&#41;](../../relational-databases/replication/execute-scripts-during-synchronization-replication-transact-sql-programming.md)   
 [Synchroniser les données](../../relational-databases/replication/synchronize-data.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
