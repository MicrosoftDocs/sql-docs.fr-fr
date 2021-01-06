---
title: Nouveautés de SQL Server 2019 sur Linux
description: Cet article présente les nouveautés de SQL Server 2019 sur Linux.
author: VanMSFT
ms.author: vanto
ms.date: 03/12/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: 9ec5d4f34d299876614383c79fb79f2c2be9114d
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97641269"
---
# <a name="whats-new-for-sql-server-2019-on-linux"></a>Nouveautés de SQL Server 2019 sur Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Cet article décrit les principaux services et fonctionnalités disponibles pour SQL Server 2019 s’exécutant sur Linux. Pour connaitre les téléchargements de packages et les problèmes connus, consultez les [Notes de publication](sql-server-linux-release-notes-2019.md).

## <a name="ubuntu-1804-supported"></a>Ubuntu 18.04 pris en charge

À compter de SQL Server 2019 CU3, Ubuntu 18.04 est pris en charge. Consultez notre guide de démarrage rapide [Installer SQL Server et créer une base de données sur Ubuntu](quickstart-install-connect-ubuntu.md?view=sql-server-linux-ver15&preserve-view=true).

## <a name="rhel-8-supported"></a>RHEL 8 pris en charge

À compter de SQL Server 2019 CU1, RHEL 8 est pris en charge. Consultez notre guide de démarrage rapide [Installer SQL Server et créer une base de données sur Red Hat](quickstart-install-connect-red-hat.md?view=sql-server-linux-ver15&preserve-view=true).

## <a name="updates"></a>Mises à jour

Les mises à jour ont été effectuées dans SQL Server 2019 sur Linux :

| Nouvelle fonctionnalité ou mise à jour | Détails |
|:-----|:-----|
|Prise en charge de la réplication |[Réplication SQL Server sur Linux](sql-server-linux-replication.md)
|Prise en charge de MSDTC (Microsoft Distributed Transaction Coordinator) |[Comment configurer MSDTC sur Linux](sql-server-linux-configure-msdtc.md) |
|Prise en charge d’OpenLDAP pour les fournisseurs AD tiers |[Tutoriel : Utiliser l’authentification Active Directory avec SQL Server sur Linux](sql-server-linux-active-directory-authentication.md) |
|Machine learning sur Linux |[Configurer le Machine Learning sur Linux](sql-server-linux-setup-machine-learning.md) |
|Améliorations `tempdb` | Par défaut, une nouvelle installation de SQL Server sur Linux crée plusieurs fichiers de données `tempdb` en fonction du nombre de cœurs logiques (avec jusqu’à 8 fichiers de données). Cela ne s’applique pas aux mises à niveau de versions mineures ou majeures sur place. Chaque fichier `tempdb` fait 8 Mo avec une croissance automatique de 64 Mo. Ce comportement est similaire à l’installation de SQL Server par défaut sur Windows. |
| PolyBase sur Linux | [Installer PolyBase](../relational-databases/polybase/polybase-linux-setup.md) sur Linux pour les connecteurs non-Hadoop.<br/><br/>[Mappage de type PolyBase](../relational-databases/polybase/polybase-type-mapping.md). |
| Prise en charge de la capture des changements de données (CDC) | La capture des changements de données (CDC) est désormais prise en charge sur Linux pour SQL Server 2019. |
| Registre de conteneurs Microsoft | Le [registre de conteneurs Microsoft](https://azure.microsoft.com/blog/microsoft-syndicates-container-catalog/) remplace désormais Docker Hub pour les nouvelles images conteneur Microsoft officielles, notamment [!INCLUDE[sql-server-2019](../includes/sssqlv15-md.md)]. |
| Conteneurs non racines | [!INCLUDE[sql-server-2019](../includes/sssqlv15-md.md)] introduit la possibilité de créer des conteneurs plus sûrs en démarrant le processus [!INCLUDE[sql-server](../includes/ssnoversion-md.md)] en tant qu’utilisateur non racine par défaut. Pour plus d’informations, consultez [Générer et exécuter des conteneurs SQL Server en tant qu’utilisateur non racine](./sql-server-linux-docker-container-security.md#buildnonrootcontainer). |

## <a name="next-steps"></a>Étapes suivantes

Pour installer SQL Server sur Linux, utilisez l’un des didacticiels suivants :

- [Installer sur Red Hat Enterprise Linux](quickstart-install-connect-red-hat.md?view=sql-server-linux-ver15&preserve-view=true)
- [Installer sur SUSE Linux Enterprise Server](quickstart-install-connect-suse.md?view=sql-server-linux-ver15&preserve-view=true)
- [Installer sur Ubuntu](quickstart-install-connect-ubuntu.md?view=sql-server-linux-ver15&preserve-view=true)
- [Exécuter sur Docker](quickstart-install-connect-docker.md?view=sql-server-linux-ver15&preserve-view=true)
- [Approvisionner une machine virtuelle SQL dans Azure](/azure/virtual-machines/linux/sql/provision-sql-server-linux-virtual-machine?toc=/sql/toc/toc.json)

Pour obtenir des réponses aux questions fréquemment posées, consultez la [FAQ de SQL Server sur Linux](sql-server-linux-faq.md). Pour voir d’autres améliorations introduites dans SQL Server 2019, consultez [Nouveautés de SQL Server 2019](../sql-server/what-s-new-in-sql-server-ver15.md?view=sql-server-ver15&preserve-view=true).

[!INCLUDE[get-help-options](../includes/paragraph-content/get-help-options.md)]