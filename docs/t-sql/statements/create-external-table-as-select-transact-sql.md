---
description: CREATE EXTERNAL TABLE AS SELECT (Transact-SQL)
title: CREATE EXTERNAL TABLE AS SELECT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/10/2017
ms.prod_service: sql-data-warehouse, pdw
ms.reviewer: ''
ms.service: sql-data-warehouse
ms.subservice: design
ms.topic: conceptual
f1_keywords:
- CREATE EXTERNAL TABLE AS SELECT
- CREATE_EXTERNAL_TABLE_AS_SELECT
- CREATE EXTERNAL TABLE AS SELECT_TSQL
helpviewer_keywords:
- External, table
- PolyBase, external table
- External, table create as select
- PolyBase, create table as select
ms.assetid: 32dfe254-6df7-4437-bfd6-ca7d37557b0a
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: 14a7dce61b50ced328c103ec88973e9bf7b18c7d
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/13/2021
ms.locfileid: "100345239"
---
# <a name="create-external-table-as-select-transact-sql"></a>CREATE EXTERNAL TABLE AS SELECT (Transact-SQL)
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  Crée une table externe, puis exporte en parallèle les résultats d’une instruction SELECT [!INCLUDE[tsql](../../includes/tsql-md.md)] vers Hadoop ou le stockage blob Azure.

 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL &#40;Transact-SQL&#41;](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>Syntaxe

```syntaxsql 
CREATE EXTERNAL TABLE [ [database_name  . [ schema_name ] . ] | schema_name . ] table_name   
    WITH (   
        LOCATION = 'hdfs_folder',  
        DATA_SOURCE = external_data_source_name,  
        FILE_FORMAT = external_file_format_name  
        [ , <reject_options> [ ,...n ] ]  
    )  
    AS <select_statement>  
[;]  
  
<reject_options> ::=  
{  
    | REJECT_TYPE = value | percentage  
    | REJECT_VALUE = reject_value  
    | REJECT_SAMPLE_VALUE = reject_sample_value  
}  
  
<select_statement> ::=  
    [ WITH <common_table_expression> [ ,...n ] ]  
    SELECT <select_criteria>  
```

## <a name="arguments"></a>Arguments
 **[ [ *database_name* . [ *schema_name* ] . ] | *schema_name* . ] *table_name*** est le nom en une à trois parties du tableau à créer dans la base de données. Pour une table externe, seules les métadonnées de la table sont stockées dans la base de données relationnelle. 

 **LOCATION =  '*hdfs_folder*'** spécifie l’emplacement dans lequel écrire les résultats de l’instruction SELECT exécutée sur la source de données externes. L’emplacement correspond à un nom de dossier et peut inclure un chemin relatif au dossier racine du cluster Hadoop ou du stockage blob Azure. PolyBase va créer le chemin et le dossier s’ils n’existent pas déjà.

