---
description: Options (Explorateur d’objets SQL Server - Page Script)
title: Options (Explorateur d’objets SQL Server - Page Script)
ms.custom: seo-lt-2019
ms.date: 08/01/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- VS.ToolsOptionsPages.ObjectExplorerScripting
- VS.ToolsOptionsPages.Sql_Server_Object_Explorer.ObjectExplorerScripting
ms.assetid: 6105aec9-1b72-4cb2-bd24-fc35f6d95240
author: markingmyname
ms.author: maghan
ms.openlocfilehash: cbaf09a88e5856bfd5c8f36826d8894b947e3c0d
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250498"
---
# <a name="options-sql-server-object-explorer---scripting-page"></a>Options (Explorateur d’objets SQL Server - Page Script)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
 Utilisez cette page pour définir les options de script qui s’appliquent aux commandes suivantes dans les menus contextuels d’objet de **l’Explorateur d’objets** :  
  
-   Commandes **Edition** pour les vues et tables utilisateur.  
  
-   Commandes **Script <object> en tant que** pour les objets créés par l’utilisateur.  
  
-   Commandes **Modifier** pour les objets créés par l’utilisateur.  
  
-   Cette page définit également les valeurs par défaut des options de script de **l’Assistant Génération de scripts SQL Server**.  
  
## <a name="remarks"></a>Notes  
Les commandes **Edition** et **Modifier** et peuvent produire des résultats qui sont différents de ceux de la commande **Script <object> en tant** que pour le même paramètre d’option. Les commandes **Edition** et **Modifier** sont conçues pour modifier des objets de la base de données active durant une session de l’Éditeur de requête. La commande **Script <object> en tant que** est conçue pour générer un script pour qu’il puisse être utilisé ultérieurement pour créer des objets.  
  
## <a name="options"></a>Options  
Spécifiez les options de script en les sélectionnant parmi les paramètres disponibles dans la liste située à droite de chaque option.

> [!NOTE]
> Les paramètres par défaut listés s’appliquent seulement à l’option **Générer un script de la base de données entière et de tous les objets de base de données** et peuvent varier en cas d’utilisation de l’option **Sélectionner des objets de base de données spécifiques**.
  
