---
title: T-SQL non pris en charge par l’OLTP en mémoire
description: Découvrez les fonctionnalités Transact-SQL qui ne sont pas prises en charge pour les tables à mémoire optimisée, les procédures stockées compilées en mode natif et les fonctions définies par l’utilisateur.
ms.custom: seo-dt-2019
ms.date: 11/21/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: e3f8009c-319d-4d7b-8993-828e55ccde11
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: a41ac84e7e34e0df190c725d9dbadf84f15cac47
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/01/2021
ms.locfileid: "100353635"
---
# <a name="transact-sql-constructs-not-supported-by-in-memory-oltp"></a>Constructions Transact-SQL non prises en charge par l’OLTP en mémoire
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Les tables optimisées en mémoire, les procédures stockées compilées en mode natif et les fonctions définies par l’utilisateur ne prennent pas en charge la totalité de la surface d’exposition [!INCLUDE[tsql](../../includes/tsql-md.md)] qui est prise en charge par les tables sur disque, les procédures stockées [!INCLUDE[tsql](../../includes/tsql-md.md)] interprétées et les fonctions définies par l’utilisateur. Lorsque vous tentez d'utiliser une des fonctionnalités non prises en charge, le serveur retourne une erreur.  
  
 Le texte du message d'erreur indique le type d'instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] (fonctionnalité, opération, option, par exemple), ainsi que le nom de la fonctionnalité ou du mot clé [!INCLUDE[tsql](../../includes/tsql-md.md)] . La plupart des fonctionnalités qui ne sont pas prises en charge retournent l'erreur 10794, avec le texte du message d'erreur indiquant la fonctionnalité non prise en charge. Les tableaux suivants répertorient les fonctionnalités et les mots clés [!INCLUDE[tsql](../../includes/tsql-md.md)] qui peuvent s'afficher dans le texte du message d'erreur, ainsi que l'action corrective à entreprendre pour résoudre l'erreur.  
  
 Pour plus d'informations sur les fonctionnalités prises en charge avec les tables optimisées en mémoire et les procédures stockées compilées en mode natif, consultez :  
  
-   [Problèmes de migration pour les procédures stockées compilées en mode natif](./a-guide-to-query-processing-for-memory-optimized-tables.md)  
  
-   [Prise en charge de Transact-SQL pour OLTP en mémoire](../../relational-databases/in-memory-oltp/transact-sql-support-for-in-memory-oltp.md)  
  
-   [Fonctionnalités SQL Server non prises en charge pour l’OLTP en mémoire](../../relational-databases/in-memory-oltp/unsupported-sql-server-features-for-in-memory-oltp.md)  
  
-   [Procédures stockées compilées en mode natif](./a-guide-to-query-processing-for-memory-optimized-tables.md)  
  
## <a name="databases-that-use-in-memory-oltp"></a>Bases de données qui utilisent OLTP en mémoire  
 Le tableau suivant répertorie les fonctionnalités [!INCLUDE[tsql](../../includes/tsql-md.md)] non prises en charge, ainsi que les mots clés qui peuvent apparaître dans le texte du message d’une erreur impliquant une base de données OLTP en mémoire. Ce tableau indique également la résolution de l’erreur.  
  
|Type|Nom|Résolution|  
|----------|----------|----------------|  
|Option|AUTO_CLOSE|L'option de base de données AUTO_CLOSE=ON n'est pas prise en charge par les bases de données avec un groupe de fichiers MEMORY_OPTIMIZED_DATA.|  
|Option|ATTACH_REBUILD_LOG|L'option de base de données CREATE ATTACH_REBUILD_LOG n'est pas prise en charge par les bases de données avec un groupe de fichiers MEMORY_OPTIMIZED_DATA.|  
|Fonctionnalité|DATABASE SNAPSHOT|La création d'instantanés de base de données n'est pas prise en charge par les bases de données avec un groupe de fichiers MEMORY_OPTIMIZED_DATA.|  
|Fonctionnalité|Réplication à l'aide de sync_method 'database snapshot' ou 'database snapshot character'|La réplication à l'aide de sync_method 'database snapshot' ou 'database snapshot character' n'est pas prise en charge par les bases de données avec un groupe de fichiers MEMORY_OPTIMIZED_DATA.|  
|Fonctionnalité|DBCC CHECKDB<br /><br /> DBCC CHECKTABLE|DBCC CHECKDB ignore les tables optimisées en mémoire dans la base de données.<br /><br /> DBCC CHECKTABLE échoue pour les tables optimisées en mémoire.|  
  
