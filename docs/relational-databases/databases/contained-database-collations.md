---
title: Classements de base de données autonome | Microsoft Docs
description: Découvrez comment classer les tâches dans les bases de donnés non autonomes et autonomes. Voir les problèmes qui peuvent survenir lorsque les sessions passent d'un environnement autonome à un environnement non autonome.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- contained database, collations
ms.assetid: 4b44f6b9-2359-452f-8bb1-5520f2528483
author: stevestein
ms.author: sstein
ms.openlocfilehash: a1da6b96aea93feaacdb5384716ef6cd0ab82a06
ms.sourcegitcommit: e8c0c04eb7009a50cbd3e649c9e1b4365e8994eb
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/14/2021
ms.locfileid: "100489403"
---
# <a name="contained-database-collations"></a>Classements de base de données autonome
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Diverses propriétés affectent l'ordre de tri et la sémantique d'égalité des données textuelles, notamment le respect de la casse, le respect des accents et la langue de base utilisée. Ces qualités sont exprimées à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] par le choix du classement des données. Pour une explication plus approfondie des classements eux-mêmes, consultez [Prise en charge d’Unicode et du classement](../../relational-databases/collations/collation-and-unicode-support.md).  
  
 Les classements ne s'appliquent pas seulement aux données stockées dans les tables utilisateur, mais à tout le texte géré par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], notamment les métadonnées, les objets temporaires, les noms de variable, etc. Leur gestion est différente dans une base de données autonome et dans une base de données non autonome. Cette modification n'affecte pas de nombreux utilisateurs, mais permet l'indépendance et l'uniformité des instances. Cependant, elle peut provoquer également quelques confusions, ainsi que des problèmes pour les sessions qui accèdent à la fois à des bases de données autonomes et à des bases de données non autonomes.  
  
 Cette rubrique apporte des éclaircissements sur le contenu de la modification et indique où celle-ci peut entraîner des problèmes.  
  
## <a name="non-contained-databases"></a>Bases de données sans relation contenant-contenu  
 Toutes les bases de données ont un classement par défaut (qui peut être défini lors de la création ou de la modification d'une base de données). Ce classement est utilisé pour toutes les métadonnées de la base de données, et comme valeur par défaut de toutes les colonnes de chaîne dans la base de données. Les utilisateurs peuvent choisir un classement différent pour une colonne particulière à l’aide de la clause **COLLATE** .  
  
### <a name="example-1"></a>Exemple 1  
 Par exemple, supposons que nous travaillons à Beijing et que nous utilisons un classement chinois :  
  
```sql  
ALTER DATABASE MyDB COLLATE Chinese_Simplified_Pinyin_100_CI_AS;  
```  
  
 Si nous créons une colonne, son classement par défaut sera ce classement chinois, mais nous pouvons en choisir un autre si nous le voulons :  
  
```sql  
CREATE TABLE MyTable  
      (mycolumn1 nvarchar,  
      mycolumn2 nvarchar COLLATE Frisian_100_CS_AS);  
GO  
SELECT name, collation_name  
FROM sys.columns  
WHERE name LIKE 'mycolumn%' ;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```sql  
name            collation_name  
--------------- ----------------------------------  
mycolumn1       Chinese_Simplified_Pinyin_100_CI_AS  
mycolumn2       Frisian_100_CS_AS  
```  
  
 Cela semble relativement simple, mais plusieurs problèmes se posent. Étant donné que le classement d’une colonne dépend de la base de données dans laquelle la table est créée, des problèmes se posent pour l’utilisation des tables temporaires stockées dans **tempdb**. Le classement de **tempdb** correspond généralement au classement de l’instance, qui n’est pas obligatoirement celui de la base de données.  
  
### <a name="example-2"></a>Exemple 2  
 Par exemple, examinez la base de données (chinoise) ci-dessus utilisée sur une instance avec un classement **Latin1_General** :  
  
```sql  
CREATE TABLE T1 (T1_txt nvarchar(max)) ;  
GO  
CREATE TABLE #T2 (T2_txt nvarchar(max)) ;  
GO  
```  
  
 À première vue, ces deux tables semblent avoir le même schéma, mais comme les classements des bases de données diffèrent, les valeurs sont en fait incompatibles :  
  
```  
SELECT T1_txt, T2_txt  
FROM T1   
JOIN #T2   
    ON T1.T1_txt = #T2.T2_txt  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 Msg 468, Niveau 16, État 9, Ligne 2  
  
 Impossible de résoudre le conflit de classement entre « Latin1_General_100_CI_AS_KS_WS_SC » et « Chinese_Simplified_Pinyin_100_CI_AS » dans l'opération égal à.  
  
 Nous pouvons résoudre ce problème en classant la table temporaire de façon explicite. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] simplifie quelque peu cette opération en fournissant le mot clé **DATABASE_DEFAULT** pour la clause **COLLATE** .  
  
