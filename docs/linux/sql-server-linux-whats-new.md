---
title: Nouveautés de SQL Server 2017 sur Linux
description: Dans cet article, découvrez les principaux services et fonctionnalités disponibles pour SQL Server 2017 s’exécutant sur Linux.
author: VanMSFT
ms.author: vanto
ms.date: 04/10/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: c863cb129f1ba40c2bb73fff51adb859a8a7bade
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/08/2021
ms.locfileid: "102464650"
---
# <a name="whats-new-for-sql-server-2017-on-linux"></a>Nouveautés de SQL Server 2017 sur Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Cet article décrit les principaux services et fonctionnalités disponibles pour SQL Server 2017 s’exécutant sur Linux.

> [!NOTE]
> En plus des fonctionnalités du présent article, des mises à jour cumulatives sont mises en production à intervalles réguliers. Ces mises à jour cumulatives fournissent de nombreuses améliorations et correctifs. Pour plus d’informations sur la dernière mise en production CU, consultez [https://aka.ms/sql2017cu](https://aka.ms/sql2017cu). Pour connaitre les téléchargements de packages et les problèmes connus, consultez les [Notes de publication](sql-server-linux-release-notes.md).

## <a name="ubuntu-1804-supported"></a>Ubuntu 18.04 pris en charge

À compter de SQL Server 2017 CU20, Ubuntu 18.04 est pris en charge. Consultez notre guide de démarrage rapide [Installer SQL Server et créer une base de données sur Ubuntu](quickstart-install-connect-ubuntu.md).

## <a name="rhel-8-supported"></a>RHEL 8 pris en charge

À compter de SQL Server 2017 CU20, RHEL 8 est pris en charge. Consultez notre guide de démarrage rapide [Installer SQL Server et créer une base de données sur Red Hat](quickstart-install-connect-red-hat.md).

## <a name="sql-server-database-engine"></a>Moteur de base de données SQL Server

- Activation des fonctionnalités de Moteur de base de données de SQL Server principales.
- Support des chemins d’accès Linux natifs.
- Support IPV6.
- Support des fichiers de base de données sur NFS.
- Activation du chiffrement du [Protocole TLS](sql-server-linux-encrypted-connections.md) (Transport Layer Security).
- Activation de l’[Authentification Active Directory](sql-server-linux-active-directory-authentication.md).
- [Fonctionnalités de groupes de disponibilité](sql-server-linux-availability-group-overview.md) pour la haute disponibilité.
- Support de la [Recherche en texte intégral](sql-server-linux-setup-full-text-search.md).

## <a name="sql-server-agent"></a>SQL Server Agent

- Activation du support [SQL Server Agent](sql-server-linux-setup-sql-agent.md) pour les tâches suivantes :
  - [Tâches Transact-SQL](sql-server-linux-run-sql-server-agent-job.md)
  - [Courrier de base de messages](sql-server-linux-db-mail-sql-agent.md)
  - [Copie des journaux de transaction](sql-server-linux-use-log-shipping.md)

## <a name="sql-server-integration-services-ssis"></a>SQL Server Integration Services (SSIS)

- Capacité d’exécuter des packages SSIS sur Linux. Pour plus d’informations, consultez [Configurer SQL Server Integration Services sur Linux avec ssis-conf](sql-server-linux-configure-ssis.md).

## <a name="other-improvements"></a>Autres améliorations

- Outil de configuration de ligne de commande, [mssql-conf](sql-server-linux-configure-mssql-conf.md).
- Support de l’installation sans assistance avec les [variables d’environnement](sql-server-linux-configure-environment-variables.md).
- [Extension mssql-server de Visual Studio Code](../tools/visual-studio-code/sql-server-develop-use-vscode.md) multiplateforme.
- Générateur de script multiplateforme, [mssql-scripter](https://github.com/Microsoft/sql-xplat-cli/blob/dev/doc/usage_guide.md).
- Analyse des vues de gestion dynamique (DMV) multiplateforme, [outil DBFS](https://github.com/Microsoft/dbfs).

## <a name="next-steps"></a>Étapes suivantes

Pour installer SQL Server sur Linux, utilisez l’un des didacticiels suivants :

- [Installer sur Red Hat Enterprise Linux](quickstart-install-connect-red-hat.md)
- [Installer sur SUSE Linux Enterprise Server](quickstart-install-connect-suse.md)
- [Installer sur Ubuntu](quickstart-install-connect-ubuntu.md)
- [Exécuter sur Docker](quickstart-install-connect-docker.md)
- [Approvisionner une machine virtuelle SQL dans Azure](/azure/virtual-machines/linux/sql/provision-sql-server-linux-virtual-machine?toc=/sql/toc/toc.json)

Pour obtenir des réponses aux questions fréquemment posées, consultez la [FAQ de SQL Server sur Linux](sql-server-linux-faq.yml). Pour voir d’autres améliorations introduites dans SQL Server 2017, consultez [Nouveautés de SQL Server 2017](../sql-server/what-s-new-in-sql-server-2017.md).

[!INCLUDE[get-help-options](../includes/paragraph-content/get-help-options.md)]
