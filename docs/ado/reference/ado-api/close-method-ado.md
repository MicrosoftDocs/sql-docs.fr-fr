---
description: Close, méthode (ADO)
title: Close, méthode (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset15::Close
- _Stream::Close
- _Record::Close
helpviewer_keywords:
- Close method [ADO]
ms.assetid: 3cdf27d1-a180-4cff-8e42-95dec5fb1b55
author: rothja
ms.author: jroth
ms.openlocfilehash: 9a7c0ea176f203bd9b7b2febc17475e6af385d7f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99171513"
---
# <a name="close-method-ado"></a>Close, méthode (ADO)
Ferme un objet ouvert et tous les objets dépendants.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
object.Close  
```  
  
## <a name="remarks"></a>Notes  
 Utilisez la méthode **Close** pour fermer une [connexion](./connection-object-ado.md), un [enregistrement](./record-object-ado.md), un [jeu d’enregistrements](./recordset-object-ado.md)ou un objet de [flux](./stream-object-ado.md) pour libérer toutes les ressources système associées. La fermeture d’un objet ne le supprime pas de la mémoire ; vous pouvez modifier ses paramètres de propriété et l’ouvrir à nouveau ultérieurement. Pour éliminer complètement un objet de la mémoire, fermez l’objet, puis affectez à la variable objet la valeur *Nothing* (dans Visual Basic).  
  
## <a name="connection"></a>Connexion  
 L’utilisation de la méthode **Close** pour fermer un objet **Connection** ferme également tous les objets **Recordset** actifs associés à la connexion. Un objet de [commande](./command-object-ado.md) associé à l’objet de **connexion** que vous fermez est conservé, mais il n’est plus associé à un objet de **connexion** ; autrement dit, sa propriété [ActiveConnection](./activeconnection-property-ado.md) prend la valeur **Nothing**. En outre, la collection de [paramètres](./parameters-collection-ado.md) de l’objet de **commande** est effacée de tous les paramètres définis par le fournisseur.  
  
 Vous pouvez appeler ultérieurement la méthode [Open](./open-method-ado-connection.md) pour rétablir la connexion à la même source de données, ou à une autre. Pendant la fermeture de l’objet de **connexion** , l’appel de toutes les méthodes qui requièrent une connexion ouverte à la source de données génère une erreur.  
  
 La fermeture d’un objet de **connexion** pendant l’existence d’objets **Recordset** ouverts sur la connexion annule toutes les modifications en attente dans tous les objets **Recordset** . La fermeture explicite d’un objet **Connection** (en appelant la méthode **Close** ) pendant qu’une transaction est en cours génère une erreur. Si un objet de **connexion** se trouve hors de portée pendant qu’une transaction est en cours, ADO annule automatiquement la transaction.  
  
## <a name="recordset-record-stream"></a>Recordset, enregistrement, flux  
 L’utilisation de la méthode **Close** pour fermer un objet **Recordset**, **Record** ou **Stream** libère les données associées et tout accès exclusif que vous pouvez avoir pour les données via cet objet particulier. Vous pouvez appeler ultérieurement la méthode [Open](./open-method-ado-recordset.md) pour rouvrir l’objet avec les mêmes attributs, ou modifiés.  
  
 Lors de la fermeture d’un objet **Recordset** , l’appel de toute méthode nécessitant un curseur dynamique génère une erreur.  
  
 Si une modification est en cours alors qu’elle est en mode de mise à jour immédiate, l’appel de la méthode **Close** génère une erreur. au lieu de cela, appelez d’abord la méthode [Update](./update-method.md) ou [CancelUpdate](./cancelupdate-method-ado.md) . Si vous fermez l’objet **Recordset** en mode de mise à jour par lot, toutes les modifications apportées depuis le dernier appel [UpdateBatch](./updatebatch-method.md) sont perdues.  
  
 Si vous utilisez la méthode [clone](./clone-method-ado.md) pour créer des copies d’un objet **Recordset** ouvert, la fermeture de l’original ou d’un clone n’a aucune incidence sur les autres copies.  
  
## <a name="applies-to"></a>S'applique à  

:::row:::
    :::column:::
        [Connection, objet (ADO)](./connection-object-ado.md)  
        [Record, objet (ADO)](./record-object-ado.md)  
    :::column-end:::
    :::column:::
        [Recordset, objet (ADO)](./recordset-object-ado.md)  
        [Stream, objet (ADO)](./stream-object-ado.md)  
    :::column-end:::
:::row-end:::

## <a name="see-also"></a>Voir aussi  
 [Open et Close, exemple de méthodes (VB)](./open-and-close-methods-example-vb.md)   
 [Open et Close, exemple de méthode (VBScript)](./open-and-close-methods-example-vbscript.md)   
 [Open et Close, exemple de méthodes (VC + +)](./open-and-close-methods-example-vc.md)   
 [Open, méthode (connexion ADO)](./open-method-ado-connection.md)   
 [Open, méthode (objet Recordset ADO)](./open-method-ado-recordset.md)   
 [Save, méthode](./save-method.md)