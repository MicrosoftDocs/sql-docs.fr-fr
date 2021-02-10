---
description: En savoir plus sur la persistance des recordsets
title: En savoir plus sur la persistance des recordsets | Microsoft Docs
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
- updating data [ADO], persisting data
ms.assetid: a9b287f5-04b0-4514-8143-f67879ca9842
author: rothja
ms.author: jroth
ms.openlocfilehash: ce3b22ecd5913db0045f82e72aa20b0833a5109b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037259"
---
# <a name="more-about-recordset-persistence"></a>En savoir plus sur la persistance des recordsets
L’objet ADO Recordset prend en charge le stockage du contenu d’un objet **Recordset** dans un fichier à l’aide de sa méthode [Save](../../reference/ado-api/save-method.md) . Le fichier stocké de manière permanente peut exister sur un lecteur local, un serveur ou en tant qu’URL sur un site Web. Plus tard, le fichier peut être restauré à l’aide de la méthode [Open](../../reference/ado-api/open-method-ado-recordset.md) de l’objet **Recordset** ou de la méthode [Execute](../../reference/ado-api/execute-method-ado-connection.md) de l’objet [Connection](../../reference/ado-api/connection-object-ado.md) .  
  
 En outre, la méthode [GetString](../../reference/ado-api/getstring-method-ado.md) convertit un objet **Recordset** en un formulaire dans lequel les colonnes et les lignes sont délimitées par des caractères que vous spécifiez.  
  
 Pour conserver un **jeu d’enregistrements**, commencez par le convertir en un formulaire qui peut être stocké dans un fichier. Les objets **Recordset** peuvent être stockés au format ADTG (Advanced Data TableGram) propriétaire ou au format Open Extensible Markup Language (XML). Les exemples ADTG sont présentés dans la section suivante. Pour plus d’informations sur la persistance XML, consultez persistance [des enregistrements au format XML](./persisting-records-in-xml-format.md).  
  
 Enregistrez toutes les modifications en attente dans le fichier persistant. Cela vous permet d’émettre une requête qui retourne un objet **Recordset** , modifie le **Recordset**, l’enregistre et les modifications en attente, puis restaure ultérieurement le **Recordset**, puis met à jour la source de données avec les modifications en attente enregistrées.  
  
 Pour plus d’informations sur le stockage permanent des objets de **flux** , consultez [flux et persistance](./streams-and-persistence.md).  
  
 Pour obtenir un exemple de persistance d’un **jeu d’enregistrements** , consultez le scénario de persistance d’un jeu d’enregistrements XML.  
  
## <a name="example"></a>Exemple  
  
### <a name="save-a-recordset"></a>Enregistrer un jeu d’enregistrements :  
  
```  
Dim rs as New ADODB.Recordset  
rs.Save "c:\yourFile.adtg", adPersistADTG  
```  
  
### <a name="open-a-persisted-file-with-recordsetopen"></a>Ouvrez un fichier persistant avec Recordset. Open :  
  
```  
Dim rs as New ADODB.Recordset  
rs.Open "c:\yourFile.adtg", "Provider=MSPersist",,,adCmdFile  
```  
  
 Éventuellement, si le **Recordset** n’a pas de connexion active, vous pouvez accepter toutes les valeurs par défaut et coder les éléments suivants :  
  
```  
Dim rs as New ADODB.Recordset  
rs.Open "c:\yourFile.adtg"  
```  
  
### <a name="open-a-persisted-file-with-connectionexecute"></a>Ouvrez un fichier persistant avec Connection.Exejolie :  
  
```  
Dim conn as New ADODB.Connection  
Dim rs as ADODB.Recordset  
conn.Open "Provider=MSPersist"  
Set rs = conn.execute("c:\yourFile.adtg")  
```  
  
### <a name="open-a-persisted-file-with-rdsdatacontrol"></a>Ouvrez un fichier persistant avec RDS. DataControl  
 Dans ce cas, la propriété de **serveur** n’est pas définie.  
  
```  
Dim dc as New RDS.DataControl  
dc.Connection = "Provider=MSPersist"  
dc.SQL = "c:\yourFile.adtg"  
dc.Refresh  
```  
  
## <a name="see-also"></a>Voir aussi  
 [GetString, méthode (ADO)](../../reference/ado-api/getstring-method-ado.md)   
 [Fournisseur de persistance Microsoft OLE DB (fournisseur de services ADO)](../appendixes/microsoft-ole-db-persistence-provider-ado-service-provider.md)   
 [Recordset, objet (ADO)](../../reference/ado-api/recordset-object-ado.md)   
 [Flux et persistance](./streams-and-persistence.md)