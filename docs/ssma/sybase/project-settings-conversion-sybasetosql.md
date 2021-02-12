---
description: Paramètres du projet (Conversion) (SybaseToSQL)
title: Paramètres du projet (conversion) (SybaseToSQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/19/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: eeb80fa5-f530-4f21-beee-25f5a4b8ace6
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 66d1ce573f464f14e0b738fb4c461be031db64ba
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100070067"
---
# <a name="project-settings-conversion-sybasetosql"></a>Paramètres du projet (Conversion) (SybaseToSQL)

La page **conversion** de la boîte de dialogue **paramètres du projet** contient des paramètres qui permettent de personnaliser la façon dont SSMA convertit la syntaxe SAP Adaptive Server Enterprise (ASE) vers ou la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] syntaxe SQL Azure.

Le volet **conversion** est disponible dans les boîtes de dialogue **paramètres du projet** et paramètres du **projet par défaut** :

- Si vous souhaitez spécifier des paramètres pour tous les projets SSMA, dans le menu **Outils** , sélectionnez **paramètres du projet par défaut**, cliquez sur **général** en bas du volet gauche, puis sur **conversion**.

- Pour spécifier les paramètres du projet actif, dans le menu **Outils** , sélectionnez **paramètres du projet**, cliquez sur **général** en bas du volet gauche, puis sur **conversion**.

## <a name="miscellaneous-section"></a>Section divers

### <a name="error"></a>@@ERROR

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]/Azure SQL et ASE utilisent des codes d’erreur différents.

Utilisez ce paramètre pour spécifier le type de message (avertissement ou erreur) que SSMA affiche dans le volet de sortie ou de Liste d’erreurs lorsqu’il rencontre une référence à `@@ERROR` dans le code de l’ASE.

- Si vous sélectionnez **convertir et marquer avec un avertissement**, SSMA convertit les instructions et les marque avec des commentaires d’avertissement.
- Si vous sélectionnez **marquer avec une erreur**, SSMA ignore la conversion et marque les instructions avec des commentaires d’erreur.

Lorsque vous sélectionnez un mode de conversion dans la zone **mode** , SSMA applique le paramètre suivant :

|Mode|Valeur|
|-|-|
|Default|Convertir et marquer avec un avertissement|
|Optimistic|Convertir et marquer avec un avertissement|
|Complète|Marquer avec une erreur|

### <a name="conversion-of-like-operator"></a>Conversion d’un opérateur LIKE

Spécifie s’il faut convertir les `LIKE` opérandes pour qu’ils correspondent au comportement SAP ASE. Le point est que ASE supprime les espaces à droite dans un modèle like. La solution de contournement consiste à convertir une expression droite en type de données de longueur fixe avec une précision maximale.
  
- Sélectionnez **conversion simple** pour convertir les expressions sans correction.
- Pour utiliser le comportement ASE **, sélectionnez convertir en longueur fixe.**
  
Lorsque vous sélectionnez un mode de conversion dans la zone mode, SSMA applique le paramètre suivant :

|Mode|Valeur|
|-|-|
|Default|Conversion simple|
|Optimistic|Conversion simple|
|Complète|Cast en longueur fixe|

### <a name="convert-or-cast-empty-strings-to-numeric-types"></a>Convertir ou convertir des chaînes vides en types numériques

Spécifie comment gérer des chaînes vides ou vides `CONVERT` dans `CAST` des expressions ou avec un type numérique comme argument de type de données. Les options suivantes sont disponibles pour ce paramètre :

- Sélectionnez **conversion simple** pour convertir les expressions sans correction.
- Si l’option **chaîne vide en tant que valeur numérique zéro** est sélectionnée, le paramètre de chaîne est `{s}` remplacé par l' `CASE ltrim(rtrim({s})) WHEN "" THEN 0 else {s} END` expression.

Lorsque vous sélectionnez un mode de conversion dans la zone **mode** , SSMA applique le paramètre suivant :

