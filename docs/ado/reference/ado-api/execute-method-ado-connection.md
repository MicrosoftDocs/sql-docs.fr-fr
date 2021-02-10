---
description: Execute, méthode (objet Connection ADO)
title: Execute, méthode (connexion ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Connection15::Execute
- Connection15::raw_Execute
helpviewer_keywords:
- Execute method [ADO]
ms.assetid: 03c69320-96b2-4d85-8d49-a13b13e31578
author: rothja
ms.author: jroth
ms.openlocfilehash: c91fd13744afd106b2a0cb8e986d803f1b722c24
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100025103"
---
# <a name="execute-method-ado-connection"></a>Execute, méthode (objet Connection ADO)
Exécute la requête, l’instruction SQL, la procédure stockée ou le texte spécifique au fournisseur spécifié.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
Set recordset = connection.Execute (CommandText, RecordsAffected, Options)  
Set recordset = connection.Execute (CommandText, RecordsAffected, Options)  
```  
  
## <a name="return-value"></a>Valeur de retour  
 Retourne une référence [d’objet Recordset Object (ADO)](../../../ado/reference/ado-api/recordset-object-ado.md) .  
  
#### <a name="parameters"></a>Paramètres  
 *CommandText*  
 Valeur de **chaîne** qui contient l’instruction SQL, la procédure stockée, une URL ou un texte spécifique au fournisseur à exécuter. Si **vous le souhaitez**, les noms de table peuvent être utilisés, mais uniquement si le fournisseur prend en charge SQL. Par exemple, si le nom de table « Customers » est utilisé, ADO ajoutera automatiquement la syntaxe SQL SELECT standard pour former et passer « SELECT * FROM Customers » comme [!INCLUDE[tsql](../../../includes/tsql-md.md)] instruction au fournisseur.  
  
 *RecordsAffected*  
 facultatif. Variable de **type long** à laquelle le fournisseur retourne le nombre d’enregistrements affectés par l’opération.  
  
 *Options*  
 facultatif. Valeur de **type long** qui indique comment le fournisseur doit évaluer l’argument CommandText. Peut être un masque de masque d’une ou plusieurs valeurs [CommandTypeEnum](../../../ado/reference/ado-api/commandtypeenum.md) ou [ExecuteOptionEnum](../../../ado/reference/ado-api/executeoptionenum.md) .  
  
 **Remarque** Utilisez la  valeur ExecuteOptionEnum **adExecuteNoRecords** pour améliorer les performances en minimisant le traitement interne et pour les applications que vous migrez à partir de Visual Basic 6,0.  
  
 N’utilisez pas **adExecuteStream** avec la méthode **Execute** d’un objet **Connection** .  
  
 N’utilisez pas les valeurs CommandTypeEnum de adCmdFile ou adCmdTableDirect avec l’instruction EXECUTE. Ces valeurs ne peuvent être utilisées qu’en tant qu’options avec la [méthode Open (ADO Recordset)](../../../ado/reference/ado-api/open-method-ado-recordset.md) et les méthodes [Requery de méthode](../../../ado/reference/ado-api/requery-method.md) d’un **Recordset**.  
  
## <a name="remarks"></a>Notes  
 L’utilisation de la méthode **Execute** sur un objet de [connexion (ADO)](../../../ado/reference/ado-api/connection-object-ado.md) exécute toute requête que vous transmettez à la méthode dans l’argument CommandText sur la connexion spécifiée. Si l’argument CommandText spécifie une requête qui retourne des lignes, les résultats générés par l’exécution sont stockés dans un nouvel objet **Recordset** . Si la commande n’est pas destinée à retourner des résultats (par exemple, une requête de mise à jour SQL), le fournisseur ne retourne **rien** tant que l’option **adExecuteNoRecords** est spécifiée. Sinon, la méthode Execute retourne un **jeu d’enregistrements** fermé.  
  
 L’objet **Recordset** retourné est toujours un curseur en lecture seule et avant uniquement. Si vous avez besoin d’un objet **Recordset** avec davantage de fonctionnalités, commencez par créer un objet **Recordset** avec les paramètres de propriété souhaités, puis utilisez la méthode Open de l’objet **Recordset** [(ADO Recordset)](../../../ado/reference/ado-api/open-method-ado-recordset.md) pour exécuter la requête et retourner le type de curseur souhaité.  
  
 Le contenu de l’argument *CommandText* est spécifique au fournisseur et peut être une syntaxe SQL standard ou tout format de commande spécial pris en charge par le fournisseur.  
  
 Un événement ExecuteComplete est émis lorsque cette opération se termine.  
  
> [!NOTE]
>  Les URL utilisant le schéma http appellera automatiquement le [fournisseur Microsoft OLE DB pour la publication Internet](../../../ado/guide/appendixes/microsoft-ole-db-provider-for-internet-publishing.md). Pour plus d’informations, consultez [URL absolues et relatives](../../../ado/guide/data/absolute-and-relative-urls.md).  
  
## <a name="applies-to"></a>S'applique à  
 [Connection, objet (ADO)](../../../ado/reference/ado-api/connection-object-ado.md)
