---
title: Contexte d’expression et évaluation de requête (XQuery) | Microsoft Docs
description: Découvrez comment les informations du contexte statique et dynamique d’une expression XQuery sont utilisées pour l’analyser et l’évaluer.
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- expression context [XQuery]
- XQuery, expression context
- XQuery, query evaluation
- static context
- dynamic context [XQuery]
ms.assetid: 5059f858-086a-40d4-811e-81fedaa18b06
author: rothja
ms.author: jroth
ms.openlocfilehash: f4fd85737d1a7cdc981576f624a2a403f0eb19ee
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341956"
---
# <a name="expression-context-and-query-evaluation-xquery"></a>Contexte des expressions et évaluation des requêtes (XQuery)
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Le contexte d'une expression rassemble les informations utilisées pour l'analyser et l'évaluer. Voici les deux phases au cours desquelles XQuery est évalué :  
  
-   **Contexte statique** : il s’agit de la phase de compilation de la requête. En fonction des informations disponibles, des erreurs sont quelquefois générées au cours de l'analyse statique de la requête.  
  
-   **Contexte dynamique** : il s’agit de la phase d’exécution de la requête. Même si elle ne présente pas d'erreurs statiques (erreurs survenues lors de la compilation de la requête), la requête peut renvoyer des erreurs au cours de son exécution.  
  
## <a name="static-context"></a>Contexte statique  
 L'initialisation du contexte statique consiste à rassembler toutes les informations requises par l'analyse statique de l'expression. L'initialisation du contexte statique comprend les étapes suivantes :  
  
-   La stratégie d' **espace blanc de limite** est définie sur Strip. Par conséquent, l’espace blanc de la limite n’est pas conservé par les constructeurs d' **élément** et d' **attribut** de la requête. Par exemple :  
  
    ```  
    declare @x xml  
    set @x=''  
    select @x.query('<a>  {"Hello"}  </a>,  
  
        <b> {"Hello2"}  </b>')  
    ```  
  
     Cette requête renvoie le résultat suivant puisque l'espace limite est ignoré lors de l'analyse de l'expression XQuery :  
  
    ```  
    <a>Hello</a><b>Hello2</b>  
    ```  
  
-   Le préfixe et l'espace de noms de liaison sont initialisés pour les éléments suivants :  
  
    -   Un ensemble d'espaces de noms prédéfinis.  
  
    -   Tous les espaces de noms définis à l'aide de WITH XMLNAMESPACES. Pour plus d’informations, consultez [Ajouter des espaces de noms aux requêtes avec WITH XMLNAMESPACES](../relational-databases/xml/add-namespaces-to-queries-with-with-xmlnamespaces.md).  
  
    -   Tous les espaces de noms définis dans le prologue de la requête. Remarquez que les déclarations d'espace de noms du prologue peuvent remplacer la déclaration d'espace de noms de WITH XMLNAMESPACES. Par exemple, dans la requête suivante, WITH XMLNAMESPACES déclare un préfixe (PD) qui le lie à l’espace de noms ( `https://someURI` ). En revanche, dans la clause WHERE, le prologue de la requête remplace la liaison.  
  
        ```  
        WITH XMLNAMESPACES ('https://someURI' AS pd)  
        SELECT ProductModelID, CatalogDescription.query('  
            <Product   
                ProductModelID= "{ sql:column("ProductModelID") }"   
                />  
        ') AS Result  
        FROM Production.ProductModel  
        WHERE CatalogDescription.exist('  
            declare namespace  pd="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
             /pd:ProductDescription[(pd:Specifications)]'  
            ) = 1  
        ```  
  
     Toutes les liaisons d'espace de noms sont résolues au cours de l'initialisation du contexte statique.  
  
-   En cas d’interrogation d’une colonne ou d’une variable **XML** typée, les composants de la collection de schémas XML associée à la colonne ou à la variable sont importés dans le contexte statique. Pour plus d’informations, consultez [Comparer du XML typé et du XML non typé](../relational-databases/xml/compare-typed-xml-to-untyped-xml.md).  
  
-   Pour chaque type atomique des schémas importés, une fonction de conversion est également mise à disposition dans le contexte statique. L'exemple suivant illustre ce concept. Dans cet exemple, une requête est spécifiée par rapport à une variable **XML** typée. La collection de schémas XML associée à cette variable définit un type atomique, myType. Correspondant à ce type, une fonction de conversion, **MyType ()**, est disponible pendant l’analyse statique. L'expression de la requête (`ns:myType(0)`) renvoie une valeur de type myType.  
  
    ```  
    -- DROP XML SCHEMA COLLECTION SC  
    -- go  
    CREATE XML SCHEMA COLLECTION SC AS '<schema xmlns="http://www.w3.org/2001/XMLSchema"   
    targetNamespace="myNS" xmlns:ns="myNS"  
    xmlns:s="https://schemas.microsoft.com/sqlserver/2004/sqltypes">  
          <import namespace="https://schemas.microsoft.com/sqlserver/2004/sqltypes"/>  
          <simpleType name="myType">  
                <restriction base="int">  
                 <enumeration value="0" />  
                  <enumeration value="1"/>  
                </restriction>  
          </simpleType>  
          <element name="root" type="ns:myType"/>  
    </schema>'  
    go  
  
    DECLARE @var XML(SC)  
    SET @var = '<root xmlns="myNS">0</root>'  
    -- specify myType() casting function in the query  
    SELECT @var.query('declare namespace ns="myNS"; ns:myType(0)')  
    ```  
  
     Dans l’exemple suivant, la fonction de conversion du type XML intégré **int** est spécifiée dans l’expression.  
  
    ```  
    declare @x xml  
    set @x = ''  
    select @x.query('xs:int(5)')  
    go  
    ```  
  
 Après l'initialisation du contexte statique, l'expression de requête est analysée (compilée). L'analyse statique implique les étapes suivantes :  
  