## <a name="memory-optimized-tables"></a>Tables optimisées en mémoire  
 Le tableau suivant répertorie les fonctionnalités [!INCLUDE[tsql](../../includes/tsql-md.md)] non prises en charge, ainsi que les mots clés qui peuvent apparaître dans le texte du message d’une erreur impliquant une table optimisée en mémoire. Ce tableau indique également la résolution de l’erreur.  
  
|Type|Nom|Résolution|  
|----------|----------|----------------|  
|Fonctionnalité|ACTIVÉ|Les tables optimisées en mémoire ne peuvent pas être placées sur un groupe de fichiers ou un schéma de partition. Supprimez la clause ON de l'instruction **CREATE TABLE** .<br /><br /> Toutes les tables optimisées en mémoire sont associées à un groupe de fichiers/données optimisé en mémoire.|  
|Type de données|*Nom du type de données*|Le type de données spécifié n'est pas pris en charge. Remplacez le type par un des types de données pris en charge. Pour plus d’informations, consultez [Types de données pris en charge pour l’OLTP en mémoire](../../relational-databases/in-memory-oltp/supported-data-types-for-in-memory-oltp.md).|  
|Fonctionnalité|Colonnes calculées|**S’applique à :** [!INCLUDE[ssSQL14-md](../../includes/sssql14-md.md)] et [!INCLUDE[ssSQL15-md](../../includes/sssql16-md.md)]<br/>Les colonnes calculées ne sont pas prises en charge pour les tables mémoire optimisées. Supprimez les colonnes calculées de l'instruction **CREATE TABLE** .<br/><br/>[!INCLUDE[ssSDSFull_md](../../includes/ssSDSFull-md.md)] et SQL Server (à partir de [!INCLUDE[sssql17-md](../../includes/sssql17-md.md)]) prennent en charge les colonnes calculées dans les tables et index à mémoire optimisée.|  
|Fonctionnalité|Réplication|La réplication n'est pas pris en charge avec les tables mémoire optimisées.|  
|Fonctionnalité|FILESTREAM|Le stockage FILESTREAM n'est pas pris en charge pour les colonnes de tables mémoire optimisées. Supprimez le mot clé **FILESTREAM** de la définition de colonne.|  
|Fonctionnalité|SPARSE|Les colonnes de tables mémoire optimisées ne peuvent pas être définies comme SPARSE. Supprimez le mot clé **SPARSE** de la définition de colonne.|  
|Fonctionnalité|ROWGUIDCOL|L'option ROWGUIDCOL n'est pas prise en charge pour les colonnes de tables mémoire optimisées. Supprimez le mot clé **ROWGUIDCOL** de la définition de colonne.|  
|Fonctionnalité|FOREIGN KEY|**S’applique à :** [!INCLUDE[ssSDSFull_md](../../includes/ssSDSFull-md.md)] et SQL Server à partir de [!INCLUDE[ssSQL15-md](../../includes/sssql16-md.md)]<br/>Pour les tables à mémoire optimisée, les contraintes FOREIGN KEY ne sont prises en charge que pour les clés étrangères qui référencent des clés primaires d’autres tables à mémoire optimisée. Supprimez la contrainte de la définition de table si la clé étrangère fait référence à une contrainte unique.<br/><br/>Dans [!INCLUDE[ssSQL14-md](../../includes/sssql14-md.md)], les contraintes FOREIGN KEY ne sont pas prises en charge par les tables à mémoire optimisée.|  
|Fonctionnalité|index cluster|Spécifiez un index non cluster. Dans le cas d’un index de clé primaire, veillez à spécifier **PRIMARY KEY NONCLUSTERED**.|  
|Fonctionnalité|Transactions dans DDL|Les tables mémoire optimisées et les procédures stockées compilées en mode natif ne peuvent pas être créées ou supprimées dans le contexte d'une transaction utilisateur. Ne commencez pas de transaction et veillez à ce que le paramètre de session IMPLICIT_TRANSACTIONS ait la valeur OFF avant d'exécuter l'instruction CREATE ou DROP.|  
|Fonctionnalité|déclencheurs DDL|Les tables mémoire optimisées et les procédures stockées compilées en mode natif ne peuvent pas être créées ou supprimées s'il existe un déclencheur de serveur ou de base de données pour cette opération DDL. Supprimez les déclencheurs de serveur et base de données dans l'instruction CREATE/DROP TABLE et CREATE/DROP PROCEDURE.|  
|Fonctionnalité|EVENT NOTIFICATION|Les tables mémoire optimisées et les procédures stockées compilées en mode natif ne peuvent pas être créées ou supprimées s'il existe une notification d'événement de serveur ou de base de données pour cette opération DDL. Supprimez les notifications d'événement de serveur et de base de données sur CREATE TABLE ou DROP TABLE et CREATE PROCEDURE ou DROP PROCEDURE.|  
|Fonctionnalité|FileTable|Les tables mémoire optimisées ne peuvent pas être créées en tant que tables de fichiers. Supprimez l'argument **AS FileTable** de l'instruction **CREATE TABLE** .|  
|Opération|Mettre à jour les colonnes clés primaires|Les colonnes clés primaires des tables mémoire optimisées et les types de table ne peuvent pas être mis à jour. Si la clé primaire doit être mise à jour, supprimez l'ancienne ligne et insérez la nouvelle ligne avec la clé primaire mise à jour.|  
|Opération|CREATE INDEX|Les index sur les tables mémoire optimisées doivent être spécifiés avec l’instruction **CREATE TABLE** ou l’instruction **ALTER TABLE** .|  
|Opération|CREATE FULLTEXT INDEX|Les index de recherche en texte intégral ne sont pas pris en charge pour les tables mémoire optimisées.|  
|Opération|modification de schéma|Les tables à mémoire optimisée et les procédures stockées compilées en mode natif ne prennent pas en charge certaines modifications de schéma :<br/> [!INCLUDE[ssSDSFull_md](../../includes/ssSDSFull-md.md)] et SQL Server à compter de [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] : les opérations ALTER TABLE, ALTER PROCEDURE et sp_rename sont prises en charge. Les autres modifications de schéma, telles que l’ajout des propriétés étendues, ne sont pas prises en charge.<br/><br/>[!INCLUDE[ssSQL15-md](../../includes/sssql16-md.md)]: les opérations ALTER TABLE et ALTER PROCEDURE sont prises en charge. Les autres modifications de schéma, telles que sp_rename, ne sont pas prises en charge.<br/><br/>[!INCLUDE[ssSQL14-md](../../includes/sssql14-md.md)] : les modifications de schéma ne sont pas prises en charge. Pour modifier la définition d’une table à mémoire optimisée ou d’une procédure stockée compilée en mode natif, supprimez l’objet, puis recréez-le avec la définition souhaitée.| 
|Opération|TRUNCATE TABLE|L'opération TRUNCATE n'est pas prise en charge pour les tables mémoire optimisées. Pour supprimer toutes les lignes d’une table, supprimez toutes les lignes en utilisant **DELETE FROM**_table_ ou supprimez la table et recréez-la.|  
|Opération|ALTER AUTHORIZATION|La modification du propriétaire d'une table mémoire optimisée ou d'une procédure stockée compilée en mode natif existante n'est pas prise en charge. Supprimez ou recréez la table ou la procédure pour modifier la propriété.|  
|Opération|ALTER SCHEMA|Il n’est pas possible de transférer le schéma d’une table à mémoire optimisée ou d’une procédure stockée compilée en mode natif existantes vers un autre schéma. Pour changer de schéma, supprimez, puis recréez l’objet.|  
|Opération|DBCC CHECKTABLE|DBCC CHECKTABLE n’est pas pris en charge par les tables à mémoire optimisée. Pour vérifier l’intégrité des fichiers de point de contrôle sur disque, effectuez une sauvegarde du groupe de fichiers MEMORY_OPTIMIZED_DATA.|  
|Fonctionnalité|ANSI_PADDING OFF|L’option de session **ANSI_PADDING** doit être activée (ON) lorsque vous créez des tables optimisées en mémoire ou des procédures stockées compilées en mode natif. Exécutez **SET ANSI_PADDING ON** avant d’exécuter l’instruction CREATE.|  
|Option|DATA_COMPRESSION|La compression des données n'est pas prise en charge pour les tables optimisées en mémoire. Supprimez l'option de la définition de table.|  
|Fonctionnalité|DTC|Les tables optimisées en mémoire et les procédures stockées compilées en mode natif ne sont pas accessibles à partir de transactions distribuées. Utilisez plutôt des transactions SQL.|  
|Opération|Tables optimisées en mémoire comme cibles de MERGE|Les tables optimisées en mémoire ne peuvent pas être la cible d’une opération **MERGE** . Utilisez des instructions **INSERT**, **UPDATE** ou **DELETE** à la place.|  
  
