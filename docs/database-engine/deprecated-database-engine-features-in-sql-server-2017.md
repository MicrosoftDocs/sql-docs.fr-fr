---
title: Fonctionnalités du moteur de base de données dépréciées dans SQL Server 2017 | Microsoft Docs
titleSuffix: SQL Server 2019
description: Découvrez les fonctionnalités du moteur de base de données dépréciées qui sont toujours disponibles dans SQL Server 2017 (14.x), mais qui ne doivent pas être utilisées dans les nouvelles applications.
ms.custom: seo-lt-2019
ms.date: 12/13/2019
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: release-landing
ms.topic: conceptual
helpviewer_keywords:
- deprecated features [SQL Server]
- Database Engine [SQL Server], backward compatibility
- deprecation [SQL Server], feature list
ms.assetid: ''
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=sql-server-2017||>=sql-server-linux-2017'
ms.openlocfilehash: 5d9f07f96f83767a84f7ee0128d674f4ab955eed
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170851"
---
# <a name="deprecated-database-engine-features-in-sql-server-2017"></a>Fonctionnalités du moteur de base de données dépréciées dans SQL Server 2017

[!INCLUDE[SQL Server 2017](../includes/applies-to-version/sqlserver2017.md)]

  Cette rubrique décrit les fonctionnalités [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] déconseillées qui sont toujours disponibles dans [!INCLUDE[sssqlv14-md](../includes/sssqlv14-md.md)]. Les fonctions déconseillées ne doivent pas être utilisées dans de nouvelles applications.  
  
Quand une fonctionnalité est marquée comme étant dépréciée, cela signifie que :

- La fonctionnalité est en mode de maintenance uniquement. Aucune nouvelle modification ne lui sera apportée, notamment celles liées à l’interopérabilité avec de nouvelles fonctionnalités.
- Nous nous efforçons de ne pas retirer une fonctionnalité dépréciée des futures versions pour faciliter les mises à niveau. Cependant, dans de rares cas, nous pouvons décider de retirer définitivement une fonctionnalité de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] si elle limite de futures innovations.
- Pour un nouveau travail de développement, nous vous recommandons de ne pas utiliser des fonctionnalités dépréciées.      

