---
description: Open, méthode (objet Connection ADO)
title: Open, méthode (connexion ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Connection15::raw_Open
- Connection15::Open
- _Connection::Open
helpviewer_keywords:
- Open method [ADO]
ms.assetid: 663defab-5545-4973-9036-24d5882c9737
author: rothja
ms.author: jroth
ms.openlocfilehash: a3bca1379c776f6fad1f4ab2aa7368b424ff9e7f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99170703"
---
# <a name="open-method-ado-connection"></a>Open, méthode (objet Connection ADO)
Ouvre une connexion à une source de données.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
connection.Open ConnectionString, UserID, Password, Options  
```  
  
#### <a name="parameters"></a>Paramètres  
 *ConnectionString*  
 Facultatif. Valeur de **chaîne** qui contient les informations de connexion. Pour plus d’informations sur les paramètres valides, consultez la propriété [ConnectionString](./connectionstring-property-ado.md) .  
  
 *l'UserId*  
 Facultatif. Valeur de **chaîne** qui contient un nom d’utilisateur à utiliser lors de l’établissement de la connexion.  
  
 *Mot de passe*  
 Facultatif. Valeur de **chaîne** qui contient un mot de passe à utiliser lors de l’établissement de la connexion.  
  
 *Options*  
 Facultatif. Valeur [ConnectOptionEnum](./connectoptionenum.md) qui détermine si cette méthode doit retourner après (de façon synchrone) ou avant (de façon asynchrone) la connexion est établie.  
  
## <a name="remarks"></a>Notes  
 L’utilisation de la méthode **Open** sur un objet [Connection](./connection-object-ado.md) établit la connexion physique à une source de données. Une fois que cette méthode s’est terminée avec succès, la connexion est active et vous pouvez émettre des commandes sur celle-ci et traiter les résultats.  
  
 Utilisez l’argument facultatif *ConnectionString* pour spécifier une chaîne de connexion contenant une série d’instructions *argument* *= value* séparées par des points-virgules, ou une ressource de fichier ou de répertoire identifiée par une URL. La propriété **ConnectionString** hérite automatiquement de la valeur utilisée pour l’argument *ConnectionString* . Par conséquent, vous pouvez soit définir la propriété **ConnectionString** de l’objet de **connexion** avant de l’ouvrir, soit utiliser l’argument *ConnectionString* pour définir ou substituer les paramètres de connexion actuels pendant l’appel de la méthode **Open** .  
  
 Si vous transmettez des informations sur l’utilisateur et le mot de passe à la fois dans l’argument *ConnectionString* et dans les arguments facultatifs *userid* et *Password* , les arguments *userid* *et Password* remplacent les valeurs spécifiées dans *ConnectionString*.  
  
 Une fois que vous avez terminé vos opérations sur une **connexion** ouverte, utilisez la méthode [Close](./close-method-ado.md) pour libérer toutes les ressources système associées. La fermeture d’un objet ne le supprime pas de la mémoire ; vous pouvez modifier ses paramètres de propriété et utiliser la méthode **Open** pour l’ouvrir à nouveau ultérieurement. Pour éliminer complètement un objet de la mémoire, affectez à la variable objet la valeur *Nothing*.  
  
> [!NOTE]
>  **Utilisation des services de données distants** Lorsqu’elle est utilisée sur un objet de **connexion** côté client, la méthode **Open** n’établit pas en fait de connexion au serveur tant qu’un [jeu d’enregistrements](./recordset-object-ado.md) n’est pas ouvert sur l’objet de **connexion** .  
  
> [!NOTE]
>  Les URL utilisant le schéma http appellera automatiquement le [fournisseur Microsoft OLE DB pour la publication Internet](../../guide/appendixes/microsoft-ole-db-provider-for-internet-publishing.md). Pour plus d’informations, consultez [URL absolues et relatives](../../guide/data/absolute-and-relative-urls.md).  
  
## <a name="applies-to"></a>S'applique à  
 [Connection, objet (ADO)](./connection-object-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Open et Close, exemple de méthodes (VB)](./open-and-close-methods-example-vb.md)   
 [Open et Close, exemple de méthode (VBScript)](./open-and-close-methods-example-vbscript.md)   
 [Open et Close, exemple de méthodes (VC + +)](./open-and-close-methods-example-vc.md)   
 [Open, méthode (ADO record)](./open-method-ado-record.md)   
 [Open, méthode (objet Recordset ADO)](./open-method-ado-recordset.md)   
 [Open, méthode (objet Stream ADO)](./open-method-ado-stream.md)   
 [OpenSchema, méthode](./openschema-method.md)