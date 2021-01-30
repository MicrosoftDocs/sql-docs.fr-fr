---
description: xp_logininfo (Transact-SQL)
title: xp_logininfo (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- xp_logininfo_TSQL
- xp_logininfo
dev_langs:
- TSQL
helpviewer_keywords:
- xp_logininfo
ms.assetid: ee7162b5-e11f-4a0e-a09c-1878814dbbbd
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 6df69edd25c5c2f451e8a4aa657caf99a4c8103d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99124107"
---
# <a name="xp_logininfo-transact-sql"></a>xp_logininfo (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Retourne des informations sur les utilisateurs Windows et les groupes Windows.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
xp_logininfo [ [ @acctname = ] 'account_name' ]   
     [ , [ @option = ] 'all' | 'members' ]   
     [ , [ @privilege = ] variable_name OUTPUT]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @acctname = ] 'account_name'` Nom d’un utilisateur ou groupe Windows autorisé à accéder à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . *account_name* est de **type sysname**, avec NULL comme valeur par défaut. Si *account_name* n’est pas spécifié, tous les groupes Windows et utilisateurs Windows auxquels une autorisation de connexion a été explicitement accordée sont signalés. *account_name* doit être complet. Par exemple, « ADVWKS4\macraes » ou « BUILTIN\Administrators ».  
  
 **'all'**  |  **'Members'**  
 Spécifie s'il faut rapporter les informations sur tous les chemins d'autorisation du compte ou sur les membres du groupe Windows. l' **\@ option** est de type **varchar (10)**, avec NULL comme valeur par défaut. Sauf si **All** est spécifié, seul le premier chemin d’accès est affiché.  
  
`[ @privilege = ] variable_name` Paramètre de sortie qui retourne le niveau de privilège du compte Windows spécifié. *variable_name* est de type **varchar (10)**, avec « not recherché » comme valeur par défaut. Le niveau de privilège retourné est **User**, **admin** ou **null**.  
  
 OUTPUT  
 En cas de spécification, place *variable_name* dans le paramètre de sortie.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**nom du compte**|**sysname**|Nom de compte Windows complet.|  
|**type**|**Char (8)**|Type de compte Windows. Les valeurs valides sont **User** ou **Group**.|  
|**limités**|**char(9)**|Privilège d'accès à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Les valeurs valides sont **admin**, **User** ou **null**.|  
|**mapped login name**|**sysname**|Pour les comptes d’utilisateur dotés de privilèges utilisateur, le **nom de connexion mappé** indique le nom de connexion mappé qui [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tente d’être utilisé lors de la connexion avec ce compte à l’aide des règles mappées avec le nom de domaine ajouté avant.|  
|**permission path**|**sysname**|Membre du groupe qui autorise l'accès au compte|  
  
## <a name="remarks"></a>Notes  
 Si *account_name* est spécifié, **xp_logininfo** indique le niveau de privilège le plus élevé du groupe ou de l’utilisateur Windows spécifié. Si un utilisateur Windows dispose d'autorisations d'accès en tant qu'administrateur système et utilisateur de domaine, il est indiqué comme administrateur système. Si l'utilisateur est membre de plusieurs groupes Windows dotés des mêmes niveaux de privilèges, seul le groupe qui a obtenu le premier l'accès à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est indiqué.  
  
 Si *account_name* est un utilisateur ou un groupe Windows valide qui n’est pas associé à une [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] connexion, un jeu de résultats vide est retourné. Si *account_name* ne peut pas être identifié comme un utilisateur ou un groupe Windows valide, un message d’erreur est retourné.  
  
 Si *account_name* et **All** sont spécifiés, tous les chemins d’autorisation pour l’utilisateur ou le groupe Windows sont retournés. Si *account_name* est membre de plusieurs groupes, tous bénéficiant d’un accès à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , plusieurs lignes sont retournées. Les lignes des privilèges d' **administrateur** sont retournées avant les lignes de privilège de l' **utilisateur** et, dans un niveau de privilège, les lignes sont retournées dans l’ordre dans lequel les connexions correspondantes [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ont été créées.  
  
 Si *account_name* et **les membres** sont spécifiés, une liste des membres de niveau suivant du groupe est retournée. Si *account_name* est un groupe local, la liste peut inclure des utilisateurs locaux, des utilisateurs de domaine et des groupes. Si *account_name* est un compte de domaine, la liste est composée d’utilisateurs de domaine. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doit se connecter au contrôleur de domaine pour extraire les informations d'appartenance au groupe. Si le serveur ne peut pas contacter le contrôleur de domaine, aucune information n'est renvoyée.  
  
 **xp_logininfo** renvoie uniquement les informations des groupes globaux Active Directory, et non des groupes universels.  
  
## <a name="permissions"></a>Autorisations  
 Requiert l’appartenance au rôle serveur fixe **sysadmin** ou l’appartenance au rôle de base de données fixe **public** dans la base de données **Master** avec l’autorisation EXECUTE accordée.  
  
## <a name="examples"></a>Exemples  
 L’exemple suivant affiche des informations sur le `BUILTIN\Administrators` groupe Windows.  
  
```  
EXEC xp_logininfo 'BUILTIN\Administrators';  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sp_denylogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-denylogin-transact-sql.md)   
 [sp_grantlogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-grantlogin-transact-sql.md)   
 [sp_revokelogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-revokelogin-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Procédures stockées étendues générales &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/general-extended-stored-procedures-transact-sql.md)  
  
  
