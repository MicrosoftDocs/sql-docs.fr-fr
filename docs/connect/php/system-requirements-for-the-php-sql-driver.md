---
title: Configuration système requise pour les pilotes Microsoft SQL Server pour PHP
description: Les pilotes Microsoft pour PHP pour SQL Server prennent en charge un large éventail de versions de PHP, de systèmes d’exploitation et de versions de SQL Server.
ms.custom: ''
ms.date: 08/06/2020
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- requirements
ms.assetid: 5db4b75f-c605-4785-9560-399a533c0fc9
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: e0ae11dd3a13ac8b2071943c49ef1ae4b8c400f4
ms.sourcegitcommit: dd36d1cbe32cd5a65c6638e8f252b0bd8145e165
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/08/2020
ms.locfileid: "89540463"
---
# <a name="system-requirements-for-the-microsoft-drivers-for-php-for-sql-server"></a>Configuration système requise pour Microsoft Drivers for PHP for SQL Server

[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Cet article répertorie les composants qui doivent être installés sur votre système pour accéder aux données SQL Server ou Azure SQL Database en utilisant [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)].

La version 3.2 et versions ultérieures des pilotes Microsoft PHP pour SQL Server sont officiellement prises en charge. Pour plus d’informations sur les cycles de vie de la prise en charge et la configuration requise des pilotes PHP, consultez le [tableau de prise en charge](microsoft-php-drivers-for-sql-server-support-matrix.md).

## <a name="php"></a>PHP

