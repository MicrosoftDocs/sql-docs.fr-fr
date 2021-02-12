---
title: Vue d’ensemble des pilotes Microsoft pour PHP pour SQL Server
description: Découvrez une vue d’ensemble des Pilotes Microsoft SQLSRV et PDO_SQSRV pour PHP pour SQL Server et la façon dont vous pouvez les utiliser dans une application PHP pour accéder aux bases de données.
ms.custom: ''
ms.date: 03/27/2018
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 66559249-34c0-409d-b919-9b5bf0c4c9ec
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 687f8a87ea03ef661b96af27a574bb5cea09f847
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100045269"
---
# <a name="overview-of-the-microsoft-drivers-for-php-for-sql-server"></a>Vue d’ensemble des pilotes Microsoft pour PHP pour SQL Server

[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

[!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)] est une extension PHP qui fournit l’accès aux données de SQL Server 2005 et des versions ultérieures, y compris Azure SQL Database. L’extension fournit une interface procédurale avec le pilote SQLSRV et une interface orientée objet avec le pilote PDO_SQLSRV pour accéder aux données dans toutes les versions de SQL Server, dont Express, à compter de SQL Server 2005. La prise en charge de la version 3.1 et des versions ultérieures des pilotes commence avec SQL Server 2008. L’API [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)] prend en charge l’authentification Windows, les transactions, la liaison de paramètre, la diffusion en continu, l’accès aux métadonnées et la gestion des erreurs.  
  
La bonne version de SQL Server Native Client ou de Microsoft ODBC Drive doit être installée sur l’ordinateur où s’exécute PHP pour qu’il soit possible d’utiliser [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)].  Pour plus d’informations, consultez [Configuration système requise pour les pilotes Microsoft pour PHP pour SQL Server](../../connect/php/system-requirements-for-the-php-sql-driver.md).  
  
## <a name="in-this-section"></a>Dans cette section  
  
|Rubrique|Description|  
|---------|---------------|  
| ![Télécharger-FlècheBas-Entourée](../../ssms/media/download-icon.png)[Télécharger les pilotes pour PHP pour SQL Server](download-drivers-php-sql-server.md) | Liens de téléchargement des pilotes Microsoft pour PHP pour SQL Server. |
|[Notes de publication de Microsoft Drivers for PHP for SQL Server](../../connect/php/release-notes-php-sql-driver.md)|Présente les fonctionnalités ajoutées pour les versions 4.0, 3.2, 3.1, 3.0 et 2.0.|  
|[Ressources de support sur les pilotes Microsoft pour PHP pour SQL Server](../../connect/php/support-resources-for-the-php-sql-driver.md)|Fournit des liens vers des ressources qui peuvent être utiles quand vous développez des applications qui utilisent [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)].|  
|[À propos des exemples de code dans la documentation](../../connect/php/about-code-examples-in-the-documentation.md)|Fournit des informations qui peuvent être utiles quand vous exécutez les exemples de code dans cette documentation.|  
| &nbsp; | &nbsp; |

## <a name="reference"></a>Informations de référence

[Informations de référence sur l’API du pilote SQLSRV](../../connect/php/sqlsrv-driver-api-reference.md)  
[Référence de pilote PDO_SQLSRV](../../connect/php/pdo-sqlsrv-driver-reference.md)  
[Constantes &#40;Microsoft Drivers for PHP for SQL Server&#41;](../../connect/php/constants-microsoft-drivers-for-php-for-sql-server.md)  

## <a name="see-also"></a>Voir aussi

[Bien démarrer avec les pilotes Microsoft pour PHP pour SQL Server](../../connect/php/getting-started-with-the-php-sql-driver.md)  
[Guide de programmation pour les pilotes Microsoft pour PHP pour SQL Server](../../connect/php/programming-guide-for-php-sql-driver.md)  
[Exemple d’application &#40;pilote SQLSRV&#41;](../../connect/php/example-application-sqlsrv-driver.md)  
