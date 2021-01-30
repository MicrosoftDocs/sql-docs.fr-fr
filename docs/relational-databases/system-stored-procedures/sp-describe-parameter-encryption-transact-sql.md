---
description: sp_describe_parameter_encryption (Transact-SQL)
title: sp_describe_parameter_encryption (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/27/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: vanto
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_describe_parameter_encryption
- sp_describe_parameter_encryption_TSQL
- sys.sp_describe_parameter_encryption
- sys.sp_describe_parameter_encryption_TSQL
helpviewer_keywords:
- sp_describe_parameter_encryption
ms.assetid: 706ed441-2881-4934-8d5e-fb357ee067ce
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 41821edc34835f60cef201ae78b3448f3d3f4699
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200774"
---
# <a name="sp_describe_parameter_encryption-transact-sql"></a>sp_describe_parameter_encryption (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

  Analyse l’instruction spécifiée [!INCLUDE[tsql](../../includes/tsql-md.md)] et ses paramètres pour déterminer les paramètres qui correspondent aux colonnes de base de données qui sont protégées à l’aide de la fonctionnalité Always Encrypted. Retourne les métadonnées de chiffrement pour les paramètres qui correspondent aux colonnes chiffrées.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
sp_describe_parameter_encryption   
    [ @tsql = ] N'Transact-SQL_batch' ,   
    [ @params = ] N'parameters'   
[ ;]  
```  
  
## <a name="arguments"></a>Arguments  
 [ \@ tsql =] 'Transact-SQL_batch'  
 Une ou plusieurs instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] Transact-SQL_batch peut être de type nvarchar (n) ou nvarchar (max).  
  
 [ \@ params =] N’parameters'  
 *\@ params* fournit une chaîne de déclaration pour les paramètres du lot Transact-SQL, qui est similaire à sp_executesql. Les paramètres peuvent être de type nvarchar (n) ou nvarchar (max).  
  
 Est une chaîne qui contient les définitions de tous les paramètres qui ont été incorporés dans le [!INCLUDE[tsql](../../includes/tsql-md.md)] _batch. Cette chaîne doit être une constante Unicode ou une variable Unicode. Chaque définition de paramètre se compose d'un nom de paramètre et d'un type de données. *n* est un espace réservé qui indique des définitions de paramètres supplémentaires. Chaque paramètre spécifié dans l’instruction doit être défini dans *\@ params*. Si l' [!INCLUDE[tsql](../../includes/tsql-md.md)] instruction ou le lot dans l’instruction ne contient pas de paramètres, *\@ params* n’est pas obligatoire. La valeur par défaut de ce paramètre est NULL.  
  
## <a name="return-value"></a>Valeur renvoyée  
 0 indique une réussite. Tout autre indicateur indique un échec.  
  
## <a name="result-sets"></a>Jeux de résultats  
 **sp_describe_parameter_encryption** retourne deux jeux de résultats :  
  
-   Ensemble de résultats décrivant les clés de chiffrement configurées pour les colonnes de base de données, les paramètres de l' [!INCLUDE[tsql](../../includes/tsql-md.md)] instruction spécifiée correspondent à.  
  
-   Jeu de résultats décrivant le mode de chiffrement des paramètres particuliers. Ce jeu de résultats fait référence aux clés décrites dans le premier jeu de résultats.  
  
 Chaque ligne du premier jeu de résultats décrit une paire de clés ; clé de chiffrement de colonne chiffrée et clé principale de colonne correspondante.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**column_encryption_key_ordinal**|**int**|ID de la ligne dans le jeu de résultats.|  
|**database_id**|**int**|ID de la base de données.|  
|**column_encryption_key_id**|**int**|ID de la clé de chiffrement de colonne. Remarque : cet ID désigne une ligne dans le sys.column_encryption_keys &#40;l’affichage catalogue [Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-column-encryption-keys-transact-sql.md) .|  
|**column_encryption_key_version**|**int**|Réservé pour un usage futur. Actuellement, contient toujours 1.|  
|**column_encryption_key_metadata_version**|**Binary(8**|Horodateur représentant l’heure de création de la clé de chiffrement de colonne.|  
|**column_encryption_key_encrypted_value**|**varbinary(4000)**|Valeur chiffrée de la clé de chiffrement de colonne.|  
|**column_master_key_store_provider_name**|**sysname**|Nom du fournisseur du magasin de clés qui contient la clé principale de colonne, qui a été utilisé pour produire la valeur chiffrée de la clé de chiffrement de colonne.|  
|**column_master_key_path**|**nvarchar(4000)**|Chemin d’accès de clé de la clé principale de colonne, qui a été utilisé pour produire la valeur chiffrée de la clé de chiffrement de colonne.|  
|**column_encryption_key_encryption_algorithm_name**|**sysname**|Nom de l’algorithme de chiffrement utilisé pour produire la valeur de chiffrement de la clé de chiffrement de colonne.|  
  
 Chaque ligne du deuxième jeu de résultats contient des métadonnées de chiffrement pour un paramètre.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**parameter_ordinal**|**int**|ID de la ligne dans le jeu de résultats.|  
|**parameter_name**|**sysname**|Nom de l’un des paramètres spécifiés dans l’argument *\@ params* .|  
|**column_encryption_algorithm**|**tinyint**|Code indiquant l’algorithme de chiffrement configuré pour la colonne, le paramètre correspond à. Les valeurs actuellement prises en charge sont les suivantes : 2 pour **AEAD_AES_256_CBC_HMAC_SHA_256**.|  
|**column_encryption_type**|**tinyint**|Code indiquant le type de chiffrement configuré pour la colonne, le paramètre correspond à. Les valeurs prises en charge sont les suivantes :<br /><br /> 0-texte en clair (la colonne n’est pas chiffrée)<br /><br /> 1-chiffrement aléatoire<br /><br /> 2-chiffrement déterministe.|  
|**column_encryption_key_ordinal**|**int**|Code de la ligne dans le premier jeu de résultats. La ligne référencée décrit la clé de chiffrement de colonne configurée pour la colonne, le paramètre correspond à.|  
|**column_encryption_normalization_rule_version**|**tinyint**|Numéro de version de l’algorithme de normalisation de type.|  
  
## <a name="remarks"></a>Notes  
 Un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pilote client, prenant en charge Always Encrypted, appelle automatiquement **sp_describe_parameter_encryption** pour récupérer les métadonnées de chiffrement pour les requêtes paramétrables, émises par l’application. Par la suite, le pilote utilise les métadonnées de chiffrement pour chiffrer les valeurs des paramètres qui correspondent aux colonnes de base de données protégées par Always Encrypted et substitue les valeurs de paramètre de texte en clair, soumises par l’application, aux valeurs de paramètre chiffrées, avant d’envoyer la requête au moteur de base de données.  
  
## <a name="permissions"></a>Autorisations  
 Exigez les autorisations **afficher une définition de clé de chiffrement de colonne** et **afficher toutes les définitions de clé principale de colonne** dans la base de données.  
  
## <a name="examples"></a>Exemples  
  
```sql  
CREATE COLUMN MASTER KEY [CMK1]  
WITH  
(  
       KEY_STORE_PROVIDER_NAME = N'MSSQL_CERTIFICATE_STORE',  
    KEY_PATH = N'CurrentUser/my/A66BB0F6DD70BDFF02B62D0F87E340288E6F9305'  
);  
GO  
  
CREATE COLUMN ENCRYPTION KEY [CEK1]  
WITH VALUES  
(  
       COLUMN_MASTER_KEY = [CMK1],  
    ALGORITHM = 'RSA_OAEP',  
    ENCRYPTED_VALUE =   
0x016E000001630075007200720065006E00740075007300650072002F006D007  
9002F00610036003600620062003000660036006400640037003000620064006  
6006600300032006200360032006400300066003800370065003300340030003  
200380038006500360066003900330030003500CA0D0CEC74ECADD1804CF991  
37B4BD06BBAB15D7EA74E0C249A779C7768A5B659E0125D24FF827F5EA8CA51  
7A8E197ECA1353BA814C2B0B2E6C8AB36E3AE6A1E972D69C3C573A963ADAB6  
686CF5D24F95FE43140C4F9AF48FBA7DF2D053F3B4A1F5693A1F905440F8015B  
DB43AF8A04BE4E045B89876A0097E5FBC4E6A3B9C3C0D278C540E46C53938B8  
C957B689C4DC095821C465C73117CBA95B758232F9E5B2FCC7950B8CA00AFE3  
74DE42847E3FBC2FDD277035A2DEF529F4B735C20D980073B4965B4542A3472  
3276A1646998FC6E1C40A3FDB6ABCA98EE2B447F114D2AC7FF8C7D51657550E  
C5C2BABFFE8429B851272086DCED94332CF18FA854C1D545A28B1EF4BE64F8E0  
35175C1650F6FC5C4702ACF99850A4542B3747EAEC0CC726E091B36CE24392D8  
01ECAA684DE344FECE05812D12CD72254A014D42D0EABDA41C89FC4F545E88B  
4B8781E5FAF40D7199D4842D2BFE904D209728ED4F527CBC169E2904F6E711FF8  
1A8F4C25382A2E778DD2A58552ED031AFFDA9D9D891D98AD82155F93C58202F  
C24A77F415D4F8EF22419D62E188AC609330CCBD97CEE1AEF8A18B0195883360  
4707FDF03B2B386487CC679D7E352D0B69F9FB002E51BCD814D077E82A09C14E  
9892C1F8E0C559CFD5FA841CEF647DAB03C8191DC46B772E94D579D8C80FE93C  
3827C9F0AE04D5325BC73111E07EEEDBE67F1E2A73580085  
);  
GO  
  
CREATE TABLE t1 (  
c1 INT ENCRYPTED WITH (  
    COLUMN_ENCRYPTION_KEY = [CEK_Auto1],   
    ENCRYPTION_TYPE = Randomized,   
    ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256') NULL,  
);  
  
EXEC sp_describe_parameter_encryption N'INSERT INTO t1 VALUES(@c1)',  N'@c1 INT';  
```  
  
 Voici le premier jeu de résultats :  
  
|column_encryption_key_ordinal|database_id|column_encryption_key_id|column_encryption_key_version|column_encryption_key_metadata_version|column_encryption_key_encrypted_value|  
|--------------------------------------|------------------|---------------------------------|--------------------------------------|------------------------------------------------|-----------------------------------------------|  
|1|5|1|1|0x99EDA60083A50000|0x016E000001630075007200720065006E00740075007300650072002F006D0079002F006100360036006200620030006600360064006400370030006200640066006600300032006200360032006400300066003800370065003300340030003200380038006500360066003900330030003500CA0D0CEC74ECADD1804CF99137B4BD06BBAB15D7EA74E0C249A779C7768A5B659E0125D24FF827F5EA8CA517A8E197ECA1353BA814C2B0B2E6C8AB36E3AE6A1E972D69C3C573A963ADAB6686CF5D24F95FE43140C4F9AF48FBA7DF2D053F3B4A1F5693A1F905440F8015BDB43AF8A04BE4E045B89876A0097E5FBC4E6A3B9C3C0D278C540E46C53938B8C957B689C4DC095821C465C73117CBA95B758232F9E5B2FCC7950B8CA00AFE374DE42847E3FBC2FDD277035A2DEF529F4B735C20D980073B4965B4542A34723276A1646998FC6E1C40A3FDB6ABCA98EE2B447F114D2AC7FF8C7D51657550EC5C2BABFFE8429B851272086DCED94332CF18FA854C1D545A28B1EF4BE64F8E035175C1650F6FC5C4702ACF99850A4542B3747EAEC0CC726E091B36CE24392D801ECAA684DE344FECE05812D12CD72254A014D42D0EABDA41C89FC4F545E88B4B8781E5FAF40D7199D4842D2BFE904D209728ED4F527CBC169E2904F6E711FF81A8F4C25382A2E778DD2A58552ED031AFFDA9D9D891D98 AD82155F93C58202FC24A77F415D4F8EF22419D62E188AC609330CCBD97CEE1AEF8A18B01958833604707FDF03B2B386487CC679D7E352D0B69F9FB002E51BCD814D077E82A09C14E9892C1F8E0C559CFD5FA841CEF647DAB03C8191DC46B772E94D579D8C80FE93C3827C9F0AE04D5325BC73111E07EEEDBE67F1E2A73580085|  
  
 (Les résultats continuent.)  
  
|column_master_key_store_provider_name|column_master_key_path|column_encryption_key_encryption_algorithm_name|  
|------------------------------------------------|-------------------------------|----------------------------------------------------------|  
|MSSQL_CERTIFICATE_STORE|CurrentUser/My/A66BB0F6DD70BDFF02B62D0F87E340288E6F9305|RSA_OAEP|  
  
 Voici le deuxième jeu de résultats :  
  
|parameter_ordinal|parameter_name|column_encryption_algorithm|column_encryption_type|  
|------------------------|---------------------|-----------------------------------|------------------------------|  
|1|\@C1|1|1|  
  
 (Les résultats continuent.)  
  
|column_encryption_key_ordinal|column_encryption_normalization_rule_version|  
|--------------------------------------|------------------------------------------------------|  
|1|1|  
  
## <a name="see-also"></a>Voir aussi  
 [Always Encrypted](../../relational-databases/security/encryption/always-encrypted-database-engine.md)   
 [Développer une application à l’aide de Always Encrypted](../../relational-databases/security/encryption/always-encrypted-client-development.md)  
  
  
