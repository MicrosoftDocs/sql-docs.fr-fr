---
description: sysmail_start_sp (Transact-SQL)
title: sysmail_start_sp (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysmail_start_sp
- sysmail_start_sp_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_start_sp
ms.assetid: 25fd7bb6-cfdd-463f-bea8-c6fcb805d3f5
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: c3685409d82c2c1591ae129b93c4c3036f5925c1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181898"
---
# <a name="sysmail_start_sp-transact-sql"></a>sysmail_start_sp (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Démarre la messagerie de base de données en démarrant les objets [!INCLUDE[ssSB](../../includes/sssb-md.md)] utilisés par le programme externe.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sysmail_start_sp  
```  
  
## <a name="arguments"></a>Arguments  
 Aucun  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 None  
  
## <a name="remarks"></a>Notes  
 La messagerie de base de données n'est pas activée ou installée lors de l'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Utilisez l'Assistant Configuration de la messagerie de base de données pour activer et installer les objets de messagerie de base de données.  
  
 Cette procédure stockée se trouve dans la base de données **msdb** . Elle démarre la file d'attente de la messagerie de base de données contenant les demandes de messages sortants et active [!INCLUDE[ssSB](../../includes/sssb-md.md)] pour le programme externe.  
  
 Dès que les files d'attente ont démarré, le programme externe de la messagerie de base de données peut traiter les messages. Cette procédure vous permet de redémarrer les files d’attente une fois que les files d’attente ont été arrêtées avec la procédure stockée **sysmail_stop_sp** .  
  
> [!NOTE]  
>  Cette procédure stockée démarre simplement les files d'attente de la messagerie de base de données. Elle n'active pas la remise de messages [!INCLUDE[ssSB](../../includes/sssb-md.md)] dans la base de données.  
  
## <a name="permissions"></a>Autorisations  
 Les autorisations d’exécution pour cette procédure sont octroyées par défaut aux membres du rôle serveur fixe **sysadmin** .  
  
## <a name="examples"></a>Exemples  
 L’exemple suivant illustre le démarrage de Database Mail dans la base de données **msdb** . Il suppose que la messagerie de base de données a été activée.  
  
```  
USE msdb ;  
GO  
  
EXECUTE dbo.sysmail_start_sp ;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Messagerie de base de données](../../relational-databases/database-mail/database-mail.md)   
 [Database Mail option de configuration de serveur XPs](../../database-engine/configure-windows/database-mail-xps-server-configuration-option.md)   
 [sysmail_stop_sp &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sysmail-stop-sp-transact-sql.md)   
 [Database Mail des procédures stockées &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/database-mail-stored-procedures-transact-sql.md)  
  
  
