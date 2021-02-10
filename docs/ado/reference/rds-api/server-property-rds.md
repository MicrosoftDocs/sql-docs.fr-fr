---
description: Server, propriété (RDS)
title: Server, propriété (RDS) | Microsoft Docs
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.prod: sql
ms.prod_service: connectivity
ms.topic: reference
apitype: COM
f1_keywords:
- RDS::IBindMgr21::Server
helpviewer_keywords:
- Server property [RDS]
ms.assetid: d2727ce7-da9f-4271-ae3c-9334ef477c14
author: rothja
ms.author: jroth
ms.openlocfilehash: cc6262e9748a4fd94fd7f4e0a75214c24def0acb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052930"
---
# <a name="server-property-rds"></a>Server, propriété (RDS)
Indique le nom du Internet Information Services (IIS) et le protocole de communication.  
  
 Vous pouvez définir la propriété de **serveur** au moment du design dans les balises d’objet du [RDS. DataControl](./datacontrol-object-rds.md) , ou au moment de l’exécution dans le code de script.  
  
> [!IMPORTANT]
>  À compter de Windows 8 et de Windows Server 2012, les composants serveur RDS ne sont plus inclus dans le système d’exploitation Windows (pour plus d’informations, consultez le livre de recettes sur la compatibilité avec Windows 8 et [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) ). Les composants clients RDS seront supprimés dans une prochaine version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Les applications qui utilisent RDS doivent migrer vers le [service de données WCF](/dotnet/framework/wcf/).  
  
## <a name="syntax"></a>Syntaxe  
 **HTTP**  
  
 Syntaxe au moment du design  
  
```  
  
<PARAM NAME="Server" VALUE="http:  
//awebsrvr:port  
">  
  
```  
  
 Syntaxe Runtime  
  
```  
  
DataControl  
.Server="https://  
awebsrvr:port  
"  
  
```  
  
 **HTTPS**  
  
 Syntaxe au moment du design  
  
```  
  
<PARAM NAME="Server" VALUE="https://awebsrvr:port">  
```  
  
 Syntaxe Runtime  
  
```  
  
DataControl.Server="https://awebsrvr:port"  
```  
  
 **DCOM**  
  
 Syntaxe au moment du design  
  
```  
  
<PARAM NAME="Server" VALUE="  
computername  
">  
  
```  
  
 Syntaxe Runtime  
  
```  
  
DataControl.Server="computername"  
```  
  
 **In-process**  
  
 Syntaxe au moment du design  
  
```  
  
<PARAM NAME="Server" VALUE="">  
  
```  
  
 Syntaxe Runtime  
  
```  
  
DataControl.Server=""  
```  
  
## <a name="parameters"></a>Paramètres  
 *awebsrvr* ou *ComputerName*  
 Valeur de **chaîne** qui contient un chemin d’accès Internet ou intranet, ou un nom d’ordinateur, si le serveur se trouve sur un ordinateur distant ; ou une chaîne vide si le serveur se trouve sur l’ordinateur local.  
  
 *port*  
 facultatif. Port utilisé pour se connecter à un serveur exécutant IIS. Le numéro de port est défini dans Internet Explorer (dans le menu **affichage** , cliquez sur **options**, puis sélectionnez l’onglet **connexion** ) ou IIS.  
  
 *DataControl*  
 Variable objet qui représente un objet **RDS. DataControl** .  
  
## <a name="remarks"></a>Notes  
 Le serveur est l’emplacement où **RDS.** Demande de DataControl (autrement dit, une requête ou une mise à jour) est traitée. Par défaut, toutes les demandes sont traitées par l’objet [RDSServer. DataFactory](./datafactory-object-rdsserver.md) , [msdfmap. ](../../guide/remote-data-service/datafactory-customization.md) Et [MSDFMAP.INI](../../guide/remote-data-service/understanding-the-customization-file.md) fichier sur le serveur spécifié. N’oubliez pas que lorsque vous modifiez des serveurs pour rapprocher des paramètres dans les anciens et nouveaux fichiers de **MSDFMAP.INI** . Les incompatibilités peuvent entraîner l’échec des requêtes qui aboutissent sur un serveur sur un autre. Si la propriété de serveur est définie sur la chaîne vide «», ces objets seront utilisés sur l’ordinateur local.  
  
## <a name="applies-to"></a>S'applique à  
 [DataControl, objet (RDS)](./datacontrol-object-rds.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Server, exemple de propriété (VBScript)](./server-property-example-vbscript.md)   
 [Connect, propriété (RDS)](./connect-property-rds.md)   
 [Propriété SQL](./sql-property.md)   
 [SubmitChanges, méthode (RDS)](./submitchanges-method-rds.md)