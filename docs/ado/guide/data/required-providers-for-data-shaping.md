---
description: Fournisseurs nécessaires pour la mise en forme des données
title: Fournisseurs requis pour la mise en forme des données | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- providers [ADO], data shaping
- data shaping [ADO], providers required
ms.assetid: d49d48d2-ac2d-4c11-895c-5a149b444620
author: rothja
ms.author: jroth
ms.openlocfilehash: eb3d91f23666c900f59a6ffc8e6b94af9bcdb4d7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032531"
---
# <a name="required-providers-for-data-shaping"></a>Fournisseurs nécessaires pour la mise en forme des données
La mise en forme des données nécessite généralement deux fournisseurs. Le fournisseur de services, [Data Shaping Service pour OLE DB](../../../ado/guide/appendixes/microsoft-data-shaping-service-for-ole-db-ado-service-provider.md), fournit les fonctionnalités de mise en forme des données, et un fournisseur de données, tel que le fournisseur OLE DB pour SQL Server, fournit des lignes de données pour remplir le [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md)mis en forme.  
  
 Le nom du fournisseur de services (MSDataShape) peut être spécifié en tant que valeur de la propriété du [fournisseur](../../../ado/reference/ado-api/provider-property-ado.md) d’objets de [connexion](../../../ado/reference/ado-api/connection-object-ado.md) ou du mot clé de chaîne de connexion « Provider = MSDataShape ; ».  
  
 Le nom du fournisseur de données peut être spécifié en tant que valeur de la propriété dynamique **fournisseur de données** , qui est ajoutée à la collection [de propriétés](../../../ado/reference/ado-api/properties-collection-ado.md) de l’objet de **connexion** par le service de mise en forme des données pour OLE DB ou le mot clé de chaîne de connexion «**fournisseur de données =** _Provider_».  
  
 Aucun fournisseur de données n’est requis si le **jeu d’enregistrements** n’est pas rempli (par exemple, comme dans un **Recordset** fabriqué où des colonnes sont créées avec le nouveau mot clé). Dans ce cas, spécifiez «**fournisseur de données =** None ; ».  
  
## <a name="example"></a>Exemple  
  
```  
Dim cnn As New ADODB.Connection  
cnn.Provider = "MSDataShape"  
cnn.Open "Data Provider=SQLOLEDB;Integrated Security=SSPI;Database=Northwind"  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Exemple de mise en forme des données](../../../ado/guide/data/data-shaping-example.md)   
 [Grammaire de forme formelle](../../../ado/guide/data/formal-shape-grammar.md)   
 [Généralités sur les commandes SHAPE](../../../ado/guide/data/shape-commands-in-general.md)
