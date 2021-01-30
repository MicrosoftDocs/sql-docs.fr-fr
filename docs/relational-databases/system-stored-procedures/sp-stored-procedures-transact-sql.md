---
description: sp_stored_procedures (Transact-SQL)
title: sp_stored_procedures (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_stored_procedures_TSQL
- sp_stored_procedures
dev_langs:
- TSQL
helpviewer_keywords:
- sp_stored_procedures
ms.assetid: fe52dd83-000a-4665-83fb-7a0024193dec
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 3266b5619701d28a4fc65eb79d3f3e16c75e40b6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193221"
---
# <a name="sp_stored_procedures-transact-sql"></a>sp_stored_procedures (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Renvoie la liste des procédures stockées dans l'environnement actuel.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_stored_procedures [ [ @sp_name = ] 'name' ]   
    [ , [ @sp_owner = ] 'schema']   
    [ , [ @sp_qualifier = ] 'qualifier' ]  
    [ , [@fUsePattern = ] 'fUsePattern' ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @sp_name = ] 'name'` Nom de la procédure utilisée pour retourner les informations de catalogue. *Name* est de type **nvarchar (390)**, avec NULL comme valeur par défaut. La recherche de correspondance avec des caractères génériques est prise en charge.  
  
`[ @sp_owner = ] 'schema'` Nom du schéma auquel appartient la procédure. *Schema* est de type **nvarchar (384)**, avec NULL comme valeur par défaut. La recherche de correspondance avec des caractères génériques est prise en charge. Si *owner* n’est pas spécifié, les règles de visibilité des procédures par défaut du SGBD sous-jacent s’appliquent.  
  
 Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], si le schéma actif contient une procédure avec le nom spécifié, celle-ci est renvoyée. Si une procédure stockée non qualifiée est spécifiée, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] la recherche dans l'ordre suivant :  
  
-   Schéma **sys** de la base de données active.  
  
-   Le schéma par défaut de l'appelant est exécuté dans un traitement ou dans des instructions SQL dynamiques ; ou bien, si le nom de la procédure non qualifiée figure dans le corps d'une autre définition de procédure, le schéma qui contient cette procédure est ensuite recherché.  
  
-   Schéma **dbo** dans la base de données active.  
  
`[ @qualifier = ] 'qualifier'` Nom du qualificateur de la procédure. *qualifier* est de **type sysname**, avec NULL comme valeur par défaut. Divers produits SGBD prennent en charge les noms de tables en trois parties dans le formulaire (_qualificateur_**.** _schéma_**.** _nom_. Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , le *qualificateur* représente le nom de la base de données. Dans certains produits, elle représente le nom du serveur de l'environnement de base de données de la table.  
  
`[ @fUsePattern = ] 'fUsePattern'` Détermine si le trait de soulignement (_), le pourcentage (%) ou les crochets []) sont interprétés comme des caractères génériques. *fUsePattern* est de **bits**, avec 1 comme valeur par défaut.  
  
 **0** = les critères spéciaux sont désactivés.  
  
 **1** = les critères spéciaux sont activés.  
  
## <a name="return-code-values"></a>Codet de retour  
 Aucun  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**PROCEDURE_QUALIFIER**|**sysname**|Nom du qualificateur de procédure. Cette colonne peut être NULL.|  
|**PROCEDURE_OWNER**|**sysname**|Nom du propriétaire de la procédure. Cette colonne renvoie toujours une valeur.|  
|**PROCEDURE_NAME**|**nvarchar (134)**|Nom de la procédure. Cette colonne renvoie toujours une valeur.|  
|**NUM_INPUT_PARAMS**|**int**|Réservé pour un usage futur.|  
|**NUM_OUTPUT_PARAMS**|**int**|Réservé pour un usage futur.|  
|**NUM_RESULT_SETS**|**int**|Réservé pour un usage futur.|  
|**NOTES**|**varchar (254)**|Description de la procédure. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne retourne pas de valeur pour cette colonne.|  
|**PROCEDURE_TYPE**|**smallint**|Type de procédure. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] retourne toujours 2.0. Cette valeur peut être l'une des suivantes :<br /><br /> 0 = SQL_PT_UNKNOWN<br /><br /> 1 = SQL_PT_PROCEDURE<br /><br /> 2 = SQL_PT_FUNCTION|  
  
## <a name="remarks"></a>Notes  
 Pour une interopérabilité maximale, le client de la passerelle doit utiliser seulement les recherches de correspondance avec des caractères génériques de SQL (caractères génériques % et _).  
  
 Les informations d'autorisations relatives à l'exécution par l'utilisateur actuel d'une procédure stockée particulière ne sont pas nécessairement vérifiées ; l'accès n'est donc pas garanti. Notez que la dénomination en trois parties est utilisée. Cela signifie que seules les procédures stockées locales, et non pas les procédures stockées distantes (qui utilisent la dénomination en quatre parties), sont renvoyées dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si l’attribut de serveur ACCESSIBLE_SPROC est Y dans le jeu de résultats pour **sp_server_info**, seules les procédures stockées qui peuvent être exécutées par l’utilisateur actuel sont retournées.  
  
 **sp_stored_procedures** est équivalent à **SQLProcedures** dans ODBC. Les résultats retournés sont triés par **PROCEDURE_QUALIFIER**, **PROCEDURE_OWNER** et **PROCEDURE_NAME**.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation SELECT sur le schéma.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-returning-all-stored-procedures-in-the-current-database"></a>R. Renvoi de toutes les procédures stockées présentes dans la base de données active  
 L'exemple suivant retourne toutes les procédures stockées contenues dans la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_stored_procedures;  
```  
  
### <a name="b-returning-a-single-stored-procedure"></a>B. Renvoi d'une procédure stockée unique  
 L'exemple suivant retourne un jeu de résultats pour la procédure stockée `uspLogError`.  
  
```  
USE AdventureWorks2012;  
GO  
sp_stored_procedures N'uspLogError', N'dbo', N'AdventureWorks2012', 1;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées de catalogue &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/catalog-stored-procedures-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
