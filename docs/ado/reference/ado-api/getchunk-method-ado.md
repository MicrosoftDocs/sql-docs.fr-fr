---
description: GetChunk, méthode (ADO)
title: GetChunk, méthode (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Field20::raw_GetChunk
- Field20::GetChunk
helpviewer_keywords:
- GetChunk method [ADO]
ms.assetid: fc268e22-205b-44a3-9038-ffed51e23e10
author: rothja
ms.author: jroth
ms.openlocfilehash: 7d9e47713f3bd4d86f35cff2acac0255ed4ab5c7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99164577"
---
# <a name="getchunk-method-ado"></a>GetChunk, méthode (ADO)
Retourne la totalité ou une partie du contenu d’un objet de texte ou de [champ](./field-object.md) de données binaire volumineux.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
variable = field.GetChunk(Size)  
```  
  
## <a name="return-value"></a>Valeur de retour  
 Retourne une valeur de **type Variant**.  
  
#### <a name="parameters"></a>Paramètres  
 *Taille*  
 Expression **longue** qui est égale au nombre d’octets ou de caractères que vous souhaitez récupérer.  
  
## <a name="remarks"></a>Notes  
 Utilisez la méthode **GetChunk** sur un objet **Field** pour récupérer tout ou partie de ses données binaires ou caractères longues. Dans les situations où la mémoire système est limitée, vous pouvez utiliser la méthode **GetChunk** pour manipuler des valeurs longues en plusieurs parties, plutôt que dans leur intégralité.  
  
 Les données retournées par un appel à **GetChunk** sont affectées à une *variable*. Si la *taille* est supérieure aux données restantes, la méthode **GetChunk** retourne uniquement les données restantes sans *variable* de remplissage avec des espaces vides. Si le champ est vide, la méthode **GetChunk** retourne une valeur null.  
  
 Chaque appel **GetChunk** suivant récupère les données à partir de l’endroit où l’appel **GetChunk** précédent s’est arrêté. Toutefois, si vous extrayez des données d’un champ, puis que vous définissez ou lisez la valeur d’un autre champ dans l’enregistrement actif, ADO suppose que vous avez terminé la récupération des données du premier champ. Si vous renommez la méthode **GetChunk** sur le premier champ, ADO interprète l’appel comme une nouvelle opération **GetChunk** et commence la lecture à partir du début des données. L’accès aux champs d’autres objets [Recordset](./recordset-object-ado.md) qui ne sont pas des clones du premier objet **Recordset** n’interrompt pas les opérations **GetChunk** .  
  
 Si le **bit adFldLong** dans la propriété [attributes](./attributes-property-ado.md) d’un objet de **champ** a la valeur **true**, vous pouvez utiliser la méthode **GetChunk** pour ce champ.  
  
 S’il n’existe aucun enregistrement en cours lorsque vous utilisez la méthode **GetChunk** sur un objet **Field** , l’erreur 3021 (aucun enregistrement en cours) se produit.  
  
> [!NOTE]
>  La méthode **GetChunk** ne fonctionne pas sur les objets **Field** d’un objet [Record](./record-object-ado.md) . Elle n’exécute aucune opération et génère une erreur d’exécution.  
  
## <a name="applies-to"></a>S'applique à  
 [Objet Field](./field-object.md)  
  
## <a name="see-also"></a>Voir aussi  
 [AppendChunk et GetChunk, exemple de méthodes (VB)](./appendchunk-and-getchunk-methods-example-vb.md)   
 [AppendChunk et GetChunk, exemple de méthodes (VC + +)](./appendchunk-and-getchunk-methods-example-vc.md)   
 [AppendChunk, méthode (ADO)](./appendchunk-method-ado.md)   
 [Attributes, propriété (ADO)](./attributes-property-ado.md)