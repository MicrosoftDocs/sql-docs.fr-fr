---
description: DefaultDatabase, propriété
title: Propriété DefaultDatabase | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Connection15::DefaultDatabase
helpviewer_keywords:
- DefaultDatabase property
ms.assetid: 41e8a8dd-e69c-4a09-8736-93502e01961c
author: rothja
ms.author: jroth
ms.openlocfilehash: db35621095f74e2dd1905d0cc5803cbefcf5f2a6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100034419"
---
# <a name="defaultdatabase-property"></a>DefaultDatabase, propriété
Indique la base de données par défaut pour un objet de [connexion](../../../ado/reference/ado-api/connection-object-ado.md) .  
  
## <a name="settings-and-return-values"></a>Paramètres et valeurs de retour  
 Définit ou retourne une valeur de **chaîne** qui prend la valeur du nom d’une base de données disponible à partir du fournisseur.  
  
## <a name="remarks"></a>Notes  
 Utilisez la propriété **DefaultDatabase** pour définir ou retourner le nom de la base de données par défaut sur un objet de **connexion** spécifique.  
  
 S’il existe une base de données par défaut, les chaînes SQL peuvent utiliser une syntaxe non qualifiée pour accéder aux objets de cette base de données. Pour accéder à des objets dans une base de données autre que celle spécifiée dans la propriété **DefaultDatabase** , vous devez qualifier les noms d’objets avec le nom de la base de données souhaitée. Lors de la connexion, le fournisseur écrit les informations de base de données par défaut dans la propriété **DefaultDatabase** . Certains fournisseurs n’autorisent qu’une seule base de données par connexion, auquel cas vous ne pouvez pas modifier la propriété **DefaultDatabase** .  
  
 Certaines sources de données et fournisseurs peuvent ne pas prendre en charge cette fonctionnalité et peuvent retourner une erreur ou une chaîne vide.  
  
> [!NOTE]
>  **Utilisation des services de données distants** Cette propriété n’est pas disponible sur un objet de **connexion** côté client.  
  
## <a name="applies-to"></a>S'applique à  
 [Connection, objet (ADO)](../../../ado/reference/ado-api/connection-object-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Provider et DefaultDatabase, exemple de propriétés (VB)](../../../ado/reference/ado-api/provider-and-defaultdatabase-properties-example-vb.md)   
 [Provider et DefaultDatabase, exemple de propriétés (VC++)](../../../ado/reference/ado-api/provider-and-defaultdatabase-properties-example-vc.md)   
