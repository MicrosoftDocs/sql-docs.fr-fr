---
description: Solutions pour le service RDA (Remote Data Access)
title: Solutions pour l’accès aux données à distance | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- RDS [ADO]
ms.assetid: d311cc67-7db7-4c43-9590-d465564695e4
author: rothja
ms.author: jroth
ms.openlocfilehash: 01516d04ce20fc1590ca41985078bcd47f47e47e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100031771"
---
# <a name="solutions-for-remote-data-access"></a>Solutions pour le service RDA (Remote Data Access)
## <a name="the-issue"></a>Le problème  
 ADO permet à votre application d’accéder directement aux sources de données et de les modifier (parfois appelées système à deux niveaux). Par exemple, si votre connexion est à la source de données qui contient vos données, il s’agit d’une connexion directe dans un système à deux niveaux.  
  
> [!IMPORTANT]
>  À compter de Windows 8 et de Windows Server 2012, les composants serveur RDS ne sont plus inclus dans le système d’exploitation Windows (pour plus d’informations, consultez le livre de recettes sur la compatibilité avec Windows 8 et [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) ). Les composants clients RDS seront supprimés dans une prochaine version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Les applications qui utilisent RDS doivent migrer vers le [service de données WCF](/dotnet/framework/wcf/).  
  
 Toutefois, vous souhaiterez peut-être accéder aux sources de données indirectement par le biais d’un intermédiaire tel que Microsoft® Internet Information Services (IIS). Cette organisation est parfois appelée système à trois niveaux. IIS est un système client/serveur qui offre un moyen efficace pour une application locale ou cliente d’appeler un programme distant, ou serveur, sur Internet ou sur un intranet. Le programme serveur accède à la source de données et traite éventuellement les données acquises.  
  
 Par exemple, votre page Web intranet contient une application écrite en Microsoft® Visual Basic Scripting Edition (VBScript), qui se connecte à IIS. IIS se connecte à son tour à la source de données réelle, récupère les données, les traite d’une certaine façon, puis renvoie les informations traitées à votre application.  
  
 Dans cet exemple, votre application ne se connecte jamais directement à la source de données ; IIS. Et IIS a accédé aux données au moyen d’ADO.  
  
> [!NOTE]
>  L’application client/serveur n’a pas besoin d’être basée sur Internet ou sur un intranet (c’est-à-dire, sur le Web). elle peut être composée uniquement de programmes compilés sur un réseau local. Toutefois, il s’agit d’une application basée sur le Web.  
  
 Comme un contrôle visuel, tel qu’une grille, une case à cocher ou une liste, peut utiliser les informations retournées, les informations retournées doivent être facilement utilisées par un contrôle visuel.  
  
 Vous souhaitez une interface de programmation d’applications simple et efficace qui prend en charge les systèmes à trois niveaux et retourne des informations aussi facilement que si elles avaient été récupérées sur un système à deux niveaux. RDS (Remote Data Service) est cette interface.  
  
## <a name="the-solution"></a>La solution  
 RDS définit un modèle de programmation (la séquence d’activités nécessaire pour accéder à une source de données et la mettre à jour) pour accéder aux données par le biais d’un intermédiaire, tel que Internet Information Services (IIS). Le modèle de programmation résume l’ensemble des fonctionnalités de RDS.  
  
## <a name="see-also"></a>Voir aussi  
 [Modèle de programmation RDS de base](./basic-rds-programming-model.md)   
 [Scénario RDS](./rds-scenario.md)   
 [Didacticiel RDS](./rds-tutorial.md)   
 [Utilisation et sécurité de RDS](./rds-usage-and-security.md)