|Mode|Valeur|
|-|-|
|Default|Conversion simple|
|Optimistic|Conversion simple|
|Complète|Chaîne vide en tant que valeur numérique zéro|

### <a name="concatenation-of-null"></a>Concaténation de NULL

Ce paramètre spécifie comment convertir la concaténation de chaînes avec `NULL` . Les options suivantes peuvent être définies pour ce paramètre particulier :

- Si l’option **encapsuler avec la fonction IsNull** est sélectionnée, toutes les constantes non constantes `string_expression` de concaténation seront encapsulées avec `ISNULL(string_expression)` et les `NULL` s seront remplacées par une chaîne vide.
- **Conserver la syntaxe actuelle** conserve la syntaxe d’origine.

Lorsque vous sélectionnez un mode de conversion dans la zone **mode** , SSMA applique le paramètre suivant :

|Mode|Valeur|
|-|-|
|Default|Conserver la syntaxe actuelle|
|Optimistic|Conserver la syntaxe actuelle|
|Complète|Wrap avec la fonction ISNULL|

### <a name="conversion-of-empty-strings"></a>Conversion de chaînes vides

Ce paramètre spécifie comment convertir des chaînes vides. Les options suivantes peuvent être définies pour ce paramètre particulier :

- **Remplacer toutes les expressions de chaîne par des espaces**
- **Remplacer les constantes de chaîne vides par des espaces**

Pour utiliser le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] comportement SQL/Azure, sélectionnez **conserver la syntaxe actuelle**.

Lorsque vous sélectionnez un mode de conversion dans la zone **mode** , SSMA applique le paramètre suivant :

|Mode|Valeur|
|-|-|
|Default|Conserver la syntaxe actuelle|
|Optimistic|Conserver la syntaxe actuelle|
|Complète|Remplacer toutes les expressions de chaîne par des espaces|

### <a name="convert-and-cast-binary-string-conversion"></a>Conversion de chaînes binaires CONVERT et CAST

La conversion de valeurs binaires en nombres peut retourner des valeurs différentes sur différentes plateformes. Par exemple, sur les processeurs x86, `CONVERT(integer, 0x00000100)` retourne `65536` dans ASE, mais `256` dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . ASE retourne également des valeurs différentes en fonction de l’ordre des octets.

Utilisez ce paramètre pour contrôler la façon dont SSMA convertit `CONVERT` les `CAST` expressions et qui contiennent des valeurs binaires :

- Sélectionnez **conversion simple** pour convertir les expressions sans avertissement ni correction. Utilisez ce paramètre si vous savez que le serveur ASE a un ordre d’octet qui ne nécessite pas de modification de la valeur binaire.
- Sélectionnez **convertir et corriger** pour que SSMA convertisse et corrigez les expressions à utiliser sur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . L’ordre des octets dans les constantes littérales est inversé. Toutes les autres valeurs binaires (telles que les variables et les colonnes binaires) sont signalées par des erreurs. Utilisez cette valeur si vous savez que le serveur ASE a un ordre d’octet nécessitant des modifications des valeurs binaires.

Sélectionnez **convertir et marquer avec avertissement** pour que SSMA convertisse et corrige les expressions, et marque toutes les expressions converties avec des commentaires d’avertissement.

Lorsque vous sélectionnez un mode de conversion dans la zone **mode** , SSMA applique le paramètre suivant :

|Mode|Valeur|
|-|-|
|Default|Convertir et marquer avec un avertissement|
|Optimistic|Conversion simple|
|Complète|Convertir et corriger|

### <a name="dynamic-sql"></a>SQL dynamique

Utilisez ce paramètre pour spécifier le type de message (avertissement ou erreur) que SSMA affiche dans le volet de **sortie** ou de **liste d’erreurs** lorsqu’il rencontre du code SQL dynamique dans le code de l’ASE.

