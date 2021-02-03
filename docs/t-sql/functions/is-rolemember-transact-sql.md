---
description: IS_ROLEMEMBER (Transact-SQL)
title: IS_ROLEMEMBER (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- IS_ROLEMEMBER
- IS_ROLEMEMBER_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- roles [SQL Server], members
- IS_ROLEMEMBER function
- members [SQL Server], verifying
ms.assetid: 73efa688-ae91-4014-98bc-1cabe47321f7
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: fe767dab30645db9f34090e6182f60329f049b74
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208001"
---
# <a name="is_rolemember-transact-sql"></a>IS_ROLEMEMBER (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Indique si un principal de base de données spécifié est membre du rôle de base de données spécifié.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
IS_ROLEMEMBER ( 'role' [ , 'database_principal' ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 **'** *role* **'**  
 Nom du rôle de base de données vérifié. *role* est de type **sysname**.  
  
 **'** *database_principal* **'**  
 Nom de l'utilisateur de base de données, du rôle de base de données ou du rôle d'application à vérifier. *database_principal* est de type **sysname**, avec NULL comme valeur par défaut. Si aucune valeur n'est spécifiée, le résultat est basé sur le contexte d'exécution actuel. Si le paramètre contient le mot NULL, retourne NULL.  
  
## <a name="return-types"></a>Types de retour  
 **int**  
  
|Valeur retournée|Description|  
|------------------|-----------------|  
|0|*database_principal* n’est pas un membre de *role*.|  
|1|*database_principal* est un membre de *role*.|  
|NULL|*database_principal* ou *role* n’est pas valide, ou vous ne disposez pas de l’autorisation nécessaire pour afficher l’appartenance au rôle.|  
  
## <a name="remarks"></a>Remarques  
 Utilisez IS_ROLEMEMBER pour déterminer si l'utilisateur actuel peut réaliser une action nécessitant les autorisations du rôle de base de données.  
  
 Si *database_principal* est basé sur une connexion Windows, telle que Contoso\Mary5, IS_ROLEMEMBER retourne NULL, sauf si *database_principal* s’est vu accorder ou refuser l’accès direct à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Si le paramètre *database_principal* facultatif n’est pas fourni et si *database_principal* est basé sur une connexion de domaine Windows, il peut s’agir d’un membre d’un rôle de base de données via une appartenance à un groupe Windows. Pour résoudre de telles appartenances indirectes, IS_ROLEMEMBER demande des informations sur l'appartenance au groupe Windows à partir du contrôleur du domaine. Si le contrôleur du domaine est inaccessible ou ne répond pas, IS_ROLEMEMBER retourne des informations sur l'appartenance au rôle, en prenant uniquement en considération l'utilisateur et ses groupes locaux. Si l'utilisateur spécifié n'est pas l'utilisateur actuel, la valeur retournée par IS_ROLEMEMBER peut différer de la dernière mise à jour de données de l'authentificateur (par exemple Active Directory) sur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Si le paramètre *database_principal* facultatif est fourni, le principal de la base de données interrogé doit être présent dans sys.database_principals, sinon IS_ROLEMEMBER retournera NULL. Cela indique que *database_principal* n’est pas valide dans cette base de données.  
  
 Lorsque le paramètre *database_principal* est basé sur une connexion de domaine ou sur un groupe Windows et que le contrôleur de domaine n’est pas accessible, les appels à IS_ROLEMEMBER échouent et peuvent renvoyer des données incorrectes ou incomplètes.  
  
 Si le contrôleur de domaine n'est pas accessible, l'appel à IS_ROLEMEMBER retourne des informations exactes lorsque le principal Windows peut être authentifié localement, par exemple un compte Windows local ou un compte de connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 **IS_ROLEMEMBER** retourne toujours 0 lorsqu’un groupe Windows est utilisé comme argument de principal de base de données, et ce groupe Windows est membre d’un autre groupe Windows qui est, à son tour, membre du rôle de base de données spécifié.  
  
 Le contrôle de compte d’utilisateur trouvé dans [!INCLUDE[wiprlhext](../../includes/wiprlhext-md.md)] et Windows Server 2008 peut également renvoyer des résultats différents. Cela varie selon que l'utilisateur a accédé au serveur en tant que membre de groupe Windows ou en tant qu'utilisateur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] spécifique.  
  
 Cette fonction évalue l'appartenance au rôle, et non l'autorisation sous-jacente. Par exemple, le rôle de base de données fixe **db_owner** dispose de l’autorisation **CONTROL DATABASE**. Si l’utilisateur possède l’autorisation **CONTROL DATABASE**, mais n’est pas membre du rôle, cette fonction signale correctement que l’utilisateur n’est pas membre du rôle **db_owner**, bien qu’il dispose des mêmes autorisations.  
  
## <a name="related-functions"></a>Fonctions connexes  
 Pour déterminer si l’utilisateur actuel est membre du groupe Windows spécifié ou du rôle de base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], utilisez [IS_MEMBER &#40;Transact-SQL&#41;](../../t-sql/functions/is-member-transact-sql.md). Pour déterminer si une connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est membre d’un rôle de serveur, utilisez [IS_SRVROLEMEMBER &#40;Transact-SQL&#41;](../../t-sql/functions/is-srvrolemember-transact-sql.md).  
  
## <a name="permissions"></a>Autorisations  
 Requiert l'autorisation VIEW DEFINITION sur le rôle de base de données.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant indique si l'utilisateur actuel est membre du rôle de base de données fixe `db_datareader`.  
  
```sql  
IF IS_ROLEMEMBER ('db_datareader') = 1  
   print 'Current user is a member of the db_datareader role'  
ELSE IF IS_ROLEMEMBER ('db_datareader') = 0  
   print 'Current user is NOT a member of the db_datareader role'  
ELSE IF IS_ROLEMEMBER ('db_datareader') IS NULL  
   print 'ERROR: The database role specified is not valid.';  
```  
  
## <a name="see-also"></a>Voir aussi  
 [CREATE ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-role-transact-sql.md)   
 [ALTER ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-role-transact-sql.md)   
 [DROP ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-role-transact-sql.md)   
 [CREATE SERVER ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-server-role-transact-sql.md)   
 [ALTER SERVER ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-server-role-transact-sql.md)   
 [DROP SERVER ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-server-role-transact-sql.md)   
 [IS_MEMBER &#40;Transact-SQL&#41;](../../t-sql/functions/is-member-transact-sql.md)   
 [IS_SRVROLEMEMBER &#40;Transact-SQL&#41;](../../t-sql/functions/is-srvrolemember-transact-sql.md)   
 [Fonctions de sécurité &#40;Transact-SQL&#41;](../../t-sql/functions/security-functions-transact-sql.md)  
  
  