Vous pouvez surveiller l'utilisation de fonctionnalités déconseillées à l'aide du compteur de performance Objet [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Fonctionnalités déconseillées et des événements de suivi. Pour plus d’informations, consultez [Utiliser des objets SQL Server](../relational-databases/performance-monitor/use-sql-server-objects.md).  

Vous pouvez également obtenir les valeurs de ces compteurs en exécutant l’instruction suivante :  

```sql
SELECT * FROM sys.dm_os_performance_counters
WHERE object_name = 'SQLServer:Deprecated Features';
```

> [!NOTE]
> Cette liste est identique à la liste [!INCLUDE[sssql15-md](../includes/sssql16-md.md)]. Aucune nouvelle fonctionnalité du moteur de base de données supprimée ou dépréciée n’a été annoncée pour [!INCLUDE[sssqlv14-md](../includes/sssqlv14-md.md)].

## <a name="features-deprecated-in-the-next-version-of-sql-server"></a>Fonctionnalités dépréciées dans la prochaine version de SQL Server

Les fonctionnalités du moteur de base de données SQL Server décrites ci-dessous sont dépréciées dans la prochaine version de SQL Server. Évitez d'utiliser ces fonctionnalités dans vos nouveaux développements et modifiez dès que possible les applications qui y ont recours. La valeur **Nom de la fonctionnalité** apparaît dans les événements de trace comme ObjectName et dans les compteurs de performance et `sys.dm_os_performance_counters` comme nom d’instance. La valeur de l’ **ID de la fonctionnalité** apparaît dans les événements de suivi comme ObjectId.

### <a name="back-up-and-restore"></a>Sauvegarde et restauration

| Fonctionnalité déconseillée | Remplacement | Nom de la fonctionnalité | ID de la fonctionnalité |
|--------------------|-------------|--------------|------------|
| RESTORE { DATABASE &#124; LOG } WITH [MEDIA]PASSWORD est toujours déconseillé.<br /><br />BACKUP { DATABASE &#124; LOG } WITH PASSWORD et BACKUP { DATABASE &#124; LOG } WITH MEDIAPASSWORD ont été retirés. | Aucun. | BACKUP DATABASE ou LOG WITH PASSWORD<br /><br />BACKUP DATABASE ou LOG WITH MEDIAPASSWORD | 104<br /><br /> 103 |

### <a name="compatibility-levels"></a>Niveaux de compatibilité

| Fonctionnalité déconseillée | Remplacement | Nom de la fonctionnalité | ID de la fonctionnalité |
|--------------------|-------------|--------------|------------|
Mise à niveau à partir de la version 100 (SQL Server 2008 et SQL Server 2008 R2). | Quand le [support](/lifecycle/products/?products=sql-server) d’une version de SQL Server prend fin, le niveau de compatibilité de base de données associé est marqué comme étant déprécié. Cependant, nous continuons le plus longtemps possible d’assurer le support des applications certifiées sur tous les niveaux de compatibilité de base de données pris en charge de façon à faciliter la mise à niveau. Pour plus d’informations sur les niveaux de compatibilité, consultez [Niveau de compatibilité ALTER DATABASE &#40;Transact-SQL&#41;](../t-sql/statements/alter-database-transact-sql-compatibility-level.md). | Niveau de compatibilité 100 de la base de données | 108 |

### <a name="database-objects"></a>Objets de base de données

| Fonctionnalité déconseillée | Remplacement | Nom de la fonctionnalité | ID de la fonctionnalité |
|--------------------|-------------|--------------|------------|
| Possibilité de retourner les jeux de résultats à partir de déclencheurs | None | Le déclencheur retourne des résultats | 12 |

### <a name="encryption"></a>Chiffrement

| Fonctionnalité déconseillée | Remplacement | Nom de la fonctionnalité | ID de la fonctionnalité |
|--------------------|-------------|--------------|------------|
| Le chiffrement à l'aide de RC4 ou RC4_128 est déconseillé et est planifié pour être supprimé dans la prochaine version. Les déchiffrements de RC4 et RC4_128 ne sont pas dépréciés. | Utilisez un autre algorithme de chiffrement, par exemple AES. | Algorithme de chiffrement déconseillé | 253 |
| L’utilisation de MD2, MD4, MD5, SHA et SHA-1 est dépréciée. | Utilisez SHA2_256 ou SHA2_512 à la place. Des algorithmes plus anciens continuent de fonctionner, mais ils déclenchent un événement de dépréciation. |Algorithme de hachage déconseillé | None |

### <a name="remote-servers"></a>Serveurs distants

| Fonctionnalité déconseillée | Remplacement | Nom de la fonctionnalité | ID de la fonctionnalité |
|--------------------|-------------|--------------|------------|
| sp_addremotelogin<br /><br />sp_addserver <br /><br /> sp_dropremotelogin <br /><br /> sp_helpremotelogin <br /><br /> sp_remoteoption|Remplacez les serveurs distants à l'aide de serveurs liés. sp_addserver ne peut être utilisé qu’avec l’option « local ». | sp_addremotelogin<br /><br />sp_addserver <br /><br /> sp_dropremotelogin <br /><br /> sp_helpremotelogin <br /><br > sp_remoteoption | 70 <br /><br /> 69 <br /><br /> 71 <br /><br /> 72 <br /><br /> 73 |
| \@\@remserver | Remplacez les serveurs distants à l'aide de serveurs liés. | None | None |
| SET REMOTE_PROC_TRANSACTIONS|Remplacez les serveurs distants à l'aide de serveurs liés. | SET REMOTE_PROC_TRANSACTIONS | 110 |

### <a name="transact-sql"></a>Transact-SQL

| Fonctionnalité déconseillée | Remplacement | Nom de la fonctionnalité | ID de la fonctionnalité |
|--------------------|-------------|--------------|------------|
| **SET ROWCOUNT** pour les instructions **INSERT**, **UPDATE** et **DELETE** | Mot clé TOP | SET ROWCOUNT | 109 |
| Indicateur de table HOLDLOCK sans parenthèses. | Utilisez HOLDLOCK avec la parenthèse. | Indicateur de table HOLDLOCK sans parenthèses | 167 |

## <a name="features-deprecated-in-a-future-version-of-sql-server"></a>Fonctionnalités dépréciées dans une future version de SQL Server

Les fonctionnalités du moteur de base de données SQL Server décrites ci-dessous sont prises en charge dans la prochaine version de SQL Server. La version spécifique de SQL Server n’a pas été déterminée.

### <a name="back-up-and-restore"></a>Sauvegarde et restauration

| Fonctionnalité déconseillée | Remplacement | Nom de la fonctionnalité |
|--------------------|-------------|--------------|
| BACKUP { DATABASE &#124; LOG } TO TAPE <br /><br /> BACKUP { DATABASE &#124; LOG } TO *device_that_is_a_tape*|BACKUP { DATABASE &#124; LOG } TO DISK <br /><br /> BACKUP { DATABASE &#124; LOG } TO *device_that_is_a_disk* | BACKUP DATABASE ou LOG TO TAPE |
| sp_addumpdevice '**tape**' | sp_addumpdevice '**disk**' | ADDING TAPE DEVICE |
| sp_helpdevice | sys.backup_devices | sp_helpdevice |

### <a name="compatibility-levels"></a>Niveaux de compatibilité

| Fonctionnalité déconseillée | Remplacement | Nom de la fonctionnalité |
|--------------------|-------------|--------------|
| sp_dbcmptlevel|ALTER DATABASE ... SET COMPATIBILITY_LEVEL. Pour plus d’informations, consultez [Niveau de compatibilité ALTER DATABASE &#40;Transact-SQL&#41;](../t-sql/statements/alter-database-transact-sql-compatibility-level.md). | sp_dbcmptlevel |
| Niveau de compatibilité 110 et 120 de la base de données | Projetez de mettre à niveau la base de données et l'application avant la prochaine version. Cependant, nous continuons le plus longtemps possible d’assurer le support des applications certifiées sur tous les niveaux de compatibilité de base de données pris en charge de façon à faciliter la mise à niveau. Pour plus d’informations sur les niveaux de compatibilité, consultez [Niveau de compatibilité ALTER DATABASE &#40;Transact-SQL&#41;](../t-sql/statements/alter-database-transact-sql-compatibility-level.md). | Niveau de compatibilité 110 de la base de données <br /><br /> Niveau de compatibilité 120 de la base de données |

### <a name="collations"></a>Classements

| Fonctionnalité déconseillée | Remplacement | Nom de la fonctionnalité |
|--------------------|-------------|--------------|
| Korean_Wansung_Unicode <br /><br /> Lithuanian_Classic <br /><br /> SQL_AltDiction_CP1253_CS_AS | Aucun. Ces classements existent dans SQL Server 2005 (9.x), mais ils ne sont pas visibles via fn_helpcollations. | Korean_Wansung_Unicode <br /><br /> Lithuanian_Classic <br /><br /> SQL_AltDiction_CP1253_CS_AS |
| Hindi <br /><br /> Macédonien | Ces classements existent dans SQL Server 2005 (9.x) et les versions ultérieures, mais ils ne sont pas visibles via fn_helpcollations. Utilisez à la place Macedonian_FYROM_90 et Indic_General_90.|Hindi <br /><br /> Macédonien |
| Azeri_Latin_90 <br /><br /> Azeri_Cyrilllic_90 | Azeri_Latin_100 <br /><br /> Azeri_Cyrilllic_100 | Azeri_Latin_90 <br /><br /> Azeri_Cyrilllic_90 |

### <a name="data-types"></a>Types de données

| Fonctionnalité déconseillée | Remplacement | Nom de la fonctionnalité |
|--------------------|-------------|--------------|
| sp_addtype <br /><br /> sp_droptype|CREATE TYPE<br /><br /> DROP TYPE | sp_addtype<br /><br /> sp_droptype |
| Syntaxe de **timestamp** pour le type de données **rowversion** | Syntaxe du type de données **rowversion** | timestamp |
| Possibilité d'insérer des valeurs NULL dans les colonnes **timestamp** . | Utilisez DEFAULT à la place. | INSERT NULL dans des colonnes TIMESTAMP |
| Option de table 'text in row'|Utilisez les types de données **varchar(max)** , **nvarchar(max)** et **varbinary(max)** . Pour plus d’informations, consultez [sp_tableoption &#40;Transact-SQL&#41;](../relational-databases/system-stored-procedures/sp-tableoption-transact-sql.md).|Option de table text in row |
| Types de données :<br /><br /> **text**<br /><br /> **ntext**<br /><br /> **image**|Utilisez les types de données **varchar(max)** , **nvarchar(max)** et **varbinary(max)** .|Types de données : **text**, **ntext** ou **image** |

### <a name="database-management"></a>Gestion de bases de données

| Fonctionnalité déconseillée | Remplacement | Nom de la fonctionnalité |
|--------------------|-------------|--------------|
| sp_attach_db <br /><br /> sp_attach_single_file_db|Instruction CREATE DATABASE avec l'option FOR ATTACH. Pour recréer plusieurs fichiers journaux, lorsqu'un ou plusieurs d'entre eux possèdent un nouvel emplacement, utilisez FOR ATTACH_REBUILD_LOG. | sp_attach_db <br /><br /> sp_attach_single_file_db |
| sp_certify_removable<br /><br /> sp_create_removable|sp_detach_db|sp_certify_removable<br /><br /> sp_create_removable |
| sp_dbremove | DROP DATABASE | sp_dbremove |
| sp_renamedb | MODIFY NAME dans ALTER DATABASE | sp_renamedb |

### <a name="database-objects"></a>Objets de base de données

| Fonctionnalité déconseillée | Remplacement | Nom de la fonctionnalité |
|--------------------|-------------|--------------|
| CREATE DEFAULT<br /><br /> DROP DEFAULT<br /><br /> sp_bindefault<br /><br /> sp_unbindefault|Mot clé DEFAULT dans CREATE TABLE et ALTER TABLE|CREATE_DROP_DEFAULT<br /><br /> sp_bindefault<br /><br /> sp_unbindefault |
| CREATE RULE<br /><br /> DROP RULE<br /><br /> sp_bindrule<br /><br /> sp_unbindrule | Mot clé CHECK dans CREATE TABLE et ALTER TABLE | CREATE_DROP_RULE<br /><br /> sp_bindrule<br /><br /> sp_unbindrule |
| sp_change_users_login | Utilisez ALTER USER. | sp_change_users_login |
| sp_depends | sys.dm_sql_referencing_entities et sys.dm_sql_referenced_entities | sp_depends |
| sp_getbindtoken | Utilisez MARS ou les transactions distribuées. | sp_getbindtoken |

### <a name="database-options"></a>Options de la base de données

| Fonctionnalité déconseillée | Remplacement | Nom de la fonctionnalité |
|--------------------|-------------|--------------|
| sp_bindsession | Utilisez MARS ou les transactions distribuées. | sp_bindsession |
| sp_resetstatus | ALTER DATABASE SET { ONLINE &#124; EMERGENCY } | sp_resetstatus
| Option TORN_PAGE_DETECTION d'ALTER DATABASE|Option PAGE_VERIFY TORN_PAGE DETECTION d'ALTER DATABASE | ALTER DATABASE WITH TORN_PAGE_DETECTION |

### <a name="dbcc"></a>DBCC

| Fonctionnalité déconseillée | Remplacement | Nom de la fonctionnalité |
|--------------------|-------------|--------------|
| DBCC DBREINDEX|Option REBUILD d'ALTER INDEX. | DBCC DBREINDEX |
| DBCC INDEXDEFRAG|Option REORGANIZE d'ALTER INDEX | DBCC INDEXDEFRAG |
| DBCC SHOWCONTIG|sys.dm_db_index_physical_stats | DBCC SHOWCONTIG |
| DBCC PINTABLE<br /><br /> DBCC UNPINTABLE | Cette option n'a aucun effet. | DBCC [UN] PINTABLE |

### <a name="extended-properties"></a>Propriétés étendues

| Fonctionnalité déconseillée | Remplacement | Nom de la fonctionnalité |
|--------------------|-------------|--------------|
| Level0type = 'type' et Level0type = 'USER' pour ajouter des propriétés étendues aux objets de type de niveau 1 ou 2. | Utilisez Level0type = 'USER' uniquement pour ajouter une propriété étendue directement à un utilisateur ou un rôle.<br /><br /> Utilisez Level0type = 'SCHEMA' pour ajouter une propriété étendue aux types level-1 comme TABLE ou VIEW ou aux types level-2 comme COLUMN ou TRIGGER. Pour plus d’informations, consultez [sp_addextendedproperty &#40;Transact-SQL&#41;](../relational-databases/system-stored-procedures/sp-addextendedproperty-transact-sql.md). | EXTPROP_LEVEL0TYPE<br /><br /> EXTPROP_LEVEL0USER |

### <a name="extended-stored-procedures"></a>Procédures stockées étendues

| Fonctionnalité déconseillée | Remplacement | Nom de la fonctionnalité |
|--------------------|-------------|--------------|
| xp_grantlogin<br /><br /> xp_revokelogin<br /><br /> xp_loginConfig|Utiliser CREATE_LOGIN<br /><br /> Utiliser l'argument DROP LOGIN IsIntegratedSecurityOnly de SERVERPROPERTY|xp_grantlogin<br /><br /> xp_revokelogin<br /><br /> xp_loginConfig |

### <a name="extended-stored-procedures-programming"></a>Programmation de procédures stockées étendues

| Fonctionnalité déconseillée | Remplacement | Nom de la fonctionnalité |
|--------------------|-------------|--------------|
| srv_alloc<br /><br /> srv_convert<br /><br /> srv_describe<br /><br /> srv_getbindtoken<br /><br /> srv_got_attention<br /><br /> srv_message_handler<br /><br /> srv_paramdata<br /><br /> srv_paraminfo<br /><br /> srv_paramlen<br /><br /> srv_parammaxlen<br /><br /> srv_paramname<br /><br /> srv_paramnumber<br /><br /> srv_paramset<br /><br /> srv_paramsetoutput<br /><br /> srv_paramstatus<br /><br /> srv_paramtype<br /><br /> srv_pfield<br /><br /> srv_pfieldex<br /><br /> srv_rpcdb<br /><br /> srv_rpcname<br /><br /> srv_rpcnumber<br /><br /> srv_rpcoptions<br /><br /> srv_rpcowner<br /><br /> srv_rpcparams<br /><br /> srv_senddone<br /><br /> srv_sendmsg<br /><br /> srv_sendrow<br /><br /> srv_setcoldata<br /><br /> srv_setcollen<br /><br /> srv_setutype<br /><br /> srv_willconvert<br /><br /> srv_wsendmsg | Utilisez l'intégration CLR à la place. | XP_API |
| sp_addextendedproc<br /><br /> sp_dropextendedproc<br /><br /> sp_helpextendedproc | Utilisez l'intégration CLR à la place. | sp_addextendedproc<br /><br /> sp_dropextendedproc<br /><br /> sp_helpextendedproc |
| xp_grantlogin<br /><br /> xp_revokelogin<br /><br /> xp_loginConfig|Utiliser CREATE_LOGIN<br /><br /> Utiliser l'argument DROP LOGIN IsIntegratedSecurityOnly de SERVERPROPERTY | xp_grantlogin<br /><br /> xp_revokelogin<br /><br /> xp_loginConfig |

### <a name="high-availability"></a>Haute disponibilité

| Fonctionnalité déconseillée | Remplacement | Nom de la fonctionnalité |
|--------------------|-------------|--------------|
| mise en miroir de bases de données | Groupes de disponibilité Always On<br /><br /> Si votre édition de SQL Server ne prend pas en charge les groupes de disponibilité Always On, utilisez la copie des journaux de session. | DATABASE_MIRRORING |

### <a name="index-options"></a>Options d'index

| Fonctionnalité déconseillée | Remplacement | Nom de la fonctionnalité |
|--------------------|-------------|--------------|
| sp_indexoption | ALTER INDEX | sp_indexoption |
| Syntaxe CREATE TABLE, ALTER TABLE ou CREATE INDEX sans parenthèses autour des options. | Réécrivez l'instruction de manière à utiliser la syntaxe actuelle. | INDEX_OPTION |

### <a name="instance-options"></a>Options d'instance

| Fonctionnalité déconseillée | Remplacement | Nom de la fonctionnalité |
|--------------------|-------------|--------------|
| option sp_configure 'allow updates'|Les tables système ne peuvent plus être mises à jour. La valeur n'a pas d'effet. | sp_configure allow updates' |
| Options sp_configure : <br /><br /> 'locks' <br /><br /> 'open objects'<br /><br /> 'set working set size' | Désormais configuré automatiquement. La valeur n'a pas d'effet. | sp_configure 'locks'<br /><br /> sp_configure 'open objects'<br /><br /> sp_configure 'set working set size' |
| Option sp_configure 'priority boost' | Les tables système ne peuvent plus être mises à jour. La valeur n'a pas d'effet. Utilisez à la place l’option start /high … program.exe de Windows. | sp_configure "priority boost" |
| Option sp_configure 'remote proc trans' | Les tables système ne peuvent plus être mises à jour. La valeur n'a pas d'effet. | sp_configure 'remote proc trans' |

### <a name="linked-servers"></a>Serveurs liés

| Fonctionnalité déconseillée | Remplacement | Nom de la fonctionnalité |
|--------------------|-------------|--------------|
| Spécification du fournisseur SQLOLEDB pour les serveurs liés. | SQL Server Native Client (SQLNCLI) | SQLOLEDDB pour les serveurs liés |

### <a name="metadata"></a>Métadonnées

| Fonctionnalité déconseillée | Remplacement | Nom de la fonctionnalité |
|--------------------|-------------|--------------|
| FILE_ID<br /><br /> INDEXKEY_PROPERTY | FILE_IDEX<br /><br /> sys.index_columns | FILE_ID<br /><br /> INDEXKEY_PROPERTY |

### <a name="native-xml-web-services"></a>Services Web XML natifs

| Fonctionnalité déconseillée | Remplacement | Nom de la fonctionnalité |
|--------------------|-------------|--------------|
| Instruction CREATE ENDPOINT ou ALTER ENDPOINT avec l'option FOR SOAP.<br /><br /> sys.endpoint_webmethods<br /><br /> sys.soap_endpoints|Utilisez à la place WFC (Windows Communications Foundation) ou ASP.NET. | CREATE/ALTER ENDPOINT<br /><br /> sys.endpoint_webmethods<br /><br /> EXT_soap_endpoints<br /><br /> sys.soap_endpoints |

### <a name="other"></a>Autres

| Fonctionnalité déconseillée | Remplacement | Nom de la fonctionnalité |
|--------------------|-------------|--------------|
| DB-Library<br /><br />Embedded SQL for C|Bien que le moteur de base de données prenne toujours en charge les connexions des applications existantes qui utilisent les API DB-Library et Embedded SQL, il n'inclut pas les fichiers ou la documentation nécessaires aux tâches de programmation dans les applications qui utilisent ces API. Une version future du moteur de base de données SQL Server n’intègre plus la prise en charge des connexions à partir des applications DB-Library ou Embedded SQL. N'utilisez pas DB-Library ni Embedded SQL pour développer de nouvelles applications. Supprimez toutes les dépendances à DB-Library ou à Embedded SQL lorsque vous modifiez les applications existantes. À la place de ces API, utilisez l’espace de noms SQLClient ou une API comme ODBC. SQL Server 2019 (15.x) n’inclut pas la DLL DB-Library nécessaire à l’exécution de ces applications. Pour exécuter des applications DB-Library ou Embedded SQL, vous avez besoin de la DLL DB-Library fournie dans SQL Server version 6.5, SQL Server 7.0 ou SQL Server 2000 (8.x). | None |

### <a name="security"></a>Sécurité

| Fonctionnalité déconseillée | Remplacement | Nom de la fonctionnalité |
|--------------------|-------------|--------------|
| Syntaxe ALTER LOGIN WITH SET CREDENTIAL | Remplacée par la nouvelle syntaxe ALTER LOGIN ADD et DROP CREDENTIAL | ALTER LOGIN WITH SET CREDENTIAL |
| sp_addapprole<br /><br /> sp_dropapprole | CREATE APPLICATION ROLE<br /><br /> DROP APPLICATION ROLE | sp_addapprole<br /><br /> sp_dropapprole |
| sp_addlogin<br /><br /> sp_droplogin | CREATE LOGIN<br /><br /> DROP LOGIN|sp_addlogin<br /><br /> sp_droplogin |
| sp_adduser<br /><br /> sp_dropuser | CREATE USER<br /><br /> DROP USER|sp_adduser<br /><br /> sp_dropuser |
| sp_grantdbaccess<br /><br /> sp_revokedbaccess|CREATE USER<br /><br /> DROP USER|sp_grantdbaccess<br /><br /> sp_revokedbaccess |
| sp_addrole<br /><br /> sp_droprole | CREATE ROLE<br /><br /> DROP ROLE|sp_addrole<br /><br /> sp_droprole |
| sp_approlepassword<br /><br /> sp_password | ALTER APPLICATION ROLE<br /><br /> ALTER LOGIN|sp_approlepassword<br /><br /> sp_password |
| sp_changedbowner|ALTER AUTHORIZATION | sp_changedbowner |
| sp_changeobjectowner|ALTER SCHEMA ou ALTER AUTHORIZATION | sp_changeobjectowner |
| sp_control_dbmasterkey_password | Une clé principale doit exister et le mot de passe doit être correct.|sp_control_dbmasterkey_password |
| sp_defaultdb<br /><br /> sp_defaultlanguage | ALTER LOGIN|sp_defaultdb<br /><br /> sp_defaultlanguage |
| sp_denylogin<br /><br /> sp_grantlogin<br /><br /> sp_revokelogin|ALTER LOGIN DISABLE<br /><br /> CREATE LOGIN<br /><br /> DROP LOGIN|sp_denylogin<br /><br /> sp_grantlogin<br /><br /> sp_revokelogin |
| USER_ID|DATABASE_PRINCIPAL_ID | USER_ID |
| sp_srvrolepermission<br /><br /> sp_dbfixedrolepermission|Ces procédures stockées renvoient des informations qui étaient correctes dans [!INCLUDE[ssVersion2000](../includes/ssversion2000-md.md)]. Le résultat ne reflète pas les modifications apportées à la hiérarchie d’autorisations implémentée dans SQL Server 2008. Pour plus d'informations, consultez [Autorisations des rôles serveur fixes](https://msdn.microsoft.com/library/ms175892\(SQL.100\).aspx). | sp_srvrolepermission<br /><br /> sp_dbfixedrolepermission |
| GRANT ALL<br /><br /> DENY ALL<br /><br /> REVOKE ALL|Autorisations spécifiques GRANT, DENY et REVOKE.|Autorisation ALL |
| Fonction intrinsèque PERMISSIONS | Interrogez à la place sys.fn_my_permissions. | PERMISSIONS |
| SETUSER | EXECUTE AS | SETUSER |
| Algorithmes de chiffrement RC4 et DESX|Utilisez un autre algorithme, par exemple AES. | Algorithme DESX |

### <a name="server-configuration-options"></a>Options de configuration de serveur

| Fonctionnalité déconseillée | Remplacement | Nom de la fonctionnalité |
|--------------------|-------------|--------------|
| Audit C2 (option) Trace par défaut activée (option)<br /><br /> Trace par défaut activée (option) | [common criteria compliance enabled (option de configuration de serveur)](../database-engine/configure-windows/common-criteria-compliance-enabled-server-configuration-option.md)<br /><br /> [Événements étendus](../relational-databases/extended-events/extended-events.md) | sp_configure 'c2 audit mode'<br /><br />sp_configure 'default trace enabled' |

### <a name="smo-classes"></a>Classes SMO

| Fonctionnalité déconseillée | Remplacement | Nom de la fonctionnalité |
|--------------------|-------------|--------------|
| *Microsoft.SQLServer. Management.Smo.Information* (classe)<br /><br />*Microsoft.SQLServer. Management.Smo.Settings* (classe)<br /><br />*Microsoft.SQLServer.Management. Smo.DatabaseOptions* (classe)<br /><br />*Microsoft.SqlServer.Management.Smo. DatabaseDdlTrigger.NotForReplication* (propriété) | *Microsoft.SqlServer.  Management.Smo.Server* (classe)<br /><br />**Microsoft.SqlServer.  Management.Smo.Server* (classe)<br /><br />* Microsoft.SqlServer. Management.Smo.Database* (classe)<br /><br />None | None |

### <a name="sql-server-agent"></a>SQL Server Agent

| Fonctionnalité déconseillée | Remplacement | Nom de la fonctionnalité |
|--------------------|-------------|--------------|
| Notification **net send**<br /><br />Notification par radiomessagerie | Notification par e-mail<br /><br />Notification par e-mail | None |

### <a name="sql-server-management-studio"></a>SQL Server Management Studio

| Fonctionnalité déconseillée | Remplacement | Nom de la fonctionnalité |
|--------------------|-------------|--------------|
| Intégration de l’Explorateur de solutions dans SQL Server Management Studio | | None |

### <a name="system-stored-procedures-and-functions"></a>Fonctions et procédures stockées système

| Fonctionnalité déconseillée | Remplacement | Nom de la fonctionnalité |
|--------------------|-------------|--------------|
| sp_db_increased_partitions | Aucun. La prise en charge des partitions augmentées est disponible dans SQL Server 2019 (15.x). | sp_db_increased_partitions |
| fn_virtualservernodes<br /><br />fn_servershareddrives | sys.dm_os_cluster_nodes<br /><br />sys.dm_io_cluster_shared_drives | fn_virtualservernodes<br /><br /> fn_servershareddrives |
| fn_get_sql | sys.dm_exec_sql_text | fn_get_sql |
| sp_lock | sys.dm_tran_locks | sp_lock |

### <a name="system-tables"></a>Tables système

| Fonctionnalité déconseillée | Remplacement | Nom de la fonctionnalité |
|--------------------|-------------|--------------|
| sysaltfiles<br /><br />syscacheobjects<br /><br />syscolumns<br /><br />syscomments<br /><br />sysconfigures<br /><br />sysconstraints<br /><br />syscurconfigs<br /><br />sysdatabases<br /><br />sysdepends<br /><br />sysdevices<br /><br />sysfilegroups<br /><br />sysfiles<br /><br />sysforeignkeys<br /><br />sysfulltextcatalogs<br /><br />sysindexes<br /><br />sysindexkeys<br /><br />syslockinfo<br /><br />syslogins<br /><br />sysmembers<br /><br />sysmessages<br /><br />sysobjects<br /><br />sysoledbusers<br /><br />sysopentapes<br /><br />sysperfinfo<br /><br />syspermissions<br /><br />sysprocesses<br /><br />sysprotects<br /><br />sysreferences<br /><br />sysremotelogins<br /><br />sysservers<br /><br />systypes<br /><br />sysusers|Vues de compatibilité. Pour plus d’informations, consultez [Vues de compatibilité &#40;Transact-SQL&#41;](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md).<br /><br />**Important :** Les vues de compatibilité n’exposent pas de métadonnées pour les fonctionnalités qui ont été introduites dans SQL Server 2005 (9.x). Il est recommandé de mettre à niveau les applications pour pouvoir utiliser les affichages catalogue. Pour plus d’informations, consultez [Affichages catalogue &#40;Transact-SQL&#41;](../relational-databases/system-catalog-views/catalog-views-transact-sql.md). | sysaltfiles<br /><br />syscacheobjects<br /><br />syscolumns<br /><br />syscomments<br /><br />sysconfigures<br /><br />sysconstraints<br /><br />syscurconfigs<br /><br />sysdatabases<br /><br />sysdepends<br /><br />sysdevices<br /><br />sysfilegroups<br /><br />sysfiles<br /><br />sysforeignkeys<br /><br />sysfulltextcatalogs<br /><br />sysindexes<br /><br />sysindexkeys<br /><br />syslockinfo<br /><br />syslogins<br /><br />sysmembers<br /><br />sysmessages<br /><br />sysobjects<br /><br />sysoledbusers<br /><br />sysopentapes<br /><br />sysperfinfo<br /><br />syspermissions<br /><br />sysprocesses<br /><br />sysprotects<br /><br />sysreferences<br /><br />sysremotelogins<br /><br />sysservers<br /><br />systypes<br /><br />sysusers |
| sys.numbered_procedures<br /><br />sys.numbered_procedure_parameters | None | numbered_procedures<br /><br />numbered_procedure_parameters |

### <a name="sql-trace-stored-procedures-functions-and-catalog-views"></a>Procédures stockées, fonctions et affichages catalogue Trace SQL

| Fonctionnalité déconseillée | Remplacement | Nom de la fonctionnalité |
|--------------------|-------------|--------------|
| sp_trace_create<br /><br />sp_trace_setevent<br /><br />sp_trace_setfilter<br /><br />sp_trace_setstatus<br /><br />fn_trace_geteventinfo<br /><br />fn_trace_getfilterinfo<br /><br />fn_trace_getinfo<br /><br />fn_trace_gettable<br /><br />sys.traces<br /><br />sys.trace_events<br /><br />sys.trace_event_bindings<br /><br />sys.trace_categories<br /><br />sys.trace_columns<br /><br />sys.trace_subclass_values|[Événements étendus](../relational-databases/extended-events/extended-events.md) | sp_trace_create<br /><br />sp_trace_setevent<br /><br />sp_trace_setfilter<br /><br />sp_trace_setstatus<br /><br />fn_trace_geteventinfo<br /><br />fn_trace_getfilterinfo<br /><br />fn_trace_getinfo<br /><br />fn_trace_gettable<br /><br />sys.traces<br /><br />sys.trace_events<br /><br />sys.trace_event_bindings<br /><br />sys.trace_categories<br /><br />sys.trace_columns<br /><br />sys.trace_subclass_values |

### <a name="system-views"></a>Vues système

| Fonctionnalité déconseillée | Remplacement | Nom de la fonctionnalité |
|--------------------|-------------|--------------|
| sys.sql_dependencies|sys.sql_expression_dependencies|sys.sql_dependencies |

### <a name="table-compression"></a>Compression de table

| Fonctionnalité déconseillée | Remplacement | Nom de la fonctionnalité |
|--------------------|-------------|--------------|
| Utilisation du format de stockage vardecimal. | Le format de stockage vardecimal est déconseillé. La compression des données SQL Server 2019 (15.x) compresse les valeurs décimales ainsi que d’autres types de données. Nous vous recommandons d'utiliser la compression de données au lieu du format de stockage vardecimal. | Format de stockage vardecimal |
| Utilisation de la procédure sp_db_vardecimal_storage_format.|Le format de stockage vardecimal est déconseillé. La compression des données SQL Server 2019 (15.x) compresse les valeurs décimales ainsi que d’autres types de données. Nous vous recommandons d'utiliser la compression de données au lieu du format de stockage vardecimal. | sp_db_vardecimal_storage_format |
| Utilisation de la procédure sp_estimated_rowsize_reduction_for_vardecimal.|Utilisez à la place la compression de données et la procédure sp_estimate_data_compression_savings. |sp_estimated_rowsize_reduction_for_vardecimal |

### <a name="text-pointers"></a>Pointeurs de texte

| Fonctionnalité déconseillée | Remplacement | Nom de la fonctionnalité |
|--------------------|-------------|--------------|
| WRITETEXT<br /><br />UPDATETEXT<br /><br />READTEXT|None|UPDATETEXT ou WRITETEXT<br /><br />READTEXT |
| TEXTPTR()<br /><br />TEXTVALID() | None | TEXTPTR<br /><br />TEXTVALID |

### <a name="transact-sql"></a>Transact-SQL

| Fonctionnalité déconseillée | Remplacement | Nom de la fonctionnalité |
|--------------------|-------------|--------------|
| Séquence d'appel de fonction :: | Remplacée par SELECT *column_list* FROM sys.\<*function_name*>().<br /><br />Par exemple, remplacez `SELECT * FROM ::fn_virtualfilestats(2,1)`par `SELECT * FROM sys.fn_virtualfilestats(2,1)`. | Syntaxe d'appel de fonction '::' |
| Références de colonnes en 3 et 4 parties. | Les noms en deux parties sont la norme.|Nom de la colonne à plus de deux parties |
| Une chaîne entre guillemets utilisée en tant qu'alias de colonne pour une expression dans une liste SELECT :<br /><br />'*string_alias*' = *expression* | *expression* [AS] *column_alias*<br /><br />*expression* [AS] [*column_alias*]<br /><br />*expression* [AS] "*column_alias*"<br /><br />*expression* [AS] '*column_alias*'<br /><br />*column_alias* = *expression* | Littéraux de chaîne comme alias de colonne |
| Procédures numérotées | Aucun. Ne pas utiliser. | ProcNums |
| Syntaxe *table_name.index_name* dans DROP INDEX|Syntaxe *index_name* ON *table_name* dans DROP INDEX.|DROP INDEX avec nom en deux parties |
| Pas d’instructions Transact-SQL se terminant par un point-virgule.|Terminez les instructions Transact-SQL par un point-virgule ( ; ). | None |
| GROUP BY ALL|Utilisez la solution personnalisée cas par cas avec UNION ou une table dérivée. | GROUP BY ALL |
| ROWGUIDCOL comme nom de colonne dans les instructions DML.|Utilisez $rowguid.|ROWGUIDCOL |
| IDENTITYCOL comme nom de colonne dans les instructions DML.|Utilisez $identity.|IDENTITYCOL |
| Utilisation de #, ## comme table temporaire et noms de procédure stockée temporaires. | Utilisez au moins un caractère supplémentaire.|'#' et '##' comme nom des tables temporaires et procédures stockées
| Utilisez \@, \@\@ ou \@\@ comme identificateurs Transact-SQL. | N’utilisez pas \@, \@\@ ou des noms commençant par \@\@ comme identificateurs. | \@ et noms commençant par \@\@ comme identificateurs Transact-SQL |
| Utilisation du mot clé DEFAULT comme valeur par défaut.|N'utilisez pas le mot DEFAULT comme valeur par défaut. | Mot clé DEFAULT comme valeur par défaut. |
| Utilisation d'un espace comme séparateur entre les indicateurs de table.|Utilisez une virgule pour séparer les indicateurs de table. | Indicateurs de table multiples sans virgule |
| La liste de sélection d’une vue indexée d’agrégation doit contenir COUNT_BIG (\*) dans le mode de compatibilité 90. | Utilisez COUNT_BIG (\*). | Liste de sélection de vue d’index sans COUNT_BIG(\*) |
| Application indirecte des indicateurs de table à un appel d'une fonction table à plusieurs instructions via une vue.|Aucun.|Indicateurs TVF indirects |
| Syntaxe ALTER DATABASE :<br /><br />MODIFY FILEGROUP READONLY<br /><br />MODIFY FILEGROUP READWRITE | MODIFY FILEGROUP READ_ONLY<br /><br />MODIFY FILEGROUP READ_WRITE | MODIFY FILEGROUP READONLY<br /><br />MODIFY FILEGROUP READWRITE |
| SET ANSI_NULLS OFF et option de base de données ANSI_NULLS OFF<br /><br />SET ANSI_PADDING OFF et option de base de données ANSI_PADDING OFF<br /><br />SET CONCAT_NULL_YIELDS_NULL OFF et option de base de données CONCAT_NULL_YIELDS_NULL OFF<br /><br />SET OFFSETS | Aucun. <br /><br /> ANSI_NULLS, ANSI_PADDING et CONCAT_NULLS_YIELDS_NULL ont toujours la valeur ON. SET OFFSETS n’est pas disponible. | SET ANSI_NULLS OFF <br /><br /> SET ANSI_PADDING OFF<br /><br />SET CONCAT_NULL_YIELDS_NULL OFF<br /><br />SET OFFSETS<br /><br />ALTER DATABASE SET ANSI_NULLS OFF<br /><br />ALTER DATABASE SET ANSI_PADDING OFF <br /><br /> ALTER DATABASE SET CONCAT_NULL_YIELDS_NULL OFF |
| SET FMTONLY | [sys.dm_exec_describe_first_result_set &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-exec-describe-first-result-set-transact-sql.md), [sys.dm_exec_describe_first_result_set_for_object &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-exec-describe-first-result-set-for-object-transact-sql.md), [sp_describe_first_result_set &#40;Transact-SQL&#41;](../relational-databases/system-stored-procedures/sp-describe-first-result-set-transact-sql.md), and [sp_describe_undeclared_parameters &#40;Transact-SQL&#41;](../relational-databases/system-stored-procedures/sp-describe-undeclared-parameters-transact-sql.md). | SET FMTONLY |
| Spécification de NOLOCK ou READUNCOMMITTED dans la clause FROM d'une instruction UPDATE ou DELETE. | Supprimez les indicateurs de table NOLOCK ou READUNCOMMITTED de la clause FROM. | NOLOCK ou READUNCOMMITTED dans UPDATE ou DELETE |
| Spécification des indicateurs de table sans utilisation du mot clé WITH. | Utilisez WITH. | Indicateur de table sans WITH |
| INSERT_HINTS | | INSERT_HINTS |

### <a name="tools"></a>Outils

| Fonctionnalité déconseillée | Remplacement | Nom de la fonctionnalité |
|--------------------|-------------|--------------|
| Générateur de profils SQL Server pour la capture de trace | Utilisez le Générateur de profils d'événements étendus incorporé dans SQL Server Management Studio.|SQL Server Profiler |
| SQL Server Profiler pour Trace Replay | [SQL Server Distributed Replay](../tools/distributed-replay/sql-server-distributed-replay.md) |

### <a name="trace-management-objects"></a>Objets TMO (Trace Management Objects)

| Fonctionnalité déconseillée | Remplacement | Nom de la fonctionnalité |
|--------------------|-------------|--------------|
| L'espace de noms Microsoft.SqlServer.Management.Trace (contient les API pour les objets Trace et Replay SQL Server)|Configuration de trace : <xref:Microsoft.SqlServer.Management.XEvent><br /><br />Lecture de trace : <xref:Microsoft.SqlServer.XEvent.Linq><br /><br />Relecture de trace : None | |

### <a name="xml"></a>XML

| Fonctionnalité déconseillée | Remplacement | Nom de la fonctionnalité |
|--------------------|-------------|--------------|
| Génération de schéma XDR en ligne | La directive XMLDATA de l'option FOR XML est déconseillée. Utilisez la génération XSD en mode RAW et AUTO. Il n'existe aucune solution de remplacement pour la directive XMLDATA en mode EXPLICIT. | XMLDATA |

> [!NOTE]
> Le paramètre **OUTPUT** de cookie pour **sp_setapprole** est actuellement documenté comme **varbinary(8000)** , ce qui correspond à la longueur maximale correcte. Cependant, l’implémentation actuelle retourne **varbinary(50)** . Si les développeurs ont alloué **varbinary(50)** , l’application peut nécessiter des modifications si la taille de retour des cookies augmente dans une future version. Bien qu'il ne s'agisse pas d'un problème de suppression de fonctionnalités, ce phénomène est mentionné dans cette rubrique car les réglages de l'application sont similaires. Pour plus d’informations, consultez [sp_setapprole &#40;Transact-SQL&#41;](../relational-databases/system-stored-procedures/sp-setapprole-transact-sql.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Discontinued Database Engine Functionality in SQL Server 2016 (Fonctionnalités du moteur de base de données supprimées dans SQL Server 2016)](./discontinued-database-engine-functionality-in-sql-server.md)  
