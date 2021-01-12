---
description: ALTER TRIGGER (Transact-SQL)
title: ALTER TRIGGER (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/08/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ALTER TRIGGER
- ALTER_TRIGGER_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- DDL triggers, modifying
- triggers [SQL Server], modifying
- modifying triggers
- ALTER TRIGGER statement
- DML triggers, modifying
ms.assetid: 2a99c7c1-ac2f-4637-aa7c-3d1bf514e500
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 63cb1d939fc4c46c2382c369c66d79764f2d6211
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098446"
---
# <a name="alter-trigger-transact-sql"></a>ALTER TRIGGER (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Modifie la définition d'un déclencheur DML, DDL ou de connexion précédemment créé à l'aide de l'instruction CREATE TRIGGER. Les déclencheurs sont créés à l'aide de la commande CREATE TRIGGER. Ils peuvent être créés directement à partir d’instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] ou de méthodes d’assembly créées dans le CLR (Common Language Runtime) [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] et chargées dans une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d’informations sur les paramètres utilisés dans l’instruction ALTER TRIGGER, consultez [CREATE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/create-trigger-transact-sql.md).  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
-- SQL Server Syntax  
-- Trigger on an INSERT, UPDATE, or DELETE statement to a table or view (DML Trigger)  

ALTER TRIGGER schema_name.trigger_name   
ON  ( table | view )   
[ WITH <dml_trigger_option> [ ,...n ] ]  
 ( FOR | AFTER | INSTEAD OF )   
{ [ DELETE ] [ , ] [ INSERT ] [ , ] [ UPDATE ] }   
[ NOT FOR REPLICATION ]   
AS { sql_statement [ ; ] [ ...n ] | EXTERNAL NAME <method specifier>   
[ ; ] }   
  
<dml_trigger_option> ::=  
    [ ENCRYPTION ]  
    [ <EXECUTE AS Clause> ]  
  
<method_specifier> ::=  
    assembly_name.class_name.method_name  
  
-- Trigger on an INSERT, UPDATE, or DELETE statement to a table 
-- (DML Trigger on memory-optimized tables)  

ALTER TRIGGER schema_name.trigger_name   
ON  ( table  )   
[ WITH <dml_trigger_option> [ ,...n ] ]  
 ( FOR | AFTER )   
{ [ DELETE ] [ , ] [ INSERT ] [ , ] [ UPDATE ] }   
AS { sql_statement [ ; ] [ ...n ] }   
  
<dml_trigger_option> ::=  
    [ NATIVE_COMPILATION ]  
    [ SCHEMABINDING ]  
    [ <EXECUTE AS Clause> ]  
  
-- Trigger on a CREATE, ALTER, DROP, GRANT, DENY, REVOKE, 
-- or UPDATE statement (DDL Trigger)  
  
ALTER TRIGGER trigger_name   
ON { DATABASE | ALL SERVER }   
[ WITH <ddl_trigger_option> [ ,...n ] ]  
{ FOR | AFTER } { event_type [ ,...n ] | event_group }   
AS { sql_statement [ ; ] | EXTERNAL NAME <method specifier>   
[ ; ] }  
}   
  
<ddl_trigger_option> ::=  
    [ ENCRYPTION ]  
    [ <EXECUTE AS Clause> ]  
  
<method_specifier> ::=  
    assembly_name.class_name.method_name  
  
-- Trigger on a LOGON event (Logon Trigger)  

ALTER TRIGGER trigger_name   
ON ALL SERVER   
[ WITH <logon_trigger_option> [ ,...n ] ]  
{ FOR| AFTER } LOGON   
AS { sql_statement  [ ; ] [ ,...n ] | EXTERNAL NAME < method specifier >  
  [ ; ] }  
  
<logon_trigger_option> ::=  
    [ ENCRYPTION ]  
    [ EXECUTE AS Clause ]  
  
<method_specifier> ::=  
    assembly_name.class_name.method_name  
```  
  
```syntaxsql
-- Azure SQL Database Syntax   
-- Trigger on an INSERT, UPDATE, or DELETE statement to a table or view (DML Trigger)   
  
ALTER TRIGGER schema_name. trigger_name   
ON (table | view )   
 [ WITH <dml_trigger_option> [ ,...n ] ]   
 ( FOR | AFTER | INSTEAD OF )   
{ [ DELETE ] [ , ] [ INSERT ] [ , ] [ UPDATE ] }   
AS { sql_statement [ ; ] [...n ] }   
  
<dml_trigger_option> ::=   
    [ <EXECUTE AS Clause> ]   
  
