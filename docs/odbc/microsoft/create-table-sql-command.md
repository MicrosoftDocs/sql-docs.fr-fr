---
description: CREATE TABLE, commande SQL
title: CREATE TABLE-commande SQL | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- CREATE TABLE [ODBC]
ms.assetid: be2143ba-fc16-42c9-84f7-8985cd924860
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 0fb1e91c03f7a1a1fee98ce887be991370450999
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183205"
---
# <a name="create-table---sql-command"></a>CREATE TABLE, commande SQL
Crée une table avec les champs spécifiés.  
  
 Le pilote ODBC Visual FoxPro prend en charge la syntaxe du langage Visual FoxPro natif pour cette commande. Pour obtenir des informations spécifiques au pilote, consultez la **section Remarques** sur le pilote.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
CREATE TABLE | DBF TableName1 [NAME LongTableName] [FREE]  
   (FieldName1FieldType [(nFieldWidth [, nPrecision])]  
      [NULL | NOT NULL]   
      [CHECK lExpression1 [ERROR cMessageText1]]  
      [DEFAULT eExpression1]  
      [PRIMARY KEY | UNIQUE]  
      [REFERENCES TableName2 [TAG TagName1]]  
      [NOCPTRANS]  
   [, FieldName2 ...]  
      [, PRIMARY KEY eExpression2 TAG TagName2  
      |, UNIQUE eExpression3 TAG TagName3]  
      [, FOREIGN KEY eExpression4 TAG TagName4 [NODUP]  
            REFERENCES TableName3 [TAG TagName5]]  
      [, CHECK lExpression2 [ERROR cMessageText2]])  
| FROM ARRAY ArrayName  
```  
  
## <a name="arguments"></a>Arguments  
 CREATE TABLE &#124; DBF *TableName1*  
 Spécifie le nom de la table à créer. Les options TABLE et DBF sont identiques.  
  
 NOM *LongTableName*  
 Spécifie un nom long pour la table. Un nom de table long ne peut être spécifié que lorsqu’une base de données est ouverte, car les noms de tables longs sont stockés dans des bases de données.  
  
 Les noms longs peuvent contenir jusqu’à 128 caractères et peuvent être utilisés à la place des noms de fichiers courts dans la base de données.  
  
 FREE  
 Spécifie que la table ne sera pas ajoutée à une base de données ouverte. La version gratuite n’est pas obligatoire si une base de données n’est pas ouverte.  
  
 *(FieldName1 FieldType* [( *NFieldWidth* [, *nPrecision*])]  
 Spécifie le nom du champ, le type de champ, la largeur du champ et la précision du champ (nombre de décimales), respectivement.  
  
 *FieldType* est une lettre unique indiquant le type de [données](../../odbc/microsoft/visual-foxpro-field-data-types.md)du champ. Certains types de données de champ requièrent que vous spécifiiez *nFieldWidth* ou *nPrecision* , ou les deux.  
  
 *nFieldWidth* et *nPrecision* sont ignorés pour les types D, G, I, L, M, P, T et Y. *nPrecision* est défini par défaut sur zéro (pas de décimale) si *nPrecision* n’est pas inclus pour les types B, F ou N.  
  
 NULL  
 Autorise les valeurs NULL dans le champ.  
  
 NOT NULL  
 Empêche les valeurs NULL dans le champ.  
  
 Si vous omettez NULL et NOT NULL, la valeur actuelle de SET NULL détermine si les valeurs NULL sont autorisées dans le champ. Toutefois, si vous omettez NULL et NOT NULL et que vous incluez la clause PRIMARY KEY ou UNIQUE, la valeur actuelle de SET NULL est ignorée et la valeur par défaut du champ est NOT NULL.  
  
 VÉRIFIER *lExpression1*  
 Spécifie une règle de validation pour le champ. *lExpression1* peut être une fonction définie par l’utilisateur. Chaque fois qu’un enregistrement vide est ajouté, la règle de validation est vérifiée. Une erreur est générée si la règle de validation n’autorise pas la présence d’une valeur de champ vide dans un enregistrement ajouté.  
  
 ERREUR *cMessageText1*  
 Spécifie le message d’erreur affiché par Visual FoxPro lorsque la règle de champ génère une erreur. Le message s’affiche uniquement lorsque les données sont modifiées dans une fenêtre Parcourir ou une fenêtre d’édition.  
  
 *EExpression1* par défaut  
 Spécifie une valeur par défaut pour le champ. Le type de données de *eExpression1* doit être le même que le type de données du champ.  
  
 PRIMARY KEY  
 Crée un index primaire pour le champ. La balise d’index primaire porte le même nom que le champ.  
  
 UNIQUE  
 Crée un index candidat pour le champ. La balise d’index candidat a le même nom que le champ.  
  
> [!NOTE]  
>  Les index candidats (créés en incluant l’option UNIQUE dans CREATE TABLE ou ALTER TABLE-SQL) ne sont pas les mêmes que les index créés avec l’option UNIQUE dans la commande INDEX. Un index créé avec l’option UNIQUE dans la commande INDEX autorise les clés d’index dupliquées ; les index candidats n’autorisent pas les clés d’index en double. Pour plus d’informations sur son option UNIQUE, consultez [index](../../odbc/microsoft/index-command.md) .  
  
 Les valeurs NULL et les enregistrements en double ne sont pas autorisés dans un champ utilisé pour un index principal ou candidat. Toutefois, Visual FoxPro ne génère pas d’erreur si vous créez un index principal ou candidat pour un champ qui prend en charge les valeurs NULL. Visual FoxPro génère une erreur si vous tentez d’entrer une valeur null ou dupliquée dans un champ utilisé pour un index principal ou candidat.  
  
 REFERENCEs *TableName2*[tag *TagName1*]  
 Spécifie la table parente à laquelle une relation persistante est établie. Si vous omettez la BALIse *TagName1*, la relation est établie à l’aide de la clé d’index primaire de la table parente. Si la table parente n’a pas d’index primaire, Visual FoxPro génère une erreur.  
  
 Incluez la BALIse *TagName1* pour établir une relation basée sur une balise d’index existante pour la table parente. Les noms de balises d’index peuvent contenir jusqu’à 10 caractères.  
  
 La table parente ne peut pas être une table libre.  
  
 NOCPTRANS  
 Empêche la traduction vers une page de codes différente pour les champs de type caractère et mémo. Si la table est convertie en une autre page de codes, les champs pour lesquels NOCPTRANS a été spécifié ne sont pas traduits. NOCPTRANS peut être spécifié uniquement pour les champs de type caractère et mémo.  
  
 L’exemple suivant crée une table nommée MyTable contenant deux champs de caractères et deux champs Mémo. Le deuxième champ de caractère, Char2, et le deuxième champ MEMO, memo2, incluent NOCPTRANS pour empêcher la traduction.  
  
```  
CREATE TABLE mytable (char1 C(10), char2 C(10) NOCPTRANS,;  
   memo1 M, memo2 M NOCPTRANS)  
