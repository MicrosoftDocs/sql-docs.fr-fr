---
title: CREATE TRIGGER (Transact-SQL)
description: Référence Transact-SQL pour l’instruction CREATE TRIGGER qui permet de créer un déclencheur DML, DDL ou de connexion.
ms.date: 10/30/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CREATE TRIGGER
- TRIGGER
- CREATE_TRIGGER_TSQL
- TRIGGER_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- recursive DML triggers [SQL Server]
- CREATE TRIGGER statement
- multiple triggers
- deferred name resolution, DML triggers
- DML triggers, creating
- nested triggers
- server-scoped triggers
- DDL triggers, creating
- triggers [SQL Server], creating
- database-scoped triggers [SQL Server]
ms.assetid: edeced03-decd-44c3-8c74-2c02f801d3e7
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.reviewer: mathoma
ms.openlocfilehash: d4b71ddb3cb0b8dbd60ccfb9df778baaaff3f68d
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170331"
---
# <a name="create-trigger-transact-sql"></a>CREATE TRIGGER (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Crée un déclencheur DML, DDL ou de connexion. Un déclencheur est un type particulier de procédure stockée qui s’exécute automatiquement quand un événement se produit sur le serveur de base de données. Les déclencheurs DML s'exécutent lorsqu'un utilisateur essaie de modifier des données via un événement DML (Data Manipulation Language). Les événements DML sont des instructions INSERT, UPDATE ou DELETE exécutées sur une table ou une vue. Ces déclencheurs s'activent au déclenchement d'un événement valide, que des lignes de table soient affectées ou non. Pour plus d'informations, consultez [DML Triggers](../../relational-databases/triggers/dml-triggers.md).  
  
Les déclencheurs DDL s’exécutent en réponse à différents événements de langage de définition de données (DDL). Ces événements correspondent essentiellement aux instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] CREATE, ALTER et DROP et à certaines procédures stockées système qui effectuent des opérations de type DDL. 

Les déclencheurs LOGON sont activés en réponse à l’événement LOGON qui est levé quand une session utilisateur est établie. Vous pouvez créer des déclencheurs directement à partir d’instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] ou de méthodes d’assembly créées dans le CLR (Common Language Runtime) [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] et chargées dans une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vous permet de créer plusieurs déclencheurs pour toute instruction spécifique.  
  
> [!IMPORTANT]  
>  Un code malveillant présent dans des déclencheurs peut s'exécuter sous des privilèges promus. Pour plus d’informations sur la façon de réduire cette menace, consultez [Gérer la sécurité des déclencheurs](../../relational-databases/triggers/manage-trigger-security.md).  
  
> [!NOTE]  
>  L’intégration du CLR .NET Framework à SQL Server est décrite dans cet article. L’intégration du CLR ne s’applique pas à Azure SQL Database.  
  
