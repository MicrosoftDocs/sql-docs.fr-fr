---
title: Sécurisation de SQL Server | Microsoft Docs
description: Ces articles décrivent la création et l’implémentation d’un plan de sécurité efficace dans SQL Server. Apprenez-en davantage sur la plateforme, l’authentification, les objets et les applications.
ms.custom: ''
ms.date: 06/21/2019
ms.prod: sql
ms.prod_service: security
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
f1_keywords:
- Security [SQL Server]
helpviewer_keywords:
- database objects [SQL Server], security
- SQL Server, security
- operating systems [SQL Server], security
- security [SQL Server], planning
- applications [SQL Server], security
ms.assetid: 4d93489e-e9bb-45b3-8354-21f58209965d
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 2f3082f83296efe396fa2fca447782e13588df38
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340126"
---
# <a name="securing-sql-server"></a>Sécurisation de SQL Server

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

La sécurisation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut être vue comme une série d’étapes impliquant quatre domaines : la plateforme, l’authentification, les objets (notamment les données) et les applications qui accèdent au système. Les rubriques suivantes vous guideront tout au long des processus de création et de mise en place d'un plan de sécurité efficace.  
  
 Vous trouverez de plus amples informations sur la sécurité de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur le site web de [SQL Server](https://go.microsoft.com/fwlink/?LinkID=31629) . Cela inclut un guide de recommandations et une liste de contrôle de sécurité. Ce site contient également les informations et les téléchargements les plus récents sur les Service Packs.  
  
## <a name="platform-and-network-security"></a>Sécurité de la plateforme et du réseau  
 La plateforme de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] comprend le matériel physique et les systèmes de mise en réseau qui permettent de connecter des clients aux serveurs de base de données, ainsi que les fichiers binaires utilisés pour traiter les demandes de base de données.  
  
### <a name="physical-security"></a>Sécurité physique  
 Les méthodes conseillées en matière de sécurité physique limitent strictement l'accès aux composants matériels et serveur physiques. Par exemple, utilisez des salles verrouillées à accès restreint pour le matériel serveur de base de données et les périphériques réseau. Qui plus est, limitez l'accès au support de sauvegarde en stockant ce dernier à un emplacement hors site sécurisé.  
  
 La mise en œuvre de la sécurité physique du réseau commence par le souci de tenir le réseau hors de portée de tout utilisateur non autorisé. Le tableau ci-dessous contient des informations supplémentaires sur la sécurité réseau.  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|[!INCLUDE[ssEW](../../includes/ssew-md.md)] et accès réseau à d’autres éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|« Configuration et sécurisation d'un environnement serveur » dans la documentation en ligne de [!INCLUDE[ssEW](../../includes/ssew-md.md)]|  
| &nbsp; | &nbsp; |
  
### <a name="operating-system-security"></a>Sécurité du système d'exploitation  
 Les Service Packs et les mises à niveau des systèmes d'exploitation apportent des améliorations importantes en termes de sécurité. Appliquez l'ensemble des mises à jour et des mises à niveau au système d'exploitation après les avoir testés avec les applications de base de données.  
  
 Les pare-feu offrent également des méthodes de mise en œuvre efficaces de la sécurité. D'un point de vue logique, un pare-feu est un outil de séparation ou de restriction du trafic réseau que vous pouvez configurer pour appliquer la stratégie de sécurité des données de votre organisation. Si vous utilisez un pare-feu, vous augmentez la sécurité au niveau du système d'exploitation en définissant un point d'engorgement où vos mesures de sécurité peuvent se concentrer. Le tableau ci-dessous contient davantage d'informations sur la façon d'utiliser un pare-feu avec [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Configuration d’un pare-feu fonctionnant avec [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|[Configurer un pare-feu Windows pour accéder au moteur de base de données](../../database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access.md)|  