Pour plus d’informations sur le téléchargement et l’installation des derniers fichiers binaires PHP stables, consultez le [site web PHP](https://php.net).  Les pilotes Microsoft SQL Server pour PHP requièrent les versions appropriées de PHP, comme indiqué dans [Prise en charge de la version de PHP](microsoft-php-drivers-for-sql-server-support-matrix.md#php-version-support).

-   La version correcte du fichier de pilote doit être activée avec sa version PHP correspondante. Consultez [Versions de pilotes](#driver-versions) pour plus d’informations sur les différents fichiers de pilotes.  Pour télécharger les pilotes, consultez [Télécharger Microsoft Drivers for PHP for SQL Server](download-drivers-php-sql-server.md). Pour plus d’informations sur la configuration du pilote PHP, consultez [Chargement de Microsoft Drivers for PHP for SQL Server](loading-the-php-sql-driver.md).

-   Un serveur web est nécessaire. Votre serveur web doit être configuré pour exécuter PHP. Pour plus d’informations sur l’hébergement d’applications PHP avec IIS, consultez le [tutoriel sur le site web de PHP](http://docs.php.net/manual/da/install.windows.iis7.php).

    Les [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)] ont été testés à l’aide d’IIS 10 avec FastCGI.

    > [!NOTE]
    > Microsoft prend en charge uniquement IIS.

## <a name="odbc-driver"></a>Pilote ODBC

La version correcte de Microsoft ODBC Driver pour SQL Server est requise sur l’ordinateur sur lequel PHP est en cours d’exécution. Vous pouvez télécharger sur [cette page](../odbc/download-odbc-driver-for-sql-server.md) toutes les versions prises en charge du pilote pour les plateformes prises en charge.

Si vous téléchargez la version Windows du pilote sur une version 64 bits de Windows, le programme d’installation ODBC 64 bits installe les pilotes ODBC 32 bits et 64 bits. Si vous utilisez une version 32 bits de Windows, utilisez le programme d’installation ODBC x86. Sur les plateformes non Windows, seules les versions 64 bits du pilote sont disponibles.

|Version du pilote PHP &#8594;<br />Version du pilote ODBC &#8595;|5.8|5.6|5.3|5.2|4.3|4.0|3.2|
|---|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
|ODBC Driver 17+ |Oui|Oui|Oui|Oui|   |   |   |
|ODBC Driver 13.1|Oui|Oui|Oui|Oui|Oui|Oui|   |
|ODBC Driver 13  |   |   |   |   |   |Oui|   |
|ODBC Driver 11  |Oui|Oui|Oui|Oui|Oui|Oui|Oui|

Si vous utilisez le pilote SQLSRV, [sqlsrv_client_info](sqlsrv-client-info.md) retourne des informations sur la version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Microsoft ODBC Driver pour SQL Server utilisée par [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]. Si vous utilisez le pilote PDO_SQLSRV, vous pouvez utiliser [PDO::getAttribute](pdo-getattribute.md) pour découvrir la version.

## <a name="sql-server"></a>SQL Server

Pour plus d’informations sur les versions de SQL Server prises en charge, consultez les [versions de bases de données prises en charge](microsoft-php-drivers-for-sql-server-support-matrix.md#sql-server-version-certified-compatibility).

## <a name="operating-systems"></a>Systèmes d'exploitation

Pour plus d’informations sur les systèmes d’exploitation pris en charge, consultez [Systèmes d’exploitation pris en charge](microsoft-php-drivers-for-sql-server-support-matrix.md#supported-operating-systems).

## <a name="driver-versions"></a>Versions de pilotes

Cette section répertorie les pilotes qui sont inclus dans chaque version du [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]. Chaque package d’installation contient les fichiers de pilote SQLSRV et PDO_SQLSRV dans les variantes avec et sans thread. Sur Windows, ils sont également disponibles en version 32 bits et 64 bits. Pour configurer le pilote à utiliser avec le runtime PHP, suivez les instructions d’installation fournies dans [Chargement des pilotes Microsoft SQL Server pour PHP](loading-the-php-sql-driver.md).

Sur les versions Linux et macOS prises en charge, les pilotes appropriés peuvent être installés à l’aide du système de package PECL de PHP, en suivant les [instructions d’installation pour Linux et macOS](installation-tutorial-linux-mac.md). Vous pouvez également télécharger des fichiers binaires prédéfinis pour votre plateforme sur la page du projet GitHub [Pilotes Microsoft SQL Server pour PHP](https://github.com/Microsoft/msphpsql/releases) ; les tableaux ci-dessous répertorient les fichiers disponibles dans les packages binaires prédéfinis.

**Microsoft Drivers 5.8 PHP pour SQL Server :**

Sur Windows, les versions suivantes du pilote sont incluses :

|Fichier de pilote|Version PHP|Thread-safe ?|Utiliser avec .dll PHP|
|---------------|:---------------:|:----------------:|---------------------|
|32-bit php_sqlsrv_72_nts.dll<br />32-bit php_pdo_sqlsrv_72_nts.dll|7.2|non |php7.dll 32 bits|
|32-bit php_sqlsrv_72_ts.dll <br />32-bit php_pdo_sqlsrv_72_ts.dll |7.2|Oui|php7ts.dll 32 bits|
|64-bit php_sqlsrv_72_nts.dll<br />64-bit php_pdo_sqlsrv_72_nts.dll|7.2|non |php7.dll 64 bits|
|64-bit php_sqlsrv_72_ts.dll <br />64-bit php_pdo_sqlsrv_72_ts.dll |7.2|Oui|php7ts.dll 64 bits|
|32-bit php_sqlsrv_73_nts.dll<br />32-bit php_pdo_sqlsrv_73_nts.dll|7.3|non |php7.dll 32 bits|
|32-bit php_sqlsrv_73_ts.dll <br />32-bit php_pdo_sqlsrv_73_ts.dll |7.3|Oui|php7ts.dll 32 bits|
|64-bit php_sqlsrv_73_nts.dll<br />64-bit php_pdo_sqlsrv_73_nts.dll|7.3|non |php7.dll 64 bits|
|64-bit php_sqlsrv_73_ts.dll <br />64-bit php_pdo_sqlsrv_73_ts.dll |7.3|Oui|php7ts.dll 64 bits|
|php_sqlsrv_74_nts.dll 32 bits<br />php_pdo_sqlsrv_74_nts.dll 32 bits|7.4|non |php7.dll 32 bits|
|php_sqlsrv_74_ts.dll 32 bits <br />php_pdo_sqlsrv_74_ts.dll 32 bits |7.4|Oui|php7ts.dll 32 bits|
|php_sqlsrv_74_nts.dll 64 bits<br />php_pdo_sqlsrv_74_nts.dll 64 bits|7.4|non |php7.dll 64 bits|
|php_sqlsrv_74_ts.dll 64 bits <br />php_pdo_sqlsrv_74_ts.dll 64 bits |7.4|Oui|php7ts.dll 64 bits|

Sur Linux, les versions suivantes du pilote sont incluses :

|Fichier de pilote|Version PHP|Thread-safe ?|
|---------------|:---------------:|:----------------:|
|php_sqlsrv_72_nts.so<br />php_pdo_sqlsrv_72_nts.so|7.2|non |
|php_sqlsrv_72_ts.so <br />php_pdo_sqlsrv_72_ts.so |7.2|Oui|
|php_sqlsrv_73_nts.so<br />php_pdo_sqlsrv_73_nts.so|7.3|non |
|php_sqlsrv_73_ts.so <br />php_pdo_sqlsrv_73_ts.so |7.3|Oui|
|php_sqlsrv_74_nts.so<br />php_pdo_sqlsrv_74_nts.so|7.4|non |
|php_sqlsrv_74_ts.so <br />php_pdo_sqlsrv_74_ts.so |7.4|Oui|

**Microsoft Drivers 5.6 PHP pour SQL Server :**

Sur Windows, les versions suivantes du pilote sont incluses :

|Fichier de pilote|Version PHP|Thread-safe ?|Utiliser avec .dll PHP|
|---------------|:---------------:|:----------------:|---------------------|
|32-bit php_sqlsrv_71_nts.dll<br />32-bit php_pdo_sqlsrv_71_nts.dll|7.1|non |php7.dll 32 bits|
|32-bit php_sqlsrv_71_ts.dll <br />32-bit php_pdo_sqlsrv_71_ts.dll |7.1|Oui|php7ts.dll 32 bits|
|64-bit php_sqlsrv_71_nts.dll<br />64-bit php_pdo_sqlsrv_71_nts.dll|7.1|non |php7.dll 64 bits|
|64-bit php_sqlsrv_71_ts.dll <br />64-bit php_pdo_sqlsrv_71_ts.dll |7.1|Oui|php7ts.dll 64 bits|
|32-bit php_sqlsrv_72_nts.dll<br />32-bit php_pdo_sqlsrv_72_nts.dll|7.2|non |php7.dll 32 bits|
|32-bit php_sqlsrv_72_ts.dll <br />32-bit php_pdo_sqlsrv_72_ts.dll |7.2|Oui|php7ts.dll 32 bits|
|64-bit php_sqlsrv_72_nts.dll<br />64-bit php_pdo_sqlsrv_72_nts.dll|7.2|non |php7.dll 64 bits|
|64-bit php_sqlsrv_72_ts.dll <br />64-bit php_pdo_sqlsrv_72_ts.dll |7.2|Oui|php7ts.dll 64 bits|
|32-bit php_sqlsrv_73_nts.dll<br />32-bit php_pdo_sqlsrv_73_nts.dll|7.3|non |php7.dll 32 bits|
|32-bit php_sqlsrv_73_ts.dll <br />32-bit php_pdo_sqlsrv_73_ts.dll |7.3|Oui|php7ts.dll 32 bits|
|64-bit php_sqlsrv_73_nts.dll<br />64-bit php_pdo_sqlsrv_73_nts.dll|7.3|non |php7.dll 64 bits|
|64-bit php_sqlsrv_73_ts.dll <br />64-bit php_pdo_sqlsrv_73_ts.dll |7.3|Oui|php7ts.dll 64 bits|

Sur Linux, les versions suivantes du pilote sont incluses :

|Fichier de pilote|Version PHP|Thread-safe ?|
|---------------|:---------------:|:----------------:|
|php_sqlsrv_71_nts.so<br />php_pdo_sqlsrv_71_nts.so|7.1|non |
|php_sqlsrv_71_ts.so <br />php_pdo_sqlsrv_71_ts.so |7.1|Oui|
|php_sqlsrv_72_nts.so<br />php_pdo_sqlsrv_72_nts.so|7.2|non |
|php_sqlsrv_72_ts.so <br />php_pdo_sqlsrv_72_ts.so |7.2|Oui|
|php_sqlsrv_73_nts.so<br />php_pdo_sqlsrv_73_nts.so|7.3|non |
|php_sqlsrv_73_ts.so <br />php_pdo_sqlsrv_73_ts.so |7.3|Oui|

**Microsoft Drivers 5.3 for PHP for SQL Server :**

Sur Windows, les versions suivantes du pilote sont incluses :

|Fichier de pilote|Version PHP|Thread-safe ?|Utiliser avec .dll PHP|
|---------------|:---------------:|:----------------:|---------------------|
|32-bit php_sqlsrv_7_nts.dll <br />32-bit php_pdo_sqlsrv_7_nts.dll |7.0|non |php7.dll 32 bits|
|32-bit php_sqlsrv_7_ts.dll  <br />32-bit php_pdo_sqlsrv_7_ts.dll  |7.0|Oui|php7ts.dll 32 bits|
|64-bit php_sqlsrv_7_nts.dll <br />64-bit php_pdo_sqlsrv_7_nts.dll |7.0|non |php7.dll 64 bits|
|64-bit php_sqlsrv_7_ts.dll  <br />64-bit php_pdo_sqlsrv_7_ts.dll  |7.0|Oui|php7ts.dll 64 bits|
|32-bit php_sqlsrv_71_nts.dll<br />32-bit php_pdo_sqlsrv_71_nts.dll|7.1|non |php7.dll 32 bits|
|32-bit php_sqlsrv_71_ts.dll <br />32-bit php_pdo_sqlsrv_71_ts.dll |7.1|Oui|php7ts.dll 32 bits|
|64-bit php_sqlsrv_71_nts.dll<br />64-bit php_pdo_sqlsrv_71_nts.dll|7.1|non |php7.dll 64 bits|
|64-bit php_sqlsrv_71_ts.dll <br />64-bit php_pdo_sqlsrv_71_ts.dll |7.1|Oui|php7ts.dll 64 bits|
|32-bit php_sqlsrv_72_nts.dll<br />32-bit php_pdo_sqlsrv_72_nts.dll|7.2|non |php7.dll 32 bits|
|32-bit php_sqlsrv_72_ts.dll <br />32-bit php_pdo_sqlsrv_72_ts.dll |7.2|Oui|php7ts.dll 32 bits|
|64-bit php_sqlsrv_72_nts.dll<br />64-bit php_pdo_sqlsrv_72_nts.dll|7.2|non |php7.dll 64 bits|
|64-bit php_sqlsrv_72_ts.dll <br />64-bit php_pdo_sqlsrv_72_ts.dll |7.2|Oui|php7ts.dll 64 bits|

Sur Linux, les versions suivantes du pilote sont incluses :

|Fichier de pilote|Version PHP|Thread-safe ?|
|---------------|:---------------:|:----------------:|
|php_sqlsrv_7_nts.so <br />php_pdo_sqlsrv_7_nts.so |7.0|non |
|php_sqlsrv_7_ts.so  <br />php_pdo_sqlsrv_7_ts.so  |7.0|Oui|
|php_sqlsrv_71_nts.so<br />php_pdo_sqlsrv_71_nts.so|7.1|non |
|php_sqlsrv_71_ts.so <br />php_pdo_sqlsrv_71_ts.so |7.1|Oui|
|php_sqlsrv_72_nts.so<br />php_pdo_sqlsrv_72_nts.so|7.2|non |
|php_sqlsrv_72_ts.so <br />php_pdo_sqlsrv_72_ts.so |7.2|Oui|

**Microsoft Drivers 5.2 for PHP for SQL Server :**

Sur Windows, les versions suivantes du pilote sont incluses :

|Fichier de pilote|Version PHP|Thread-safe ?|Utiliser avec .dll PHP|
|---------------|:---------------:|:----------------:|---------------------|
|32-bit php_sqlsrv_7_nts.dll <br />32-bit php_pdo_sqlsrv_7_nts.dll |7.0|non |php7.dll 32 bits|
|32-bit php_sqlsrv_7_ts.dll  <br />32-bit php_pdo_sqlsrv_7_ts.dll  |7.0|Oui|php7ts.dll 32 bits|
|64-bit php_sqlsrv_7_nts.dll <br />64-bit php_pdo_sqlsrv_7_nts.dll |7.0|non |php7.dll 64 bits|
|64-bit php_sqlsrv_7_ts.dll  <br />64-bit php_pdo_sqlsrv_7_ts.dll  |7.0|Oui|php7ts.dll 64 bits|
|32-bit php_sqlsrv_71_nts.dll<br />32-bit php_pdo_sqlsrv_71_nts.dll|7.1|non |php7.dll 32 bits|
|32-bit php_sqlsrv_71_ts.dll <br />32-bit php_pdo_sqlsrv_71_ts.dll |7.1|Oui|php7ts.dll 32 bits|
|64-bit php_sqlsrv_71_nts.dll<br />64-bit php_pdo_sqlsrv_71_nts.dll|7.1|non |php7.dll 64 bits|
|64-bit php_sqlsrv_71_ts.dll <br />64-bit php_pdo_sqlsrv_71_ts.dll |7.1|Oui|php7ts.dll 64 bits|
|32-bit php_sqlsrv_72_nts.dll<br />32-bit php_pdo_sqlsrv_72_nts.dll|7.2|non |php7.dll 32 bits|
|32-bit php_sqlsrv_72_ts.dll <br />32-bit php_pdo_sqlsrv_72_ts.dll |7.2|Oui|php7ts.dll 32 bits|
|64-bit php_sqlsrv_72_nts.dll<br />64-bit php_pdo_sqlsrv_72_nts.dll|7.2|non |php7.dll 64 bits|
|64-bit php_sqlsrv_72_ts.dll <br />64-bit php_pdo_sqlsrv_72_ts.dll |7.2|Oui|php7ts.dll 64 bits|

Sur Linux, les versions suivantes du pilote sont incluses :

|Fichier de pilote|Version PHP|Thread-safe ?|
|---------------|:---------------:|:----------------:|
|php_sqlsrv_7_nts.so <br />php_pdo_sqlsrv_7_nts.so |7.0|non |
|php_sqlsrv_7_ts.so  <br />php_pdo_sqlsrv_7_ts.so  |7.0|Oui|
|php_sqlsrv_71_nts.so<br />php_pdo_sqlsrv_71_nts.so|7.1|non |
|php_sqlsrv_71_ts.so <br />php_pdo_sqlsrv_71_ts.so |7.1|Oui|
|php_sqlsrv_72_nts.so<br />php_pdo_sqlsrv_72_nts.so|7.2|non |
|php_sqlsrv_72_ts.so <br />php_pdo_sqlsrv_72_ts.so |7.2|Oui|

**Microsoft Drivers 4.3 for PHP for SQL Server :**

Sur Windows, les versions suivantes du pilote sont incluses :

|Fichier de pilote|Version PHP|Thread-safe ?|Utiliser avec .dll PHP|
|---------------|:---------------:|:----------------:|---------------------|
|32-bit php_sqlsrv_7_nts.dll <br />32-bit php_pdo_sqlsrv_7_nts.dll |7.0|non |php7.dll 32 bits|
|32-bit php_sqlsrv_7_ts.dll  <br />32-bit php_pdo_sqlsrv_7_ts.dll  |7.0|Oui|php7ts.dll 32 bits|
|64-bit php_sqlsrv_7_nts.dll <br />64-bit php_pdo_sqlsrv_7_nts.dll |7.0|non |php7.dll 64 bits|
|64-bit php_sqlsrv_7_ts.dll  <br />64-bit php_pdo_sqlsrv_7_ts.dll  |7.0|Oui|php7ts.dll 64 bits|
|32-bit php_sqlsrv_71_nts.dll<br />32-bit php_pdo_sqlsrv_71_nts.dll|7.1|non |php7.dll 32 bits|
|32-bit php_sqlsrv_71_ts.dll <br />32-bit php_pdo_sqlsrv_71_ts.dll |7.1|Oui|php7ts.dll 32 bits|
|64-bit php_sqlsrv_71_nts.dll<br />64-bit php_pdo_sqlsrv_71_nts.dll|7.1|non |php7.dll 64 bits|
|64-bit php_sqlsrv_71_ts.dll <br />64-bit php_pdo_sqlsrv_71_ts.dll |7.1|Oui|php7ts.dll 64 bits|

Sur Linux, les versions suivantes du pilote sont incluses :

|Fichier de pilote|Version PHP|Thread-safe ?|
|---------------|:---------------:|:----------------:|
|php_sqlsrv_7_nts.so <br />php_pdo_sqlsrv_7_nts.so |7.0|non |
|php_sqlsrv_7_ts.so  <br />php_pdo_sqlsrv_7_ts.so  |7.0|Oui|
|php_sqlsrv_71_nts.so<br />php_pdo_sqlsrv_71_nts.so|7.1|non |
|php_sqlsrv_71_ts.so <br />php_pdo_sqlsrv_71_ts.so |7.1|Oui|

**Microsoft Drivers 4.0 for PHP for SQL Server :**

Sur Windows, les versions suivantes du pilote sont incluses :

|Fichier de pilote|Version PHP|Thread-safe ?|Utiliser avec .dll PHP|
|---------------|:---------------:|:----------------:|---------------------|
|php_sqlsrv_7_nts_x86.dll<br />php_pdo_sqlsrv_7_nts_x86.dll|7.0|non|php7.dll 32 bits|
|php_sqlsrv_7_ts_x86.dll<br />php_pdo_sqlsrv_7_ts_x86.dll|7.0|Oui|php7ts.dll 32 bits|
|php_sqlsrv_7_nts_x64.dll<br />php_pdo_sqlsrv_7_nts_x64.dll|7.0|non|php7.dll 64 bits|
|php_sqlsrv_7_ts_x64.dll<br />php_pdo_sqlsrv_7_ts_x64.dll|7.0|Oui|php7ts.dll 64 bits|

Sur Linux, les versions suivantes du pilote sont incluses :

|Fichier de pilote|Version PHP|Thread-safe ?|
|---------------|:---------------:|:----------------:|
|php_sqlsrv_7_nts.so <br />php_pdo_sqlsrv_7_nts.so |7.0|non |
|php_sqlsrv_7_ts.so  <br />php_pdo_sqlsrv_7_ts.so  |7.0|Oui|

**Microsoft Drivers 3.2 for PHP for SQL Server :**

Sur Windows, les versions suivantes du pilote sont incluses :

|Fichier de pilote|Version PHP|Thread-safe ?|Utiliser avec .dll PHP|
|---------------|:---------------:|:----------------:|---------------------|
|php_sqlsrv_54_nts.dll<br />php_pdo_sqlsrv_54_nts.dll|5.4|non|php5.dll|
|php_sqlsrv_54_ts.dll<br />php_pdo_sqlsrv_54_ts.dll|5.4|Oui|php5ts.dll|
|php_sqlsrv_55_nts.dll<br />php_pdo_sqlsrv_55_nts.dll|5.5|non|php5.dll|
|php_sqlsrv_55_ts.dll<br />php_pdo_sqlsrv_55_ts.dll|5.5|Oui|php5ts.dll|
|php_sqlsrv_56_nts.dll<br />php_pdo_sqlsrv_56_nts.dll|5.6|non|php5.dll|
|php_sqlsrv_56_ts.dll<br />php_pdo_sqlsrv_56_ts.dll|5.6|Oui|php5ts.dll|

## <a name="see-also"></a>Voir aussi

- [Bien démarrer avec les pilotes Microsoft pour PHP pour SQL Server](getting-started-with-the-php-sql-driver.md)
- [Guide de programmation pour les pilotes Microsoft pour PHP pour SQL Server](programming-guide-for-php-sql-driver.md)
- [Informations de référence sur l’API du pilote SQLSRV](sqlsrv-driver-api-reference.md)
- [Référence API du pilote PDO_SQLSRV](pdo-sqlsrv-driver-reference.md)
