---
description: Espaces de noms
title: Espaces de noms | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- namespaces in ADO
ms.assetid: efff5569-db52-451d-a039-2e74870534da
author: rothja
ms.author: jroth
ms.openlocfilehash: a4ab26dd8f767315a3392a2434689561fdb391d6
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88453141"
---
# <a name="namespaces"></a>Espaces de noms
Le format de persistance XML dans ADO utilise les quatre espaces de noms suivants.  
  
## <a name="remarks"></a>Notes  
 Le format de persistance XML dans ADO utilise les quatre espaces de noms suivants.  
  
|Préfixe|Description|  
|------------|-----------------|  
|s|Fait référence à l’espace de noms « XML-Data » contenant les éléments et les attributs qui définissent le schéma du Recordset actuel.|  
|dt|Fait référence à la spécification des définitions des types de données.|  
|rs|Fait référence à l’espace de noms contenant des éléments et des attributs spécifiques aux attributs et propriétés ADO Recordset.|  
|z|Fait référence au schéma de l’ensemble de lignes actuel.|  
  
 Un client ne doit pas ajouter ses propres balises à ces espaces de noms, comme défini par la spécification. Par exemple, un client ne doit pas définir un espace de noms comme « urn : schemas-microsoft-com : rowset », puis écrire un texte tel que « RS : MyOwnTag ». Pour en savoir plus sur les espaces de noms, consultez la recommandation du W3C sur les [espaces de noms dans XML](http://www.w3.org/TR/REC-xml-names/).  
  
> [!IMPORTANT]
>  L’ID de la balise de schéma doit être « RowsetSchema » et l’espace de noms utilisé pour faire référence au schéma de l’ensemble de lignes actuel doit pointer sur « #RowsetSchema ».  
  
 Notez que le préfixe de l’espace de noms (le composant entre le signe deux-points et le signe égal) est arbitraire.  
  
```  
xmlns:rs="urn:schemas-microsoft-com:rowset"  
```  
  
 L’utilisateur peut définir cette valeur sur n’importe quel nom, à condition que ce nom soit utilisé de manière cohérente dans tout le document XML. ADO écrit toujours les lettres « s », « RS », « DT » et « z », mais ces noms de préfixe ne sont pas codés en dur dans le composant de chargement.  
  
## <a name="see-also"></a>Voir aussi  
 [Persistance des enregistrements au format XML](../../../ado/guide/data/persisting-records-in-xml-format.md)
