---
description: WillExecute, événement (ADO)
title: WillExecute, événement (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- WillExecute
- Connection::WillExecute
helpviewer_keywords:
- WillExecute event [ADO]
ms.assetid: dd755e46-f589-48a3-93a9-51ff998d44b5
author: rothja
ms.author: jroth
ms.openlocfilehash: cffc2e0a79ff4bffd83eadac20cf522df6d0c9cc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99172364"
---
# <a name="willexecute-event-ado"></a>WillExecute, événement (ADO)
L’événement **WillExecute** est appelé juste avant qu’une commande en attente ne s’exécute sur une connexion.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
WillExecute Source, CursorType, LockType, Options, adStatus, pCommand, pRecordset, pConnection  
```  
  
#### <a name="parameters"></a>Paramètres  
 *Source*  
 **Chaîne** qui contient une commande SQL ou un nom de procédure stockée.  
  
 *CursorType*  
 [CursorTypeEnum](./cursortypeenum.md) qui contient le type de curseur pour le **jeu d’enregistrements** qui sera ouvert. Avec ce paramètre, vous pouvez remplacer le curseur par n’importe quel type au cours d’une opération d’ouverture **d’un jeu d’enregistrements**[(ADO Recordset)](./open-method-ado-recordset.md) . *CursorType* sera ignoré pour toute autre opération.  
  
 *Verrou*  
 [LockTypeEnum](./locktypeenum.md) qui contient le type de verrou pour le **Recordset** qui sera ouvert. Avec ce paramètre, vous pouvez remplacer le verrou par n’importe quel type pendant une opération **RecordsetOpen** . *LockType* sera ignoré pour toute autre opération.  
  
 *Options*  
 Valeur de **type long** qui indique des options qui peuvent être utilisées pour exécuter la commande ou ouvrir le **jeu d’enregistrements**.  
  
 *adStatus*  
 Valeur d’état [EventStatusEnum](./eventstatusenum.md) qui peut être **adStatusCantDeny** ou **adStatusOK** lorsque cet événement est appelé. S’il s’agit de **adStatusCantDeny**, cet événement peut ne pas demander l’annulation de l’opération en attente.  
  
 *pCommand*  
 Objet de [commande (ADO)](./command-object-ado.md) auquel cette notification d’événement s’applique.  
  
 *pRecordset*  
 Objet [Recordset Object (ADO)](./recordset-object-ado.md) auquel cette notification d’événement s’applique.  
  
 *pConnection*  
 Objet de [connexion (ADO)](./connection-object-ado.md) auquel cette notification d’événement s’applique.  
  
## <a name="remarks"></a>Notes  
 Un événement **WillExecute** peut se produire en raison d’une connexion.  Méthode [Execute (ADO Connection)](./execute-method-ado-connection.md), [Execute Method (commande ADO)](./execute-method-ado-command.md)ou [méthode Open (ADO Recordset)](./open-method-ado-recordset.md) le paramètre *pConnection* doit toujours contenir une référence valide à un objet **Connection** . Si l’événement est dû à **Connection.Exejolies**, les paramètres *prerecordset* et *pCommand* ont la valeur **Nothing**. Si l’événement est dû à **Recordset. Open**, le paramètre *Recordset* fait référence à l’objet **Recordset** et le paramètre *pCommand* a la valeur **Nothing**. Si l’événement est dû à **Command.Exejolies**, le paramètre *pCommand* fait référence à l’objet **Command** et le paramètre *Recordset* a la valeur **Nothing**.  
  
 **WillExecute** vous permet d’examiner et de modifier les paramètres d’exécution en attente. Cet événement peut retourner une demande indiquant que la commande en attente doit être annulée.  
  
> [!NOTE]
>  Si la source d’origine d’une **commande** est un flux spécifié par la propriété CommandStream de la [propriété (ADO)](./commandstream-property-ado.md) , l’affectation d’une nouvelle chaîne au paramètre _source_ **WillExecute** modifie la source de la **commande**. La propriété **CommandStream** sera effacée et la propriété [CommandText (ADO)](./commandtext-property-ado.md) sera mise à jour avec la nouvelle source. Le flux d’origine spécifié par **CommandStream** sera libéré et ne sera pas accessible.  
  
 Si le dialecte de la nouvelle chaîne source diffère du paramètre d’origine de la propriété de la [propriété dialecte](./dialect-property.md) (qui correspond à **CommandStream**), le dialecte approprié doit être spécifié en définissant la propriété **Dialect** de l’objet Command référencé par *pCommand*.  
  
## <a name="see-also"></a>Voir aussi  
 [Exemple de modèle d’événements ADO (VC + +)](./ado-events-model-example-vc.md)   
 [Résumé du gestionnaire d’événements ADO](../../guide/data/ado-event-handler-summary.md)   
 [Connection, objet (ADO)](./connection-object-ado.md)