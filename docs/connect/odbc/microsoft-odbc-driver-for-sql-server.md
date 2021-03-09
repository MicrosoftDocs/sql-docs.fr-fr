---
title: Pilote Microsoft ODBC pour SQL Server
description: Microsoft ODBC Driver for SQL Server fournit la connectivité à SQL Server et Azure SQL Database via les API ODBC standard.
ms.custom: ''
ms.date: 05/06/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 9f2ae91b-06af-4c9a-9d24-062df7bc4662
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b107155d9672b2a1d7b905a387e48373905dab55
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/08/2021
ms.locfileid: "102464180"
---
# <a name="microsoft-odbc-driver-for-sql-server"></a>Pilote Microsoft ODBC pour SQL Server

[!INCLUDE[ODBC_Current_Version](../../includes/odbc-latest-release.md)]

[!INCLUDE[Driver_ODBC_Download](../../includes/driver_odbc_download.md)]

ODBC est l’API d’accès aux données native principale pour les applications écrites en C et en C++ pour SQL Server. Il existe un pilote ODBC pour la plupart des sources de données. Les autres langages qui peuvent utiliser ODBC incluent COBOL, Perl, PHP et Python. ODBC est largement utilisé dans les scénarios d’intégration des données.

Le pilote ODBC est fourni avec des outils comme [**sqlcmd**](../../tools/sqlcmd-utility.md) et [**bcp**](../../tools/bcp-utility.md). L’utilitaire **sqlcmd** permet d’exécuter des instructions Transact-SQL, des procédures système et des scripts SQL. L’utilitaire **bcp** copie en bloc des données entre une instance de Microsoft SQL Server et un fichier de données dans le format choisi. Vous pouvez utiliser **bcp** pour importer un grand nombre de nouvelles lignes dans des tables SQL Server ou pour exporter des données de tables dans des fichiers de données.  

## <a name="code-example-in-c"></a>Exemple de code en C++

L’exemple C++ suivant montre comment utiliser les API ODBC pour se connecter et accéder à une base de données :

- [Exemple de code C++, utilisant ODBC](../../odbc/reference/sample-odbc-program.md)

## <a name="download"></a>Téléchargement

- ![Télécharger-FlècheBas dans un cercle](../../ssms/media/download-icon.png)[Pour télécharger le pilote ODBC](download-odbc-driver-for-sql-server.md)

## <a name="documentation"></a>Documentation

### <a name="features"></a>Fonctionnalités

- [Fournisseurs de magasins de clés personnalisés](../../connect/odbc/custom-keystore-providers.md)
- [Classification des données](../../connect/odbc/data-classification.md)
- [Attributs et mots clés de chaîne de connexion et DSN](dsn-connection-string-attribute.md)
- [SQL Server Native Client](../../relational-databases/native-client/features/sql-server-native-client-features.md) (les fonctionnalités disponibles s’appliquent également, sans OLEDB, à ODBC Driver for SQL Server)
- [Utilisation d’Always Encrypted](../../connect/odbc/using-always-encrypted-with-the-odbc-driver.md)
- [Utilisation d’Azure Active Directory](../../connect/odbc/using-azure-active-directory.md)
- [Utilisation de la résolution d’adresses IP réseau transparente](../../connect/odbc/using-transparent-network-ip-resolution.md)
- [Utilisation de transactions XA](../../connect/odbc/use-xa-with-dtc.md)

### <a name="linux-and-macos"></a>Linux et macOS

- [Installation du pilote sur Linux](../../connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server.md)
- [Installation du pilote sur macOS](../../connect/odbc/linux-mac/install-microsoft-odbc-driver-sql-server-macos.md)
- [Connexion à SQL Server](../../connect/odbc/linux-mac/connection-string-keywords-and-data-source-names-dsns.md)
- [Connexion avec **bcp**](../../connect/odbc/linux-mac/connecting-with-bcp.md)
- [Connexion avec **sqlcmd**](../../connect/odbc/linux-mac/connecting-with-sqlcmd.md)
- [Traçage de l’accès aux données](../../connect/odbc/linux-mac/data-access-tracing-with-the-odbc-driver-on-linux.md)
- [Questions fréquentes (FAQ)](../../connect/odbc/linux-mac/frequently-asked-questions-faq-for-odbc-linux.yml)
- [Installation du Gestionnaire de pilotes](../../connect/odbc/linux-mac/installing-the-driver-manager.md)
- [Problèmes connus](../../connect/odbc/linux-mac/known-issues-in-this-version-of-the-driver.md)
- [Instructions de programmation](../../connect/odbc/linux-mac/programming-guidelines.md)
- [Notes de publication](../../connect/odbc/linux-mac/release-notes-odbc-sql-server-linux-mac.md)
- [Notes de publication (mssql-tools)](../../connect/odbc/linux-mac/release-notes-tools.md)
- [Prise en charge de la haute disponibilité et de la récupération d’urgence](../../connect/odbc/linux-mac/odbc-driver-on-linux-support-for-high-availability-disaster-recovery.md)
- [Utilisation de l’authentification intégrée (Kerberos)](../../connect/odbc/linux-mac/using-integrated-authentication.md)

### <a name="windows"></a>Windows

- [Exemple d’exécution asynchrone (méthode de notification)](../../connect/odbc/windows/asynchronous-execution-notification-method-sample.md)
- [Résilience de connexion du pilote ODBC Windows](../../connect/odbc/windows/connection-resiliency-in-the-windows-odbc-driver.md)
- [Regroupement de connexions prenant en charge les pilotes](../../connect/odbc/windows/driver-aware-connection-pooling-in-the-odbc-driver-for-sql-server.md)
- [Fonctionnalités et changements de comportement](../../connect/odbc/windows/features-of-the-microsoft-odbc-driver-for-sql-server-on-windows.md)
- [Notes de publication relatives à ODBC pour SQL Server sur Windows](windows/release-notes-odbc-sql-server-windows.md)
- [Configuration système requise, installation et fichiers de pilote](../../connect/odbc/windows/system-requirements-installation-and-driver-files.md)

## <a name="community"></a>Communauté

- [Blog sur les pilotes SQL Server](https://techcommunity.microsoft.com/t5/SQL-Server/bg-p/SQLServer/label-name/SQLServerDrivers)  
- [Forum sur l’accès aux données SQL Server](https://social.technet.microsoft.com/Forums/en/sqldataaccess/threads)  