### <a name="general-scripting-options"></a>Options de script générales  
**Délimiter des instructions individuelles**  
Sépare les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] par un délimiteur de traitement. Pour modifier le délimiteur de traitement par défaut de **l’Éditeur de requête**, sélectionnez **Outils**/**Options**/**Exécution de la requête**/**SQL Server**/**Général**/**Délimiteur de traitement**. La valeur par défaut est FALSE. Pour plus d’informations, consultez [GO (Transact-SQL)](../../t-sql/language-elements/sql-server-utilities-statements-go.md).  
  
**Inclure des en-têtes descriptifs**  
Ajoute des commentaires descriptifs au script en séparant le script en sections pour chaque objet. La valeur par défaut est True. Pour plus d’informations, consultez [/ *...* / (Commentaire) (Transact-SQL)](../../t-sql/language-elements/slash-star-comment-transact-sql.md).  
  
**Inclure l’activation de la compression de vardecimal**  
Inclut les options de stockage VarDecimal. La valeur par défaut est FALSE. Pour plus d’informations, consultez [sp_db_vardecimal_storage_format (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-db-vardecimal-storage-format-transact-sql.md).  
  
**Générer le script de suivi des modifications**  
Inclut des informations de suivi des modifications dans le script.  
  
**Générer un script pour les catalogues de texte intégral**  
Inclut un script pour les catalogues de texte intégral. La valeur par défaut est FALSE. Pour plus d’informations, consultez [CREATE FULLTEXT CATALOG (Transact-SQL)](../../t-sql/statements/create-fulltext-catalog-transact-sql.md).  
  
**Générer un script USE <database>**  
Ajoute l’instruction USE DATABASE au script pour créer des objets de base de données dans le contexte de la base de données de **l’Explorateur d’objets** active. Lorsqu'il est prévu que le script sera utilisé dans une base de données différente, sélectionnez False afin d'omettre l'instruction. La valeur par défaut est True. Pour plus d’informations, consultez [USE (Transact-SQL)](../../t-sql/language-elements/use-transact-sql.md).  
  
### <a name="object-scripting-options"></a>Options de scripts d’objets  

**Vérifier l’existence de l’objet** Vérifiez qu’un objet avec le nom donné existe avant de le supprimer ou le modifier, ou qu’un objet avec le nom donné n’existe pas avant de le créer. Pour plus d’informations, consultez [IF... ELSE (Transact-SQL)](../../t-sql/language-elements/if-else-transact-sql.md) et [EXISTS (Transact-SQL)](../../t-sql/language-elements/exists-transact-sql.md).

**Générer un script pour les objets dépendants**  
Génère un script pour les objets supplémentaires qui sont requis lorsque le script de l'objet sélectionné est exécuté. La valeur par défaut est FALSE.  
  
**Noms d'objet de qualification de schéma**  
Qualifie les noms d'objets avec le schéma de l'objet. La valeur par défaut est FALSE. Pour plus d’informations, consultez [Créer un schéma de base de données](../../relational-databases/security/authentication-access/create-a-database-schema.md).  

**Options de compression des données de script** Inclut les options de compression de données dans le script. La valeur par défaut est FALSE.

**Générer un script pour les propriétés étendues**  
Inclut les propriétés étendues dans le script, si l'objet en possède. La valeur par défaut est FALSE. Pour plus d’informations, consultez [sp_addextendedproperty (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-addextendedproperty-transact-sql.md).  
  
**Propriétaire de script**  
Inclut le propriétaire dans le script généré. La valeur par défaut est FALSE.  
  
**Générer un script pour les autorisations**  
Inclut les autorisations sur les objets de base de données dans le script. La valeur par défaut est True. Pour plus d’informations, consultez [Autorisations](../../relational-databases/security/permissions-database-engine.md).  
  
### <a name="tableview-options"></a>Options de table/vue  
Les options suivantes s'appliquent uniquement aux scripts des tables et des vues.  
  
**Convertir les types de données définis par l'utilisateur en types de base**  
Convertit les types de données définis par l'utilisateur en types de base à partir desquels ils ont été créés. Utilisez True lorsque les types de données définis par l'utilisateur de la base de données source n'existent pas dans la base de données où le script sera exécuté. Utilisez False pour conserver les types de données définis par l'utilisateur. La valeur par défaut est FALSE. Pour plus d’informations, consultez [CREATE TYPE (Transact-SQL)](../../t-sql/statements/create-type-transact-sql.md).  
  
**Générer des commandes SET ANSI PADDING**  
Ajoute l'instruction SET ANSI_PADDING avant et après chaque instruction CREATE TABLE. La valeur par défaut est True. Pour plus d’informations, consultez [SET ANSI_PADDING (Transact-SQL)](../../t-sql/statements/set-ansi-padding-transact-sql.md).  
  
**Inclure un classement**  
Inclut un classement dans la définition de colonne. La valeur par défaut est True. Pour plus d’informations, consultez [Prise en charge d’Unicode et du classement](../../relational-databases/collations/collation-and-unicode-support.md).  
  
**Inclure la propriété IDENTITY**  
Inclut des définitions pour la valeur de départ IDENTITY et l'incrément IDENTITY. La valeur par défaut est True. Pour plus d’informations, consultez [IDENTITY (propriété) (Transact-SQL)](../../t-sql/statements/create-table-transact-sql-identity-property.md).  
  
**Références aux clés étrangères de qualification de schéma**  
Ajoute le nom de schéma aux références de table des contraintes FOREIGN KEY. La valeur par défaut est True.  
  
**Valeurs par défaut et règles liées aux scripts**  
Inclut les appels aux procédures stockées liées **sp_bindefault** et **sp_bindrule** . La valeur par défaut est True. Pour plus d’informations, consultez [sp_bindefault (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-bindefault-transact-sql.md) et [sp_bindrule (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-bindrule-transact-sql.md).  
  
**Générer un script pour les contraintes CHECK**  
Ajoute des [contraintes CHECK](../../relational-databases/tables/unique-constraints-and-check-constraints.md) au script. La valeur par défaut est True.  
  
**Valeurs de script par défaut**  
Inclut les valeurs de colonne par défaut dans le script. La valeur par défaut est FALSE. Pour plus d’informations, consultez [CREATE DEFAULT (Transact-SQL)](../../t-sql/statements/create-default-transact-sql.md).  
  
**Générer un script pour les groupes de fichiers**  
Spécifie le groupe de fichiers dans la clause ON pour des définitions de table. La valeur par défaut est FALSE. Pour plus d’informations, consultez [CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md).  
  
**Générer un script pour les clés étrangères**  
Inclut des [contraintes FOREIGN KEY](../../relational-databases/tables/primary-and-foreign-key-constraints.md) dans le script. La valeur par défaut est FALSE.  
  
**Générer un script pour les index de recherche en texte intégral**  
Inclut les index de recherche en texte intégral dans le script. La valeur par défaut est FALSE. Pour plus d’informations, consultez [CREATE FULLTEXT INDEX (Transact-SQL)](../../t-sql/statements/create-fulltext-index-transact-sql.md).  
  
**Générer un script pour les index**  
Inclut des index cluster, non cluster et XML dans le script. La valeur par défaut est True. Pour plus d’informations, consultez [CREATE INDEX (Transact-SQL)](../../t-sql/statements/create-index-transact-sql.md).  
  
**Générer un script pour les schémas de partition**  
Inclut des schémas de partition de table dans le script. La valeur par défaut est FALSE. Pour plus d’informations, consultez [CREATE PARTITION SCHEME (Transact-SQL)](../../t-sql/statements/create-partition-scheme-transact-sql.md).  
  
**Générer un script pour les clés primaires**  
Inclut des [contraintes de clé primaire et de clé étrangère](../../relational-databases/tables/primary-and-foreign-key-constraints.md) dans le script. La valeur par défaut est True.  
  
**Générer un script pour les statistiques**  
Inclut des statistiques définies par l'utilisateur dans le script. La valeur par défaut est FALSE. Pour plus d’informations, consultez [CREATE STATISTICS (Transact-SQL)](../../t-sql/statements/create-statistics-transact-sql.md).  
  
**Générer un script pour les déclencheurs**  
Inclut des déclencheurs dans le script. La valeur par défaut est FALSE. Pour plus d’informations, consultez [CREATE TRIGGER (Transact-SQL)](../../t-sql/statements/create-trigger-transact-sql.md).  
  
**Générer un script pour les clés uniques**  
Inclut des [contraintes uniques et des contraintes de validation](../../relational-databases/tables/unique-constraints-and-check-constraints.md) dans le script. La valeur par défaut est FALSE.  
  
**Générer un script pour les colonnes de vue**  
Déclare des colonnes de vue dans les en-têtes de vue. La valeur par défaut est FALSE. Pour plus d’informations, consultez [CREATE VIEW (Transact-SQL)](../../t-sql/statements/create-view-transact-sql.md).  
  
**Inclure les noms de système DRI**  
Inclut les noms de contraintes générés par le système pour appliquer l'intégrité référentielle déclarative. La valeur par défaut est FALSE. Pour plus d’informations, consultez [REFERENTIAL_CONSTRAINTS (Transact-SQL)](../../relational-databases/system-information-schema-views/referential-constraints-transact-sql.md).  
  
### <a name="version-options"></a>Options de version

**Faire correspondre les paramètres de script à la source** Si cette option est activée, l’édition et le type de moteur des scripts générés sont définies sur les valeurs du serveur pour lequel l’objet fait l’objet d’un script. Cette option désactive (et ignore) les autres options de version. 

**Script pour l’édition du moteur de base de données** Les scripts générés sont ciblés sur l’[édition de moteur](/dotnet/api/microsoft.sqlserver.management.smo.edition) spécifiée.

**Script pour le type du moteur de base de données** Les scripts générés sont ciblés sur le [type de moteur de base de données](/previous-versions/sql/sql-server-2014/ee642509(v=sql.120)) spécifié.

**Générer un script pour la version du serveur**  
Les scripts générés sont ciblés sur la version spécifiée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Les fonctionnalités qui sont des nouveautés de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] ne peuvent pas faire l'objet d'un script pour les versions antérieures. Certains scripts qui sont créés pour [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] ne peuvent pas être exécutés sur les serveurs exécutant une version antérieure de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]ou sur une base de données qui possède un [paramètre de niveau de compatibilité de base de données](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md)antérieur.  

## <a name="see-also"></a>Voir aussi  
[Générer des scripts (SQL Server Management Studio)](../scripting/generate-scripts-sql-server-management-studio.md)  
