---
description: Section de schéma
title: Section schéma | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Schema section [ADO]
ms.assetid: 4ac6e524-2c92-48e8-b871-0a4b5c8fda18
author: rothja
ms.author: jroth
ms.openlocfilehash: 346957b2b060bf990cf77bfe7784f67b13a37c37
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037009"
---
# <a name="schema-section"></a>Section de schéma
La section du schéma est obligatoire. Comme le montre l’exemple précédent, ADO écrit des métadonnées détaillées sur chaque colonne pour conserver la sémantique des valeurs de données le plus possible pour la mise à jour. Toutefois, pour charger dans le XML, ADO requiert uniquement les noms des colonnes et l’ensemble de lignes auquel ils appartiennent. Voici un exemple de schéma minimal :  
  
```  
<xml xmlns:s="uuid:BDC6E3F0-6DA3-11d1-A2A3-00AA00C14882"  
    xmlns:rs="urn:schemas-microsoft-com:rowset"  
    xmlns:z="#RowsetSchema">  
  <s:Schema id="RowsetSchema">  
    <s:ElementType name="row" content="eltOnly">  
      <s:AttributeType name="ShipperID"/>  
      <s:AttributeType name="CompanyName"/>  
      <s:AttributeType name="Phone"/>  
      <s:Extends type="rs:rowbase"/>  
    </s:ElementType>  
  </s:Schema>  
  <rs:data>  
...  
  </rs:data>  
</xml>  
```  
  
 Dans l’exemple précédent, ADO traite les données comme des chaînes de longueur variable, car aucune information de type n’est incluse dans le schéma.  
  
## <a name="creating-aliases-for-column-names"></a>Création d’alias pour les noms de colonnes  
 L’attribut RS : Name vous permet de créer un alias pour un nom de colonne afin qu’un nom convivial puisse apparaître dans les informations de colonne exposées par l’ensemble de lignes et un nom plus petit peut être utilisé dans la section de données. Par exemple, le schéma précédent peut être modifié pour mapper ShipperID à S1, CompanyName à S2 et téléphone à S3 comme suit :  
  
```  
<s:Schema id="RowsetSchema">   
<s:ElementType name="row" content="eltOnly" rs:updatable="true">   
<s:AttributeType name="s1" rs:name="ShipperID" rs:number="1" ...>   
...  
</s:AttributeType>   
<s:AttributeType name="s2" rs:name="CompanyName" rs:number="2" ...>   
...  
</s:AttributeType>   
<s:AttributeType name="s3" rs:name="Phone" rs:number="3" ...>   
...  
</s:AttributeType>   
...  
</s:ElementType>   
</s:Schema>  
```  
  
 Ensuite, dans la section des données, la ligne utilise l’attribut Name (et non RS : Name) pour faire référence à cette colonne :  
  
```  
"<row s1="1" s2="Speedy Express" s3="(503) 555-9831"/>  
```  
  
 La création d’alias pour les noms de colonnes est requise chaque fois qu’un nom de colonne n’est pas un attribut ou un nom de balise valide en XML. Par exemple, « LastName » doit avoir un alias, car les noms avec des espaces incorporés sont des attributs non valides. La ligne suivante n’est pas correctement gérée par l’analyseur XML. vous devez donc créer un alias pour un autre nom qui ne dispose pas d’un espace incorporé.  
  
```  
<row last name="Jones"/>  
```  
  
 La valeur que vous utilisez pour l’attribut name doit être utilisée de façon cohérente à chaque endroit où la colonne est référencée dans les sections Schema and Data (schéma et données) du document XML. L’exemple suivant illustre l’utilisation cohérente de S1 :  
  
```  
<s:Schema id="RowsetSchema">  
  <s:ElementType name="row" content="eltOnly">  
    <s:attribute type="s1"/>  
    <s:attribute type="CompanyName"/>  
    <s:attribute type="s3"/>  
    <s:extends type="rs:rowbase"/>  
  </s:ElementType>  
  <s:AttributeType name="s1" rs:name="ShipperID" rs:number="1"   
    rs:maydefer="true" rs:writeunknown="true">  
    <s:datatype dt:type="i4" dt:maxLength="4" rs:precision="10"   
      rs:fixedlength="true" rs:maybenull="true"/>  
  </s:AttributeType>  
</s:Schema>  
<rs:data>  
  <z:row s1="1" CompanyName="Speedy Express" s3="(503) 555-9831"/>  
</rs:data>  
```  
  
 De même, étant donné qu’aucun alias n’est défini pour `CompanyName` dans l’exemple précédent, `CompanyName` doit être utilisé de manière cohérente dans tout le document.  
  
