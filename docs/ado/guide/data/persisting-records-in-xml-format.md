---
description: Persistance des enregistrements au format XML
title: Persistance des enregistrements au format XML | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- persisting data [ADO]
- data updates [ADO], persisting data
- data persistence [ADO]
- XML persistence [ADO]
- updating data [ADO], persisting data
ms.assetid: f3113ec4-ae31-428f-89c6-bc1024f128ea
author: rothja
ms.author: jroth
ms.openlocfilehash: 6f29f624fe9f55287dcd8944fd04da0426d9552c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032621"
---
# <a name="persisting-records-in-xml-format"></a>Persistance des enregistrements au format XML
À l’instar du format ADTG, la persistance des **recordsets** au format XML est implémentée avec le fournisseur de persistance Microsoft OLE DB. Ce fournisseur génère un ensemble de lignes avant uniquement et en lecture seule à partir d’un fichier ou d’un flux XML enregistré qui contient les informations de schéma générées par ADO. De même, il peut utiliser un **jeu d’enregistrements** ADO, générer du code XML et l’enregistrer dans un fichier ou tout objet qui implémente l’interface com **IStream** . (En fait, un fichier est simplement un autre exemple d’un objet qui prend en charge **IStream**.) Pour les versions 2,5 et ultérieures, ADO s’appuie sur l’analyseur XML de Microsoft (MSXML) pour charger le code XML dans le **jeu d’enregistrements**; par conséquent, msxml.dll est obligatoire.  
  
> [!NOTE]
>  Certaines limitations s’appliquent lors de l’enregistrement de **recordsets** hiérarchiques (formes de données) au format XML. Vous ne pouvez pas enregistrer dans XML si le **jeu d’enregistrements** hiérarchique contient des mises à jour en attente et que vous ne pouvez pas enregistrer un **jeu d’enregistrements** hiérarchique paramétré. Pour plus d’informations, consultez [persistance des recordsets filtrés et hiérarchiques](../../../ado/guide/data/persisting-filtered-and-hierarchical-recordsets.md).  
  
 Le moyen le plus simple de conserver des données au format XML et de les charger à nouveau par le biais d’ADO est avec les méthodes **Save** et **Open** , respectivement. L’exemple de code ADO suivant montre comment enregistrer les données de la table **titles** dans un fichier nommé titles. SAV.  
  
```  
Dim rs as new Recordset  
Dim rs2 as new Recordset  
Dim c as new Connection  
Dim s as new Stream  
  
' Query the Titles table.  
c.Open "provider=sqloledb;data source=MySQLServer;initial catalog=pubs;Integrated Security='SSPI'"  
rs.cursorlocation = adUseClient  
rs.Open "select * from titles", c, adOpenStatic  
  
' Save to the file in the XML format. Note that if you don't specify   
' adPersistXML, a binary format (ADTG) will be used by default.  
rs.Save "titles.sav", adPersistXML  
  
' Save the recordset into the ADO Stream object.  
rs.save s, adPersistXML  
rs.Close  
c.Close  
  
set rs = nothing  
  
' Reopen the file.  
rs.Open "titles.sav",,,,adCmdFile  
' Open the Stream back into a Recordset.  
rs2.open s  
```  
  
 ADO conserve toujours l’objet **Recordset** entier. Si vous souhaitez conserver un sous-ensemble de lignes de l’objet **Recordset** , utilisez la méthode **Filter** pour limiter les lignes ou modifier votre clause de sélection. Toutefois, vous devez ouvrir un objet **Recordset** avec un curseur côté client (**CursorLocation**  =  **adUseClient**) pour utiliser la méthode **Filter** pour l’enregistrement d’un sous-ensemble de lignes. Par exemple, pour récupérer des titres qui commencent par la lettre « b », vous pouvez appliquer un filtre à un objet **Recordset** ouvert :  
  
```  
rs.Filter "title_id like 'B*'"  
rs.Save "btitles.sav", adPersistXML  
```  
  
 ADO utilise toujours l’ensemble de lignes du moteur de curseur client pour produire un objet **Recordset** de défilement et pouvant faire l’objet d’un signet en plus des données avant uniquement générées par le fournisseur de persistance.  
  
 Cette section contient les rubriques suivantes :  
  
-   [Format de persistance XML](../../../ado/guide/data/xml-persistence-format.md)  
  
-   [Espaces de noms](../../../ado/guide/data/namespaces.md)  
  
-   [Section de schéma](../../../ado/guide/data/schema-section.md)  
  
-   [Section de données](../../../ado/guide/data/data-section.md)  
  
-   [Recordsets hiérarchiques dans XML](../../../ado/guide/data/hierarchical-recordsets-in-xml.md)  
  
-   [Propriétés dynamiques du recordset dans XML](../../../ado/guide/data/recordset-dynamic-properties-in-xml.md)  
  
-   [Transformations XSLT](../../../ado/guide/data/xslt-transformations.md)  
  
-   [Enregistrement dans l’objet DOM XML](../../../ado/guide/data/saving-to-the-xml-dom-object.md)  
  
-   [Considérations relatives à la sécurité de XML](../../../ado/guide/data/xml-security-considerations.md)  
  
-   [Scénario de persistance des recordsets XML](../../../ado/guide/data/xml-recordset-persistence-scenario.md)
