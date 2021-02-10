---
description: Recordsets hiérarchiques dans XML
title: Jeux d’enregistrements hiérarchiques dans XML | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- hierarchical Recordsets [ADO], in XML
ms.assetid: 5d4b11c4-c94f-4910-b99b-5b9abc50d791
author: rothja
ms.author: jroth
ms.openlocfilehash: 9919489df58853a3fa8eea3711583cdf57a0849f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100033709"
---
# <a name="hierarchical-recordsets-in-xml"></a>Recordsets hiérarchiques dans XML
ADO permet la persistance d’objets Recordset hiérarchiques en XML. Avec les objets Recordset hiérarchiques, la valeur d’un champ dans le jeu d’enregistrements parent est un autre Recordset. De tels champs sont représentés en tant qu’éléments enfants dans le flux XML plutôt qu’en tant qu’attribut.  
  
## <a name="remarks"></a>Notes  
 L’exemple suivant illustre ce cas :  
  
```  
Rs.Open "SHAPE {select stor_id, stor_name, state from stores} APPEND ({select stor_id, ord_num, ord_date, qty from sales} AS rsSales RELATE stor_id TO stor_id)", "Provider=MSDataShape;DSN=pubs;Integrated Security=SSPI;"  
```  
  
 Le format XML de l’objet Recordset persistant est le suivant :  
  
```  
<xml xmlns:s="uuid:BDC6E3F0-6DA3-11d1-A2A3-00AA00C14882"     xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882"     xmlns:rs="urn:schemas-microsoft-com:rowset"   
    xmlns:z="#RowsetSchema">   
  <s:Schema id="RowsetSchema">   
    <s:ElementType name="row" content="eltOnly" rs:updatable="true">   
      <s:AttributeType name="stor_id" rs:number="1"   
        rs:writeunknown="true">   
        <s:datatype dt:type="string" dt:maxLength="4"   
          rs:fixedlength="true" rs:maybenull="false"/>   
      </s:AttributeType>   
      <s:AttributeType name="stor_name" rs:number="2" rs:nullable="true"   
        rs:writeunknown="true">   
          <s:datatype dt:type="string" dt:maxLength="40"/>   
      </s:AttributeType>   
      <s:AttributeType name="state" rs:number="3" rs:nullable="true"   
        rs:writeunknown="true">   
        <s:datatype dt:type="string" dt:maxLength="2"   
          rs:fixedlength="true"/>   
      </s:AttributeType>   
      <s:ElementType name="rsSales" content="eltOnly"   
        rs:updatable="true" rs:relation="010000000100000000000000">   
        <s:AttributeType name="stor_id" rs:number="1"   
          rs:writeunknown="true">   
          <s:datatype dt:type="string" dt:maxLength="4"   
            rs:fixedlength="true" rs:maybenull="false"/>   
        </s:AttributeType>   
        <s:AttributeType name="ord_num" rs:number="2"   
          rs:writeunknown="true">   
          <s:datatype dt:type="string" dt:maxLength="20"   
            rs:maybenull="false"/>   
        </s:AttributeType>   
        <s:AttributeType name="ord_date" rs:number="3"   
          rs:writeunknown="true">   
            <s:datatype dt:type="dateTime" dt:maxLength="16"   
              rs:scale="3" rs:precision="23" rs:fixedlength="true"   
              rs:maybenull="false"/>   
        </s:AttributeType>   
        <s:AttributeType name="qty" rs:number="4" rs:writeunknown="true">   
          <s:datatype dt:type="i2" dt:maxLength="2" rs:precision="5"   
            rs:fixedlength="true" rs:maybenull="false"/>   
        </s:AttributeType>   
        <s:extends type="rs:rowbase"/>   
      </s:ElementType>   
      <s:extends type="rs:rowbase"/>   
    </s:ElementType>   
  </s:Schema>   
  <rs:data>   
    <z:row stor_id="6380" stor_name="Eric the Read Books" state="WA">   
      <rsSales stor_id="6380" ord_num="6871"   
        ord_date="1994-09-14T00:00:00" qty="5"/>   
      <rsSales stor_id="6380" ord_num="722a"   
        ord_date="1994-09-13T00:00:00" qty="3"/>   
    </z:row>   
    <z:row stor_id="7066" stor_name="Barnum's" state="CA">   
      <rsSales stor_id="7066" ord_num="A2976"   
        ord_date="1993-05-24T00:00:00" qty="50"/>   
      <rsSales stor_id="7066" ord_num="QA7442.3"   
        ord_date="1994-09-13T00:00:00" qty="75"/>   
    </z:row>   
    <z:row stor_id="7067" stor_name="News & Brews" state="CA">   
      <rsSales stor_id="7067" ord_num="D4482"   
        ord_date="1994-09-14T00:00:00" qty="10"/>   
      <rsSales stor_id="7067" ord_num="P2121"   
        ord_date="1992-06-15T00:00:00" qty="40"/>   
      <rsSales stor_id="7067" ord_num="P2121"   
        ord_date="1992-06-15T00:00:00" qty="20"/>   
      <rsSales stor_id="7067" ord_num="P2121"   
        ord_date="1992-06-15T00:00:00" qty="20"/>   
    </z:row>   
  </rs:data>   
</xml>   
```  
  
 L’ordre exact des colonnes dans le jeu d’enregistrements parent n’est pas évident lorsqu’il est rendu persistant de cette manière. Tout champ du parent peut contenir un Recordset enfant. Le fournisseur de persistance conserve en premier toutes les colonnes scalaires en tant qu’attributs, puis conserve toutes les « colonnes » de l’ensemble d’enregistrements enfants en tant qu’éléments enfants de la ligne parente. La position ordinale du champ dans le jeu d’enregistrements parent peut être obtenue en examinant la définition de schéma du Recordset. Chaque champ a une propriété OLE DB, RS : Number, définie dans l’espace de noms du schéma de l’ensemble d’enregistrements qui contient le nombre ordinal pour ce champ.  
  
 Les noms de tous les champs dans le jeu d’enregistrements enfant sont concaténés avec le nom du champ dans le jeu d’enregistrements parent qui contient cet enfant. Cela permet de s’assurer qu’il n’y a pas de collisions de noms dans les cas où les jeux d’enregistrements parents et enfants contiennent tous les deux un champ obtenu à partir de deux tables différentes mais nommé au singulier.  
  
 Lorsque vous enregistrez des recordsets hiérarchiques dans XML, vous devez être conscient des restrictions suivantes dans ADO :  
  
