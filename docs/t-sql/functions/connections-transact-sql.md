---
description: '&#x40;&#x40;CONNECTIONS (Transact-SQL)'
title: '@@CONNECTIONS (Transact-SQL) | Microsoft Docs'
ms.custom: ''
ms.date: 09/18/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- '@@CONNECTIONS'
- '@@CONNECTIONS_TSQL'
dev_langs:
- TSQL
helpviewer_keywords:
- '@@CONNECTIONS function'
- connections [SQL Server], number of
- connections [SQL Server], attempted
- number of connection attempts
- attempted connections
ms.assetid: c59836a8-443c-4b9a-8b96-8863ada97ac7
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 6bfa624000d0bcd036dbf75570d03db945430e31
ms.sourcegitcommit: cc23d8646041336d119b74bf239a6ac305ff3d31
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/23/2020
ms.locfileid: "91116113"
---
# <a name="x40x40connections-transact-sql"></a>&#x40;&#x40;CONNECTIONS (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Cette fonction renvoie le nombre de tentatives de connexion, réussies ou non, depuis le dernier démarrage de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
  
![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
@@CONNECTIONS  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Types de retour
**integer**
  
## <a name="remarks"></a>Remarques  
Les connexions sont différentes des utilisateurs. Une application, par exemple, peut très bien ouvrir plusieurs connexions au serveur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à l'insu de l'utilisateur.
  
Exécutez **sp_monitor** pour afficher un rapport contenant plusieurs statistiques [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], notamment le nombre de tentatives de connexion.
  
@@MAX_CONNECTIONS est le nombre maximal autorisé de connexions simultanées au serveur. @@CONNECTIONS est incrémenté à chaque tentative de connexion ; par conséquent, @@CONNECTIONS peut être supérieur à @@MAX_CONNECTIONS.
  
## <a name="examples"></a>Exemples  
Cet exemple retourne le nombre de tentatives de connexion à la date et à l’heure actuelles.
  
```sql
SELECT GETDATE() AS 'Today''s Date and Time',   
@@CONNECTIONS AS 'Login Attempts';  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
``` 
Today's Date and Time  Login Attempts  
---------------------- --------------  
12/5/2006 10:32:45 AM  211023         
```  
  
## <a name="see-also"></a>Voir aussi
[Fonctions statistiques système &#40;Transact-SQL&#41;](../../t-sql/functions/system-statistical-functions-transact-sql.md)  
[sp_monitor &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-monitor-transact-sql.md)
  
  
