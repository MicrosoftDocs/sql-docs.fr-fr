---
description: DBCC CHECKIDENT (Transact-SQL)
title: DBCC CHECKIDENT (Transact-SQL)
ms.custom: ''
ms.date: 03/07/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CHECKIDENT
- DBCC CHECKIDENT
- CHECKIDENT_TSQL
- DBCC_CHECKIDENT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- checking identity values
- reseeding identity values
- resetting identity values
- identity values [SQL Server]
- identity values [SQL Server], checking
- modifying identity values
- current identity values
- DBCC CHECKIDENT statement
- identity values [SQL Server], reseeding
- reporting current identity values
ms.assetid: 2c00ee51-2062-4e47-8b19-d90f524c6427
author: pmasl
ms.author: umajay
monikerRange: = azuresqldb-current || >= sql-server-2016 || >= sql-server-linux-2017 || = azure-sqldw-latest
ms.openlocfilehash: bb5a426514c8ddd9607114f444119429deb58aeb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346061"
---
# <a name="dbcc-checkident-transact-sql"></a>DBCC CHECKIDENT (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  Vérifie la valeur d’identité actuelle pour la table spécifiée dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] et, si nécessaire, change cette valeur. Vous pouvez également utiliser DBCC CHECKIDENT pour définir manuellement une nouvelle valeur d'identité actuelle pour la colonne d'identité.  
  
 ![Icône Lien d’article](../../database-engine/configure-windows/media/topic-link.gif "Icône Lien d’article") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  

DBCC CHECKIDENT
 (
    table_name  
        [, { NORESEED | { RESEED [, new_reseed_value ] } } ]  
)  
[ WITH NO_INFOMSGS ]  
```  

```syntaxsql
-- Syntax for Azure Synapse Analytics
DBCC CHECKIDENT   
 (   
    table_name  
        [RESEED, new_reseed_value ]   
)  
[ WITH NO_INFOMSGS ]  
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments

 *table_name*  
 Nom de la table pour laquelle la valeur d'identité courante est vérifiée. La table spécifiée doit posséder une colonne d'identité. Les noms de tables doivent respecter les règles applicables aux [identificateurs](../../relational-databases/databases/database-identifiers.md). Les noms en deux ou trois parties doivent être délimités, comme 'Personne.TypeAdresse' ou [Personne.TypeAdresse].
  
 NORESEED  
 Spécifie que la valeur d’identité courante ne doit pas être changée.  
  
 RESEED  
 Spécifie que la valeur d'identité courante doit être modifiée.  
  
 *new_reseed_value*  
 Nouvelle valeur à utiliser comme valeur actuelle de la colonne d'identité.  
  
 WITH NO_INFOMSGS  
 Supprime tous les messages d'information.  
  
## <a name="remarks"></a>Notes

 Les corrections spécifiques effectuées sur la valeur d'identité courante dépendent des spécifications de paramètres.  
  
