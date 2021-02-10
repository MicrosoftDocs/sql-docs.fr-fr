---
description: Fichier de personnalisation, section Logs
title: Section journaux des fichiers de personnalisation | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- logs section in RDS [ADO]
- customization file in RDS [ADO]
ms.assetid: a368e264-865c-41ee-be00-d9097255c2ea
author: rothja
ms.author: jroth
ms.openlocfilehash: 34485c91a20b8c5df668e5eddd11e442418844a9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036529"
---
# <a name="customization-file-logs-section"></a>Fichier de personnalisation, section Logs
La section **logs** contient une entrée de fichier journal qui spécifie le nom d’un fichier qui enregistre les erreurs lors de l’opération du **DataFactory**.  
  
> [!IMPORTANT]
>  À compter de Windows 8 et de Windows Server 2012, les composants serveur RDS ne sont plus inclus dans le système d’exploitation Windows (pour plus d’informations, consultez le livre de recettes sur la compatibilité avec Windows 8 et [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) ). Les composants clients RDS seront supprimés dans une prochaine version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Les applications qui utilisent RDS doivent migrer vers le [service de données WCF](/dotnet/framework/wcf/).  
  
## <a name="syntax"></a>Syntaxe  
 Une entrée de fichier journal se présente sous la forme suivante :  
  
```console
  
err=  
FileName  
  
```  
  
## <a name="remarks"></a>Notes  
  
|Partie|Description|  
|----------|-----------------|  
|**Raise**|Chaîne littérale qui indique qu’il s’agit d’une entrée de fichier journal.|  
|*FileName*|Un chemin d’accès complet et un nom de fichier. Le nom de fichier par défaut est **c:\msdfmap.log**.|  
  
 Le fichier journal contient le nom d’utilisateur, HRESULT, la date et l’heure de chaque erreur.  
  
## <a name="see-also"></a>Voir aussi  
 [Section de connexion au fichier de personnalisation](./customization-file-connect-section.md)   
 [Section SQL du fichier de personnalisation](./customization-file-sql-section.md)   
 [Section UserList du fichier de personnalisation](./customization-file-userlist-section.md)   
 [Personnalisation de DataFactory](./datafactory-customization.md)   
 [Paramètres client requis](./required-client-settings.md)   
 [Fonctionnement du fichier de personnalisation](./understanding-the-customization-file.md)   
 [Écriture d’un gestionnaire personnalisé](./writing-your-own-customized-handler.md)