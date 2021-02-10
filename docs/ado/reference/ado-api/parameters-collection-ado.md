---
description: Parameters, collection (ADO)
title: Parameters, collection (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Command15::get_Parameters
- Command15::Parameters
- Command15::GetParameters
helpviewer_keywords:
- Parameters collection [ADO]
ms.assetid: 497cae10-3913-422a-9753-dcbb0a639b1b
author: rothja
ms.author: jroth
ms.openlocfilehash: 93d34581e7ea19ad584161e12375101a06256cbd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100041109"
---
# <a name="parameters-collection-ado"></a>Parameters, collection (ADO)
Contient tous les objets [Parameter](./parameter-object.md) d’un objet [Command](./command-object-ado.md) .  
  
## <a name="remarks"></a>Notes  
 Un objet **Command** a une collection **Parameters** composée d’objets **Parameter** .  
  
 L’utilisation de la méthode [Refresh](./refresh-method-ado.md) sur la collection **Parameters** d’un objet **Command** récupère les informations sur les paramètres du fournisseur pour la procédure stockée ou la requête paramétrable spécifiée dans l’objet **Command** . Certains fournisseurs ne prennent pas en charge les appels de procédure stockée ou les requêtes paramétrables ; l’appel de la méthode **Refresh** sur la collection **Parameters** lors de l’utilisation d’un tel fournisseur retourne une erreur.  
  
 Si vous n’avez pas défini vos propres objets de **paramètre** et que vous accédez à la collection de **paramètres** avant d’appeler la méthode **Refresh** , ADO appelle automatiquement la méthode et remplit la collection pour vous.  
  
 Vous pouvez réduire les appels au fournisseur pour améliorer les performances si vous connaissez les propriétés des paramètres associés à la procédure stockée ou à la requête paramétrable que vous souhaitez appeler. Utilisez la méthode [CreateParameter](./createparameter-method-ado.md) pour créer des objets **Parameter** avec les paramètres de propriété appropriés et utilisez la méthode [Append](./append-method-ado.md) pour les ajouter à la collection **Parameters** . Cela vous permet de définir et de retourner des valeurs de paramètres sans avoir à appeler le fournisseur pour obtenir les informations sur les paramètres. Si vous écrivez dans un fournisseur qui ne fournit pas d’informations de paramètre, vous devez remplir manuellement la collection de **paramètres** à l’aide de cette méthode pour pouvoir utiliser des paramètres. Utilisez la méthode [Delete](./delete-method-ado-parameters-collection.md) pour supprimer des objets **Parameter** de la collection **Parameters** , si nécessaire.  
  
 Les objets de la collection **Parameters** d’un **Recordset** sont hors de portée (devenant donc indisponible) lorsque le **Recordset** est fermé.  
  
 Lors de l’appel d’une procédure stockée avec une **commande**, le paramètre de sortie/valeur de retour d’une procédure stockée est récupéré comme suit :  
  
1.  Lors de l’appel d’une procédure stockée qui n’a pas de paramètres, la méthode **Refresh** de la collection **Parameters** doit être appelée avant d’appeler la méthode **Execute** sur l’objet **Command** .  
  
2.  Lors de l’appel d’une procédure stockée avec des paramètres et de l’ajout explicite d’un paramètre à la collection **Parameters** avec **Append**, le paramètre de sortie/valeur de retour doit être ajouté à la collection **Parameters** . La valeur de retour doit d’abord être ajoutée à la collection de **paramètres** . Utilisez **Append** pour ajouter les autres paramètres dans la collection **Parameters** dans l’ordre de définition. Par exemple, la procédure stockée SPWithParam a deux paramètres. Le premier *paramètre,, est* un paramètre d’entrée défini comme adVarChar (20), tandis que le second paramètre, *subparast*, est un paramètre de sortie défini comme adVarChar (20). Vous pouvez récupérer le paramètre de sortie/valeur de retour avec le code suivant.  
  
    ```vb
    ' Open Connection Conn  
    set ccmd = CreateObject("ADODB.Command")  
    ccmd.Activeconnection= Conn  
  
    ccmd.CommandText="SPWithParam"  
    ccmd.commandType = 4 'adCmdStoredProc  
  
    ccmd.parameters.Append ccmd.CreateParameter(, adInteger, adParamReturnValue, , NULL)   ' return value  
    ccmd.parameters.Append ccmd.CreateParameter("InParam", adVarChar, adParamInput, 20, "hello world")   ' input parameter  
    ccmd.parameters.Append ccmd.CreateParameter("OutParam", adVarChar, adParamOutput, 20, NULL)   ' output parameter  
  
    ccmd.execute()  
  
    ' Access ccmd.parameters(0) as return value of this stored procedure  
    ' Access ccmd.parameters("OutParam") as the output parameter of this stored procedure.  
  
    ```  
  
3.  Lorsque vous appelez une procédure stockée avec des paramètres et configurez les paramètres en appelant la méthode **Item** sur la collection **Parameters** , vous pouvez récupérer la valeur de retour/le paramètre de sortie de la procédure stockée à partir de la collection **Parameters** . Par exemple, la procédure stockée SPWithParam a deux paramètres. Le premier *paramètre,, est* un paramètre d’entrée défini comme adVarChar (20), tandis que le second paramètre, *subparast*, est un paramètre de sortie défini comme adVarChar (20). Vous pouvez récupérer le paramètre de sortie/valeur de retour avec le code suivant.  
  
    ```vb
    ' Open Connection Conn  
    set ccmd = CreateObject("ADODB.Command")  
    ccmd.Activeconnection= Conn  
  
    ccmd.CommandText="SPWithParam"  
    ccmd.commandType = 4 'adCmdStoredProc  
  
    ccmd.parameters.Item("InParam").value = "hello world" ' input parameter  
    ccmd.execute()  
  
    ' Access ccmd.parameters(0) as return value of stored procedure  
    ' Access ccmd.parameters(2) or ccmd.parameters("OutParam") as the output parameter.  
    ```  
  
 Cette section contient la rubrique suivante.  
  
-   [Propriétés, méthodes et événements de la collection Parameters](./parameters-collection-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Append, méthode (ADO)](./append-method-ado.md)   
 [CreateParameter, méthode (ADO)](./createparameter-method-ado.md)   
 [Objet Parameter](./parameter-object.md)