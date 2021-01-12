---
description: CHANGETABLE (Transact-SQL)
title: CHANGETABLE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/08/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- CHANGETABLE_TSQL
- CHANGETABLE
dev_langs:
- TSQL
helpviewer_keywords:
- CHANGETABLE
- change tracking [SQL Server], CHANGETABLE
ms.assetid: d405fb8d-3b02-4327-8d45-f643df7f501a
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 6ffbd16a2a62b9adcc5ff4e2f31dcee9f133538a
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098724"
---
# <a name="changetable-transact-sql"></a>CHANGETABLE (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Retourne des informations de suivi des modifications pour une table. Vous pouvez utiliser cette instruction pour retourner toutes les modifications pour une table ou les informations de suivi des modifications pour une ligne spécifique.  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```sql
CHANGETABLE (  
    { CHANGES table , last_sync_version  
    | VERSION table , <primary_key_values> } )  
[AS] table_alias [ ( column_alias [ ,...n ] )  
  
<primary_key_values> ::=  
( column_name [ , ...n ] ) , ( value [ , ...n ] )  
```  
  
## <a name="arguments"></a>Arguments  
 *Table* des modifications, *last_sync_version*  
 Retourne des informations de suivi pour toutes les modifications apportées à une table depuis la version spécifiée par *last_sync_version*.  
  
 *table*  
 Table définie par l'utilisateur sur laquelle obtenir le suivi des modifications. Le suivi des modifications doit être activé sur la table. Un nom de table en une, deux, trois ou quatre parties peut être utilisé. Le nom de table peut être un synonyme de la table.  
  
 *last_sync_version*  
 Lorsqu'elle obtient des modifications, l'application appelante doit spécifier le point à partir duquel les modifications sont requises. L'argument last_sync_version spécifie ce point. La fonction retourne des informations pour toutes les lignes qui ont été modifiées depuis la version considérée. L'application exécute une requête pour recevoir les modifications avec une version supérieure à last_sync_version.  
  
 En général, avant d’obtenir des modifications, l’application appellera **CHANGE_TRACKING_CURRENT_VERSION ()** pour obtenir la version qui sera utilisée la prochaine fois que des modifications seront nécessaires. Par conséquent, l'application n'a pas besoin d'interpréter ou comprendre la valeur réelle.  
  
 Comme last_sync_version est obtenu par l'application appelante, celle-ci doit rendre la valeur persistante. Si l'application perd cette valeur, elle doit alors réinitialiser les données.  
  
 *last_sync_version* est de type **bigint**. La valeur doit être scalaire. Une expression entraîne une erreur de syntaxe.  
  
 Si la valeur est NULL, toutes les modifications suivies sont retournées.  
  
 les *last_sync_version* doivent être validées pour s’assurer qu’elles ne sont pas trop anciennes, car certaines ou toutes les informations de modification ont peut-être été nettoyées en fonction de la période de rétention configurée pour la base de données. Pour plus d’informations, consultez [CHANGE_TRACKING_MIN_VALID_VERSION &#40;Transact-sql&#41;](../../relational-databases/system-functions/change-tracking-min-valid-version-transact-sql.md) et [options ALTER DATABASE SET &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md).  
  
 *Table* de version, {<primary_key_values>}  
 Retourne les informations de suivi des modifications les plus récentes pour une ligne spécifiée. Les valeurs de clé primaire doivent identifier la ligne. <primary_key_values> identifie les colonnes de clé primaire et spécifie les valeurs. Les noms des colonnes clés primaires peuvent être spécifiés dans n'importe quel ordre.  
  
 *Table*  
 Table définie par l'utilisateur sur laquelle obtenir les informations de suivi des modifications. Le suivi des modifications doit être activé sur la table. Un nom de table en une, deux, trois ou quatre parties peut être utilisé. Le nom de table peut être un synonyme de la table.  
  
 *column_name*  
 Spécifie le nom de la colonne ou des colonnes clés primaires. Plusieurs noms de colonne peuvent être spécifiés dans un ordre quelconque.  
  
 *Valeur*  
 Valeur de la clé primaire. S’il existe plusieurs colonnes de clé primaire, les valeurs doivent être spécifiées dans le même ordre que celui où les colonnes apparaissent dans la liste *column_name* .  
  
 SERVIR *table_alias* [(*column_alias* [,... *n* ])]  
 Fournit des noms pour les résultats retournés par CHANGETABLE.  
  
 *table_alias*  
 Nom d'alias de la table retournée par CHANGETABLE. *table_alias* est obligatoire et doit être un [identificateur](../../relational-databases/databases/database-identifiers.md)valide.  
  
 *column_alias*  
 Alias de colonne facultatif ou liste d'alias de colonne pour les colonnes retournées par CHANGETABLE. Ce paramètre permet de personnaliser les noms de colonne au cas où il existerait des noms en double dans les résultats.  
  
## <a name="return-types"></a>Types de retour  
 **table**  
  
## <a name="return-values"></a>Valeurs de retour  
  
### <a name="changetable-changes"></a>CHANGETABLE CHANGES  
 Lorsque CHANGES est spécifié, zéro, une ou plusieurs lignes contenant les colonnes suivantes sont retournées.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|SYS_CHANGE_VERSION|**bigint**|Valeur de version associée à la dernière modification apportée à la ligne|  
|SYS_CHANGE_CREATION_VERSION|**bigint**|Valeurs de version associées à la dernière opération d'insertion.|  
|SYS_CHANGE_OPERATION|**nchar(1)**|Spécifie le type de modification :<br /><br /> **U** = mise à jour<br /><br /> **I** = insertion<br /><br /> **D** = supprimer|  
|SYS_CHANGE_COLUMNS|**varbinary(4100)**|Répertorie les colonnes qui ont été modifiées depuis la version last_sync_version (de référence). Notez que les colonnes calculées ne sont jamais répertoriées comme modifiées.<br /><br /> La valeur est NULL si l'une des conditions suivantes est remplie :<br /><br /> le suivi des modifications de colonne n'est pas activé ;<br /><br /> il s'agit d'une opération d'insertion ou de suppression ;<br /><br /> toutes les colonnes dépourvues de clés primaires ont été mises à jour en une seule opération. Cette valeur binaire ne doit pas être interprétée directement. Au lieu de cela, pour l’interpréter, utilisez [CHANGE_TRACKING_IS_COLUMN_IN_MASK ()](../../relational-databases/system-functions/change-tracking-is-column-in-mask-transact-sql.md).|  
|SYS_CHANGE_CONTEXT|**varbinary(128)**|Modifiez les informations de contexte que vous pouvez éventuellement spécifier à l’aide de la clause [with](../../relational-databases/system-functions/with-change-tracking-context-transact-sql.md) dans le cadre d’une instruction INSERT, Update ou DELETE.|  
|\<primary key column value>|Identique aux colonnes de table utilisateur|Valeurs de clés primaires pour la table faisant l'objet d'un suivi. Ces valeurs identifient de manière unique chaque ligne dans la table utilisateur.|  
  
### <a name="changetable-version"></a>CHANGETABLE VERSION  
 Lorsque VERSION est spécifié, une ligne contenant les colonnes suivantes est retournée.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|SYS_CHANGE_VERSION|**bigint**|Valeur de la version actuelle des modifications associée à la ligne.<br /><br /> La valeur est NULL si aucune modification n'a pas été apportée pendant un délai dépassant la période de rétention de suivi des modifications, ou si la ligne n'a pas été modifiée depuis l'activation du suivi des modifications.|  
|SYS_CHANGE_CONTEXT|**varbinary(128)**|Modifiez les informations de contexte que vous pouvez éventuellement spécifier à l'aide de la clause WITH dans le cadre d'une instruction INSERT, UPDATE ou DELETE.|  
|\<primary key column value>|Identique aux colonnes de table utilisateur|Valeurs de clés primaires pour la table faisant l'objet d'un suivi. Ces valeurs identifient de manière unique chaque ligne dans la table utilisateur.|  
  
## <a name="remarks"></a>Notes  
 La fonction CHANGETABLE est utilisée en général dans la clause FROM d'une requête comme s'il s'agissait d'une table.  
  
## <a name="changetablechanges"></a>CHANGETABLE(CHANGES...)  
 Pour obtenir des données de ligne pour des lignes nouvelles ou modifiées, joignez le jeu de résultats à la table utilisateur en utilisant les colonnes clés primaires. Une seule ligne est retournée pour chaque ligne de la table utilisateur qui a été modifiée, même si plusieurs modifications ont été apportées à la même ligne depuis la valeur de *last_sync_version* .  
  
 Les modifications de colonne clé primaire ne sont jamais marquées comme des mises à jour. La modification d'une valeur de clé primaire est considérée comme une suppression de la valeur ancienne et une insertion de la valeur nouvelle.  
  
 Si vous supprimez une ligne puis insérez une ligne dotée de l'ancienne clé primaire, la modification est considérée comme une mise à jour de toutes les colonnes de la ligne.  
  
 Les valeurs retournées pour les colonnes SYS_CHANGE_OPERATION et SYS_CHANGE_COLUMNS sont relatives à la ligne de base (last_sync_version) spécifiée. Par exemple, si une opération d’insertion a été effectuée à la version 10 et une opération de mise à jour à la version 15, et si le *last_sync_version* de la ligne de base est 12, une mise à jour est signalée. Si la valeur *last_sync_version* est 8, une insertion sera signalée. SYS_CHANGE_COLUMNS ne signalera jamais les colonnes calculées comme ayant été mises à jour.  
  
 En général, toutes les opérations d'insertion, de mise à jour ou de suppression de données dans les tables utilisateur font l'objet d'un suivi, y compris l'instruction MERGE.  
  
 Les opérations suivantes qui impliquent les données des tables utilisateur ne font pas l'objet d'un suivi :  
  
-   Exécution de l'instruction UPDATETEXT  
  
     Cette instruction est déconseillée et sera supprimée dans une prochaine version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Toutefois, les modifications apportées en utilisant la clause .WRITE de l'instruction UPDATE font l'objet d'un suivi.  
  
-   Suppression de lignes à l'aide de TRUNCATE TABLE  
  
     Lorsqu'une table est tronquée, les informations sur la version du suivi des modifications associées à la table sont réinitialisées comme si le suivi des modifications venait d'être activé sur la table. Une application cliente doit toujours valider sa dernière version synchronisée. La validation échoue si la table a été tronquée.  
  
## <a name="changetableversion"></a>CHANGETABLE(VERSION...)  
 Un jeu de résultats vide est retourné si une clé primaire inexistante est spécifiée.  
  
 La valeur de SYS_CHANGE_VERSION peut être NULL si aucune modification n'a pas été apportée pendant une période plus longue que la période de rétention (par exemple, le nettoyage a supprimé les informations de modification) ou si la ligne n'a jamais été modifiée depuis l'activation du suivi des modifications pour la table.  
  
## <a name="permissions"></a>Autorisations  
 Requiert les autorisations suivantes sur la table spécifiée par la valeur de la *table* pour obtenir les informations de suivi des modifications :  
  
-   Autorisation SELECT sur les colonnes clés primaires  
  
-   VIEW CHANGE TRACKING  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-returning-rows-for-an-initial-synchronization-of-data"></a>R. Retour de lignes pour une synchronisation initiale des données  
 L'exemple suivant montre comment obtenir des données pour une synchronisation initiale des données de table. La requête retourne toutes les données de ligne et leurs versions associées. Vous pouvez ensuite insérer ou ajouter ces données au système qui contiendra les données synchronisées.  
  
```sql  
-- Get all current rows with associated version  
SELECT e.[Emp ID], e.SSN, e.FirstName, e.LastName,  
    c.SYS_CHANGE_VERSION, c.SYS_CHANGE_CONTEXT  
FROM Employees AS e  
CROSS APPLY CHANGETABLE   
    (VERSION Employees, ([Emp ID], SSN), (e.[Emp ID], e.SSN)) AS c;  
```  
  
### <a name="b-listing-all-changes-that-were-made-since-a-specific-version"></a>B. Liste de toutes les modifications apportées depuis une version spécifique  
 L'exemple suivant répertorie toutes les modifications apportées à une table depuis la version spécifiée (`@last_sync_version)`. [Emp ID] et SSN sont des colonnes dans une clé primaire composite.  
  
```sql  
DECLARE @last_sync_version bigint;  
SET @last_sync_version = <value obtained from query>;  
SELECT [Emp ID], SSN,  
    SYS_CHANGE_VERSION, SYS_CHANGE_OPERATION,  
    SYS_CHANGE_COLUMNS, SYS_CHANGE_CONTEXT   