- Si vous sélectionnez **convertir et marquer avec avertissement**, SSMA convertit le SQL dynamique et marque les instructions avec des commentaires d’avertissement.
- Si vous sélectionnez **marquer avec une erreur**, SSMA ignore la conversion et marque les instructions avec des commentaires d’erreur.

Lorsque vous sélectionnez un mode de conversion dans la zone **mode** , SSMA applique le paramètre suivant :

|Mode|Valeur|
|-|-|
|Default|Convertir et marquer avec un avertissement|
|Optimistic|Convertir et marquer avec un avertissement|
|Complète|Marquer avec une erreur|

### <a name="equality-check-conversion"></a>Conversion de contrôle d’égalité

Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] /Azure SQL, si le `ANSI_NULLS` paramètre a la valeur on, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] SQL/Azure retourne `UNKNOWN` quand une comparaison d’égalité contient une `NULL` valeur. Si `ANSI_NULLS` est désactivé, les comparaisons d’égalité qui contiennent `NULL` des valeurs retournent la valeur true lorsque la colonne et l’expression comparées ou les deux expressions sont toutes les deux `NULL` . Par défaut ( `ANSINULL OFF` ), les comparaisons d’égalité SAP ASE se comportent comme [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] /Azure SQL avec `ANSI_NULLS OFF` .

- Si vous sélectionnez la **conversion simple**, SSMA convertit le code ASE en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] syntaxe SQL/Azure sans contrôles supplémentaires pour les `NULL` valeurs. Utilisez ce paramètre si `ANSI_NULLS` est `OFF` dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] /Azure SQL ou si vous souhaitez modifier les comparaisons d’égalité au cas par cas.
- Si vous sélectionnez **prendre en compte les valeurs NULL**, SSMA ajoute des vérifications de `NULL` valeurs à l’aide des `IS NULL` `IS NOT NULL` clauses et.

Lorsque vous sélectionnez un mode de conversion dans la zone **mode** , SSMA applique le paramètre suivant :

|Mode|Valeur|
|-|-|
|Default|Conversion simple|
|Optimistic|Conversion simple|
|Complète|Considérer les valeurs NULL|

### <a name="format-strings"></a>Chaînes de format

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]/Azure SQL ne prend plus en charge l' `format_string` argument dans les `PRINT` `RAISERROR` instructions et. `format_string`Argument autorisé à placer des paramètres remplaçables directement dans la chaîne, puis à remplacer les paramètres au moment de l’exécution. Au lieu de cela, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] requiert la chaîne complète à l’aide d’un littéral de chaîne ou d’une chaîne générée à l’aide d’une variable. Pour plus d’informations, consultez la rubrique [Print ( [!INCLUDE[tsql](../../includes/tsql-md.md)] )](../../t-sql/language-elements/print-transact-sql.md) .

Lorsque SSMA rencontre un `format_string` argument, il peut soit générer un littéral de chaîne à l’aide des variables, soit créer une nouvelle variable et générer une chaîne à l’aide de cette variable.

- Pour utiliser un littéral de chaîne pour les `PRINT` `RAISERROR` fonctions et, sélectionnez **créer une nouvelle chaîne**.

    Dans ce mode, si une instruction PRINT ou RAISERROR n’utilise pas d’espaces réservés ni de variables locales, l’instruction est inchangée. Caractères double pour cent (%%) sont remplacées par un seul caractère de pourcentage (%) dans les littéraux de chaîne d’impression.

    Si une instruction PRINT ou RAISERROR utilise des espaces réservés et une ou plusieurs variables locales, comme dans l’exemple suivant :

    ```sql
    PRINT 'Total: %1!%%', @percent
    ```

    SSMA la convertit en la syntaxe suivante :

    ```sql
    PRINT 'Total: '+ CAST(@percent AS varchar(max)) + '%'
    ```

    Si `format_string` est une variable, par exemple dans l’instruction suivante :  

    ```sql
    PRINT @fmt, @arg1, @arg2
    ```

    SSMA ne peut pas effectuer de conversion de chaîne simple et doit créer une variable :

    ```sql
    DECLARE @print_format_1 varchar(max)
    SET @print_format_1 =
        REPLACE (@fmt, '%%', '%')
    SET @print_format_1 =
        REPLACE (@print_format_1, '%1!',
        CAST (@arg1 AS varchar(max)))
    SET @print_format_1 =
        REPLACE (@print_format_1, '%2!',
        CAST (@arg2 AS varchar(max)))
    PRINT @print_format_1
    ```

    Lorsqu’il utilise l’option **créer un nouveau mode de chaîne** , SSMA suppose que l' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] option `CONCAT_NULL_YIELDS_NULL` est `OFF` . Par conséquent, SSMA ne vérifie pas les arguments null.

