---
description: CREATE SECURITY POLICY (Transact-SQL)
title: CREATE SECURITY POLICY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/10/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SECURITY_POLICY_TSQL
- CREATE SECURITY
- SECURITY
- CREATE_SECURITY_POLICY_TSQL
- CREATE_SECURITY_TSQL
- SECURITY POLICY
- SECURITY_TSQL
- CREATE SECURITY POLICY
dev_langs:
- TSQL
helpviewer_keywords:
- RLS
- CREATE SECURITY POLICY statement
- Row-Level Security
ms.assetid: d6ab70ee-0fa2-469c-96f6-a3c16d673bc8
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: ddebebc1a204af546734425ffd76781948cd82de
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99188485"
---
# <a name="create-security-policy-transact-sql"></a>CREATE SECURITY POLICY (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

  Crée une stratégie de sécurité pour la sécurité au niveau des lignes.  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
CREATE SECURITY POLICY [schema_name. ] security_policy_name    
    { ADD [ FILTER | BLOCK ] } PREDICATE tvf_schema_name.security_predicate_function_name   
      ( { column_name | expression } [ , ...n] ) ON table_schema_name. table_name    
      [ <block_dml_operation> ] , [ , ...n] 
    [ WITH ( STATE = { ON | OFF }  [,] [ SCHEMABINDING = { ON | OFF } ] ) ]  
    [ NOT FOR REPLICATION ] 
[;]  
  
<block_dml_operation>  
    [ { AFTER { INSERT | UPDATE } }   
    | { BEFORE { UPDATE | DELETE } } ]  