FROM CHANGETABLE (CHANGES Employees, @last_sync_version) AS C;  
```  
  
### <a name="c-obtaining-all-changed-data-for-a-synchronization"></a>C. Obtention de toutes les données modifiées pour une synchronisation  
 L'exemple suivant montre comment obtenir toutes les données modifiées. Cette requête joint les informations de suivi des modifications à la table utilisateur afin que les informations de la table utilisateur soient retournées. Un `LEFT OUTER JOIN` est utilisé afin qu'une ligne soit retournée pour les lignes supprimées.  
  
```sql  
-- Get all changes (inserts, updates, deletes)  
DECLARE @last_sync_version bigint;  
SET @last_sync_version = <value obtained from query>;  
SELECT e.FirstName, e.LastName, c.[Emp ID], c.SSN,  
    c.SYS_CHANGE_VERSION, c.SYS_CHANGE_OPERATION,  
    c.SYS_CHANGE_COLUMNS, c.SYS_CHANGE_CONTEXT   
FROM CHANGETABLE (CHANGES Employees, @last_sync_version) AS c  
    LEFT OUTER JOIN Employees AS e  
        ON e.[Emp ID] = c.[Emp ID] AND e.SSN = c.SSN;  
```  
  
### <a name="d-detecting-conflicts-by-using-changetableversion"></a>D. Détection des conflits à l'aide de CHANGETABLE(VERSION...)  
 L'exemple suivant montre comment mettre à jour une ligne uniquement si celle-ci n'a pas changé depuis la dernière synchronisation. Le numéro de version de la ligne spécifique est obtenu à l'aide de `CHANGETABLE`. Si la ligne a été mise à jour, aucune modification n'est appliquée et la requête retourne des informations sur la modification la plus récente apportée à la ligne.  
  
```sql  
-- @last_sync_version must be set to a valid value  
UPDATE  
    SalesLT.Product  
SET  
    ListPrice = @new_listprice  
FROM  
    SalesLT.Product AS P  
WHERE  
    ProductID = @product_id AND  
    @last_sync_version >= ISNULL (  
        (SELECT CT.SYS_CHANGE_VERSION FROM   
            CHANGETABLE(VERSION SalesLT.Product,  
            (ProductID), (P.ProductID)) AS CT),  
        0);  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions de suivi des modifications &#40;Transact-SQL&#41;](../../relational-databases/system-functions/change-tracking-functions-transact-sql.md)   
 [Suivi des modifications de données &#40;SQL Server&#41;](../../relational-databases/track-changes/track-data-changes-sql-server.md)   
 [CHANGE_TRACKING_IS_COLUMN_IN_MASK &#40;Transact-SQL&#41;](../../relational-databases/system-functions/change-tracking-is-column-in-mask-transact-sql.md)   
 [CHANGE_TRACKING_CURRENT_VERSION &#40;Transact-SQL&#41;](../../relational-databases/system-functions/change-tracking-current-version-transact-sql.md)   
 [CHANGE_TRACKING_MIN_VALID_VERSION &#40;Transact-SQL&#41;](../../relational-databases/system-functions/change-tracking-min-valid-version-transact-sql.md)  
  
  
