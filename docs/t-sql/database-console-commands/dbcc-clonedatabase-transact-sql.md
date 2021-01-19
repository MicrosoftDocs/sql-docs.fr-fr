---
description: DBCC CLONEDATABASE (Transact-SQL)
title: DBCC CLONEDATABASE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 04/23/2019
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CLONEDATABASE
- CLONE DATABASE
- DBCC_CLONEDATABASE_TSQL
- DBCC CLONEDATABASE
- DBCC CLONE DATABASE
- CLONEDATABASE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- database cloning [SQL Server]
- cloning databases
- clone databases
- cloning database
- clone database
- copying databases
- copy databases
- copying database
- copy database
- NO_STATISTICS option
- NO_QUERYSTORE option
- VERIFY_CLONEDB option
- BACKUP_CLONEDB option
- database copying [SQL Server]
- database cloning [SQL Server]
- DBCC CLONEDATABASE statement
ms.assetid: ''
author: bluefooted
ms.author: pamela
manager: amitban
ms.openlocfilehash: 2de4f0e84b39d1384e342eab3b7b3d0bfd101611
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/13/2021
ms.locfileid: "98172581"
---
# <a name="dbcc-clonedatabase-transact-sql"></a>DBCC CLONEDATABASE (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Génère un clone de schéma uniquement d’une base de données avec DBCC CLONEDATABASE pour identifier la cause des problèmes de performances liés à l’optimiseur de requête.

