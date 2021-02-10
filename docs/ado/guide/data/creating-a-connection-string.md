---
description: Création d’une chaîne de connexion
title: Création d’une chaîne de connexion | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/20/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- connections [ADO]
- connection strings [ADO]
ms.assetid: 14eae122-2d1e-40c8-b88e-b7cb8dfbc93b
author: rothja
ms.author: jroth
ms.openlocfilehash: eda5438d4fbda93b9e8c19602b9236d1812e6826
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100033289"
---
# <a name="creating-a-connection-string"></a>Création d’une chaîne de connexion
Une chaîne de connexion se compose d’une liste de paires argument/valeur (c’est-à-dire des paramètres), séparées par des points-virgules. Par exemple :  
  
```syntax
"arg1=val1; arg2=val2; ... argN=valN;"  
```  
  
 Tous les paramètres doivent être reconnus par ADO ou par le fournisseur spécifié.  
  
 ADO reconnaît les cinq arguments suivants dans une chaîne de connexion.  
  
|Argument|Description|  
|--------------|-----------------|  
|*Fournisseur*|Spécifie le nom d’un fournisseur à utiliser pour la connexion.|  
|*Nom de fichier*|Spécifie le nom d’un fichier spécifique au fournisseur (par exemple, un objet de source de données persistant) contenant les informations de connexion prédéfinies.|  
|*URL*|Spécifie la chaîne de connexion comme une URL absolue identifiant une ressource, telle qu’un fichier ou un répertoire.|  
|*Fournisseur distant*|Spécifie le nom d’un fournisseur à utiliser lors de l’ouverture d’une connexion côté client. (Service de données distant uniquement.)|  
|*Serveur distant*|Spécifie le nom du chemin d’accès du serveur à utiliser lors de l’ouverture d’une connexion côté client. (Service de données distant uniquement.)|  
  
 Les autres arguments sont passés au fournisseur nommé dans l’argument *Provider* , sans aucun traitement par ADO.  
  
 L’application HelloData dans [HelloData : une simple application ADO](./hellodata-a-simple-ado-application.md) utilisait la chaîne de connexion suivante :  
  
```vb
m_sConnStr = "Provider=SQLOLEDB;Data Source=MySqlServer;" & _  
             "Initial Catalog=Northwind;Integrated Security='SSPI';"  
```  
  
 Dans cette chaîne de connexion, ADO reconnaît uniquement le `"Provider=SQLOLEDB"` paramètre, qui spécifie le fournisseur Microsoft OLE DB pour SQL Server comme source de données ADO. Le reste des paires argument/valeur, `"Data Source=MySqlServer; Initial Catalog=Northwind;Integrated Security='SSPI';"` , est passé textuellement à ce fournisseur. Le type et la validité de ces paramètres sont spécifiques au fournisseur. Pour plus d’informations sur les paramètres valides qui peuvent être transmis dans la chaîne de connexion, consultez la documentation de chaque fournisseur.  
  
 Selon la documentation du fournisseur de OLE DB pour SQL Server, vous pouvez remplacer le paramètre de *source de données* « Server » par « Server » par le paramètre de *catalogue initial* . Ainsi, la chaîne de connexion suivante produit des résultats identiques à ceux ci-dessus :  
  
```vb
m_sConnStr = "Provider=SQLOLEDB;Server=MySqlServer;" & _  
             "Database=Northwind;Integrated Security='SSPI';"  
```