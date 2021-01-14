---
title: Présentation de codes (SQLXML)
description: Découvrez la codes SQLXML 4,0 qui peut être utilisée pour insérer, mettre à jour ou supprimer des données dans une base de données.
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- explicit schema mapping [SQLXML]
- updategrams [SQLXML], mapping schema specifying
- namespaces [SQLXML]
- updategrams [SQLXML], about updategrams
- attribute-centric mapping
- invalid characters [SQLXML]
- element-centric mapping [SQLXML]
- mapping schema [SQLXML], updategrams
- namespaces [SQLXML], updategrams
- executing updategrams [SQLXML]
- implicit schema mapping
ms.assetid: cfe24e82-a645-4f93-ab16-39c21f90cce6
author: MightyPen
ms.author: genemi
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 6a416950247208c682d0fdc923db71357405fd0e
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170621"
---
# <a name="introduction-to-updategrams-sqlxml-40"></a>Présentation des codes de mise à jour (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  Vous pouvez modifier (insérer, mettre à jour ou supprimer) une base de données dans [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] à partir d’un document XML existant à l’aide d’un mise à jour ou de la [!INCLUDE[tsql](../../../includes/tsql-md.md)] fonction OpenXml.  
  
 La fonction OPENXML modifie une base de données en fragmentant le document XML existant et en fournissant un ensemble de lignes qui peut être passé à une instruction INSERT, UPDATE ou DELETE. Avec OPENXML, les opérations sont effectuées directement sur les tables de base de données. Par conséquent, OPENXML est particulièrement approprié partout où les fournisseurs d'ensembles de lignes, tels qu'une table, peuvent apparaître comme source.  
  
 Comme OPENXML, un code de mise à jour vous permet d'insérer, de mettre à jour ou de supprimer des données dans la base de données ; toutefois, un code de mise à jour fonctionne sur les vues XML fournies par le schéma XSD annoté (ou un XDR) ; par exemple, les mises à jour sont appliquées à la vue XML fournie par le schéma de mappage. Le schéma de mappage, à son tour, possède les informations nécessaires pour mapper des éléments et des attributs XML aux tables et colonnes de base de données correspondantes. Le code de mise à jour utilise ces informations de mappage pour mettre à jour les tables et colonnes de base de données.  
  