![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
DBCC CLONEDATABASE   
(  
    source_database_name
    ,  target_database_name
)
    [ WITH { [ NO_STATISTICS ] [ , NO_QUERYSTORE ] [ , VERIFY_CLONEDB | SERVICEBROKER ] [ , BACKUP_CLONEDB ] } ]     
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
*source_database_name*  
Nom de la base de données à copier. 
  
*target_database_name*  
Nom de la base de données dans laquelle la base de données est copiée. Cette base de données est créée par DBCC CLONEDATABASE et ne doit pas déjà exister. 
  
NO_STATISTICS  
Indique si les statistiques de table/index doivent être exclues du clone. Si cette option n’est pas spécifiée, les statistiques de table/index sont automatiquement incluses. Cette option est disponible à partir de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP2 CU3 et de [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] SP1.

NO_QUERYSTORE<br>
Précise si les données du magasin des requêtes doivent être exclues du clone. Si cette option n’est pas spécifiée, les données du magasin des requêtes sont copiées dans le clone si le magasin des requêtes est activé dans la base de données source. Cette option est disponible à partir de [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] SP1.

VERIFY_CLONEDB  
Vérifie la cohérence de la nouvelle base de données.  Cette option est obligatoire si la base de données clonée est censée être utilisée en production.  L’activation de VERIFY_CLONEDB a aussi pour effet de désactiver la collecte de statistiques et du magasin des requêtes. Cela revient donc à exécuter WITH VERIFY_CLONEDB, NO_STATISTICS, NO_QUERYSTORE.  Cette option est disponible à partir de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP3, [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] SP2 et [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU8.

> [!NOTE]  
> La commande suivante peut être utilisée pour vérifier que la base de données clonée est prête pour la production : <br/>`SELECT DATABASEPROPERTYEX('clone_database_name', 'IsVerifiedClone')`


SERVICEBROKER<br>
Spécifie si les catalogues système liés à Service Broker doivent être inclus dans le clone.  L’option SERVICEBROKER ne peut pas être utilisée en association avec VERIFY_CLONEDB.  Cette option est disponible à partir de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP3, [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] SP2 et [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU8.

BACKUP_CLONEDB  
Crée et vérifie une sauvegarde de la base de données de clonage.  Dans le cas d’une utilisation avec VERIFY_CLONEDB, la base de données de clonage est vérifiée avant que la sauvegarde soit effectuée.  Cette option est disponible à partir de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP3, [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] SP2 et [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU8.
  
## <a name="remarks"></a>Remarques
Les validations suivantes sont effectuées par DBCC CLONEDATABASE. La commande échoue si l’une des validations échoue.
- La base de données source doit être une base de données utilisateur. Le clonage des bases de données système (MASTER, model, msdb, tempdb, distribution, etc.) n’est pas autorisé.
- La base de données source doit être en ligne ou accessible en lecture.
- Une base de données qui utilise le même nom que la base de données de clonage ne doit pas déjà exister.
- La commande n’est pas une transaction utilisateur.

Si toutes les validations réussissent, le clonage de la base de données source est effectué par les opérations suivantes :
- Crée une base de données de destination qui utilise la même structure de fichier que la source, mais avec les tailles de fichier par défaut de la base de données model.
- Crée un instantané interne de la base de données source.
- Copie les métadonnées système de la base de données source vers la base de données de destination.
- Copie l’intégralité du schéma pour tous les objets de la base de données source vers la base de données de destination.
- Copie les statistiques pour tous les index de la base de données source vers la base de données de destination.

> [!NOTE]  
> La nouvelle base de données générée à partir de DBCC CLONEDATABASE est destinée principalement au dépannage et au diagnostic.  Pour que la base de données clonée puisse être utilisée comme base de données de production, l’option VERIFY_CLONEDB doit être utilisée.

Tous les fichiers de la base de données cible héritent des paramètres de taille et de croissance de la base de données model. Les noms de fichiers de la base de données de destination respectent la convention nom_fichier_source_trait de soulignement_numéro aléatoire. Si le nom de fichier généré existe déjà dans le dossier de destination, DBCC CLONEDATABASE échoue.

DBCC CLONEDATABASE ne prend pas en charge la création d’un clone s’il existe des objets utilisateur (tables, index, schémas, rôles, etc.) qui ont été créés dans la base de données model. Si des objets utilisateur sont présents dans la base de données model, le clone de base de données échoue avec le message d’erreur suivant :

```
Msg 2601, Level 14, State 1, Line 1
Cannot insert duplicate key row in object <system table> with unique index 'index name'. The duplicate key value is <key value>   
```

> [!IMPORTANT]
> Si vous avez des index columnstore, consultez [Remarques à prendre en compte lors de l'optimisation des requêtes avec des index Columnstore sur des bases de données clones](https://techcommunity.microsoft.com/t5/SQL-Server/Considerations-when-tuning-your-queries-with-columnstore-indexes/ba-p/385294) pour mettre à jour les statistiques d’index columnstore avant d’exécuter la commande **DBCC CLONEDATABASE**.  À compter de SQL Server 2019, les étapes manuelles listées dans l’article ci-dessus ne seront plus requises, car la commande **DBCC CLONEDATABASE** collecte automatiquement ces informations.

<a name="ctp23"></a>

## <a name="stats-blob-for-columnstore-indexes"></a>Objets blob de statistiques pour les index columnstore

[!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)], `DBCC CLONEDATABASE` capture automatiquement les objets blob de statistiques pour les index columnstore ; aucune étape manuelle n’est donc requise.`DBCC CLONEDATABASE` crée une copie de schéma uniquement d’une base de données qui comporte tous les éléments nécessaires pour résoudre les problèmes de performances des requêtes sans copier les données. Dans les versions précédentes de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], la commande ne copiait pas les statistiques permettant de résoudre avec précision les problèmes des requêtes d’index columnstore, ce qui obligeait l’utilisateur à effectuer des étapes manuelles pour capturer ces informations.

Pour plus d’informations sur la sécurité des données dans les bases de données clonées, consultez [Understanding data security in cloned databases](https://techcommunity.microsoft.com/t5/SQL-Server/Understanding-data-security-in-cloned-databases-created-using/ba-p/385287).

## <a name="internal-database-snapshot"></a>Instantané de base de données interne
DBCC CLONEDATABASE utilise un instantané de base de données interne de la base de données source pour assurer la cohérence transactionnelle nécessaire à l’exécution de la copie. L’utilisation de cet instantané évite les problèmes de blocage et de concurrence pendant l’exécution de ces commandes. Si un instantané ne peut pas être créé, DBCC CLONEDATABASE échoue. 

Les verrous au niveau de la base de données sont conservés pendant les étapes suivantes du processus de copie :
- Validation de la base de données source
- Obtention du verrou S pour la base de données source
- Création d’un instantané de la base de données source
- Création d’une base de données de clonage (base de données vide héritée de la base de données model)
- Obtention du verrou X pour la base de données de clonage
- Copie des métadonnées dans la base de données de clonage
- Libération de tous les verrous de base de données

Dès lors que l’exécution de la commande est terminée, l’instantané interne est supprimé. Les options TRUSTWORTHY et DB_CHAINING sont désactivées sur une base de données clonée. 

## <a name="supported-objects"></a>Objets pris en charge
Seuls les objets suivants peuvent être clonés dans la base de données de destination. Les objets chiffrés sont clonés, mais ne peuvent pas être utilisés dans la base de données de clonage. Les objets qui ne sont pas répertoriés dans la section suivante ne sont pas pris en charge dans le clone : 
- APPLICATION ROLE
- AVAILABILITY GROUP
- COLUMNSTORE INDEX
- CDB
- CDC
- CLR (à partir de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP2 CU3, [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] SP1 et versions ultérieures)
- DATABASE PROPERTIES
- DEFAULT
- FILES AND FILEGROUPS
- Texte intégral (à partir de [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] SP1 CU2)
- FUNCTION
- INDEX
- Connexion
- PARTITION FUNCTION
- PARTITION SCHEME
- PROCEDURE   
> [!NOTE]   
> Les procédures [!INCLUDE[tsql](../../includes/tsql-md.md)] sont prises en charge dans toutes les versions à partir de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP2. Les procédures CLR sont prises en charge à partir de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP2 CU3. Les procédures compilées en mode natif sont prises en charge à partir de [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] SP1.  

- QUERY STORE (à partir de [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] SP1)   
> [!NOTE]   
> Les données du magasin des requêtes ne sont copiées que si cette fonction est activée dans la base de données source. Pour copier les statistiques d’exécution les plus récentes dans le magasin des requêtes, exécutez sp_query_store_flush_db pour vider les statistiques d’exécution dans le magasin des requêtes avant d’exécuter DBCC CLONEDATABASE.  

- ROLE
- RULE
- SCHEMA
- SEQUENCE
- SPATIAL INDEX
- STATISTICS
- SYNONYM
- TABLE
- MEMORY OPTIMIZED TABLES (uniquement dans [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] SP1 et versions ultérieures).
- FILESTREAM AND FILETABLE OBJECTS (à partir de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP2 CU3, [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] SP1 et versions ultérieures). 
- TRIGGER
- TYPE
- UPGRADED DB
- Utilisateur
- VIEW
- XML INDEX
- XML SCHEMA COLLECTION  

## <a name="permissions"></a>Autorisations  
Nécessite l'appartenance au rôle serveur fixe **sysadmin** .

## <a name="error-log-messages"></a>Messages du journal des erreurs
Les messages suivants sont un exemple des messages enregistrés dans le journal des erreurs pendant le processus de clonage :

```
2018-03-26 15:33:56.05 spid53 Database cloning for 'sourcedb' has started with target as 'sourcedb_clone'.

2018-03-26 15:33:56.46 spid53 Starting up database 'sourcedb_clone'.

2018-03-26 15:33:57.80 spid53 Setting database option TRUSTWORTHY to OFF for database 'sourcedb_clone'.

2018-03-26 15:33:57.80 spid53 Setting database option DB_CHAINING to OFF for database 'sourcedb_clone'.

2018-03-26 15:33:57.88 spid53 Starting up database 'sourcedb_clone'.

2018-03-26 15:33:57.91 spid53 Database 'sourcedb_clone' is a cloned database. A cloned database should be used for diagnostic purposes only and is not supported for use in a production environment.

2018-03-26 15:33:57.92 spid53 Database cloning for 'sourcedb' has finished. Cloned database is 'sourcedb_clone'.
```

## <a name="database-properties"></a>Propriétés de la base de données
`DATABASEPROPERTYEX('dbname', 'IsClone')` retourne 1 si la base de données a été générée à l’aide de DBCC CLONEDATABASE.

`DATABASEPROPERTYEX('dbname', 'IsVerifiedClone')` retourne 1 si la base de données a été vérifiée avec succès à l’aide de WITH VERIFY_CLONEDB.

## <a name="examples"></a>Exemples  
  
### <a name="a-creating-a-clone-of-a-database-that-includes-schema-statistics-and-query-store"></a>R. Création d’un clone de base de données qui inclut un schéma, des statistiques et un magasin des requêtes 
L’exemple suivant crée un clone de la base de données AdventureWorks qui inclut un schéma, des statistiques et des données du magasin des requêtes ([!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] SP1 et ultérieur)

```sql  
DBCC CLONEDATABASE (AdventureWorks, AdventureWorks_Clone);    
GO 
```  
  
### <a name="b-creating-a-schema-only-clone-of-a-database-without-statistics"></a>B. Création d’un clone de schéma uniquement d’une base de données sans statistiques 
L’exemple suivant crée un clone de la base de données AdventureWorks qui n’inclut pas de statistiques ([!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP2 CU3 et ultérieur)

```sql  
DBCC CLONEDATABASE (AdventureWorks, AdventureWorks_Clone) WITH NO_STATISTICS;    
GO 
```  

### <a name="c-creating-a-schema-only-clone-of-a-database-without-statistics-and-query-store"></a>C. Création d’un clone de schéma uniquement d’une base de données sans statistiques ni magasin des requêtes 
L’exemple suivant crée un clone de la base de données AdventureWorks qui n’inclut pas de statistiques ni de données du magasin des requêtes ([!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] SP1 et ultérieur)

```sql  
DBCC CLONEDATABASE (AdventureWorks, AdventureWorks_Clone) WITH NO_STATISTICS, NO_QUERYSTORE;    
GO 
```  

### <a name="d-creating-a-clone-of-a-database-that-is-verified-for-production-use"></a>D. Création d’un clone de base de données qui est vérifié pour une utilisation en production
L’exemple suivant crée un clone de schéma uniquement de la base de données AdventureWorks, sans statistiques ni données du magasin des requêtes, qui est vérifié pour une utilisation comme base de données de production ([!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] SP2 et ultérieur).

```sql  
DBCC CLONEDATABASE (AdventureWorks, AdventureWorks_Clone) WITH VERIFY_CLONEDB;    
GO 
```  
  
### <a name="e-creating-a-clone-of-a-database-that-is-verified-for-production-use-that-includes-a-backup-of-the-cloned-database"></a>E. Création d’un clone de base de données qui est vérifié pour une utilisation en production qui inclut une sauvegarde de la base de données clonée
L’exemple suivant crée un clone de schéma uniquement de la base de données AdventureWorks sans statistiques ni données du magasin des requêtes qui est vérifié pour une utilisation comme base de données de production.  Une sauvegarde vérifiée de la base de données clonée est également créée ([!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] SP2 et ultérieur).

```sql  
DBCC CLONEDATABASE (AdventureWorks, AdventureWorks_Clone) WITH VERIFY_CLONEDB, BACKUP_CLONEDB;    
GO 
```

## <a name="see-also"></a>Voir aussi
[DBCC &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-transact-sql.md)    
[Guide pratique pour générer un script des métadonnées de la base de données nécessaires à la création d’une base de données de statistiques uniquement dans SQL Server](https://support.microsoft.com/help/914288)   

