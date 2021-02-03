---
description: IS_MEMBER (Transact-SQL)
title: IS_MEMBER (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- IS_MEMBER
- IS_MEMBER_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- database roles [SQL Server], members
- current member status
- roles [SQL Server], members
- testing member status
- members [SQL Server]
- checking member status
- IS_MEMBER function
- verifying member status
- groups [SQL Server], members
- members [SQL Server], verifying
ms.assetid: 77cb68a0-19b7-4fe1-ab17-e5587699631b
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: e938e2cfbe93c137ce24d77d7f942b1764f521be
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179665"
---
# <a name="is_member-transact-sql"></a>IS_MEMBER (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Indique si l'utilisateur actuel est membre du groupe [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows ou du rôle de base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] spécifié. La fonction IS_MEMBER n’est pas prise en charge pour les groupes Azure Active Directory.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
IS_MEMBER ( { 'group' | 'role' } )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 **'** *group* **'**  
**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et ultérieur
  
 Nom du groupe Windows en cours de vérification ; doit respecter le format *Domaine*\\*Groupe*. *group* est de type **sysname**.  
  
 **'** *role* **'**  
 Nom du rôle [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en cours de vérification. *role* est de type **sysname** et peut comprendre les rôles de base de données fixes ou définis par l’utilisateur, mais pas les rôles de serveur.  
  
## <a name="return-types"></a>Types de retour  
 **int**  
  
## <a name="remarks"></a>Notes  
 IS_MEMBER retourne les valeurs suivantes.  
  
|Valeur retournée|Description|  
|------------------|-----------------|  
|0|L’utilisateur actuel n’est membre ni de *group* ni de *role*.|  
|1|L’utilisateur actuel est membre de *group* ou de *role*.|  
|NULL|*group* ou *role* n’est pas valide. En cas d'interrogation par une connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou une connexion utilisant un rôle d'application, retourne NULL pour un groupe Windows.|  
  
 IS_MEMBER détermine l'appartenance au groupe Windows en examinant un jeton d'accès créé par Windows. Le jeton d'accès ne reflète pas les modifications apportées à l'appartenance au groupe après la connexion d'un utilisateur à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. L’appartenance au groupe Windows ne peut pas faire l’objet d’une requête par une connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou un rôle d’application [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Pour ajouter et supprimer des membres dans un rôle de base de données, utilisez [ALTER ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-role-transact-sql.md). Pour ajouter et supprimer des membres dans un rôle de serveur, utilisez [ALTER SERVER ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-server-role-transact-sql.md).  
  
 Cette fonction évalue l'appartenance au rôle, et non l'autorisation sous-jacente. Par exemple, le rôle de base de données fixe **db_owner** dispose de l’autorisation **CONTROL DATABASE**. Si l’utilisateur possède l’autorisation **CONTROL DATABASE**, mais n’est pas membre du rôle, cette fonction signale correctement que l’utilisateur n’est pas membre du rôle **db_owner**, bien qu’il dispose des mêmes autorisations.  
  
 Les membres du rôle de serveur fixe **sysadmin** se connectent comme utilisateur **dbo** dans toutes les bases de données. La vérification de l’autorisation pour le membre du rôle de serveur fixe **sysadmin** vérifie les autorisations pour **dbo**, et pas pour le compte de connexion d’origine. Étant donné que **dbo** ne peut pas être ajouté à un rôle de base de données et qu’il n’existe pas dans les groupes Windows, **dbo** retourne toujours 0 (ou NULL si le rôle n’existe pas).  
  
## <a name="related-functions"></a>Fonctions connexes  
 Pour déterminer si une autre connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est membre d’un rôle de base de données, utilisez [IS_ROLEMEMBER &#40;Transact-SQL&#41;](../../t-sql/functions/is-rolemember-transact-sql.md). Pour déterminer si une connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est membre d’un rôle de serveur, utilisez [IS_SRVROLEMEMBER &#40;Transact-SQL&#41;](../../t-sql/functions/is-srvrolemember-transact-sql.md).  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant vérifie si l'utilisateur actuel est membre d'un rôle de base de données ou d'un groupe de domaines Windows.  
  
```sql  
-- Test membership in db_owner and print appropriate message.  
IF IS_MEMBER ('db_owner') = 1  
   PRINT 'Current user is a member of the db_owner role'  
ELSE IF IS_MEMBER ('db_owner') = 0  
   PRINT 'Current user is NOT a member of the db_owner role'  
ELSE IF IS_MEMBER ('db_owner') IS NULL  
   PRINT 'ERROR: Invalid group / role specified';  
GO  
  
-- Execute SELECT if user is a member of ADVWORKS\Shipping.  
IF IS_MEMBER ('ADVWORKS\Shipping') = 1  
   SELECT 'User ' + USER + ' is a member of ADVWORKS\Shipping.';   
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [IS_SRVROLEMEMBER &#40;Transact-SQL&#41;](../../t-sql/functions/is-srvrolemember-transact-sql.md)   
 [Principaux &#40;moteur de base de données&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [Affichages catalogue de sécurité &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)   
 [Fonctions de sécurité &#40;Transact-SQL&#41;](../../t-sql/functions/security-functions-transact-sql.md)  
  
  
