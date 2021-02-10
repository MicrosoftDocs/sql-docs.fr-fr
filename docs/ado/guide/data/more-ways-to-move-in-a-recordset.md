---
description: Autres méthodes de déplacement dans un recordset
title: Autres méthodes de déplacement dans un jeu d’enregistrements | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- MoveNext method [ADO]
- MoveLast method [ADO]
- MoveFirst method [ADO]
- Recordset object [ADO], moving
- MovePrevious method [ADO]
ms.assetid: 9f8cf1b2-3def-453f-a0ff-4646c5f15262
author: rothja
ms.author: jroth
ms.openlocfilehash: 78acb389a38896d05a684b0dbd940942a026655e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032711"
---
# <a name="more-ways-to-move-in-a-recordset"></a>Autres méthodes de déplacement dans un recordset
Les quatre méthodes suivantes permettent de se déplacer ou de faire défiler dans le **jeu d’enregistrements**: [MoveFirst, MoveLast, MoveNext et MovePrevious](../../reference/ado-api/movefirst-movelast-movenext-and-moveprevious-methods-ado.md). (Certaines de ces méthodes ne sont pas disponibles sur les curseurs avant uniquement.)  
  
 **MoveFirst** change la position actuelle de l’enregistrement en premier enregistrement dans le **jeu d’enregistrements**. **MoveLast** remplace la position de l’enregistrement actuel par le dernier enregistrement du **Recordset**. Pour utiliser **MoveFirst** ou **MoveLast**, l’objet **Recordset** doit prendre en charge les signets ou le déplacement du curseur vers l’arrière. dans le cas contraire, l’appel de méthode génère une erreur.  
  
 **MoveNext** déplace la position d’enregistrement active d’un point à un autre. Si vous êtes sur le dernier enregistrement lorsque vous appelez **MoveNext**, **EOF** prend la valeur **true**. **MovePrevious** déplace la position de l’enregistrement actif d’un emplacement vers l’arrière. Si vous êtes sur le premier enregistrement lorsque vous appelez **MovePrevious**, **BOF** prend la valeur **true**. Il est recommandé de vérifier les propriétés **EOF** et **BOF** lors de l’utilisation de ces méthodes et de ramener le curseur à une position d’enregistrement actif valide si vous quittez l’une ou l’autre des extrémités de l’ensemble d' **enregistrements**, comme illustré ici :  
  
```  
. . .  
oRs.MoveNext  
If oRs.EOF Then oRs.MoveLast  
. . .   
```  
  
 Ou, dans le cas de la méthode **MovePrevious** :  
  
```  
. . .   
oRs.MovePrevious  
If oRs.BOF Then oRs.MoveFirst  
. . .  
```  
  
 Dans les cas où l' **objet Recordset** a été filtré ou trié et que les données de l’enregistrement actif sont modifiées, la position peut également changer. Dans ce cas, la méthode **MoveNext** fonctionne normalement, mais sachez que la position est déplacée d’un enregistrement vers l’avant à partir de la nouvelle position, et non de l’ancienne position. Par exemple, la modification des données dans l’enregistrement actif, de sorte que l’enregistrement soit déplacé à la fin de l’ensemble d' **enregistrements** trié, signifie que l’appel de **MoveNext** entraîne la définition par ADO de l’enregistrement actif à la position qui suit le dernier enregistrement dans le **jeu d’enregistrements** (**EOF**  =  **true**).  
  
 Le comportement des différentes méthodes Move de l’objet **Recordset** dépend, dans une certaine mesure, des données contenues dans le **jeu d’enregistrements**. Les nouveaux enregistrements ajoutés au **Recordset** sont initialement ajoutés dans un ordre particulier, qui est défini par la source de données et peuvent être dépendants implicitement ou explicitement sur les données du nouvel enregistrement. Par exemple, si un tri ou une jointure est effectué dans la requête qui remplit le **Recordset**, le nouvel enregistrement est inséré à l’emplacement approprié dans le **jeu d’enregistrements**. Si le classement n’est pas spécifié explicitement lors de la création du **Recordset**, les modifications apportées à l’implémentation de la source de données peuvent entraîner la modification de l’ordre des lignes retournées par inadvertance. En outre, les fonctions de tri, de filtrage et de modification de l’ensemble d' **enregistrements** peuvent affecter l’ordre et, éventuellement, les lignes de l’ensemble d’enregistrements qui seront visibles.  
  
 Par conséquent, **MoveNext**, **MovePrevious**, **MoveFirst**, **MoveLast** et **Move** sont tous sensibles aux autres opérations effectuées sur le même **Recordset**. ADO essaiera toujours de maintenir votre position actuelle jusqu’à ce que vous la déplaciez explicitement, mais parfois, les modifications intervenues compliquent la compréhension des effets d’un déplacement suivant. Par exemple, si vous appelez **MoveFirst** pour positionner sur la première ligne d’un **jeu d’enregistrements** trié et que vous modifiez l’ordre croissant en ordre décroissant, vous êtes toujours sur la même ligne, mais il s’agit maintenant de la dernière ligne dans le **jeu d’enregistrements**. **MoveFirst** vous permet d’atteindre une autre ligne (la nouvelle première ligne).  
  
 Autre exemple : Si vous êtes positionné sur une ligne particulière au milieu d’un **jeu d’enregistrements** et que vous appelez **Delete** , puis appelez **MoveNext**, vous êtes maintenant sur l’enregistrement qui suit immédiatement l’enregistrement supprimé. Toutefois, l’appel de **MovePrevious** fait de l’enregistrement précédant celui que vous avez supprimé l’enregistrement actif, car l’enregistrement supprimé n’est plus compté dans l’appartenance active de l’ensemble d' **enregistrements**.  
  
 Il est particulièrement difficile de définir une sémantique de déplacement cohérente sur tous les fournisseurs pour les méthodes qui se déplacent par rapport à l’enregistrement actuel- **MovePrevious**, **MoveNext** et **Move** -en cas de modification des données dans l’enregistrement actuel. Par exemple, si vous utilisez un **jeu d’enregistrements** trié et filtré, et que vous modifiez les données de l’enregistrement en cours afin qu’elles précèdent tous les autres enregistrements, mais que vos données modifiées ne correspondent plus au filtre, il n’est pas évident qu’une opération **MoveNext** doit vous entraîner. La conclusion la plus sûre est que le mouvement relatif dans un **jeu d’enregistrements** est plus risqué que le mouvement absolu (par exemple, en utilisant **MoveFirst** ou **MoveLast**) lorsque les données changent alors que les enregistrements sont modifiés, ajoutés ou supprimés. Le tri et le filtrage doivent être basés sur une clé primaire ou un ID, car ce type de valeur ne doit pas changer.