-- Trigger on a CREATE, ALTER, DROP, GRANT, DENY, REVOKE, or UPDATE statement (DDL Trigger)   
  
ALTER TRIGGER trigger_name   
ON { DATABASE }   
 [ WITH <ddl_trigger_option> [ ,...n ] ]   
{ FOR | AFTER } { event_type [ ,...n ] | event_group }   
AS { sql_statement   
[ ; ] }  
}   
  
<ddl_trigger_option> ::=   
    [ <EXECUTE AS Clause> ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *schema_name*  
 Nom du schéma auquel appartient le déclencheur DML. La portée des déclencheurs DML se limite au schéma de la table ou de la vue sur laquelle ils sont créés. *schema**_name* est facultatif uniquement si le déclencheur DML et sa table ou vue correspondante appartiennent au schéma par défaut. Vous ne pouvez pas spécifier *schema_name* pour des déclencheurs DDL ou de connexion.  
  
 *trigger_name*  
 Déclencheur existant à modifier.  
  
 *table* | *view*  
 Table ou vue sur laquelle le déclencheur DML est exécuté. La spécification du nom qualifié complet de la table ou de la vue est facultative.  
  
 DATABASE  
 Applique l'étendue d'un déclencheur DDL à la base de données active. S’il est spécifié, le déclencheur est activé chaque fois qu’*event_type* ou *event_group* se produit dans la base de données active.  
  
 ALL SERVER  
 **S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.  
  
 Applique l'étendue d'un déclencheur DDL ou de connexion au serveur actif. S’il est spécifié, le déclencheur est activé chaque fois qu’*event_type* ou *event_group* se produit à un endroit quelconque sur le serveur actif.  
  
 WITH ENCRYPTION  
 **S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.  
  
 Chiffre les entrées de sys.syscomments sys.sql_modules contenant le texte de l’instruction ALTER TRIGGER. L'utilisation de l'argument WITH ENCRYPTION évite la publication du déclencheur dans le cadre de la réplication [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Il n'est pas possible de spécifier WITH ENCRYPTION pour les déclencheurs CLR.  
  
> [!NOTE]  
>  Si un déclencheur est créé en utilisant WITH ENCRYPTION, il doit être spécifié de nouveau dans l'instruction ALTER TRIGGER pour que cette option reste activée.  
  
 EXECUTE AS  
 Spécifie le contexte de sécurité dans lequel le déclencheur est exécuté. Vous permet de contrôler le compte utilisateur utilisé par l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour valider les autorisations sur tous les objets de la base de données référencés par le déclencheur.  
  
 Pour plus d’informations, consultez [Clause EXECUTE AS &#40;Transact-SQL&#41;](../../t-sql/statements/execute-as-clause-transact-sql.md).  
  
 NATIVE_COMPILATION  
 Indique que le déclencheur est compilé en mode natif.  
  
 Cette option est obligatoire pour les déclencheurs sur les tables optimisées en mémoire.  
  
 SCHEMABINDING  
 Garantit que les tables référencées par un déclencheur ne peuvent pas être supprimées ou modifiées.  
  
 Cette option est obligatoire pour les déclencheurs sur les tables optimisées en mémoire et n’est pas prise en charge pour les déclencheurs sur des tables traditionnelles.  
  
 AFTER  
 Spécifie que le déclencheur est activé uniquement après l'exécution normale de l'instruction SQL de déclenchement. Toutes les actions CASCADE et les vérifications des contraintes de référence doivent également avoir été exécutées sans erreur pour que ce déclencheur puisse être exécuté.  
  
 AFTER est la valeur par défaut, uniquement si le mot clé FOR est spécifié.  
  
 Les déclencheurs DML AFTER peuvent être définis uniquement sur des tables.  
  
 INSTEAD OF  
 Spécifie que le déclencheur est exécuté au lieu de l'instruction SQL de déclenchement, ce qui supplante les actions des instructions de déclenchement. Il n'est pas possible de spécifier INSTEAD OF pour des déclencheurs DDL ou de connexion.  
  
 Il est possible de définir au plus un déclencheur INSTEAD OF par instruction INSERT, UPDATE ou DELETE sur une table ou une vue. Vous pouvez cependant définir des vues sur des vues, où chaque vue a son propre déclencheur INSTEAD OF.  
  
 Les déclencheurs INSTEAD OF ne sont pas autorisés sur les vues créées à l'aide de WITH CHECK OPTION. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] génère une erreur si un déclencheur INSTEAD OF est ajouté à une vue pour laquelle l'option WITH CHECK OPTION a été spécifiée. L'utilisateur doit supprimer cette option à l'aide d'ALTER VIEW avant de définir le déclencheur INSTEAD OF.  
  
 { [ DELETE ] [ , ] [ INSERT ] [ , ] [ UPDATE ] } | { [INSERT ] [ , ] [ UPDATE ] }  
 Spécifie quelles instructions de modification des données activent le déclencheur DML lorsqu'elles sont exécutées sur cette table ou vue. Vous devez spécifier au moins une option. Vous pouvez combiner toutes les options dans n'importe quel ordre dans la définition du déclencheur. Si plusieurs options sont spécifiées, séparez-les par des virgules.  
  
 Dans le cas des déclencheurs INSTEAD OF, l'option DELETE n'est pas autorisée sur les tables dont la relation référentielle spécifie une action en cascade ON DELETE. De même, l'option UPDATE n'est pas autorisée sur les tables dont la relation référentielle spécifie une action en cascade ON UPDATE. Pour plus d’informations, consultez [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md).  
  
 *event_type*  
 Nom de l'événement du langage [!INCLUDE[tsql](../../includes/tsql-md.md)] qui, après l'exécution, provoque l'exécution d'un déclencheur DDL. Les événements valides pour les déclencheurs DDL sont répertoriés dans [Événements DDL](../../relational-databases/triggers/ddl-events.md).  
  
 *event_group*  
 Nom d'un groupe prédéfini d'événements du langage [!INCLUDE[tsql](../../includes/tsql-md.md)]. Le déclencheur DDL est activé après l’exécution de n’importe quel événement du langage [!INCLUDE[tsql](../../includes/tsql-md.md)] appartenant à *event_group*. Les groupes d’événements valides pour les déclencheurs DDL sont répertoriés dans [Groupes d’événements DDL](../../relational-databases/triggers/ddl-event-groups.md). Une fois l’exécution d’ALTER TRIGGER terminée, *event_group* fait aussi office de macro en ajoutant les types d’événements qu’il couvre à la vue de catalogue sys.trigger_events.  
  
 NOT FOR REPLICATION  
 **S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.  
  
 Indique que le déclencheur ne doit pas être exécuté lorsqu'un agent de réplication modifie la table impliquée dans le déclencheur.  
  
 *sql_statement*  
 Conditions et actions du déclencheur.  
  
 Pour les déclencheurs sur les tables optimisées en mémoire, la seule instruction *sql_statement* autorisée au niveau supérieur est un bloc ATOMIC. Le code T-SQL autorisé dans le bloc ATOMIC est limité par le code T-SQL autorisé dans les procédures natives.  
  
 EXTERNAL NAME \<method_specifier>  
 **S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.  
  
 Spécifie les méthodes d'un assembly à lier avec le déclencheur. La méthode ne doit prendre aucun argument et retourner une valeur vide. *class_name* doit être un identificateur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] valide et doit exister comme classe dans l’assembly avec une visibilité de l’assembly. La classe ne peut pas être imbriquée.  
  
## <a name="remarks"></a>Notes  
 Pour plus d’informations sur ALTER TRIGGER, consultez la section Notes dans [CREATE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/create-trigger-transact-sql.md).  
  
> [!NOTE]  
>  Les options EXTERNAL_NAME et ON_ALL_SERVER ne sont pas disponibles dans une base de données autonome.  
  
## <a name="dml-triggers"></a>Déclencheurs DML  
 ALTER TRIGGER gère manuellement les vues qui peuvent être mises à jour par le biais de déclencheurs INSTEAD OF sur des tables et des vues. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] applique ALTER TRIGGER de la même manière pour tous les types de déclencheurs (AFTER, INSTEAD-OF).  
  
 Vous pouvez spécifier le premier et le dernier déclencheur AFTER à exécuter sur une table à l'aide de sp_settriggerorder. Seuls le premier et le dernier déclencheur AFTER peuvent être spécifiés sur une table. S'il y a d'autres déclencheurs AFTER sur la même table, ils sont exécutés de manière aléatoire.  
  
 Si une instruction ALTER TRIGGER modifie un premier ou un dernier déclencheur, le premier ou le dernier attribut défini sur le déclencheur modifié est supprimé et la valeur du rang d'exécution doit être réinitialisée avec sp_settriggerorder.  
  
 Un déclencheur AFTER est exécuté seulement après que l'instruction SQL de déclenchement se soit exécutée correctement. Cette exécution réussie inclut toutes les actions d'intégrité référentielle en cascade et les vérifications des contraintes associées à l'objet mis à jour ou supprimé. L'opération du déclencheur AFTER vérifie les effets de l'instruction de déclenchement, ainsi que toutes les actions UPDATE et DELETE référentielles en cascade générées par cette dernière.  
  
 Lorsqu'une action DELETE sur une table enfant ou une table de référence résulte d'une instruction DELETE en cascade depuis une table parente, et qu'un déclencheur INSTEAD OF est défini sur DELETE dans la table enfant, le déclencheur est ignoré et l'action DELETE est exécutée.  
  
## <a name="ddl-triggers"></a>Déclencheurs DDL  
 À la différence des déclencheurs DML, le champ d'action des déclencheurs DDL ne correspond pas aux schémas. OBJECT_ID, OBJECT_NAME, OBJECTPROPERTY et OBJECTPROPERTY(EX) ne peuvent donc pas être utilisés lors de requêtes sur les métadonnées concernant les déclencheurs DDL. Utilisez plutôt les affichages catalogue. Pour plus d’informations, consultez [Obtenir des informations sur les déclencheurs DDL](../../relational-databases/triggers/get-information-about-ddl-triggers.md).  
  
## <a name="logon-triggers"></a>Déclencheurs de connexion  
 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] ne prend pas en charge les déclencheurs sur les événements de connexion.  
  
## <a name="permissions"></a>Autorisations  
 La modification d'un déclencheur DML nécessite une autorisation ALTER sur la table ou la vue sur laquelle le déclencheur est défini.  
  
 La modification d'un déclencheur DDL défini avec une étendue de serveur (ON ALL SERVER) ou d'un déclencheur de connexion nécessite l'autorisation CONTROL SERVER sur le serveur. Modifier un déclencheur DDL défini avec une étendue de base de données (ON DATABASE) nécessite une autorisation ALTER ANY DATABASE DDL TRIGGER sur la base de données active.  
  
## <a name="examples"></a>Exemples  
 L’exemple suivant crée un déclencheur DML dans la base de données AdventureWorks 2012. Ce déclencheur envoie un message défini par l’utilisateur au client quand un utilisateur tente d’ajouter ou de changer les données de la table `SalesPersonQuotaHistory`. Le déclencheur est ensuite modifié à l'aide de l'instruction `ALTER TRIGGER` pour n'appliquer le déclencheur qu'aux activités `INSERT`. Ce déclencheur est utile car il rappelle à l'utilisateur qui met à jour ou insère des lignes dans cette table de notifier également le département `Compensation` .  
  
```sql  
CREATE TRIGGER Sales.bonus_reminder  
ON Sales.SalesPersonQuotaHistory  
WITH ENCRYPTION  
AFTER INSERT, UPDATE   
AS RAISERROR ('Notify Compensation', 16, 10);  
GO  

-- Now, change the trigger.  
ALTER TRIGGER Sales.bonus_reminder  
ON Sales.SalesPersonQuotaHistory  
AFTER INSERT  
AS RAISERROR ('Notify Compensation', 16, 10);  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [DROP TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/drop-trigger-transact-sql.md)   
 [ENABLE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/enable-trigger-transact-sql.md)   
 [DISABLE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/disable-trigger-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)   
 [sp_helptrigger &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helptrigger-transact-sql.md)   
 [Créer une procédure stockée](../../relational-databases/stored-procedures/create-a-stored-procedure.md)   
 [sp_addmessage &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmessage-transact-sql.md)   
 [Transactions](../../relational-databases/native-client-ole-db-transactions/transactions.md)   
 [Obtenir des informations sur les déclencheurs DML](../../relational-databases/triggers/get-information-about-dml-triggers.md)   
 [Obtenir des informations sur les déclencheurs DDL](../../relational-databases/triggers/get-information-about-ddl-triggers.md)   
 [sys.triggers &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-triggers-transact-sql.md)   
 [sys.trigger_events &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-trigger-events-transact-sql.md)   
 [sys.sql_modules &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md)   
 [sys.assembly_modules &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-assembly-modules-transact-sql.md)   
 [sys.server_triggers &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-triggers-transact-sql.md)   
 [sys.server_trigger_events &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-trigger-events-transact-sql.md)   
 [sys.server_sql_modules &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-sql-modules-transact-sql.md)   
 [sys.server_assembly_modules &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-assembly-modules-transact-sql.md)   
 [Modifier le schéma dans les bases de données de publication](../../relational-databases/replication/publish/make-schema-changes-on-publication-databases.md)  
  
  