|Commande DBCC CHECKIDENT|Correction(s) d'identité effectuée(s)|  
|-----------------------------|---------------------------------------------|  
|DBCC CHECKIDENT ( *table_name*, NORESEED )|La valeur d'identité courante n'est pas redéfinie. DBCC CHECKIDENT renvoie la valeur d'identité actuelle et la valeur maximale actuelle de la colonne d'identité. Si les deux valeurs diffèrent, vous devez redéfinir la valeur d'identité afin d'éviter les erreurs ou écarts potentiels dans la séquence de valeurs.|  
|DBCC CHECKIDENT ( *table_name* )<br /><br /> or<br /><br /> DBCC CHECKIDENT ( *table_name*, RESEED )|Si la valeur d'identité actuelle pour une table est inférieure à la valeur d'identité maximale stockée dans la colonne d'identité, elle est redéfinie à l'aide de cette valeur maximale dans la colonne d'identité. Consultez la section Exceptions qui suit.|  
|DBCC CHECKIDENT ( *table_name*, RESEED, *new_reseed_value* )|La valeur d’identité actuelle est définie sur *new_reseed_value*. Si aucune ligne n’a été insérée dans la table depuis sa création, ou si toutes les lignes ont été supprimées à l’aide de l’instruction TRUNCATE TABLE, la première ligne insérée après l’exécution de DBCC CHECKIDENT utilise *new_reseed_value* comme valeur d’identité. Si des lignes sont présentes dans la table ou si toutes les lignes ont été supprimées avec l’instruction DELETE, la ligne insérée suivante utilise *new_reseed_value* + la valeur de [l’incrément actuel](../../t-sql/functions/ident-incr-transact-sql.md). Si une transaction insère une ligne et est ensuite restaurée, la prochaine ligne insérée utilise *new_reseed_value* + la valeur [current increment](../../t-sql/functions/ident-incr-transact-sql.md) comme si la ligne avait été supprimée. Si la table n'est pas vide, le fait d'attribuer à la valeur d'identité un nombre inférieur à la valeur maximale dans la colonne d'identité peut aboutir à l'une des situations suivantes :<br /><br /> -Si une contrainte PRIMARY KEY ou UNIQUE existe sur la colonne d’identité, les opérations d’insertion ultérieures dans la table déclenchent le message d’erreur 2627, car la valeur d’identité générée entre en conflit avec les valeurs existantes.<br /><br /> -Si aucune contrainte PRIMARY KEY ou UNIQUE n’existe, les opérations d’insertion ultérieures aboutissent à des valeurs d’identité dupliquées.|  
  
## <a name="exceptions"></a>Exceptions

 Le tableau suivant liste les conditions dans lesquelles DBCC CHECKIDENT ne redéfinit pas automatiquement la valeur d’identité actuelle et indique comment redéfinir celle-ci.  
  
|Condition|Méthodes de redéfinition|  
|---------------|-------------------|  
|La valeur d'identité actuelle est supérieure à la valeur maximale de la table.|Exécutez DBCC CHECKIDENT (*table_name*, NORESEED) pour déterminer la valeur maximale actuelle dans la colonne. Ensuite, spécifiez cette valeur comme la *new_reseed_value* dans une commande DBCC CHECKIDENT (*table_name*, RESEED,*new_reseed_value*).<br /><br /> OU<br /><br /> Exécutez DBCC CHECKIDENT (*table_name*, RESEED,*new_reseed_value*) en définissant *new_reseed_value* sur une valeur très faible, puis exécutez DBCC CHECKIDENT (*table_name*, RESEED) pour corriger la valeur.|  
|Toutes les lignes sont supprimées de la table.|Exécutez DBCC CHECKIDENT (*table_name*, RESEED,*new_reseed_value*) en définissant *new_reseed_value* sur la nouvelle valeur de départ.|  
  
## <a name="changing-the-seed-value"></a>Modification de la valeur de départ

 La valeur de départ est la valeur insérée dans une colonne d’identité pour la première ligne chargée dans la table. Tous les lignes suivantes contiennent la valeur d'identité actuelle à laquelle s'ajoute la valeur d'incrément (la valeur d'identité actuelle étant la dernière valeur d'identité générée pour la table ou la vue).  
  
 Vous ne pouvez pas utiliser DBCC CHECKIDENT pour les tâches suivantes :  
  
- Changer la valeur de départ d’origine spécifiée pour une colonne d’identité lors de la création de la table ou de la vue.  
  
- attribuer une nouvelle valeur de départ à des lignes existantes d'une table ou d'une vue.  
  
 Pour changer la valeur de départ d’origine dans des lignes existantes, supprimez la colonne d’identité et recréez-la en spécifiant la nouvelle valeur de départ. Lorsque la table contient des données, les numéros d'identité sont ajoutés aux lignes existantes avec les les valeurs de départ et d'incrément. L’ordre dans lequel les lignes sont mises à jour n’est pas garanti.  
  
