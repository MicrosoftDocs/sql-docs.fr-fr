---
description: IsolationLevel, propriété
title: Propriété IsolationLevel | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Connection15::IsolationLevel
helpviewer_keywords:
- IsolationLevel property
ms.assetid: ea84e4b2-fbf2-4eef-b9ce-796b22e21800
author: rothja
ms.author: jroth
ms.openlocfilehash: dc9f116c565321051184d16fa0cdf963cc4bfe0e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99170924"
---
# <a name="isolationlevel-property"></a>IsolationLevel, propriété
Indique le niveau d’isolation d’un objet de [connexion](./connection-object-ado.md) .  
  
## <a name="settings-and-return-values"></a>Paramètres et valeurs de retour  
 Définit ou retourne une valeur [IsolationLevelEnum](./isolationlevelenum.md) . La valeur par défaut est **adXactReadCommitted**.  
  
## <a name="remarks"></a>Notes  
 Utilisez la propriété **IsolationLevel** pour définir le niveau d’isolation d’un objet de **connexion** . Le paramètre ne prend pas effet avant la prochaine fois que vous appelez la méthode [BeginTrans](./begintrans-committrans-and-rollbacktrans-methods-ado.md) . Si le niveau d’isolation que vous demandez n’est pas disponible, le fournisseur peut retourner le niveau d’isolation supérieur suivant sans mettre à jour la propriété **IsolationLevel** .  
  
 La propriété **IsolationLevel** est en lecture/écriture.  
  
> [!NOTE]
>  **Utilisation des services de données distants** Lorsqu’elle est utilisée sur un objet de **connexion** côté client, la propriété **IsolationLevel** ne peut être définie qu’avec **adXactUnspecified**. Étant donné que les utilisateurs travaillent avec des objets **Recordset** déconnectés sur un cache côté client, il peut y avoir des problèmes multi-utilisateurs. Par exemple, lorsque deux utilisateurs différents essaient de mettre à jour le même enregistrement, le service de données distant autorise simplement l’utilisateur qui met d’abord à jour l’enregistrement à « Win ». La demande de mise à jour du deuxième utilisateur échouera avec une erreur.  
  
## <a name="applies-to"></a>S'applique à  
 [Connection, objet (ADO)](./connection-object-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [IsolationLevel et mode, exemple de propriétés (VB)](./isolationlevel-and-mode-properties-example-vb.md)   
 [IsolationLevel et mode, exemple de propriétés (VC + +)](./isolationlevel-and-mode-properties-example-vc.md)