- Pour que SSMA génère une nouvelle variable pour chaque `PRINT` `RAISERROR` instruction et, puis utilise cette variable pour la valeur de chaîne, sélectionnez **créer une nouvelle variable**.

    Dans ce mode, si une `PRINT` `RAISERROR` instruction ou n’utilise pas d’espaces réservés ni de variables locales, SSMA remplace tous les caractères double ( `%%` ) par des caractères à un seul pourcentage pour se conformer à la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] syntaxe SQL de/Azure.

    Si une `PRINT` `RAISERROR` instruction ou utilise des espaces réservés et une ou plusieurs variables locales, comme dans l’exemple suivant :

    ```sql
    PRINT 'Total: %1!%%', @percent
    ```

    SSMA la convertit en la syntaxe suivante :

    ```sql
    DECLARE @print_format_1 varchar(max)
    SET @print_format_1 = 'Total: %1!%'
    SET @print_format_1 =
        REPLACE (@print_format_1, '%1!',
        ISNULL(CAST (@percent AS VARCHAR(max)), ''))
    PRINT @print_format_1
    ```

    Si `format_string` est une variable, par exemple dans l’instruction suivante :

    ```sql
    PRINT @fmt, @arg1, @arg2
    ```

    SSMA crée une variable comme suit, en vérifiant la présence de valeurs NULL dans chaque argument :

    ```sql
    DECLARE @print_format_1 varchar(max)
    SET @print_format_1 =
        REPLACE (@fmt, '%%', '%')
    SET @print_format_1 =
        REPLACE (@print_format_1, '%1!',
        ISNULL(CAST (@arg1 AS varchar(max)),''))
    SET @print_format_1 =
        REPLACE (@print_format_1, '%2!',
        ISNULL(CAST (@arg2 AS varchar(max)),''))
    PRINT @print_format_1
    ```

Lorsque vous sélectionnez un mode de conversion dans la zone **mode** , SSMA applique le paramètre suivant :

|Mode|Valeur|
|-|-|
|Default|Créer une nouvelle chaîne|
|Optimistic|Créer une nouvelle chaîne|
|Complète|Créer une variable|

### <a name="insert-an-explicit-value-into-a-timestamp-column"></a>Insérer une valeur explicite dans une colonne timestamp

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]/Azure SQL ne prend pas en charge l’insertion de valeurs explicites dans une colonne timestamp.

- Pour exclure les colonnes timestamp des `INSERT` instructions, sélectionnez **exclure une colonne**.
- Pour imprimer un message d’erreur chaque fois qu’une colonne timestamp se trouve dans une `INSERT` instruction, sélectionnez **Mark with Error**. Dans ce mode, les instructions ne sont `INSERT` pas converties et sont marquées avec des commentaires d’erreur.

Lorsque vous sélectionnez un mode de conversion dans la zone **mode** , SSMA applique le paramètre suivant :

|Mode|Valeur|
|-|-|
|Default|Exclure une colonne|
|Optimistic|Exclure une colonne|
|Complète|Marquer avec une erreur|

### <a name="store-temporary-objects-defined-in-procedures"></a>Stocker des objets temporaires définis dans des procédures

