---
description: sp_changereplicationserverpasswords (Transact-SQL)
title: sp_changereplicationserverpasswords (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_changereplicationserverpasswords_TSQL
- sp_changereplicationserverpasswords
helpviewer_keywords:
- sp_changereplicationserverpasswords
ms.assetid: 9333da96-3a1c-4adb-9a74-5dac9ce596df
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: e301e37f252a81cf4e7c2634f291bc61bc2979c1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207047"
---
# <a name="sp_changereplicationserverpasswords-transact-sql"></a>sp_changereplicationserverpasswords (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Modifie les mots de passe stockés pour le [!INCLUDE[msCoName](../../includes/msconame-md.md)] compte Windows ou la [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] connexion utilisée par les agents de réplication lors de la connexion aux serveurs dans une topologie de réplication. En temps normal, vous devriez modifier un mot de passe pour chacun des Agents en cours d'exécution sur un serveur, même s'ils utilisent tous la même connexion ou le même compte. Cette procédure stockée vous permet de modifier le mot de passe pour toutes les instances d'une connexion ou d'un compte [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] donnés utilisés par tous les Agents de réplication exécutés sur un serveur. Cette procédure stockée est exécutée sur la base de données master de n'importe quel serveur de la topologie de réplication.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_changereplicationserverpasswords [ @login_type = ] login_type  
        , [ @login = ] 'login'   
        , [ @password = ] 'password'  
    [ , [ @server = ] 'server' ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @login_type = ] login_type` Type d’authentification pour les informations d’identification fournies. *LOGIN_TYPE* est de **type tinyint**, sans valeur par défaut.  
  
 **1** = authentification intégrée de Windows  
  
 **0**  =  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] authentification  
  
`[ @login = ] 'login'` Nom du compte Windows ou de la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] connexion en cours de modification. *login* est de type **nvarchar (257)**, sans valeur par défaut  
  
`[ @password = ] 'password'` Nouveau mot de passe à stocker pour la *connexion* spécifiée. *Password* est de **type sysname**, sans valeur par défaut.  
  
> [!NOTE]  
>  Après avoir modifié un mot de passe de réplication, vous devez arrêter puis redémarrer chaque Agent qui utilise ce mot de passe afin que les modifications apportées prennent effet.  
  
`[ @server = ] 'server'` Connexion serveur pour laquelle le mot de passe stocké est en cours de modification. *Server* est de **type sysname** et peut prendre l’une des valeurs suivantes :  
  
|Valeur|Description|  
|-----------|-----------------|  
|**conseiller**|Toutes les connexions d'Agent au serveur de distribution.|  
|**publisher**|Toutes les connexions d'Agent au serveur de publication.|  
|**côté**|Toutes les connexions d'Agent à l'Abonné.|  
|**%** valeurs|Toutes les connexions d'Agent à tous les serveurs d'une topologie de réplication.|  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_changereplicationserverpasswords** est utilisé avec tous les types de réplications.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** peuvent exécuter **sp_changereplicationserverpasswords**.  
  
## <a name="see-also"></a>Voir aussi  
 [Afficher et modifier les paramètres de sécurité de la réplication](../../relational-databases/replication/security/view-and-modify-replication-security-settings.md)  
  
  
