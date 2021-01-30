---
description: sp_dsninfo (Transact-SQL)
title: sp_dsninfo (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_dsninfo
- sp_dsninfo_TSQL
helpviewer_keywords:
- sp_dsninfo
ms.assetid: 34648615-814b-42bc-95a3-50e86b42ec4d
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 65a3f12555d1dbb0e26702229fdcc47664cc4c82
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186972"
---
# <a name="sp_dsninfo-transact-sql"></a>sp_dsninfo (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Retourne les informations sur la source de données ODBC ou OLE DB à partir du serveur de distribution associé au serveur actuel. Cette procédure stockée est exécutée sur n’importe quelle base de données du serveur de distribution.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_dsninfo [ @dsn =] 'dsn'   
    [ , [ @infotype =] 'info_type']   
    [ , [ @login =] 'login']   
    [ , [ @password =] 'password']  
    [ , [ @dso_type=] dso_type]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @dsn = ] 'dsn'` Nom du serveur lié DSN ODBC ou OLE DB. *DSN* est de type **varchar (128)**, sans valeur par défaut.  
  
`[ @infotype = ] 'info_type'` Type d’informations à retourner. Si *Info_type* n’est pas spécifié ou si null est spécifié, tous les types d’informations sont retournés. *Info_type* est de type **varchar (128)**, avec NULL comme valeur par défaut et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**DBMS_NAME**|Spécifie le nom du fournisseur de la source de données.|  
|**DBMS_VERSION**|Spécifie la version de la source de données.|  
|**DATABASE_NAME**|Spécifie le nom de la base de données.|  
|**SQL_SUBSCRIBER**|Spécifie que la source de données peut être un Abonné.|  
  
`[ @login = ] 'login'` Nom de connexion de la source de données. Si la source de données comporte une connexion, spécifiez NULL ou omettez le paramètre. *login* est de type **varchar (128)**, avec NULL comme valeur par défaut.  
  
`[ @password = ] 'password'` Mot de passe de la connexion. Si la source de données comporte une connexion, spécifiez NULL ou omettez le paramètre. *Password* est de type **varchar (128)**, avec NULL comme valeur par défaut.  
  
`[ @dso_type = ] dso_type` Type de source de données. *dso_type* est de **type int** et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**1** (par défaut)|Source de données ODBC|  
|**3**|Source de données OLE DB|  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**Type d’informations**|**nvarchar (64)**|Types d'information, tels que DBMS_NAME, DBMS_VERSION, DATABASE_NAME, SQL_SUBSCRIBER.|  
|**Valeur**|**nvarchar(512)**|Valeur du type d'information associé.|  
  
## <a name="remarks"></a>Notes  
 **sp_dsninfo** est utilisé dans tous les types de réplications.  
  
 **sp_dsninfo** récupère les informations de source de données ODBC ou OLE DB qui indiquent si la base de données peut être utilisée pour la réplication ou l’interrogation.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** peuvent exécuter **sp_dsninfo**.  
  
## <a name="see-also"></a>Voir aussi  
 [sp_enumdsn &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-enumdsn-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
