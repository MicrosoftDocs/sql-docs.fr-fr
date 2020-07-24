---
title: Sécurité de l’intégration du CLR | Microsoft Docs
description: SQL Server l’intégration avec la sécurité .NET Framework CLR gère l’accès entre les objets. Les vérifications de sécurité effectuées sur les objets dépendent des appels impliqués.
ms.custom: ''
ms.date: 07/22/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: clr
ms.topic: reference
helpviewer_keywords:
- security [CLR integration]
- authorization [CLR integration]
- common language runtime [SQL Server], security
- database objects [CLR integration], security
ms.assetid: 05d7a471-c5d5-4730-b903-e4edc8157bb4
author: rothja
ms.author: jroth
ms.openlocfilehash: dd0f9f37b3381705a2e739276a7a044837fb20ad
ms.sourcegitcommit: 768f046107642f72693514f51bf2cbd00f58f58a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87110160"
---
# <a name="clr-integration-security"></a>Sécurité de l'intégration du CLR

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Le modèle de sécurité de l'intégration [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] avec le Common Language Runtime (CLR) [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)] gère et sécurise l'accès entre types différents d'objets CLR et non-CLR qui s'exécutent dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Ces objets peuvent être appelés par une instruction [!INCLUDE[tsql](../../../includes/tsql-md.md)] ou un autre objet CLR qui s'exécute dans le serveur. Les appels entre objets portent le nom de liens. Les types de vérifications de sécurité effectués sur ces objets dépendent des types de liens impliqués.  
  
 Le modèle de sécurité d'intégration du CLR a les objectifs suivants :  
  
-   Par défaut, l'exécution de code utilisateur managé sur [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ne doit pas compromettre l'intégrité et la stabilité de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. L'exécution d'opérations susceptibles de compromettre la robustesse de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] doit être protégée par des autorisations de haut niveau appropriées.  
  
-   Le code utilisateur managé ne doit pas accéder de façon non autorisée aux données utilisateur ou autre code utilisateur dans la base de données. Le code défini par l'utilisateur doit s'exécuter sous le contexte de sécurité de la session utilisateur qui l'a appelé et avec les privilèges corrects pour ce contexte de sécurité.  
  
-   Il doit y avoir des contrôles pour restreindre le code utilisateur à accéder à toute ressource située à l'extérieur du serveur, de sorte qu'il soit utilisé strictement pour l'accès aux données et le calcul locaux.  
  
-   Le code défini par l'utilisateur ne doit pas être en mesure d'accéder de façon non autorisée aux ressources système du fait de son exécution dans le processus [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] intègre maintenant le modèle de sécurité basé sur utilisateur de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] avec le modèle de sécurité basé sur l'accès du code du CLR. Quelques-uns des avantages offerts par cette approche combinée de la sécurité sont discutés dans cette section.  
  
 Le tableau suivant décrit les rubriques de cette section.  
  
 [Sécurité d'accès du code de l'intégration du CLR](../../../relational-databases/clr-integration/security/clr-integration-code-access-security.md)  
 Discute du modèle de sécurité d'accès du code pour le code managé.  
  
 [Attributs de protection de l'hôte et programmation de l'intégration CLR](../../../relational-databases/clr-integration-security-host-protection-attributes/host-protection-attributes-and-clr-integration-programming.md)  
 Fournit des informations à propos des valeurs d'attributs de protection de l'hôte (HPA) interdites dans les assemblys SAFE et EXTERNAL_ACCESS.  
  
 [Liens dans la sécurité d'intégration du CLR](https://msdn.microsoft.com/library/168efd01-d12e-4bdf-a1b3-0b5c76474eaf)  
 Décrit comment les segments de code utilisateur peuvent s'appeler dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
 [Emprunt d'identité et sécurité de l'intégration du CLR](https://msdn.microsoft.com/library/1495a7af-2248-4cee-afdb-9269fb3a7774)  
 Discute la manière dont le code managé accède aux ressources externes à l'aide de l'emprunt d'identité.  
  
 [Autorisation d'appelants partiellement approuvés](https://msdn.microsoft.com/library/20b0248f-36da-4fc3-97d2-3789fcf6e084)  
 Discute des problèmes qui surviennent lorsqu'une méthode managée appelle une méthode dans une classe contenue dans un autre assembly.  
  
 [Domaines d'application et sécurité de l'intégration du CLR](/previous-versions/sql/2014/database-engine/dev-guide/allowing-partially-trusted-callers?view=sql-server-2014)  
 Décrit la façon dont les assemblys sont chargés dans les domaines d'application.  
  
## <a name="see-also"></a>Voir aussi  
 [Gestion des assemblys d'intégration du CLR](../../../relational-databases/clr-integration/assemblies/managing-clr-integration-assemblies.md)  
  
  
