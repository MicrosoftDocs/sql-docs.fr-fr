---
description: Connect, propriété (RDS)
title: Connect, propriété (RDS) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
helpviewer_keywords:
- Connect property [ADO]
ms.assetid: dbad5e77-b213-4eb8-aecf-d60f203fdb59
author: rothja
ms.author: jroth
ms.openlocfilehash: 288efb8a2af9896e731bd0248967d1aaadcb2992
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100049499"
---
# <a name="connect-property-rds"></a>Connect, propriété (RDS)
Indique le nom de la base de données à partir de laquelle les opérations de requête et de mise à jour sont exécutées.  
  
 Vous pouvez définir la propriété de **connexion** au moment du design dans le [RDS.](./datacontrol-object-rds.md) Les balises d’objet de l’objet DataControl, ou au moment de l’exécution dans le code de script (par exemple, VBScript).  
  
> [!IMPORTANT]
>  À compter de Windows 8 et de Windows Server 2012, les composants serveur RDS ne sont plus inclus dans le système d’exploitation Windows (pour plus d’informations, consultez le livre de recettes sur la compatibilité avec Windows 8 et [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) ). Les composants clients RDS seront supprimés dans une prochaine version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Les applications qui utilisent RDS doivent migrer vers le [service de données WCF](/dotnet/framework/wcf/).  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
Design time: <PARAM NAME="Connect" VALUE="ConnectionString">  
Run time: DataControl.Connect = "ConnectionString"  
```  
  
#### <a name="parameters"></a>Paramètres  
 *ConnectionString*  
 Chaîne de connexion valide. Pour plus d’informations générales sur les chaînes de connexion, consultez la propriété [ConnectionString](../ado-api/connectionstring-property-ado.md) ou la documentation de votre fournisseur.  
  
> [!NOTE]
>  En spécifiant MS Remote comme fournisseur du **RDS. DataControl** crée un scénario à quatre niveaux. Les scénarios de plus de trois niveaux n’ont pas été testés et ne devraient pas être nécessaires.  
  
 *DataControl*  
 Variable objet qui représente un objet **RDS. DataControl** .  
  
## <a name="applies-to"></a>S'applique à  
 [DataControl, objet (RDS)](./datacontrol-object-rds.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Connect, exemple de propriété (VBScript)](./connect-property-example-vbscript.md)   
 [Query, méthode (RDS)](./query-method-rds.md)   
 [Refresh, méthode (RDS)](./refresh-method-rds.md)   
 [SubmitChanges, méthode (RDS)](./submitchanges-method-rds.md)