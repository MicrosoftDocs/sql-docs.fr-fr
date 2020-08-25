---
description: CommandText, propriété (ADO)
title: CommandText, propriété (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
apitype: COM
f1_keywords:
- Command15::CommandText
helpviewer_keywords:
- CommandText property [ADO]
ms.assetid: 4dd7e82a-8da5-4a4e-b439-11a29286fa0e
author: rothja
ms.author: jroth
ms.openlocfilehash: e23ae5bffca27d7ad9940fb4f03df81645094792
ms.sourcegitcommit: 7345e4f05d6c06e1bcd73747a4a47873b3f3251f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/24/2020
ms.locfileid: "88776118"
---
# <a name="commandtext-property-ado"></a>CommandText, propriété (ADO)
Indique le texte d’une commande à délivrer à un fournisseur.  
  
## <a name="settings-and-return-values"></a>Paramètres et valeurs de retour  
 Obtient ou définit une valeur de **chaîne** qui contient une commande de fournisseur, telle qu’une instruction SQL, un nom de table, une URL relative ou un appel de procédure stockée. La valeur par défaut est la chaîne vide ("").  
  
## <a name="remarks"></a>Notes  
 Utilisez la propriété **CommandText** pour définir ou retourner le texte d’une commande représentée par un objet [Command](./command-object-ado.md) . Il s’agit généralement d’une instruction SQL, mais peut également être n’importe quel autre type d’instruction de commande reconnu par le fournisseur, tel qu’un appel de procédure stockée. Une instruction SQL doit être du dialecte ou de la version particuliers pris en charge par le processeur de requêtes du fournisseur.  
  
 Si la propriété [Prepared](./prepared-property-ado.md) de l’objet **Command** est définie sur **true** et que l’objet **Command** est lié à une connexion ouverte lorsque vous définissez la propriété **CommandText** , ADO prépare la requête (autrement dit, un formulaire compilé de la requête stockée par le fournisseur) lorsque vous appelez les méthodes [Execute](./execute-method-ado-command.md) ou [Open](./open-method-ado-connection.md) .  
  
 En fonction du paramètre de propriété [CommandType](./commandtype-property-ado.md) , ADO peut modifier la propriété **CommandText** . Vous pouvez lire la propriété **CommandText** à tout moment pour voir le texte de la commande que ADO utilisera lors de l’exécution.  
  
 Utilisez la propriété **CommandText** pour définir ou retourner une URL relative qui spécifie une ressource, telle qu’un fichier ou un répertoire. La ressource est relative à un emplacement spécifié explicitement par une URL absolue, ou implicitement par un objet de [connexion](./connection-object-ado.md) ouvert.  
  
> [!NOTE]
>  Les URL utilisant le schéma http appellera automatiquement le [fournisseur Microsoft OLE DB pour la publication Internet](../../guide/appendixes/microsoft-ole-db-provider-for-internet-publishing.md). Pour plus d’informations, consultez [URL absolues et relatives](../../guide/data/absolute-and-relative-urls.md).  
  
## <a name="applies-to"></a>S'applique à  
 [Command, objet (ADO)](./command-object-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [ActiveConnection, CommandText, CommandTimeout, CommandType, size et direction, exemple de propriétés (VB)](./activeconnection-commandtext-commandtimeout-commandtype-size-example-vb.md)   
 [ActiveConnection, CommandText, CommandTimeout, CommandType, size et direction, exemple de propriétés (VC + +)](./activeconnection-commandtext-commandtimeout-commandtype-size-example-vc.md)   
 [Requery, méthode](./requery-method.md)   
 [ActiveConnection, CommandText, CommandTimeout, CommandType, size et direction, exemple de propriétés (JScript)](./activeconnection-commandtext-timeout-type-size-example-jscript.md)