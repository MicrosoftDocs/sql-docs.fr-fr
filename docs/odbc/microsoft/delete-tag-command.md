---
description: DELETE TAG, commande
title: SUPPRIMER la BALIse, commande | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- DELETE TAG command [ODBC]
ms.assetid: 4f4e1362-a5f3-4b15-8a3c-d4e96605f221
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: aa431c50fcd151d57ce7613586587a1c2342a5c7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181568"
---
# <a name="delete-tag-command"></a>DELETE TAG, commande
Supprime une balise ou des balises d’un fichier d’index composé (. CDX).  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
DELETE TAG TagName1 [OF CDXFileName1]  
   [, TagName2 [OF CDXFileName2]] ...  
  Or   
DELETE TAG ALL [OF CDXFileName]  
```  
  
## <a name="arguments"></a>Arguments  
 *TagName1* DE *CDXFileName1*[, *TagName2*[of *CDXFileName2*]]...  
 Spécifie une balise à supprimer d’un fichier d’index composé. Vous pouvez supprimer plusieurs balises à l’aide d’une BALIse DELETE, en incluant une liste de noms de balises séparés par des virgules. Si au moins deux balises portant le même nom existent dans les fichiers d’index ouverts, vous pouvez supprimer une balise d’un fichier d’index spécifique en incluant *CDXFileName*.  
  
 ALL [de *CDXFileName*]  
 Supprime chaque balise d’un fichier d’index composé. Si la table actuelle possède un fichier d’index composé structurel, toutes les balises sont supprimées du fichier d’index, le fichier d’index est supprimé du disque et l’indicateur dans l’en-tête de la table indiquant la présence d’un fichier d’index composé structurel associé est supprimé. Utilisez ALL with OF *CDXFileName* pour supprimer toutes les balises d’un fichier d’index composé ouvert autre que le fichier d’index composé structurel.  
  
## <a name="remarks"></a>Notes  
 Les fichiers d’index composés, créés avec l’INDEX, contiennent des balises correspondant à des entrées d’index. La BALIse DELETE est utilisée pour supprimer une balise ou des balises des fichiers d’index composés ouverts. Vous ne pouvez supprimer que les balises des fichiers d’index composés ouverts dans la zone de travail active. Si vous supprimez toutes les balises d’un fichier d’index composé, le fichier est supprimé du disque.  
  
 Visual FoxPro recherche d’abord une balise dans le fichier d’index composé structurel (le cas échéant). Si la balise n’est pas dans le fichier d’index composé structurel, Visual FoxPro recherche la balise dans les autres fichiers d’index composés ouverts.  
  
## <a name="see-also"></a>Voir aussi  
 [INDEX, commande](../../odbc/microsoft/index-command.md)