## <a name="data-types"></a>Types de données  
 Vous pouvez appliquer un type de données à une colonne avec l’attribut dt : type. Pour obtenir le guide définitif sur les types XML autorisés, consultez la section types de données de la [spécification du XML-Data W3C](http://www.w3.org/TR/1998/NOTE-XML-data/). Vous pouvez spécifier un type de données de deux façons : spécifiez l’attribut dt : type directement sur la définition de colonne elle-même ou utilisez la construction s :DataType comme élément imbriqué de la définition de colonne. Par exemple,  
  
```  
<s:AttributeType name="Phone" >  
  <s:datatype dt:type="string"/>  
</s:AttributeType>  
```  
  
 équivaut à :  
  
```  
<s:AttributeType name="Phone" dt:type="string"/>  
```  
  
 Si vous omettez l’attribut dt : type entièrement à partir de la définition de ligne, par défaut, le type de la colonne sera une chaîne de longueur variable.  
  
 Si vous avez plus d’informations de type que le simple nom de type (par exemple, DT : maxLength), il est plus lisible d’utiliser l’élément enfant s :DataType. Il s’agit simplement d’une convention, mais pas d’une exigence.  
  
 Les exemples suivants montrent comment inclure des informations de type dans votre schéma.  
  
```  
<!-- 1. String with no max length -->  
<s:AttributeType name="title_id"/>  
<!-or -->  
<s:AttributeType name="title_id" dt:type="string"/>  
  
<!-- 2. Fixed length string with max length of 6 -->  
<s:AttributeType name="title_id">  
    <s:datatype dt:type="string" dt:maxLength="6" rs:fixedlength="true" />  
</s:AttributeType>  
  
<!-- 3. Variable length string with max length of 6 -->  
<s:AttributeType name="title_id">  
    <s:datatype dt:type="string" dt:maxLength="6" />  
</s:AttributeType>  
  
<!-- 4. Integer -->  
<s:AttributeType name="title_id" dt:type="int"/>  
```  
  
 Le deuxième exemple présente une utilisation subtile de l’attribut RS : multiple. Une colonne avec l’attribut RS : multiple ayant la valeur true signifie que les données doivent avoir la longueur définie dans le schéma. Dans ce cas, une valeur valide pour title_id est « 123456 », car « 123 ». Toutefois, « 123 » n’est pas valide, car sa longueur est égale à 3, et non pas à 6. Pour une description plus complète de la propriété multiple, consultez le Guide du programmeur OLE DB.  
  
## <a name="handling-nulls"></a>Gestion des valeurs null  
 Les valeurs NULL sont gérées par l’attribut RS : MAYBENULL. Si cet attribut a la valeur true, le contenu de la colonne peut contenir une valeur null. En outre, si la colonne est introuvable dans une ligne de données, l’utilisateur qui lit les données à partir de l’ensemble de lignes obtiendra un État null à partir de IRowset :: GetData (). Considérez les définitions de colonnes suivantes de la table Shippers.  
  
```  
<s:AttributeType name="ShipperID">  
  <s:datatype dt:type="int" dt:maxLength="4"/>  
</s:AttributeType>  
<s:AttributeType name="CompanyName">  
  <s:datatype dt:type="string" dt:maxLength="40" rs:maybenull="true"/>  
</s:AttributeType>  
```  
  
 La définition autorise la valeur `CompanyName` null, mais elle `ShipperID` ne peut pas contenir de valeur null. Si la section de données contient la ligne suivante, le fournisseur de persistance définit l’état des données de la `CompanyName` colonne sur la constante d’état OLE DB DBSTATUS_S_ISNULL :  
  
```  
<z:row ShipperID="1"/>  
```  
  
 Si la ligne est entièrement vide, comme suit, le fournisseur de persistance retourne un État OLE DB DBSTATUS_E_UNAVAILABLE pour `ShipperID` et DBSTATUS_S_ISNULL pour CompanyName.  
  
```  
<z:row/>   
```  
  
 Notez qu’une chaîne de longueur nulle n’est pas la même que la valeur null.  
  
```  
<z:row ShipperID="1" CompanyName=""/>  
```  
  
 Pour la ligne précédente, le fournisseur de persistance retourne un état de OLE DB de DBSTATUS_S_OK pour les deux colonnes. `CompanyName`Dans ce cas, il s’agit simplement de «» (une chaîne de longueur nulle).  
  
 Pour plus d’informations sur les constructions de OLE DB disponibles pour une utilisation dans le schéma d’un document XML pour OLE DB, consultez la définition de « urn : schemas-microsoft-com : rowset » et le Guide du programmeur OLE DB.  
  
## <a name="see-also"></a>Voir aussi  
 [Persistance des enregistrements au format XML](../../../ado/guide/data/persisting-records-in-xml-format.md)