```  

## <a name="arguments"></a>Arguments
 *security_policy_name*  
 Nom de la stratégie de sécurité. Les noms de stratégie de sécurité doivent respecter les règles applicables aux identificateurs et doivent être uniques dans la base de données et pour son schéma.  
  
 *schema_name*  
 Nom du schéma auquel appartient la stratégie de sécurité. *schema_name* est obligatoire en raison de la liaison de schéma.  
  
 [ FILTER | BLOCK ]  
 Type de prédicat de sécurité pour la fonction liée à la table cible. Les prédicats FILTER filtrent de manière silencieuse les lignes disponibles pour les opérations de lecture. Les prédicats BLOCK bloquent de façon explicite les opérations d’écriture qui violent la fonction de prédicat.  
  
 *tvf_schema_name.security_predicate_function_name*  
 Fonction de valeur de table inline à utiliser comme prédicat et à appliquer dans les requêtes portant sur une table cible. Au plus un prédicat de sécurité peut être défini pour une opération DML particulière sur une table donnée. La fonction de valeur de table inline doit avoir été créée à l'aide de l'option SCHEMABINDING.  
  
 { *column_name* | *expression* }  
 Nom de la colonne ou expression servant de paramètre dans la fonction de prédicat de sécurité. N’importe quelle colonne de la table cible peut être utilisée. Une [expression](../../t-sql/language-elements/expressions-transact-sql.md) peut uniquement contenir des constantes, des fonctions scalaires intégrées, des opérateurs et des colonnes provenant de la table cible. Vous devez spécifier un nom de colonne ou une expression pour chaque paramètre de la fonction.  
  
 *table_schema_name.table_name*  
 Table cible à laquelle sera appliqué le prédicat de sécurité. Plusieurs stratégies de sécurité désactivées peuvent cibler une même table pour une opération DML particulière, mais une seule peut être activée à un moment donné.  
  
 *\<block_dml_operation>* Opération DML particulière pour laquelle le prédicat BLOCK est appliqué. AFTER spécifie que le prédicat est évalué sur les valeurs des lignes après l’exécution de l’opération DML (INSERT ou UPDATE). AFTER spécifie que le prédicat est évalué sur les valeurs des lignes avant l’exécution de l’opération DML (UPDATE ou DELETE). Si aucune opération n’est spécifiée, le prédicat s’applique à toutes les opérations.  
  
 [ STATE = { **ON** | OFF } ]  
 Permet à la stratégie de sécurité d'appliquer ses prédicats de sécurité sur les tables cibles, ou l'empêche d'effectuer cette opération. Sauf indication contraire, la stratégie de sécurité en cours de création est activée.  
  
 [ SCHEMABINDING = { **ON** | OFF } ]  
 Indique si toutes les fonctions de prédicat de la stratégie doivent être créées avec l’option SCHEMABINDING. Par défaut, ce paramètre est défini sur **ON** et toutes les fonctions doivent être créées avec SCHEMABINDING.  
  
 NOT FOR REPLICATION  
 Indique que la stratégie de sécurité ne doit pas être exécutée quand un agent de réplication modifie l'objet cible. Pour plus d’informations, consultez [Contrôler le comportement de déclencheurs et de contraintes au cours de la synchronisation &#40;programmation Transact-SQL de la réplication&#41;](../../relational-databases/replication/control-behavior-of-triggers-and-constraints-in-synchronization.md).  
  
 [*table_schema_name*.] *table_name*  
 Table cible à laquelle sera appliqué le prédicat de sécurité. Plusieurs stratégies de sécurité désactivées peuvent cibler une même table, mais une seule peut être activée à un moment donné.  
  

## <a name="remarks"></a>Notes
 Quand vous utilisez des fonctions de prédicat avec des tables optimisées en mémoire, vous devez inclure **SCHEMABINDING** et utiliser l’indicateur de compilation **WITH NATIVE_COMPILATION**.  
  
 Les prédicats BLOCK sont évalués après l’exécution de l’opération DML correspondante. Par conséquent, une requête READ UNCOMMITTED peut voir les valeurs temporaires qui seront restaurées.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation ALTER ANY SECURITY POLICY et l'autorisation ALTER sur le schéma.  
  
 En outre, les autorisations suivantes sont requises pour chaque prédicat ajouté :  
  
-   Les autorisations SELECT et REFERENCES sur la fonction utilisée en tant que prédicat  
  
-   L'autorisation REFERENCES sur la table cible liée à la stratégie  
  
-   L'autorisation REFERENCES sur chaque colonne de la table cible utilisée comme arguments  
  
## <a name="examples"></a>Exemples  
 Les exemples suivants illustrent l'utilisation de la syntaxe **CREATE SECURITY POLICY** . Pour obtenir un exemple de scénario de stratégie de sécurité complet, consultez [Sécurité au niveau des lignes](../../relational-databases/security/row-level-security.md).  
  
### <a name="a-creating-a-security-policy"></a>R. Création d'une stratégie de sécurité  
 La syntaxe suivante crée une stratégie de sécurité avec un prédicat de filtre pour la table Customer et laisse la stratégie de sécurité désactivée.  
  
```  
CREATE SECURITY POLICY [FederatedSecurityPolicy]   
ADD FILTER PREDICATE [rls].[fn_securitypredicate]([CustomerId])   
ON [dbo].[Customer];  
```  
  
### <a name="b-creating-a-policy-that-affects-multiple-tables"></a>B. Création d'une stratégie qui affecte plusieurs tables  
 La syntaxe suivante crée une stratégie de sécurité avec trois prédicats de filtre sur trois tables différentes et active la stratégie de sécurité.  
  
```sql  
CREATE SECURITY POLICY [FederatedSecurityPolicy]   
ADD FILTER PREDICATE [rls].[fn_securitypredicate1]([CustomerId])   
    ON [dbo].[Customer],  
ADD FILTER PREDICATE [rls].[fn_securitypredicate1]([VendorId])   
    ON [dbo].[ Vendor],  
ADD FILTER PREDICATE [rls].[fn_securitypredicate2]([WingId])   
    ON [dbo].[Patient]  
WITH (STATE = ON);  
```  
  
### <a name="c-creating-a-policy-with-multiple-types-of-security-predicates"></a>C. Création d’une stratégie avec plusieurs types de prédicats de sécurité  
 Ajout d’un prédicat FILTER et d’un prédicat BLOCK à la table Sales.  
  
```sql  
CREATE SECURITY POLICY rls.SecPol  
    ADD FILTER PREDICATE rls.tenantAccessPredicate(TenantId) ON dbo.Sales,  
    ADD BLOCK PREDICATE rls.tenantAccessPredicate(TenantId) ON dbo.Sales AFTER INSERT;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Sécurité au niveau des lignes](../../relational-databases/security/row-level-security.md)   
 [ALTER SECURITY POLICY &#40;Transact-SQL&#41;](../../t-sql/statements/alter-security-policy-transact-sql.md)   
 [DROP SECURITY POLICY &#40;Transact-SQL&#41;](../../t-sql/statements/drop-security-policy-transact-sql.md)   
 [sys.security_policies &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-security-policies-transact-sql.md)   
 [sys.security_predicates &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-security-predicates-transact-sql.md)  
  
  