Ce paramètre spécifie si les définitions des objets temporaires qui apparaissent dans les procédures doivent être stockées dans les métadonnées sources lors de la conversion.

- Sélectionnez **Oui** pour stocker dans les métadonnées.
- Sélectionnez **non** si les objets n’ont pas besoin d’être stockés.

|Mode|Valeur|
|-|-|
|Default|Oui|
|Optimistic|Oui|
|Complète|Non|

### <a name="proxy-table-conversion"></a>Conversion de table proxy

Spécifie si les tables proxy ASE sont converties en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tables SQL/Azure, si elles ne sont pas converties et si le code est marqué avec des commentaires d’erreur.

- Sélectionnez **convertir** pour convertir les tables proxy en tables normales.
- Sélectionnez **marquer avec erreur** pour marquer simplement le code de la table proxy avec des commentaires d’erreur.

Lorsque vous sélectionnez un mode de conversion dans la zone **mode** , SSMA applique le paramètre suivant :

|Mode|Valeur|
|-|-|
|Default|Marquer avec une erreur|
|Optimistic|Marquer avec une erreur|
|Complète|Marquer avec une erreur|

### <a name="raiserror-base-message-number"></a>Numéro de message de base RAISERROR

Les messages utilisateur ASE sont stockés dans chaque base de données. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] les messages utilisateur sont stockés de manière centralisée et mis à disposition par le biais de l' `sys.messages` affichage catalogue. En outre, les messages utilisateur ASE commencent à `20000` , mais [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] les messages d’erreur commencent à `50001` .

Ce paramètre spécifie le nombre à ajouter au numéro de message utilisateur de l’ASE pour le convertir en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] message utilisateur. Si votre [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] contient des messages utilisateur dans l' `sys.messages` affichage catalogue, vous devrez peut-être modifier ce nombre en lui attribuant une valeur plus élevée. Ainsi, les numéros des messages convertis ne sont pas en conflit avec les numéros de message existants.

Notez les points suivants :
  
- Les messages ASE de la plage proviennent `17000` - `19999` de la `sysmessages` table système et ne sont pas convertis.
- Si le numéro de message référencé dans l' `RAISERROR` instruction est une constante, SSMA ajoute le numéro de message de base à la constante pour déterminer le nouveau numéro de message utilisateur.
- Si le numéro de message référencé est une variable ou une expression, SSMA crée une variable locale intermédiaire.
- En **mode optimiste**, SSMA suppose que l' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] option `CONCAT_NULL_YIELDS_NULL` est et n' `OFF` effectue aucune vérification des `NULL` arguments.
- En **mode complet**, SSMA vérifie les `NULL` arguments.
- `RAISERROR` l' `arg-list` argument with n’est pas converti.

Lorsque vous sélectionnez un mode de conversion dans la zone **mode** , SSMA applique le paramètre suivant :

|Mode|Valeur|
|-|-|
|Default|30001|
|Optimistic|30001|
|Complète|30001|

### <a name="system-objects"></a>Objets système

Utilisez ce paramètre pour spécifier le type de message (avertissement ou erreur) que SSMA affiche dans le volet de **sortie** ou de **liste d’erreurs** lorsqu’il rencontre l’utilisation d’objets système ASE.

- Si vous sélectionnez **convertir et marquer avec avertissement**, SSMA convertit les références aux objets système et marque les instructions avec des commentaires d’avertissement.
- Si vous sélectionnez **marquer avec une erreur**, SSMA ne convertit pas les références aux objets système et marque les instructions avec des commentaires d’erreur.

Lorsque vous sélectionnez un mode de conversion dans la zone **mode** , SSMA applique le paramètre suivant :

|Mode|Valeur|
|-|-|
|Default|Convertir et marquer avec un avertissement|
|Optimistic|Convertir et marquer avec un avertissement|
|Complète|Marquer avec une erreur|

### <a name="unresolved-identifiers"></a>Identificateurs non résolus

