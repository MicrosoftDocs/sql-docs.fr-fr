---
description: MSpublications (Transact-SQL)
title: MSpublications (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSpublications
- MSpublications_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSpublications system table
ms.assetid: 7a0b3457-7265-4f24-a255-7f055d908f20
author: cawrites
ms.author: chadam
ms.openlocfilehash: 089270c4c95b35e25f44339a7f001b5ac194ec01
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098172"
---
# <a name="mspublications-transact-sql"></a>MSpublications (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La table **MSpublications** contient une ligne pour chaque publication répliquée par un serveur de publication. Cette table est stockée dans la base de données de distribution.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**publisher_id**|**smallint**|ID du serveur de publication.|  
|**publisher_db**|**sysname**|Nom de la base de données du serveur de publication.|  
|**édition**|**sysname**|Nom de la publication.|  
|**publication_id**|**int**|ID de la publication.|  
|**publication_type**|**int**|Type de publication :<br /><br /> **0** = transactionnel.<br /><br /> **1** = instantané.<br /><br /> **2** = fusion.|  
|**thirdparty_flag**|**bit**|Indique si une publication est une [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] base de données :<br /><br /> **0**  =  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .<br /><br /> **1** = source de données autre que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
|**independent_agent**|**bit**|Indique s'il existe un Agent de distribution autonome pour cette publication.|  
|**immediate_sync**|**bit**|Indique si des fichiers de synchronisation sont créés ou recréés lors de chaque exécution de l'Agent d'instantané.|  
|**allow_push**|**bit**|Indique si des abonnements par envoi de données (push) peuvent être créés pour la publication concernée|  
|**allow_pull**|**bit**|Indique si des abonnements par extraction de données (pull) peuvent être créés pour la publication concernée|  
|**allow_anonymous**|**bit**|Indique si des abonnements anonymes peuvent être créés pour la publication concernée|  
|**description**|**nvarchar(255)**|Description de la publication.|  
|**vendor_name**|**nvarchar(100**|Nom du fournisseur si le serveur de publication n'est pas une base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**fixation**|**int**|Période de rétention de la publication, en heures.|  
|**sync_method**|**int**|Méthode de synchronisation :<br /><br /> **0** = natif (produit une sortie de copie en bloc en mode natif de toutes les tables).<br /><br /> **1** = caractère (produit une sortie de copie en bloc en mode caractère de toutes les tables).<br /><br /> **3** = simultanée (produit une sortie de copie en bloc en mode natif de toutes les tables, mais ne verrouille pas la table pendant l’instantané).<br /><br /> **4** = concurrent_c (produit une sortie de copie en bloc en mode caractère de toutes les tables, mais ne verrouille pas la table pendant l’instantané)<br /><br /> Les valeurs **3** et **4** sont disponibles pour la réplication transactionnelle et la réplication de fusion, mais pas pour la réplication d’instantané.|  
|**allow_subscription_copy**|**bit**|Active ou désactive la possibilité de copier les bases de données d'abonnement qui sont abonnées à la publication. **0** signifie que la copie est désactivée, et **1** signifie qu’elle est activée.|  
|**thirdparty_options**|**int**|Spécifie si l’affichage d’une publication dans le dossier réplication de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] est supprimé :<br /><br /> **0** = afficher une publication hétérogène dans le dossier réplication de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] .<br /><br /> **1** = supprimer l’affichage d’une publication hétérogène dans le dossier réplication dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] .|  
|**allow_queued_tran**|**bit**|Indique si la publication autorise la mise à jour en attente :<br /><br /> **0 =** La publication n’est pas en file d’attente.<br /><br /> **1** = la publication est mise en file d’attente.|  
|**options**|**int**|Aucune information n'est disponible pour cette version.|  
  
## <a name="see-also"></a>Voir aussi  
 [Tables de réplication &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vues de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