```  
  
 CLÉ primaire *eExpression2* balise *TagName2*  
 Spécifie un index primaire à créer. *eExpression2* spécifie un champ ou une combinaison de champs dans la table. TAG *TagName2* spécifie le nom de la balise d’index primaire qui est créée. Les noms de balises d’index peuvent contenir jusqu’à 10 caractères.  
  
 Étant donné qu’une table ne peut avoir qu’un seul index primaire, vous ne pouvez pas inclure cette clause si vous avez déjà créé un index primaire pour un champ. Visual FoxPro génère une erreur si vous incluez plusieurs clauses de clé primaire dans CREATE TABLE.  
  
 Étiquette *EEXPRESSION3* unique *TagName3*  
 Crée un index candidat. *eExpression3* spécifie un champ ou une combinaison de champs dans la table. Toutefois, si vous avez créé un index primaire avec l’une des options de clé primaire, vous ne pouvez pas inclure le champ qui a été spécifié pour l’index primaire. TAG *TagName3* spécifie un nom de balise pour la balise d’index candidat qui est créée. Les noms de balises d’index peuvent contenir jusqu’à 10 caractères.  
  
 Une table peut avoir plusieurs index candidats.  
  
 CLÉ étrangère *eExpression4* balise *TagName4*[NODUP]  
 Crée un index étranger (non primaire) et établit une relation avec une table parente. *eExpression4* spécifie l’expression de clé d’index étrangère et *TagName4* spécifie le nom de la balise de clé d’index étrangère qui est créée. Les noms de balises d’index peuvent contenir jusqu’à 10 caractères. Incluez NODUP pour créer un index étranger candidat.  
  
 Vous pouvez créer plusieurs index étrangers pour la table, mais les expressions d’index étrangères doivent spécifier des champs différents dans la table.  
  
 REFERENCEs *TableName3*[tag *TagName5*]  
 Spécifie la table parente à laquelle une relation persistante est établie. Incluez la BALIse *TagName5* pour établir une relation basée sur une balise d’index pour la table parente. Les noms de balises d’index peuvent contenir jusqu’à 10 caractères. Par défaut, si vous omettez la BALIse *TagName5,* la relation est établie à l’aide de la clé d’index primaire de la table parente.  
  
 VÉRIFIER *eExpression2*[Error *cMessageText2*]  
 Spécifie la règle de validation de table. ERREUR *cMessageText2* spécifie le message d’erreur affiché par Visual FoxPro lors de l’exécution de la règle de validation de table. Le message s’affiche uniquement lorsque les données sont modifiées dans une fenêtre Parcourir ou une fenêtre d’édition.  
  
 À partir du tableau *arrayName*  
 Spécifie le nom d’un tableau existant dont le contenu est le nom, le type, la précision et l’échelle pour chaque champ de la table. Le contenu du tableau peut être défini avec la fonction **AFIELDS**().  
  
## <a name="remarks"></a>Notes  
 La nouvelle table est ouverte dans la zone de travail disponible la plus basse et est accessible par son alias. La nouvelle table est ouverte en mode exclusif, quelle que soit la valeur actuelle de SET EXCLUSIVE.  
  
 Si une base de données est ouverte et que vous n’incluez pas la clause FREE, la nouvelle table est ajoutée à la base de données. Vous ne pouvez pas créer une nouvelle table portant le même nom qu’une table dans la base de données.  
  
 Si une base de données est ouverte, CREATE TABLE-SQL requiert l’utilisation exclusive de la base de données. Pour ouvrir une base de données en vue d’une utilisation exclusive, incluez EXCLUSIVE dans la base de données ouverte.  
  
 Si une base de données n’est pas ouverte au moment de la création de la table, y compris les clauses nom, vérification, valeur par défaut, clé étrangère, clé primaire ou références génèrent une erreur.  
  
> [!NOTE]  
>  CREATE TABLE syntaxe utilise des virgules pour séparer certaines options de CREATE TABLE. En outre, les clauses NULL, NOT NULL, CHECK, DEFAULT, PRIMARY KEY et UNIQUE doivent être placées entre parenthèses contenant les définitions de colonne.  
  
## <a name="driver-remarks"></a>Remarques sur le pilote  
 Lorsque votre application envoie l’instruction SQL ODBC CREATE TABLE à la source de données, le pilote ODBC Visual FoxPro convertit la commande en commande Visual FoxProCREATE TABLE à l’aide de la syntaxe indiquée dans le tableau suivant.  
  
|Syntaxe ODBC|Syntaxe Visual FoxPro|  
|-----------------|--------------------------|  
|Nom de la *table de base* de CREATE TABLE<br /><br /> (*type de données de l’identificateur de colonne*<br /><br /> [NON NULL]<br /><br /> [,*type de données de l’identificateur de colonne*<br /><br /> [NON NULL]...)|CREATE TABLE *TableName1* [name *LongTableName*]<br /><br /> (*FieldName1* *FieldType*<br /><br /> [(*nFieldWidth* [, *nPrecision*])]<br /><br /> [NOT NULL])|  
  
 Lorsque vous créez une table à l’aide du pilote, le pilote ferme la table immédiatement après sa création afin d’autoriser d’autres utilisateurs à accéder à la table. Cela diffère de Visual FoxPro, ce qui laisse la table ouverte exclusivement lors de la création. Toutefois, si une procédure stockée de votre source de données contenant une instruction CREATE TABLE est exécutée, la table reste ouverte.  
  
 Si la source de données est une base de données (fichier. DBC), le pilote ODBC Visual FoxPro crée une table nommée *LongTableName* portant le même nom que le nom de la *table de base*.  
  
### <a name="using-data-definition-language-ddl"></a>Utilisation du langage de définition de données (DDL)  
 Vous ne pouvez pas inclure DDL dans les emplacements suivants :  
  
-   Dans une instruction SQL batch qui requiert une transaction  
  
-   En mode de validation manuelle, après une instruction qui nécessitait une transaction, à moins que votre application n’appelle d’abord **SQLTransact**.  
  
 Par exemple, si vous souhaitez créer une table temporaire, vous devez créer la table avant de commencer l’instruction qui requiert une transaction. Si vous incluez l’instruction CREATE TABLE dans une instruction SQL batch qui requiert une transaction, le pilote retourne un message d’erreur.  
  
## <a name="see-also"></a>Voir aussi  
 [ALTER TABLE-commande SQL](../../odbc/microsoft/alter-table-sql-command.md)   
 [Types de données pris en charge (pilote ODBC Visual FoxPro)](../../odbc/microsoft/supported-data-types-visual-foxpro-odbc-driver.md)   
 [Commande INSERT-SQL](../../odbc/microsoft/insert-sql-command.md)   
 [SELECT, commande SQL](../../odbc/microsoft/select-sql-command.md)