-   Un jeu d’enregistrements hiérarchique avec des mises à jour en attente ne peut pas être conservé dans XML.  
  
-   Un jeu d’enregistrements hiérarchique créé à l’aide d’une commande SHAPE paramétrable ne peut pas être conservé (au format XML ou ADTG).  
  
-   ADO enregistre actuellement la relation entre le parent et les recordsets enfants sous la forme d’un objet BLOB (Binary Large Object). Les balises XML pour décrire cette relation n’ont pas encore été définies dans l’espace de noms du schéma de l’ensemble de lignes.  
  
-   Lorsqu’un jeu d’enregistrements hiérarchique est enregistré, tous les jeux d’enregistrements enfants sont enregistrés en même temps. Si le jeu d’enregistrements actuel est un enfant d’un autre Recordset, son parent n’est pas enregistré. Tous les jeux d’enregistrements enfants qui forment la sous-arborescence du Recordset actuel sont enregistrés.  
  
 Lorsqu’un jeu d’enregistrements hiérarchique est rouvert à partir de son format persistant XML, vous devez tenir compte des limitations suivantes :  
  
-   Si l’enregistrement enfant contient des enregistrements pour lesquels il n’y a pas d’enregistrements parents correspondants, ces lignes ne sont pas écrites dans la représentation XML de l’ensemble d’enregistrements hiérarchique. Par conséquent, ces lignes sont perdues lorsque le Recordset est rouvert à partir de son emplacement persistant.  
  
-   Si un enregistrement enfant a des références à plusieurs enregistrements parents, lors de la réouverture du Recordset, le jeu d’enregistrements enfant peut contenir des enregistrements en double. Toutefois, ces doublons ne sont visibles que si l’utilisateur travaille directement avec l’ensemble de lignes enfant sous-jacent. Si un chapitre est utilisé pour naviguer dans le jeu d’enregistrements enfant (il s’agit de la seule façon de naviguer dans ADO), les doublons ne sont pas visibles.  
  
## <a name="see-also"></a>Voir aussi  
 [Persistance des enregistrements au format XML](./persisting-records-in-xml-format.md)