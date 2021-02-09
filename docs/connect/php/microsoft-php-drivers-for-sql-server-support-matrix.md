---
title: Matrice de prise en charge des pilotes Microsoft pour PHP
description: Cette page contient la matrice de support et la politique de support des pilotes Microsoft PHP pour SQL Server.
ms.custom: ''
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ca05a57c88f93a890cf546b3b1c60382f672d3be
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076431"
---
# <a name="microsoft-php-drivers-for-sql-server-support-matrix"></a>Matrice de prise en charge des pilotes PHP pour SQL Server

[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Cette page contient la matrice de support et la politique de support des pilotes Microsoft PHP pour SQL Server.

## <a name="microsoft-php-drivers-support-lifecycle-matrix-and-policy"></a>Matrice et politique de support des pilotes Microsoft PHP

La politique de support de Microsoft fournit des informations transparentes et prévisibles concernant la politique de support des produits Microsoft. Les versions 3.x, 4.x et 5.x des pilotes PHP bénéficient de cinq ans de support standard à partir de la date de publication du pilote. Le support standard est défini sur le [site web de la politique de support de Microsoft](https://support.microsoft.com/lifecycle).

Les options de support étendu et personnalisé ne sont pas disponibles pour les pilotes Microsoft PHP.

Les pilotes Microsoft PHP suivants bénéficient d’un support jusqu’à la date de fin de support indiquée.

|Nom du pilote|Version de package du pilote|Fin du support standard|
|-|:-:|-|
|Microsoft PHP Drivers 5.9 pour SQL Server|5.9|29 janvier 2026|
|Microsoft PHP Drivers 5.8 pour SQL Server|5.8|31 janvier 2025|
|Microsoft PHP Drivers 5.6 pour SQL Server|5.6|21 février 2024|
|Microsoft PHP Drivers 5.3 pour SQL Server|5.3|20 juillet 2023|
|Microsoft PHP Drivers 5.2 pour SQL Server|5.2|9 février 2023|
|Microsoft PHP Drivers 4.3 pour SQL Server|4.3|6 juillet 2022|
|Microsoft PHP Drivers 4.0 pour SQL Server|4.0|11 juillet 2021|
| &nbsp; | &nbsp; | &nbsp; |

Les pilotes Microsoft PHP suivants ne sont plus supportés.

|Nom du pilote|Version de package du pilote|Fin du support standard|
|-|:-:|-|
|Microsoft PHP Drivers 3.2 pour SQL Server|3.2|9 mars 2020|
|Microsoft PHP Drivers 3.1 pour SQL Server|3.1|12 décembre 2019|
|Microsoft PHP Drivers 3.0 pour SQL Server|3.0|6 mars 2017|
|Microsoft PHP Drivers 2.0 pour SQL Server|2.0|10 août 2015|
|Microsoft PHP Drivers 1.0 pour SQL Server|1.0|28 avril 2014|
| &nbsp; | &nbsp; | &nbsp; |

## <a name="sql-server-version-certified-compatibility"></a>Compatibilité certifiée de la version SQL Server
 La matrice suivante répertorie les versions de bases de données qui ont été testées et certifiées comme étant compatibles avec la version du pilote correspondante. Nous nous efforçons de maintenir une compatibilité descendante avec les versions de pilote précédentes, mais seul le dernier pilote pris en charge est testé et certifié avec les nouvelles versions de SQL Server, à mesure que SQL Server est publié.

|Version du pilote&nbsp;&#8594;<br />Version de base de données &#8595;|5.9|5.8|5.6|5.3|5.2|4.3|4.0|3.2|
|---|---|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
|Azure SQL Database        |Oui|Oui|Oui|Oui|Oui|Oui|   |   |
|Azure SQL Managed Instance|Oui|Oui|Oui|Oui|Oui|Oui|   |   |
|Azure Synapse Analytics   |Oui|Oui|Oui|Oui|Oui|Oui|   |   |
|SQL Server 2019           |Oui|Oui|   |   |   |   |   |   |
|SQL Server 2017           |Oui|Oui|Oui|Oui|Oui|Oui|   |   |
|SQL Server 2016           |Oui|Oui|Oui|Oui|Oui|Oui|Oui|   |
|SQL Server 2014           |Oui|Oui|Oui|Oui|Oui|Oui|Oui|Oui|
|SQL Server 2012           |Oui|Oui|Oui|Oui|Oui|Oui|Oui|Oui|
|SQL Server 2008 R2        |   |   |Oui|Oui|Oui|Oui|Oui|Oui|
|SQL Server 2008           |   |   |   |   |   |   |Oui|Oui|
| &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; |

Pour plus d’informations sur l’utilisation de PHP avec Azure SQL Database, consultez [Connexion à Microsoft Azure SQL Database](connecting-to-microsoft-azure-sql-database.md).

## <a name="php-version-support"></a>Prise en charge de la version de PHP

Les versions suivantes de PHP sont prises en charge avec la version répertoriée des pilotes Microsoft PHP :

|Version du pilote&nbsp;&#8594;<br />&#8595; Version de PHP|5.9|5.8|5.6|5.3|5.2|4.3|4.0|3.2|
|:---:|---|---|---|---|---|---|---|---|
|8.0|8.0.0+ |                |                |                |                |       |        |        |
|7.4|7.4.0+ |7.4.0+          |                |                |                |       |        |        |
|7.3|7.3.0+ |7.3.0+          |7.3.0+          |                |                |       |        |        |
|7.2|       |7.2 +<sup>1</sup>|7.2 +<sup>1</sup>|7.2 +<sup>1</sup>|7.2 +<sup>1</sup>|       |        |        |
|7.1|       |                |7.1.0+          |7.1.0+          |7.1.0+          |7.1.0+ |        |        |
|7.0|       |                |                |7.0.0+          |7.0.0+          |7.0.0+ |7.0.0+  |        |
|5.6|       |                |                |                |                |       |        |5.6.4+  |
|5.5|       |                |                |                |                |       |        |5.5.16+ |
|5.4|       |                |                |                |                |       |        |5.4.32  |
| &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; |

<sup>1</sup> Les versions 7.2.1 et ultérieures sont prises en charge sur Windows, tandis que les versions 7.2.0 et ultérieures sont prises en charge sur Linux et macOS.

## <a name="supported-operating-systems"></a>Systèmes d’exploitation pris en charge

Les versions suivantes du système d'exploitation Windows sont prises en charge avec la version répertoriée des pilotes Microsoft PHP :

|Version du pilote&nbsp;&#8594;<br />&#8595; Système d’exploitation|5.9|5.8|5.6|5.3|5.2|4.3|4.0|3.2|
|---|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
|Windows Server 2019                 |Oui|Oui|Oui|   |   |   |   |   |
|Windows Server 2016                 |Oui|Oui|Oui|Oui|Oui|Oui|   |   |
|Windows Server 2012 R2              |Oui|Oui|Oui|Oui|Oui|Oui|Oui|Oui|
|Windows Server 2012                 |Oui|Oui|Oui|Oui|Oui|Oui|Oui|Oui|
|Windows Server 2008 R2 SP1          |   |   |   |   |   |   |Oui|Oui|
|Windows Server 2008 R2              |   |   |   |   |   |   |   |   |
|Windows Server 2008 SP2             |   |   |   |   |   |   |Oui|Oui|
|Windows 10                          |Oui|Oui|Oui|Oui|Oui|Oui|Oui|   |
|Windows 8.1                         |Oui|Oui|Oui|Oui|Oui|Oui|Oui|Oui|
|Windows 8                           |   |   |   |   |   |Oui|Oui|Oui|
|Windows 7 SP1                       |   |   |   |   |   |   |Oui|Oui|
|Windows Vista SP2                   |   |   |   |   |   |   |Oui|Oui|
| &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; |

Les versions suivantes des systèmes d'exploitation Linux et macOS (64 bits uniquement) sont prises en charge avec la version répertoriée des pilotes Microsoft PHP :

|Version du pilote&nbsp;&#8594;<br />&#8595; Système d’exploitation|5.9|5.8|5.6|5.3|5.2|4.3|4.0|3.2|
|---|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
|Ubuntu 20.10 (64 bits)               |Vrai|   |   |   |   |   |   |   |
|Ubuntu 20.04 (64 bits)               |Oui|Oui|   |   |   |   |   |   |
|Ubuntu 19.10 (64 bits)               |   |Oui|   |   |   |   |   |   |
|Ubuntu 18.10 (64 bits)               |   |   |Oui|   |   |   |   |   |
|Ubuntu 18.04 (64 bits)               |Oui|Oui|Oui|Oui|   |   |   |   |
|Ubuntu 17.10 (64 bits)               |   |   |   |Oui|Oui|   |   |   |
|Ubuntu 16.04 (64 bits)               |Oui|Oui|Oui|Oui|Oui|Oui|Oui|   |
|Ubuntu 15.10 (64 bits)               |   |   |   |   |   |Oui|   |   |
|Ubuntu 15.04 (64 bits)               |   |   |   |   |   |   |Oui|   |
|Debian 10 (64 bits)                  |Oui|Oui|   |   |   |   |   |   |
|Debian 9 (64 bits)                   |Oui|Oui|Oui|Oui|Oui|   |   |   |
|Debian 8 (64 bits)                   |   |Oui|Oui|Oui|Oui|Oui|   |   |
|Red Hat Enterprise Linux 8 (64 bits) |Oui|Oui|   |   |   |   |   |   |
|Red Hat Enterprise Linux 7 (64-bit) |Oui|Oui|Oui|Oui|Oui|Oui|Oui|   |
|Red Hat Enterprise Linux 15 (64 bits)   |Oui|Oui|Oui|   |   |   |   |   |
|Red Hat Enterprise Linux 12 (64 bits)   |Oui|Oui|Oui|Oui|Oui|   |   |   |
|Alpine Linux 3.12 (64 bits)          |Vrai|   |   |   |   |   |   |   |
|Alpine Linux 3.11 (64 bits)          |Oui|Oui<sup>1</sup>|   |   |   |   |   |   |
|macOS Big Sur (64 bits)              |Oui|   |   |   |   |   |   |   |
|macOS Catalina (64 bits)             |Oui|Oui|   |   |   |   |   |   |
|macOS Mojave (64 bits)               |Oui|Oui|Oui|   |   |   |   |   |
|macOS High Sierra (64 bits)          |   |Oui|Oui|Oui|   |   |   |   |
|macOS Sierra (64 bits)               |   |   |Oui|Oui|Oui|Oui|   |   |
|macOS El Capitan (64 bits)           |   |   |   |Oui|Oui|Oui|   |   |
| &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; |

<sup>1</sup> La prise en charge d’Alpine Linux est expérimentale pour la version 5.8.0. La version 5.8.1 ajoute la prise en charge de la production.

## <a name="see-also"></a>Voir aussi

[Notes de publication](release-notes-php-sql-driver.md)

[Ressources de support](support-resources-for-the-php-sql-driver.md)

[Configuration requise](system-requirements-for-the-php-sql-driver.md)