Les fichiers externes sont écrits dans *hdfs_folder* et sont nommés *QueryID_date_time_ID.format*, où *ID* est un identificateur incrémentiel et *format* est le format des données exportées. Par exemple, QID776_20160130_182739_0. orc.

 **DATA_SOURCE = *external_data_source_name*** spécifie le nom de l’objet de source de données externe contenant l’emplacement dans lequel les données externes sont stockées ou vont être stockées. L’emplacement est soit un cluster Hadoop Cluster, soit un stockage blob Azure. Pour créer une source de données externe, utilisez [CREATE EXTERNAL DATA SOURCE &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-data-source-transact-sql.md).

 **FILE_FORMAT = *external_file_format_name*** spécifie le nom de l’objet de format de fichier externe qui contient le format du fichier de données externe. Pour créer un format de fichier externe, utilisez [CREATE EXTERNAL FILE FORMAT &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-file-format-transact-sql.md).

 **Les options REJECT** ne s’appliquent pas au moment où l’instruction CREATE EXTERNAL TABLE AS SELECT est exécutée. Ici, les options sont spécifiées pour que la base de données puisse les utiliser ultérieurement, lors de l’importation des données à partir de la table externe. Plus tard, quand l’instruction CREATE TABLE AS SELECT sélectionnera des données dans la table externe, la base de données utilisera les options REJECT pour déterminer le nombre ou le pourcentage de lignes dont l’importation peut échouer avant que l’importation ne soit arrêtée.

   - **REJECT_VALUE = *reject_value*** spécifie la valeur ou le pourcentage de lignes dont l’importation peut échouer avant que l’importation ne soit arrêtée.

   - **REJECT_TYPE = **value** | percentage** précise si l’option REJECT_VALUE est spécifiée comme une valeur littérale ou un pourcentage.

      - La **valeur** est utilisée si REJECT_VALUE est une valeur littérale et non un pourcentage. La base de données cesse d’importer des lignes à partir du fichier de données externe lorsque le nombre de lignes ayant échoué dépasse la valeur de *reject_value*.

        Par exemple, si REJECT_VALUE = 5 et REJECT_TYPE = value, la base de données cesse d’importer des lignes après l’échec d’importation de cinq lignes.

      - Le **pourcentage** est utilisé si REJECT_VALUE est un pourcentage et non une valeur littérale. La base de données cesse d’importer des lignes à partir du fichier de données externe lorsque le pourcentage (*percentage*) de lignes ayant échoué dépasse la valeur de *reject_value*. Le pourcentage de lignes ayant échoué est calculé à intervalles.

   - **REJECT_SAMPLE_VALUE = *reject_sample_value*** est nécessaire lorsque REJECT_TYPE = percentage. Cela spécifie le nombre de lignes qu’il faut tenter d’importer avant que la base de données ne recalcule le pourcentage de lignes ayant échoué.

      Par exemple, si REJECT_SAMPLE_VALUE = 1000, la base de données calcule le pourcentage de lignes ayant échoué après avoir tenté d’importer 1 000 lignes à partir du fichier de données externe. Si le pourcentage de lignes ayant échoué est inférieur à la valeur de *reject_value*, la base de données tente de charger 1 000 autres lignes. La base de données continue de recalculer le pourcentage de lignes ayant échoué après avoir tenté d’importer chacune des 1 000 lignes supplémentaires.

     > [!NOTE]
     >  Comme la base de données calcule le pourcentage de lignes ayant échoué à intervalles, le pourcentage de lignes ayant échoué peut dépasser la valeur de *reject_value*.

     **Exemple :**

     Cet exemple montre comment les trois options REJECT interagissent les unes avec les autres. Par exemple, si REJECT_TYPE = percentage, REJECT_VALUE = 30 et REJECT_SAMPLE_VALUE = 100, le scénario suivant peut se produire :

     - La base de données tente de charger les 100 premières lignes, dont 25 ne sont pas importées et 75 sont importées.
     - Le pourcentage de lignes ayant échoué qui est obtenu est de 25 %, ce qui est inférieur à la valeur de rejet de 30 %. Par conséquent,il n’est pas nécessaire de stopper le chargement.
     - La base de données tente de charger les 100 lignes suivantes. Cette fois-ci, 25 sont importées et 75 ne sont pas importées.
     - Le pourcentage de lignes ayant échoué est recalculé et on obtient 50 %. Le pourcentage de lignes ayant échoué a donc dépassé la valeur de rejet de 30 %.
     - Le chargement échoue après la tentative d’importation de 200 lignes et l’échec de 50 % d’entre elles, ce qui est supérieur à la limite de 30 % spécifiée.

 **WITH *common_table_expression*** spécifie un jeu de résultats nommé temporaire, désigné par le terme d'expression de table commune (CTE, Common Table Expression). Pour plus d’informations, consultez [WITH common_table_expression &#40;Transact-SQL&#41;](../../t-sql/queries/with-common-table-expression-transact-sql.md) 

 **SELECT\<select_criteria>** remplit la nouvelle table avec les résultats d’une instruction SELECT. *select_criteria* correspond au corps de l’instruction SELECT qui détermine les données qui sont copiées dans la nouvelle table. Pour plus d’informations sur les instructions SELECT, consultez [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md).

## <a name="permissions"></a>Autorisations

 Pour exécuter cette commande, les *utilisateurs de la base de données* ont besoin des autorisations ou appartenances suivantes :

- Autorisation **ALTER SCHEMA** pour le schéma local devant contenir la nouvelle table ou appartenance au rôle de base de données fixe **db_ddladmin**.
- Autorisation **CREATE TABLE** ou appartenance au rôle de base de données fixe **db_ddladmin**
- Autorisation **SELECT** pour les objets référencés dans *select_criteria*.

 La connexion a besoin de toutes ces autorisations :

- **ADMINISTER BULK OPERATIONS**
- **ALTER ANY EXTERNAL DATA SOURCE**
- **ALTER ANY EXTERNAL FILE FORMAT**
- Autorisation d’**écriture** pour lire et écrire dans le dossier externe du cluster Hadoop ou du stockage Blob Azure.

 > [!IMPORTANT]
 >  L’autorisation ALTER ANY EXTERNAL DATA SOURCE accorde à n’importe quel principal la possibilité de créer et de modifier tout objet de source de données externe. Par conséquent, elle permet également d’accéder à toutes les informations d’identification délimitées à la base de données. Cette autorisation doit être considérée comme fournissant des privilèges très élevés et doit être accordée uniquement aux principaux de confiance du système.

