---
description: Flux de commandes
title: Flux de commandes | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- command streams [ADO]
- streams [ADO], command
ms.assetid: 0ac09dbe-2665-411e-8fbb-d1efe6c777be
author: rothja
ms.author: jroth
ms.openlocfilehash: 1c0872f20754d093020ece2830d389de32f09032
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037739"
---
# <a name="command-streams"></a>Flux de commandes
ADO a toujours pris en charge l’entrée de commande dans un format de chaîne spécifié par la propriété **CommandText** . En guise d’alternative, avec ADO 2,7 ou une version ultérieure, vous pouvez également utiliser un flux d’informations pour l’entrée de commande en affectant le flux à la propriété **CommandStream** . Vous pouvez assigner un objet de **flux** ADO ou tout objet qui prend en charge l’interface com **IStream** .  
  
 Le contenu du flux de commande est simplement transmis d’ADO à votre fournisseur. votre fournisseur doit donc prendre en charge l’entrée de commande par flux pour que cette fonctionnalité fonctionne. Par exemple, SQL Server prend en charge les requêtes sous forme de modèles XML ou d’extensions OpenXML pour Transact-SQL.  
  
 Étant donné que les détails du flux doivent être interprétés par le fournisseur, vous devez spécifier le dialecte de la commande en définissant la propriété **dialecte** . La valeur du **dialecte** est une chaîne contenant un GUID, qui est défini par votre fournisseur. Pour plus d’informations sur les valeurs valides pour le **dialecte** pris en charge par votre fournisseur, consultez la documentation de votre fournisseur.  
  
## <a name="xml-template-query-example"></a>Exemple de requête de modèle XML  
 L’exemple suivant est écrit en VBScript dans la base de données Northwind.  
  
 Tout d’abord, initialisez et ouvrez l’objet de **flux** qui sera utilisé pour contenir le flux de requête :  
  
```  
Dim adoStreamQuery  
Set adoStreamQuery = Server.CreateObject("ADODB.Stream")  
adoStreamQuery.Open  
```  
  
 Le contenu du flux de requête est une requête de modèle XML.  
  
 La requête de modèle requiert une référence à l’espace de noms XML identifié par le préfixe SQL : de la \<sql:query> balise. Une instruction SQL SELECT est incluse en tant que contenu du modèle XML et affectée à une variable de chaîne comme suit :  
  
```  
sQuery = "<ROOT xmlns:sql='urn:schemas-microsoft-com:xml-sql'>  
<sql:query> SELECT * FROM PRODUCTS ORDER BY PRODUCTNAME </sql:query>  
</ROOT>"  
```  
  
 Ensuite, écrivez la chaîne dans le flux :  
  
```  
adoStreamQuery.WriteText sQuery, adWriteChar  
adoStreamQuery.Position = 0  
```  
  
 Affectez adoStreamQuery à la propriété **CommandStream** d’un objet **Command** ADO :  
  
```  
Dim adoCmd  
Set adoCmd  = Server.CreateObject("ADODB.Command"")  
adoCmd.CommandStream = adoStreamQuery  
```  
  
 Spécifiez le **dialecte** du langage de commande, qui indique comment le fournisseur d’OLE DB SQL Server doit interpréter le flux de commande. Dialecte spécifié par un GUID spécifique au fournisseur :  
  
```  
adoCmd.Dialect = "{5D531CB2-E6Ed-11D2-B252-00C04F681B71}"  
```  
  
 Enfin, exécutez la requête et renvoyez les résultats à un objet **Recordset** :  
  
```  
Set objRS = adoCmd.Execute  
```
