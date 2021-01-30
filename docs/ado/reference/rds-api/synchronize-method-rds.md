---
description: Synchronize, méthode (RDS)
title: Synchronize, méthode (RDS) | Microsoft Docs
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.prod: sql
ms.prod_service: connectivity
ms.topic: reference
apitype: COM
helpviewer_keywords:
- Synchronize method [ADO]
ms.assetid: 7af42866-7db2-4174-8251-388a2cf741f2
author: rothja
ms.author: jroth
ms.openlocfilehash: 455628537f876b1500f0d53f29d4869da9b48d1d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99168741"
---
# <a name="synchronize-method-rds"></a>Synchronize, méthode (RDS)
Synchronisez le Recordset donné avec la base de données spécifiée par la chaîne de connexion pour l’utiliser dans ADO 2,5 et versions ultérieures.  
  
> [!IMPORTANT]
>  À compter de Windows 8 et de Windows Server 2012, les composants serveur RDS ne sont plus inclus dans le système d’exploitation Windows (pour plus d’informations, consultez le livre de recettes sur la compatibilité avec Windows 8 et [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) ). Les composants clients RDS seront supprimés dans une prochaine version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Les applications qui utilisent RDS doivent migrer vers le [service de données WCF](/dotnet/framework/wcf/).  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
object.Synchronize(ConnectionString As String, HandlerString As String, lSynchronizeOptions As Long, ppRecordset As Object, pStatusArray, [lcid As Long], [pInformation)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *ConnectionString*  
 Chaîne utilisée pour se connecter au fournisseur de OLE DB dans lequel la demande sera envoyée. Si un gestionnaire est utilisé, le gestionnaire peut modifier ou remplacer la chaîne de connexion.  
  
 *HandlerString*  
 La chaîne identifie le gestionnaire à utiliser avec cette exécution. La chaîne contient deux parties. La première partie contient le nom (ProgID) du gestionnaire à utiliser. La deuxième partie de la chaîne contient des arguments à passer au gestionnaire. La façon dont la chaîne d’arguments est interprétée est spécifique au gestionnaire. Les deux parties sont séparées par la première instance d’une virgule dans la chaîne (même si la chaîne d’arguments peut contenir des virgules supplémentaires). Les arguments sont facultatifs.  
  
 *lSynchronizeOptions*  
 Masque de bits des options de synchronisation.  
  
 1 = les mises à jour *UpdateTransact* de la base de données sont encapsulées dans une transaction. La transaction est abandonnée en cas d’échec de l’une des mises à jour.  
  
 2 =*RefreshWithUpdate* provoque le renvoi des États de ligne lorsque ni *Refresh* , ni *RefreshConflicts* ne sont définis.  
  
 4 =*Actualiser* l’ensemble d’enregistrements est actualisé avec les données actuelles de la base de données. Les mises à jour en attente ne sont pas transmises à la base de données. Si ce bit n’est pas défini, le jeu d’enregistrements n’est pas actualisé et toutes les mises à jour en attente sont envoyées à la base de données.  
  
 8 =*RefreshConflicts* toutes les lignes avec des modifications en attente ne peuvent pas être mises à jour. Les lignes qui n’ont pas pu être mises à jour sont actualisées avec les données actuelles de la base de données.  
  
 *ppRecordset*  
 Pointeur vers le recordset à synchroniser.  
  
 *pStatusArray*  
 Variant utilisé pour retourner un tableau sécurisé des États de ligne pour les lignes affectées par Synchronize. Non défini si aucune des options de synchronisation suivantes n’est définie : *RefreshWithUpdate*, *Refresh* et *RefreshConflicts*.  
  
 *lcid*  
 LCID utilisé pour générer les erreurs retournées dans *pInformation*.  
  
 *pInformation*  
 Pointeur vers une erreur d’informations retournée par l' **instruction EXECUTE**. Si la valeur est NULL, aucune information d’erreur n’est retournée.  
  
## <a name="remarks"></a>Notes  
 Le paramètre *HandlerString* peut avoir la valeur null. Ce qui se passe dans ce cas dépend de la configuration du serveur RDS. La chaîne de gestionnaire « MSDFMAP. Handler » indique que le gestionnaire fourni par Microsoft (Msdfmap.dll) doit être utilisé. Une chaîne de gestionnaire « MASDFMAP. Handler, sample.ini » indique que le gestionnaire de Msdfmap.dll doit être utilisé et que l’argument « sample.ini » doit être passé au gestionnaire. Msdfmap.dll interprète alors l’argument comme une direction pour utiliser la sample.ini pour vérifier la connexion et les chaînes de requête.  
  
## <a name="applies-to"></a>S'applique à  
 [DataFactory, objet (RDSServer)](./datafactory-object-rdsserver.md)