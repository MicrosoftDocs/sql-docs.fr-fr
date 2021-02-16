---
description: SET ANSI_WARNINGS (Transact-SQL)
title: SET ANSI_WARNINGS (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 04/15/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SET ANSI_WARNINGS
- ANSI_WARNINGS_TSQL
- ANSI_WARNINGS
- SET_ANSI_WARNINGS_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- errors [SQL Server], ISO standard behavior
- warnings [SQL Server]
- SET ANSI_WARNINGS statement
- ANSI_WARNINGS option
ms.assetid: f82aaab0-334f-427b-89b0-de4af596b4fa
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 13b4638d0ce8076b337b4879893ae23242fa00c4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350016"
---
# <a name="set-ansi_warnings-transact-sql"></a>SET ANSI_WARNINGS (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Spécifie le comportement conforme à la norme ISO pour plusieurs conditions d'erreur :  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe

### <a name="syntax-for-ssnoversion-mdmd-and-sssodfull-mdmd"></a>Syntaxe pour [!INCLUDE[ssnoversion-md.md](../../includes/ssnoversion-md.md)] et [!INCLUDE[sssodfull-md.md](../../includes/sssodfull-md.md)]
```syntaxsql
SET ANSI_WARNINGS { ON | OFF }
```

### <a name="syntax-for-sssdw-mdmd-and-sspdw-mdmd"></a>Syntaxe pour [!INCLUDE[sssdw-md.md](../../includes/sssdw-md.md)] et [!INCLUDE[sspdw-md.md](../../includes/sspdw-md.md)]
```syntaxsql
SET ANSI_WARNINGS ON
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>Notes
 SET ANSI_WARNINGS a une incidence sur les conditions suivantes :  
  
-   Lorsque la valeur est définie à ON et que des valeurs NULL figurent dans des fonctions d'agrégation (par exemple, SUM, AVG, MAX, MIN, STDEV, STDEVP, VAR, VARP ou COUNT), un message d'avertissement est généré. Lorsque la valeur est définie à OFF, aucun avertissement n'est émis.  
  
-   Lorsque la valeur est définie à ON, les erreurs de division par zéro et de dépassement de capacité arithmétique provoquent l'annulation de l'instruction et l'émission d'un message d'erreur. Lorsque la valeur est définie à OFF, les erreurs de division par zéro et de dépassement arithmétique entraînent le renvoi de valeurs NULL. Une erreur de division par zéro ou de dépassement arithmétique provoque le retour de valeurs NULL si une opération INSERT ou UPDATE est tentée sur une colonne de type **character**, Unicode ou **binary** contenant une nouvelle valeur dont la longueur est supérieure à la taille maximale de la colonne. Conformément à la norme ISO, si l’option SET ANSI_WARNINGS est activée (valeur ON), l’opération INSERT ou UPDATE est annulée. Les espaces blancs sont ignorés dans des colonnes de type caractère et les zéros sont ignorés dans les colonnes de type binaire. Lorsque la valeur est définie à OFF, les données sont tronquées de façon à correspondre à la taille de la colonne, et l'instruction s'exécute correctement.  
  
> [!NOTE]  
> Quand la troncature se produit au cours d’une conversion à partir de ou vers des données de type **binary** ou **varbinary**, aucun message d’avertissement ou d’erreur n’est émis, quelles que soient les options SET.  
  
> [!NOTE]  
> L'option ANSI_WARNINGS n'est pas reconnue lors d'un passage de paramètres dans une procédure stockée ou dans une fonction définie par l'utilisateur, ou bien lors de la déclaration et de la définition de variables dans une instruction par lot. Par exemple, si une variable est définie comme **char(3)** , puis réglée sur une valeur supérieure à trois caractères, les données sont tronquées à la taille définie et l’instruction INSERT ou UPDATE réussit.  
  
L'option user options de la procédure stockée sp_configure peut servir à définir la valeur par défaut de ANSI_WARNINGS pour toutes les connexions au serveur. Pour plus d'informations, consultez [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md).  
  
ANSI_WARNINGS doit être activé (valeur ON) lors de la création ou de la manipulation d’index dans des colonnes calculées ou des vues indexées. Si SET ANSI_WARNINGS est désactivé (OFF), les instructions CREATE, UPDATE, INSERT et DELETE dans des tables comportant des index sur des colonnes calculées ou des vues indexées échouent. Pour plus d’informations sur les paramètres de l’option SET obligatoire avec les vues indexées et les index sur des colonnes calculées, consultez « Remarques sur l’utilisation des instructions SET » dans [Instructions SET &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md).  
  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] comprend l'option de base de données ANSI_WARNINGS. C'est l'équivalent de SET ANSI_WARNINGS. Lorsque SET ANSI_WARNINGS est défini à ON, des erreurs ou des avertissements sont générés dans le cas d'une division par zéro, d'une chaîne trop longue pour la colonne de la base de données, ou de toute autre erreur similaire. Lorsque SET ANSI_WARNINGS est défini à OFF, ces erreurs et avertissements ne sont pas générés. La valeur par défaut de SET ANSI_WARNINGS dans la base de données model est définie à OFF. Si elle n'est pas spécifiée, la valeur de ANSI_WARNINGS s'applique. Si l’option SET ANSI_WARNINGS est OFF, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilise la valeur de la colonne is_ansi_warnings_on dans la vue de catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).  
  
> [!IMPORTANT]
> La valeur de ANSI_WARNINGS doit être définie à ON lors de l'exécution de requêtes distribuées.  
  
Les clients, tels que le pilote ODBC client natif [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], le fournisseur OLE DB client natif [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et le pilote Microsoft JDBC pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] définissent automatiquement ANSI_WARNINGS sur ON avec un indicateur de connexion. Cette option peut être configurée dans les sources de données et les attributs de connexion ODBC, définis dans l'application avant la connexion. La valeur par défaut pour SET ANSI_WARNINGS est OFF lorsqu'il s'agit de connexions à partir d'applications de bibliothèques de bases de données. Pour plus d’informations, consultez [LOGIN7](/openspecs/windows_protocols/ms-tds/773a62b6-ee89-4c02-9e5e-344882630aac) dans les spécifications du protocole TDS (Tabular Data Stream). 

Quand ANSI_DEFAULTS est ON, ANSI_WARNINGS est activé.  
  
La valeur d’ANSI_WARNINGS est définie au moment de l’exécution, pas au moment de l’analyse.  
  
Si la valeur de SET ARITHABORT ou de SET ARITHIGNORE est définie à OFF et que SET ANSI_WARNINGS a la valeur ON, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] renvoie malgré tout un message d'erreur quand il rencontre une erreur de division par zéro ou de dépassement de capacité.  
  
Pour afficher la valeur actuelle de ce paramètre, exécutez la requête suivante.  
  
```sql  
DECLARE @ANSI_WARN VARCHAR(3) = 'OFF';  
IF ( (8 & @@OPTIONS) = 8 ) SET @ANSI_WARN = 'ON';  
SELECT @ANSI_WARN AS ANSI_WARNINGS;  
```  
  
## <a name="permissions"></a>Autorisations  
Nécessite l'appartenance au rôle **public** .  
  
## <a name="examples"></a>Exemples  
L'exemple suivant illustre les trois cas mentionnés plus haut, avec SET ANSI_WARNINGS prenant la valeur ON ou OFF.  
  
```sql  
CREATE TABLE T1   
(  
   a int,   
   b int NULL,   
   c varchar(20)  
);  
GO  
  
SET NOCOUNT ON;  
  
INSERT INTO T1   
VALUES (1, NULL, '')   
      ,(1, 0, '')  
      ,(2, 1, '')  
      ,(2, 2, '');  
  
SET NOCOUNT OFF;  
GO  
```

Définissez à présent ANSI_WARNINGS avec la valeur ON et effectuez un test.

```sql
PRINT '**** Setting ANSI_WARNINGS ON';  
GO  
  
SET ANSI_WARNINGS ON;  
GO  
  
PRINT 'Testing NULL in aggregate';  
GO  
SELECT a, SUM(b)   
FROM T1   
GROUP BY a;  
GO  
  
PRINT 'Testing String Overflow in INSERT';  
GO  
INSERT INTO T1   
VALUES (3, 3, 'Text string longer than 20 characters');  
GO  
  
PRINT 'Testing Divide by zero';  
GO  
SELECT a / b AS ab   
FROM T1;  
GO  
```

Définissez à présent ANSI_WARNINGS avec la valeur OFF et effectuez un test.

```sql
PRINT '**** Setting ANSI_WARNINGS OFF';  
GO  
SET ANSI_WARNINGS OFF;  
GO  
  
PRINT 'Testing NULL in aggregate';  
GO  
SELECT a, SUM(b)   
FROM T1   
GROUP BY a;  
GO  
  
PRINT 'Testing String Overflow in INSERT';  
GO  
INSERT INTO T1   
VALUES (4, 4, 'Text string longer than 20 characters');  
GO  
SELECT a, b, c   
FROM T1  
WHERE a = 4;  
GO  
  
PRINT 'Testing Divide by zero';  
GO  
SELECT a / b AS ab   
FROM T1;  
GO  
  
DROP TABLE T1;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [Instructions SET &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md)   
 [SET ANSI_DEFAULTS &#40;Transact-SQL&#41;](../../t-sql/statements/set-ansi-defaults-transact-sql.md)   
 [SESSIONPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/sessionproperty-transact-sql.md)  
  
