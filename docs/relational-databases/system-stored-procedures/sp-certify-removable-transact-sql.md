---
description: sp_certify_removable (Transact-SQL)
title: sp_certify_removable (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_certify_removable_TSQL
- sp_certify_removable
dev_langs:
- TSQL
helpviewer_keywords:
- sp_certify_removable
ms.assetid: ca12767f-0ae5-4652-b523-c23473f100a1
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 69a0c6e1331144fc340ad15bb103e593d3ba8f65
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99197675"
---
# <a name="sp_certify_removable-transact-sql"></a>sp_certify_removable (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Vérifie qu'une base de données est correctement configurée pour la distribution sur support de sauvegarde amovible et rend compte à l'utilisateur des éventuels problèmes.  
  
> **IMPORTANT** [! INCLUez à la place[ssNoteDepFutureAvoid](../../t-sql/statements/create-database-transact-sql.md) .  
  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_certify_removable [ @dbname= ] 'dbname'  
     [ , [ @autofix = ] 'auto' ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @dbname = ] 'dbname'` Spécifie la base de données à vérifier. *dbname* est de **type sysname**.  
  
`[ @autofix = ] 'auto'` Donne la propriété de la base de données et de tous les objets de base de données à l’administrateur système, et supprime les utilisateurs de base de données créés par l’utilisateur et les autorisations non définies par défaut. *auto* est de type **nvarchar (4)**, avec NULL comme valeur par défaut.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="remarks"></a>Notes  
 Si la base de données est correctement configurée, **sp_certify_removable** effectue les opérations suivantes :  
  
-   Elle met la base de données hors ligne pour permettre la copie des fichiers.  
  
-   Elle met à jour les statistiques de toutes les tables et rend compte des éventuels problèmes de propriété et d'utilisateur.  
  
-   Elle marque également les groupes de fichiers de données en lecture seule pour que ces fichiers puissent être copiés sur des supports en lecture seule.  
  
 L'administrateur système doit être le propriétaire de la base de données et de tous les objets de la base de données. L’administrateur système est un utilisateur connu qui existe sur tous les serveurs qui exécutent [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et peut être censé exister lorsque la base de données est distribuée et installée ultérieurement.  
  
 Si vous exécutez **sp_certify_removable** sans la valeur **auto** et qu’il retourne des informations sur l’une des conditions suivantes :  
  
-   L'administrateur système n'est pas le propriétaire de la base de données.  
  
-   Il existe des utilisateurs créés par l'utilisateur.  
  
-   L'administrateur système ne possède pas tous les objets de la base de données.  
  
-   Des autorisations non établies par défaut ont été accordées.  
  
 Vous pouvez y remédier des deux manières suivantes :  
  
-   Utilisez [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Outils et procédures, puis réexécutez **sp_certify_removable** .  
  
-   Exécutez simplement **sp_certify_removable** avec la valeur **auto** .  
  
 Remarquez que cette procédure stockée ne recherche que les utilisateurs et les autorisations des utilisateurs. Vous pouvez ajouter des groupes à la base de données et leur accorder des autorisations. Pour plus d’informations, consultez [GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-transact-sql.md).  
  
## <a name="permissions"></a>Autorisations  
 Les autorisations d’exécution sont limitées aux membres du rôle serveur fixe **sysadmin** .  
  
## <a name="examples"></a>Exemples  
 Cet exemple certifie que la base de données `inventory` est prête à être supprimée.  
  
```  
EXEC sp_certify_removable inventory, AUTO;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Attacher et détacher une base de données &#40;SQL Server&#41;](../../relational-databases/databases/database-detach-and-attach-sql-server.md)   
 [sp_create_removable &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-create-removable-transact-sql.md)   
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [sp_dbremove &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbremove-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
