---
title: Notes de publication de SQL Server 2019 | Microsoft Docs
description: Consultez les informations sur les limitations de SQL Server 2019 (15.x), les problèmes connus, les ressources d’aide et d’autres notes de publication.
ms.date: 11/04/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: release-landing
ms.topic: article
ms.assetid: 13942af8-5a40-4cef-80f5-918386767a47
author: MikeRayMSFT
ms.author: mikeray
monikerRange: = sql-server-ver15
ms.openlocfilehash: e8a9f06dc33acab6862027cd2fdb55bb4aca4dfc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100080430"
---
# <a name="sql-server-2019-release-notes"></a>Notes de publication de [!INCLUDE[SQL Server 2019](../includes/sssql19-md.md)]
[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Cet article décrit les limitations et les problèmes connus pour [!INCLUDE[SQL Server 2019](../includes/sssql19-md.md)]. Pour plus d’informations, consultez :

> [Nouveautés de [!INCLUDE[SQL Server 2019](../includes/sssql19-md.md)]](../sql-server/what-s-new-in-sql-server-ver15.md)

## [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)]

[!INCLUDE[SQL Server 2019](../includes/sssql19-md.md)] est la dernière version publique de [!INCLUDE[SQL Server 2019](../includes/ssnoversion-md.md)].

Vous trouverez des informations complètes sur les licences dans le dossier `License Terms` sur le support d’installation.

## <a name="documentation"></a>Documentation

- **Problème et impact sur le client** : la documentation [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] peut être filtrée par version. Utilisez le contrôle en haut à gauche de chaque page de documentation pour filtrer selon vos besoins.

## <a name="build-number"></a>Numéro de build

Le numéro de build RTM pour SQL Server 2019 est `15.0.2000.5`.

[!INCLUDE [sql-server-servicing-updates-version-15](../includes/sql-server-servicing-updates-version-15.md)]

## <a name="sql-server-installation-may-fail-if-ssms-18x-is-installed"></a>L’installation de SQL Server peut échouer si SSMS 18.x est installé

- **Problème et impact sur le client** : [!INCLUDE[SQL Server 2019](../includes/sssql19-md.md)] l’installation échoue lorsque les installations suivantes sont effectuées dans cet ordre :
  1. SQL Server Management Studio (SSMS) version 18.0, 18.1, 18.2 ou 18.3 est installé sur le serveur.
  1. Une tentative d’installation de [!INCLUDE[SQL Server 2019](../includes/sssql19-md.md)] est effectuée à partir d’un support amovible. Par exemple, le support d’installation est un DVD.

- **Solution de contournement** :
  1. Désinstallez toute version de SSMS antérieure à SSMS 18.3.1.
  1. Installez une version plus récente de SSMS (18.3.1 ou une version ultérieure). Pour obtenir la dernière version, consultez [Télécharger SSMS](../ssms/download-sql-server-management-studio-ssms.md).
  1. Installez [!INCLUDE[SQL Server 2019](../includes/sssql19-md.md)] normalement.

- **S’applique à** : [!INCLUDE[SQL Server 2019](../includes/sssql19-md.md)]

## <a name="utf-8-collations"></a>Classements UTF-8

- **Problème et impact sur le client** : les classements prenant en charge UTF-8 ne peuvent pas être utilisés avec les fonctionnalités suivantes :
  - [Tables OLTP en mémoire](../relational-databases/in-memory-oltp/introduction-to-memory-optimized-tables.md)
  - [Always Encrypted avec enclaves sécurisées](../relational-databases/security/encryption/always-encrypted-enclaves.md) (quand vous n’utilisez pas d’enclaves sécurisées, [Always Encrypted](../relational-databases/security/encryption/always-encrypted-database-engine.md) peut utiliser UTF-8)

  > [!WARNING]
  > La création d’un [bacpac](../relational-databases/data-tier-applications/data-tier-applications.md#bacpac) d’une base de données contenant des colonnes de table définies en tant que [CHAR ou VARCHAR](../t-sql/data-types/char-and-varchar-transact-sql.md) qui utilisent plus de 4000 octets échoue.
  
  > [!NOTE]
  > Actuellement, l’interface utilisateur d’Azure Data Studio et de SQL Server Data Tools (SSDT) ne permet pas de choisir des classements prenant en charge UTF-8. Il en va différemment de l’interface utilisateur de la dernière version 18 de [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] SSMS.

- **S’applique à** : [!INCLUDE[SQL Server 2019](../includes/sssql19-md.md)] RTM

## <a name="master-data-service-notification-email-contains-broken-link"></a>L’e-mail de notification Master Data Services contient un lien rompu

- **Problème et impact sur le client** : L’e-mail de notification Master Data Services (MDS) contient un lien rompu. Le lien accède à une page qui retourne une erreur semblable au message suivant :

   `The view 'Index' or its master was not found or no view engine supports the searched locations.`

- **Solution de contournement** : Ouvrez le portail MDS et accédez à la ressource manuellement.

- **S’applique à** : [!INCLUDE[SQL Server 2019](../includes/sssql19-md.md)] RTM

## <a name="see-also"></a>Voir aussi

- [Configurations matérielle et logicielle requises pour l’installation de SQL Server](../sql-server/install/hardware-and-software-requirements-for-installing-sql-server-ver15.md)

## <a name="machine-learning-services"></a>Machine Learning Services

Pour les problèmes rencontrés dans SQL Server Machine Learning Services, consultez [Problèmes connus dans SQL Server Machine Learning Services](../machine-learning/troubleshooting/known-issues-for-sql-server-machine-learning-services.md).

[!INCLUDE[get-help-options-msft-only](../includes/paragraph-content/get-help-options.md)]
