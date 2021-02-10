---
description: BOF, EOF, propriétés (ADO)
title: BOF, EOF, propriétés (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset15::BOF
- Recordset15::EOF
helpviewer_keywords:
- EOF property [ADO]
- BOF property [ADO]
ms.assetid: 36c31ab2-f3b6-4281-89b6-db7e04e38fd2
author: rothja
ms.author: jroth
ms.openlocfilehash: 8dd77c7bd3e7c92ec5eb970d0134db4e6e39258f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100035489"
---
# <a name="bof-eof-properties-ado"></a>BOF, EOF, propriétés (ADO)
-   **BOF** Indique que la position actuelle de l’enregistrement est antérieure au premier enregistrement d’un objet [Recordset](./recordset-object-ado.md) .  
  
-   **EOF** Indique que la position actuelle de l’enregistrement est après le dernier enregistrement d’un objet **Recordset** .  
  
## <a name="return-value"></a>Valeur de retour  
 Les propriétés **BOF** et **EOF** retournent des valeurs **booléennes** .  
  
## <a name="remarks"></a>Notes  
 Utilisez les propriétés **BOF** et **EOF** pour déterminer si un objet **Recordset** contient des enregistrements ou si vous avez dépassé les limites d’un objet **Recordset** lorsque vous passez d’un enregistrement à l’autre.  
  
 La propriété **BOF** retourne **true** (-1) si la position actuelle de l’enregistrement est antérieure au premier enregistrement et **false** (0) si la position actuelle de l’enregistrement est égale ou postérieure au premier enregistrement.  
  
 La propriété **EOF** retourne la **valeur true** si la position actuelle de l’enregistrement est après le dernier enregistrement et la **valeur false** si la position actuelle de l’enregistrement est le ou avant le dernier enregistrement.  
  
 Si la propriété **BOF** ou **EOF** a la **valeur true**, il n’y a pas d’enregistrement actif.  
  
 Si vous ouvrez un objet **Recordset** qui ne contient aucun enregistrement, les propriétés **BOF** et **EOF** ont la valeur **true** (pour plus d’informations sur cet état d’un **Recordset**, consultez la propriété [RecordCount](./recordcount-property-ado.md) ). Lorsque vous ouvrez un objet **Recordset** qui contient au moins un enregistrement, le premier enregistrement est l’enregistrement actif et les propriétés **BOF** et **EOF** ont la **valeur false**.  
  
 Si vous supprimez le dernier enregistrement restant dans l’objet **Recordset** , les propriétés **BOF** et **EOF** peuvent rester **false** jusqu’à ce que vous tentiez de repositionner l’enregistrement actif.  
  
 Ce tableau montre les méthodes **Move** autorisées avec différentes combinaisons des propriétés **BOF** et **EOF** .  
  
||MoveFirst<br /><br /> MoveLast|MovePrevious<br /><br /> Déplacer < 0|Déplacer 0|MoveNext<br /><br /> Déplacer > 0|  
|------|-----------------------------|---------------------------------|------------|-----------------------------|  
|**BOF** = **True**, **EOF** = **false**|Autorisé|Error|Error|Autorisé|  
|**BOF** = **False**, **EOF** = **true**|Autorisé|Autorisé|Error|Error|  
|Les deux sont **vraies**|Error|Error|Error|Error|  
|Les deux **fausses**|Autorisé|Autorisé|Autorisé|Autorisé|  
  
 L’autorisation d’une méthode **Move** ne garantit pas que la méthode parviendra à trouver un enregistrement. Cela signifie uniquement que l’appel de la méthode **Move** spécifiée ne génère pas d’erreur.  
  
 Le tableau suivant montre ce qui se passe aux paramètres de propriété **BOF** et **EOF** lorsque vous appelez différentes méthodes **Move** , mais que vous ne parvenez pas à localiser un enregistrement.  
  
||Business|EOF|  
|------|---------|---------|  
|**MoveFirst**, **MoveLast**|Définir sur **true**|Définir sur **true**|  
|**Déplacer** 0|Aucun changement|Aucun changement|  
|**MovePrevious**, **déplacer** < 0|Définir sur **true**|Aucun changement|  
|**MoveNext**, **Move** > 0|Aucun changement|Définir sur **true**|  
  
## <a name="applies-to"></a>S'applique à  
 [Recordset, objet (ADO)](./recordset-object-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [BOF, EOF et Bookmark, exemple de propriétés (VB)](./bof-eof-and-bookmark-properties-example-vb.md)   
 [BOF, EOF et Bookmark, exemple de propriétés (VC + +)](./bof-eof-and-bookmark-properties-example-vc.md)