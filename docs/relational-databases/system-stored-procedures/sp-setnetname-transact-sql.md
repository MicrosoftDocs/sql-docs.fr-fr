---
description: sp_setnetname (Transact-SQL)
title: sp_setnetname (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_setnetname
- sp_setnetname_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_setnetname
ms.assetid: f416ba81-3835-4588-b0a3-2fe75589490e
author: markingmyname
ms.author: maghan
ms.openlocfilehash: f84120579d3eae3b2755eaab89eff137af6190db
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193497"
---
# <a name="sp_setnetname-transact-sql"></a>sp_setnetname (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Définit les noms réseau dans **sys. Servers** sur leurs noms d’ordinateurs réseau réels pour les instances distantes de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Cette procédure peut être utilisée pour activer l'exécution d'appels de procédures stockées distantes à des ordinateurs dont le nom réseau contient des identificateurs [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non valides.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_setnetname  
@server = 'server',   
     @netname = 'network_name'  
```  
  
## <a name="arguments"></a>Arguments  
 **@server = '** *serveur* **'**  
 Nom du serveur distant tel qu'il est référencé dans la syntaxe RPC codée par l'utilisateur. Une seule ligne dans **sys. Servers** doit déjà exister pour utiliser ce *serveur*. *server* est de type **sysname** et n'a pas de valeur par défaut.  
  
 **@netname = '** *network_name* **'**  
 Nom réseau de l'ordinateur auquel les appels de procédures stockées distantes sont effectuées. *network_name* est de **type sysname**, sans valeur par défaut.  
  
 Ce nom doit correspondre au nom d'ordinateur [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows et peut comprendre des caractères non autorisés dans les identificateurs [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 None  
  
## <a name="remarks"></a>Notes  
 Certains appels de procédures stockées distantes à des ordinateurs Windows peuvent rencontrer des problèmes si le nom d'ordinateur contient des identificateurs non valides.  
  
 Les serveurs liés et les serveurs distants résidant dans le même espace de nom, ils ne peuvent pas avoir le même nom. Toutefois, vous pouvez définir un serveur lié et un serveur distant sur un serveur spécifié en affectant des noms différents et en utilisant **sp_setnetname** pour définir le nom réseau de l’un d’eux sur le nom réseau du serveur sous-jacent.  
  
```  
--Assume sqlserv2 is actual name of SQL Server   
--database server  
EXEC sp_addlinkedserver 'sqlserv2';  
GO  
EXEC sp_addserver 'rpcserv2';  
GO  
EXEC sp_setnetname 'rpcserv2', 'sqlserv2';  
```  
  
> [!NOTE]  
>  L’utilisation de **sp_setnetname** pour faire pointer un serveur lié vers le serveur local n’est pas prise en charge. Les serveurs référencés de cette manière ne peuvent pas participer à une transaction distribuée.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l’appartenance aux rôles serveur fixes **sysadmin** et **setupadmin** .  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant montre une séquence d'administration courante utilisée sur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour effectuer des appels de procédures stockées distantes.  
  
```  
USE master;  
GO  
EXEC sp_addserver 'Win_1';  
EXEC sp_setnetname 'Win_1','Win-1';  
EXEC Win_1.master.dbo.sp_who;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Moteur de base de données des procédures stockées &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [sp_addlinkedserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md)   
 [sp_addserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addserver-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
