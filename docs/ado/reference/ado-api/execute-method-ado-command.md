---
description: Execute, méthode (commande ADO)
title: Execute, méthode (commande ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Command15::Execute
- Command15::raw_Execute
helpviewer_keywords:
- Execute method [ADO]
ms.assetid: f84a5ff3-0528-4ad7-9bea-9a15103378dd
author: rothja
ms.author: jroth
ms.openlocfilehash: 74e86bfb03ca77d103594009b7ddbbddb6b5b894
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100034159"
---
# <a name="execute-method-ado-command"></a>Execute, méthode (commande ADO)
Exécute la requête, l’instruction SQL ou la procédure stockée spécifiée dans la propriété [CommandText](../../../ado/reference/ado-api/commandtext-property-ado.md) ou [CommandStream](../../../ado/reference/ado-api/commandstream-property-ado.md) de l' [objet Command](../../../ado/reference/ado-api/command-object-ado.md).  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
Set recordset = command.Execute( RecordsAffected, Parameters, Options )  
```  
  
## <a name="return-value"></a>Valeur de retour  
 Retourne une référence [d’objet Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) , un flux, ou **rien**.  
  
#### <a name="parameters"></a>Paramètres  
 *RecordsAffected*  
 facultatif. Variable de **type long** à laquelle le fournisseur retourne le nombre d’enregistrements affectés par l’opération. Le paramètre *RecordsAffected* s’applique uniquement aux requêtes d’action ou aux procédures stockées. *RecordsAffected* ne retourne pas le nombre d’enregistrements renvoyés par une requête ou une procédure stockée qui retourne un résultat. Pour obtenir ces informations, utilisez la propriété [RecordCount](../../../ado/reference/ado-api/recordcount-property-ado.md) . La méthode **Execute** ne retourne pas les informations correctes lorsqu’elle est utilisée avec **adAsyncExecute**, simplement parce que lorsqu’une commande est exécutée de manière asynchrone, le nombre d’enregistrements affectés n’est peut-être pas encore connu au moment du retour de la méthode.  
  
 *Paramètres*  
 facultatif. Tableau **Variant** de valeurs de paramètre utilisées conjointement avec la chaîne d’entrée ou le flux spécifié dans **CommandText** ou **CommandStream**. (Les paramètres de sortie ne retournent pas les valeurs correctes lorsqu’ils sont passés dans cet argument.)  
  
 *Options*  
 facultatif. Valeur de **type long** qui indique comment le fournisseur doit évaluer le [CommandText](../../../ado/reference/ado-api/commandtext-property-ado.md) ou la propriété [CommandStream](../../../ado/reference/ado-api/commandstream-property-ado.md) de l’objet [Command](../../../ado/reference/ado-api/command-object-ado.md) . Peut être une valeur de masque de masque créée à l’aide de valeurs [CommandTypeEnum](../../../ado/reference/ado-api/commandtypeenum.md) et/ou [ExecuteOptionEnum](../../../ado/reference/ado-api/executeoptionenum.md) . Par exemple, vous pouvez utiliser **adCmdText** et **adExecuteNoRecords** en combinaison si vous souhaitez qu’ADO évalue la valeur de la propriété **CommandText** en tant que texte, et indique que la commande doit ignorer et ne pas retourner les enregistrements qui peuvent être générés lors de l’exécution du texte de la commande.  
  
> [!NOTE]
>  Utilisez la  valeur ExecuteOptionEnum **adExecuteNoRecords** pour améliorer les performances en minimisant le traitement interne. Si **adExecuteStream** a été spécifié, les options **adAsyncFetch** et **adAsynchFetchNonBlocking** sont ignorées. N’utilisez pas les valeurs **CommandTypeEnum** de **adCmdFile** ou **adCmdTableDirect** avec l' **instruction EXECUTE**. Ces valeurs ne peuvent être utilisées qu’en tant qu’options avec les méthodes d' [ouverture](../../../ado/reference/ado-api/open-method-ado-recordset.md) et de [rerequête](../../../ado/reference/ado-api/requery-method.md) d’un **Recordset**.  
  
## <a name="remarks"></a>Notes  
 L’utilisation de la méthode **Execute** sur un objet **Command** exécute la requête spécifiée dans la propriété **CommandText** ou **CommandStream** de l’objet.  
  
 Les résultats sont retournés dans un **jeu d’enregistrements** (par défaut) ou sous la forme d’un flux d’informations binaires. Pour obtenir un flux binaire, spécifiez **adExecuteStream** dans *options*, puis fournissez un flux en définissant **Command. Properties (« flux de sortie »)**. Un objet de **flux** ADO peut être spécifié pour recevoir les résultats, ou un autre objet de flux, tel que l’objet de réponse IIS, peut être spécifié. Si aucun flux n’a été spécifié avant d’appeler **Execute** avec **adExecuteStream**, une erreur se produit. La position du flux au retour de l' **instruction EXECUTE** est spécifique au fournisseur.  
  
 Si la commande n’est pas destinée à retourner des résultats (par exemple, une requête de mise à jour SQL), le fournisseur ne retourne **rien** tant que l’option **adExecuteNoRecords** est spécifiée. Sinon, la méthode Execute retourne un **jeu d’enregistrements** fermé. Certains langages d’application vous permettent d’ignorer cette valeur de retour si aucun **Recordset** n’est souhaité.  
  
 L' **instruction EXECUTE** génère une erreur si l’utilisateur spécifie une valeur pour **CommandStream** lorsque **CommandType** est **adCmdStoredProc**, **adCmdTable** ou **adCmdTableDirect**.  
  
 Si la requête possède des paramètres, les valeurs actuelles des paramètres de l’objet de **commande** sont utilisées, sauf si vous les remplacez par des valeurs de paramètre passées avec l’appel d' **exécution** . Vous pouvez substituer un sous-ensemble des paramètres en omettant les nouvelles valeurs de certains des paramètres lors de l’appel de la méthode **Execute** . L’ordre dans lequel vous spécifiez les paramètres correspond à l’ordre dans lequel la méthode les passe. Par exemple, si vous avez quatre (ou plus) paramètres et que vous souhaitez passer de nouvelles valeurs uniquement pour le premier et le quatrième paramètres, vous devez passer `Array(var1,,,var4)` comme argument de *paramètres* .  
  
> [!NOTE]
>  Les paramètres de sortie ne retournent pas les valeurs correctes quand elles sont passées dans l’argument *Parameters* .  
  
 Un événement [ExecuteComplete](../../../ado/reference/ado-api/executecomplete-event-ado.md) est émis lorsque cette opération se termine.  
  
> [!NOTE]
>  Lorsque vous émettez des commandes contenant des URL, celles qui utilisent le schéma http appellent automatiquement le [fournisseur Microsoft OLE DB pour la publication Internet](../../../ado/guide/appendixes/microsoft-ole-db-provider-for-internet-publishing.md). Pour plus d’informations, consultez [URL absolues et relatives](../../../ado/guide/data/absolute-and-relative-urls.md).  
  
## <a name="applies-to"></a>S'applique à  
 [Command, objet (ADO)](../../../ado/reference/ado-api/command-object-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Execute, Requery et Clear, exemples de méthodes (VB)](../../../ado/reference/ado-api/execute-requery-and-clear-methods-example-vb.md)   
 [Execute, Requery et Clear, exemple de méthode (VBScript)](../../../ado/reference/ado-api/execute-requery-and-clear-methods-example-vbscript.md)   
 [Execute, Requery et Clear, exemples de méthodes (VC + +)](../../../ado/reference/ado-api/execute-requery-and-clear-methods-example-vc.md)   
 [CommandStream, propriété (ADO)](../../../ado/reference/ado-api/commandstream-property-ado.md)   
 [CommandText, propriété (ADO)](../../../ado/reference/ado-api/commandtext-property-ado.md)   
 [CommandTypeEnum](../../../ado/reference/ado-api/commandtypeenum.md)   
 [Execute, méthode (connexion ADO)](../../../ado/reference/ado-api/execute-method-ado-connection.md)   
 [ExecuteComplete, événement (ADO)](../../../ado/reference/ado-api/executecomplete-event-ado.md)