## <a name="error-handling"></a>Gestion des erreurs
 Lorsque CREATE EXTERNAL TABLE AS SELECT exporte des données vers un fichier texte délimité, aucun fichier de rejet n’est disponible pour les lignes dont l’exportation échoue.

 Lorsque vous créez la table externe, la base de données tente de se connecter au cluster Hadoop externe ou au stockage blob Azure. Si la connexion échoue, la commande échoue et la table externe n’est pas créée. L’échec de la commande peut prendre plusieurs minutes, car la base de données tente de se connecter au moins trois fois.

 Si CREATE EXTERNAL TABLE AS SELECT est annulé ou échoue, la base de données effectue une tentative de suppression des nouveaux fichiers et des dossiers déjà créés dans la source de données externe.

 La base de données signale les erreurs Java qui peuvent se produire dans la source de données externe lors de l’exportation des données.

##  <a name="general-remarks"></a><a name="GeneralRemarks"></a> Remarques générales
 Une fois l’instruction CREATE EXTERNAL TABLE AS SELECT terminée, vous pouvez exécuter des requêtes [!INCLUDE[tsql](../../includes/tsql-md.md)] sur la table externe. Ces opérations vont importer des données dans la base de données pendant la durée de la requête, sauf si vous importez les données à l’aide de l’instruction CREATE TABLE AS SELECT.

 Le nom et la définition de la table externe sont stockés dans les métadonnées de la base de données. Les données sont stockées dans la source de données externe.

 Les fichiers externes sont nommés *QueryID_date_time_ID.format*, où *ID* est un identificateur incrémentiel et *format* est le format des données exportées. Par exemple, QID776_20160130_182739_0. orc.

 L’instruction CREATE EXTERNAL TABLE AS SELECT crée toujours une table non partitionnée, même si la table source est partitionnée.

 Pour les plans de requête, créés avec EXPLAIN, la base de données utilise les opérations de plan de requête suivantes pour les tables externes :

- Déplacement aléatoire externe
- Déplacement de diffusion externe
- Déplacement de partition externe

 **S’applique à :** Parallel Data Warehouse

