---
description: DataSpace, objet (RDS)
title: DataSpace, objet (RDS) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
apitype: COM
helpviewer_keywords:
- DataSpace object [RDS]
ms.assetid: 9194bffa-5bdf-4dff-af86-f7158c23bfa7
author: rothja
ms.author: jroth
ms.openlocfilehash: e30697dd8b751ec3122683a792a242e6faa0fff0
ms.sourcegitcommit: c7f40918dc3ecdb0ed2ef5c237a3996cb4cd268d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91720850"
---
# <a name="dataspace-object-rds"></a>DataSpace, objet (RDS)
> [!IMPORTANT]
>  À compter de Windows 8 et de Windows Server 2012, les composants serveur RDS ne sont plus inclus dans le système d’exploitation Windows (pour plus d’informations, consultez le livre de recettes sur la compatibilité avec Windows 8 et [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) ). Les composants clients RDS seront supprimés dans une prochaine version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Les applications qui utilisent RDS doivent migrer vers le [service de données WCF](/dotnet/framework/wcf/).  
  
 Crée des proxies côté client pour des objets métier personnalisés situés sur la couche intermédiaire.  
  
 Le service de données distant nécessite des proxies d’objets métier afin que les composants côté client puissent communiquer avec les objets métier situés sur la couche intermédiaire. Les proxies facilitent l’empaquetage, le désassemblage et le transport (marshaling) des données de l’ensemble d' [enregistrements](../ado-api/recordset-object-ado.md) de l’application au-delà des limites des processus ou des ordinateurs.  
  
 Remote Data Service utilise le **RDS. ** Méthode [CreateObject](./createobject-method-rds.md) de l’objet DataSpace pour créer des proxys d’objets métier. Le proxy d’objet métier est créé dynamiquement chaque fois qu’une instance de son équivalent d’objet métier de couche intermédiaire est créée. Remote Data Service prend en charge les protocoles suivants : HTTP, HTTPs (HTTP Secure Sockets), DCOM et in-process (les composants clients et l’objet métier résident sur le même ordinateur).  
  
> [!NOTE]
>  RDS se comporte de manière « sans état » lorsque le **RDS. ** L’objet DataSpace utilise les protocoles HTTP ou HTTPS. Autrement dit, toutes les informations internes relatives à une demande du client sont supprimées une fois que le serveur a renvoyé une réponse.  
  
> [!NOTE]
>  Bien que l’objet métier semble exister pendant la durée de vie du proxy de l’objet métier, l’objet métier existe réellement uniquement jusqu’à ce qu’une réponse soit envoyée à une demande. Lorsqu’une demande est émise (autrement dit, une méthode est appelée sur l’objet métier), le proxy ouvre une nouvelle connexion au serveur et le serveur crée une nouvelle instance de l’objet métier. Une fois que l’objet métier a répondu à la demande, le serveur détruit l’objet métier et ferme la connexion.  
  
> [!NOTE]
>  Ce comportement signifie que vous ne pouvez pas transmettre des données d’une requête à une autre à l’aide d’une variable ou d’une propriété d’objet métier. Vous devez utiliser un autre mécanisme, tel qu’un fichier ou un argument de méthode, pour conserver les données d’État.  
  
 ID de classe pour le **RDS. ** L’objet DataSpace est BD96C556-65A3-11D0-983A-00C04FC29E36.  
  
 L’objet **DataSpace** est sécurisé pour l’écriture de scripts.  
  
 Cette section contient la rubrique suivante.  
  
-   [Propriétés, méthodes et événements de l’objet DataSpace (RDS)](./dataspace-object-rds-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Voir aussi  
 [DataSpace (exemple d’objet) et CreateObject (exemple de méthode) (VBScript)](./dataspace-object-and-createobject-method-example-vbscript.md)