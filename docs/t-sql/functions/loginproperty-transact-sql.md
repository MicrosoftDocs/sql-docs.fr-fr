---
description: LOGINPROPERTY (Transact-SQL)
title: LOGINPROPERTY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- BadPasswordCount_TSQL
- BadPasswordTime_TSQL
- IsLockedIsMustChange
- PasswordLastSetTime
- LOGINPROPERTY_TSQL
- LockoutTime
- BadPasswordCount
- PasswordHash
- HistoryLengthIsExpired
- LockoutTime_TSQL
- PasswordHash_TSQL
- HistoryLengthIsExpired_TSQL
- PasswordLastSetTime_TSQL
- BadPasswordTime
- IsLockedIsMustChange_TSQL
- LOGINPROPERTY
dev_langs:
- TSQL
helpviewer_keywords:
- default database
- LOGINPROPERTY function
ms.assetid: b34df777-79b0-49a5-88db-b99998479a5d
author: VanMSFT
ms.author: vanto
monikerRange: = azuresqldb-current || = azuresqldb-mi-current || >= sql-server-2016 || >= sql-server-linux-2017||=azure-sqldw-latest
ms.openlocfilehash: c31812d98ddb8315adfe86c17f880e757ce25197
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/04/2021
ms.locfileid: "101836913"
---
# <a name="loginproperty-transact-sql"></a>LOGINPROPERTY (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa.md)]

  Retourne des informations sur les paramètres de stratégie de connexion.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
LOGINPROPERTY ( 'login_name' , 'property_name' )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *login_name*  
 Nom d'une connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour laquelle l'état des propriétés de connexion est retourné.  
  
 *propertyname*  
 Expression contenant les informations de propriétés à retourner pour le compte de connexion. *propertyname* peut avoir l’une des valeurs suivantes.  
  
|Value|Description|  
|-----------|-----------------|  
|**BadPasswordCount**|Retourne le nombre de tentatives de connexion consécutives effectuées avec un mot de passe incorrect.|  
|**BadPasswordTime**|Retourne l'heure de la dernière tentative de connexion effectuée avec un mot de passe incorrect.|  
|**DaysUntilExpiration**|Retourne le nombre de jours avant l'expiration du mot de passe.|  
|**DefaultDatabase**|Retourne la base de données par défaut de la connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] telle que stockée dans les métadonnées ou **master** si aucune base de données n’est spécifiée. Retourne NULL pour les utilisateurs fournis non-[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (par exemple, les utilisateurs authentifiés Windows).|  
|**DefaultLanguage**|Retourne la langue par défaut de connexion telle que stockée dans les métadonnées. Retourne NULL pour les utilisateurs fournis non-[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], par exemple les utilisateurs authentifiés Windows.|  
|**HistoryLength**|Retourne le nombre de mots de passe faisant l'objet d'un suivi pour le compte de connexion à l'aide du mécanisme d'application des stratégies de mot de passe. Si la valeur est 0, la stratégie de mot de passe n'est pas appliquée. La reprise de l'application de la stratégie du mot de passe redémarre à 1.|  
|**IsExpired**|Indique si le compte de connexion a expiré.|  
|**IsLocked**|Indique si le compte de connexion est verrouillé.|  
|**IsMustChange**|Indique si le compte de connexion doit modifier son mot de passe lors de la prochaine connexion.|  
|**LockoutTime**|Retourne la date à laquelle la connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a été verrouillée en raison du dépassement du nombre de tentatives de connexion autorisé.|  
|**PasswordHash**|Retourne le hachage du mot de passe.|  
|**PasswordLastSetTime**|Retourne la date à laquelle le mot de passe actuel a été défini.|  
|**PasswordHashAlgorithm**|Retourne l'algorithme utilisé pour hacher le mot de passe.|  
  
## <a name="returns"></a>Retours  
 Le type de données dépend de la valeur demandée.  
  
 **IsLocked**, **IsExpired** et **IsMustChange** sont de type **int**.  
  
-   1 si la connexion est dotée de l'état spécifié.  
  
-   0 si la connexion n'est pas dotée de l'état spécifié.  
  
 **BadPasswordCount** et **HistoryLength** sont de type **int**.  
  
 **BadPasswordTime**, **LockoutTime**, **PasswordLastSetTime** sont de type **datetime**.  
  
 **PasswordHash** est de type **varbinary**.  
  
 NULL si la connexion n'est pas une connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] valide.  
  
 **DaysUntilExpiration** est de type **int**.  
  
-   0 si la connexion a expiré ou si elle expirera le jour de l'interrogation.  
  
-   -1 si la stratégie de sécurité locale dans Windows ne fait jamais expirer le mot de passe.  
  
-   NULL si CHECK_POLICY ou CHECK_EXPIRATION a la valeur OFF pour une connexion, ou si le système d'exploitation ne prend pas en charge la stratégie de mot de passe.  
  
 **PasswordHashAlgorithm** est de type int.  
  
-   0 pour un hachage SQL7.0  
  
-   1 pour un hachage SHA-1  
  
-   2 pour un hachage SHA-2  
  
-   NULL si le compte de connexion n'est pas un compte de connexion SQL Server valide.  
  
## <a name="remarks"></a>Notes  
 Cette fonction intégrée retourne des informations sur les paramètres de stratégie de mot de passe d'une connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Étant donné que les noms des propriétés ne respectent pas la casse, les noms de propriété tels que **BadPasswordCount** et **badpasswordcount** sont équivalents. Les valeurs des propriétés **PasswordHash, PasswordHashAlgorithm** et **PasswordLastSetTime** sont disponibles sur toutes les configurations de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prises en charge, mais les autres propriétés ne sont disponibles que si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est en cours d’exécution sous [!INCLUDE[winxpsvr](../../includes/winxpsvr-md.md)] et que CHECK_POLICY et CHECK_EXPIRATION sont tous les deux activés. Pour plus d'informations, consultez [Password Policy](../../relational-databases/security/password-policy.md).  
  
## <a name="permissions"></a>Autorisations  
 Requiert l'autorisation VIEW sur la connexion. Pour la demande du hachage de mot de passe, requiert en outre l'autorisation CONTROL SERVER.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-checking-whether-a-login-must-change-its-password"></a>R. Détermination de la nécessité de modifier le mot de passe d'un compte de connexion  
 L’exemple suivant vérifie si la connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]`John3` doit changer son mot de passe la prochaine fois qu’elle se connecte à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
```sql  
SELECT LOGINPROPERTY('John3', 'IsMustChange');  
GO  
```  
  
### <a name="b-checking-whether-a-login-is-locked-out"></a>B. Vérification du verrouillage d'une connexion  
 L'exemple suivant vérifie si la connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]`John3` est verrouillée.  
  
```sql  
SELECT LOGINPROPERTY('John3', 'IsLocked');  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [CREATE LOGIN &#40;Transact-SQL&#41;](../../t-sql/statements/create-login-transact-sql.md)   
 [sys.server_principals &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md)  
  
  
