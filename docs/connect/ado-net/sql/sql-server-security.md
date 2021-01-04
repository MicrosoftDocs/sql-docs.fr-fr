---
title: Sécurité de SQL Server
description: Fournit une vue d’ensemble de la sécurité de SQL Server et des scénarios d’application pour créer des applications ADO.NET sécurisées qui ciblent SQL Server.
ms.date: 09/26/2019
ms.assetid: 9053724d-a1fb-4f0f-b9dc-7f6dd893e8ff
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-kaywon
ms.openlocfilehash: e70f6da7755b28c400cc80a5ee6de79a004e4e42
ms.sourcegitcommit: 2add15a99df7b85d271adb261523689984dfd134
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/10/2020
ms.locfileid: "97038917"
---
# <a name="sql-server-security"></a>Sécurité de SQL Server

[!INCLUDE[Driver_ADONET_Download](../../../includes/driver_adonet_download.md)]

SQL Server dispose de nombreuses fonctionnalités qui prennent en charge la création d’applications de base de données sécurisées.  
  
Les principaux éléments à prendre en compte au niveau de la sécurité, tels que le vol ou l’altération de données, concernent toutes les versions de SQL Server. L’intégrité des données doit également être considérée comme un problème de sécurité. Si les données ne sont pas protégées, il est possible qu’elles deviennent inutilisables si la manipulation de données ad hoc est autorisée et si les données sont modifiées par inadvertance ou par malveillance avec des valeurs incorrectes ou supprimées entièrement. En outre, il existe souvent des exigences légales qui doivent être respectées, telles que le stockage correct des informations confidentielles. Le stockage de certains types de données personnelles est entièrement inscrit, en fonction des lois qui s’appliquent dans une juridiction donnée.  
  
Chaque version de SQL Server présente des fonctionnalités de sécurité différentes, comme chaque version de Windows, les versions les plus récentes apportant des améliorations par rapport aux plus anciennes. Il est important de comprendre que les fonctionnalités de sécurité seules ne peuvent pas garantir une application de base de données sécurisée. Chaque application de base de données est unique dans ses exigences, son environnement d’exécution, son modèle de déploiement, son emplacement physique et l’analyse des utilisateurs. Certaines applications locales dans l’étendue peuvent uniquement nécessiter une sécurité minimale, tandis que d’autres applications locales ou des applications déployées sur Internet peuvent nécessiter des mesures de sécurité strictes, ainsi qu’une surveillance et une évaluation continues.  
  
Les exigences de sécurité d’une application de base de données SQL Server doivent être examinées au moment de la conception, et non pas à une étape ultérieure. L’évaluation des menaces au début du cycle de développement vous donne la possibilité d’atténuer les dommages potentiels lorsqu’une vulnérabilité est détectée.  
  
Même si la conception initiale d’une application est saine, de nouvelles menaces peuvent émerger au fur et à mesure que le système évolue. En créant plusieurs lignes de défense autour de votre base de données, vous pouvez réduire les dommages infligés par une violation de la sécurité. Votre première ligne de défense consiste à réduire la surface d’attaque en n’accordant jamais plus d’autorisations que ce qui est absolument nécessaire.  
  
Les rubriques de cette section décrivent brièvement les fonctionnalités de sécurité de SQL Server qui concernent les développeurs, et fournissent des liens vers des rubriques pertinentes dans la Documentation en ligne de SQL Server et d’autres ressources qui fournissent des explications plus détaillées.  
  
## <a name="in-this-section"></a>Contenu de cette section  
[Authentification dans SQL Server](authentication-sql-server.md)  
Décrit les connexions et l’authentification dans SQL Server et fournit des liens vers des ressources supplémentaires. 

[Authentification Azure Active Directory](azure-active-directory-authentication.md)  
Explique comment utiliser les modes d’authentification Azure Active Directory pris en charge pour se connecter aux sources de données Azure SQL avec SqlClient.
  
[Scénarios de sécurité des applications dans SQL Server](application-security-scenarios-sql-server.md)  
Contient des rubriques présentant différents scénarios de sécurité pour les applications ADO.NET et SQL Server.  
  
[Sécurité de SQL Server Express](sql-server-express-security.md)  
Décrit les considérations en matière de sécurité pour SQL Server Express.  
  
## <a name="related-sections"></a>Sections connexes  
[Centre de sécurité pour le moteur de base de données SQL Server et Azure SQL Database](../../../relational-databases/security/security-center-for-sql-server-database-engine-and-azure-sql-database.md)  
Décrit les considérations en matière de sécurité pour SQL Server et Azure SQL Database.

[Considérations sur la sécurité pour une installation SQL Server](../../../sql-server/install/security-considerations-for-a-sql-server-installation.md)  
Décrit les problèmes de sécurité à prendre en compte avant d’installer SQL Server.

## <a name="next-steps"></a>Étapes suivantes
- [SQL Server et ADO.NET](index.md)