## <a name="indexes-on-memory-optimized-tables"></a>Index sur des tables optimisées en mémoire  
 Le tableau suivant répertorie les fonctionnalités et les mots clés [!INCLUDE[tsql](../../includes/tsql-md.md)] qui peuvent s'afficher dans le texte d'un message d'erreur qui implique un index sur une table optimisée en mémoire, ainsi que l'action corrective à entreprendre pour résoudre l'erreur.  
  
|Type|Nom|Résolution|  
|----------|----------|----------------|  
|Fonctionnalité|Index filtré|Les index filtrés ne sont pris en charge avec les tables optimisées en mémoire. Omettez la clause **WHERE** de la spécification d'index.|  
|Fonctionnalité|Colonnes incluses|La spécification de colonnes incluses n'est pas nécessaire pour les tables mémoire optimisées. Toutes les colonnes de la table mémoire optimisée sont incluses implicitement dans chaque index mémoire optimisé.|  
|Opération|DROP INDEX|La suppression des index sur les tables mémoire optimisées n'est pas prise en charge. Vous pouvez supprimer des index à l’aide de ALTER TABLE.<br /><br /> Pour plus d’informations, consultez [Modification des tables mémoire optimisées](../../relational-databases/in-memory-oltp/altering-memory-optimized-tables.md).|  
|Option d'index|*Option d’index*|Une seule option d’index est prise en charge : BUCKET_COUNT pour les index de hachage.|  
  
