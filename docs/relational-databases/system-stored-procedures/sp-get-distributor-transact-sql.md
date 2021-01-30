---
description: sp_get_distributor (Transact-SQL)
title: sp_get_distributor (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_get_distributor
- sp_get_distributor_TSQL
helpviewer_keywords:
- sp_get_distributor
ms.assetid: f0134448-bc17-4f2f-bd81-619351ce56ac
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 82c9e743cf3c1ddce87642fb3320b74eb5f7b6e1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99105580"
---
# <a name="sp_get_distributor-transact-sql"></a>sp_get_distributor (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Détermine si un serveur de distribution est installé sur un serveur. Cette procédure stockée est exécutée sur n'importe quelle base de données de l'ordinateur sur lequel le serveur de distribution est recherché.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_get_distributor   
```  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**ordinateur**|**int**|**0** = non ; **1** = Oui|  
|**serveur de distribution**|**sysname**|Nom du serveur de distribution|  
|**base de distribution de distribution installée**|**int**|**0** = non ; **1** = Oui|  
|**is distribution publisher**|**int**|**0** = non ; **1** = Oui|  
|**a un serveur de publication de distribution distant**|**int**|**0** = non ; **1** = Oui|  
  
## <a name="remarks"></a>Notes  
 **sp_get_distributor** est principalement utilisé par le [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] dans un instantané, une réplication transactionnelle et une réplication de fusion.  
  
## <a name="permissions"></a>Autorisations  
 N’importe quel utilisateur peut exécuter **sp_get_distributor**. Un jeu de résultats non NULL est retourné lorsque cette procédure stockée est exécutée par les membres des rôles de base de données fixes **db_owner** ou **replmonitor** sur la base de données de distribution, ou les membres du rôle de base de données fixe **db_owner** sur au moins une base de données publiée. Un jeu de résultats non NULL est également retourné lorsque cette procédure stockée est exécutée par des utilisateurs dans la liste d’accès à la publication (PAL) d’au moins une base de données publiée, ou dans la liste d’accès aux publications de la base de données de distribution pour un serveur de publication non SQL Server, peut également exécuter **sp_get_distributor**.  
  
## <a name="see-also"></a>Voir aussi  
 [Configurer la publication et la distribution](../../relational-databases/replication/configure-publishing-and-distribution.md)   
 [Script d’information du serveur de distribution et du serveur de publication](../../relational-databases/replication/administration/distributor-and-publisher-information-script.md)   
 [Procédures stockées de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
