---
title: Configuration requise (ODBC Driver pour SQL Server)
description: Cette rubrique répertorie la configuration requise pour ODBC Driver pour SQL Server sur Linux et les systèmes d’exploitation macOS.
ms.custom: ''
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- prerequisites
- system requirements
- requirements
ms.assetid: f03b7fdd-0e9d-4e74-958d-e8c87e027348
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: f78fdfcb8fe19856483403c4675f973c4f5c97b3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198382"
---
# <a name="system-requirements-linux-and-macos"></a>Configuration requise (Linux et macOS)

[!INCLUDE[Driver_ODBC_Download](../../../includes/driver_odbc_download.md)]

Cette rubrique répertorie la configuration requise pour utiliser [!INCLUDE[msCoName](../../../includes/msconame_md.md)] ODBC Driver for [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sur Linux et macOS.

## <a name="sql-version-compatibility"></a>Compatibilité des versions de SQL

La compatibilité des versions SQL avec les pilotes Linux et macOS est identique à celle des [pilotes Windows](../windows/system-requirements-installation-and-driver-files.md#sql-version-compatibility).

## <a name="operating-system-support"></a>Prise en charge du système d’exploitation

Les versions 17, 13.1 et 13 des pilotes Linux et macOS sont prises en charge sur l’architecture 64 bits des systèmes d’exploitation suivants :

|Version du pilote&nbsp;&#8594;<br />Système d’exploitation &#8595;     |17,7|17.6|17.5|17.4|17.3|17.2|17.1|17.0|13.1|13|
|-------------------------------|----|----|----|----|----|----|----|----|----|---|
|Apple OS X 10.11 (El Capitan)  |    |    |    |Oui |Oui |Oui |Oui |Oui |Oui |Oui|
|Apple macOS 10.12 (Sierra)     |    |    |    |Oui |Oui |Oui |Oui |Oui |Oui |Oui|
|Apple macOS 10.13 (High Sierra)|    |Oui |Oui |Oui |Oui |Oui |Oui |Oui |Oui |Oui|
|Apple macOS 10.14 (Mojave)     |Oui |Oui |Oui |Oui |Oui |    |    |    |    |   |
|Apple macOS 10.15 (Catalina)   |Oui |Oui |Oui |    |    |    |    |    |    |   |
|Apple macOS 11.0 (Big Sur)     |Oui |    |    |    |    |    |    |    |    |   |
|Alpine Linux 3.11              |Oui |Oui |Oui |    |    |    |    |    |    |   |
|Alpine Linux 3.12              |Oui |    |    |    |    |    |    |    |    |   |
|Debian Linux 8                 |    |Oui |Oui |Oui |Oui |Oui |Oui |Oui |Oui |Oui|
|Debian Linux 9                 |Oui |Oui |Oui |Oui |Oui |Oui |Oui |Oui |Oui |Oui|
|Debian Linux 10                |Oui |Oui |Oui |Oui |    |    |    |    |    |   |
|Oracle Linux 7                 |Oui |    |    |    |    |    |    |    |    |   |
|Oracle Linux 8                 |Oui |Oui |Oui |    |    |    |    |    |    |   |
|Red Hat Enterprise Linux 6      |Oui |Oui |Oui |Oui |Oui |Oui |Oui |Oui |Oui |Oui|
|Red Hat Enterprise Linux 7      |Oui |Oui |Oui |Oui |Oui |Oui |Oui |Oui |Oui |Oui|
|RedHat Enterprise Linux 8      |Oui |Oui |Oui |Oui |    |    |    |    |    |   |
|SUSE Linux Enterprise Server 11<sup>1</sup>|Oui |Oui |Oui |Oui |Oui |Oui |Oui |Oui |Oui |Oui|
|SUSE Linux Enterprise Server 12|Oui |Oui |Oui |Oui |Oui |Oui |Oui |Oui |Oui |Oui|
|SUSE Linux Enterprise Server 15|Oui |Oui |Oui |Oui |Oui |    |    |    |    |   |
|Ubuntu Linux 14.04             |    |    |    |Oui |Oui |Oui |Oui |Oui |Oui |Oui|
|Ubuntu Linux 16.04             |Oui |Oui |Oui |Oui |Oui |Oui |Oui |Oui |Oui |Oui|
|Ubuntu Linux 18.04             |Oui |Oui |Oui |Oui |Oui |Oui |    |    |    |   |
|Ubuntu Linux 20.04             |Oui |Oui |    |    |    |    |    |    |    |   |
|Ubuntu Linux 20.10             |Oui |    |    |    |    |    |    |    |    |   |

<sup>1</sup> ODBC Driver 17 prend uniquement en charge SUSE Linux Enterprise Server 11 SP4.

Les packages d’installation de [!INCLUDE[msCoName](../../../includes/msconame_md.md)] ODBC Driver 13, 13.1 et 17 pour [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sur Linux et macOS résolvent automatiquement les dépendances du pilote quand ils sont installés à l’aide du système de gestion des packages de votre distribution, comme décrit dans [Installer le pilote ODBC (Linux)](installing-the-microsoft-odbc-driver-for-sql-server.md) et [Installer le pilote ODBC (macOS)](install-microsoft-odbc-driver-sql-server-macos.md).

## <a name="microsoft-odbc-driver-11-for-sql-server"></a>Pilote Microsoft ODBC 11 pour SQL Server  
  
* Gestionnaire de pilotes UnixODBC 2.3.0 64 bits, conçu pour SQLLEN/SQLULEN 64 bits. Les versions ultérieures du Gestionnaire de pilotes UnixODBC 64 bits ne sont pas prises en charge avec le pilote ODBC sur Linux. Consultez la rubrique [Installing the Driver Manager](../../../connect/odbc/linux-mac/installing-the-driver-manager.md) (éventuellement en anglais) pour plus d'informations.  
  
* Le pilote ODBC pour **Red Hat Enterprise Linux 5 (64 bits)** requiert les packages suivants que vous pouvez télécharger ici : [Microsoft ODBC Driver 11 for SQL Server - Red Hat Linux](https://go.microsoft.com/fwlink/?LinkId=267321)  
  * `glibc`  
  * `libgcc`  
  * `libstdc++`  
  * `e2fsprogs-libs`  
  * `krb5-libs`  
  * `openssl`  
  
* Le pilote ODBC pour **Red Hat Enterprise Linux 6 (64 bits)** requiert les packages suivants que vous pouvez télécharger ici : [Microsoft ODBC Driver 11 for SQL Server - Red Hat Linux](https://go.microsoft.com/fwlink/?LinkId=267321)  
  * `glibc`  
  * `libgcc`  
  * `libstdc++`  
  * `libuuid`  
  * `krb5-libs`  
  * `openssl`  
  
* Le pilote ODBC pour **SUSE Linux Enterprise 11 Service Pack 2 (64 bits)** requiert les packages suivants que vous pouvez télécharger ici : [Microsoft ODBC Driver 11 Preview for SQL Server - SUSE Linux](https://go.microsoft.com/fwlink/?LinkId=264916)  
  * `glibc`  
  * `libstdc++46`  
  * `libgcc46`  
  * `libuuid1`  
  * `krb5`  
  * `libopenssl0_9_8`  
  
## <a name="see-also"></a>Voir aussi

[Installation du Gestionnaire de pilotes](../../../connect/odbc/linux-mac/installing-the-driver-manager.md)  
[Problèmes connus dans cette version du pilote](../../../connect/odbc/linux-mac/known-issues-in-this-version-of-the-driver.md)  
[Notes de publication](../../../connect/odbc/linux-mac/release-notes-odbc-sql-server-linux-mac.md)  
