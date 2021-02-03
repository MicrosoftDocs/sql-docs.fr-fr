---
description: SELECT - Clause FOR (Transact-SQL)
title: FOR, clause (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/08/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- FOR
- FOR CLAUSE
- FOR_TSQL
- FOR_CLAUSE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- XML option [SQL Server]
- BROWSE option
- FOR clause [Transact-SQL]
ms.assetid: 08a6f084-8f73-4f2a-bae4-3c7513dc99b9
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 99b47c3d47e7403e04cfff4b62ff5c1d78bb6071
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207548"
---
# <a name="select---for-clause-transact-sql"></a>SELECT - Clause FOR (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

La clause FOR vous permet de spécifier l’une des options suivantes pour les résultats de la requête.
  
-   Autoriser les mises à jour pendant la consultation des données de la requête dans un curseur de mode de navigation en spécifiant **FOR BROWSE**.  
  
-   Mettre les résultats de la requête au format XML en spécifiant **FOR XML**.  
  
-   Mettre les résultats de la requête au format JSON en spécifiant **FOR JSON**.  

![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
[ FOR { BROWSE | <XML> | <JSON>} ]  
  
<XML> ::=  
XML   
{   
    { RAW [ ( 'ElementName' ) ] | AUTO }   
    [   
        <CommonDirectivesForXML>   
        [ , { XMLDATA | XMLSCHEMA [ ( 'TargetNameSpaceURI' ) ] } ]   
        [ , ELEMENTS [ XSINIL | ABSENT ]   
    ]  
  | EXPLICIT   
    [   
        <CommonDirectivesForXML>   
        [ , XMLDATA ]   
    ]  
  | PATH [ ( 'ElementName' ) ]   
    [  
        <CommonDirectivesForXML>   
        [ , ELEMENTS [ XSINIL | ABSENT ] ]  
    ]  
}   
  
<CommonDirectivesForXML> ::=   
[ , BINARY BASE64 ]  
[ , TYPE ]  
[ , ROOT [ ( 'RootName' ) ] ]  
  
<JSON> ::=  
JSON   
{   
    { AUTO | PATH }   
    [   
        [ , ROOT [ ( 'RootName' ) ] ]  
        [ , INCLUDE_NULL_VALUES ]  
        [ , WITHOUT_ARRAY_WRAPPER ]  
    ]  
  
}
```
  
## <a name="for-browse"></a>FOR BROWSE

 BROWSE  
 Indique que des mises à jour sont autorisées pendant la consultation des données dans un curseur de mode de navigation DB-Library. Il est possible de naviguer dans une table d’une application si la table inclut une colonne **timestamp**, si elle possède un index unique, et si l’option FOR BROWSE se trouve à la fin des instructions SELECT envoyées à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
> [!NOTE]
> Il est impossible d’utiliser \<lock_hint> HOLDLOCK dans une instruction SELECT qui comporte l’option FOR BROWSE.
  
 FOR BROWSE ne peut pas apparaître dans des instructions SELECT qui sont jointes par l'opérateur UNION.  
  
> [!NOTE]
> Lorsque les colonnes clés d'index unique d'une table peuvent accepter les valeurs NULL, et si la table se trouve sur le côté intérieur d'une jointure externe, l'index n'est pas pris en charge par le mode de navigation.  
  
 Le mode de navigation vous permet d'analyser les lignes de la table [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et d'y mettre à jour les données, une ligne après l'autre. Pour accéder à une table [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans votre application en mode de navigation, vous devez utiliser l’une des deux options suivantes :  
  
-   L’instruction SELECT utilisée pour accéder aux données de la table [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doit se terminer par les mots clés **FOR BROWSE**. Quand vous activez l’option **FOR BROWSE** pour utiliser le mode de navigation, des tables temporaires sont créées.  
  
-   Vous devez exécuter l’instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] suivante pour activer le mode de navigation en utilisant l’option **NO_BROWSETABLE** :  
  
    ```sql
    SET NO_BROWSETABLE ON  
    ```  
  
     Quand vous activez l’option **NO_BROWSETABLE**, toutes les instructions SELECT se comportent comme si elles incluaient l’option **FOR BROWSE**. Toutefois, l’option **NO_BROWSETABLE** ne crée pas les tables temporaires que l’option **FOR BROWSE** utilise généralement pour envoyer les résultats à votre application.  
  
 Lorsque vous essayez d’accéder aux données de tables [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en mode de navigation à l’aide d’une requête SELECT contenant une instruction de jointure externe, et qu’un index unique est défini sur la table présente sur le côté interne d’une instruction de jointure externe, le mode de navigation ne prend pas en charge l’index unique. Le mode de navigation prend en charge l'index unique uniquement lorsque toutes les colonnes clés d'index unique peuvent accepter les valeurs NULL. Le mode de navigation ne prend pas en charge l'index unique si les conditions suivantes sont remplies :  
  
-   Vous essayez d’accéder aux données de tables [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en mode de navigation en utilisant une requête SELECT qui contient une instruction de jointure externe.  
  
-   Un index unique est défini sur la table qui est présente sur le côté intérieur d'une instruction de jointure externe.  
  
 Pour reproduire ce comportement en mode de navigation, procédez comme suit :  
  
1.  Dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], créez une base de données appelée SampleDB.  
  
2.  Dans la base de données SampleDB, créez une table tleft et une table tright contenant une seule colonne nommée c1. Définissez un index unique sur la colonne c1 dans la table tleft et configurez la colonne pour qu'elle accepte les valeurs NULL. Pour cela, exécutez les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] suivantes dans une fenêtre de requête appropriée :  
  
    ```sql
    CREATE TABLE tleft(c1 INT NULL UNIQUE) ;  
    GO   
    CREATE TABLE tright(c1 INT NULL) ;  
    GO  
    ```  
  
3.  Insérez plusieurs valeurs dans la table tleft et la table tright. Veillez à insérer une valeur NULL dans la table tleft. Pour cela, exécutez les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] suivantes dans la fenêtre de requête :  
  
    ```sql
    INSERT INTO tleft VALUES(2) ;  
    INSERT INTO tleft VALUES(NULL) ;  
    INSERT INTO tright VALUES(1) ;  
    INSERT INTO tright VALUES(3) ;  
    INSERT INTO tright VALUES(NULL) ;  
    GO  
    ```  
  
4.  Activez l’option **NO_BROWSETABLE**. Pour cela, exécutez les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] suivantes dans la fenêtre de requête :  
  
    ```sql
    SET NO_BROWSETABLE ON ;  
    GO  
    ```  
  
5.  Accédez aux données des tables tleft et tright en utilisant une instruction de jointure externe dans la requête SELECT. Vérifiez que la table tleft se trouve sur le côté intérieur de l'instruction de jointure externe. Pour cela, exécutez les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] suivantes dans la fenêtre de requête :  
  
    ```sql
    SELECT tleft.c1   
    FROM tleft   
    RIGHT JOIN tright   
    ON tleft.c1 = tright.c1   
    WHERE tright.c1 <> 2 ;
    ```  
  
     Remarquez la sortie suivante dans le volet Résultats :  
  
     c1  
  
     ---\-  
  
     NULL  
  
     NULL  
  
 Après avoir exécuté la requête SELECT pour accéder aux tables en mode de navigation, le jeu de résultats de la requête SELECT contient deux valeurs NULL pour la colonne c1 dans la table tleft à cause de la définition de l'instruction de jointure externe droite. Par conséquent, dans le jeu de résultats, vous ne pouvez pas distinguer les valeurs NULL provenant de la table de celles introduites par l'instruction de jointure externe droite. Vous pouvez recevoir des résultats incorrects si vous devez ignorer les valeurs NULL du jeu de résultats.  
  
> [!NOTE]
> Si les colonnes incluses dans l'index unique n'acceptent pas de valeurs NULL, toutes les valeurs NULL contenues dans le jeu de résultats ont été introduites par l'instruction de jointure externe droite.  
  
## <a name="for-xml"></a>FOR XML

 XML  
 Spécifie que les résultats d'une requête doivent être retournés sous la forme d'un document XML. L’un des modes XML suivants doit être indiqué : RAW, AUTO, EXPLICIT. Pour plus d’informations sur les données XML et sur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez [FOR XML &#40;SQL Server&#41;](../../relational-databases/xml/for-xml-sql-server.md).  
  
 RAW [ **('** _ElementName_ **')** ]  
 Prend le résultat de la requête et transforme chaque ligne du jeu de résultats en un élément XML avec un identificateur générique \<row /> comme balise d’élément. Vous pouvez éventuellement spécifier un nom pour l'élément de ligne. La sortie de code XML utilise le nom *ElementName* spécifié pour identifier l’élément de ligne généré pour chaque ligne. Pour plus d’informations, consultez [Utiliser le mode RAW avec FOR XML](../../relational-databases/xml/use-raw-mode-with-for-xml.md).
  
 AUTO  
 Renvoie les résultats de la requête dans une arborescence XML simple et imbriquée. Chaque table dans la clause FROM pour laquelle au moins une colonne existe dans la clause SELECT est représentée comme un élément XML. Les colonnes listées dans la clause SELECT sont mappées vers les attributs d'éléments appropriés. Pour plus d’informations, consultez [Utiliser le mode AUTO avec FOR XML](../../relational-databases/xml/use-auto-mode-with-for-xml.md).  
  
 EXPLICIT  
 Spécifie que la forme de l'arborescence XML résultante est définie de manière explicite. L'utilisation de ce mode nécessite toutefois que les requêtes soient écrites d'une manière particulière, de sorte que les informations complémentaires sur l'imbrication souhaitée soient spécifiées de manière explicite. Pour plus d’informations, consultez [Utiliser le mode EXPLICIT avec FOR XML](../../relational-databases/xml/use-explicit-mode-with-for-xml.md).  
  
 XMLDATA  
 Retourne un schéma XDR inclus, mais n'ajoute pas l'élément racine au résultat. Si XMLDATA est spécifié, le schéma XDR est ajouté au document.  
  
> [!IMPORTANT]
> La directive XMLDATA est **dépréciée**. Utilisez la génération XSD en mode RAW et AUTO. Il n'y a aucun remplacement pour la directive XMLDATA en mode EXPLICIT. [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  

_Supprimer les sauts de ligne indésirables :_ vous pouvez utiliser SSMS (SQL Server Management Studio) pour émettre une requête qui utilise la clause FOR XML. Il est arrive parfois qu’une grande chaîne XML soit retournée et affichée dans une cellule de grille. La longueur de cette chaîne XML peut être supérieure à celle d’une ligne unique dans une cellule de grille SSMS. Dans ce cas, SSMS peut insérer des caractères de fin de ligne entre les longs segments de la chaîne XML. De tels sauts de ligne peuvent se produire au milieu d’une sous-chaîne qui n’est pas censée être scindée en plusieurs lignes. Pour empêcher les sauts de ligne, vous pouvez utiliser un cast AS XMLDATA. Cette solution s’applique également quand vous utilisez FOR JSON PATH. La technique est décrite sur Stack Overflow et est indiquée dans l’instruction SELECT de l’exemple Transact-SQL suivant :

- [Using SQL Server FOR XML: Convert Result Datatype to Text/varchar/string whatever?](https://stackoverflow.com/questions/5655332/using-sql-server-for-xml-convert-result-datatype-to-text-varchar-string-whate/5658758#5658758)

    ```sql
    SELECT CAST(
        (SELECT column1, column2
            FROM my_table
            FOR XML PATH('')
        )
            AS VARCHAR(MAX)
    ) AS XMLDATA ;
    ```

<!-- The preceding Stack Overflow example is per MicrosoftDocs/sql-docs Issue 1501.  2019-01-06 -->

 XMLSCHEMA [ **('** _TargetNameSpaceURI_ **')** ]  
 Retourne le schéma XSD inclus. Lorsque vous spécifiez cette directive, vous pouvez éventuellement spécifier un URI d'espace de noms cible, qui retourne l'espace de noms spécifié dans le schéma. Pour plus d’informations, consultez [Générer un schéma XSD Inline](../../relational-databases/xml/generate-an-inline-xsd-schema.md).  
  
 ELEMENTS  
 Spécifie que les colonnes sont retournées sous la forme de sous-éléments. Sinon, elles sont mappées avec des attributs XML. Cette option est prise en charge dans les modes RAW, AUTO et PATH uniquement. Pour plus d’informations, consultez [Utiliser le mode RAW avec FOR XML](../../relational-databases/xml/use-raw-mode-with-for-xml.md).  
  
 XSINIL  
 Spécifie qu’un élément ayant un attribut **xsi:nil** défini à **True** doit être créé pour les valeurs de colonne NULL. Cette option peut uniquement être spécifiée avec la directive ELEMENTS. Pour plus d'informations, consultez les pages suivantes :

- [Générer des éléments pour des valeurs NULL avec le paramètre XSINIL](../../relational-databases/xml/generate-elements-for-null-values-with-the-xsinil-parameter.md).
- [Clause FOR XML sur l’instruction SELECT](../../relational-databases/xml/for-xml-sql-server.md)
  
 ABSENT  
 Indique que pour les valeurs de colonne NULL, les éléments XML correspondants ne seront pas ajoutés dans le résultat XML. Vous ne devez spécifier cette option qu'avec ELEMENTS.  
  
 PATH [ **('** _ElementName_ **')** ]  
 Génère un élément wrapper \<row> pour chaque ligne du jeu de résultats. Vous pouvez éventuellement spécifier un nom d’élément pour l’élément wrapper \<row>. Si une chaîne vide est fournie, comme FOR XML PATH ( **''** ) ), aucun élément wrapper n’est généré. L'utilisation de PATH peut constituer une solution plus simple pour les requêtes écrites à l'aide de la directive EXPLICIT. Pour plus d’informations, consultez [Utiliser le mode PATH avec FOR XML](../../relational-databases/xml/use-path-mode-with-for-xml.md).  
  
 BINARY BASE64  
 Spécifie que la requête retourne les données binaires au format binaire encodé base64. Cette option doit être spécifiée lors de l'extraction de données binaires en mode RAW et EXPLICIT. Cette option est utilisée par défaut en mode AUTO.  
  
 TYPE  
 Spécifie que la requête retourne des résultats de type **xml**. Pour plus d’informations, consultez [Directive TYPE dans les requêtes FOR XML](../../relational-databases/xml/type-directive-in-for-xml-queries.md).  
  
 ROOT [ **('** _RootName_ **')** ]  
 Spécifie qu'un élément de premier niveau unique doit être ajouté au XML résultant. Vous pouvez, si vous le souhaitez, spécifier le nom d'élément racine à générer. Si le nom de racine facultatif n’est pas spécifié, l’élément \<root> par défaut est ajouté.  
  
 Pour plus d’informations, consultez [FOR XML &#40;SQL Server&#41;](../../relational-databases/xml/for-xml-sql-server.md).  
  
 **Exemple FOR XML**  
  
 L'exemple suivant spécifie `FOR XML AUTO` avec les options `TYPE` et `XMLSCHEMA`. Comme l’option `TYPE` a été spécifiée, le jeu de résultats est retourné au client comme un type **xml**. L'option `XMLSCHEMA` spécifie que le schéma XSD inclus est intégré aux données XML retournées, et l'option `ELEMENTS` spécifie que le résultat XML est centré sur les éléments.  
  
```sql
USE AdventureWorks2012;  
GO  
SELECT p.BusinessEntityID, FirstName, LastName, PhoneNumber AS Phone  
FROM Person.Person AS p  
JOIN Person.PersonPhone AS pph ON p.BusinessEntityID  = pph.BusinessEntityID  
WHERE LastName LIKE 'G%'  
ORDER BY LastName, FirstName   
FOR XML AUTO, TYPE, XMLSCHEMA, ELEMENTS XSINIL;  
```  
  
## <a name="for-json"></a>FOR JSON

 JSON  
 Spécifiez FOR JSON pour retourner les résultats d’une requête au format texte JSON. Vous devez également spécifier l’un des modes JSON suivants : AUTO ou PATH. Pour plus d’informations sur la clause **FOR JSON**, consultez [Mettre les résultats de requête au format JSON avec FOR JSON &#40;SQL Server&#41;](../../relational-databases/json/format-query-results-as-json-with-for-json-sql-server.md).  
  
 AUTO  
 Mettez la sortie JSON automatiquement en forme en fonction de la structure de l’instruction **SELECT**  
            en spécifiant **FOR JSON AUTO**. Pour obtenir plus d’informations et des exemples, consultez [Mettre en forme automatiquement la sortie JSON avec le mode AUTO &#40;SQL Server&#41;](../../relational-databases/json/format-json-output-automatically-with-auto-mode-sql-server.md).  
  
 PATH  
 Gardez le contrôle total du format de la sortie JSON en spécifiant   
            **FOR JSON PATH**. Le mode **PATH** vous permet de créer des objets wrapper et d’imbriquer des propriétés complexes. Pour obtenir plus d’informations et des exemples, consultez [Mettre en forme la sortie JSON imbriquée avec le mode PATH &#40;SQL Server&#41;](../../relational-databases/json/format-nested-json-output-with-path-mode-sql-server.md).  
  
 INCLUDE_NULL_VALUES  
 Incluez les valeurs NULL dans la sortie JSON en spécifiant l’option **INCLUDE_NULL_VALUES** avec la clause **FOR JSON**. Si vous ne spécifiez pas cette option, la sortie n’inclut pas les propriétés JSON pour les valeurs NULL dans les résultats de la requête. Pour obtenir plus d’informations et des exemples, consultez[Inclure des valeurs NULL dans une sortie JSON avec l’option INCLUDE_NULL_VALUES &#40;SQL Server&#41;](../../relational-databases/json/include-null-values-in-json-include-null-values-option.md).  
  
 ROOT [ **('** _RootName_ **')** ]  
 Ajoutez un élément racine unique à la sortie JSON en spécifiant l’option **ROOT** avec la clause **FOR JSON**. Si vous ne spécifiez pas l’option **ROOT** , la sortie JSON n'aura pas d’élément racine. Pour obtenir plus d’informations et des exemples, consultez [Ajouter un nœud racine à la sortie JSON avec l’option ROOT &#40;SQL Server&#41;](../../relational-databases/json/add-a-root-node-to-json-output-with-the-root-option-sql-server.md).  
  
 WITHOUT_ARRAY_WRAPPER  
 Supprimez les crochets qui entourent par défaut la sortie JSON en spécifiant l’option **WITHOUT_ARRAY_WRAPPER** avec la clause **FOR JSON**. Si vous ne spécifiez pas cette option, la sortie JSON est placée entre crochets. Utilisez l’option **WITHOUT_ARRAY_WRAPPER** pour générer un seul objet JSON en sortie.  Pour plus d’informations, consultez [Supprimer les crochets de la sortie JSON avec l’option WITHOUT_ARRAY_WRAPPER &#40;SQL Server&#41;](../../relational-databases/json/remove-square-brackets-from-json-without-array-wrapper-option.md).  
  
 Pour plus d’informations, consultez [Mettre les résultats de requête au format JSON avec FOR JSON &#40;SQL Server&#41;](../../relational-databases/json/format-query-results-as-json-with-for-json-sql-server.md).  
  
## <a name="see-also"></a>Voir aussi

 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)

