---
description: ALTER TABLE, commande SQL
title: ALTER TABLE-commande SQL | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- alter table [ODBC]
ms.assetid: 3a01a291-f4d9-43bc-a725-5a95546ff364
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 186647c40517c5ecf77a55e6bbc08155c98ffed9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205761"
---
# <a name="alter-table---sql-command"></a>ALTER TABLE, commande SQL
Modifie par programmation la structure d’une table.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
ALTER TABLE TableName1  
   ADD | ALTER [COLUMN] FieldName1  
      FieldType [(nFieldWidth [, nPrecision])]  
      [NULL | NOT NULL]  
      [CHECK lExpression1 [ERROR cMessageText1]]  
      [DEFAULT eExpression1]  
      [PRIMARY KEY | UNIQUE]  
      [REFERENCES TableName2 [TAG TagName1]]  
      [NOCPTRANS]  
 - Or -  
ALTER TABLE TableName1  
   ALTER [COLUMN] FieldName2  
      [NULL | NOT NULL]  
      [SET DEFAULT eExpression2]  
      [SET CHECK lExpression2 [ERROR cMessageText2]]  
      [DROP DEFAULT]  
      [DROP CHECK]  
 - Or -  
ALTER TABLE TableName1  
   [DROP [COLUMN] FieldName3]  
   [SET CHECK lExpression3 [ERROR cMessageText3]]  
   [DROP CHECK]  
   [ADD PRIMARY KEY eExpression3 TAG TagName2]  
   [DROP PRIMARY KEY]  
   [ADD UNIQUE eExpression4 [TAG TagName3]]  
   [DROP UNIQUE TAG TagName4]  
   [ADD FOREIGN KEY [eExpression5] TAG TagName4  
      REFERENCES TableName2 [TAG TagName5]]  
   [DROP FOREIGN KEY TAG TagName6 [SAVE]]  
   [RENAME COLUMN FieldName4 TO FieldName5]  
   [NOVALIDATE]  
```  
  
## <a name="arguments"></a>Arguments  
 *TableName1*  
 Spécifie le nom de la table dont la structure est modifiée.  
  
 Ajouter [colonne] *FieldName1*  
 Spécifie le nom du champ à ajouter.  
  
 ALTER [colonne] *FieldName1*  
 Spécifie le nom d’un champ existant à modifier.  
  
 *FieldType* [( *NFieldWidth* [, *nPrecision*]])  
 Spécifie le type de champ, la largeur de champ et la précision de champ (nombre de décimales) d’un champ nouveau ou modifié.  
  
 *FieldType* est une lettre unique qui indique le type de [données](../../odbc/microsoft/visual-foxpro-field-data-types.md)du champ. Certains types de données de champ requièrent que vous spécifiiez *nFieldWidth* ou *nPrecision* , ou les deux.  
  
 *nFieldWidth* et *nPrecision* sont ignorés pour les types D, G, I, L, M, P, T et Y. Par défaut, *nPrecision* est égal à zéro (pas de décimale) si *nPrecision* n’est pas inclus pour les types B, F ou N.  
  
 NULL &#124; NOT NULL  
 Autorise ou empêche les valeurs NULL dans le champ.  
  
 Si vous omettez NULL et NOT NULL, la valeur actuelle de SET NULL détermine si les valeurs NULL sont autorisées dans le champ. Toutefois, si vous omettez NULL et NOT NULL et que vous incluez la clause PRIMARY KEY ou UNIQUE, la valeur actuelle de SET NULL est ignorée et le champ n’est pas NULL par défaut.  
  
 VÉRIFIER *lExpression1*  
 Spécifie une règle de validation pour le champ. *lExpression1* doit correspondre à une expression logique et peut être une fonction définie par l’utilisateur ou une procédure stockée. Chaque fois qu’un enregistrement vide est ajouté, la règle de validation est vérifiée. Une erreur est générée si la règle de validation n’autorise pas la présence d’une valeur de champ vide dans un enregistrement ajouté.  
  
 ERREUR *cMessageText1*  
 Spécifie le message d’erreur affiché lorsque la règle de validation de champ génère une erreur.  
  
 *EExpression1* par défaut  
 Spécifie une valeur par défaut pour le champ. Le type de données de *eExpression1* doit être le même que celui du champ.  
  
 PRIMARY KEY  
 Crée une balise d’index primaire. La balise d’index porte le même nom que le champ.  
  
 UNIQUE  
 Crée une balise d’index candidat portant le même nom que le champ.  
  
> [!NOTE]  
>  Les index candidats (créés avec l’option UNIQUE, fournie pour la compatibilité ANSI dans ALTER TABLE ou CREATE TABLE) diffèrent des index créés à l’aide de l’option UNIQUE dans la commande INDEX. Un index créé à l’aide de l’option UNIQUE dans la commande INDEX autorise les clés d’index dupliquées ; les index candidats n’autorisent pas les clés d’index en double.  
  
 Les valeurs NULL et les enregistrements en double ne sont pas autorisés dans un champ utilisé pour un index principal ou candidat.  
  
 Si vous créez un nouveau champ à l’aide de l’ajout d’une colonne, Visual FoxPro ne génère pas d’erreur si vous créez un index principal ou candidat pour un champ qui prend en charge les valeurs NULL. Toutefois, Visual FoxPro génère une erreur si vous essayez d’entrer une valeur null ou dupliquée dans un champ utilisé pour un index principal ou candidat.  
  
 Si vous modifiez un champ existant et si l’expression d’index principale ou candidat est composée de champs dans la table, Visual FoxPro vérifie les champs pour déterminer s’ils contiennent des valeurs null ou des enregistrements en double. Si c’est le cas, Visual FoxPro génère une erreur et la table n’est pas modifiée.  
  
 RÉFÉRENCE *TableName2* balise *TagName1*  
 Spécifie la table parente à laquelle une relation persistante est établie. TAG *TagName1* spécifie la balise d’index de la table parente sur laquelle la relation est basée. Les noms de balises d’index peuvent contenir jusqu’à 10 caractères.  
  
 NOCPTRANS  
 Empêche la traduction vers une page de codes différente pour les champs de type caractère et mémo. Si la table est convertie en une autre page de codes, les champs pour lesquels NOCPTRANS a été spécifié ne sont pas traduits. NOCPTRANS peut être spécifié uniquement pour les champs de type caractère et mémo.  
  
 L’exemple suivant crée une table nommée MyTable qui contient deux champs de caractères et deux champs Mémo. Le deuxième champ de caractère, Char2, et le deuxième champ MEMO, memo2, incluent NOCPTRANS pour empêcher la traduction.  
  
```  
CREATE TABLE mytable (char1 C(10), char2 C(10) NOCPTRANS,;  
   memo1 M, memo2 M NOCPTRANS)  