## <a name="nonclustered-hash-indexes"></a>Index de hachage non cluster  
 Le tableau suivant répertorie les fonctionnalités et les mots clés [!INCLUDE[tsql](../../includes/tsql-md.md)] qui peuvent s'afficher dans le texte d'un message d'erreur qui implique un index de hachage non cluster, ainsi que l'action corrective à entreprendre pour résoudre l'erreur.  
  
|Type|Nom|Résolution|  
|----------|----------|----------------|  
|Option|ASC/DESC|Les index de hachage non cluster ne sont pas ordonnés. Supprimez les mots clés **ASC** et **DESC** de la spécification de clé d'index.|  
  
## <a name="natively-compiled-stored-procedures-and-user-defined-functions"></a>Procédures stockées compilées en mode natif et fonctions définies par l’utilisateur  
 Le tableau suivant répertorie les fonctionnalités [!INCLUDE[tsql](../../includes/tsql-md.md)] et les mots clés qui peuvent s’afficher dans le texte du message d’une erreur impliquant des procédures stockées compilées en mode natif et des fonctions définies par l’utilisateur, ainsi que l’action corrective à mettre en œuvre pour résoudre l’erreur.  
  
|Type|Fonctionnalité|Résolution|  
|----------|-------------|----------------|  
|Fonctionnalité|Variables de table inline|Les types de table ne peuvent pas être déclarés inline avec des déclarations de variable. Les types de table doivent être déclarés explicitement à l'aide d'une instruction **CREATE TYPE** .|  
|Fonctionnalité|Curseurs|Les curseurs ne sont pas pris en charge sur ou dans les procédures stockées compilées en mode natif.<br /><br /> Lors de l'exécution de la procédure à partir du client, utilisez RPC plutôt que l'API de curseur. Avec ODBC, évitez l'instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] . **EXECUTE**, et spécifiez directement le nom de la procédure à la place.<br /><br /> Lors de l'exécution de la procédure à partir d'une autre procédure stockée ou d'un lot [!INCLUDE[tsql](../../includes/tsql-md.md)] , évitez d'utiliser un curseur avec la procédure stockée compilée en mode natif.<br /><br /> Lors de la création d’une procédure stockée compilée en mode natif, plutôt que d’utiliser un curseur, utilisez une logique basée sur un ensemble ou une boucle **WHILE** .|  
|Fonctionnalité|Valeurs par défaut non constantes des paramètres|Lors de l'utilisation des valeurs par défaut avec des paramètres sur les procédures stockées compilées en mode natif, les valeurs doivent être constantes. Supprimez les caractères génériques des déclarations de paramètre.|  
|Fonctionnalité|EXTERNAL|Les procédures stockées CLR ne peuvent pas être compilées en mode natif. Supprimez la clause AS EXTERNAL ou l'option NATIVE_COMPILATION de l'instruction CREATE PROCEDURE.|  
|Fonctionnalité|Procédures stockées numérotées|Les procédures stockées compilées en mode natif ne peuvent pas être numérotées. Supprimez le **;** _numéro_ de l’instruction **CREATE PROCEDURE** .|  
|Fonctionnalité|Instructions INSERT ... VALUES multilignes|Impossible d'insérer plusieurs lignes en utilisant la même instruction **INSERT** dans une procédure stockée compilée en mode natif. Créez des instructions **INSERT** pour chaque ligne.|  
|Fonctionnalité|Expressions de table communes|Les expressions de table communes ne sont pas prises en charge dans les procédures stockées compilées en mode natif. Réécrire la requête.|  
|Fonctionnalité|COMPUTE|La clause **COMPUTE** n'est pas prise en charge. Supprimez-la de la requête.|  
|Fonctionnalité|SELECT INTO|La clause **INTO** n'est pas prise en charge avec l'instruction **SELECT** . Réécrivez la requête ainsi : **INSERT INTO** _Table_ **SELECT**.|  
|Fonctionnalité|liste de colonnes d'insertion incomplète|En général, les instructions INSERT doivent spécifier des valeurs pour toutes les colonnes de la table.<br /><br /> Toutefois, nous prenons en charge les contraintes DEFAULT et les colonnes IDENTITY(1,1) sur les tables optimisées en mémoire. Ces colonnes peuvent être (et c’est une obligation dans le cas des colonnes IDENTITY) omises de la liste des colonnes INSERT.|  
|Fonctionnalité|*Fonction*|Certaines fonctions intégrées ne sont pas prises en charge dans les procédures stockées compilées en mode natif. Supprimez la fonction rejetée de la procédure stockée. Pour plus d’informations sur les fonctions intégrées prises en charge, consultez<br />[Fonctionnalités prises en charge pour les modules T-SQL compilés en mode natif](../../relational-databases/in-memory-oltp/supported-features-for-natively-compiled-t-sql-modules.md), ou<br />[Procédures stockées compilées en mode natif](./a-guide-to-query-processing-for-memory-optimized-tables.md).|  
|Fonctionnalité|CASE|**S’applique à :** [!INCLUDE[ssSQL14-md](../../includes/sssql14-md.md)] et SQL Server à partir de [!INCLUDE[ssSQL15-md](../../includes/sssql16-md.md)]<br/>Les expressions **CASE** ne sont pas prises en charge dans les requêtes des procédures stockées compilées en mode natif. Créez des requêtes pour chaque cas. Pour plus d’informations, consultez [Implémentation d’une expression CASE dans une procédure stockée compilée en mode natif](../../relational-databases/in-memory-oltp/implementing-a-case-expression-in-a-natively-compiled-stored-procedure.md).<br/><br/>[!INCLUDE[ssSDSFull_md](../../includes/ssSDSFull-md.md)] et SQL Server (à partir de [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)]) prennent en charge les expressions CASE.|  
|Fonctionnalité|INSERT EXECUTE|Supprimez la référence.|  
|Fonctionnalité|Exécutez|Prise en charge uniquement pour exécuter des procédures stockées compilées en mode natif et des fonctions définies par l’utilisateur.|  
|Fonctionnalité|agrégats définis par l'utilisateur|Les fonctions d'agrégation définies par l'utilisateur ne peuvent pas être utilisées dans les procédures stockées compilées en mode natif. Supprimez la référence à la fonction dans la procédure.|  
|Fonctionnalité|métadonnées de modes de navigation|Les procédures stockées compilées en mode natif ne prennent pas en charge les métadonnées de modes de navigation. Assurez-vous que l’option de session **NO_BROWSETABLE** est désactivée (OFF).|  
|Fonctionnalité|DELETE avec la clause FROM|La clause **FROM** n'est pas prise en charge pour les instructions **DELETE** avec une table source dans les procédures stockées compilées en mode natif.<br /><br /> **DELETE** avec la clause **FROM** est prise en charge lorsqu'elle est utilisée pour indiquer de la table dans laquelle la suppression doit avoir lieu.|  
|Fonctionnalité|UPDATE avec la clause FROM|La clause **FROM** n'est pas prise en charge pour les instructions **UPDATE** dans les procédures stockées compilées en mode natif.| 
|Fonctionnalité|procédures temporaires|Les procédures stockées temporaires ne peuvent pas être compilées en mode natif. Créez une procédure stockée compilée en mode natif permanente ou une procédure stockée [!INCLUDE[tsql](../../includes/tsql-md.md)] interprétée.|  
|Niveau d'isolation|READ UNCOMMITTED|Le niveau d'isolation READ UNCOMMITTED n'est pas pris en charge pour les procédures stockées compilées en mode natif. Utilisez un niveau d'isolation pris en charge, tel que SNAPSHOT.|  
|Niveau d'isolation|READ COMMITTED|Le niveau d’isolation READ UNCOMMITTED n’est pas pris en charge dans les procédures stockées compilées en mode natif. Utilisez un niveau d'isolation pris en charge, tel que SNAPSHOT.|  
|Fonctionnalité|tables temporaires|Les tables de tempdb ne peuvent pas être utilisées dans les procédures stockées compilées en mode natif. À la place, utilisez une variable de table ou une table mémoire optimisée avec DURABILITY=SCHEMA_ONLY.|  
|Fonctionnalité|DTC|Les tables optimisées en mémoire et les procédures stockées compilées en mode natif ne sont pas accessibles à partir de transactions distribuées. Utilisez plutôt des transactions SQL.|  
|Fonctionnalité|EXECUTE WITH RECOMPILE|L'option **WITH RECOMPILE** n'est pas pris en charge avec les procédures stockées compilées en mode natif.|  
|Fonctionnalité|Exécution à partir de la connexion administrateur dédiée.|Les procédures stockées compilées en mode natif ne peuvent pas être exécutées à partir de la connexion administrateur dédiée. Utilisez plutôt une connexion normale.|  
|Opération|point d'enregistrement|Les procédures stockées compilées en mode natif ne peuvent pas être appelées à partir de transactions qui possèdent un point de sauvegarde actif. Supprimez le point de sauvegarde de la transaction.|  
|Opération|ALTER AUTHORIZATION|La modification du propriétaire d'une table mémoire optimisée ou d'une procédure stockée compilée en mode natif existante n'est pas prise en charge. Supprimez ou recréez la table ou la procédure pour modifier la propriété.|  
|Opérateur|OPENROWSET|Cet opérateur n'est pas pris en charge. Supprimez **OPENROWSET** de la procédure stockée compilée en mode natif.|  
|Opérateur|OPENQUERY|Cet opérateur n'est pas pris en charge. Supprimez **OPENQUERY** de la procédure stockée compilée en mode natif.|  
|Opérateur|OPENDATASOURCE|Cet opérateur n'est pas pris en charge. Supprimez **OPENDATASOURCE** de la procédure stockée compilée en mode natif.|  
|Opérateur|OPENXML|Cet opérateur n'est pas pris en charge. Supprimez **OPENXML** de la procédure stockée compilée en mode natif.|  
|Opérateur|CONTAINSTABLE|Cet opérateur n'est pas pris en charge. Supprimez **CONTAINSTABLE** de la procédure stockée compilée en mode natif.|  
|Opérateur|FREETEXTTABLE|Cet opérateur n'est pas pris en charge. Supprimez **FREETEXTTABLE** de la procédure stockée compilée en mode natif.|  
|Fonctionnalité|fonctions table|Les fonctions table ne peuvent pas être référencées à partir de procédures stockées compilées en mode natif. Une solution de contournement possible pour cette restriction consiste à ajouter la logique des fonctions tables au corps de la procédure.|  
|Opérateur|CHANGETABLE|Cet opérateur n'est pas pris en charge. Supprimez **CHANGETABLE** de la procédure stockée compilée en mode natif.|  
|Opérateur|GOTO|Cet opérateur n'est pas pris en charge. Utilisez d'autres constructions de procédure, telles que WHILE.|  
|Opérateur|OFFSET|Cet opérateur n'est pas pris en charge. Supprimez **OFFSET** de la procédure stockée compilée en mode natif.|  
|Opérateur|INTERSECT|Cet opérateur n'est pas pris en charge. Supprimez **INTERSECT** de la procédure stockée compilée en mode natif. Dans certain cas, INNER JOIN permet d'obtenir le même résultat.|  
|Opérateur|EXCEPT|Cet opérateur n'est pas pris en charge. Supprimez **EXCEPT** de la procédure stockée compilée en mode natif.|  
|Opérateur|APPLY|**S’applique à :** [!INCLUDE[ssSQL14-md](../../includes/sssql14-md.md)] et SQL Server à partir de [!INCLUDE[ssSQL15-md](../../includes/sssql16-md.md)]<br/>Cet opérateur n'est pas pris en charge. Supprimez **APPLY** de la procédure stockée compilée en mode natif.<br/><br/>[!INCLUDE[ssSDSFull_md](../../includes/ssSDSFull-md.md)] et SQL Server (à partir de [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)]) prennent en charge l’opérateur APPLY dans les modules compilés en mode natif.|  
|Opérateur|PIVOT|Cet opérateur n'est pas pris en charge. Supprimez **PIVOT** de la procédure stockée compilée en mode natif.|  
|Opérateur|UNPIVOT|Cet opérateur n'est pas pris en charge. Supprimez **UNPIVOT** de la procédure stockée compilée en mode natif.|  
|Opérateur|CONTAINS|Cet opérateur n'est pas pris en charge. Supprimez **CONTAINS** de la procédure stockée compilée en mode natif.|  
|Opérateur|FREETEXT|Cet opérateur n'est pas pris en charge. Supprimez **FREETEXT** de la procédure stockée compilée en mode natif.|  
|Opérateur|TSEQUAL|Cet opérateur n'est pas pris en charge. Supprimez **TSEQUAL** de la procédure stockée compilée en mode natif.|  
|Opérateur|LIKE|Cet opérateur n'est pas pris en charge. Supprimez **LIKE** de la procédure stockée compilée en mode natif.|  
|Opérateur|NEXT VALUE FOR|Les séquences ne peuvent pas être référencées dans les procédures stockées compilées en mode natif. Obtenez la valeur en utilisant [!INCLUDE[tsql](../../includes/tsql-md.md)]interprété, puis passez-la dans la procédure stockée compilée en mode natif. Pour plus d’informations, consultez [Implémentation d’IDENTITY dans une table mémoire optimisée](../../relational-databases/in-memory-oltp/implementing-identity-in-a-memory-optimized-table.md).|  
|Option Set|*option*|Les options SET peuvent être modifiées dans les procédures stockées compilées en mode natif. Certaines options peuvent être définies avec l'instruction BEGIN ATOMIC. Pour plus d'informations, consultez la section sur les blocs atomiques dans [Natively Compiled Stored Procedures](./a-guide-to-query-processing-for-memory-optimized-tables.md).|  
|Opérande|TABLESAMPLE|Cet opérateur n'est pas pris en charge. Supprimez **TABLESAMPLE** de la procédure stockée compilée en mode natif.|  
|Option|RECOMPILE|Les procédures stockées compilées en mode natif sont compilées au moment de la création. Supprimez **RECOMPILE** de la définition de procédure.<br /><br /> Vous pouvez exécuter sp_recompile sur une procédure stockée compilée en mode natif, ce qui entraîne sa recompilation lors de la prochaine exécution.|  
|Option|ENCRYPTION|Cette option n'est pas prise en charge. Supprimez **ENCRYPTION** de la définition de procédure.|  
|Option|FOR REPLICATION|Les procédures stockées compilées en mode natif ne peuvent pas être créées pour la réplication. Supprimez **FOR REPLICATION** de la définition de procédure.|  
|Option|FOR XML|Cette option n'est pas prise en charge. Supprimez **FOR XML** de la procédure stockée compilée en mode natif.|  
|Option|FOR BROWSE|Cette option n'est pas prise en charge. Supprimez **FOR BROWSE** de la procédure stockée compilée en mode natif.|  
|Indicateur de jointure|HASH, MERGE|Les procédures stockées compilées en mode natif ne prennent en charge que les jointures de boucles imbriquées. Les jointures de hachage et de fusion ne sont pas prises en charge. Supprimez l'indicateur de jointure.|  
|Indicateur de requête|*Indicateur de requête*|Cet indicateur de requête n'est pas pris en charge dans les procédures stockées compilées en mode natif. Pour obtenir les indicateurs de requête pris en charge, consultez [Indicateurs de requête &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-query.md).|  
|Option|PERCENT|Cette option n'est pas prise en charge avec les clauses **TOP** . Supprimez **PERCENT** de la requête dans la procédure stockée compilée en mode natif.|  
|Option|WITH TIES|**S’applique à :** [!INCLUDE[ssSDS14_md](../../includes/sssql14-md.md)] et [!INCLUDE[ssSQL15-md](../../includes/sssql16-md.md)]<br/>Cette option n'est pas prise en charge avec les clauses **TOP** . Supprimez **WITH TIES** de la requête dans la procédure stockée compilée en mode natif.<br/><br/>[!INCLUDE[ssSDSFull_md](../../includes/ssSDSFull-md.md)] et SQL Server (à partir de [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)]) prennent en charge **TOP WITH TIES**.|  
|Fonction d'agrégation|*Fonction d’agrégation*|Certaines fonctions d’agrégation ne sont pas prises en charge. Pour plus d’informations sur les fonctions d’agrégation prises en charge dans les modules T-SQL compilés en mode natif, consultez [Fonctionnalités prises en charge pour les modules T-SQL compilés en mode natif](../../relational-databases/in-memory-oltp/supported-features-for-natively-compiled-t-sql-modules.md).|  
|Fonction de classement|*Fonction de classement*|Les fonctions de classement ne sont pas prises en charge dans les procédures stockées compilées en mode natif. Supprimez-les de la définition de procédure.|  
|Fonction|*Fonction*|Cette fonction n'est pas prise en charge. Pour plus d’informations sur les fonctions d’agrégation prises en charge dans les modules T-SQL compilés en mode natif, consultez [Fonctionnalités prises en charge pour les modules T-SQL compilés en mode natif](../../relational-databases/in-memory-oltp/supported-features-for-natively-compiled-t-sql-modules.md).|  
|.|*Instruction*|Cette instruction n'est pas prise en charge. Pour plus d’informations sur les fonctions d’agrégation prises en charge dans les modules T-SQL compilés en mode natif, consultez [Fonctionnalités prises en charge pour les modules T-SQL compilés en mode natif](../../relational-databases/in-memory-oltp/supported-features-for-natively-compiled-t-sql-modules.md).|  
|Fonctionnalité|MIN et MAX utilisé avec des chaînes binaires et de caractères|Les fonctions d'agrégation **MIN** et **MAX** ne peuvent pas être utilisées pour les valeurs de chaîne de caractère et binaire dans les procédures stockées compilées en mode natif.|  
|Fonctionnalité|GROUP BY ALL|ALL ne peut pas être utilisé avec les clauses GROUP BY dans des procédures stockées compilées en mode natif. Supprimez ALL de la clause GROUP BY.|  
|Fonctionnalité|GROUP BY ()|Le regroupement par une liste vide n'est pas pris en charge. Supprimez la clause GROUP BY, ou incluez des colonnes dans la liste de regroupement.|  
|Fonctionnalité|ROLLUP|**ROLLUP** ne peut pas être utilisé avec les clauses **GROUP BY** dans des procédures stockées compilées en mode natif. Supprimez **ROLLUP** de la définition de procédure.|  
|Fonctionnalité|CUBE|**CUBE** ne peut pas être utilisé avec les clauses **GROUP BY** dans des procédures stockées compilées en mode natif. Supprimez **CUBE** de la définition de procédure.|  
|Fonctionnalité|GROUPING SETS|**GROUPING SETS** ne peut pas être utilisé avec les clauses **GROUP BY** dans des procédures stockées compilées en mode natif. Supprimez **GROUPING SETS** de la définition de procédure.|  
|Fonctionnalité|BEGIN TRANSACTION, COMMIT TRANSACTION et ROLLBACK TRANSACTION|Utilisez des blocs ATOMIC pour contrôler les transactions et la gestion des erreurs. Pour plus d’informations, consultez [Atomic Blocks](../../relational-databases/in-memory-oltp/atomic-blocks-in-native-procedures.md).|  
|Fonctionnalité|Déclarations de variables de table inline.|Les variables de table doivent référencer les types de tables mémoire optimisées définis. Vous devez créer un type de table mémoire optimisée et utiliser ce type pour la déclaration de variable, plutôt que spécifier le type inline.|  
|Fonctionnalité|Tables sur disque|Les tables sur disque ne sont pas accessibles à partir de procédures stockées compilées en mode natif. Supprimez les références aux tables sur disque dans les procédures stockées compilées en mode natif. Sinon, migrez les tables sur disques vers des tables mémoire optimisées.|  
|Fonctionnalité|Les vues|Les vues ne sont pas accessibles à partir de procédures stockées compilées en mode natif. Au lieu d'utiliser des vues, référencez les tables de base sous-jacentes.|  
|Fonctionnalité|Fonctions table|**S’applique à** : [!INCLUDE[ssSDSFull_md](../../includes/ssSDSFull-md.md)] et SQL Server à partir de [!INCLUDE[ssSQL15-md](../../includes/sssql16-md.md)]<br/>Les fonctions table à instructions multiples ne sont pas accessibles à partir des modules T-SQL compilés en mode natif. Les fonctions table inline sont prises en charge, mais elles doivent être créées avec WITH NATIVE_COMPILATION.<br/><br/>**S’applique à** : [!INCLUDE[ssSQL14-md](../../includes/ssSQL14-md.md)]<br/>Les fonctions table ne peuvent pas être référencées à partir des modules T-SQL compilés en mode natif.|  
|Option|PRINT|Supprimer la référence|  
|Fonctionnalité|DDL|No DDL est pris en charge dans les modules T-SQL compilés en mode natif.|  
|Option|STATISTICS XML|Non pris en charge. Lorsque vous exécutez une requête, avec l’option STATISTICS XML activée, le contenu XML est renvoyé sans la partie de la procédure stockée compilée en mode natif.|  
  
## <a name="transactions-that-access-memory-optimized-tables"></a>Transactions qui accèdent aux tables mémoire optimisées  
 Le tableau suivant répertorie les fonctionnalités et les mots clés [!INCLUDE[tsql](../../includes/tsql-md.md)] qui peuvent s'afficher dans le texte d'un message d'erreur qui implique des transactions qui accèdent aux tables mémoire optimisées, ainsi que l'action corrective à entreprendre pour résoudre l'erreur.  
  
|Type|Nom|Résolution|  
|----------|----------|----------------|  
|Fonctionnalité|point d'enregistrement|La création de points de sauvegarde dans des transactions qui accèdent aux tables mémoire optimisées n'est pas prise en charge.|  
|Fonctionnalité|transaction liée|Les sessions liées ne peuvent pas participer dans des transactions qui accèdent aux tables mémoire optimisées. Ne liez pas la session avant d'exécuter la procédure.|  
|Fonctionnalité|DTC|Les transactions qui accèdent aux tables mémoire optimisées ne peuvent pas être des transactions distribuées.|  
  
## <a name="see-also"></a>Voir aussi  
 [Migration vers OLTP en mémoire](./plan-your-adoption-of-in-memory-oltp-features-in-sql-server.md)  
  
