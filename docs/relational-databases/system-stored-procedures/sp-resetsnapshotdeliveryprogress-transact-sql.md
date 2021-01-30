---
description: sp_resetsnapshotdeliveryprogress (Transact-SQL)
title: sp_resetsnapshotdeliveryprogress (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_resetsnapshotdeliveryprogress
- sp_resetsnapshotdeliveryprogress_TSQL
helpviewer_keywords:
- sp_resetsnapshotdeliveryprogress
ms.assetid: 5df7d86b-d343-4d9b-88b1-74429ed092e6
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 4b0d858fdaf03a50a4d87447dc57c09f4a79792c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99158421"
---
# <a name="sp_resetsnapshotdeliveryprogress-transact-sql"></a>sp_resetsnapshotdeliveryprogress (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Réinitialise le processus de livraison de l'instantané pour un abonnement par extraction de données (pull), afin de permettre le redémarrage de ce processus. Exécuté sur la base de données d'abonnement de l'abonné.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_resetsnapshotdeliveryprogress [ [ @verbose_level = ] verbose_level ]  
    [ , [ @drop_table = ] 'drop_table' ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @verbose_level = ] verbose_level` Spécifie la quantité d’informations retournées. *verbose_level* est de **type int**, avec **1** comme valeur par défaut. La valeur **1** signifie qu’une erreur est retournée si les verrous nécessaires ne peuvent pas être obtenus sur la table **MSsnapshotdeliveryprogress** , et **0** signifie qu’aucune erreur n’est retournée.  
  
`[ @drop_table = ] 'drop_table'` Indique s’il faut supprimer ou tronquer la table contenant des informations sur la progression de l’instantané. *DROP_TABLE* est de type **nvarchar (5)**, avec **false** comme valeur par défaut. FALSE signifie que la table est tronquée tandis que TRUE indique que la table est supprimée.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_resetsnapshotdeliveryprogress** supprime toutes les lignes de la table **MSsnapshotdeliveryprogress** . Elle supprime efficacement toutes les métadonnées laissées sur la base de données d'abonnement par les progressions précédentes des processus de livraison d'instantanés.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** ou du rôle de base de données fixe **db_owner** peuvent exécuter **sp_resetsnapshotdeliveryprogress**.  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