1.  Analyse de la requête.  
  
2.  Résolution des noms de fonction et de type spécifiés dans l'expression.  
  
3.  Typage statique de la requête. De cette façon, le type de la requête est sûr. Par exemple, la requête suivante retourne une erreur statique, car l' **+** opérateur requiert des arguments de type primitifs numériques :  
  
    ```  
    declare @x xml  
    set @x=''  
    SELECT @x.query('"x" + 4')  
    ```  
  
     Dans l’exemple suivant, l’opérateur **value ()** requiert un singleton. Comme spécifié dans le schéma XML, il peut y avoir plusieurs \<Elem> éléments. L'analyse statique de l'expression détermine que le type n'est pas sûr et une erreur statique est générée. Pour corriger cette erreur, vous devez réécrire l'expression de façon à spécifier explicitement un singleton (`data(/x:Elem)[1]`).  
  
    ```  
    DROP XML SCHEMA COLLECTION SC  
    go  
    CREATE XML SCHEMA COLLECTION SC AS '<schema xmlns="http://www.w3.org/2001/XMLSchema"   
    targetNamespace="myNS" xmlns:ns="myNS"  
    xmlns:s="https://schemas.microsoft.com/sqlserver/2004/sqltypes">  
          <import namespace="https://schemas.microsoft.com/sqlserver/2004/sqltypes"/>  
          <element name="Elem" type="string"/>  
    </schema>'  
    go  
  
    declare @x xml (SC)  
    set @x='<Elem xmlns="myNS">test</Elem><Elem xmlns="myNS">test2</Elem>'  
    SELECT @x.value('declare namespace x="myNS"; data(/x:Elem)[1]','varchar(20)')  
    ```  
  
     Pour plus d’informations, consultez [XQuery et typage statique](../xquery/xquery-and-static-typing.md).  
  
### <a name="implementation-restrictions"></a>Limites de la mise en œuvre  
 Voici les limites liées à un contexte statique :  
  
-   Le mode de compatibilité XPath n'est pas pris en charge.  
  
-   En cas de construction XML, seul le mode de construction strip est pris en charge. Il s'agit du paramètre par défaut. Par conséquent, le type du nœud d’élément construit est **xdt :** le type non typé et les attributs sont de type **xdt : untypedAtomic** .  
  
-   Seul le mode de classement ordered est pris en charge.  
  
-   Seule la stratégie d'espaces XML strip est prise en charge.  
  
-   La fonctionnalité URI de base n'est pas prise en charge.  
  
-   **FN : doc ()** n’est pas pris en charge.  
  
-   **FN : collection ()** n’est pas pris en charge.  
  
-   XQuery Static Flagger n'est pas fourni.  
  
-   Le classement associé au type de données **XML** est utilisé. Ce classement est toujours paramétré sur le classement des points de code Unicode.  
  
## <a name="dynamic-context"></a>Contexte dynamique  
 Le contexte dynamique fait référence aux informations dont vous devez disposer au moment de l'exécution de l'expression. Outre le contexte statique, les informations suivantes sont initialisées comme partie du contexte dynamique :  
  
-   Le focus de l'expression tel que l'élément contextuel, la position contextuelle et la taille du contexte, est initialisé comme le montre l'exemple suivant. Notez que toutes ces valeurs peuvent être remplacées par la [méthode nodes ()](../t-sql/xml/nodes-method-xml-data-type.md).  
  
    -   Le type de données **XML** définit l’élément de contexte, le nœud en cours de traitement, sur le nœud de document.  
  
    -   La position contextuelle, c'est-à-dire la position de l'élément contextuel par rapport aux nœuds à traiter, est tout d'abord définie à 1.  
  
    -   La taille du contexte, c'est-à-dire le nombre d'éléments de la séquence à traiter, est tout d'abord définie à 1 puisqu'il y a toujours un seul nœud de document.  
  
### <a name="implementation-restrictions"></a>Limites de la mise en œuvre  
 Voici les limites liées à un contexte dynamique :  
  
-   Les fonctions de contexte de **date et d’heure actuelles** , **FN : current-date**, **FN : Current-Time** et **FN : Current-DateTime**, ne sont pas prises en charge.  
  
-   Le **fuseau horaire implicite** est fixé à l’heure UTC + 0 et ne peut pas être modifié.  
  
-   La fonction **FN : doc ()** n’est pas prise en charge. Toutes les requêtes sont exécutées sur des colonnes de type **XML** ou des variables.  
  
-   La fonction **FN : collection ()** n’est pas prise en charge.  
  
## <a name="see-also"></a>Voir aussi  
 [Notions de base de XQuery](../xquery/xquery-basics.md)   
 [Comparer du XML typé et du XML non typé](../relational-databases/xml/compare-typed-xml-to-untyped-xml.md)   
 [Collections de schémas XML &#40;SQL Server&#41;](../relational-databases/xml/xml-schema-collections-sql-server.md)  
  
  
