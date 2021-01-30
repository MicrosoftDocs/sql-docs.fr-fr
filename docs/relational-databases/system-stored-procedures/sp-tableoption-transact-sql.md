---
description: sp_tableoption (Transact-SQL)
title: sp_tableoption (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/11/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_tableoption_TSQL
- sp_tableoption
dev_langs:
- TSQL
helpviewer_keywords:
- sp_tableoption
ms.assetid: 0a57462c-1057-4c7d-bce3-852cc898341d
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: d2846b289c31777adec6d548addba11a9c7e463d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204339"
---
# <a name="sp_tableoption-transact-sql"></a>sp_tableoption (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Définit les valeurs d'option des tables définies par l'utilisateur. sp_tableoption peut être utilisé pour contrôler le comportement dans la ligne des tables avec des colonnes de type **varchar (max)**, **nvarchar (max)**, **varbinary (max)**, **XML**, **Text**, **ntext**, **image** ou de type large défini par l’utilisateur.  
  
> [!IMPORTANT]  
>  La fonctionnalité text in row sera supprimée dans une future version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour stocker des données de valeur élevée, nous vous recommandons d’utiliser les types de données **varchar (max)**, **nvarchar (max)** et **varbinary (max)** .  
  

 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_tableoption [ @TableNamePattern = ] 'table'   
     , [ @OptionName = ] 'option_name'   
     ,[ @OptionValue =] 'value'  
```  
  
## <a name="arguments"></a>Arguments  
 [ @TableNamePattern =] '*table*'  
 Spécifie le nom qualifié ou non d'une table de base de données définie par l'utilisateur. Si un nom de table complet (incluant un nom de base de données) est fourni, le nom de base de données doit être celui de la base de données en cours. Vous ne pouvez pas définir simultanément les options des tables pour plusieurs tables. *table* est de type **nvarchar (776)**, sans valeur par défaut.  
  
 [ @OptionName =] '*option_name*'  
 Spécifie un nom d'option de table. *option_name* est de type **varchar (35)**, sans valeur par défaut null. *option_name* peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|table lock on bulk load|Désactivée (valeur par défaut), oblige le processus de chargement en masse effectué sur les tables définies par l'utilisateur à obtenir des verrous de lignes. Activée, oblige le processus de chargement en masse effectué sur les tables définies par l'utilisateur à obtenir un verrou de mise à jour en bloc.|  
|insert row lock|N'est plus pris en charge.<br /><br /> Cette option n'a aucun effet sur le comportement de verrouillage de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et elle n'est incluse qu'à des fins de compatibilité des scripts et des procédures existants.|  
|text in row|Si la valeur est OFF ou 0 (désactivé, valeur par défaut), le comportement en cours n'est pas modifié, et la ligne ne contient pas d'objet BLOB.<br /><br /> Quand elle est spécifiée et que @OptionValue est activé (Enabled) ou une valeur entière comprise entre 24 et 7000, les nouvelles chaînes **Text**, **ntext** ou **image** sont stockées directement dans la ligne de données. Toutes les données d’objet BLOB (Binary Large Object : **Text**, **ntext** ou **image** ) existantes seront remplacées par le format Text in Row lorsque la valeur de l’objet blob est mise à jour. Pour plus d'informations, consultez la section Notes.|  
|large value types out of row|1 = **varchar (max)**, **nvarchar (max)**, **varbinary (max)**, **XML** et les colonnes de type défini par l’utilisateur (UDT) volumineuses de la table sont stockées hors ligne, avec un pointeur de 16 octets vers la racine.<br /><br /> 0 = **varchar (max)**, **nvarchar (max)**, **varbinary (max)**, **XML** et les valeurs UDT volumineuses sont stockées directement dans la ligne de données, jusqu’à une limite de 8000 octets et tant que la valeur peut être contenue dans l’enregistrement. Si la valeur ne tient pas dans l'enregistrement, un pointeur est stocké dans la ligne et le reste est stocké hors de la ligne dans l'espace de stockage LOB. La valeur par défaut est 0.<br /><br /> Le type défini par l’utilisateur (UDT) volumineux s’applique à : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures. <br /><br /> Utilisez l’option TEXTIMAGE_ON de [Create table](../../t-sql/statements/create-table-transact-sql.md) pour spécifier un emplacement pour le stockage des types de données volumineux. |  
|format de stockage vardecimal|**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.<br /><br /> Lorsque la valeur est TRUE, ON ou 1, la table désignée est activée pour le format de stockage vardecimal. Lorsque la valeur est FALSE, OFF ou 0, la table n'est pas activée pour le format de stockage vardecimal. Le format de stockage vardecimal ne peut être activé que si la base de données a été activée pour le format de stockage vardecimal à l’aide de [sp_db_vardecimal_storage_format](../../relational-databases/system-stored-procedures/sp-db-vardecimal-storage-format-transact-sql.md). Dans [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures, le format de stockage **vardecimal** est déconseillé. Utilisez plutôt la compression ROW. Pour plus d’informations, consultez [Compression de données](../../relational-databases/data-compression/data-compression.md). La valeur par défaut est 0.|  
  
 [ @OptionValue =] '*valeur*'  
 Indique si le *option_name* est activé (true, on ou 1) ou désactivé (false, OFF ou 0). la *valeur* est de type **varchar (12)**, sans valeur par défaut. la *valeur* ne respecte pas la casse.  
  
 Pour l'option text in row, les valeurs d'option valides sont 0, ON, OFF ou un entier compris entre 24 et 7 000. Lorsque la *valeur* est on, la limite par défaut est de 256 octets.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (succès) ou numéro d'erreur (échec)  
  
## <a name="remarks"></a>Notes  
 La procédure sp_tableoption peut être utilisée uniquement pour définir les valeurs des options des tables définies par l'utilisateur. Pour afficher les propriétés d’une table, utilisez OBJECTPROPERTY ou Query sys. tables.  
  
 L'option text in row de sp_tableoption peut être activée ou désactivée uniquement pour les tables qui contiennent des colonnes de texte. Si la table ne contient pas de colonne de texte, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] génère une erreur.  
  
 Lorsque l’option text in Row est activée, le @OptionValue paramètre permet aux utilisateurs de spécifier la taille maximale à stocker dans une ligne pour un objet BLOB. La valeur par défaut est de 256 octets, mais les valeurs peuvent être comprises entre 24 et 7 000 octets.  
  
 les chaînes **Text**, **ntext** ou **image** sont stockées dans la ligne de données si les conditions suivantes s’appliquent :  
  
-   l'option text in row est activée ;  
  
-   La longueur de la chaîne est plus petite que la limite spécifiée dans @OptionValue  
  
-   l'espace disque disponible s'avère suffisant dans la ligne de données.  
  
 Lorsque les chaînes d’objets BLOB sont stockées dans la ligne de données, la lecture et l’écriture des chaînes **Text**, **ntext** ou **image** peuvent être aussi rapides que la lecture ou l’écriture de chaînes de caractères et binaires. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n'a pas besoin d'accéder à des pages séparées pour lire ou écrire la chaîne BLOB.  
  
 Si une chaîne **Text**, **ntext** ou **image** est supérieure à la limite spécifiée ou à l’espace disponible dans la ligne, les pointeurs sont stockés à la place dans la ligne. Les conditions concernant le stockage des chaînes BLOB dans la ligne sont toujours applicables : la ligne de données doit disposer d'un espace suffisant pour contenir les pointeurs.  
  
 Les chaînes d'objets BLOB et les pointeurs stockés dans la ligne d'une table sont considérés comme des chaînes de longueur variable. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n'utilise que le nombre d'octets nécessaires au stockage de la chaîne ou du pointeur.  
  
 Les chaînes d'objets BLOB existantes ne sont pas converties immédiatement lorsque l'option text in row est activée pour la première fois. Ces chaînes ne sont converties que lors de leur mise à jour. De même, lorsque la limite de l’option text in Row est augmentée, les chaînes **Text**, **ntext** ou **image** déjà présentes dans la ligne de données ne sont pas converties pour respecter la nouvelle limite jusqu’au moment où elles sont mises à jour.  
  
> [!NOTE]  
>  La désactivation de l'option text in row ou la réduction de sa limite nécessite la conversion de tous les objets BLOB, ce qui peut rallonger le processus, en fonction du nombre de chaînes d'objets BLOB à convertir. La table est verrouillée au cours du processus de conversion.  
  
 Une variable de table, comprenant une fonction chargée de retourner une variable de table, possède automatiquement l'option text in row activée avec une limite incluse par défaut de 256 octets. Cette option ne peut pas être modifiée.  
  
 L’option text in Row prend en charge les fonctions TEXTPTR, WRITETEXT, UPDATETEXT et READTEXT. Les utilisateurs peuvent lire des parties d'objet BLOB avec la fonction SUBSTRING(), mais sans oublier que les pointeurs de texte en ligne ont des durées et des limites différentes des autres pointeurs de texte.  
  
 Pour rétablir une table du format de stockage vardecimal au format de stockage décimal normal, la base de données doit être en mode de récupération SIMPLE. Le changement de mode de récupération va rompre la séquence de journaux de transactions consécutifs à des fins de sauvegarde. Par conséquent, vous devez créer une sauvegarde de base de données complète après avoir supprimé le format de stockage vardecimal d'une table.  
  
 Si vous convertissez une colonne existante de type de données LOB (Text, ntext ou image) en types de valeurs de petite à moyenne taille (varchar (max), nvarchar (max) ou varbinary (max)), et que la plupart des instructions ne font pas **référence aux colonnes** de type de valeur élevée de votre **large_value_types_out_of_row** environnement Lorsque la valeur de l’option **large_value_types_out_of_row** est modifiée, les valeurs varchar (max), nvarchar (max), varbinary (max) et XML existantes ne sont pas immédiatement converties. Le stockage des chaînes est modifié lorsqu'elles sont mises à jour. Les nouvelles valeurs insérées dans une table sont stockées en fonction de l'option de table active. Pour obtenir des résultats immédiats, effectuez une copie des données, puis remplissez à nouveau la table après avoir modifié le paramètre de **large_value_types_out_of_row** ou mettez à jour chaque colonne de types de valeur de petite à moyenne taille vers elle-même afin que le stockage des chaînes soit modifié avec l’option de table en vigueur. Vous pouvez également recréer les index sur la table après la mise à jour ou le nouveau remplissage afin de condenser la table. 
    
  
## <a name="permissions"></a>Autorisations  
 L'exécution de sp_tableoption nécessite une autorisation ALTER sur la table.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-storing-xml-data-out-of-the-row"></a>R. Stockage des données xml hors de la ligne  
 L’exemple suivant spécifie que les données **XML** de la `HumanResources.JobCandidate` table sont stockées hors de la ligne.  
  
```sql  
USE AdventureWorks2012;  
GO  
EXEC sp_tableoption 'HumanResources.JobCandidate', 'large value types out of row', 1;  
```  
  
### <a name="b-enabling-vardecimal-storage-format-on-a-table"></a>B. Activation du format de stockage vardecimal sur une table  
 L’exemple suivant modifie la `Production.WorkOrderRouting` table pour stocker le `decimal` type de données dans le `vardecimal` format de stockage.  

```sql  
USE master;  
GO  
-- The database must be enabled for vardecimal storage format  
-- before a table can be enabled for vardecimal storage format  
EXEC sp_db_vardecimal_storage_format 'AdventureWorks2012', 'ON';  
GO  
USE AdventureWorks2012;  
GO  
EXEC sp_tableoption 'Production.WorkOrderRouting',   
   'vardecimal storage format', 'ON';  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sys.tables &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-tables-transact-sql.md)   
 [OBJECTPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/objectproperty-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Moteur de base de données des procédures stockées &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)  
  
  