Utilisez ce paramètre pour spécifier le type de message (avertissement ou erreur) que SSMA affiche dans le volet de **sortie** ou de **liste d’erreurs** lorsqu’il ne peut pas résoudre un identificateur.

- Si vous sélectionnez **convertir et marquer avec un avertissement**, SSMA tente de convertir les références aux identificateurs non résolus et marque les instructions avec des commentaires d’avertissement.
- Si vous sélectionnez **marquer avec une erreur**, SSMA ne convertit pas les références aux identificateurs non résolus et marque les instructions avec des commentaires d’erreur.

Lorsque vous sélectionnez un mode de conversion dans la zone **mode** , SSMA applique le paramètre suivant :

|Mode|Valeur|
|-|-|
|Default|Convertir et marquer avec un avertissement|
|Optimistic|Convertir et marquer avec un avertissement|
|Complète|Marquer avec une erreur|

## <a name="system-functions-section"></a>Section fonctions système

### <a name="charindex-function"></a>CHARINDEX (fonction)

Dans ASE, `CHARINDEX` retourne `NULL` uniquement si toutes les expressions d’entrée sont `NULL` . [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]/Azure SQL renverra `NULL` si une expression d’entrée est `NULL` .

- Pour utiliser le comportement ASE, sélectionnez **remplacer la fonction**. Tous les appels à `CHARINDEX` la fonction sont remplacés par un appel à `CHARINDEX_VARCHAR` ou à  `CHARINDEX_NVARCHAR` la fonction définie par l’utilisateur en fonction du type de paramètres transmis (créé dans la base de données utilisateur sous le nom de schéma `s2ss` ) pour émuler le comportement de SAP ASE.
- Pour utiliser le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] comportement SQL/Azure, sélectionnez **conserver la syntaxe actuelle**.

Lorsque vous sélectionnez un mode de conversion dans la zone **mode** , SSMA applique le paramètre suivant :

|Mode|Valeur|
|-|-|
|Default|Conserver la syntaxe actuelle|
|Optimistic|Conserver la syntaxe actuelle|
|Complète|Replace, fonction|
  
### <a name="datalength-function"></a>DATALENGTH (fonction)

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]/Azure SQL et ASE diffèrent dans la valeur retournée par la `DATALENGTH` fonction lorsque la valeur est un espace unique. Dans ce cas, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] SQL/Azure retourne `0` et ASE retourne `1` .

- Pour utiliser le comportement ASE, sélectionnez **remplacer la fonction**. Tous les appels à la `DATALENGTH` fonction sont substitués par l' `CASE` expression pour émuler le comportement de SAP ASE.
- Pour utiliser le comportement par défaut de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] SQL/Azure, sélectionnez **conserver la syntaxe actuelle**.

Lorsque vous sélectionnez un mode de conversion dans la zone **mode** , SSMA applique le paramètre suivant :

|Mode|Valeur|
|-|-|
|Default|Conserver la syntaxe actuelle|
|Optimistic|Conserver la syntaxe actuelle|
|Complète|Replace, fonction|

### <a name="index_col-function"></a>INDEX_COL (fonction)

ASE prend en charge un `user_id` argument facultatif pour la `INDEX_COL` fonction ; Toutefois, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] SQL/Azure ne prend pas en charge cet argument. Si vous utilisez l' `user_id` argument, cette fonction ne peut pas être convertie en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] syntaxe SQL/Azure.

- Pour utiliser le comportement ASE, sélectionnez **convertir la fonction**. Si le code contient l' `user_id` argument, SSMA affiche une erreur.
- Pour afficher un message d’erreur chaque fois qu' `INDEX_COL` il est rencontré, sélectionnez **marquer avec une erreur**. SSMA ne convertit pas les références à la fonction et marque l’instruction avec des commentaires d’erreur.

|Mode|Valeur|
|-|-|
|Default|Marquer avec une erreur|
|Optimistic|Marquer avec une erreur|
|Complète|Marquer avec une erreur|

