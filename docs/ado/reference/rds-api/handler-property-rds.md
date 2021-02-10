---
description: Handler, propriété (RDS)
title: Handler, propriété (RDS) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
helpviewer_keywords:
- Handler property [ADO]
ms.assetid: fdc34362-6d47-4727-b171-8d033159408e
author: rothja
ms.author: jroth
ms.openlocfilehash: 4829eadfc91be74ae3f487d9f315162788b7ca0e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100053120"
---
# <a name="handler-property-rds"></a>Handler, propriété (RDS)
Indique le nom d’un programme de personnalisation côté serveur (gestionnaire) qui étend les fonctionnalités de [RDSServer. DataFactory](./datafactory-object-rdsserver.md)et de tous les paramètres utilisés par le *Gestionnaire*.  
  
 **S’applique à :** [DataControl, objet (RDS)](./datacontrol-object-rds.md)  
  
> [!IMPORTANT]
>  À compter de Windows 8 et de Windows Server 2012, les composants serveur RDS ne sont plus inclus dans le système d’exploitation Windows (pour plus d’informations, consultez le livre de recettes sur la compatibilité avec Windows 8 et [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) ). Les composants clients RDS seront supprimés dans une prochaine version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Les applications qui utilisent RDS doivent migrer vers le [service de données WCF](/dotnet/framework/wcf/).  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
DataControl.Handler = String  
```  
  
#### <a name="parameters"></a>Paramètres  
 *DataControl*  
 Variable objet qui représente un objet [RDS. DataControl](./datacontrol-object-rds.md) .  
  
 *Chaîne*  
 Valeur de **chaîne** qui contient le nom du gestionnaire et des paramètres, tous séparés par des virgules (par exemple, `"handlerName,parm1,parm2,...,parm` *N* `"` ).  
  
## <a name="remarks"></a>Notes  
 Cette propriété prend en charge la [personnalisation](../../guide/remote-data-service/datafactory-customization.md), une fonctionnalité qui requiert la définition de la propriété [CursorLocation](../ado-api/cursorlocation-property-ado.md) sur **adUseClient**.  
  
 Le nom du gestionnaire et ses paramètres, le cas échéant, sont séparés par des virgules (","). Un comportement imprévisible se produira si un point-virgule (« ; ») apparaît n’importe où dans la *chaîne*. Vous pouvez écrire votre propre gestionnaire, à condition qu’il prenne en charge l’interface **IDataFactoryHandler** .  
  
 Le nom du gestionnaire par défaut est **msdfmap. Et son** paramètre par défaut est un fichier de personnalisation nommé **MSDFMAP.INI**. Utilisez cette propriété pour appeler d’autres fichiers de personnalisation créés par l’administrateur du serveur.  
  
 L’alternative à la définition de la propriété de **Gestionnaire** consiste à spécifier un gestionnaire et des paramètres dans la propriété [ConnectionString](../ado-api/connectionstring-property-ado.md) . autrement dit, «**handler =**_HandlerName, paramètre1, paramètre2,...;_».  
  
## <a name="applies-to"></a>S'applique à  
 [DataControl, objet (RDS)](./datacontrol-object-rds.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Handler, exemple de propriété (VB)](./handler-property-example-vb.md)   
 [Personnalisation de DataFactory](../../guide/remote-data-service/datafactory-customization.md)   
 [DataFactory, objet (RDSServer)](./datafactory-object-rdsserver.md)