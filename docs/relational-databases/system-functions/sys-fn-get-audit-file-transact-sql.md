---
description: sys.fn_get_audit_file (Transact-SQL)
title: sys.fn_get_audit_file (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 02/19/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- fn_get_audit_file_TSQL
- sys.fn_get_audit_file_TSQL
- fn_get_audit_file
- sys.fn_get_audit_file
dev_langs:
- TSQL
helpviewer_keywords:
- sys.fn_get_audit_file function
- fn_get_audit_file function
ms.assetid: d6a78d14-bb1f-4987-b7b6-579ddd4167f5
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current||=azure-sqldw-latest
ms.openlocfilehash: 1210ec1da44d68aaf778145da8a02bf3f3092e2c
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093839"
---
# <a name="sysfn_get_audit_file-transact-sql"></a>sys.fn_get_audit_file (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]    

  Retourne des informations à partir d'un fichier d'audit créé par un audit du serveur dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d’informations, consultez [SQL Server Audit &#40;moteur de base de données&#41;](../../relational-databases/security/auditing/sql-server-audit-database-engine.md).  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
fn_get_audit_file ( file_pattern,   
    { default | initial_file_name | NULL },   
    { default | audit_record_offset | NULL } )  
```  
  
## <a name="arguments"></a>Arguments  
 *file_pattern*  
 Spécifie le répertoire ou chemin d'accès et nom de fichier du jeu de fichiers d'audit à lire. Type **nvarchar (260)**. 
 
 - **SQL Server** :
    
    Cet argument doit inclure à la fois un chemin d'accès (lettre de lecteur ou partage réseau) et un nom de fichier qui peut inclure un caractère générique. Un seul astérisque (*) peut être utilisé pour collecter plusieurs fichiers à partir d’un jeu de fichiers d’audit. Par exemple :  
  
    -   **\<path>\\\** _-Collecter tous les fichiers d’audit à l’emplacement spécifié.  
  
    -   _* \<path> \LoginsAudit_{Guid} * * _-collecter tous les fichiers d’audit qui ont le nom et la paire de GUID spécifiés.  
  
    -   _* \<path> \LoginsAudit_{GUID} _00_29384. sqlaudit * *-collecter un fichier d’audit spécifique.  
  
 - **Azure SQL Database**:
 
    Cet argument est utilisé pour spécifier une URL d’objet BLOB (y compris le point de terminaison de stockage et le conteneur). Bien qu’il ne prenne pas en charge un caractère générique astérisque, vous pouvez utiliser un préfixe de nom de fichier partiel (au lieu du nom complet de l’objet BLOB) pour collecter plusieurs fichiers (objets BLOB) qui commencent par ce préfixe. Par exemple :
 
      - **\<Storage_endpoint\>/\<Container\>/\<ServerName\>/\<DatabaseName\>/** -collecte tous les fichiers d’audit (objets BLOB) pour la base de données spécifique.    
      
      - **\<Storage_endpoint\> / \<Container\> / \<ServerName\> / \<DatabaseName\> / \<AuditName\> / \<CreationDate\> / \<FileName\> . Xel** -collecte un fichier d’audit spécifique (objet BLOB).
  
> [!NOTE]  
>  Le passage d'un chemin d'accès sans modèle de nom de fichier génère une erreur.  
  
 *initial_file_name*  
 Spécifie le chemin d'accès et le nom d'un fichier spécifique dans le jeu de fichiers d'audit à partir duquel commencer à lire des enregistrements d'audit. Type **nvarchar (260)**.  
  
> [!NOTE]  
>  L’argument *initial_file_name* doit contenir des entrées valides ou doit contenir la valeur par défaut | Valeur NULL.  
  
 *audit_record_offset*  
 Spécifie un emplacement connu avec le fichier spécifié pour l'initial_file_name. Lorsque cet argument est utilisé, la fonction commence à lire au premier enregistrement de la mémoire tampon qui suit le décalage spécifié.  
  
> [!NOTE]  
>  L’argument *audit_record_offset* doit contenir des entrées valides ou doit contenir la valeur par défaut | Valeur NULL. Le type est **bigint**.  
  
## <a name="tables-returned"></a>Tables retournées  
 Le tableau suivant décrit le contenu de fichier d'audit qui peut être retourné par cette fonction.  
  
| Nom de la colonne | Type | Description |  
|-------------|------|-------------|  
| action_id | **varchar(4)** | ID de l'action. N'accepte pas la valeur NULL. |  
| additional_information | **nvarchar(4000)** | Les informations uniques qui s'appliquent seulement à un événement unique sont retournées au format XML. Un petit nombre d'actions pouvant être auditées contient ce type d'informations.<br /><br /> Un niveau de pile TSQL est affiché au format XML pour les actions auxquelles la pile TSQL est associée. Le format XML est le suivant :<br /><br /> `<tsql_stack><frame nest_level = '%u' database_name = '%.*s' schema_name = '%.*s' object_name = '%.*s' /></tsql_stack>`<br /><br /> Niveau_imbrication_cadre indique le niveau d'imbrication actuel du cadre. Le nom du module est représenté dans un format en trois parties (nom_base_de_données, nom_schéma et nom_objet).  Le nom du module sera analysé pour échapper les caractères XML non valides, tels que `'\<'` , `'>'` , `'/'` , `'_x'` . Elles seront placées dans une séquence d’échappement sous la forme `_xHHHH\_` . HHHH représente le code UCS-2 hexadécimal à quatre chiffres du caractère.<br /><br /> Autorise la valeur NULL. Retourne NULL lorsqu'il n'y a pas d'informations supplémentaires signalées par l'événement. |
| affected_rows | **bigint** | **S’applique à**: Azure SQL Database uniquement<br /><br /> Nombre de lignes affectées par l’instruction exécutée. |  
| application_name | **nvarchar(128)** | **S’applique à**: Azure SQL Database + SQL Server (à partir de 2017)<br /><br /> Nom de l’application cliente qui a exécuté l’instruction qui a provoqué l’événement d’audit |  
| audit_file_offset | **bigint** | **S’applique à**: SQL Server uniquement<br /><br /> Offset de la mémoire tampon dans le fichier qui contient l'enregistrement d'audit. N'accepte pas la valeur NULL. |  
| audit_schema_version | **int** | Toujours 1 |  
| class_type | **varchar(2)** | Type d'entité pouvant être auditée sur laquelle l'audit se produit. N'accepte pas la valeur NULL. |  
| client_ip | **nvarchar(128)** | **S’applique à**: Azure SQL Database + SQL Server (à partir de 2017)<br /><br />  Adresse IP source de l’application cliente |  
| connection_id | GUID | **S’applique à**: Azure SQL Database et SQL Managed instance<br /><br /> ID de la connexion dans le serveur |
| data_sensitivity_information | nvarchar(4000) | **S’applique à**: Azure SQL Database uniquement<br /><br /> Types des informations et étiquettes de sensibilité renvoyées par la requête auditée, en fonction des colonnes classifiées dans la base de données. En savoir plus sur [la découverte et la classification des données Azure SQL Database](/azure/sql-database/sql-database-data-discovery-and-classification) |
| database_name | **sysname** | Contexte de base de données dans lequel l'action s'est produite. Autorise la valeur NULL. Retourne NULL pour les audits qui se produisent au niveau du serveur. |  
| database_principal_id | **int** |ID du contexte de l'utilisateur de base de données dans lequel l'action est effectuée. N'accepte pas la valeur NULL. Retourne 0 si cela ne s'applique pas. Par exemple, une opération de serveur.|
| database_principal_name | **sysname** | Utilisateur actuel. Autorise la valeur NULL. Retourne NULL si non disponible. |  
| duration_milliseconds | **bigint** | **S’applique à**: Azure SQL Database et SQL Managed instance<br /><br /> Durée d’exécution de la requête en millisecondes |
| event_time | **datetime2** | Date et heure auxquelles l'action pouvant être auditée est déclenchée. N'accepte pas la valeur NULL. |  
| file_name | **varchar(260)** | Chemin d'accès et nom du fichier journal d'audit d'où provenait l'enregistrement. N'accepte pas la valeur NULL. |
| is_column_permission | **bit** | Indicateur précisant s’il s’agit d’une autorisation au niveau de la colonne. N'accepte pas la valeur NULL. Retourne 0 lorsque le permission_bitmask = 0.<br /> 1 = vrai<br /> 0 = faux |
| object_id | **int** | ID de l’entité sur laquelle l’audit s’est produit. Ce dernier est détaillé ci-après :<br /> Objets de serveur<br /> Bases de données<br /> Objets de base de données<br /> Objets de schéma<br /> N'accepte pas la valeur NULL. Retourne 0 si l'entité est le serveur lui-même ou si l'audit n'est pas effectué à un niveau objet. Par exemple, Authentification. |  
| object_name | **sysname** | Nom de l’entité sur laquelle l’audit s’est produit. Ce dernier est détaillé ci-après :<br /> Objets de serveur<br /> Bases de données<br /> Objets de base de données<br /> Objets de schéma<br /> Autorise la valeur NULL. Retourne NULL si l'entité est le serveur lui-même ou si l'audit n'est pas effectué à un niveau objet. Par exemple, Authentification. |
| permission_bitmask | **varbinary(16)** | Dans certaines actions, il s'agit des autorisations qui ont été accordées, refusées ou révoquées. |
| response_rows | **bigint** | **S’applique à**: Azure SQL Database et SQL Managed instance<br /><br /> Nombre de lignes retournées dans le jeu de résultats. |  
| schema_name | **sysname** | Contexte du schéma dans lequel l’action s’est produite. Autorise la valeur NULL. Retourne NULL pour les audits qui se produisent en dehors d’un schéma. |  
| sequence_group_id | **varbinary** | **S’applique à**: SQL Server uniquement (à partir de 2016)<br /><br />  Identificateur unique |  
| sequence_number | **int** | Assure le suivi de la séquence d'enregistrements dans un enregistrement d'audit unique qui était trop grand pour la mémoire tampon d'écriture pour audits. N'accepte pas la valeur NULL. |  
| server_instance_name | **sysname** | Nom de l'instance de serveur où l'audit s'est produit. Le format de server\instance standard est utilisé. |  
| server_principal_id | **int** | ID du contexte de connexion dans lequel l'action est effectuée. N'accepte pas la valeur NULL. |  
| server_principal_name | **sysname** | Connexion actuelle. Autorise la valeur NULL. |  
| server_principal_sid | **varbinary** | SID de la connexion actuelle. Autorise la valeur NULL. |  
| session_id | **smallint** | ID de la session au cours de laquelle l'événement s'est produit. N'accepte pas la valeur NULL. |  
| session_server_principal_name | **sysname** | Principal de serveur pour la session. Autorise la valeur NULL. |  
| statement | **nvarchar(4000)** | Instruction TSQL si elle existe. Autorise la valeur NULL. Retourne NULL si non applicable. |  
| réussi | **bit** | Indique si l’action qui a déclenché l’événement a réussi. N'accepte pas la valeur NULL. Pour tous les événements autres que les événements de connexion, cet argument signale uniquement le succès ou l'échec de la vérification des autorisations, mais n'indique rien sur l'opération.<br /> 1 = succès<br /> 0 = échec |
| target_database_principal_id | **int** | Principal de base de données sur lequel l’opération GRANT/DENY/REVOKE est effectuée. N'accepte pas la valeur NULL. Retourne 0 si non applicable. |  
| target_database_principal_name | **sysname** | Utilisateur cible de l’action. Autorise la valeur NULL. Retourne NULL si non applicable. |  
| target_server_principal_id | **int** | Principal de serveur sur lequel l’opération GRANT/DENY/REVOKE est effectuée. N'accepte pas la valeur NULL. Retourne 0 si non applicable. |  
| target_server_principal_name | **sysname** | Connexion cible de l’action. Autorise la valeur NULL. Retourne NULL si non applicable. |  
| target_server_principal_sid | **varbinary** | SID de la connexion cible. Autorise la valeur NULL. Retourne NULL si non applicable. |  
| transaction_id | **bigint** | **S’applique à**: SQL Server uniquement (à partir de 2016)<br /><br /> Identificateur unique permettant d’identifier plusieurs événements d’audit dans une seule transaction |  
| user_defined_event_id | **smallint** | **S’applique à**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures, Azure SQL Database et SQL Managed instance<br /><br /> L’ID d’événement défini par l’utilisateur est passé comme argument à **sp_audit_write**. **Null** pour les événements système (par défaut) et différent de zéro pour l’événement défini par l’utilisateur. Pour plus d’informations, consultez [sp_audit_write &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-audit-write-transact-sql.md). |  
| user_defined_information | **nvarchar(4000)** | **S’applique à**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures, Azure SQL Database et SQL Managed instance<br /><br /> Utilisé pour enregistrer les informations supplémentaires que l’utilisateur souhaite enregistrer dans le journal d’audit à l’aide de la procédure stockée **sp_audit_write** . |  

  
## <a name="remarks"></a>Notes  
 Si l’argument *file_pattern* passé à **fn_get_audit_file** fait référence à un chemin d’accès ou à un fichier qui n’existe pas, ou si le fichier n’est pas un fichier d’audit, le message d’erreur **MSG_INVALID_AUDIT_FILE** est retourné.  
  
## <a name="permissions"></a>Autorisations

- **SQL Server**: requiert l’autorisation **Control Server** .  
- **Azure SQL Database**: requiert l’autorisation **Control Database** .     
  - Les administrateurs de serveur peuvent accéder aux journaux d’audit de toutes les bases de données sur le serveur.
  - Les administrateurs non serveur ne peuvent accéder qu’aux journaux d’audit de la base de données active.
  - Les objets BLOB qui ne répondent pas aux critères ci-dessus seront ignorés (une liste d’objets BLOB ignorés s’affichera dans le message de sortie de la requête) et la fonction renverra uniquement les journaux des objets BLOB pour lesquels l’accès est autorisé.  
  
## <a name="examples"></a>Exemples

- **SQL Server**

  Cet exemple lit à partir d'un fichier nommé `\\serverName\Audit\HIPAA_AUDIT.sqlaudit`.  
  
  ```  
  SELECT * FROM sys.fn_get_audit_file ('\\serverName\Audit\HIPAA_AUDIT.sqlaudit',default,default);  
  GO  
  ```  

- **Azure SQL Database**

  Cet exemple lit à partir d’un fichier nommé `ShiraServer/MayaDB/SqlDbAuditing_Audit/2017-07-14/10_45_22_173_1.xel` :  
  
  ```  
  SELECT * FROM sys.fn_get_audit_file ('https://mystorage.blob.core.windows.net/sqldbauditlogs/ShiraServer/MayaDB/SqlDbAuditing_Audit/2017-07-14/10_45_22_173_1.xel',default,default);
  GO  
  ```  

  Cet exemple lit à partir du même fichier que ci-dessus, mais avec des clauses T-SQL supplémentaires (clause **Top**, **order by** et **Where** pour filtrer les enregistrements d’audit retournés par la fonction) :
  
  ```  
  SELECT TOP 10 * FROM sys.fn_get_audit_file ('https://mystorage.blob.core.windows.net/sqldbauditlogs/ShiraServer/MayaDB/SqlDbAuditing_Audit/2017-07-14/10_45_22_173_1.xel',default,default)
  WHERE server_principal_name = 'admin1'
  ORDER BY event_time
  GO
  ```  

  Cet exemple lit tous les journaux d’audit des serveurs qui commencent par `Sh` : 
  
  ```  
  SELECT * FROM sys.fn_get_audit_file ('https://mystorage.blob.core.windows.net/sqldbauditlogs/Sh',default,default);
  GO  
  ```

Pour obtenir un exemple complet de création d’audit, consultez [SQL Server Audit &#40;moteur de base de données&#41;](../../relational-databases/security/auditing/sql-server-audit-database-engine.md).

Pour plus d’informations sur la configuration de l’audit de Azure SQL Database, consultez [prise en main de l’audit de SQL Database](/azure/sql-database/sql-database-auditing).
  
## <a name="see-also"></a>Voir aussi  
 [CREATE SERVER AUDIT &#40;Transact-SQL&#41;](../../t-sql/statements/create-server-audit-transact-sql.md)   
 [ALTER SERVER AUDIT  &#40;Transact-SQL&#41;](../../t-sql/statements/alter-server-audit-transact-sql.md)   
 [DROP SERVER AUDIT  &#40;Transact-SQL&#41;](../../t-sql/statements/drop-server-audit-transact-sql.md)   
 [CREATE SERVER AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../t-sql/statements/create-server-audit-specification-transact-sql.md)   
 [ALTER SERVER AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-server-audit-specification-transact-sql.md)   
 [DROP SERVER AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../t-sql/statements/drop-server-audit-specification-transact-sql.md)   
 [CREATE DATABASE AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../t-sql/statements/create-database-audit-specification-transact-sql.md)   
 [ALTER DATABASE AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-audit-specification-transact-sql.md)   
 [DROP DATABASE AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../t-sql/statements/drop-database-audit-specification-transact-sql.md)   
 [ALTER AUTHORIZATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-authorization-transact-sql.md)   
 [sys.server_audits &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-audits-transact-sql.md)   
 [sys.server_file_audits &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-file-audits-transact-sql.md)   
 [sys.server_audit_specifications &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-audit-specifications-transact-sql.md)   
 [sys.server_audit_specification_details &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-audit-specification-details-transact-sql.md)   
 [sys.database_audit_specifications &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-audit-specifications-transact-sql.md)   
 [sys.database_audit_specification_details &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-audit-specification-details-transact-sql.md)   
 [sys.dm_server_audit_status &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-server-audit-status-transact-sql.md)   
 [sys.dm_audit_actions &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-audit-actions-transact-sql.md)   
 [sys.dm_audit_class_type_map &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-audit-class-type-map-transact-sql.md)   
 [Créer un audit du serveur et une spécification d’audit du serveur](../../relational-databases/security/auditing/create-a-server-audit-and-server-audit-specification.md)  
  
