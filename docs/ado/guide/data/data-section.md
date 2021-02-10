---
description: Section de données
title: Section données | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- data section [ADO]
ms.assetid: 43dc42a8-7057-48e6-93d6-880d5c5c51a4
author: rothja
ms.author: jroth
ms.openlocfilehash: 09b79b0001ff448ecd333a4ec601c4ff42febf6d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037659"
---
# <a name="data-section"></a>Section de données
La section Data définit les données de l’ensemble de lignes, ainsi que toutes les mises à jour, insertions ou suppressions en attente. La section de données peut contenir zéro ou plusieurs lignes. Elle ne peut contenir que des données d’un ensemble de lignes où la ligne est définie par le schéma. En outre, comme indiqué précédemment, les colonnes sans données peuvent être omises. Si un attribut ou un sous-élément est utilisé dans la section de données et que cette construction n’a pas été définie dans la section de schéma, elle est ignorée en mode silencieux.  
  
## <a name="string"></a>String  
 Les caractères XML réservés dans les données de texte doivent être remplacés par les entités de caractères appropriées. Par exemple, dans le nom de la société « garage de Joe », le guillemet simple doit être remplacé par une entité. La ligne réelle ressemble à ce qui suit :  
  
```  
<z:row CompanyName="Joe's Garage"/>  
```  
  
 Les caractères suivants sont réservés dans XML et doivent être remplacés par des entités de caractères : {', ", &, \<,> }.  
  
## <a name="binary"></a>Binary  
 Les données binaires sont encodées par bin. hex (autrement dit, un octet est mappé à deux caractères, un caractère par Quartet).  
  
## <a name="datetime"></a>DateTime  
 Le format de VT_DATE variant n’est pas directement pris en charge par les types de données XML-Data. Le format correct pour les dates avec un composant de données et d’heure est AAAA-MM-JJThh : mm : SS.  
  
 Pour plus d’informations sur les formats de date spécifiés par XML, consultez la [spécification W3C XML-Data](https://go.microsoft.com/fwlink/?LinkId=5692).  
  
 Lorsque la spécification de XML-Data définit deux types de données équivalents (par exemple, I4 = = int), ADO écrit le nom convivial, mais lit les deux.  
  
## <a name="managing-pending-changes"></a>Gestion des modifications en attente  
 Un jeu d’enregistrements peut être ouvert en mode de mise à jour immédiate ou par lot. Lorsqu’ils sont ouverts en mode de mise à jour par lot avec des curseurs côté client, toutes les modifications apportées à l’ensemble d’enregistrements sont dans un état d’attente jusqu’à ce que la méthode UpdateBatch soit appelée. Les modifications en attente sont également conservées lors de l’enregistrement du Recordset. En XML, ils sont représentés par l’utilisation des éléments « Update » définis dans urn : schemas-microsoft-com : rowset. En outre, si un ensemble de lignes peut être mis à jour, la propriété pouvant être mise à jour doit avoir la valeur true dans la définition de la ligne. Par exemple, pour définir que la table Shippers contient des modifications en attente, la définition de ligne ressemble à ce qui suit.  
  
```  
<s:ElementType name="row" content="eltOnly" updatable="true">  
  <s:attribute type="ShipperID"/>  
  <s:attribute type="CompanyName"/>  
  <s:attribute type="Phone"/>  
  <s:extends type="rs:rowbase"/>  
</s:ElementType>  
```  
  
 Cela indique au fournisseur de persistance d’exposer les données afin qu’ADO puisse construire un objet Recordset pouvant être mis à jour.  
  
 Les exemples de données suivants montrent comment les insertions, les modifications et les suppressions apparaissent dans le fichier persistant.  
  
```  
<rs:data>  
  <z:row ShipperID="2" CompanyName="United Package"   
    Phone="(503) 555-3199"/>  
<rs:update>  
  <rs:original>  
    <z:row ShipperID="3" CompanyName="Federal Shipping"   
      Phone="(503) 555-9931"/>  
  </rs:original>  
  <z:row Phone="(503) 552-7134"/>  
</rs:update>  
<rs:insert>  
  <z:row ShipperID="12" CompanyName="Lightning Shipping"   
    Phone="(505) 111-2222"/>  
  <z:row ShipperID="13" CompanyName="Thunder Overnight"   
    Phone="(505) 111-2222"/>  
  <z:row ShipperID="14" CompanyName="Blue Angel Air Delivery"   
    Phone="(505) 111-2222"/>  
</rs:insert>  
<rs:delete>  
  <z:row ShipperID="1" CompanyName="Speedy Express" Phone="(503) 555-9831"/>  
</rs:delete>  
</rs:data>  
```  
  
 Une mise à jour contient toujours l’intégralité des données de ligne d’origine, suivies des données de ligne modifiées. La ligne modifiée peut contenir toutes les colonnes ou uniquement celles qui ont été réellement modifiées. Dans l’exemple précédent, la ligne de Shipper 2 n’est pas modifiée et seule la colonne Phone a changé de valeur pour Shipper 3 et est donc la seule colonne incluse dans la ligne modifiée. Les lignes insérées pour les expéditeurs 12, 13 et 14 sont regroupées sous une étiquette RS : Insert. Notez que les lignes supprimées peuvent également être regroupées par lot, bien que cela ne soit pas illustré dans l’exemple précédent.  
  
## <a name="see-also"></a>Voir aussi  
 [Persistance des enregistrements au format XML](./persisting-records-in-xml-format.md)