> [!NOTE]  
>  Cette documentation suppose une connaissance suffisante des modèles et de la prise en charge des schémas de mappage dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Pour plus d’informations, consultez [Introduction aux schémas XSD Annotés &#40;SQLXML 4,0&#41;](../../../relational-databases/sqlxml/annotated-xsd-schemas/introduction-to-annotated-xsd-schemas-sqlxml-4-0.md). Pour les applications héritées qui utilisent XDR, consultez [schémas XDR Annotés &#40;dépréciés dans SQLXML 4,0&#41;](../../../relational-databases/sqlxml/annotated-xsd-schemas/annotated-xdr-schemas-deprecated-in-sqlxml-4-0.md).  
  
## <a name="required-namespaces-in-the-updategram"></a>Espace de noms requis dans le code de mise à jour  
 Les mots clés d’un mise à jour, tels que **\<sync>** , **\<before>** et **\<after>** , existent dans l’espace de noms **urn : schemas-microsoft-com : XML-mise à jour** . Le préfixe d'espace de noms employé est arbitraire. Dans cette documentation, le préfixe **attribut updg** désigne l’espace de noms **mise à jour** .  
  
## <a name="reviewing-syntax"></a>Vérification de la syntaxe  
 Un mise à jour est un modèle avec **\<sync>** des **\<before>** blocs, et **\<after>** qui forment la syntaxe du mise à jour. Le code ci-dessous illustre cette syntaxe dans sa forme la plus simple :  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
  <updg:sync [mapping-schema= "AnnotatedSchemaFile.xml"] >  
    <updg:before>  
        ...  
    </updg:before>  
    <updg:after>  
        ...  
    </updg:after>  
  </updg:sync>  
</ROOT>  
```  
  
 Les définitions ci-dessous décrivent le rôle de chacun de ces blocs :  
  
 **\<before>**  
 Identifie l'état existant (également appelé « état before ») de l'instance de l'enregistrement.  
  
 **\<after>**  
 Identifie le nouvel état qu'auront les données après modification.  
  
 **\<sync>**  
 Contient les **\<before>** **\<after>** blocs et. Un **\<sync>** bloc peut contenir plusieurs jeux de **\<before>** **\<after>** blocs et. S’il y a plus d’un ensemble **\<before>** de **\<after>** blocs et, ces blocs (même s’ils sont vides) doivent être spécifiés en tant que paires. En outre, un mise à jour peut avoir plusieurs **\<sync>** blocs. Chaque **\<sync>** bloc est une unité de transaction (ce qui signifie que tous les éléments du **\<sync>** bloc sont exécutés ou que rien n’est fait). Si vous spécifiez plusieurs **\<sync>** blocs dans un mise à jour, la défaillance d’un **\<sync>** bloc n’affecte pas les autres **\<sync>** blocs.  
  
 Le fait qu’un mise à jour supprime, insère ou met à jour une instance d’enregistrement dépend du contenu des **\<before>** **\<after>** blocs et :  
  
-   Si une instance d’enregistrement apparaît uniquement dans le **\<before>** bloc sans instance correspondante dans le **\<after>** bloc, mise à jour effectue une opération de suppression.  
  
-   Si une instance d’enregistrement apparaît uniquement dans le **\<after>** bloc sans instance correspondante dans le **\<before>** bloc, il s’agit d’une opération d’insertion.  
  
-   Si une instance d’enregistrement apparaît dans le **\<before>** bloc et a une instance correspondante dans le **\<after>** bloc, il s’agit d’une opération de mise à jour. Dans ce cas, mise à jour met à jour l’instance d’enregistrement avec les valeurs spécifiées dans le **\<after>** bloc.  
  
## <a name="specifying-a-mapping-schema-in-the-updategram"></a>Spécification d'un schéma de mappage dans le code de mise à jour  
 Dans un code de mise à jour, l'abstraction XML fournie par un schéma de mappage (les schémas XSD et XDR sont pris en charge) peut être implicite ou explicite (autrement dit, un code de mise à jour peut fonctionner avec ou sans schéma de mappage spécifié). Si vous ne spécifiez pas de schéma de mappage, le mise à jour suppose un mappage implicite (le mappage par défaut), où chaque élément du **\<before>** bloc ou bloc **\<after>** est mappé à une table et l’élément enfant ou l’attribut enfant de chaque élément est mappé à une colonne de la base de données. Si vous spécifiez explicitement un schéma de mappage, les éléments et les attributs dans le code de mise à jour doivent correspondre aux éléments et aux attributs dans le schéma de mappage.  
  
### <a name="implicit-default-mapping"></a>Mappage implicite (par défaut)  
 Dans la plupart des cas, un code de mise à jour qui effectue des mises à jour simples peut ne pas requérir de schéma de mappage. Dans ce cas, le code de mise à jour compte sur le schéma de mappage par défaut.  
  
 Le code de mise à jour ci-dessous illustre un mappage implicite. Dans cet exemple, le code de mise à jour insère un nouveau client dans la table Sales.Customer. Étant donné que ce mise à jour utilise le mappage implicite, l' \<Sales.Customer> élément est mappé à la table Sales. Customer, et les attributs CustomerID et SalesPersonID sont mappés aux colonnes correspondantes de la table Sales. Customer.  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
<updg:sync >  
<updg:before>  
</updg:before>  
<updg:after>  
    <Sales.Customer CustomerID="1" SalesPersonID="277" />  
    </updg:after>  
</updg:sync>  
</ROOT>  
```  
  
### <a name="explicit-mapping"></a>Mappage explicite  
 Si vous spécifiez un schéma de mappage (XSD ou XDR), le code de mise à jour utilise le schéma pour déterminer les tables et colonnes de base de données qui seront mises à jour.  
  
 Si le mise à jour effectue une mise à jour complexe (par exemple, en insérant des enregistrements dans plusieurs tables sur la base de la relation parent-enfant spécifiée dans le schéma de mappage), vous devez fournir explicitement le schéma de mappage à l’aide de l’attribut **mapping-schema** sur lequel mise à jour s’exécute.  
  
 Comme un code de mise à jour est un modèle, le chemin d'accès spécifié pour le schéma de mappage dans le code de mise à jour est relatif à l'emplacement du fichier modèle (relatif à l'emplacement où le code de mise à jour est stocké). Pour plus d’informations, consultez [spécification d’un schéma de mappage annoté dans un mise à jour &#40;SQLXML 4,0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/updategrams/specifying-an-annotated-mapping-schema-in-an-updategram-sqlxml-4-0.md).  
  
## <a name="element-centric-and-attribute-centric-mapping-in-updategrams"></a>Mappage centré sur les éléments et mappage centré sur les attributs dans les codes de mise à jour  
 Avec le mappage par défaut (lorsque le schéma de mappage n'est pas spécifié dans le code de mise à jour), les éléments de code de mise à jour sont mappés à des tables et les éléments enfants (dans le cas du mappage centré sur les éléments) et les attributs (dans le cas du mappage centré sur les attributs) sont mappés à des colonnes.  
  
### <a name="element-centric-mapping"></a>Mappage centré sur les éléments  
 Dans un code de mise à jour centré sur les éléments, un élément contient des éléments enfants qui désignent les propriétés de l'élément. Pour bénéficier d'un exemple, reportez-vous au code de mise à jour ci-dessous. L' **\<Person.Contact>** élément contient les **\<FirstName>** **\<LastName>** éléments enfants et. Ces éléments enfants sont des propriétés de l' **\<Person.Contact>** élément.  
  
 Étant donné que ce mise à jour ne spécifie pas de schéma de mappage, mise à jour utilise le mappage implicite, où l' **\<Person.Contact>** élément est mappé à la table Person. contact et ses éléments enfants sont mappés aux colonnes FirstName et LastName.  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
<updg:sync >  
  <updg:after>  
    <Person.Contact>  
       <FirstName>Catherine</FirstName>  
       <LastName>Abel</LastName>  
    </Person.Contact>  
  </updg:after>  
</updg:sync>  
</ROOT>  
```  
  
### <a name="attribute-centric-mapping"></a>mappage centré sur les attributs  
 Dans un mappage centré sur les attributs, les éléments ont des attributs. Le code de mise à jour ci-dessous utilise le mappage centré sur les attributs. Dans cet exemple, l' **\<Person.Contact>** élément se compose des attributs **FirstName** et **LastName** . Ces attributs sont les propriétés de l' **\<Person.Contact>** élément. Comme dans l’exemple précédent, ce mise à jour ne spécifie aucun schéma de mappage. il s’appuie donc sur le mappage implicite pour mapper l' **\<Person.Contact>** élément à la table Person. contact et les attributs de l’élément aux colonnes respectives dans la table.  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
<updg:sync >  
  <updg:before>  
  </updg:before>  
  <updg:after>  
    <Person.Contact FirstName="Catherine" LastName="Abel" />  
  </updg:after>  
</updg:sync>  
</ROOT>  
```  
  
### <a name="using-both-element-centric-and-attribute-centric-mapping"></a>Utilisation du mappage centré sur les éléments et du mappage centré sur les attributs  
 Vous pouvez spécifier une association du mappage centré sur les éléments et du mappage centré sur les attributs, comme l'illustre le code de mise à jour ci-dessous. Notez que l' **\<Person.Contact>** élément contient à la fois un attribut et un élément enfant. Également, ce code de mise à jour s'appuie sur un mappage implicite. Ainsi, l’attribut **FirstName** et l' **\<LastName>** élément enfant sont mappés aux colonnes correspondantes de la table Person. contact.  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
<updg:sync >  
  <updg:before>  
  </updg:before>  
  <updg:after>  
    <Person.Contact FirstName="Catherine" >  
       <LastName>Abel</LastName>  
    </Person.Contact>  
  </updg:after>  
</updg:sync>  
</ROOT>  
```  
  
## <a name="working-with-characters-valid-in-sql-server-but-not-valid-in-xml"></a>Utilisation des caractères valides dans SQL Server mais non valides dans XML  
 Dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], les noms des tables peuvent inclure un espace. Toutefois, ce type de nom de table n'est pas valide dans XML.  
  
 Pour encoder des caractères qui sont des [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] identificateurs valides, mais qui ne sont pas des identificateurs XML valides, utilisez' __xHHHH \_ \_ 'comme valeur d’encodage, où HHHH représente le code UCS-2 hexadécimal à quatre chiffres du caractère dans l’ordre binaire le plus significatif. À l’aide de ce schéma d’encodage, un espace est remplacé par x0020 (le code hexadécimal à quatre chiffres pour un espace); ainsi, le nom de la table [Order Details] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] est _x005B_Order_x0020_Details_x005D \_ en XML.  
  
 De même, vous devrez peut-être spécifier des noms d’éléments en trois parties, tels que \<[database].[owner].[table]> . Étant donné que les crochets ([et]) ne sont pas valides en XML, vous devez spécifier ce \<_x005B_database_x005D\_._x005B_owner_x005D\_._x005B_table_x005D\_> qui correspond à, où _x005B \_ est l’encodage du crochet gauche ([) et _x005D \_ est l’encodage du crochet droit (]).  
  
## <a name="executing-updategrams"></a>Exécution de codes de mise à jour  
 Comme un code de mise à jour est un modèle, tous les mécanismes de traitement d'un modèle s'appliquent aux codes de mise à jour. Pour SQLXML 4.0, vous pouvez exécuter un code de mise à jour d'une des façons suivantes :  
  
-   En le soumettant dans une commande ADO.  
  
-   En le soumettant sous la forme d'une commande OLE DB.  
  
## <a name="see-also"></a>Voir aussi  
 [Considérations sur la sécurité mise à jour &#40;SQLXML 4,0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/security/updategram-security-considerations-sqlxml-4-0.md)  
  
  
