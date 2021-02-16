---
title: Extraire, transformer et charger des données sur Linux avec SSIS
description: Découvrez comment exécuter des packages SQL Server Integration Services (SSIS) sur Linux. Découvrez également où trouver plus d’informations sur les fonctionnalités SSIS.
author: lrtoyou1223
ms.author: lle
ms.reviewer: maghan
ms.date: 01/09/2018
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: e21864da00069c6a41138a8506c98ab49e07bf3b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339121"
---
# <a name="extract-transform-and-load-data-on-linux-with-ssis"></a>Extraire, transformer et charger des données sur Linux avec SSIS

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Cet article explique comment exécuter des packages SQL Server Integration Services (SSIS) sur Linux. SSIS résout des problèmes complexes d'intégration de données en extrayant des données de plusieurs sources et formats, en transformant et en nettoyant les données, puis en les chargeant vers différentes destinations. 

Les packages SSIS fonctionnant sur Linux peuvent se connecter à Microsoft SQL Server sur Windows en local ou dans le cloud, sur Linux ou dans Docker. Ils peuvent également se connecter à Azure SQL Database, à Azure Synapse Analytics, à des sources de données ODBC, à des fichiers plats et à d'autres sources de données, notamment des sources ADO.NET, des fichiers XML et des services OData.

Pour plus d’informations sur les capacités de SSIS, consultez [SQL Server Integration Services](../integration-services/sql-server-integration-services.md).

## <a name="prerequisites"></a>Prérequis

Pour exécuter des packages SSIS sur un ordinateur Linux, vous devez d'abord installer SQL Server Integration Services. SSIS n'est pas inclus dans l'installation de SQL Server pour les ordinateurs Linux. Pour obtenir des instructions d'installation, voir [Installer SQL Server Integration Services](sql-server-linux-setup-ssis.md).

Vous devez également disposer d'un ordinateur Windows pour créer et gérer les packages. Les outils de conception et de gestion SSIS sont des applications Windows actuellement non disponibles pour les ordinateurs Linux. 

## <a name="run-an-ssis-package"></a>Exécuter un package SSIS

Pour exécuter un paquet SSIS sur un ordinateur Linux, procédez comme suit :

1.  Copiez le package SSIS sur l'ordinateur Linux.
2.  Exécutez la commande suivante :
    ```
    $ dtexec /F \<package name \> /DE <protection password>
    ```

## <a name="run-an-encrypted-password-protected-package"></a>Exécutez un package chiffré (protégé par un mot de passe)
Il existe trois façons d'exécuter un package SSIS chiffré avec un mot de passe :

1.  Définir la valeur de la variable d'environnement `SSIS_PACKAGE_DECRYPT`, comme indiqué dans l'exemple suivant :

    ```
    SSIS_PACKAGE_DECRYPT=test /opt/ssis/bin/dtexec /f package.dtsx
    ```

2.  Spécifier l'option `/de[crypt]` pour entrer le mot de passe de manière interactive, comme indiqué dans l'exemple suivant :

    ```
    /opt/ssis/bin/dtexec /f package.dtsx /de
    
    Enter decryption password:
    ```

3.  Spécifier l'option `/de` pour fournir le mot de passe dans la ligne de commande, comme indiqué dans l'exemple suivant. Cette méthode n'est pas recommandée car elle stocke le mot de passe de déchiffrement avec la commande dans l'historique des commandes.

    ```
    opt/ssis/bin/dtexec /f package.dtsx /de test
    
    Warning: Using /De[crypt] <password> may store decryption password in command history.
    
    You can use /De[crypt] instead to enter interactive mode,
    or use environment variable SSIS_PACKAGE_DECRYPT to set decryption password.
    ```

## <a name="design-packages"></a>Concevoir des packages