### <a name="index_colorder-function"></a>INDEX_COLORDER fonction)

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]/Azure SQL n’a pas de `INDEX_COLORDER` fonction système.

- Pour utiliser le comportement ASE, sélectionnez **convertir la fonction**. Tous les appels à `INDEX_COLORDER` la fonction sont substitués par un appel à une fonction définie par l’utilisateur portant le même nom `INDEX_COLORDER` (créé dans la base de données utilisateur sous le nom de schéma `s2ss` ) qui émule le comportement de SAP ASE.
- Pour imprimer un message d’erreur chaque fois qu' `INDEX_COLORDER` il est rencontré, sélectionnez **marquer avec une erreur**. SSMA ne convertit pas les références à la fonction et marque l’instruction avec des commentaires d’erreur.

Lorsque vous sélectionnez un mode de conversion dans la zone **mode** , SSMA applique le paramètre suivant :

|Mode|Valeur|
|-|-|
|Default|Marquer avec une erreur|
|Optimistic|Marquer avec une erreur|
|Complète|Marquer avec une erreur|

### <a name="left-and-right-functions"></a>Fonctions LEFT et RIGHT

`LEFT` les `RIGHT` fonctions et dans ASE se comportent différemment pour le paramètre de longueur négative.

- Pour utiliser le comportement ASE, sélectionnez **remplacer la fonction**. Le paramètre de longueur est ensuite remplacé par l' `CASE` expression qui retourne une `NULL` valeur négative.
- Pour utiliser le comportement de SQL Server, sélectionnez **conserver la syntaxe actuelle**.

Lorsque vous sélectionnez un mode de conversion dans la zone **mode** , SSMA applique le paramètre suivant :

|Mode|Valeur|
|-|-|
|Default|Conserver la syntaxe actuelle|
|Optimistic|Conserver la syntaxe actuelle|
|Complète|Replace, fonction|

> [!NOTE]
> Si le paramètre de longueur est une valeur littérale et non une expression complexe, la valeur de longueur est toujours remplacée par, `NULL` quel que soit le paramètre du projet.

### <a name="next_identity-function"></a>NEXT_IDENTITY fonction)

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]/Azure SQL n’a pas de `NEXT_IDENTITY` fonction système.

- Pour utiliser le comportement ASE, sélectionnez **convertir la fonction**. Tous les appels à `NEXT_IDENTITY` Function sont substitués par une expression `(IDENT_CURRENT(parameter Value) + IDENT_INCR(parameter Value)` qui émule le comportement de SAP ASE.
- Pour imprimer un message d’erreur chaque fois qu' `NEXT_IDENTITY` il est rencontré, sélectionnez **marquer avec une erreur**. SSMA ne convertit pas les références à la fonction et marque l’instruction avec des commentaires d’erreur.

Lorsque vous sélectionnez un mode de conversion dans la zone **mode** , SSMA applique le paramètre suivant :

|Mode|Valeur|
|-|-|
|Default|Marquer avec une erreur|
|Optimistic|Marquer avec une erreur|
|Complète|Marquer avec une erreur|

**Mode par défaut/optimiste/mode complet :** Marquer avec une erreur

### <a name="patindex-function"></a>PATINDEX, fonction

Spécifie s’il faut convertir la `PATINDEX` fonction pour qu’elle corresponde au comportement SAP ASE. Le point est que ASE supprime les espaces à droite dans un modèle de recherche. La solution de contournement consiste à convertir une expression de valeur en type de données de longueur fixe avec une précision maximale et une `rtrim` fonction d’application pour le modèle de recherche.

- Pour utiliser le comportement ASE, sélectionnez **utiliser**.  
- Pour utiliser le comportement par défaut de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] SQL/Azure, sélectionnez **ne pas utiliser**.

Lorsque vous sélectionnez un mode de conversion dans la zone **mode** , SSMA applique le paramètre suivant :