```  
  
 ALTER [colonne] *FieldName2*  
 Spécifie le nom d’un champ existant à modifier.  
  
 DÉFINIR la valeur par défaut *eExpression2*  
 Spécifie une nouvelle valeur par défaut pour un champ existant. Le type de données de *eExpression2* doit être le même que celui du champ.  
  
 DÉFINIR la vérification *lExpression2*  
 Spécifie une nouvelle règle de validation pour un champ existant. *lExpression2* doit correspondre à une expression logique et peut être une fonction définie par l’utilisateur ou une procédure stockée.  
  
 ERREUR *cMessageText2*  
 Spécifie le message d’erreur affiché lorsque la règle de validation de champ génère une erreur. Le message s’affiche uniquement lorsque les données sont modifiées dans une fenêtre Parcourir ou modifier.  
  
 DROP DEFAULT  
 Supprime la valeur par défaut d’un champ existant.  
  
 DÉPOSER LA VÉRIFICATION  
 Supprime la règle de validation pour un champ existant.  
  
 DROP [colonne] *FieldName3*  
 Spécifie un champ à supprimer de la table. La suppression d’un champ de la table supprime également le paramètre de valeur par défaut et la règle de validation de champ du champ.  
  
 Si la clé d’index ou les expressions de déclencheur référencent le champ, les expressions ne sont pas valides lorsque le champ est supprimé. Dans ce cas, aucune erreur n’est générée lorsque le champ est supprimé, mais que la clé d’index ou les expressions de déclencheur non valides génèrent des erreurs au moment de l’exécution.  
  
 DÉFINIR la vérification *lExpression3*  
 Spécifie la règle de validation de table. *lExpression3* doit correspondre à une expression logique et peut être une fonction définie par l’utilisateur ou une procédure stockée.  
  
 ERREUR *cMessageText3*  
 Spécifie le message d’erreur affiché lorsque la règle de validation de table génère une erreur. Le message s’affiche uniquement lorsque les données sont modifiées dans une fenêtre Parcourir ou modifier.  
  
 DÉPOSER LA VÉRIFICATION  
 Supprime la règle de validation de la table.  
  
 Ajouter une balise *eExpression3* de clé primaire *TagName2*  
 Ajoute un index primaire à la table. *eExpression3* spécifie l’expression de clé d’index primaire et *TagName2* spécifie le nom de la balise d’index primaire. Les noms de balises d’index peuvent contenir jusqu’à 10 caractères. Si la BALIse *TagName2* est omise et que *eExpression3* est un champ unique, la balise d’index primaire porte le même nom que le champ spécifié dans *eExpression3*.  
  
 SUPPRIMER LA CLÉ PRIMAIRE  
 Supprime l’index primaire et sa balise d’index. Étant donné qu’une table ne peut avoir qu’une seule clé primaire, il n’est pas nécessaire de spécifier le nom de la clé primaire. La suppression de l’index principal supprime également toutes les relations persistantes en fonction de la clé primaire.  
  
 ADD UNIQUE *eExpression4*[tag *TagName3*]  
 Ajoute un index candidat à la table. *eExpression4* spécifie l’expression de clé d’index candidat et *TagName3* spécifie le nom de la balise d’index candidat. Les noms de balises d’index peuvent contenir jusqu’à 10 caractères. Si vous omettez TAG *TagName3* et si *eExpression4* est un champ unique, la balise d’index candidat a le même nom que le champ spécifié dans *eExpression4*.  
  
 SUPPRIMER la BALIse UNIQUE *TagName4*  
 Supprime l’index candidat et sa balise d’index. Étant donné qu’une table peut avoir plusieurs clés candidates, vous devez spécifier le nom de la balise d’index candidat.  
  
 Ajouter une balise de clé étrangère [ *eExpression5*] *TagName4*  
 Ajoute un index étranger (non primaire) à la table. *eExpression5* spécifie l’expression de clé d’index étrangère et *TagName4* spécifie le nom de la balise d’index étrangère. Les noms de balises d’index peuvent contenir jusqu’à 10 caractères.  
  
 REFERENCEs *TableName2*[tag *TagName5*]  
 Spécifie la table parente à laquelle une relation persistante est établie. Incluez la BALIse *TagName5* pour établir une relation basée sur une balise d’index existante pour la table parente. Les noms de balises d’index peuvent contenir jusqu’à 10 caractères. Si vous omettez la BALIse *TagName5*, la relation est établie à l’aide de la balise d’index primaire de la table parente.  
  
 SUPPRIMER la BALIse de clé étrangère *TagName6*[Enregistrer]  
 Supprime une clé étrangère dont la balise d’index est *TagName6*. Si vous omettez SAVE, la balise d’index est supprimée de l’index structurel. Incluez enregistrer pour empêcher la suppression de la balise d’index de l’index structurel.  
  
 Renommer la colonne *FieldName4* en *FieldName5*  
 Vous permet de modifier le nom d’un champ dans la table. *FieldName4* spécifie le nom du champ qui est renommé. *FieldName5* spécifie le nouveau nom du champ.  
  
> [!CAUTION]  
>  Soyez vigilant lorsque vous renommez des champs de table, car les expressions d’index, les règles de validation de champ et de table, les commandes et les fonctions peuvent référencer les noms de champs d’origine.  
  
 NOVALIDATE  
 Spécifie que Visual FoxPro permet d’apporter des modifications à la structure de la table. Ces modifications peuvent violer l’intégrité des données dans la table. Par défaut, Visual FoxPro empêche ALTER TABLE d’apporter des modifications qui enfreignent l’intégrité des données dans la table. Incluez novalidate pour remplacer ce comportement par défaut.  
  
## <a name="remarks"></a>Notes  
 ALTER TABLE peut être utilisé pour modifier la structure d’une table qui n’a pas été ajoutée à une base de données. Toutefois, Visual FoxPro génère une erreur si vous incluez la clause par défaut, la clé étrangère, la clé primaire, les références ou les clauses SET lors de la modification d’une table libre.  
  
 ALTER TABLE peut reconstruire la table en créant un nouvel en-tête de table et en ajoutant des enregistrements à l’en-tête de la table. Par exemple, la modification du type ou de la largeur d’un champ peut entraîner la reconstruction de la table.  
  
 Une fois qu’une table est reconstruite, des règles de validation de champ sont exécutées pour tous les champs dont le type ou la largeur est modifié. Si vous modifiez le type ou la largeur de n’importe quel champ de la table, la règle de table est exécutée.  
  
 Si vous modifiez des règles de validation de champ ou de table pour une table qui contient des enregistrements, Visual FoxPro teste les nouvelles règles de validation de champ ou de table par rapport aux données existantes et émet un avertissement sur la première occurrence d’une règle de validation de champ ou de table ou d’une violation de déclencheur.  
  
 Si la table que vous modifiez se trouve dans une base de données, ALTER TABLE-SQL requiert l’utilisation exclusive de la base de données. Pour ouvrir une base de données en vue d’une utilisation exclusive, incluez EXCLUSIVE dans la base de données ouverte.  
  
## <a name="see-also"></a>Voir aussi  
 [CREATE TABLE-commande SQL](../../odbc/microsoft/create-table-sql-command.md)   
 [INDEX, commande](../../odbc/microsoft/index-command.md)