**Se connecter à des sources de données ODBC**. Avec SSIS sur Linux CTP 2.1 Refresh et versions ultérieures, les packages SSIS peuvent utiliser les connexions ODBC sur Linux. Cette fonctionnalité a été testée avec les pilotes SQL Server et MySQL ODBC, mais elle devrait également fonctionner avec tout pilote ODBC Unicode conforme au standard ODBC. Au moment de la conception, vous pouvez fournir un DSN ou une chaîne de connexion pour vous connecter aux données ODBC ; vous pouvez également utiliser l'authentification Windows. Pour plus d'informations, consultez le [billet de blog annonçant le support d’ODBC sur Linux](https://blogs.msdn.microsoft.com/ssis/2017/06/16/odbc-is-supported-in-ssis-on-linux-ssis-helsinki-ctp2-1-refresh/).

**Chemins d'accès**. Fournissez des chemins de style Windows dans vos packages SSIS. SSIS sur Linux ne prend pas en charge les chemins de style Linux, mais mappe les chemins de style Windows aux chemins de style Linux lors de l'exécution. Ensuite, par exemple, SSIS sur Linux mappe le chemin de style Windows `C:\test` au chemin de style Linux `/test`.

## <a name="deploy-packages"></a>Déployer des packages
Vous ne pouvez stocker des packages dans le système de fichiers sur Linux que dans cette version. La base de données du catalogue SSIS et le service SSIS hérité ne sont pas disponibles sur Linux pour le déploiement et le stockage des packages.

## <a name="schedule-packages"></a>Planifier les packages
Vous pouvez utiliser les outils de planification du système Linux tels que `cron` pour planifier les packages. Vous ne pouvez pas utiliser SQL Agent sur Linux pour planifier l'exécution des packages dans cette version. Pour plus d’informations, voir [Planifier des packages SSIS sur Linux avec cron](sql-server-linux-schedule-ssis-packages.md).

## <a name="limitations-and-known-issues"></a>Limitations et problèmes connus

Pour des informations détaillées sur les limitations et les problèmes connus de SSIS sur Linux, voir [Limitations et problèmes connus pour SSIS sur Linux](sql-server-linux-ssis-known-issues.md).

## <a name="more-info-about-ssis-on-linux"></a>En savoir plus sur SSIS sur Linux

Pour plus d’informations concernant SSIS sur Linux, consultez les billets de blog suivants :

-   [SSIS sur Linux est disponible dans SQL Server CTP2.1](https://blogs.msdn.microsoft.com/ssis/2017/05/17/ssis-helsinki-is-available-in-sql-server-vnext-ctp2-1/)
-   [ODBC est pris en charge dans SSIS sur Linux (SQL Server CTP 2.1 refresh)](https://blogs.msdn.microsoft.com/ssis/2017/06/16/odbc-is-supported-in-ssis-on-linux-ssis-helsinki-ctp2-1-refresh/)

## <a name="more-info-about-ssis"></a>En savoir plus sur SSIS

Microsoft SQL Server Integration Services (SSIS) est une plateforme conçue pour créer des solutions d’intégration de données à hautes performances, notamment des packages d’extraction, de transformation et de chargement (ETL) pour l’entreposage de données. Pour plus d’informations, consultez [SQL Server Integration Services](../integration-services/sql-server-integration-services.md).

SSIS inclut les fonctionnalités suivantes :
- Des outils graphiques et des assistants pour construire et déboguer des packages sur Windows
- Une variété de tâches pour exécuter des fonctions de workflow telles que les opérations FTP, l'exécution d'instructions SQL et l'envoi d’e-mails
- Diverses sources de données et de destinations pour l'extraction et le chargement des données
- Une variété de transformations pour le nettoyage, l'agrégation, la fusion et la copie des données
- Des interfaces de programmation d'applications (API) pour étendre le SSIS avec vos propres scripts et composants personnalisés

Pour bien démarrer avec SSIS, téléchargez la dernière version de [SQL Server Data Tools (SSDT)](../integration-services/ssis-how-to-create-an-etl-package.md).

Pour en savoir plus sur SSIS, consultez les articles suivants :
- [En savoir plus sur SQL Server Integration Services](../integration-services/sql-server-integration-services.md)
- [Outils de gestion et de développement SQL Server Integration Services (SSIS)](../integration-services/integration-services-ssis-development-and-management-tools.md)
- [Tutoriels SQL Server Integration Services](../integration-services/integration-services-tutorials.md)

## <a name="related-content-about-ssis-on-linux"></a>Contenu connexe relatif à SSIS sur Linux
-   [Installer SQL Server Integration Services (SSIS) sur Linux](sql-server-linux-setup-ssis.md)
-   [Configurer SQL Server Integration Services sur Linux avec ssis-conf](sql-server-linux-configure-ssis.md)
-   [Limitations et problèmes connus pour SSIS sur Linux](sql-server-linux-ssis-known-issues.md)
-   [Planifier l’exécution du package SQL Server Integration Services sur Linux avec cron](sql-server-linux-schedule-ssis-packages.md)