---
description: sp_enumdsn (Transact-SQL)
title: sp_enumdsn (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_enumdsn
- sp_enumdsn_TSQL
helpviewer_keywords:
- sp_enumdsn
ms.assetid: 171cbc7d-7406-4cb0-8602-9405243bfd1d
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 711957655b7bb918fa49834160fd63329eb80343
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193607"
---
# <a name="sp_enumdsn-transact-sql"></a>sp_enumdsn (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Retourne la liste de tous les noms de source de données ODBC et OLE DB définis pour un serveur utilisant un compte d'utilisateur [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows spécifique. Cette procédure stockée est exécutée sur n'importe quelle base de données du serveur de publication.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_enumdsn  
```  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**Nom de la source de données**|**sysname**|Nom de la source de données.|  
|**Description**|**varchar(255)**|Description de la source de données.|  
|**Type**|**int**|Type de la source de données :<br /><br /> **1** = DSN ODBC<br /><br /> **3** = OLE DB source de données|  
|**Nom du fournisseur**|**varchar(255)**|Nom du fournisseur OLE DB. La valeur est NULL pour un DSN ODBC.|  
  
## <a name="remarks"></a>Notes  
 Chaque [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] service dispose d’un contexte utilisateur. Par contexte utilisateur, on entend un ensemble d'entrées du Registre qui comprend les définitions des sources de données ODBC pour cet utilisateur. Le contexte utilisateur est fourni par le nom d'utilisateur sous lequel s'exécute [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Par exemple, si le serveur s'exécute dans le contexte utilisateur du compte système, les noms de source de données (DSN) retournés seront tous des DSN système associés au compte système. Si le serveur s'exécute sous un compte d'utilisateur privé, seuls les DSN définis pour le compte privé de cet utilisateur sont retournés.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** peuvent exécuter **sp_enumdsn**.  
  
## <a name="see-also"></a>Voir aussi  
 [sp_dsninfo &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dsninfo-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
