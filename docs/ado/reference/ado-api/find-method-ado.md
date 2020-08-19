---
description: Find, méthode (ADO)
title: Find, méthode (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
apitype: COM
f1_keywords:
- Recordset15::raw_Find
- Recordset15::Find
helpviewer_keywords:
- Find method [ADO]
ms.assetid: 55c9810a-d8ca-46c2-a9dc-80e7ee7aa188
author: rothja
ms.author: jroth
ms.openlocfilehash: d4c633cd1296c9433fbb7dfc185146c8b65e686b
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88443651"
---
# <a name="find-method-ado"></a>Find, méthode (ADO)
Recherche dans un [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) la ligne qui répond aux critères spécifiés. Éventuellement, la direction de la recherche, la ligne de départ et le décalage à partir de la ligne de départ peuvent être spécifiés. Si les critères sont satisfaits, la position de ligne actuelle est définie sur l’enregistrement trouvé ; dans le cas contraire, la position est définie à la fin (ou au début) du **Recordset**.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
Find (Criteria, SkipRows, SearchDirection, Start)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *Critères*  
 Valeur de **chaîne** qui contient une instruction spécifiant le nom de colonne, l’opérateur de comparaison et la valeur à utiliser dans la recherche.  
  
 *SkipRows*  
 facultatif. Valeur de **type long** , dont la valeur par défaut est zéro, qui spécifie l’offset de ligne à partir de la ligne actuelle ou du signet de *début* pour commencer la recherche. Par défaut, la recherche commence sur la ligne actuelle.  
  
 *SearchDirection*  
 facultatif. Valeur de [SearchDirectionEnum](../../../ado/reference/ado-api/searchdirectionenum.md) qui spécifie si la recherche doit commencer sur la ligne actuelle ou sur la ligne suivante disponible dans le sens de la recherche. Une recherche ayant échoué s’arrête à la fin de l’ensemble d' **enregistrements** si la valeur est **adSearchForward**. Une recherche ayant échoué s’arrête au début de l’ensemble d' **enregistrements** si la valeur est **adSearchBackward**.  
  
 *Start*  
 facultatif. Signet de **variante** qui fonctionne comme position de départ pour la recherche.  
  
## <a name="remarks"></a>Notes  
 Seul un nom de colonne unique peut être spécifié dans *critères*. Cette méthode ne prend pas en charge les recherches sur plusieurs colonnes.  
  
 L’opérateur de comparaison dans *Criteria* peut être " **>** " (supérieur à), "* * \<**" (less than), "=" (equal), "> =" (supérieur ou égal à), "<=" (inférieur ou égal à), "<>" (différent de) ou "like" (critères spéciaux).  
  
 La valeur dans *Criteria* peut être une chaîne, un nombre à virgule flottante ou une date. Les valeurs de chaîne sont délimitées par des guillemets simples ou par des signes dièse (#) (par exemple, "State = 'WA" "ou" state = #WA # "). Les valeurs de date sont délimitées par des signes « # » (dièse) (par exemple, « start_date > #7/22/97 # »). Ces valeurs peuvent contenir des heures, des minutes et des secondes pour indiquer des horodatages, mais elles ne doivent pas contenir de millisecondes ou des erreurs se produisent.  
  
 Si l’opérateur de comparaison est « like », la valeur de chaîne peut contenir un astérisque (*) pour rechercher une ou plusieurs occurrences de n’importe quel caractère ou sous-chaîne. Par exemple, « État comme «m » \* » correspond à Maine et Massachusetts. Vous pouvez également utiliser des astérisques de début et de fin pour rechercher une sous-chaîne contenue dans les valeurs. Par exemple, « State like » \* \* correspond à Alaska, Arkansas et Massachusetts.  
  
 Les astérisques peuvent être utilisés uniquement à la fin d’une chaîne de critères, ou au début et à la fin d’une chaîne de critères, comme indiqué ci-dessus. Vous ne pouvez pas utiliser l’astérisque comme caractère générique de début (« * STR »), ni comme caractère générique incorporé ( \* r). Cela génère une erreur.  
  
> [!NOTE]
>  Une erreur se produit si une position de ligne actuelle n’est pas définie avant l’appel de **Find**. Toute méthode qui définit la position de ligne, par exemple [MoveFirst](../../../ado/reference/ado-api/movefirst-movelast-movenext-and-moveprevious-methods-ado.md), doit être appelée avant l’appel de **Find**.  
  
> [!NOTE]
>  Si vous appelez la méthode **Find** sur un Recordset et que la position actuelle dans le jeu d’enregistrements se trouve au dernier enregistrement ou fin de fichier (EOF), vous ne trouverez rien. Vous devez appeler la méthode **MoveFirst** pour définir la position/le curseur actuel au début du Recordset.  
  
## <a name="applies-to"></a>S'applique à  
 [Recordset, objet (ADO)](../../../ado/reference/ado-api/recordset-object-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Find, exemple de méthode (VB)](../../../ado/reference/ado-api/find-method-example-vb.md)   
 [Propriété d’index](../../../ado/reference/ado-api/index-property.md)   
 [Optimize, propriété dynamique (ADO)](../../../ado/reference/ado-api/optimize-property-dynamic-ado.md)   
 [Seek, méthode](../../../ado/reference/ado-api/seek-method.md)
