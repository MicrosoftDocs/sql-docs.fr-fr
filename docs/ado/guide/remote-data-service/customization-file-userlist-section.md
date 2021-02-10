---
description: Fichier de personnalisation, section UserList
title: Section UserList du fichier de personnalisation | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- UserList section in rds [ADO]
- customization file in RDS [ADO]
ms.assetid: 42e8ec20-eaac-4a95-8cb8-4bba93a75bcb
author: rothja
ms.author: jroth
ms.openlocfilehash: 4ca0d2e0a2bff2e5f9a3ea057220ac8cda14c9af
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036509"
---
# <a name="customization-file-userlist-section"></a>Fichier de personnalisation, section UserList
La section **UserList** concerne la section **Connect** avec le même paramètre *identificateur* de section.  
  
 Cette section peut contenir une *entrée d’accès utilisateur* qui spécifie des droits d’accès pour l’utilisateur spécifié et remplace l' *entrée d’accès* *par défaut* dans la section de **connexion** correspondante.  
  
> [!IMPORTANT]
>  À compter de Windows 8 et de Windows Server 2012, les composants serveur RDS ne sont plus inclus dans le système d’exploitation Windows (pour plus d’informations, consultez le livre de recettes sur la compatibilité avec Windows 8 et [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) ). Les composants clients RDS seront supprimés dans une prochaine version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Les applications qui utilisent RDS doivent migrer vers le [service de données WCF](/dotnet/framework/wcf/).  
  
## <a name="syntax"></a>Syntaxe  
 Une entrée d’accès utilisateur se présente sous la forme suivante :  
  
 _nom d’utilisateur_**=**   
 **_accessRights_**  
  
|Partie|Description|  
|----------|-----------------|  
|*userName*|*Nom d’utilisateur* de la personne qui utilise cette connexion. Les noms d’utilisateur valides sont établis avec la boîte de dialogue de **Service Manager** IIS.|  
|**_accessRights_**|L’un des droits d’accès suivants :<br /><br /> -   **NoAccess** -l’utilisateur ne peut pas accéder à la source de données.<br />-   **ReadOnly** : l’utilisateur peut lire la source de données.<br />-   **ReadWrite** : l’utilisateur peut lire ou écrire dans la source de données.|  
  
## <a name="see-also"></a>Voir aussi  
 [Section de connexion au fichier de personnalisation](./customization-file-connect-section.md)   
 [Section journaux des fichiers de personnalisation](./customization-file-logs-section.md)   
 [Section SQL du fichier de personnalisation](./customization-file-sql-section.md)   
 [Personnalisation de DataFactory](./datafactory-customization.md)   
 [Paramètres client requis](./required-client-settings.md)   
 [Fonctionnement du fichier de personnalisation](./understanding-the-customization-file.md)   
 [Écriture d’un gestionnaire personnalisé](./writing-your-own-customized-handler.md)