```sql  
CREATE TABLE T1 (T1_txt nvarchar(max)) ;  
GO  
CREATE TABLE #T2 (T2_txt nvarchar(max) COLLATE DATABASE_DEFAULT);  
GO  
SELECT T1_txt, T2_txt  
FROM T1   
JOIN #T2   
    ON T1.T1_txt = #T2.T2_txt ;  
```  
  
 Ce code s'exécute maintenant sans erreur.  
  
 Nous pouvons également consulter le comportement dépendant du classement avec des variables. Observez la fonction suivante :  
  
```  
CREATE FUNCTION f(@x INT) RETURNS INT  
AS BEGIN   
      DECLARE @I INT = 1  
      DECLARE @İ INT = 2  
      RETURN @x * @i  
END;  
```  
  
 C'est une fonction assez particulière. Dans un classement respectant la casse, le @i dans la clause return ne peut pas être lié un autre @I ni à @İ. Dans un classement Latin1_General sans respect de la casse, @i est lié à @I, et la fonction retourne 1. Mais dans un classement turc sans respect de la casse, @i est lié à @İ et la fonction retourne 2. Cela peut causer des dégâts dans une base de données qui se déplace entre des instances aux classements différents.  
  
## <a name="contained-databases"></a>Bases de données autonomes  
 Comme un objectif de la conception de bases de données autonomes est de les rendre autonomes, la dépendance de l’instance et des classements de **tempdb** doit être supprimée. Pour cela, les bases de données autonomes présentent le concept de classement de catalogue. Le classement de catalogue est utilisé pour les métadonnées système et les objets transitoires. Des informations complémentaires sont fournies ci-dessous.  
  
 Dans une base de données autonome, le classement de catalogue est **Latin1_General_100_CI_AS_WS_KS_SC**. Ce classement est le même pour toutes les bases de données autonomes sur toutes les instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et ne peut pas être changé.  
  
 Le classement de base de données est conservé, mais est utilisé uniquement comme classement par défaut des données utilisateur. Par défaut, le classement de base de données est égal au classement de base de données model, mais peut être modifié par l’utilisateur via une commande **CREATE** ou **ALTER DATABASE** comme avec les bases de données non autonomes.  
  
 Un nouveau mot clé, **CATALOG_DEFAULT**, est disponible dans la clause **COLLATE** . Il est utilisé comme un raccourci du classement actuel de métadonnées à la fois dans les bases de données autonomes et non autonomes. Autrement dit, dans une base de données non autonome, **CATALOG_DEFAULT** retourne le classement de base de données actuel, puisque les métadonnées sont assemblées dans le classement de base de données. Dans une base de données autonome, ces deux valeurs peuvent être différentes, puisque l'utilisateur peut modifier le classement de base de données afin qu'il ne corresponde pas au classement de catalogue.  
  
 Le comportement de différents objets dans les bases de données autonomes ou non autonomes est résumé dans ce tableau :  
  
|Élément|Base de données non autonome|Base de données autonome|  
|-|-|-|  
|Données utilisateur (valeur par défaut)|DATABASE_DEFAULT|DATABASE_DEFAULT|  
|Données Temp (valeur par défaut)|Classement TempDB|DATABASE_DEFAULT|  
|Métadonnées|DATABASE_DEFAULT / CATALOG_DEFAULT|CATALOG_DEFAULT|  
|Métadonnées temporaires|Classement TempDB|CATALOG_DEFAULT|  
|Variables|Classement d'instance|CATALOG_DEFAULT|  
|Étiquettes goto|Classement d'instance|CATALOG_DEFAULT|  
|Noms de curseur|Classement d'instance|CATALOG_DEFAULT|  
  
 Dans l’exemple de table temp décrit précédemment, nous pouvons voir que ce comportement de classement rend la clause **COLLATE** explicite inutile dans la plupart des utilisations de la table temp. Dans une base de données autonome, ce code s'exécute désormais sans erreur, même si les classements d'instance et de base de données diffèrent :  
  
```sql  
CREATE TABLE T1 (T1_txt nvarchar(max)) ;  
GO  
CREATE TABLE #T2 (T2_txt nvarchar(max));  
GO  
SELECT T1_txt, T2_txt  
FROM T1   
JOIN #T2   
    ON T1.T1_txt = #T2.T2_txt ;  
```  
  
 Cela fonctionne parce que `T1_txt` et `T2_txt` sont assemblés dans le classement de base de données de la base de données autonome.  
  
## <a name="crossing-between-contained-and-uncontained-contexts"></a>Passer d'un contexte à relation contenant-contenu à un contexte sans relation contenant-contenu  
 Tant qu'une session dans une base de données autonome reste contenue, elle doit rester dans la base de données à laquelle elle s'est connectée. Dans ce cas, le comportement est très simple. Mais si une session passe d'un contexte à relation contenant-contenu à un contexte sans relation contenant-contenu, le comportement devient plus complexe, puisque deux ensembles de règles doivent être liés. Cela peut arriver dans une base de données partiellement autonome, puisqu’un utilisateur peut exécuter une opération **USE** sur une autre base de données. Dans ce cas, la différence des règles de classement est gérée selon le principe suivant.  
  
