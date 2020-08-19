---
description: Fichier de personnalisation, section connect
title: Section de connexion du fichier de personnalisation | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- connect section in RDS [ADO]
- customization file in RDS [ADO]
ms.assetid: d50eb3cc-a822-486f-b80b-65bb50547ecd
author: rothja
ms.author: jroth
ms.openlocfilehash: 02377ff40a56c8169576a5653ac21953946aaa1d
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88452261"
---
# <a name="customization-file-connect-section"></a>Fichier de personnalisation, section connect
Le comportement par défaut du gestionnaire consiste à refuser toutes les connexions. La section **Connect** spécifie des exceptions à ce comportement. Par exemple, si toutes les sections **Connect** étaient absentes ou vides, aucune connexion n’a pu être établie par défaut.  
  
 La section **Connect** peut contenir les éléments suivants :  
  
-   Entrée d’accès par défaut qui spécifie les opérations de lecture et d’écriture par défaut autorisées sur cette connexion. S’il n’y a pas d’entrée d’accès par défaut dans la section, la section sera ignorée.  
  
-   Nouvelle chaîne de connexion qui remplace la chaîne de connexion du client.  
  
> [!IMPORTANT]
>  À compter de Windows 8 et de Windows Server 2012, les composants serveur RDS ne sont plus inclus dans le système d’exploitation Windows (pour plus d’informations, consultez le livre de recettes sur la compatibilité avec Windows 8 et [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) ). Les composants clients RDS seront supprimés dans une prochaine version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Les applications qui utilisent RDS doivent migrer vers le [service de données WCF](https://go.microsoft.com/fwlink/?LinkId=199565).  
  
## <a name="syntax"></a>Syntaxe  
 Une entrée d’accès par défaut se présente sous la forme suivante :  
  
```console
  
Access=  
accessRight  
  
```  
  
 Une entrée de chaîne de connexion de remplacement se présente sous la forme suivante :  
  
```console
  
Connect=  
connectionString  
  
```  
  
## <a name="remarks"></a>Notes  
  
|Élément|Description|  
|----------|-----------------|  
|**Connexion**|Chaîne littérale qui indique qu’il s’agit d’une entrée de chaîne de connexion.|  
|**_connectionString_**|Chaîne qui remplace l’intégralité de la chaîne de connexion du client.|  
|**y accéder**|Chaîne littérale qui indique qu’il s’agit d’une entrée d’accès.|  
|**_accessRight_**|L’un des droits d’accès suivants :<br /><br /> -   **NoAccess** -l’utilisateur ne peut pas accéder à la source de données.<br />-   **ReadOnly** : l’utilisateur peut lire la source de données.<br />-   **ReadWrite** : l’utilisateur peut lire ou écrire dans la source de données.|  
  
 Si vous souhaitez autoriser une connexion (en désactivant le comportement du gestionnaire par défaut), définissez l’entrée d’accès dans la section **connexion par défaut** sur `Access=ReadWrite` , puis supprimez ou commentez toute autre section d' _identificateur_ de **connexion** .  
  
## <a name="see-also"></a>Voir aussi  
 [Section journaux des fichiers de personnalisation](../../../ado/guide/remote-data-service/customization-file-logs-section.md)   
 [Section SQL du fichier de personnalisation](../../../ado/guide/remote-data-service/customization-file-sql-section.md)   
 [Section UserList du fichier de personnalisation](../../../ado/guide/remote-data-service/customization-file-userlist-section.md)   
 [Personnalisation de DataFactory](../../../ado/guide/remote-data-service/datafactory-customization.md)   
 [Paramètres client requis](../../../ado/guide/remote-data-service/required-client-settings.md)   
 [Fonctionnement du fichier de personnalisation](../../../ado/guide/remote-data-service/understanding-the-customization-file.md)   
 [Écriture d’un gestionnaire personnalisé](../../../ado/guide/remote-data-service/writing-your-own-customized-handler.md)



