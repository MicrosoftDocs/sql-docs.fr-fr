---
description: ConnectionTimeout, propriété (ADO)
title: ConnectionTimeout, propriété (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Connection15::ConnectionTimeout
helpviewer_keywords:
- ConnectionTimeout property [ADO]
ms.assetid: 8904a403-1383-4b4b-b53d-5c01d6f5deac
author: rothja
ms.author: jroth
ms.openlocfilehash: 266d57defde6abaae34e558496092ec2df7caebb
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99164589"
---
# <a name="connectiontimeout-property-ado"></a>ConnectionTimeout, propriété (ADO)
Indique le délai d’attente lors de l’établissement d’une connexion avant que la tentative ne soit abandonnée et qu’une erreur ne soit générée.  
  
## <a name="settings-and-return-values"></a>Paramètres et valeurs de retour  
 Définit ou retourne une valeur de **type long** qui indique, en secondes, le délai d’attente de l’ouverture de la connexion. La valeur par défaut est 15.  
  
## <a name="remarks"></a>Notes  
 Utilisez la propriété **ConnectionTimeout** sur un objet de [connexion](./connection-object-ado.md) si des retards de trafic réseau ou une utilisation intensive du serveur rendent nécessaire l’abandon d’une tentative de connexion. Si l’heure du paramètre de propriété **ConnectionTimeout** s’écoule avant l’ouverture de la connexion, une erreur se produit et ADO annule la tentative. Si vous affectez à la propriété la valeur zéro, ADO attend indéfiniment que la connexion soit ouverte. Assurez-vous que le fournisseur dans lequel vous écrivez le code prend en charge la fonctionnalité **ConnectionTimeout** .  
  
 La propriété **ConnectionTimeout** est en lecture/écriture lorsque la connexion est fermée et en lecture seule lorsqu’elle est ouverte.  
  
## <a name="applies-to"></a>S'applique à  
 [Connection, objet (ADO)](./connection-object-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [ConnectionString, ConnectionTimeout et State, exemple de propriétés (VB)](./connectionstring-connectiontimeout-and-state-properties-example-vb.md)   
 [ConnectionString, ConnectionTimeout et State, exemple de propriétés (VC + +)](./connectionstring-connectiontimeout-and-state-properties-example-vc.md)   
 [CommandTimeout, propriété (ADO)](./commandtimeout-property-ado.md)