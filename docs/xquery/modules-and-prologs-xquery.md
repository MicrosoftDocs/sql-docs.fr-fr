---
title: Modules et prologues (XQuery) | Microsoft Docs
description: Découvrez les spécifications qui ne sont pas prises en charge lors de la déclaration d’un espace de noms dans un prologue XQuery.
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- XQuery, prolog
- prolog
ms.assetid: 0f17b4a4-6234-41d4-a996-6db4e27bff7e
author: rothja
ms.author: jroth
ms.openlocfilehash: 841ed55566b9da0c8abd5c3c84c963ba2a70b6c9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341864"
---
# <a name="modules-and-prologs-xquery"></a>Modules et prologues (XQuery)
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Le [prologue XQuery](../xquery/modules-and-prologs-xquery-prolog.md) est une série de déclarations d’espaces de noms. En utilisant la déclaration d'espace de noms en prologue, vous pouvez spécifier une liaison entre un préfixe et un espace de noms, puis utiliser le préfixe dans le corps de la requête.  
  
## <a name="implementation-limitations"></a>Limites de mise en œuvre  
 Les spécifications XQuery suivantes ne sont pas prises en charge dans cette implémentation :  
  
-   Déclaration de module (`version`)  
  
-   Déclaration de module (`module namespace`)  
  
-   Xmpspacedeclaration ( `xmlspace` )  
  
-   Déclaration de classement par défaut (`declare default collation`)  
  
-   Déclaration d'URI de base (`declare base-uri`)  
  
-   Déclaration de construction (`declare construction`)  
  
-   Déclaration d'ordre par défaut (`declare ordering`)  
  
-   Importation de schéma (`import schema namespace`)  
  
-   Importation de module (`import module`)  
  
-   Déclaration de variable dans le prologue (`declare variable`)  
  
-   Déclaration de fonction (`declare function`)  
  
## <a name="in-this-section"></a>Dans cette section  
 [Prologue XQuery](../xquery/modules-and-prologs-xquery-prolog.md)  
 Décrit le prologue XQuery.  
  
## <a name="see-also"></a>Voir aussi  
 [Références relatives au langage Xquery &#40;SQL Server&#41;](../xquery/xquery-language-reference-sql-server.md)  
  
  
