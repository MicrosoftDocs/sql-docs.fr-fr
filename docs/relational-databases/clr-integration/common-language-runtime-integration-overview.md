---
title: Vue d’ensemble du CLR (Common Language Runtime)
description: L’intégration du CLR avec SQL Server vous permet d’implémenter certaines fonctionnalités à l’aide de n’importe quel langage de .NET Framework comme SQL Server modules côté serveur.
ms.custom: seo-lt-2019
ms.date: 06/20/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: clr
ms.topic: reference
helpviewer_keywords:
- managed code [SQL Server]
- common language runtime [SQL Server], about CLR integration
- cross-language integration
- integrating CLR [SQL Server]
- .NET Framework [SQL Server], common language runtime
- code access security [CLR integration]
- managed code [SQL Server], CLR integration
ms.assetid: 7be9e644-36a2-48fc-9206-faf59fdff4d7
author: rothja
ms.author: jroth
ms.openlocfilehash: a9ffcef000e043195d46bf98a11253205b5bc15b
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/01/2021
ms.locfileid: "100348029"
---
# <a name="common-language-runtime-integration"></a>Intégration du Common Language Runtime
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]et [Azure SQL Managed instance](/azure/sql-database/sql-database-managed-instance-index) vous permettent d’implémenter certaines des fonctionnalités à l’aide des langages .net à l’aide de l’intégration du Common Language Runtime natif (CLR) comme SQL Server modules côté serveur (procédures, fonctions et déclencheurs). Le CLR fournit le code managé avec des services tels que l'intégration interlangage, la sécurité d'accès du code, la gestion de la durée de vie des objets et la prise en charge du débogage et des profils. Pour les utilisateurs et les développeurs d'applications [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], l'intégration du CLR permet désormais d'écrire des procédures stockées, des déclencheurs, des types définis par l'utilisateur, des fonctions définies par l'utilisateur (fonctions scalaires et fonctions table) et des fonctions d'agrégation définies par l'utilisateur, à l'aide de langages du .NET Framework, tels que [!INCLUDE[msCoName](../../includes/msconame-md.md)] Visual Basic .NET et [!INCLUDE[msCoName](../../includes/msconame-md.md)] Visual C#. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] inclut la version 4 préinstallée du .NET Framework.  

> [!WARNING]
>  CLR utilise la sécurité d’accès du code (CAS) dans le .NET Framework, qui n’est plus pris en charge comme limite de sécurité. Un assembly CLR créé avec `PERMISSION_SET = SAFE` peut être en mesure d’accéder à des ressources système externes, d’appeler du code non managé et d’acquérir des privilèges sysadmin. À compter de [!INCLUDE[sssql17](../../includes/sssql17-md.md)], une option de `sp_configure` appelée `clr strict security` est introduite pour renforcer la sécurité des assemblys CLR. `clr strict security` est activée par défaut et traite les assemblys `SAFE` et `EXTERNAL_ACCESS` comme s’ils étaient marqués `UNSAFE`. L’option `clr strict security` peut être désactivée pour assurer une compatibilité descendante, mais ceci n’est pas recommandé. Microsoft recommande que tous les assemblys soient signés par un certificat ou une clé asymétrique avec une connexion correspondante à laquelle a été accordée l’autorisation `UNSAFE ASSEMBLY` dans la base de données master. Pour plus d’informations, consultez [CLR strict security](../../database-engine/configure-windows/clr-strict-security.md). Les administrateurs [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peuvent également ajouter des assemblys à une liste d’assemblys, que le moteur de base de données doit approuver. Pour plus d’informations, consultez [sys.sp_add_trusted_assembly](../../relational-databases/system-stored-procedures/sys-sp-add-trusted-assembly-transact-sql.md).

Vous pouvez également regarder cette vidéo de 6 minutes qui vous montre comment utiliser CLR dans Azure SQL Managed Instance :

> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/Its-just-SQL-CLR-in-Azure-SQL-Database-Managed-Instance/player?WT.mc_id=dataexposed-c9-niner]



## <a name="when-to-use-clr-modules"></a>Quand utiliser les modules CLR ?

L’intégration du CLR vous permet d’implémenter des fonctionnalités complexes disponibles dans .NET Framework telles que les expressions régulières, le code pour l’accès aux ressources externes (serveurs, services Web, bases de données), le chiffrement personnalisé, etc. Voici quelques-uns des avantages de l’intégration du CLR côté serveur :
  
-   **Un meilleur modèle de programmation.** Les langages .NET Framework sont à bien des égards plus riches que Transact-SQL, en proposant des constructions et des fonctions qui n'étaient pas jusque-là accessibles aux développeurs [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Les développeurs peuvent aussi tirer parti de la puissance de la bibliothèque du .NET Framework, qui propose un ensemble complet de classes pour résoudre rapidement et efficacement les problèmes de programmation.  
  
-   **Sécurité améliorée.** Le code managé s'exécute dans un environnement CLR, hébergé par le moteur de base de données. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] met celui-ci à profit pour fournir une alternative plus sûre et plus sécurisée aux procédures stockées étendues disponibles dans les versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   **Possibilité de définir des types de données et des fonctions d'agrégation.** Les types définis par l’utilisateur et les agrégats définis par l’utilisateur sont deux nouveaux objets de base de données managés qui étendent les fonctionnalités de stockage et d’interrogation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
-   **Développement simplifié via un environnement standardisé.** Le développement de bases de données est intégré aux prochaines versions de l'environnement de développement  [!INCLUDE[msCoName](../../includes/msconame-md.md)] Visual Studio .NET. Les développeurs se servent des mêmes outils pour développer et déboguer les objets de base de données et les scripts que ceux qu'ils utilisent pour écrire des composants et services .NET Framework de couche intermédiaire ou client.  
  
-   **Potentiel pour des performances et une extensibilité améliorées.** Dans de nombreux cas, la compilation du langage .NET Framework et les modèles d'exécution offrent des performances améliorées par rapport à Transact-SQL.  
  
 Le tableau ci-dessous répertorie les rubriques de cette section.  
  
 [Vue d'ensemble de l'intégration du CLR](../../relational-databases/clr-integration/clr-integration-overview.md)  
 Décrit les types d'objets qui peuvent être créés à l'aide de l'intégration du CLR et passe en revue les spécifications requises pour générer des objets de base de données à l'aide de l'intégration du CLR.  
  
 [Nouveautés dans l'intégration du CLR](../../relational-databases/clr-integration/clr-integration-what-s-new.md)  
 Décrit les nouvelles fonctionnalités de cette version.  
  
 [Architecture d'intégration du CLR](./clr-integration-architecture-clr-hosted-environment.md)  
 Décrit les objectifs de conception de l'intégration du CLR.  
  
 [Activation de l’intégration du CLR](../../relational-databases/clr-integration/clr-integration-enabling.md)  
 Décrit comment activer l'intégration du CLR.  
  
## <a name="see-also"></a>Voir aussi  
 [Installation du .NET Framework](https://technet.microsoft.com/library/ms166014\(v=SQL.105\).aspx)  ( [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] uniquement)   
 [Performances de l'intégration du CLR](../../relational-databases/clr-integration/clr-integration-architecture-performance.md)  
  
