---
description: sp_browsesnapshotfolder (Transact-SQL)
title: sp_browsesnapshotfolder (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_browsesnapshotfolder
- sp_browsesnapshotfolder_TSQL
helpviewer_keywords:
- sp_browsesnapshotfolder
ms.assetid: 0872edf2-4038-4bc1-a68d-05ebfad434d2
author: markingmyname
ms.author: maghan
ms.openlocfilehash: a74c1325b2af76a96c0fc544dfbbf136b0561225
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199188"
---
# <a name="sp_browsesnapshotfolder-transact-sql"></a>sp_browsesnapshotfolder (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Retourne le chemin d'accès complet du dernier instantané généré pour une publication. Cette procédure stockée est exécutée sur le serveur de publication dans la base de données de publication.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_browsesnapshotfolder [@publication= ] 'publication'  
    { [ , [ @subscriber = ] 'subscriber' ]  
 [ , [ @subscriber_db = ] 'subscriber_db' ] }  
```  
  
## <a name="arguments"></a>Arguments  
`[ @publication = ] 'publication'` Nom de la publication qui contient l’article. *publication* est de **type sysname**, sans valeur par défaut.  
  
`[ @subscriber = ] 'subscriber'` Nom de l’abonné. *Subscriber* est de **type sysname**, avec NULL comme valeur par défaut.  
  
`[ @subscriber_db = ] 'subscriber_db'` Nom de la base de données d’abonnement. *subscriber_db* est de **type sysname**, avec NULL comme valeur par défaut.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**snapshot_folder**|**nvarchar(512)**|Chemin d'accès complet du répertoire de l'instantané.|  
  
## <a name="remarks"></a>Notes  
 **sp_browsesnapshotfolder** est utilisé dans la réplication d’instantané et la réplication transactionnelle.  
  
 Si les champs *Subscriber* et *subscriber_db* sont laissés null, la procédure stockée retourne le dossier d’instantané de l’instantané le plus récent qu’il peut trouver pour la publication. Si les champs *Subscriber* et *subscriber_db* sont spécifiés, la procédure stockée retourne le dossier d’instantané de l’abonnement spécifié. Si aucun instantané n'a été généré pour la publication, un jeu de résultats vide est retourné.  
  
 Si la publication est configurée de sorte à générer les fichiers d'instantané dans le répertoire de travail du serveur de publication et dans son dossier d'instantané, le jeu de résultats comporte deux lignes. La première ligne contient le dossier d'instantané de la publication, la seconde le répertoire de travail du serveur de publication. **sp_browsesnapshotfolder** est utile pour déterminer le répertoire dans lequel les fichiers d’instantanés sont générés.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** ou du rôle de base de données fixe **db_owner** peuvent exécuter **sp_browsesnapshotfolder**.  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
