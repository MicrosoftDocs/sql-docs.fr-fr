---
description: '&#x40;&#x40;SERVERNAME (Transact-SQL)'
title: '@@SERVERNAME (Transact-SQL) | Microsoft Docs'
ms.custom: ''
ms.date: 09/07/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- '@@SERVERNAME'
- '@@SERVERNAME_TSQL'
dev_langs:
- TSQL
helpviewer_keywords:
- '@@SERVERNAME function'
- local servers [SQL Server]
ms.assetid: b0ef33fb-954a-4294-b05b-a87c14ce25a3
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 167f21cdc1f2a1e0d5558d1d95cd42005a031bd1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191102"
---
# <a name="x40x40servername-transact-sql"></a>&#x40;&#x40;SERVERNAME (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Renvoie le nom du serveur local qui exécute [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
 ![Icône Lien d’article](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
@@SERVERNAME  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Types de retour
 **nvarchar**  
  
## <a name="remarks"></a>Notes  
 Lors de l'installation, le programme d'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] définit le nom du serveur avec le nom de l'ordinateur. Pour modifier le nom du serveur, utilisez **sp_addserver**, puis redémarrez [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Quand plusieurs instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont installées, @@SERVERNAME retourne les informations relatives au nom du serveur local suivantes si le nom du serveur local n’a pas changé depuis l’installation.  
  
|Instance|Informations sur le serveur|  
|--------------|------------------------|  
|Instance par défaut|'*nom_serveur*'|  
|Instance nommée|'*nom_serveur*\\*nom_instance*'|  
|instance de cluster de basculement - instance par défaut|'*network_name_for_fci_in_wsfc*'|  
|instance de cluster de basculement - instance nommée|'*network_name_for_fci_in_wsfc*\\*nominstance*'|  
  
 Bien que la fonction @@SERVERNAME et la propriété SERVERNAME de la fonction SERVERPROPERTY puissent renvoyer des chaînes de mêmes formats, les informations peuvent être différentes. La propriété SERVERNAME rapporte automatiquement les modifications apportées au nom réseau de l'ordinateur.  
  
 En revanche, @@SERVERNAME ne signale pas les modifications de ce type. @@SERVERNAME signale les modifications apportées au nom du serveur local à l'aide de la procédure stockée **sp_addserver** ou **sp_dropserver**.  
  
## <a name="examples"></a>Exemples  
 L’exemple suivant montre l’utilisation de `@@SERVERNAME`.  
  
```sql  
SELECT @@SERVERNAME AS 'Server Name'  
```  
  
 Un exemple d'ensemble de résultats est présenté ci-dessous.  
  
```  
Server Name  
---------------------------------  
ACCTG  
  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions de configuration &#40;Transact-SQL&#41;](../../t-sql/functions/configuration-functions-transact-sql.md)   
 [SERVERPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/serverproperty-transact-sql.md)   
 [sp_addserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addserver-transact-sql.md)  
  
  
