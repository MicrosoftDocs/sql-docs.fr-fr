---
description: Prepared, propriété (ADO)
title: Prepared, propriété (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Command15::Prepared
helpviewer_keywords:
- Prepared property [ADO]
ms.assetid: 11ca8825-765e-4bb4-a6ce-3f6564ad8755
author: rothja
ms.author: jroth
ms.openlocfilehash: f20fb9b9f4050ebb0f382b2e36e2f7d5b6231fa9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99170553"
---
# <a name="prepared-property-ado"></a>Prepared, propriété (ADO)
Indique s’il faut enregistrer une version compilée d’une [commande](./command-object-ado.md) avant l’exécution.  
  
## <a name="settings-and-return-values"></a>Paramètres et valeurs de retour  
 Définit ou retourne une valeur **booléenne** qui, si elle a la valeur **true**, indique que la commande doit être préparée.  
  
## <a name="remarks"></a>Notes  
 Utilisez la propriété **Prepared** pour que le fournisseur enregistre une version préparée (ou compilée) de la requête spécifiée dans la propriété [CommandText](./commandtext-property-ado.md) avant la première exécution d’un objet [Command](./command-object-ado.md) . Cela peut ralentir la première exécution d’une commande, mais une fois que le fournisseur compile une commande, le fournisseur utilise la version compilée de la commande pour toutes les exécutions ultérieures, ce qui entraîne une amélioration des performances.  
  
 Si la propriété a la **valeur false**, le fournisseur exécutera directement l’objet de **commande** sans créer une version compilée.  
  
 Si le fournisseur ne prend pas en charge la préparation de la commande, il peut retourner une erreur lorsque cette propriété a la valeur **true**. Si le fournisseur ne retourne pas d’erreur, il ignore simplement la demande de préparation de la commande et affecte la **valeur false** à la propriété **Prepared** .  
  
## <a name="applies-to"></a>S'applique à  
 [Command, objet (ADO)](./command-object-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Prepared, exemple de propriété (VB)](./prepared-property-example-vb.md)   
 [Prepared, exemple de propriété (VC++)](./prepared-property-example-vc.md)