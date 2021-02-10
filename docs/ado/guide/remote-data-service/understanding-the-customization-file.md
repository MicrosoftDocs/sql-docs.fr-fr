---
description: Présentation du fichier de personnalisation
title: Fonctionnement du fichier de personnalisation | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- customization file in RDS [ADO]
ms.assetid: 136f74bf-8d86-4a41-be66-c86cbcf81548
author: rothja
ms.author: jroth
ms.openlocfilehash: 1d2b674e5ecc927989873c82303f2b2410c31d31
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036059"
---
# <a name="understanding-the-customization-file"></a>Présentation du fichier de personnalisation
Chaque en-tête de section du fichier de personnalisation se compose de crochets (**[]**) contenant un type et un paramètre. Les quatre types de section sont indiqués par les chaînes littérales **Connect**, **SQL**, **UserList** ou **logs**. Le paramètre est la chaîne littérale, la valeur par défaut, un identificateur spécifié par l’utilisateur ou rien.  
  
> [!IMPORTANT]
>  À compter de Windows 8 et de Windows Server 2012, les composants serveur RDS ne sont plus inclus dans le système d’exploitation Windows (pour plus d’informations, consultez le livre de recettes sur la compatibilité avec Windows 8 et [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) ). Les composants clients RDS seront supprimés dans une prochaine version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Les applications qui utilisent RDS doivent migrer vers le [service de données WCF](/dotnet/framework/wcf/).  
  
 Par conséquent, chaque section est marquée avec l’un des en-têtes de section suivants :  
  
```console
  
[ connect default ] [ connect    
identifier   
] [ sql  default ] [ sql    
identifier   
] [ userlist    
identifier   
] [ logs ]  
  
```  
  
 Les en-têtes de section comportent les éléments suivants.  
  
|Partie|Description|  
|----------|-----------------|  
|**connect**|Chaîne littérale qui modifie une chaîne de connexion.|  
|**sql**|Chaîne littérale qui modifie une chaîne de commande.|  
|**userlist**|Chaîne littérale qui modifie les droits d’accès d’un utilisateur spécifique.|  
|**logs**|Chaîne littérale qui spécifie un enregistrement de fichier journal d’erreurs opérationnelles.|  
|**default**|Chaîne littérale qui est utilisée si aucun identificateur n’est spécifié ou trouvé.|  
|*identifier*|Chaîne qui correspond à une chaîne dans la chaîne de **connexion** ou de **commande** .<br /><br /> -Utilisez cette section si l’en-tête de section contient **Connect** et que la chaîne d’identificateur est trouvée dans la chaîne de connexion.<br />-Utilisez cette section si l’en-tête de section contient **SQL** et que la chaîne d’identificateur est trouvée dans la chaîne de commande.<br />-Utilisez cette section si l’en-tête de section contient **UserList** et que la chaîne d’identificateur correspond à un identificateur de section de **connexion** .|  
  
 Le **DataFactory** appelle le gestionnaire, en passant les paramètres du client. Le gestionnaire recherche les chaînes entières dans les paramètres client qui correspondent aux identificateurs dans les en-têtes de section appropriés. Si une correspondance est trouvée, le contenu de cette section est appliqué au paramètre client.  
  
 Une section particulière est utilisée dans les circonstances suivantes :  
  
-   Une section de **connexion** est utilisée si la partie valeur du mot clé de chaîne de connexion client, «**Data source =**_value_», correspond à un identificateur de section de **connexion** . 
  
-   Une section **SQL** est utilisée si la chaîne de commande du client contient une chaîne qui correspond à un identificateur de section **SQL** .  
  
-   Une section **Connect** ou **SQL** avec un paramètre default est utilisée en l’absence d’identificateur correspondant.  
  
-   Une section **UserList** est utilisée si l’identificateur de la section **UserList** correspond à un identificateur de section de **connexion** . En cas de correspondance, le contenu de la section **UserList** est appliqué à la connexion régie par la section **Connect** .  
  
-   Si la chaîne d’une connexion ou d’une chaîne de commande ne correspond pas à l’identificateur d’un en-tête de la section **Connect** ou **SQL** et qu’il n’existe pas d’en-tête **Connect** ou **SQL** avec un paramètre default, la chaîne du client est utilisée sans modification.  
  
-   La section **logs** est utilisée chaque fois que le **DataFactory** est en cours d’exécution.  
  
## <a name="see-also"></a>Voir aussi  
 [Section de connexion au fichier de personnalisation](./customization-file-connect-section.md)   
 [Section journaux des fichiers de personnalisation](./customization-file-logs-section.md)   
 [Section SQL du fichier de personnalisation](./customization-file-sql-section.md)   
 [Section UserList du fichier de personnalisation](./customization-file-userlist-section.md)   
 [Personnalisation de DataFactory](./datafactory-customization.md)   
 [Paramètres client requis](./required-client-settings.md)   
 [Écriture d’un gestionnaire personnalisé](./writing-your-own-customized-handler.md)