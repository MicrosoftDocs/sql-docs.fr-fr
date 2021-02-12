---
title: Considérations sur la sécurité pour les extensions | Microsoft Docs
description: En savoir plus sur les stratégies, les conditions et les exigences de sécurité qui déterminent comment les serveurs de rapports accordent des autorisations aux extensions Reporting Services.
ms.date: 03/06/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: extensions
ms.topic: reference
helpviewer_keywords:
- security [Reporting Services], extensions
- extensions [Reporting Services], security
- permissions [Reporting Services], extensions
ms.assetid: 58cbdfeb-1105-4a7d-a3b8-b897ff95f367
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: b8994d24fe6beacfd9dc11417d529575760e7dff
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100065244"
---
# <a name="security-considerations-for-extensions"></a>Considérations sur la sécurité pour les extensions
  Toute application qui cible le CLR (Common Language Runtime) doit interagir avec le système de sécurité du CLR. Lorsqu'une application de ce type est exécutée, elle est automatiquement évaluée et reçoit un jeu d'autorisations de la part du CLR. En fonction des autorisations reçues par l'application, elle continue de s'exécuter ou génère une exception de sécurité. Les paramètres et stratégies de sécurité locale définis dans les fichiers de configuration de stratégie de sécurité pour un serveur de rapports particulier définissent les autorisations de code reçues par un assembly.  
  
 Avant de demander des autorisations, vous devez savoir quelles ressources et opérations protégées votre code d'extension projette d'utiliser, et vous devez savoir quelles autorisations protègent ces ressources et opérations. De plus, vous devez conserver une trace de toutes les ressources auxquelles les méthodes des bibliothèques de classes accèdent et qui sont appelées par les composants de l'extension. Pour plus d'informations, consultez « Demande d'autorisations » dans le Guide du développeur [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)].  
  
 Les extensions déployées sur un serveur de rapports doivent s’exécuter avec une confiance totale, ce qui signifie que votre extension doit faire partie d’un groupe de codes auquel le jeu d’autorisations **FullTrust** est accordé. Cela signifie également que votre extension peut avoir accès à certaines ressources et opérations serveur disponibles par le biais du CLR selon l'utilisateur qui est authentifié pour un rapport particulier. Pour plus d’informations sur les groupes de codes et les extensions, consultez [Sécurité d’accès du code dans Reporting Services](../../reporting-services/extensions/secure-development/code-access-security-in-reporting-services.md).  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] applique la sécurité [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] à toutes ses extensions.  
  
 Les conditions suivantes s'appliquent au déploiement des extensions pour le traitement des données, aux extensions de remise, aux extensions de rendu et aux extensions de sécurité dans [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] :  
  
-   Seul l'administrateur local a l'autorisation de déployer une extension.  
  
-   Seuls les utilisateurs disposant des autorisations de lecture/écriture appropriées peuvent modifier les fichiers de configuration du composant [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] en cours d'extension.  
  
-   Seuls les utilisateurs dotés de privilèges ont l'autorisation de modifier les fichiers de stratégie de sécurité et d'activer la sécurité d'accès du code pour une extension.  
  
 Pour plus d’informations sur la sécurité d’accès du code dans [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], consultez [Développement sécurisé &#40;Reporting Services&#41;](../../reporting-services/extensions/secure-development/secure-development-reporting-services.md).  
  
 Pour plus d'informations sur la sécurité du [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)], consultez « Sécurité .NET Framework » dans le Guide du développeur [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)].  
  
## <a name="initialization-of-extension-assemblies"></a>Initialisation des assemblys d'extension  
 Lorsque des extensions commencent à être chargées dans la mémoire par le serveur de rapports, elles utilisent les informations d'identification de compte de service car certains assemblys d'extension requièrent des autorisations spécifiques pour accéder aux ressources système, lire des fichiers de configuration et charger d'autres assemblys dépendants. Toutefois, une fois un assembly chargé et initialisé, tous les appels suivants aux assemblys d'extension utilisent les informations d'identification du compte d'utilisateur sous lequel la session a été ouverte.  
  
## <a name="see-also"></a>Voir aussi  
 [Extensions Reporting Services](../../reporting-services/extensions/reporting-services-extensions.md)   
 [Bibliothèque d'extensions Reporting Services](../../reporting-services/extensions/reporting-services-extension-library.md)  
  
  
