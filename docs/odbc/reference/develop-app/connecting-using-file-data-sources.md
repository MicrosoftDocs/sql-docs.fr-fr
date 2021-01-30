---
description: Connexion à l’aide de sources de données de fichier
title: Connexion à l’aide de sources de données de fichiers | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- connecting to driver [ODBC], file data sources
- SQLDriverConnect function [ODBC], connecting using file data sources
- connecting to data source [ODBC], SQLDriverConnect
- connecting to driver [ODBC], SQLDriverConnect
- connecting to data source [ODBC], file data sources
- file data sources [ODBC]
ms.assetid: 3003f8c2-8be6-41cc-8d9c-612e9bd0f3ae
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 285b2ba9d553d43a3b1ca7fdda8c08434201acdc
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99077007"
---
# <a name="connecting-using-file-data-sources"></a>Connexion à l’aide de sources de données de fichier
Les informations de connexion pour une source de données de fichier sont stockées dans un fichier. DSN. Par conséquent, la chaîne de connexion peut être utilisée à plusieurs reprises par un seul utilisateur ou partagée entre plusieurs utilisateurs si le pilote approprié est installé. Le fichier contient un nom de pilote (ou un autre nom de source de données dans le cas d’une source de données de fichier non partagée) et éventuellement une chaîne de connexion qui peut être utilisée par **SQLDriverConnect**. Le gestionnaire de pilotes génère la chaîne de connexion pour l’appel à **SQLDriverConnect** à partir des mots clés du fichier. DSN.  
  
 Une source de données de fichier permet à une application de spécifier des options de connexion sans avoir à créer une chaîne de connexion pour une utilisation avec **SQLDriverConnect**. La source de données de fichier est généralement créée en spécifiant le mot clé **SaveFile** , qui oblige le gestionnaire de pilotes à enregistrer la chaîne de connexion de sortie créée par un appel à **SQLDriverConnect** dans le fichier. DSN. Cette chaîne de connexion peut être utilisée à plusieurs reprises en appelant **SQLDriverConnect** avec le mot clé **FILEDSN** . Cela permet de rationaliser le processus de connexion et de fournir une source persistante de la chaîne de connexion.  
  
 Vous pouvez également créer des sources de données de fichier en appelant **SQLCreateDataSource** dans la dll du programme d’installation. Les informations peuvent être écrites dans le fichier. DSN en appelant **SQLWriteFileDSN** et lire à partir du fichier. DSN en appelant **SQLReadFileDSN**; ces deux fonctions se trouvent également dans la DLL du programme d’installation. Pour plus d’informations sur la DLL du programme d’installation, consultez [Configuration des sources de données](../../../odbc/reference/install/configuring-data-sources.md).  
  
 Les mots clés utilisés pour les informations de connexion se trouvent dans la section [ODBC] d’un fichier. DSN. Les informations minimales qu’un fichier partageable. DSN aurait dans la section [ODBC] est le mot clé DRIVER :  
  
```  
DRIVER = SQL Server  
```  
  
 Le fichier Shareable. DSN contient généralement une chaîne de connexion, comme suit :  
  
```  
DRIVER = SQL Server  
UID = Larry  
DATABASE = MyDB  
```  
  
 Lorsque la source de données de fichier ne peut pas être partagée, le fichier. DSN contient uniquement un mot clé **DSN** . Quand le gestionnaire de pilotes reçoit les informations dans une source de données de fichier non partageable, il se connecte en fonction des besoins de la source de données indiquée par le mot clé **DSN** . Un fichier. DSN non partageable contient le mot clé suivant :  
  
```  
DSN = MyDataSource  
```  
  
 La chaîne de connexion utilisée pour une source de données de fichier est l’Union des mots clés spécifiés dans le fichier. DSN et les mots clés spécifiés dans la chaîne de connexion dans l’appel à **SQLDriverConnect**. Si l’un des mots clés du fichier. DSN est en conflit avec des mots clés dans la chaîne de connexion, le gestionnaire de pilotes décide de la valeur de mot clé à utiliser. Pour plus d’informations, consultez [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md).  
  
## <a name="see-also"></a>Voir aussi  
 [https://support.microsoft.com/kb/165866](https://mskb.pkisolutions.com/kb/165866)
