---
description: MarshalOptions, propriété (ADO)
title: MarshalOptions, propriété (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset15::MarshalOptions
helpviewer_keywords:
- MarshalOptions property [ADO]
ms.assetid: 390c8abf-133e-40da-8b99-8f748a983e4f
author: rothja
ms.author: jroth
ms.openlocfilehash: 8797d968a342a8e68f3d3c3e19319d6a1bdc2505
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100041799"
---
# <a name="marshaloptions-property-ado"></a>MarshalOptions, propriété (ADO)
Indique les enregistrements du [Recordset](./recordset-object-ado.md) qui doivent être remarshalés sur le serveur.  
  
## <a name="settings-and-return-values"></a>Paramètres et valeurs de retour  
 Définit ou retourne une valeur [MarshalOptionsEnum](./marshaloptionsenum.md) . La valeur par défaut est **adMarshalAll**.  
  
## <a name="remarks"></a>Notes  
 Lors de l’utilisation d’un [Recordset](./recordset-object-ado.md)côté client, les enregistrements qui ont été modifiés sur le client sont réécrits sur la couche intermédiaire ou le serveur Web via une technique appelée marshaling, processus d’empaquetage et d’envoi de paramètres de méthode d’interface à travers les limites de thread ou de processus. La définition de la propriété **MarshalOptions** peut améliorer les performances lorsque les données distantes modifiées sont marshalées pour être mises à jour vers la couche intermédiaire ou le serveur Web.  
  
> [!NOTE]
>  **Utilisation des services de données distants** Cette propriété est utilisée uniquement sur un **jeu d’enregistrements** côté client.  
  
## <a name="applies-to"></a>S'applique à  
 [Recordset, objet (ADO)](./recordset-object-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [MarshalOptions, exemple de propriété (VB)](./marshaloptions-property-example-vb.md)   
 [MarshalOptions, exemple de propriété (VC++)](./marshaloptions-property-example-vc.md)