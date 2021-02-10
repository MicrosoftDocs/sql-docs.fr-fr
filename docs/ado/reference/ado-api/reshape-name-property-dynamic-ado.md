---
description: Reshape Name, propriété dynamique (ADO)
title: Reformer le nom Property-Dynamic (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
helpviewer_keywords:
- Reshape Name property [ADO]
ms.assetid: 690229d1-46cc-42e6-a57d-4438251fe248
author: rothja
ms.author: jroth
ms.openlocfilehash: 3af587c222e093a7918f7e390d66e64380801016
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100051600"
---
# <a name="reshape-name-property-dynamic-ado"></a>Reshape Name, propriété dynamique (ADO)
Spécifie un nom pour l’objet [Recordset](./recordset-object-ado.md) .  
  
## <a name="return-values"></a>Valeurs de retour  
 Retourne une valeur de **chaîne** qui est le nom de l’ensemble d' **enregistrements**.  
  
## <a name="remarks"></a>Notes  
 Les noms sont conservés pendant la durée de la connexion ou jusqu’à la fermeture du **Recordset** .  
  
 La propriété **Remodel Name** est principalement destinée à être utilisée avec la nouvelle fonctionnalité de mise en forme de [Microsoft Data shaping service pour OLE DB](../../guide/appendixes/microsoft-data-shaping-service-for-ole-db-ado-service-provider.md) fournisseur de services. Les noms doivent être uniques pour participer à la remise en forme.  
  
 Cette propriété est en lecture seule, mais peut être définie indirectement lors de la création d’un **jeu d’enregistrements** . Par exemple, si une clause d’une commande SHAPE crée un **Recordset** et lui attribue un nom d’alias à l’aide du mot clé **As** , l’alias est affecté à la propriété **Remodel Name** . Si aucun alias n’est déclaré, la propriété **Remodel Name** reçoit un nom unique généré par le service de mise en forme des données. Si le nom d’alias est identique au nom d’un **jeu d’enregistrements** existant, aucun **jeu d’enregistrements** ne peut être remodelé tant que l’un d’eux n’est pas libéré. Le comportement par défaut peut être modifié en définissant un nom unique dans la propriété **Remodel Name** de la connexion ADO sur **true**. La définition de cette propriété donne au service de mise en forme des données la permission de modifier le nom attribué à l’utilisateur, si nécessaire, pour garantir l’unicité. Pour plus d’informations sur la mise en forme, consultez [Microsoft Data Shaping Service pour OLE DB (fournisseur de services ADO)](../../guide/appendixes/microsoft-data-shaping-service-for-ole-db-ado-service-provider.md).  
  
 Utilisez la propriété **reformer le nom** lorsque vous souhaitez faire référence à un **jeu d’enregistrements** dans une commande Shape ou lorsque vous ne connaissez pas le nom, car il a été généré par le service de mise en forme des données. Dans ce cas, vous pouvez générer une commande SHAPE en concaténant la commande autour de la chaîne retournée par la propriété **Remodel Name** .  
  
 **Remodel Name** est une propriété dynamique ajoutée à la collection [Properties](./properties-collection-ado.md) de l’objet **Recordset** lorsque la propriété [CursorLocation](./cursorlocation-property-ado.md) a la valeur **adUseClient**.  
  
## <a name="applies-to"></a>S'applique à  
 [Recordset, objet (ADO)](./recordset-object-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Microsoft Data Shaping Service pour OLE DB (fournisseur de services ADO)](../../guide/appendixes/microsoft-data-shaping-service-for-ole-db-ado-service-provider.md)   
 [Mettre en forme les commandes en général](../../guide/data/shape-commands-in-general.md)   
 [Recordset, objet (ADO)](./recordset-object-ado.md)