## <a name="result-sets"></a>Jeux de résultats

 Que vous spécifiiez ou non les options pour une table qui contient une colonne d’identité, DBCC CHECKIDENT retourne le message suivant pour toutes les opérations, sauf une. Cette opération spécifie une nouvelle valeur de départ.  
  
`Checking identity information: current identity value '\<current identity value>', current column value '\<current column value>'. DBCC execution completed. If DBCC printed error messages, contact your system administrator.`
  
 Lors de l’utilisation de DBCC CHECKIDENT pour spécifier une nouvelle valeur de départ à l’aide de RESEED *new_reseed_value*, le message suivant est retourné.  
  
`Checking identity information: current identity value '\<current identity value>'. DBCC execution completed. If DBCC printed error messages, contact your system administrator.`
  
## <a name="permissions"></a>Autorisations

 L’appelant doit être propriétaire du schéma qui contient la table, ou être membre du rôle serveur fixe **sysadmin**, du rôle de base de données fixe **db_owner** ou du rôle de base de données fixe **db_ddladmin**.

[!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] requiert les autorisations **db_owner**.
  
## <a name="examples"></a>Exemples  
  
### <a name="a-resetting-the-current-identity-value-if-its-needed"></a>R. Redéfinition de la valeur d’identité courante, si nécessaire  
 Le cas échéant, l’exemple suivant redéfinit la valeur d’identité actuelle pour la table spécifiée dans la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
```sql
USE AdventureWorks2012;  
GO  
DBCC CHECKIDENT ('Person.AddressType');  
GO  
```  
  
### <a name="b-reporting-the-current-identity-value"></a>B. Présentation de la valeur d'identité courante

 L’exemple suivant signale la valeur d’identité actuelle de la table spécifiée dans la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] et ne corrige pas cette valeur si elle est incorrecte.  
  
```sql
USE AdventureWorks2012;
GO  
DBCC CHECKIDENT ('Person.AddressType', NORESEED);
GO  
```  
  
### <a name="c-forcing-the-current-identity-value-to-a-new-value"></a>C. Attribution forcée d’une nouvelle valeur pour la valeur d’identité actuelle  
 L'exemple suivant impose la valeur 10 pour la valeur d'identité actuelle dans la colonne `AddressTypeID` de la table `AddressType`. Étant donné que la table contient déjà des lignes, la ligne insérée suivante utilise la valeur 11 : la nouvelle valeur de l’identité actuelle plus 1 (qui est la valeur d’incrément de la colonne).  

```sql
USE AdventureWorks2012;  
GO  
DBCC CHECKIDENT ('Person.AddressType', RESEED, 10);  
GO  
```

### <a name="d-resetting-the-identity-value-on-an-empty-table"></a>D. Réinitialisation de la valeur d’identité sur une table vide

 L’exemple suivant impose la valeur 1 pour la valeur d’identité actuelle dans la colonne `ErrorLogID` de la table `ErrorLog`, après la suppression de tous les enregistrements de la table. Étant donné que la table ne contient aucune ligne, la ligne suivante insérée utilise la valeur 1, autrement dit la nouvelle valeur d’identité actuelle, sans ajouter la valeur d’incrément définie pour la colonne.  
  
```sql
USE AdventureWorks2012;  
GO  
TRUNCATE TABLE dbo.ErrorLog
GO
DBCC CHECKIDENT ('dbo.ErrorLog', RESEED, 1);  
GO  
```  
  
## <a name="see-also"></a>Voir aussi

[ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)  
[CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)  
[DBCC &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-transact-sql.md)  
[IDENTITY &#40;propriété&#41; &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql-identity-property.md)  
[Répliquer des colonnes d’identité](../../relational-databases/replication/publish/replicate-identity-columns.md)  
[USE &#40;Transact-SQL&#41;](../../t-sql/language-elements/use-transact-sql.md)  
[IDENT_SEED &#40;Transact-SQL&#41;](../../t-sql/functions/ident-seed-transact-sql.md)  
[IDENT_INCR &#40;Transact-SQL&#41;](../../t-sql/functions/ident-incr-transact-sql.md)  