L’un des prérequis à la création d’une table externe est que la connectivité Hadoop doit être configurée par l’administrateur de l’appliance. Pour plus d’informations, consultez « Configurer la connectivité aux données externes (Analytics Platform System) » dans la documentation Analytics Platform System, que vous pouvez télécharger dans le [centre de téléchargement Microsoft](https://www.microsoft.com/download/details.aspx?id=48241).

## <a name="limitations-and-restrictions"></a>Limitations et restrictions
 Les données de la table externe sont situées en dehors de la base de données. C’est pourquoi les opérations de sauvegarde et de restauration ne fonctionnent que sur les données stockées dans la base de données. De ce fait, seules les métadonnées sont sauvegardées et restaurées.

 La base de données ne vérifie pas la connexion à la source de données externe lorsque vous restaurez une sauvegarde de base de données contenant une table externe. Si la source d’origine n’est pas accessible, la restauration de métadonnées de la table externe sera quand même effectuée, mais les opérations SELECT exécutées sur la table externe échoueront.

 La base de données ne garantit pas la cohérence des données entre la base de données et les données externes. Vous seul êtes responsable de maintenir la cohérence entre les données externes et la base de données.

 Les opérations DML ne sont pas prises en charge avec les tables externes. Par exemple, vous ne pouvez pas utiliser les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] update, insert ou delete [!INCLUDE[tsql](../../includes/tsql-md.md)] pour modifier les données externes.

 CREATE TABLE, DROP TABLE, CREATE STATISTICS, DROP STATISTICS, CREATE VIEW et DROP VIEW sont les seules opérations DDL qui sont autorisées avec les tables externes.

 Les tables externes pour le pool SQL serverless ne peuvent pas être créées à un emplacement où vous avez actuellement des données. Pour réutiliser un emplacement qui a été utilisé pour stocker des données, l’emplacement doit être supprimé manuellement sur ADLS.

 PolyBase peut consommer un maximum de 33 000 fichiers par dossier lors de l’exécution simultanée de 32 requêtes PolyBase. Ce nombre maximal inclut les fichiers et les sous-dossiers de chaque dossier HDFS. Si le degré de concurrence est inférieur à 32, un utilisateur peut exécuter des requêtes PolyBase sur des dossiers dans des systèmes HDFS contenant plus de 33 000 fichiers. Nous recommandons aux utilisateurs de Hadoop et PolyBase de raccourcir au maximum les chemins de fichiers et de ne pas utiliser plus de 30 000 fichiers par dossier HDFS. Lorsque trop de fichiers sont référencés, une exception d’insuffisance de mémoire JVM est levée.

 [SET ROWCOUNT &#40;Transact-SQL&#41;](../../t-sql/statements/set-rowcount-transact-sql.md) n’a aucun effet sur CREATE EXTERNAL TABLE AS SELECT. Pour obtenir un comportement similaire, utilisez [TOP &#40;Transact-SQL&#41;](../../t-sql/queries/top-transact-sql.md).

 Lorsque CREATE EXTERNAL TABLE AS SELECT sélectionne des données à partir d’un RCFile, les valeurs de colonne du RCFile ne doivent pas contenir le caractère « | ».

La définition de CREATE EXTERNAL TABLE AS SELECT sur des fichiers Parquet ou ORC entraînera des erreurs pouvant inclure des enregistrements rejetés quand les caractères suivants sont présents dans les données :

- |
- “ (guillemet)
- /r/n
- /r
- /n

Pour utiliser CREATE EXTERNAL TABLE AS SELECT avec ces caractères, vous devez d’abord exécuter l’instruction CREATE EXTERNAL TABLE AS SELECT pour exporter les données vers des fichiers texte délimités, avant de les convertir en fichiers Parquet ou ORC à l’aide d’un outil externe.

## <a name="locking"></a>Verrouillage
 Applique un verrou partagé sur l’objet SCHEMARESOLUTION.

##  <a name="examples"></a><a name="Examples"></a> Exemples

### <a name="a-create-a-hadoop-table-by-using-create-external-table-as-select"></a>R. Créer une table Hadoop à l’aide de CREATE EXTERNAL TABLE AS SELECT

 L’exemple suivant crée une nouvelle table externe nommée `hdfsCustomer` qui utilise les définitions de colonne et les données de la table source `dimCustomer`.

 La définition de table est stockée dans la base de données, et les résultats de l’instruction SELECT sont exportés vers le fichier « /pdwdata/customer.tbl » dans la source de données externe Hadoop *customer_ds*. Le fichier est mis en forme selon le format de fichier externe *customer_ff*.

 Le nom de fichier est généré par la base de données et contient l’ID de requête pour faciliter l’alignement du fichier sur la requête qui l’a généré.

 Le chemin `hdfs://xxx.xxx.xxx.xxx:5000/files/` qui précède le répertoire Client doit déjà exister. Si le répertoire Client n’existe pas, la base de données crée le répertoire.

> [!NOTE]
>  Cet exemple spécifie 5000. Si le port n’est pas spécifié, la base de données utilise le port 8020 comme port par défaut.

 L’emplacement et le nom de fichier Hadoop résultants seront : `hdfs:// xxx.xxx.xxx.xxx:5000/files/Customer/ QueryID_YearMonthDay_HourMinutesSeconds_FileIndex.txt.`.

```sql  
-- Example is based on AdventureWorks   
CREATE EXTERNAL TABLE hdfsCustomer  
WITH (  
        LOCATION='/pdwdata/customer.tbl',  
        DATA_SOURCE = customer_ds,  
        FILE_FORMAT = customer_ff  
) AS SELECT * FROM dimCustomer;  
```

### <a name="b-use-a-query-hint-with-create-external-table-as-select"></a>B. Utiliser un indicateur de requête avec CREATE EXTERNAL TABLE AS SELECT

 Cette requête présente la syntaxe de base pour utiliser un indicateur de jointure de requête avec l’instruction CREATE EXTERNAL TABLE AS SELECT. Une fois la requête envoyée, la base de données utilise la stratégie de jointure hachée pour générer le plan de requête. Pour plus d’informations sur les indicateurs de jointure et sur l’utilisation de la clause OPTION, consultez [Clause OPTION &#40;Transact-SQL&#41;](../../t-sql/queries/option-clause-transact-sql.md).

> [!NOTE]
>  Cet exemple spécifie 5000. Si le port n’est pas spécifié, la base de données utilise le port 8020 comme port par défaut.

```sql  
-- Example is based on AdventureWorks  
CREATE EXTERNAL TABLE dbo.FactInternetSalesNew  
WITH   
    (   
        LOCATION = '/files/Customer',  
        DATA_SOURCE = customer_ds,  
        FILE_FORMAT = customer_ff  
    )  
AS SELECT T1.* FROM dbo.FactInternetSales T1 JOIN dbo.DimCustomer T2  
ON ( T1.CustomerKey = T2.CustomerKey )  
OPTION ( HASH JOIN );  
```

## <a name="see-also"></a>Voir aussi
 - [CREATE EXTERNAL DATA SOURCE &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-data-source-transact-sql.md)
 - [CREATE EXTERNAL FILE FORMAT &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-file-format-transact-sql.md)
 - [CREATE EXTERNAL TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-table-transact-sql.md)
 - [CREATE TABLE &#40;Azure Synapse Analytics, Parallel Data Warehouse&#41;](~/t-sql/statements/create-table-azure-sql-data-warehouse.md)
 - [CREATE TABLE AS SELECT &#40;Azure Synapse Analytics&#41;](../../t-sql/statements/create-table-as-select-azure-sql-data-warehouse.md)
 - [DROP TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-table-transact-sql.md)
 - [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)