|Mode|Valeur|
|-|-|
|Default|À ne pas utiliser|
|Optimistic|À ne pas utiliser|
|Complète|Utilisation|

### <a name="replicate-function"></a>REPLICATE (fonction)

La `REPLICATE` fonction répète une chaîne le nombre de fois spécifié. Dans ASE, si vous spécifiez de répéter la chaîne à zéro heure, le résultat est `NULL` . Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] /Azure SQL, le résultat est une chaîne vide.

- Pour utiliser le comportement ASE, sélectionnez **remplacer la fonction**. Tous les appels à `REPLICATE` la fonction sont remplacés par un appel à `REPLICATE_VARCHAR` ou à `REPLICATE_NVARCHAR` la fonction définie par l’utilisateur en fonction du type de paramètres transmis (créé dans la base de données utilisateur sous le nom de schéma `s2ss` ) pour émuler le comportement de SAP ASE.
- Pour utiliser le comportement par défaut de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] SQL/Azure, sélectionnez **remplacer la fonction**.

Lorsque vous sélectionnez un mode de conversion dans la zone **mode** , SSMA applique le paramètre suivant :

|Mode|Valeur|
|-|-|
|Default|Replace, fonction|
|Optimistic|Replace, fonction|
|Complète|Replace, fonction|

### <a name="trim-ltrim-rtrim-function"></a>Fonction TRIM (LTRIM, RTRIM)

Ce paramètre spécifie s’il faut remplacer `TRIM` les appels à, `LTRIM` et `RTRIM` les fonctions avec les fonctions de syntaxe équivalentes à SAP ASE ou pour conserver la syntaxe actuelle. Les options suivantes sont disponibles pour ce paramètre particulier :

- **Replace, fonction**
- **Conserver la syntaxe actuelle**

Lorsque vous sélectionnez un mode de conversion dans la zone **mode** , SSMA applique le paramètre suivant :

|Mode|Valeur|
|-|-|
|Default|Replace, fonction|
|Optimistic|Replace, fonction|
|Complète|Replace, fonction|

### <a name="substring-function"></a>SUBSTRING (fonction)

Dans ASE, la fonction `SUBSTRING(expression, start, length)` retourne `NULL` si une valeur de début supérieure au nombre de caractères dans l’expression est spécifiée, ou si la longueur est égale à zéro. Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] /Azure SQL, l’expression équivalente retourne une chaîne vide.

- Pour utiliser le comportement ASE, sélectionnez **remplacer la fonction**. Tous les appels à `SUBSTRING` Function sont substitués par un appel à ou à une `SUBSTRING_VARCHAR` `SUBSTRING_NVARCHAR` `SUBSTRING_VARBINARY` fonction définie par l’utilisateur en fonction du type de paramètres transmis (créé dans la base de données utilisateur sous le nom de schéma `s2ss` ) pour ÉMULER le comportement de SAP ASE.
- Pour utiliser le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] comportement SQL/Azure, sélectionnez **conserver la syntaxe actuelle**.

Lorsque vous sélectionnez un mode de conversion dans la zone **mode** , SSMA applique le paramètre suivant :

|Mode|Valeur|
|-|-|
|Default|Conserver la syntaxe actuelle|
|Optimistic|Conserver la syntaxe actuelle|
|Complète|Replace, fonction|

## <a name="tables-section"></a>Section tables

### <a name="add-primary-key"></a>Ajouter une clé primaire

Crée une clé primaire dans la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] table ou SQL Azure si une table SAP ASE n’a pas de clé primaire ou d’index unique.

|Mode|Valeur|
|-|-|
|Default|Non|
|Optimistic|Non|
|Complète|Oui|

> [!NOTE]
> En cas de connexion à Azure SQL, la valeur par défaut est **Oui** .

## <a name="see-also"></a>Voir aussi

[Guide de référence de l’interface utilisateur (SybaseToSQL)](../../ssma/sybase/user-interface-reference-sybasetosql.md)