|Configuration d’un pare-feu fonctionnant avec [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]|[Service Integration Services &#40;Service SSIS&#41;](../../integration-services/service/integration-services-service-ssis-service.md)|  
|Configuration d’un pare-feu fonctionnant avec [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]|[Configurer le Pare-feu Windows pour autoriser l’accès à Analysis Services](/analysis-services/instances/configure-the-windows-firewall-to-allow-analysis-services-access)|  
|Ouverture de ports spécifiques sur un pare-feu pour permettre l’accès à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|[Configurer le Pare-feu Windows pour autoriser l’accès à SQL Server](../../sql-server/install/configure-the-windows-firewall-to-allow-sql-server-access.md)|  
|Configuration de la prise en charge de la Protection étendue de l'authentification à l'aide de la liaison de canal et liaison de service|[Se connecter au moteur de base de données à l'aide de la protection étendue](../../database-engine/configure-windows/connect-to-the-database-engine-using-extended-protection.md)|  
| &nbsp; | &nbsp; |
  
 La réduction de la surface d'exposition est une mesure de sécurité qui implique l'arrêt ou la désactivation de composants inutilisés. La réduction de la surface d'exposition permet de renforcer la sécurité en réduisant les risques d'attaque à l'encontre d'un système. L'élément principal à prendre en compte pour limiter la surface d'exposition de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] implique l'exécution des services requis dotés des privilèges minimaux en attribuant uniquement les droits appropriés aux services et aux utilisateurs. Le tableau ci-dessous contient des informations supplémentaires sur l'accès aux services et au système.  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Services requis pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|[Configurer les comptes de service Windows et les autorisations](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md)|  
| &nbsp; | &nbsp; |
  
 Si votre système [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilise Internet Information Services (IIS), d’autres étapes sont nécessaires pour mieux sécuriser la surface de la plateforme. Le tableau ci-dessous contient des informations sur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et les services Internet (IIS).  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Sécurité IIS avec [!INCLUDE[ssEW](../../includes/ssew-md.md)]|« Sécurité IIS » dans la documentation en ligne de [!INCLUDE[ssEW](../../includes/ssew-md.md)]|  
|[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] Authentification|[Authentification dans Reporting Services](../../reporting-services/extensions/security-extension/authentication-in-reporting-services.md)|  
|[!INCLUDE[ssEW](../../includes/ssew-md.md)] et accès à IIS|« Organigramme de la sécurité Internet Information Services » dans la documentation en ligne de [!INCLUDE[ssEW](../../includes/ssew-md.md)]|  
| &nbsp; | &nbsp; |
  
### <a name="sql-server-operating-system-files-security"></a>Sécurité des fichiers du système d'exploitation SQL Server  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilise les fichiers du système d’exploitation pour son fonctionnement et le stockage des données. Les méthodes conseillées en matière de sécurité des fichiers exigent que vous restreigniez l'accès à ces fichiers. Le tableau ci-dessous contient des informations sur ces fichiers.  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Fichiers programme|[Emplacements des fichiers pour les instances par défaut et les instances nommées de SQL Server](../../sql-server/install/file-locations-for-default-and-named-instances-of-sql-server.md)|  
| &nbsp; | &nbsp; |
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Les Service Packs et les mises à niveau offrent une sécurité accrue. Pour identifier les derniers Service Packs disponibles pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez le site web de [SQL Server](https://go.microsoft.com/fwlink/?LinkID=31629) .  
  
 Vous pouvez utiliser le script ci-dessous pour déterminer quel Service Pack est installé sur le système.  
  
```sql
SELECT CONVERT(char(20), SERVERPROPERTY('productlevel'));  
```  
  
## <a name="principals-and-database-object-security"></a>Sécurité des principaux et des objets de base de données  
 Les principaux désignent les individus, les groupes et les processus auxquels l'accès à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]a été accordé. Les « éléments sécurisables » sont le serveur, la base de données et les objets que contient la base de données. Chacun de ces éléments possède un ensemble d'autorisations que vous pouvez configurer pour réduire la surface d'exposition de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Le tableau ci-dessous contient des informations sur les principaux et les éléments sécurisables.  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Utilisateurs, rôles et processus de serveur et de base de données|[Principaux &#40;moteur de base de données&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)|  
|Sécurité des serveurs et des objets de base de données|[Éléments sécurisables](../../relational-databases/security/securables.md)|  
|Hiérarchie de sécurité de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|[Hiérarchie des autorisations &#40;moteur de base de données&#41;](../../relational-databases/security/permissions-hierarchy-database-engine.md)|  
| &nbsp; | &nbsp; |
  
### <a name="encryption-and-certificates"></a>Chiffrement et certificats  
 Le chiffrement ne résout pas les problèmes de contrôle d'accès. Toutefois, il améliore la sécurité en limitant la perte de données même dans le cas rare où les contrôles d'accès sont ignorés. Par exemple, si l'ordinateur hôte de la base de données est mal configuré et qu'un utilisateur malveillant parvient à se procurer des données sensibles, telles que des numéros de carte de crédit, les données subtilisées peuvent être inutilisables si elles sont chiffrées. Le tableau ci-dessous contient davantage d'informations sur le chiffrement dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Hiérarchie de chiffrement dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|[Hiérarchie de chiffrement](../../relational-databases/security/encryption/encryption-hierarchy.md)|  
|Implémentation de connexions sécurisées|[Activer les connexions chiffrées dans le moteur de base de données &#40;Gestionnaire de configuration SQL Server&#41;](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md)|  
|Fonctions de chiffrement|[Fonctions de chiffrement &#40;Transact-SQL&#41;](../../t-sql/functions/cryptographic-functions-transact-sql.md)|  
  
 Les certificats sont des « clés » logicielles que se partagent deux serveurs pour permettre des communications sécurisées par le biais d'une authentification renforcée. Vous pouvez créer et utiliser des certificats dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour accroître la sécurité des objets et des connexions. Le tableau ci-dessous contient des informations sur la manière d'utiliser les certificats avec [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Création d’un certificat à utiliser dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|[CREATE CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/create-certificate-transact-sql.md)|  
|Utilisation d'un certificat avec la mise en miroir de bases de données|[Utiliser des certificats pour un point de terminaison de mise en miroir de bases de données &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql.md)|  
| &nbsp; | &nbsp; |

## <a name="application-security"></a>Sécurité des applications

### <a name="client-programs"></a>Programmes clients

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Les méthodes conseillées pour la sécurité incluent notamment la création d’applications clientes sécurisées. Pour plus d’informations sur la façon de mieux sécuriser des applications clientes au niveau de la couche réseau, consultez [Configuration du réseau client](../../database-engine/configure-windows/client-network-configuration.md).

### <a name="windows-defender-application-control-wdac"></a>Contrôle d’application Windows Defender (WDAC)

<!--
This next live paragraph, about Windows Defender Application Control (WDAC), was requested by Bella Brahm, 2019/06/20. (GeneMi)

WDAC can also prevent the kind of highly sophisticated 'Nansh0u' attacks described in 'https://www.guardicore.com/2019/05/nansh0u-campaign-hackers-arsenal-grows-stronger/'. That webpage recommends this present article.
-->

Contrôle d’application Windows Defender (WDAC) empêche l’exécution de code non autorisé. WDAC permet d’atténuer efficacement la menace que posent les logiciels malveillants basés sur des fichiers exécutables. Pour plus d’informations, consultez la documentation propre à [Contrôle d’application Windows Defender](/windows/security/threat-protection/windows-defender-application-control/windows-defender-application-control).

## <a name="sql-server-security-tools-utilities-views-and-functions"></a>Outils, utilitaires, affichages catalogue et fonctions de sécurité SQL Server  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] propose des outils, des utilitaires, des affichages catalogue et des fonctions à l’aide desquels vous pouvez configurer et gérer la sécurité.  
  
### <a name="sql-server-security-tools-and-utilities"></a>Outils et utilitaires de sécurité SQL Server  
 Le tableau ci-dessous contient des informations sur les outils et les utilitaires [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dont vous pouvez vous servir pour configurer et administrer la sécurité.  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Connexion à, configuration et contrôle [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|[Utiliser SQL Server Management Studio](../../ssms/sql-server-management-studio-ssms.md)|  
|Connexion à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et exécution de requêtes à partir de l'invite de commandes|[Utilitaire sqlcmd](../../tools/sqlcmd-utility.md)|  
|Configuration et contrôle réseau pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|[Gestionnaire de configuration SQL Server](../../relational-databases/sql-server-configuration-manager.md)|  
|Activation et désactivation de fonctionnalités à l'aide de la gestion basée sur une stratégie|[Administrer des serveurs à l'aide de la Gestion basée sur des stratégies](../../relational-databases/policy-based-management/administer-servers-by-using-policy-based-management.md)|  
|Manipulation des clés symétriques d'un serveur de rapports|[Utilitaire rskeymgmt &#40;SSRS&#41;](../../reporting-services/tools/rskeymgmt-utility-ssrs.md)|  
| &nbsp; | &nbsp; |

### <a name="sql-server-security-catalog-views-and-functions"></a>Affichages catalogue et fonctions de sécurité SQL Server  
 Le [!INCLUDE[ssDE](../../includes/ssde-md.md)] présente des informations de sécurité dans plusieurs affichages et fonctions optimisés pour des raisons de performance et d'utilisation. Le tableau ci-dessous contient des informations sur les vues et les fonctions de sécurité.  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Affichages catalogue de la sécurité retournant des informations sur les autorisations de niveau serveur et base de données, les principaux, les rôles, etc. En outre, des affichages catalogue fournissent des informations sur les clés de chiffrement, les certificats et les informations d'identification.|[Affichages catalogue de sécurité &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Fonctions de sécurité qui retournent des informations sur l’utilisateur actuel, les autorisations et les schémas.|[Fonctions de sécurité &#40;Transact-SQL&#41;](../../t-sql/functions/security-functions-transact-sql.md)|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Vues de gestion dynamique de la sécurité.|[Fonctions et vues de gestion dynamique relatives à la sécurité &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/security-related-dynamic-management-views-and-functions-transact-sql.md)|  
| &nbsp; | &nbsp; |

## <a name="related-content"></a>Contenu associé  
 [Considérations sur la sécurité pour une installation SQL Server](../../sql-server/install/security-considerations-for-a-sql-server-installation.md)  
 [Centre de sécurité pour le moteur de base de données SQL Server et Azure SQL Database](../../relational-databases/security/security-center-for-sql-server-database-engine-and-azure-sql-database.md)  
[Bonnes pratiques de sécurité de SQL Server 2012 - Tâches opérationnelles et administratives](https://download.microsoft.com/download/8/F/A/8FABACD7-803E-40FC-ADF8-355E7D218F4C/SQL_Server_2012_Security_Best_Practice_Whitepaper_Apr2012.docx)   
[Blog de sécurité SQL Server](/archive/blogs/sqlsecurity/)  
[Livres blancs sur les bonnes pratiques de sécurité et la sécurité des étiquettes](/archive/blogs/sqlsecurity/security-best-practice-and-label-security-whitepapers)  
[Sécurité au niveau des lignes](../../relational-databases/security/row-level-security.md)   
[Protection de votre propriété intellectuelle SQL Server](../../relational-databases/security/protecting-your-sql-server-intellectual-property.md)   