-   Le comportement du classement d'un lot est déterminé par la base de données dans laquelle commence le lot.  
  
 Notez que cette décision est prise avant l’émission d’une commande, notamment la commande **USE** initiale. Autrement dit, si un lot commence dans une base de données autonome, mais que la première commande est une opération **USE** sur une base de données non autonome, le comportement du classement autonome sera toujours utilisé pour le lot. En conséquence, une référence à une variable, par exemple, peut donner plusieurs résultats possibles :  
  
-   La référence peut trouver exactement une correspondance. Dans ce cas, la référence fonctionnera sans erreur.  
  
-   La référence peut pas trouver de correspondance dans le classement actuel alors qu'il en existait une auparavant. Cela génère une erreur indiquant que la variable n'existe pas, bien qu'elle ait été apparemment créée.  
  
-   La référence peut trouver plusieurs correspondances qui étaient distinctes à l'origine. Cela génère également une erreur.  
  
 Illustrons ceci par quelques exemples. Pour ces exemples, nous supposons une base de données partiellement autonome, nommée `MyCDB`, dont le classement de base de données est défini sur le classement par défaut, **Latin1_General_100_CI_AS_WS_KS_SC**. Nous supposons que le classement d’instance est **Latin1_General_100_CS_AS_WS_KS_SC**. Les deux classements diffèrent uniquement en fonction du respect de la casse.  
  
### <a name="example-1"></a>Exemple 1  
 L'exemple suivant illustre le cas où la référence trouve exactement une correspondance.  
  
```  
USE MyCDB;  
GO  
  
CREATE TABLE #a(x int);  
INSERT INTO #a VALUES(1);  
GO  
  
USE master;  
GO  
  
SELECT * FROM #a;  
GO  
  
Results:  
  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
x  
-----------  
1  
```  
  
 Dans ce cas, le #a identifié est lié à la fois au classement de catalogue qui ne respecte pas la casse et au classement d'instance qui respecte la casse ; donc le code fonctionne.  
  
### <a name="example-2"></a>Exemple 2  
 L'exemple suivant illustre le cas où la référence ne trouve pas de correspondance dans le classement actuel alors qu'il en existait une auparavant.  
  
```  
USE MyCDB;  
GO  
  
CREATE TABLE #a(x int);  
INSERT INTO #A VALUES(1);  
GO  
```  
  
 Ici, le #A est lié à #a dans le classement par défaut qui ne respecte pas la casse, et l'insertion fonctionne,  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
(1 row(s) affected)  
```  
  
 mais si nous continuons le script...  
  
```  
USE master;  
GO  
  
SELECT * FROM #A;  
GO  
```  
  
 Nous obtenons une erreur lors de la tentative de lier #A dans le classement d'instance qui respecte la casse ;  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 Msg 208, Niveau 16, État 0, Ligne 2  
  
 Nom d'objet '#A' non valide.  
  
### <a name="example-3"></a>Exemple 3  
 L'exemple suivant illustre le cas où la référence trouve plusieurs correspondances qui étaient distinctes à l'origine. D’abord, nous démarrons dans **tempdb** (dont le classement qui respecte la casse est le même que celui de notre instance) et nous exécutons les instructions suivantes.  
  
```  
USE tempdb;  
GO  
  
CREATE TABLE #a(x int);  
GO  
CREATE TABLE #A(x int);  
GO  
INSERT INTO #a VALUES(1);  
GO  
INSERT INTO #A VALUES(2);  
GO  
```  
  
 Ce code réussit, puisque les tables sont distinctes dans ce classement :  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
(1 row(s) affected)  
(1 row(s) affected)  
```  
  
 Si nous nous déplaçons dans notre base de données autonome, toutefois, nous constatons que nous ne pouvons plus créer de liaison avec ces tables.  
  
```  
USE MyCDB;  
GO  
SELECT * FROM #a;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 Msg 12800, Niveau 16, État 1, Ligne 2  
  
 La référence au nom de table temporaire '#a' est ambiguë et ne peut pas être résolue. Les candidats possibles sont '#a' et '#A.'  
  
## <a name="conclusion"></a>Conclusion  
 Le comportement du classement des bases de données autonomes diffère légèrement de celui des bases de données non autonomes. Ce comportement est généralement bénéfique, car il apporte une indépendance par rapport à l'instance et de la simplicité. Certains utilisateurs peuvent avoir des problèmes, en particulier lorsqu'une session accède à la fois à des bases de données autonomes et non autonomes.  
  
## <a name="see-also"></a>Voir aussi  
 [Bases de données autonomes](../../relational-databases/databases/contained-databases.md)  
  
  
