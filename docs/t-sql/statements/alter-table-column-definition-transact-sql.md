---
description: ALTER TABLE column_definition (Transact-SQL)
title: column_definition (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/24/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- column_definition
- column_definition_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- column_definition
- ALTER TABLE statement
- column properties [SQL Server]
- column definitions [SQL Server]
ms.assetid: a1742649-ca29-4d9b-9975-661cdbf18f78
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 001417f19035ab9a304f2af3ae42207bd7600fb1
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170831"
---
# <a name="alter-table-column_definition-transact-sql"></a>ALTER TABLE column_definition (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi](../../includes/applies-to-version/sql-asdb-asdbmi.md)]

  Spécifie les propriétés d’une colonne ajoutées à une table à l’aide de l’instruction [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md).  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
column_name <data_type>  
[ FILESTREAM ]  
[ COLLATE collation_name ]   
[ NULL | NOT NULL ]  
[   
    [ CONSTRAINT constraint_name ] DEFAULT constant_expression [ WITH VALUES ]   
    | IDENTITY [ ( seed , increment ) ] [ NOT FOR REPLICATION ]   
]  
[ ROWGUIDCOL ]   
[ SPARSE ]   
[ ENCRYPTED WITH  
  ( COLUMN_ENCRYPTION_KEY = key_name ,  
      ENCRYPTION_TYPE = { DETERMINISTIC | RANDOMIZED } ,   
      ALGORITHM =  'AEAD_AES_256_CBC_HMAC_SHA_256'   
  ) ]  
[ MASKED WITH ( FUNCTION = ' mask_function ') ]  
[ <column_constraint> [ ...n ] ]  
  
<data type> ::=   
[ type_schema_name . ] type_name   
    [ ( precision [ , scale ] | max |   
        [ { CONTENT | DOCUMENT } ] xml_schema_collection ) ]   
  
<column_constraint> ::=   
[ CONSTRAINT constraint_name ]   
{     { PRIMARY KEY | UNIQUE }   
        [ CLUSTERED | NONCLUSTERED ]   
        [   
            WITH FILLFACTOR = fillfactor    
          | WITH ( < index_option > [ , ...n ] )   
        ]   
        [ ON { partition_scheme_name ( partition_column_name )   
            | filegroup | "default" } ]  
  | [ FOREIGN KEY ]   
        REFERENCES [ schema_name . ] referenced_table_name [ ( ref_column ) ]   
        [ ON DELETE { NO ACTION | CASCADE | SET NULL | SET DEFAULT } ]   
        [ ON UPDATE { NO ACTION | CASCADE | SET NULL | SET DEFAULT } ]   
        [ NOT FOR REPLICATION ]   
  | CHECK [ NOT FOR REPLICATION ] ( logical_expression )   
}  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *column_name*  
 Nom de la colonne à ajouter, modifier ou supprimer. *column_name* peut être composé de 1 à 128 caractères. Pour les nouvelles colonnes, créées avec un type de données timestamp, *column_name* peut être omis. Si aucun *column_name* n’est spécifié pour une colonne de type de données **timestamp**, le **timestamp** du nom est utilisé.  
  
 [ _type_schema_name_ **.** ] *type_name*  
 Type de données de la colonne ajoutée et schéma auquel elle appartient.  
  
 *type_name* peut être l’un des types suivants :  
  
-   type de données système [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ;  
  
-   type de données alias dérivé d'un type de données système [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Les types de données d'alias doivent être créés à l'aide de CREATE TYPE avant de pouvoir être utilisés dans une définition de table ;  
  
-   type [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] défini par l’utilisateur et schéma auquel il appartient. Un type de données [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] doit être créé à l'aide de CREATE TYPE avant de pouvoir être utilisé dans une définition de table.  
  
 Si *type_schema_name* n’est pas spécifié, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] [!INCLUDE[msCoName](../../includes/msconame-md.md)] référence *type_name* dans l’ordre suivant :  
  
-   Le type de données système [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
  
-   Le schéma par défaut de l'utilisateur actuel dans la base de données active  
  
-   Schéma **dbo** dans la base de données active.  
  
*precision*  
 Précision du type de données spécifié. Pour plus d’informations sur les valeurs de précision valides, consultez [Précision, échelle et longueur&#40;Transact-SQL&#41;](../../t-sql/data-types/precision-scale-and-length-transact-sql.md).  
  
*scale*  
 Échelle du type de données spécifié. Pour plus d’informations sur les valeurs d’échelle valides, consultez [Précision, échelle et longueur &#40;Transact-SQL&#41;](../../t-sql/data-types/precision-scale-and-length-transact-sql.md).  
  
**max**  
 S’applique uniquement aux types de données **varchar**, **nvarchar** et **varbinary**. Ceux-ci permettent de stocker 2^31 octets de données de caractères et binaires ou 2^30 octets de données Unicode.  
  
**CONTENT**  
 Spécifie que chaque instance du type de données **xml** dans *column_name* peut comprendre plusieurs éléments de niveau supérieur. CONTENT s’applique uniquement au type de données **xml** et ne peut être spécifié que si *xml_schema_collection* l’est également. Si celui-ci n'est pas spécifié, CONTENT détermine le comportement par défaut.  
  
DOCUMENT  
 Spécifie que chaque instance du type de données **xml** dans *column_name* ne peut comprendre qu’un seul élément de niveau supérieur. DOCUMENT s’applique uniquement au type de données **xml** et ne peut être spécifié que si *xml_schema_collection* l’est également.  
  
 *xml_schema_collection*  
 **S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.  
  
 S’applique uniquement au type de données **xml** pour l’association d’une collection de schémas XML au type. Avant d’inclure une colonne **xml** dans un schéma, vous devez d’abord créer ce dernier dans la base de données à l’aide de [CREATE XML SCHEMA COLLECTION](../../t-sql/statements/create-xml-schema-collection-transact-sql.md).  
  
FILESTREAM  
 Spécifie éventuellement l’attribut de stockage FILESTREAM pour la colonne dont le *type_name* est **varbinary(max)** .  
  
 Quand FILESTREAM est spécifié pour une colonne, la table doit également avoir une colonne avec le type de données **uniqueidentifier** qui a l’attribut ROWGUIDCOL. Cette colonne ne doit pas autoriser les valeurs Null et doit avoir une contrainte de colonne unique de type UNIQUE ou PRIMARY KEY. La valeur GUID de la colonne doit être fournie par une application quand des données sont insérées ou par une contrainte DEFAULT qui utilise la fonction NEWID().  
  
 La colonne ROWGUIDCOL ne peut pas être supprimée et les contraintes liées ne peuvent pas être modifiées tant qu'une colonne FILESTREAM est définie pour la table. La colonne ROWGUIDCOL peut être supprimée uniquement lorsque la dernière colonne FILESTREAM a été supprimée.  
  
 Lorsque l'attribut de stockage FILESTREAM est spécifié pour une colonne, toutes les valeurs de cette colonne sont stockées dans un conteneur de données FILESTREAM sur le système de fichiers.  
  
 Pour obtenir un exemple qui indique comment utiliser la définition de colonne, consultez [FILESTREAM &#40;SQL Server&#41;](../../relational-databases/blob/filestream-sql-server.md).  
  
COLLATE *collation_name*  
 Indique le classement de la colonne. Si l'argument n'est pas spécifié, c'est le classement par défaut de la base de données qui est affecté à la colonne. Le nom du classement peut être un nom de classement Windows ou un nom de classement SQL. Pour en obtenir la liste et des informations supplémentaires, consultez [Nom de classement Windows &#40;Transact-SQL&#41;](../../t-sql/statements/windows-collation-name-transact-sql.md) et [Nom du classement SQL Server &#40;Transact-SQL&#41;](../../t-sql/statements/sql-server-collation-name-transact-sql.md).  
  
 Vous pouvez utiliser la clause COLLATE pour spécifier les classements uniquement des colonnes ayant les types de données **char**, **varchar**, **nchar** et **nvarchar**.  
  
 Pour plus d’informations sur la clause COLLATE, consultez [COLLATE &#40;Transact-SQL&#41;](~/t-sql/statements/collations.md).  
  
 NULL | NOT NULL  
 Détermine si les valeurs Null sont autorisées dans la colonne. NULL n'est pas strictement une contrainte, mais peut être spécifié comme NOT NULL.  
  
[ CONSTRAINT *constraint_name* ]  
 Spécifie le début d'une définition de valeur DEFAULT. Pour maintenir la compatibilité avec les versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], un nom de contrainte peut être affecté à une définition DEFAULT. *constraint_name* doit suivre les règles applicables aux [identificateurs](../../relational-databases/databases/database-identifiers.md), excepté que le nom ne peut pas commencer par un signe dièse (#). Si *constraint_name* n’est pas spécifié, un nom généré par le système est affecté à la définition DEFAULT.  
  
DEFAULT  
 Mot clé spécifiant la valeur par défaut de la colonne. Une définition DEFAULT peut servir à définir les valeurs d'une nouvelle colonne dans les lignes de données existantes. Les définitions DEFAULT ne peuvent pas être appliquées aux colonnes de type **timestamp** ni à celles qui possèdent une propriété IDENTITY. Si une valeur par défaut est spécifiée pour une colonne de type défini par l’utilisateur, le type doit prendre en charge une conversion implicite de *constant_expression* vers le type défini par l’utilisateur.  
  
*constant_expression*  
 Valeur littérale, valeur NULL ou fonction système utilisée comme valeur de colonne par défaut. Si cet argument est utilisé avec une colonne définie pour être d’un type défini par l’utilisateur [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)], l’implémentation du type doit prendre en charge une conversion implicite de *constant_expression* vers le type défini par l’utilisateur.  
  
WITH VALUES   
 En cas d’ajout d’une colonne AND qui autorise les valeurs NULL avec WITH VALUES, ainsi que d’une contrainte DEFAULT, attribuez, pour les lignes existantes, la valeur donnée dans *constant_expression* DEFAULT à la nouvelle colonne. Si la colonne ajoutée n’autorise pas les valeurs NULL, sa valeur sera toujours celle qui est donnée dans *l’expression constante* DEFAULT pour les lignes existantes. À partir de SQL Server 2012, il peut s’agir d’une opération de métadonnées [adding-not-null-columns-as-an-online-operation](alter-table-transact-sql.md#adding-not-null-columns-as-an-online-operation).
Si elle est utilisée sans que la colonne associée soit elle aussi ajoutée, elle n’a aucun effet.
 
 Spécifie que la valeur donnée dans DEFAULT *constant_expression* est stockée dans une nouvelle colonne ajoutée aux lignes existantes. Si la colonne ajoutée accepte les valeurs NULL et que l'option WITH VALUES est spécifiée, la valeur par défaut est stockée dans la nouvelle colonne ajoutée aux lignes existantes. Si WITH VALUES n'est pas spécifiée pour les colonnes acceptant les valeurs NULL, la valeur NULL est stockée dans la nouvelle colonne, dans des lignes existantes. Si la nouvelle colonne n'accepte pas les valeurs NULL, la valeur par défaut est stockée dans les nouvelles lignes, que l'option WITH VALUES soit spécifiée ou pas.  
  
IDENTITY  
 Spécifie que la nouvelle colonne est une colonne d'identité. Le [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] fournit une valeur incrémentielle unique pour la colonne. Lorsque vous ajoutez des colonnes d'identificateur à des tables existantes, les numéros d'identité sont ajoutés aux lignes existantes de la table avec les valeurs de départ et d'incrément. L'ordre dans lequel les lignes sont mises à jour n'est pas garanti. Des numéros d'identité sont également générés pour toutes les nouvelles lignes qui sont ajoutées.  
  
 Les colonnes d'identité sont normalement utilisées avec les contraintes PRIMARY KEY comme identificateur unique de ligne pour la table. La propriété IDENTITY peut être affectée à une colonne **tinyint**, **smallint**, **int**, **bigint**, **decimal(p,0)** ou **numeric(p,0)** . Une seule colonne d'identité peut être créée par table. Vous ne pouvez pas utiliser le mot clé DEFAULT ni les valeurs par défaut liées avec une colonne d'identité. Vous devez spécifier à la fois la valeur de départ et l'incrément ou aucune de ces valeurs. Si aucune valeur n'est spécifiée, la valeur par défaut est de (1,1).  
  
> [!NOTE]  
>  Vous ne pouvez pas modifier une colonne de table existante pour ajouter la propriété IDENTITY.  
  
 L'ajout d'une colonne d'identité à une table publiée n'est pas pris en charge car la réplication de la colonne vers l'Abonné peut aboutir à une non convergence. Les valeurs de la colonne d'identité sur l'Abonné dépendent de l'ordre dans lequel les lignes sont physiquement stockées dans la table affectée. Les lignes pouvant être stockées différemment sur l'Abonné, la valeur de la colonne d'identité peut différer pour les mêmes lignes.  
  
 Pour désactiver la propriété IDENTITY d’une colonne en autorisant l’insertion explicite de valeurs, utilisez [SET IDENTITY_INSERT](../../t-sql/statements/set-identity-insert-transact-sql.md).  
  
*seed*  
 Valeur utilisée pour la première ligne chargée dans la table.  
  
*increment*  
 Valeur d'incrément ajoutée à la valeur d'identité de la ligne précédemment chargée.  
  
NOT FOR REPLICATION  
 **S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.  
  
 Peut être spécifié pour la propriété IDENTITY. Si cette clause est spécifiée pour la propriété IDENTITY, les valeurs ne sont pas incrémentées dans les colonnes d'identité lorsque les Agents de réplication effectuent des opérations d'insertion.  
  
ROWGUIDCOL  
 **S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.  
  
 Spécifie que la colonne est une colonne d'identificateur unique global de ligne. ROWGUIDCOL eut uniquement être affecté à une colonne **uniqueidentifier**, et une seule colonne **uniqueidentifier** par table peut être désignée comme colonne ROWGUIDCOL. ROWGUIDCOL ne peut pas être affecté aux colonnes de types de données définis par l'utilisateur.  
  
 ROWGUIDCOL n'assure pas l'unicité des valeurs stockées dans la colonne. En outre, ROWGUIDCOL ne peut de plus générer automatiquement de valeurs pour les nouvelles lignes insérées dans la table. Pour générer des valeurs uniques pour chaque colonne, utilisez la fonction NEWID sur des instructions INSERT, ou définissez la fonction NEWID par défaut pour la colonne. Pour plus d’informations, consultez [NEWID &#40;Transact-SQL&#41;](../../t-sql/functions/newid-transact-sql.md) et [INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md).  
  
SPARSE  
 Indique que la nouvelle colonne est une colonne éparse. Le stockage des colonnes éparses est optimisé pour les valeurs Null. Les colonnes éparses ne peuvent pas être désignées comme NOT NULL. Pour connaître les restrictions supplémentaires et obtenir plus d’informations sur les colonnes éparses, consultez [Utiliser des colonnes éparses](../../relational-databases/tables/use-sparse-columns.md).  
  
\<column_constraint>  
 Pour obtenir les définitions des arguments de contrainte de colonne, consultez [column_constraint &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-column-constraint-transact-sql.md).  
  
 ENCRYPTED WITH  
 Indique que les colonnes doivent être chiffrées à l’aide de la fonctionnalité [Always Encrypted](../../relational-databases/security/encryption/always-encrypted-database-engine.md).  
  
 COLUMN_ENCRYPTION_KEY = *key_name*  
 Spécifie la clé de chiffrement de colonne. Pour plus d’informations, consultez [CREATE COLUMN ENCRYPTION KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-column-encryption-key-transact-sql.md).  
  
ENCRYPTION_TYPE = { DETERMINISTIC | RANDOMIZED }  
 Le **chiffrement déterministe** utilise une méthode qui génère toujours la même valeur chiffrée pour une valeur de texte brut donnée. L’utilisation du chiffrement déterministe permet d’effectuer des recherches à l’aide de la comparaison d’égalité, d’effectuer des regroupements et de joindre des tables à l’aide de jointures d’égalité sur la base de valeurs chiffrées, mais elle peut aussi permettre à des utilisateurs non autorisés de deviner des informations concernant les valeurs chiffrées en examinant les séquences dans la colonne chiffrée. Joindre deux tables sur des colonnes chiffrées de façon déterministe n’est possible que si les deux colonnes sont chiffrées à l’aide de la même clé de chiffrement de colonne. Le chiffrement déterministe doit utiliser un classement de colonne avec un ordre de tri binaire 2 pour les colonnes de type caractère.  
  
 Le **chiffrement aléatoire** utilise une méthode qui chiffre les données de manière moins prévisible. Le chiffrement aléatoire est plus sécurisé, mais il empêche tout calcul et indexation sur les colonnes chiffrées, sauf si votre instance de SQL Server prend en charge [Always Encrypted avec enclaves sécurisées](../../relational-databases/security/encryption/always-encrypted-enclaves.md).
  
 Si vous utilisez Always Encrypted (sans enclaves sécurisées), utilisez le chiffrement déterministe pour les colonnes dans lesquelles effectuer des recherches avec des paramètres ou des paramètres de regroupement, par exemple un numéro d’identification gouvernemental. Utilisez le chiffrement aléatoire pour les données telles qu’un numéro de carte de crédit, qui ne sont pas regroupées avec d’autres enregistrements, ou utilisées pour joindre des tables, et qui ne sont pas soumises à des recherches car vous utilisez d’autres colonnes (par exemple un numéro de transaction) pour rechercher la ligne qui contient la colonne chiffrée qui vous intéresse.  

 Si vous utilisez Always Encrypted avec enclaves sécurisées, le chiffrement aléatoire est un type de chiffrement recommandé.  
  
 Les colonnes doivent être d’un type de données qualifié.  
  
ALGORITHM  
**S’applique à** : [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] et versions ultérieures, [!INCLUDE[ssSDS](../../includes/sssds-md.md)].  
Doit être **'AEAD_AES_256_CBC_HMAC_SHA_256'** .  
  
 Pour plus d’informations, notamment sur les contraintes de fonctionnalité, consultez [Always Encrypted &#40;moteur de base de données&#41;](../../relational-databases/security/encryption/always-encrypted-database-engine.md).  
  
   
ADD MASKED WITH ( FUNCTION = ' *mask_function* ')  
 **S’applique à** : [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] et versions ultérieures, [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].  
  
 Spécifie un masque dynamique des données. *mask_function* est le nom de la fonction de masquage avec les paramètres appropriés. Les options suivantes sont disponibles :  
  
-   default()  
  
-   email()  
  
-   partial()  
  
-   random()  
  
 Pour les paramètres de fonction, consultez [Masquage dynamique des données](../../relational-databases/security/dynamic-data-masking.md).  
  
## <a name="remarks"></a>Notes  
 Si une colonne ayant le type de données **uniqueidentifier** est ajoutée, elle peut être définie avec une valeur par défaut qui utilise la fonction NEWID() pour fournir les valeurs d’identificateur uniques dans la nouvelle colonne, pour chaque ligne existante de la table.  
  
 Le [!INCLUDE[ssDE](../../includes/ssde-md.md)] n'impose pas un ordre pour la spécification de DEFAULT, IDENTITY, ROWGUIDCOL ou de contraintes de colonne dans une définition de colonne.  
  
 L'instruction ALTER TABLE échoue si l'ajout de la colonne provoque un dépassement de la taille de ligne de données de 8 060 octets.  
  
## <a name="examples"></a>Exemples  
 Pour obtenir des exemples, consultez [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md).  
  
## <a name="see-also"></a>Voir aussi  
 [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)  
  
