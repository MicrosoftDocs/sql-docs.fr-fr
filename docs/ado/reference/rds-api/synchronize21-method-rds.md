---
description: Synchronize21, méthode (RDS)
title: Méthode Synchronize21 (RDS) | Microsoft Docs
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.prod: sql
ms.prod_service: connectivity
ms.topic: reference
apitype: COM
helpviewer_keywords:
- Synchronize21 method [ADO]
ms.assetid: 6b35f136-9d9a-4bdd-8144-67decfd3c4e9
author: rothja
ms.author: jroth
ms.openlocfilehash: 23ea3c2d1399f6d9e592911d8b8ae3fdbee6e523
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052840"
---
# <a name="synchronize21-method-rds"></a>Synchronize21, méthode (RDS)
Synchronisez le Recordset donné avec la base de données spécifiée par la chaîne de connexion pour l’utiliser avec ADO 2,1.  
  
> [!IMPORTANT]
>  À compter de Windows 8 et de Windows Server 2012, les composants serveur RDS ne sont plus inclus dans le système d’exploitation Windows (pour plus d’informations, consultez le livre de recettes sur la compatibilité avec Windows 8 et [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) ). Les composants clients RDS seront supprimés dans une prochaine version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Les applications qui utilisent RDS doivent migrer vers le [service de données WCF](/dotnet/framework/wcf/).  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
object.Synchronize21(ConnectionString As String, HandlerString As String, lSynchronizeOptions As Long, ppRecordset As Object, pStatusArray)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *ConnectionString*  
 Chaîne utilisée pour se connecter au fournisseur de OLE DB dans lequel la demande sera envoyée. Si un gestionnaire est utilisé, le gestionnaire peut modifier ou remplacer la chaîne de connexion.  
  
 *HandlerString*  
 La chaîne identifie le gestionnaire à utiliser avec cette exécution. La chaîne contient deux parties. La première partie contient le nom (ProgID) du gestionnaire à utiliser. La deuxième partie de la chaîne contient des arguments à passer au gestionnaire. La façon dont la chaîne d’arguments est interprétée est spécifique au gestionnaire. Les deux parties sont séparées par la première instance d’une virgule dans la chaîne. La chaîne d’arguments peut contenir des virgules supplémentaires. Les arguments sont facultatifs.  
  
 *lSynchronizeOptions*  
 Masque de bits des options de synchronisation.  
  
 1 = les mises à jour *UpdateTransact* de la base de données sont encapsulées dans une transaction. La transaction est abandonnée en cas d’échec de l’une des mises à jour.  
  
 2 =*RefreshWithUpdate* provoque le renvoi des États de ligne lorsque ni *Refresh* , ni *RefreshConflicts* ne sont définis.  
  
 4 =*Actualiser* l’ensemble d’enregistrements est actualisé avec les données actuelles de la base de données. Les mises à jour en attente ne sont pas transmises à la base de données. Si ce bit n’est pas défini, le jeu d’enregistrements n’est pas actualisé et toutes les mises à jour en attente sont envoyées à la base de données.  
  
 8 =*RefreshConflicts* toutes les lignes avec des modifications en attente ne peuvent pas être mises à jour. Les lignes qui n’ont pas pu être mises à jour sont actualisées avec les données actuelles de la base de données.  
  
 *ppRecordset*  
 Pointeur vers un pointeur vers le recordset à synchroniser.  
  
 *pStatusArray*  
 Variant utilisé pour retourner un tableau sécurisé des États de ligne pour les lignes affectées par Synchronize. Non défini si aucune des options de synchronisation suivantes n’est définie : *RefreshWithUpdate*, *Refresh* et *RefreshConflicts*.  
  
## <a name="remarks"></a>Notes  
 Le paramètre *HandlerString* peut avoir la valeur null. Ce qui se passe dans ce cas dépend de la configuration du serveur RDS. La chaîne de gestionnaire « MSDFMAP. Handler » indique que le gestionnaire fourni par Microsoft (Msdfmap.dll) doit être utilisé. Une chaîne de gestionnaire « MASDFMAP. Handler, sample.ini » indique que le gestionnaire de Msdfmap.dll doit être utilisé et que l’argument « sample.ini » doit être passé au gestionnaire. Msdfmap.dll interprète alors l’argument comme une direction pour utiliser la sample.ini pour vérifier la connexion et les chaînes de requête.  
  
> [!NOTE]
>  La méthode **Synchronize21** est simplement une version de la [méthode Synchronize (RDS)](./synchronize-method-rds.md). Lorsque vous devez utiliser la méthode **Synchronize** pour communiquer avec ADO 2,1, la méthode **Synchronize21** peut être appelée à la place. Les fonctionnalités de la méthode **Synchronize** dans ADO 2,5 et versions ultérieures sont un sur-ensemble des fonctionnalités fournies pour la même méthode dans ADO 2,1.  
  
## <a name="applies-to"></a>S'applique à  
 [DataFactory, objet (RDSServer)](./datafactory-object-rdsserver.md)