![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="sql-server-syntax"></a>Syntaxe SQL Server  
  
```syntaxsql
-- SQL Server Syntax  
-- Trigger on an INSERT, UPDATE, or DELETE statement to a table or view (DML Trigger)  
  
CREATE [ OR ALTER ] TRIGGER [ schema_name . ]trigger_name   
ON { table | view }   
[ WITH <dml_trigger_option> [ ,...n ] ]  
{ FOR | AFTER | INSTEAD OF }   
{ [ INSERT ] [ , ] [ UPDATE ] [ , ] [ DELETE ] }   
[ WITH APPEND ]  
[ NOT FOR REPLICATION ]   
AS { sql_statement  [ ; ] [ ,...n ] | EXTERNAL NAME <method specifier [ ; ] > }  
  
<dml_trigger_option> ::=  
    [ ENCRYPTION ]  
    [ EXECUTE AS Clause ]  
  
<method_specifier> ::=  
    assembly_name.class_name.method_name  
  
```  
  
```syntaxsql
-- SQL Server Syntax  
-- Trigger on an INSERT, UPDATE, or DELETE statement to a 
-- table (DML Trigger on memory-optimized tables)  
  
CREATE [ OR ALTER ] TRIGGER [ schema_name . ]trigger_name   
ON { table }   
[ WITH <dml_trigger_option> [ ,...n ] ]  
{ FOR | AFTER }   
{ [ INSERT ] [ , ] [ UPDATE ] [ , ] [ DELETE ] }   
AS { sql_statement  [ ; ] [ ,...n ] }  
  
<dml_trigger_option> ::=  
    [ NATIVE_COMPILATION ]  
    [ SCHEMABINDING ]  
    [ EXECUTE AS Clause ]  
  
```  
  
```syntaxsql
-- Trigger on a CREATE, ALTER, DROP, GRANT, DENY, 
-- REVOKE or UPDATE statement (DDL Trigger)  
  
CREATE [ OR ALTER ] TRIGGER trigger_name   
ON { ALL SERVER | DATABASE }   
[ WITH <ddl_trigger_option> [ ,...n ] ]  
{ FOR | AFTER } { event_type | event_group } [ ,...n ]  
AS { sql_statement  [ ; ] [ ,...n ] | EXTERNAL NAME < method specifier >  [ ; ] }  
  
<ddl_trigger_option> ::=  
    [ ENCRYPTION ]  
    [ EXECUTE AS Clause ]  
  
```  
  
```syntaxsql
-- Trigger on a LOGON event (Logon Trigger)  
  
CREATE [ OR ALTER ] TRIGGER trigger_name   
ON ALL SERVER   
[ WITH <logon_trigger_option> [ ,...n ] ]  
{ FOR| AFTER } LOGON    
AS { sql_statement  [ ; ] [ ,...n ] | EXTERNAL NAME < method specifier >  [ ; ] }  
  
<logon_trigger_option> ::=  
    [ ENCRYPTION ]  
    [ EXECUTE AS Clause ]  
  
```  
  
## <a name="azure-sql-database-syntax"></a>Syntaxe Azure SQL Database  
  
```syntaxsql
-- Azure SQL Database Syntax   
-- Trigger on an INSERT, UPDATE, or DELETE statement to a table or view (DML Trigger)  
  
CREATE [ OR ALTER ] TRIGGER [ schema_name . ]trigger_name   
ON { table | view }   
 [ WITH <dml_trigger_option> [ ,...n ] ]   
{ FOR | AFTER | INSTEAD OF }   
{ [ INSERT ] [ , ] [ UPDATE ] [ , ] [ DELETE ] }   
  AS { sql_statement  [ ; ] [ ,...n ] [ ; ] > }  
  
<dml_trigger_option> ::=   
        [ EXECUTE AS Clause ]  
  
```  
  
```syntaxsql
-- Azure SQL Database Syntax  
-- Trigger on a CREATE, ALTER, DROP, GRANT, DENY, 
-- REVOKE, or UPDATE STATISTICS statement (DDL Trigger)   
  
CREATE [ OR ALTER ] TRIGGER trigger_name   
ON { DATABASE }   
 [ WITH <ddl_trigger_option> [ ,...n ] ]   
{ FOR | AFTER } { event_type | event_group } [ ,...n ]   
AS { sql_statement  [ ; ] [ ,...n ]  [ ; ] }  
  
<ddl_trigger_option> ::=   
    [ EXECUTE AS Clause ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
OR ALTER  
**S’applique à** : Azure [!INCLUDE[ssSDS](../../includes/sssds-md.md)], [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] SP1). 
  
Modifie, de manière conditionnelle, le déclencheur uniquement s’il existe déjà. 
  
*schema_name*  
Nom du schéma auquel appartient le déclencheur DML. La portée des déclencheurs DML se limite au schéma de la table ou de la vue sur laquelle ils sont créés. Vous ne pouvez pas spécifier *schema_name* pour des déclencheurs DDL ou de connexion.  
  
*trigger_name*  
Nom du déclencheur. Un *trigger_name* doit respecter les règles applicables aux [identificateurs](../../relational-databases/databases/database-identifiers.md), excepté que *trigger_name* ne peut pas commencer par # ou par ##.  
  
*table* | *view*  
Table ou vue sur laquelle le déclencheur DML est exécuté. Cette table ou cette vue sont parfois appelées table de déclencheur ou vue de déclencheur. La spécification du nom complet de la table ou de la vue est facultative. Vous ne pouvez référencer une vue que via un déclencheur INSTEAD OF. Vous ne pouvez pas définir de déclencheurs DML sur des tables temporaires locales ou globales.  
  
DATABASE  
Applique l'étendue d'un déclencheur DDL à la base de données active. S’il est spécifié, le déclencheur est activé chaque fois qu’*event_type* ou *event_group* se produit dans la base de données active.  
  
ALL SERVER  
**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.  
  
Applique l'étendue d'un déclencheur DDL ou de connexion au serveur actif. S’il est spécifié, le déclencheur est activé chaque fois qu’*event_type* ou *event_group* se produit à un endroit quelconque sur le serveur actif.  
  
WITH ENCRYPTION  
**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.  
  
Masque le texte de l'instruction CREATE TRIGGER. L'utilisation de l'argument WITH ENCRYPTION évite la publication du déclencheur dans le cadre de la réplication [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Il n'est pas possible de spécifier WITH ENCRYPTION pour les déclencheurs CLR.  
  
EXECUTE AS  
Spécifie le contexte de sécurité dans lequel le déclencheur est exécuté. Cet argument permet de contrôler le compte d'utilisateur que l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilise pour valider les autorisations sur n'importe quel objet de la base de données référencé par le déclencheur.  
  
Cette option est obligatoire pour les déclencheurs sur les tables optimisées en mémoire.  
  
Pour plus d’informations, consultez [Clause EXECUTE AS &#40;Transact-SQL&#41;](../../t-sql/statements/execute-as-clause-transact-sql.md).  
  
NATIVE_COMPILATION  
Indique que le déclencheur est compilé en mode natif.  
  
Cette option est obligatoire pour les déclencheurs sur les tables optimisées en mémoire.  
  
SCHEMABINDING  
Garantit que les tables référencées par un déclencheur ne peuvent pas être supprimées ou modifiées.  
  
Cette option est obligatoire pour les déclencheurs sur les tables optimisées en mémoire et n’est pas prise en charge pour les déclencheurs sur des tables traditionnelles.  
  
FOR | AFTER  
FOR ou AFTER spécifie que le déclencheur DML est activé seulement lorsque toutes les opérations spécifiées dans l’instruction SQL de déclenchement ont été lancées correctement. Toutes les actions référentielles en cascade et les vérifications de contraintes doivent également être effectuées avec succès pour que ce déclencheur soit activé.  
  
Vous ne pouvez pas définir de déclencheurs AFTER sur des vues.  
  
INSTEAD OF  
Spécifie que le déclencheur DML est lancé *à la place de* l’instruction SQL de déclenchement, remplaçant ainsi les actions des instructions de déclenchement. Vous ne pouvez pas spécifier INSTEAD OF pour des déclencheurs DDL ou de connexion.  
  
Vous pouvez tout au plus définir un déclencheur INSTEAD OF par instruction INSERT, UPDATE ou DELETE sur une table ou une vue. Vous pouvez également définir des vues sur des vues où chaque vue a son propre déclencheur INSTEAD OF.  
  
Vous ne pouvez pas définir de déclencheurs INSTEAD OF sur des vues pouvant être mises à jour qui utilisent l'option WITH CHECK OPTION. Cela a pour effet de provoquer une erreur quand un déclencheur INSTEAD OF est ajouté à une telle vue avec l’option WITH CHECK OPTION spécifiée. Vous supprimez cette option à l'aide de l'instruction ALTER VIEW avant de définir le déclencheur INSTEAD OF.  
  
{ [ DELETE ] [ , ] [ INSERT ] [ , ] [ UPDATE ] }  
Spécifie les instructions de modification des données qui activent le déclencheur DML lorsqu'une tentative a lieu pour l'appliquer à cette table ou à cette vue. Spécifier au moins une option. Utilisez une combinaison de ces options dans un ordre quelconque dans la définition du déclencheur.  
  
Dans le cas des déclencheurs INSTEAD OF, vous ne pouvez pas utiliser l'option DELETE sur les tables dont la relation référentielle spécifie une action en cascade ON DELETE. De même, l'option UPDATE n'est pas autorisée sur les tables dont la relation référentielle spécifie une action en cascade ON UPDATE.  
  
WITH APPEND  
**S'applique à**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] jusqu'à [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)].  
  
Spécifie qu'il faut ajouter un déclencheur supplémentaire d'un type existant. La clause WITH APPEND ne peut pas s'utiliser avec des déclencheurs INSTEAD OF ou si un déclencheur AFTER est stipulé de manière explicite. Pour la compatibilité descendante, utilisez uniquement WITH APPEND lorsque FOR es spécifié, sans INSTEAD OF ou AFTER. Vous ne pouvez pas spécifier WITH APPEND si vous utilisez EXTERNAL NAME (autrement dit, si le déclencheur est un déclencheur CLR).  
  
*event_type*  
Nom d’un événement de langage [!INCLUDE[tsql](../../includes/tsql-md.md)] qui, après le lancement, provoque l'activation d'un déclencheur DDL. Les événements valides pour les déclencheurs DDL sont répertoriés dans [Événements DDL](../../relational-databases/triggers/ddl-events.md).  
  
*event_group*  
Nom d'un regroupement prédéfini d'événements de langage [!INCLUDE[tsql](../../includes/tsql-md.md)]. Le déclencheur DDL est activé après l’exécution de n’importe quel événement de langage [!INCLUDE[tsql](../../includes/tsql-md.md)] appartenant à *event_group*. Les groupes d’événements valides pour les déclencheurs DDL sont répertoriés dans [Groupes d’événements DDL](../../relational-databases/triggers/ddl-event-groups.md).  
  
Une fois l’exécution de CREATE TRIGGER terminée, *event_group* fait aussi office de macro en ajoutant les types d’événements qu’il couvre à la vue de catalogue sys.trigger_events.  
  
NOT FOR REPLICATION  
**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.  
  
Indique que le déclencheur ne doit pas être exécuté lorsqu'un aent de réplication modifie la table impliquée dans le déclencheur.  
  
*sql_statement*  
Conditions et actions du déclencheur. Les conditions du déclencheur spécifient des critères supplémentaires qui déterminent si les instructions DML, DDL ou de connexion tentées vont provoquer l'exécution des actions du déclencheur.  
  
Les actions du déclencheur spécifiées dans les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] prennent effet lors de la tentative d'opération.  
  
Les déclencheurs peuvent comprendre n’importe quel type et n’importe quel nombre d’instructions [!INCLUDE[tsql](../../includes/tsql-md.md)]. Il existe toutefois des exceptions. Pour plus d'informations, consultez la section Notes. Un déclencheur sert à vérifier ou à modifier des données suite à une instruction de modification ou de définition. Il ne doit pas retourner des données à l'utilisateur. Les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] dans un déclencheur comportent souvent un [langage de contrôle de flux](~/t-sql/language-elements/control-of-flow.md).  
  
Les déclencheurs DML utilisent les tables logiques (conceptuelles) supprimées ou insérées. Leur structure est similaire à celle de la table sur laquelle le déclencheur est défini, c'est-à-dire la table sur laquelle l'action de l'utilisateur est tentée. Les tables supprimées et insérées contiennent les anciennes ou les nouvelles valeurs des lignes que l'action de l'utilisateur peut modifier. Par exemple, pour extraire toutes les valeurs de la table `deleted`, utilisez :  
  
```sql  
SELECT * FROM deleted;  
```  
  
Pour plus d’informations, consultez [Utiliser les tables inserted et deleted](../../relational-databases/triggers/use-the-inserted-and-deleted-tables.md).  
  
Les déclencheurs DDL et de connexion capturent des informations sur l’événement de déclenchement à l’aide de la fonction [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md). Pour plus d’informations, consultez [Utiliser la fonction EVENTDATA](../../relational-databases/triggers/use-the-eventdata-function.md).  
  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] autorise la mise à jour des colonnes **text**, **ntext**, ou **image** via le déclencheur INSTEAD OF sur des tables ou des vues.  
  
> [!IMPORTANT]
>  Les types de données **ntext**, **text** et **image** seront supprimés dans une version ultérieure de [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Évitez d'utiliser ces types de données dans un nouveau développement. Prévoyez de modifier les applications qui les utilisent actuellement. Utilisez plutôt les types de données [nvarchar(max)](../../t-sql/data-types/nchar-and-nvarchar-transact-sql.md), [varchar(max)](../../t-sql/data-types/char-and-varchar-transact-sql.md)et [varbinary(max)](../../t-sql/data-types/binary-and-varbinary-transact-sql.md) . Les déclencheurs AFTER et INSTEAD OF prennent tous les deux en charge les données **varchar(MAX)** , **nvarchar(MAX)** et **varbinary(MAX)** dans les tables inserted et deleted.  
  
Pour les déclencheurs sur les tables optimisées en mémoire, la seule instruction *sql_statement* autorisée au niveau supérieur est un bloc ATOMIC. Le code T-SQL autorisé dans le bloc ATOMIC est limité par le code T-SQL autorisé dans les procédures natives.  
  
\< method_specifier > 
**S’applique à**; [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.  
  
Pour un déclencheur CLR, spécifie la méthode de liaison d'un assembly avec le déclencheur. La méthode ne doit prendre aucun argument et retourner une valeur vide. *class_name* doit être un identificateur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] valide et doit exister comme classe dans l’assembly avec une visibilité de l’assembly. Si la classe a un nom qualifié par un espace de noms qui utilise '.' pour séparer les parties de l'espace de noms, le nom de la classe doit être délimité par des crochets ([ ]) ou des guillemets doubles (" "). La classe ne peut pas être imbriquée.  
  
> [!NOTE]  
>  Par défaut, la possibilité de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] d'exécuter du code CLR est désactivée. Vous pouvez créer, modifier et supprimer des objets de base de données qui font référence à des modules de code managé. Cependant, ces références ne s’exécutent dans une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que si l’[option clr enabled](../../database-engine/configure-windows/clr-enabled-server-configuration-option.md) est activée à l’aide de [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md).  
  
## <a name="remarks-for-dml-triggers"></a>Remarques sur les déclencheurs DML  
Les déclencheurs DML s'utilisent souvent pour imposer des règles de gestion et l'intégrité des données. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournit l'intégrité référentielle déclarative (DRI) via des instructions ALTER TABLE et CREATE TABLE. Cependant, la fonctionnalité DRI ne gère pas l'intégrité référentielle entre bases de données. L'intégrité référentielle se réfère aux règles appliquées aux relations entre les clés primaires et les clés étrangères des tables. Pour appliquer l'intégrité référentielle, utilisez les contraintes PRIMARY KEY et FOREIGN KEY dans les instructions ALTER TABLE et CREATE TABLE. S'il existe des contraintes sur la table des déclencheurs, elles sont vérifiées après l'exécution du déclencheur INSTEAD OF et avant celle du déclencheur AFTER. Si les contraintes sont violées, les actions du déclencheur INSTEAD OF sont annulées et le déclencheur AFTER n'est pas activé.  
  
Vous pouvez spécifier le premier et le dernier déclencheur AFTER à exécuter sur une table à l'aide de sp_settriggerorder. Vous ne pouvez spécifier qu’un premier et un dernier déclencheur AFTER pour chaque instruction INSERT, UPDATE ou DELETE sur une table. S'il y a d'autres déclencheurs AFTER sur la même table, ils sont exécutés de manière aléatoire.  
  
Si une instruction ALTER TRIGGER modifie un premier ou un dernier déclencheur. Le premier ou le dernier attribut défini sur le déclencheur modifié est supprimé et vous devez réinitialiser la valeur de l’ordre avec sp_settriggerorder.  
  
Un déclencheur AFTER est exécuté seulement après que l'instruction SQL de déclenchement a été exécutée correctement. Cette exécution réussie inclut toutes les actions d'intégrité référentielle en cascade et les vérifications des contraintes associées à l'objet mis à jour ou supprimé. Un déclencheur AFTER n’active pas de manière récursive un déclencheur INSTEAD OF sur la même table.  
  
Si un déclencheur INSTEAD OF défini sur une table exécute une instruction portant sur cette table et qui est susceptible de l'activer de nouveau, il n'est pas appelé de façon récurrente. L'instruction est traitée comme si la table n'avait aucun déclencheur INSTEAD OF et démarre la chaîne des opérations de contrainte et des exécutions du déclencheur AFTER. Par exemple, si un déclencheur est défini comme un déclencheur INSTEAD OF INSERT pour une table. Le déclencheur exécute une instruction INSERT sur la même table et l’instruction INSERT lancée par le déclencheur INSTEAD OF n’appelle pas à nouveau le déclencheur. L'instruction INSERT lancée par le déclencheur démarre le processus d'exécution des actions de contrainte et d'activation de tout déclencheur AFTER INSERT défini pour la table.  
  
Lorsqu’un déclencheur INSTEAD OF défini sur une vue exécute une instruction portant sur cette vue et qui est susceptible de l'activer de nouveau, il n'est pas appelé de façon récurrente. Au lieu de cela, l'instruction est résolue sous forme de modifications apportées aux tables de base sous-jacentes de la vue. Dans ce cas, la définition de la vue doit respecter toutes les restrictions applicables à une vue pouvant être mise à jour. Pour obtenir une définition des vues pouvant être mises à jour, consultez [Modifier les données par l’intermédiaire d’une vue](../../relational-databases/views/modify-data-through-a-view.md).  
  
Par exemple, si un déclencheur est défini comme un déclencheur INSTEAD OF UPDATE pour une vue. Le déclencheur exécute une instruction UPDATE qui référence la même vue et l’instruction UPDATE lancée par le déclencheur INSTEAD OF n’appelle pas à nouveau le déclencheur. Elle est appliquée à la vue comme si celle-ci ne comportait pas de déclencheur INSTEAD OF. Les colonnes modifiées par l'instruction UPDATE doivent être résolues en une seule table de base. Chaque modification d'une table de base sous-jacente démarre la chaîne d'application des contraintes et d'activation des déclencheurs AFTER définis sur la table.  
  
### <a name="testing-for-update-or-insert-actions-to-specific-columns"></a>Test des actions UPDATE ou INSERT sur des colonnes spécifiques  
Vous pouvez créer un déclencheur [!INCLUDE[tsql](../../includes/tsql-md.md)] qui effectue certaines actions en fonction de modifications des instructions UPDATE ou INSERT sur des colonnes particulières. Pour cela, utilisez [UPDATE()](../../t-sql/functions/update-trigger-functions-transact-sql.md) ou [COLUMNS_UPDATED](../../t-sql/functions/columns-updated-transact-sql.md) dans le corps du déclencheur. UPDATE() teste les tentatives UPDATE ou INSERT sur une colonne. COLUMNS_UPDATED effectue un test pour déterminer si des actions UPDATE ou INSERT sont exécutées sur plusieurs colonnes. Cette fonction retourne un modèle binaire qui indique quelles colonnes ont été insérées ou mises à jour.  
  
### <a name="trigger-limitations"></a>Limitations des déclencheurs  
CREATE TRIGGER doit être la première instruction du traitement et ne peut s'appliquer qu'à une seule table.  
  
Un déclencheur n'est créé que dans la base de données active. Cependant, il peut faire référence à des objets qui se trouvent hors de la base de données active.  
  
Si le nom du schéma du déclencheur est spécifié pour qualifier le déclencheur, qualifiez le nom de la table de la même façon.  
  
La même action de déclencheur peut être définie pour plusieurs actions de l'utilisateur (par exemple, INSERT et UPDATE) dans la même instruction CREATE TRIGGER.  
  
Les déclencheurs INSTEAD OF DELETE/UPDATE ne peuvent pas être définis sur une table ayant une clé étrangère pour laquelle une action DELETE/UPDATE en cascade est définie.  
  
Vous pouvez spécifier n'importe quelle instruction SET dans le déclencheur. L'option SET sélectionnée reste active pendant l'exécution du déclencheur, puis retrouve sa valeur d'origine.  
  
Lorsqu'un déclencheur est activé, les résultats sont retournés à l'application appelante, comme pour les procédures stockées. Pour éviter le retour de résultats à une application parce qu'un déclencheur est activé, n'incluez pas d'instructions SELECT qui retournent des résultats, ni d'instructions affectant des variables dans un déclencheur. Un déclencheur qui inclut des instructions SELECT retournant des résultats à l’utilisateur ou des instructions affectant des variables nécessite un traitement spécial. Vous devriez écrire les résultats renvoyés dans chacune des applications autorisant des modifications de la table du déclencheur. Si une affectation de variable doit avoir lieu dans un déclencheur, utilisez l'instruction SET NOCOUNT au début du déclencheur, pour éviter tout retour d'un jeu de résultats.  
  
Bien qu'une instruction TRUNCATE TABLE soit appliquée dans une instruction DELETE, elle n'active pas de déclencheur parce que l'opération n'enregistre pas les suppressions de lignes individuelles. Toutefois, seuls les utilisateurs disposant d'autorisations permettant d'exécuter une instruction TRUNCATE TABLE doivent se soucier de contourner par inadvertance un déclencheur DELETE de cette façon.  
  
L'instruction WRITETEXT, enregistrée ou non dans le journal, n'active pas un déclencheur.  
  
Les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] suivantes ne sont pas autorisées dans un déclencheur DML :  

- ALTER DATABASE
- CREATE DATABASE
- DROP DATABASE
- RESTORE DATABASE
- RESTORE LOG
- RECONFIGURE

De plus, les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] suivantes ne sont pas autorisées dans le corps d'un déclencheur DML lorsque celui-ci est utilisé sur la table ou la vue cible de l'action de déclenchement.  
  
- CREATE INDEX (y compris CREATE SPATIAL INDEX et CREATE XML INDEX)
- ALTER INDEX
- DROP INDEX
- DROP TABLE
- DBCC DBREINDEX
- ALTER PARTITION FUNCTION
- ALTER TABLE quand elle est utilisée pour effectuer les actions suivantes :
    - ajout, modification ou suppression de colonnes ;
    - changement de partitions ;
    - ajout ou suppression de contraintes PRIMARY KEY ou UNIQUE.

> [!NOTE]  
>  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne prenant pas en charge les déclencheurs définis par l'utilisateur sur des tables système, nous recommandons de ne pas en créer. 

### <a name="optimizing-dml-triggers"></a>Optimisation des déclencheurs DML
Les déclencheurs fonctionnent dans des transactions (implicites ou autres). Quand ils sont ouverts, ils verrouillent les ressources. Le verrou reste en place jusqu’à ce que la transaction soit confirmée (avec COMMIT) ou rejetée (avec ROLLBACK). Plus la durée d’exécution d’un déclencheur est longue, plus le risque de blocage d’un autre processus augmente. Par conséquent, écrivez les déclencheurs afin de réduire leur durée autant que possible. Une façon d’y parvenir est de libérer un déclencheur quand une instruction DML ne change aucune ligne. 

Pour libérer un déclencheur dans une commande qui ne change aucune ligne, utilisez la variable système [ROWCOUNT_BIG](../functions/rowcount-big-transact-sql.md). 

L’extrait de code T-SQL suivant montre comment libérer le déclencheur pour une commande qui ne modifie aucune ligne. Ce code doit se trouver au début de chaque déclencheur DML :

```sql
IF (ROWCOUNT_BIG() = 0)
RETURN;
```
  
  
## <a name="remarks-for-ddl-triggers"></a>Remarques sur les déclencheurs DDL  
Les déclencheurs DDL, tout comme les déclencheurs standard, lancent des procédures stockées en réponse à un événement. Cependant, à la différence des déclencheurs standard, ils ne s'exécutent pas en réponse aux instructions UPDATE, INSERT ou DELETE sur une table ou sur une vue. Au lieu de cela, ils s'exécutent essentiellement en réponse aux instructions DDL (Data Definition Language). Les types d’instructions incluent CREATE, ALTER, DROP, GRANT, DENY, REVOKE et UPDATE STATISTICS. Certaines procédures stockées système qui effectuent des opérations de type DDL peuvent également activer des déclencheurs DDL.  
  
> [!IMPORTANT]  
>  Testez vos déclencheurs DDL afin de déterminer leurs réponses à l'exécution des procédures stockées système. Par exemple, l'instruction CREATE TYPE et les procédures stockées sp_addtype et sp_rename activent un déclencheur DDL créé à l'occasion d'un événement CREATE_TYPE.  
  
Pour plus d’informations sur les déclencheurs DDL, consultez [Déclencheurs DDL](../../relational-databases/triggers/ddl-triggers.md).  
  
Les déclencheurs DDL ne sont pas activés en réponse à des événements qui concernent les tables et les procédures stockées temporaires locales ou globales.  
  
À la différence des déclencheurs DML, le champ d'action des déclencheurs DDL ne correspond pas aux schémas. Vous ne pouvez donc pas utiliser de fonctions telles qu’OBJECT_ID, OBJECT_NAME, OBJECTPROPERTY et OBJECTPROPERTYEX pour effectuer des requêtes de métadonnées à propos de déclencheurs DDL. Utilisez plutôt les affichages catalogue. Pour plus d’informations, consultez [Obtenir des informations sur les déclencheurs DDL](../../relational-databases/triggers/get-information-about-ddl-triggers.md).  
  
> [!NOTE]  
>  Les déclencheurs DDL avec étendue au niveau serveur figurent dans le dossier **Déclencheurs** de l’Explorateur d’objets [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Ce dossier se trouve dans le dossier **Objets serveur** . Les déclencheurs DDL délimités à la base de données figurent dans le dossier **Déclencheurs de base de données**. Ce dossier se trouve dans le dossier **Programmabilité** de la base de données correspondante.  
  
## <a name="logon-triggers"></a>Déclencheurs de connexion  
Les déclencheurs de connexion exécutent des procédures stockées en réponse à un événement LOGON. Cet événement survient lorsqu'une session utilisateur est établie avec une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Les déclencheurs de connexion sont activés au terme de la phase d'authentification de connexion, mais avant l'établissement de la session utilisateur. Tous les messages provenant du corps du déclencheur et habituellement destinés à l'utilisateur, (les messages et les messages d'erreur de l'instruction PRINT, par exemple), sont donc dirigés vers le journal des erreurs [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Pour plus d’informations, consultez [Déclencheurs de connexion](../../relational-databases/triggers/logon-triggers.md).  
  
Les déclencheurs de connexion ne sont pas activés si l'authentification échoue.  
  
Les transactions distribuées ne sont pas prises en charge dans un déclencheur ouverture de session. L’erreur 3969 est retournée quand un déclencheur d’ouverture de session qui contient une transaction distribuée est activé.  
  
### <a name="disabling-a-logon-trigger"></a>Désactivation d'un déclencheur de connexion  
Un déclencheur de connexion peut empêcher les connexions au [!INCLUDE[ssDE](../../includes/ssde-md.md)] pour tous les utilisateurs, notamment les membres du rôle serveur fixe **sysadmin** . Quand un déclencheur de connexion empêche les connexions, les membres du rôle serveur fixe **sysadmin** peuvent se connecter à l’aide de la connexion administrateur dédiée, ou en démarrant le [!INCLUDE[ssDE](../../includes/ssde-md.md)] en mode de configuration minimale (-f). Pour plus d’informations, consultez [Options de démarrage du service moteur de base de données](../../database-engine/configure-windows/database-engine-service-startup-options.md).  
  
## <a name="general-trigger-considerations"></a>Considérations générales sur les déclencheurs  
  
### <a name="returning-results"></a>Retour de résultats  
Cette possibilité d'obtenir des résultats via des déclencheurs sera supprimée dans les prochaines versions de Microsoft SQL Server. Les déclencheurs qui renvoient des jeux de résultats peuvent provoquer un comportement inattendu des applications qui ne sont pas conçues pour interagir avec eux. Évitez de renvoyer des jeux de résultats provenant de déclencheurs dans un nouveau travail de développement et prévoyez la modification des applications qui y recourent actuellement. Pour empêcher les déclencheurs de retourner des jeux de résultats, attribuez la valeur 1 à l’[option Interdire les résultats à partir des déclencheurs](../../database-engine/configure-windows/disallow-results-from-triggers-server-configuration-option.md).  
  
Les déclencheurs d’ouverture de session interdisent toujours le renvoi de jeux de résultats et ce comportement n'est pas configurable. Si un déclencheur d’ouverture de session génère un jeu de résultats, le lancement du déclencheur échoue et la tentative de connexion qui a activé le déclencheur est refusée.  
  
### <a name="multiple-triggers"></a>Déclencheurs multiples  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vous permet de créer plusieurs déclencheurs pour chaque événement DML, DDL ou LOGON. Par exemple, si la commande CREATE TRIGGER FOR UPDATE est exécutée pour une table qui comporte déjà un déclencheur UPDATE, un déclencheur de mise à jour supplémentaire est créé. Dans les versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], il n'était possible de créer qu'un seul déclencheur par événement de modification de données INSERT, UPDATE ou DELETE pour chaque table.  
  
### <a name="recursive-triggers"></a>Déclencheurs récursifs  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prend également en charge l'appel récursif de déclencheurs lorsque le paramètre RECURSIVE_TRIGGERS est activé au moyen de l'instruction ALTER DATABASE.  
  
Les déclencheurs récursifs permettent les types de récurrence suivants :  
  
-   Récurrence indirecte.  
  
     Avec la récurrence indirecte, une application met à jour la table T1. Cela active le déclencheur TR1, avec pour conséquence la mise à jour de la table T2. Le déclencheur T2 est alors activé et met à jour la table T1.  
  
-   Récurrence directe.  
  
     En récurrence directe, l'application met à jour la table T1. Cela active le déclencheur TR1, avec pour conséquence la mise à jour de la table T1. La table T1 ayant été mise à jour, le déclencheur TR1 est réactivé, etc.  
  
L'exemple suivant illustre l'utilisation de la récurrence directe et indirecte. Supposons que deux déclencheurs de mise à jour, TR1 et TR2, soient définis sur la table T1. Le déclencheur TR1 met à jour la table T1 de manière récursive. Une instruction UPDATE exécute chaque déclencheur TR1 et TR2 une fois. De plus, le lancement de TR1 déclenche l'exécution de TR1 (de manière récursive) et de TR2. Les tables inserted et deleted d'un déclencheur donné contiennent des lignes qui ne correspondent qu'à l'instruction UPDATE qui a appelé le déclencheur.  
  
> [!NOTE]  
>  Le comportement précédent se produit uniquement si le paramètre RECURSIVE_TRIGGERS est activé au moyen de l'instruction ALTER DATABASE. Les différents déclencheurs définis pour un événement donné ne sont pas exécutés dans un ordre défini. Chaque déclencheur doit être indépendant.  
  
La désactivation du paramètre RECURSIVE_TRIGGERS empêche uniquement les récurrences directes. Pour désactiver également la récurrence indirecte, affectez la valeur 0 à l'option de serveur nested triggers à l'aide de sp_configure.  
  
Si un des déclencheurs effectue une opération ROLLBACK TRANSACTION, quel que soit le niveau d'imbrication, aucun autre déclencheur n'est exécuté.  
  
### <a name="nested-triggers"></a>Déclencheurs imbriqués  
Vous pouvez imbriquer des déclencheurs sur 32 niveaux maximum. Si un déclencheur modifie une table dans laquelle il y a un autre déclencheur, le second déclencheur est activé et peut en appeler un troisième, etc. Si un des déclencheurs de la chaîne provoque une boucle infinie, le niveau d'imbrication maximal est dépassé et le déclencheur est annulé. Lorsqu'un déclencheur [!INCLUDE[tsql](../../includes/tsql-md.md)] lance du code managé en référençant une routine, un type ou un agrégat CLR, cette référence compte comme un seul niveau pour le calcul de la limite des 32 niveaux d'imbrication. Les méthodes appelées à partir du code managé ne comptent pas par rapport à cette limite.  
  
Pour désactiver les déclencheurs imbriqués, attribuez la valeur 0 (off) à l'option nested triggers de sp_configure. La configuration par défaut prend en charge les déclencheurs imbriqués. Si l’option des déclencheurs imbriqués est désactivée, les déclencheurs récursifs le sont également, malgré le paramètre RECURSIVE_TRIGGERS défini avec l’instruction ALTER DATABASE.  
  
Le premier déclencheur AFTER imbriqué dans un déclencheur INSTEAD OF se déclenche même si l’option de configuration de serveur **nested triggers** est 0. Toutefois, les déclencheurs AFTER suivants ne sont pas activés avec ce paramètre. Vérifiez si vos applications contiennent des déclencheurs imbriqués pour déterminer si elles sont conformes aux règles de votre entreprise lorsque la configuration du serveur **déclencheurs imbriqués** est définie sur 0. Si ce n’est pas le cas, apportez les modifications appropriées.  
  
### <a name="deferred-name-resolution"></a>Résolution de noms différée  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] permet aux procédures stockées, aux déclencheurs et aux lots d'instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] de faire référence à des tables qui n'existent pas au moment de la compilation. Cette fonction s'appelle la résolution différée des noms.  
  
## <a name="permissions"></a>Autorisations  
La création d'un déclencheur DML nécessite l'autorisation ALTER sur la table ou la vue sur laquelle le déclencheur est créé.  
  
La création d'un déclencheur DDL avec une étendue de serveur (ON ALL SERVER) ou d’un déclencheur de connexion nécessite l'autorisation CONTROL SERVER sur le serveur. La création d'un déclencheur DDL avec l'étendue de la base de données (ON DATABASE) nécessite l'autorisation ALTER ANY DATABASE DDL TRIGGER sur la base de données active.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-using-a-dml-trigger-with-a-reminder-message"></a>R. Utilisation d'un déclencheur DML avec un message de rappel  
Le déclencheur DML suivant affiche un message à destination du client lorsque quelqu'un essaye d'ajouter ou de modifier des données dans la table `Customer` de la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```sql  
CREATE TRIGGER reminder1  
ON Sales.Customer  
AFTER INSERT, UPDATE   
AS RAISERROR ('Notify Customer Relations', 16, 10);  
GO  
```  
  
### <a name="b-using-a-dml-trigger-with-a-reminder-e-mail-message"></a>B. Utilisation d'un déclencheur DML avec un message de rappel par courrier électronique  
L'exemple suivant envoie un message électronique à une personne spécifiée (`MaryM`) lorsque la table `Customer` est modifiée.  
  
```sql  
CREATE TRIGGER reminder2  
ON Sales.Customer  
AFTER INSERT, UPDATE, DELETE   
AS  
   EXEC msdb.dbo.sp_send_dbmail  
        @profile_name = 'AdventureWorks2012 Administrator',  
        @recipients = 'danw@Adventure-Works.com',  
        @body = 'Don''t forget to print a report for the sales force.',  
        @subject = 'Reminder';  
GO  
```  
  
### <a name="c-using-a-dml-after-trigger-to-enforce-a-business-rule-between-the-purchaseorderheader-and-vendor-tables"></a>C. Utilisation d'un déclencheur DML AFTER pour imposer une règle de gestion entre les tables PurchaseOrderHeader et Vendor  
Les contraintes CHECK pouvant référencer uniquement les colonnes sur lesquelles des contraintes de niveau table ou colonne sont définies, vous devez définir toutes les contraintes entre tables (dans ce cas, des règles de gestion) sous la forme de déclencheurs.  
  
L'exemple suivant crée un déclencheur DML dans la base de données AdventureWorks2012. Ce déclencheur vérifie que les informations de conditions de crédit du fournisseur sont correctes (pas 5) lors d’une tentative d’insertion d’un nouveau bon de commande dans la table `PurchaseOrderHeader`. Pour obtenir les informations de conditions de crédit du fournisseur, la table `Vendor` doit être référencée. Si les conditions de crédit sont trop faibles, un message s'affiche et l'insertion n'a pas lieu.  
  
```sql  
-- This trigger prevents a row from being inserted in the Purchasing.PurchaseOrderHeader 
-- table when the credit rating of the specified vendor is set to 5 (below average).  
  
CREATE TRIGGER Purchasing.LowCredit ON Purchasing.PurchaseOrderHeader  
AFTER INSERT  
AS  
IF (ROWCOUNT_BIG() = 0)
RETURN;
IF EXISTS (SELECT *  
           FROM Purchasing.PurchaseOrderHeader AS p   
           JOIN inserted AS i   
           ON p.PurchaseOrderID = i.PurchaseOrderID   
           JOIN Purchasing.Vendor AS v   
           ON v.BusinessEntityID = p.VendorID  
           WHERE v.CreditRating = 5  
          )  
BEGIN  
RAISERROR ('A vendor''s credit rating is too low to accept new  
purchase orders.', 16, 1);  
ROLLBACK TRANSACTION;  
RETURN   
END;  
GO  
  
-- This statement attempts to insert a row into the PurchaseOrderHeader table  
-- for a vendor that has a below average credit rating.  
-- The AFTER INSERT trigger is fired and the INSERT transaction is rolled back.  
  
INSERT INTO Purchasing.PurchaseOrderHeader (RevisionNumber, Status, EmployeeID,  
VendorID, ShipMethodID, OrderDate, ShipDate, SubTotal, TaxAmt, Freight)  
VALUES (  
2  
,3  
,261  
,1652  
,4  
,GETDATE()  
,GETDATE()  
,44594.55  
,3567.564  
,1114.8638 );  
GO  
  
```  
  
### <a name="d-using-a-database-scoped-ddl-trigger"></a>D. Utilisation d'un déclencheur DDL avec une étendue de base de données  
L'exemple suivant utilise un déclencheur DDL pour empêcher la suppression d'un synonyme dans une base de données.  
  
```sql  
CREATE TRIGGER safety   
ON DATABASE   
FOR DROP_SYNONYM  
AS   
IF (@@ROWCOUNT = 0)
RETURN;
   RAISERROR ('You must disable Trigger "safety" to remove synonyms!', 10, 1)  
   ROLLBACK  
GO  
DROP TRIGGER safety  
ON DATABASE;  
GO  
```  
  
### <a name="e-using-a-server-scoped-ddl-trigger"></a>E. Utilisation d'un déclencheur DDL avec une étendue de serveur  
L'exemple suivant utilise un déclencheur DDL pour imprimer un message si un événement CREATE DATABASE se produit sur l'instance de serveur active. Il utilise la fonction `EVENTDATA` pour récupérer le texte de l'instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] correspondante. Pour obtenir d’autres exemples d’utilisation d’EVENTDATA dans des déclencheurs DDL, consultez [Utiliser la fonction EVENTDATA](../../relational-databases/triggers/use-the-eventdata-function.md).  
  
**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.  
  
```sql  
CREATE TRIGGER ddl_trig_database   
ON ALL SERVER   
FOR CREATE_DATABASE   
AS   
    PRINT 'Database Created.'  
    SELECT EVENTDATA().value('(/EVENT_INSTANCE/TSQLCommand/CommandText)[1]','nvarchar(max)')  
GO  
DROP TRIGGER ddl_trig_database  
ON ALL SERVER;  
GO  
```  
  
### <a name="f-using-a-logon-trigger"></a>F. Utilisation d'un déclencheur de connexion  
L’exemple de déclencheur de connexion suivant refuse une tentative de connexion à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en tant que membre de la connexion *login_test* si trois sessions utilisateur sont déjà en cours d’exécution sous cette connexion.  
  
**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.  
  
```sql  
USE master;  
GO  
CREATE LOGIN login_test WITH PASSWORD = '3KHJ6dhx(0xVYsdf' MUST_CHANGE,  
    CHECK_EXPIRATION = ON;  
GO  
GRANT VIEW SERVER STATE TO login_test;  
GO  
CREATE TRIGGER connection_limit_trigger  
ON ALL SERVER WITH EXECUTE AS 'login_test'  
FOR LOGON  
AS  
BEGIN  
IF ORIGINAL_LOGIN()= 'login_test' AND  
    (SELECT COUNT(*) FROM sys.dm_exec_sessions  
            WHERE is_user_process = 1 AND  
                original_login_name = 'login_test') > 3  
    ROLLBACK;  
END;  
  
```  
  
### <a name="g-viewing-the-events-that-cause-a-trigger-to-fire"></a>G. Affichage des événements qui provoquent l'activation d'un déclencheur  
L'exemple suivant effectue une requête sur les affichages catalogue `sys.triggers` et `sys.trigger_events` pour déterminer les événements de langage [!INCLUDE[tsql](../../includes/tsql-md.md)] qui provoquent l'activation du déclencheur `safety`. Le déclencheur, `safety`, est créé dans l’exemple D, situé ci-dessus.  
  
```sql  
SELECT TE.*  
FROM sys.trigger_events AS TE  
JOIN sys.triggers AS T ON T.object_id = TE.object_id  
WHERE T.parent_class = 0 AND T.name = 'safety';  
GO  
```  

    

## <a name="see-also"></a>Voir aussi  
 [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)   
 [ALTER TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/alter-trigger-transact-sql.md)   
 [COLUMNS_UPDATED &#40;Transact-SQL&#41;](../../t-sql/functions/columns-updated-transact-sql.md)   
 [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)   
 [DROP TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/drop-trigger-transact-sql.md)   
 [ENABLE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/enable-trigger-transact-sql.md)   
 [DISABLE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/disable-trigger-transact-sql.md)   
 [TRIGGER_NESTLEVEL &#40;Transact-SQL&#41;](../../t-sql/functions/trigger-nestlevel-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)   
 [sys.dm_sql_referenced_entities &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referenced-entities-transact-sql.md)   
 [sys.dm_sql_referencing_entities &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referencing-entities-transact-sql.md)   
 [sys.sql_expression_dependencies &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md)   
 [sp_help &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-transact-sql.md)   
 [sp_helptrigger &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helptrigger-transact-sql.md)   
 [sp_helptext &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helptext-transact-sql.md)   
 [sp_rename &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-rename-transact-sql.md)   
 [sp_settriggerorder &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-settriggerorder-transact-sql.md)   
 [UPDATE&#40;&#41; &#40;Transact-SQL&#41;](../../t-sql/functions/update-trigger-functions-transact-sql.md)   
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
  
  


