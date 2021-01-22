---
title: Notes de publication pour SQL Server 2019 sur Linux
description: Cet article contient les notes de publication et fonctionnalités prises en charge pour SQL Server 2019 s’exécutant sur Linux. Les notes de publication sont incluses dans la mise en production la plus récente et dans plusieurs mises en production précédentes.
author: VanMSFT
ms.author: vanto
ms.date: 10/01/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: 434345c505eaab389063fbc5b2c7fffc51708634
ms.sourcegitcommit: af64e2b8d498af26b973e86db5c00f8d72991295
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/14/2021
ms.locfileid: "98192996"
---
# <a name="release-notes-for-sql-server-2019-on-linux"></a>Notes de publication pour SQL Server 2019 sur Linux

[!INCLUDE[tsql-appliesto-ssver15-xxxx-xxxx-xxx-linuxonly.md](../includes/tsql-appliesto-ssver15-xxxx-xxxx-xxx-linuxonly.md)]

Les notes de publication suivantes s’appliquent à SQL Server 2019 s’exécutant sur Linux. Cet article est divisé en sections pour chaque mise en production. Chaque version a un lien vers un article de support décrivant les modifications CU, ainsi que des liens de téléchargement des packages Linux.

> [!TIP]
> Pour en savoir plus sur les nouvelles fonctionnalités de Linux dans SQL Server 2019, consultez [Nouveautés de SQL Server 2019](../sql-server/what-s-new-in-sql-server-ver15.md?view=sql-server-ver15&preserve-view=true#sql-server-on-linux).

[!INCLUDE [linux-supported-platfoms-2019](../includes/linux-supported-platfoms-2019.md)]

## <a name="tools"></a>Outils

La plupart des outils client existants qui ciblent SQL Server peuvent cibler en toute transparence SQL Server sur Linux. Certains outils peuvent avoir une exigence de version spécifique pour fonctionner correctement avec Linux. Pour obtenir la liste complète des outils SQL Server, consultez [Outils et utilitaires SQL pour SQL Server](../tools/overview-sql-tools.md).

## <a name="release-history"></a>Historique des mises en production

Le tableau suivant liste l’historique de publication des versions de SQL Server 2019.

| Libérer                   | Version       | Date de publication |
|---------------------------|---------------|--------------|
| [CU8-GDR](#cu8-GDR)               | 15.0.4083.2  | 2021-01-12   |
| [CU8](#cu8)               | 15.0.4073.23  | 2020-10-07   |
| [CU7 (Supprimé)](https://support.microsoft.com/help/4570012)     | 15.0.4063.15  | 2020-09-02   |
| [CU6](#cu6)               | 15.0.4053.23  | 2020-08-04   |
| [CU5](#cu5)               | 15.0.4043.16  | 2020-06-22   |
| [CU4](#cu4)               | 15.0.4033.1   | 31-03-2020   |
| [CU3](#cu3)               | 15.0.4023.6   | 2020-03-12   |
| [CU2](#cu2)               | 15.0.4013.40  | 2020-02-13   |
| [CU1](#cu1)               | 15.0.4003.23  | 2020-01-07   |
| [GA](#ga)                 | 15.0.2000.5   | 04-11-2019   |
| [Version Release Candidate](#rc)  | 15.0.1900.25  | 21-08-2019   |

## <a name="how-to-install-updates"></a><a id="cuinstall"></a> Comment installer les mises à jour

Si vous avez configuré le dépôt CU (mssql-server-2019), vous obtenez la dernière unité de capacité des packages SQL Server quand vous effectuez de nouvelles installations. Si vous avez besoin d’images conteneur Docker, consultez les images officielles pour [Microsoft SQL Server sur Linux pour Docker Engine](https://hub.docker.com/r/microsoft/mssql-server/). Pour plus d’informations sur la configuration du référentiel, consultez [Configurer les référentiels pour SQL Server sur Linux](sql-server-linux-change-repo.md).

Si vous mettez à jour des packages SQL Server existants, exécutez la commande de mise à jour appropriée pour chaque package afin d’obtenir la dernière CU. Pour obtenir des instructions de mise à jour spécifiques pour chaque package, consultez les guides d’installation suivants :

- [Installer un package SQL Server](sql-server-linux-setup.md#upgrade)
- [Installer un package de recherche en texte intégral](sql-server-linux-setup-full-text-search.md)
- [Installer SQL Server Integration Services](sql-server-linux-setup-ssis.md)
- [Installer SQL Server 2019 Machine Learning Services pour prendre en charge R et Python sur Linux](sql-server-linux-setup-machine-learning.md)
- [Installer le package PolyBase](../relational-databases/polybase/polybase-linux-setup.md)
- [Activer SQL Server Agent](sql-server-linux-setup-sql-agent.md)

## <a name="cu8-gdr-january-2021"></a><a id="cu8-GDR"></a> CU8-GDR (janvier 2021)

Il s’agit de la mise en production de la mise à jour cumulative 8-GDR (CU8-GDR) de SQL Server 2019 (15.x). La version du moteur de base de données SQL Server pour cette mise en production est 15.0.4083.2. Pour plus d’informations sur les correctifs et améliorations, consultez <https://support.microsoft.com/help/4577194>.

### <a name="package-details"></a>Détails du package

Pour les installations du package manuelles ou hors connexion, vous pouvez télécharger les packages RPM et Debian avec les informations figurant dans la table suivante :

> [!NOTE]
> À compter de CU1, les liens d’installation de package hors connexion pour Red Hat pointent vers les packages RHEL 8. Si vous recherchez les packages RHEL 7, reportez-vous au chemin de téléchargement <https://packages.microsoft.com/rhel/7/mssql-server-2019/>
>
> **Ubuntu 18.04** est maintenant pris en charge sur SQL Server 2019 à compter de CU3. Les liens d’installation de package hors connexion pour Ubuntu pointent vers les packages Ubuntu 18.04. Si vous recherchez les packages Ubuntu 16.04, reportez-vous au chemin de téléchargement <https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/>

| Package | Version du package | Téléchargements |
|-----|-----|-----|
| Package Red Hat RPM | 15.0.4083.2-15 | [Package RPM du moteur](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-15.0.4083.2-15.x86_64.rpm)</br>[Package RPM haute disponibilité](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-ha-15.0.4083.2-15.x86_64.rpm)</br>[Package RPM de recherche en texte intégral](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-fts-15.0.4083.2-15.x86_64.rpm)</br>[Package RPM d’extensibilité](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-extensibility-15.0.4083.2-15.x86_64.rpm)</br>[Package RPM d’extensibilité Java](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-extensibility-java-15.0.4083.2-15.x86_64.rpm)</br>[Package RPM PolyBase](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-polybase-15.0.4083.2-15.x86_64.rpm)|
| Package SLES RPM | 15.0.4083.2-15 | [Package RPM du moteur mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-15.0.4083.2-15.x86_64.rpm)</br>[Package RPM haute disponibilité](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-ha-15.0.4083.2-15.x86_64.rpm)</br>[Package RPM de recherche en texte intégral](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-fts-15.0.4083.2-15.x86_64.rpm)</br>[Package RPM d’extensibilité](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-extensibility-15.0.4083.2-15.x86_64.rpm)</br>[Package RPM d’extensibilité Java](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-extensibility-java-15.0.4083.2-15.x86_64.rpm)</br>[Package RPM PolyBase](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-polybase-15.0.4083.2-15.x86_64.rpm)|
| Package Ubuntu 18.04 Debian | 15.0.4083.2-15 | [Package Debian du moteur](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server/mssql-server_15.0.4083.2-15_amd64.deb)</br>[Package Debian haute disponibilité](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-ha/mssql-server-ha_15.0.4083.2-15_amd64.deb)</br>[Package Debian de recherche en texte intégral](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-fts/mssql-server-fts_15.0.4083.2-15_amd64.deb)</br>[Package Debian d’extensibilité](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-extensibility/mssql-server-extensibility_15.0.4083.2-15_amd64.deb)</br>[Package Debian d’extensibilité Java](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-extensibility-java/mssql-server-extensibility-java_15.0.4083.2-15_amd64.deb)</br>[Package RPM PolyBase](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-polybase/mssql-server-polybase_15.0.4083.2-15_amd64.deb)|

## <a name="cu8-september-2020"></a><a id="cu8"></a> CU8 (septembre 2020)

Il s’agit de la version de la mise à jour cumulative 8 (CU8) de SQL Server 2019 (15.x). La version du moteur de base de données SQL Server pour cette version est 15.0.4073.23. Pour plus d’informations sur les correctifs et améliorations, consultez <https://support.microsoft.com/help/4577194>.

### <a name="package-details"></a>Détails du package

Pour les installations du package manuelles ou hors connexion, vous pouvez télécharger les packages RPM et Debian avec les informations figurant dans la table suivante :

> [!NOTE]
> À compter de CU1, les liens d’installation de package hors connexion pour Red Hat pointent vers les packages RHEL 8. Si vous recherchez les packages RHEL 7, reportez-vous au chemin de téléchargement <https://packages.microsoft.com/rhel/7/mssql-server-2019/>
>
> **Ubuntu 18.04** est maintenant pris en charge sur SQL Server 2019 à compter de CU3. Les liens d’installation de package hors connexion pour Ubuntu pointent vers les packages Ubuntu 18.04. Si vous recherchez les packages Ubuntu 16.04, reportez-vous au chemin de téléchargement <https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/>

| Package | Version du package | Téléchargements |
|-----|-----|-----|
| Package Red Hat RPM | 15.0.4073.23-4 | [Package RPM du moteur](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-15.0.4073.23-4.x86_64.rpm)</br>[Package RPM haute disponibilité](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-ha-15.0.4073.23-4.x86_64.rpm)</br>[Package RPM de recherche en texte intégral](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-fts-15.0.4073.23-4.x86_64.rpm)</br>[Package RPM d’extensibilité](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-extensibility-15.0.4073.23-4.x86_64.rpm)</br>[Package RPM d’extensibilité Java](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-extensibility-java-15.0.4073.23-4.x86_64.rpm)</br>[Package RPM PolyBase](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-polybase-15.0.4073.23-4.x86_64.rpm)|
| Package SLES RPM | 15.0.4073.23-4 | [Package RPM du moteur mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-15.0.4073.23-4.x86_64.rpm)</br>[Package RPM haute disponibilité](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-ha-15.0.4073.23-4.x86_64.rpm)</br>[Package RPM de recherche en texte intégral](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-fts-15.0.4073.23-4.x86_64.rpm)</br>[Package RPM d’extensibilité](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-extensibility-15.0.4073.23-4.x86_64.rpm)</br>[Package RPM d’extensibilité Java](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-extensibility-java-15.0.4073.23-4.x86_64.rpm)</br>[Package RPM PolyBase](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-polybase-15.0.4073.23-4.x86_64.rpm)|
| Package Ubuntu 18.04 Debian | 15.0.4073.23-4 | [Package Debian du moteur](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server/mssql-server_15.0.4073.23-4_amd64.deb)</br>[Package Debian haute disponibilité](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-ha/mssql-server-ha_15.0.4073.23-4_amd64.deb)</br>[Package Debian de recherche en texte intégral](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-fts/mssql-server-fts_15.0.4073.23-4_amd64.deb)</br>[Package Debian d’extensibilité](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-extensibility/mssql-server-extensibility_15.0.4073.23-4_amd64.deb)</br>[Package Debian d’extensibilité Java](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-extensibility-java/mssql-server-extensibility-java_15.0.4073.23-4_amd64.deb)</br>[Package RPM PolyBase](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-polybase/mssql-server-polybase_15.0.4073.23-4_amd64.deb)|

## <a name="cu6-july-2020"></a><a id="cu6"></a> CU6 (juillet 2020)

Il s’agit de la version de la mise à jour cumulative 6 (CU6) de SQL Server 2019 (15.x). La version du moteur de base de données SQL Server pour cette version est 15.0.4053.23. Pour plus d’informations sur les correctifs et améliorations, consultez <https://support.microsoft.com/help/4563110>.

### <a name="package-details"></a>Détails du package

Pour les installations du package manuelles ou hors connexion, vous pouvez télécharger les packages RPM et Debian avec les informations figurant dans la table suivante :

> [!NOTE]
> À compter de CU1, les liens d’installation de package hors connexion pour Red Hat pointent vers les packages RHEL 8. Si vous recherchez les packages RHEL 7, reportez-vous au chemin de téléchargement <https://packages.microsoft.com/rhel/7/mssql-server-2019/>
>
> **Ubuntu 18.04** est maintenant pris en charge sur SQL Server 2019 à compter de CU3. Les liens d’installation de package hors connexion pour Ubuntu pointent vers les packages Ubuntu 18.04. Si vous recherchez les packages Ubuntu 16.04, reportez-vous au chemin de téléchargement <https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/>

| Package | Version du package | Téléchargements |
|-----|-----|-----|
| Package Red Hat RPM | 15.0.4053.23-2 | [Package RPM du moteur](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-15.0.4053.23-2.x86_64.rpm)</br>[Package RPM haute disponibilité](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-ha-15.0.4053.23-2.x86_64.rpm)</br>[Package RPM de recherche en texte intégral](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-fts-15.0.4053.23-2.x86_64.rpm)</br>[Package RPM d’extensibilité](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-extensibility-15.0.4053.23-2.x86_64.rpm)</br>[Package RPM d’extensibilité Java](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-extensibility-java-15.0.4053.23-2.x86_64.rpm)</br>[Package RPM PolyBase](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-polybase-15.0.4053.23-2.x86_64.rpm)|
| Package SLES RPM | 15.0.4053.23-2 | [Package RPM du moteur mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-15.0.4053.23-2.x86_64.rpm)</br>[Package RPM haute disponibilité](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-ha-15.0.4053.23-2.x86_64.rpm)</br>[Package RPM de recherche en texte intégral](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-fts-15.0.4053.23-2.x86_64.rpm)</br>[Package RPM d’extensibilité](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-extensibility-15.0.4053.23-2.x86_64.rpm)</br>[Package RPM d’extensibilité Java](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-extensibility-java-15.0.4053.23-2.x86_64.rpm)</br>[Package RPM PolyBase](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-polybase-15.0.4053.23-2.x86_64.rpm)|
| Package Ubuntu 18.04 Debian | 15.0.4053.23-2 | [Package Debian du moteur](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server/mssql-server_15.0.4053.23-2_amd64.deb)</br>[Package Debian haute disponibilité](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-ha/mssql-server-ha_15.0.4053.23-2_amd64.deb)</br>[Package Debian de recherche en texte intégral](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-fts/mssql-server-fts_15.0.4053.23-2_amd64.deb)</br>[Package Debian d’extensibilité](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-extensibility/mssql-server-extensibility_15.0.4053.23-2_amd64.deb)</br>[Package Debian d’extensibilité Java](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-extensibility-java/mssql-server-extensibility-java_15.0.4053.23-2_amd64.deb)</br>[Package RPM PolyBase](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-polybase/mssql-server-polybase_15.0.4053.23-2_amd64.deb)|

## <a name="cu5-june-2020"></a><a id="cu5"></a> CU5 (juin 2020)

Il s’agit de la version de la mise à jour cumulative 5 (CU5) de SQL Server 2019 (15.x). La version du moteur de base de données SQL Server pour cette version est 15.0.4043.16. Pour plus d’informations sur les correctifs et améliorations, consultez <https://support.microsoft.com/help/4552255>.

### <a name="package-details"></a>Détails du package

Pour les installations du package manuelles ou hors connexion, vous pouvez télécharger les packages RPM et Debian avec les informations figurant dans la table suivante :

> [!NOTE]
> À compter de CU1, les liens d’installation de package hors connexion pour Red Hat pointent vers les packages RHEL 8. Si vous recherchez les packages RHEL 7, reportez-vous au chemin de téléchargement <https://packages.microsoft.com/rhel/7/mssql-server-2019/>
>
> **Ubuntu 18.04** est maintenant pris en charge sur SQL Server 2019 à compter de CU3. Les liens d’installation de package hors connexion pour Ubuntu pointent vers les packages Ubuntu 18.04. Si vous recherchez les packages Ubuntu 16.04, reportez-vous au chemin de téléchargement <https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/>

| Package | Version du package | Téléchargements |
|-----|-----|-----|
| Package Red Hat RPM | 15.0.4043.16-4 | [Package RPM du moteur](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-15.0.4043.16-4.x86_64.rpm)</br>[Package RPM haute disponibilité](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-ha-15.0.4043.16-4.x86_64.rpm)</br>[Package RPM de recherche en texte intégral](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-fts-15.0.4043.16-4.x86_64.rpm)</br>[Package RPM d’extensibilité](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-extensibility-15.0.4043.16-4.x86_64.rpm)</br>[Package RPM d’extensibilité Java](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-extensibility-java-15.0.4043.16-4.x86_64.rpm)</br>[Package RPM PolyBase](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-polybase-15.0.4043.16-4.x86_64.rpm)|
| Package SLES RPM | 15.0.4043.16-4 | [Package RPM du moteur mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-15.0.4043.16-4.x86_64.rpm)</br>[Package RPM haute disponibilité](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-ha-15.0.4043.16-4.x86_64.rpm)</br>[Package RPM de recherche en texte intégral](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-fts-15.0.4043.16-4.x86_64.rpm)</br>[Package RPM d’extensibilité](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-extensibility-15.0.4043.16-4.x86_64.rpm)</br>[Package RPM d’extensibilité Java](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-extensibility-java-15.0.4043.16-4.x86_64.rpm)</br>[Package RPM PolyBase](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-polybase-15.0.4043.16-4.x86_64.rpm)|
| Package Ubuntu 18.04 Debian | 15.0.4043.16-4 | [Package Debian du moteur](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server/mssql-server_15.0.4043.16-4_amd64.deb)</br>[Package Debian haute disponibilité](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-ha/mssql-server-ha_15.0.4043.16-4_amd64.deb)</br>[Package Debian de recherche en texte intégral](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-fts/mssql-server-fts_15.0.4043.16-4_amd64.deb)</br>[Package Debian d’extensibilité](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-extensibility/mssql-server-extensibility_15.0.4043.16-4_amd64.deb)</br>[Package Debian d’extensibilité Java](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-extensibility-java/mssql-server-extensibility-java_15.0.4043.16-4_amd64.deb)</br>[Package RPM PolyBase](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-polybase/mssql-server-polybase_15.0.4043.16-4_amd64.deb)|

## <a name="cu4-april-2020"></a><a id="cu4"></a> CU4 (avril 2020)

Il s’agit de la version de la mise à jour cumulative 4 (CU4) de SQL Server 2019 (15.x). La version du moteur de base de données SQL Server pour cette version est 15.0.4033.1. Pour plus d’informations sur les correctifs et améliorations, consultez <https://support.microsoft.com/help/4548597>.

### <a name="package-details"></a>Détails du package

Pour les installations du package manuelles ou hors connexion, vous pouvez télécharger les packages RPM et Debian avec les informations figurant dans la table suivante :

> [!NOTE]
> À compter de CU1, les liens d’installation de package hors connexion pour Red Hat pointent vers les packages RHEL 8. Si vous recherchez les packages RHEL 7, reportez-vous au chemin de téléchargement <https://packages.microsoft.com/rhel/7/mssql-server-2019/>
>
> **Ubuntu 18.04** est maintenant pris en charge sur SQL Server 2019 à compter de CU3. Les liens d’installation de package hors connexion pour Ubuntu pointent vers les packages Ubuntu 18.04. Si vous recherchez les packages Ubuntu 16.04, reportez-vous au chemin de téléchargement <https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/>

| Package | Version du package | Téléchargements |
|-----|-----|-----|
| Package Red Hat RPM | 15.0.4033.1-2 | [Package RPM du moteur](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-15.0.4033.1-2.x86_64.rpm)</br>[Package RPM haute disponibilité](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-ha-15.0.4033.1-2.x86_64.rpm)</br>[Package RPM de recherche en texte intégral](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-fts-15.0.4033.1-2.x86_64.rpm)</br>[Package RPM d’extensibilité](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-extensibility-15.0.4033.1-2.x86_64.rpm)</br>[Package RPM d’extensibilité Java](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-extensibility-java-15.0.4033.1-2.x86_64.rpm)</br>[Package RPM PolyBase](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-polybase-15.0.4033.1-2.x86_64.rpm)|
| Package SLES RPM | 15.0.4033.1-2 | [Package RPM du moteur mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-15.0.4033.1-2.x86_64.rpm)</br>[Package RPM haute disponibilité](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-ha-15.0.4033.1-2.x86_64.rpm)</br>[Package RPM de recherche en texte intégral](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-fts-15.0.4033.1-2.x86_64.rpm)</br>[Package RPM d’extensibilité](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-extensibility-15.0.4033.1-2.x86_64.rpm)</br>[Package RPM d’extensibilité Java](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-extensibility-java-15.0.4033.1-2.x86_64.rpm)</br>[Package RPM PolyBase](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-polybase-15.0.4033.1-2.x86_64.rpm)|
| Package Ubuntu 18.04 Debian | 15.0.4033.1-2 | [Package Debian du moteur](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server/mssql-server_15.0.4033.1-2_amd64.deb)</br>[Package Debian haute disponibilité](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-ha/mssql-server-ha_15.0.4033.1-2_amd64.deb)</br>[Package Debian de recherche en texte intégral](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-fts/mssql-server-fts_15.0.4033.1-2_amd64.deb)</br>[Package Debian d’extensibilité](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-extensibility/mssql-server-extensibility_15.0.4033.1-2_amd64.deb)</br>[Package Debian d’extensibilité Java](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-extensibility-java/mssql-server-extensibility-java_15.0.4033.1-2_amd64.deb)</br>[Package RPM PolyBase](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-polybase/mssql-server-polybase_15.0.4033.1-2_amd64.deb)|

## <a name="cu3-march-2020"></a><a id="cu3"></a> CU3 (mars 2020)

Il s’agit de la version de la mise à jour cumulative 3 (CU3) de SQL Server 2019 (15.x). La version du moteur de base de données SQL Server pour cette version est 15.0.4023.6. Pour plus d’informations sur les correctifs et améliorations, consultez <https://support.microsoft.com/help/4538853>.

### <a name="package-details"></a>Détails du package

Pour les installations du package manuelles ou hors connexion, vous pouvez télécharger les packages RPM et Debian avec les informations figurant dans la table suivante :

> [!NOTE]
> À compter de CU1, les liens d’installation de package hors connexion pour Red Hat pointent vers les packages RHEL 8. Si vous recherchez les packages RHEL 7, reportez-vous au chemin de téléchargement <https://packages.microsoft.com/rhel/7/mssql-server-2019/>
>
> **Ubuntu 18.04** est maintenant pris en charge sur SQL Server 2019 à compter de CU3. Les liens d’installation de package hors connexion pour Ubuntu pointent vers les packages Ubuntu 18.04. Si vous recherchez les packages Ubuntu 16.04, reportez-vous au chemin de téléchargement <https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/>

| Package | Version du package | Téléchargements |
|-----|-----|-----|
| Package Red Hat RPM | 15.0.4023.6-2 | [Package RPM du moteur](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-15.0.4023.6-2.x86_64.rpm)</br>[Package RPM haute disponibilité](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-ha-15.0.4023.6-2.x86_64.rpm)</br>[Package RPM de recherche en texte intégral](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-fts-15.0.4023.6-2.x86_64.rpm)</br>[Package RPM d’extensibilité](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-extensibility-15.0.4023.6-2.x86_64.rpm)</br>[Package RPM d’extensibilité Java](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-extensibility-java-15.0.4023.6-2.x86_64.rpm)</br>[Package RPM PolyBase](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-polybase-15.0.4023.6-2.x86_64.rpm)|
| Package SLES RPM | 15.0.4023.6-2 | [Package RPM du moteur mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-15.0.4023.6-2.x86_64.rpm)</br>[Package RPM haute disponibilité](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-ha-15.0.4023.6-2.x86_64.rpm)</br>[Package RPM de recherche en texte intégral](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-fts-15.0.4023.6-2.x86_64.rpm)</br>[Package RPM d’extensibilité](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-extensibility-15.0.4023.6-2.x86_64.rpm)</br>[Package RPM d’extensibilité Java](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-extensibility-java-15.0.4023.6-2.x86_64.rpm)</br>[Package RPM PolyBase](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-polybase-15.0.4023.6-2.x86_64.rpm)|
| Package Ubuntu 18.04 Debian | 15.0.4023.6-2 | [Package Debian du moteur](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server/mssql-server_15.0.4023.6-2_amd64.deb)</br>[Package Debian haute disponibilité](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-ha/mssql-server-ha_15.0.4023.6-2_amd64.deb)</br>[Package Debian de recherche en texte intégral](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-fts/mssql-server-fts_15.0.4023.6-2_amd64.deb)</br>[Package Debian d’extensibilité](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-extensibility/mssql-server-extensibility_15.0.4023.6-2_amd64.deb)</br>[Package Debian d’extensibilité Java](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-extensibility-java/mssql-server-extensibility-java_15.0.4023.6-2_amd64.deb)</br>[Package RPM PolyBase](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-polybase/mssql-server-polybase_15.0.4023.6-2_amd64.deb)|

## <a name="cu2-february-2020"></a><a id="cu2"></a> CU2 (février 2020)

Il s’agit de la version de la mise à jour cumulative 2 (CU2) de SQL Server 2019 (15.x). La version du moteur de base de données SQL Server pour cette version est 15.0.4013.40. Pour plus d’informations sur les correctifs et améliorations, consultez <https://support.microsoft.com/help/4536075>.

### <a name="package-details"></a>Détails du package

Pour les installations du package manuelles ou hors connexion, vous pouvez télécharger les packages RPM et Debian avec les informations figurant dans la table suivante :

> [!NOTE]
> À compter de CU1, les liens d’installation de package hors connexion pour Red Hat pointent vers les packages RHEL 8. Si vous recherchez les packages RHEL 7, reportez-vous au chemin de téléchargement <https://packages.microsoft.com/rhel/7/mssql-server-2019/>

| Package | Version du package | Téléchargements |
|-----|-----|-----|
| Package Red Hat RPM | 15.0.4013.40-8 | [Package RPM du moteur](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-15.0.4013.40-8.x86_64.rpm)</br>[Package RPM haute disponibilité](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-ha-15.0.4013.40-8.x86_64.rpm)</br>[Package RPM de recherche en texte intégral](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-fts-15.0.4013.40-8.x86_64.rpm)</br>[Package RPM d’extensibilité](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-extensibility-15.0.4013.40-8.x86_64.rpm)</br>[Package RPM d’extensibilité Java](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-extensibility-java-15.0.4013.40-8.x86_64.rpm)</br>[Package RPM PolyBase](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-polybase-15.0.4013.40-8.x86_64.rpm)|
| Package SLES RPM | 15.0.4013.40-8 | [Package RPM du moteur mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-15.0.4013.40-8.x86_64.rpm)</br>[Package RPM haute disponibilité](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-ha-15.0.4013.40-8.x86_64.rpm)</br>[Package RPM de recherche en texte intégral](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-fts-15.0.4013.40-8.x86_64.rpm)</br>[Package RPM d’extensibilité](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-extensibility-15.0.4013.40-8.x86_64.rpm)</br>[Package RPM d’extensibilité Java](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-extensibility-java-15.0.4013.40-8.x86_64.rpm)</br>[Package RPM PolyBase](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-polybase-15.0.4013.40-8.x86_64.rpm)|
| Package Ubuntu 16.04 Debian | 15.0.4013.40-8 | [Package Debian du moteur](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/mssql-server/mssql-server_15.0.4013.40-8_amd64.deb)</br>[Package Debian haute disponibilité](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/mssql-server-ha/mssql-server-ha_15.0.4013.40-8_amd64.deb)</br>[Package Debian de recherche en texte intégral](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/mssql-server-fts/mssql-server-fts_15.0.4013.40-8_amd64.deb)</br>[Package Debian d’extensibilité](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/mssql-server-extensibility/mssql-server-extensibility_15.0.4013.40-8_amd64.deb)</br>[Package Debian d’extensibilité Java](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/mssql-server-extensibility-java/mssql-server-extensibility-java_15.0.4013.40-8_amd64.deb)</br>[Package RPM PolyBase](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/mssql-server-polybase/mssql-server-polybase_15.0.4013.40-8_amd64.deb)|

## <a name="cu1-january-2020"></a><a id="cu1"></a> CU1 (Janvier 2020)

Il s’agit de la version de la mise à jour cumulative 1 (CU1) de SQL Server 2019 (15.x). La version du moteur de base de données SQL Server pour cette version est 15.0.4003.23. Pour plus d’informations sur les correctifs et les améliorations de cette version, consultez <https://support.microsoft.com/en-us/help/4527376>.

### <a name="package-details"></a>Détails du package

Pour les installations du package manuelles ou hors connexion, vous pouvez télécharger les packages RPM et Debian avec les informations figurant dans la table suivante :

> [!NOTE]
> À compter de CU1, les liens d’installation de package hors connexion pour Red Hat pointent vers les packages RHEL 8. Si vous recherchez les packages RHEL 7, reportez-vous au chemin de téléchargement <https://packages.microsoft.com/rhel/7/mssql-server-2019/>

| Package | Version du package | Téléchargements |
|-----|-----|-----|
| Package Red Hat RPM | 15.0.4003.23-3 | [Package RPM du moteur](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-15.0.4003.23-3.x86_64.rpm)</br>[Package RPM haute disponibilité](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-ha-15.0.4003.23-3.x86_64.rpm)</br>[Package RPM de recherche en texte intégral](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-fts-15.0.4003.23-3.x86_64.rpm)</br>[Package RPM d’extensibilité](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-extensibility-15.0.4003.23-3.x86_64.rpm)</br>[Package RPM d’extensibilité Java](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-extensibility-java-15.0.4003.23-3.x86_64.rpm)</br>[Package RPM PolyBase](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-polybase-15.0.4003.23-3.x86_64.rpm)|
| Package SLES RPM | 15.0.4003.23-3 | [Package RPM du moteur mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-15.0.4003.23-3.x86_64.rpm)</br>[Package RPM haute disponibilité](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-ha-15.0.4003.23-3.x86_64.rpm)</br>[Package RPM de recherche en texte intégral](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-fts-15.0.4003.23-3.x86_64.rpm)</br>[Package RPM d’extensibilité](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-extensibility-15.0.4003.23-3.x86_64.rpm)</br>[Package RPM d’extensibilité Java](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-extensibility-java-15.0.4003.23-3.x86_64.rpm)</br>[Package RPM PolyBase](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-polybase-15.0.4003.23-3.x86_64.rpm)|
| Package Ubuntu 16.04 Debian | 15.0.4003.23-3 | [Package Debian du moteur](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/mssql-server/mssql-server_15.0.4003.23-3_amd64.deb)</br>[Package Debian haute disponibilité](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/mssql-server-ha/mssql-server-ha_15.0.4003.23-3_amd64.deb)</br>[Package Debian de recherche en texte intégral](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/mssql-server-fts/mssql-server-fts_15.0.4003.23-3_amd64.deb)</br>[Package Debian d’extensibilité](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/mssql-server-extensibility/mssql-server-extensibility_15.0.4003.23-3_amd64.deb)</br>[Package Debian d’extensibilité Java](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/mssql-server-extensibility-java/mssql-server-extensibility-java_15.0.4003.23-3_amd64.deb)</br>[Package RPM PolyBase](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/mssql-server-polybase/mssql-server-polybase_15.0.4003.23-3_amd64.deb)|

## <a name="ga-november-2019"></a><a id="ga"></a> Disponibilité générale (novembre 2019)

Il s’agit de la version en disponibilité générale de SQL Server 2019 (15.x). La version du moteur de base de données SQL Server pour cette version est 15.0.2000.5.

### <a name="package-details"></a>Détails du package

Pour les installations du package manuelles ou hors connexion, vous pouvez télécharger les packages RPM et Debian avec les informations figurant dans la table suivante :

| Package | Version du package | Téléchargements |
|-----|-----|-----|
| Package Red Hat RPM | 15.0.2000.5-5 | [Package RPM du moteur](https://packages.microsoft.com/rhel/7/mssql-server-2019/mssql-server-15.0.2000.5-5.x86_64.rpm)</br>[Package RPM haute disponibilité](https://packages.microsoft.com/rhel/7/mssql-server-2019/mssql-server-ha-15.0.2000.5-5.x86_64.rpm)</br>[Package RPM de recherche en texte intégral](https://packages.microsoft.com/rhel/7/mssql-server-2019/mssql-server-fts-15.0.2000.5-5.x86_64.rpm)</br>[Package RPM d’extensibilité](https://packages.microsoft.com/rhel/7/mssql-server-2019/mssql-server-extensibility-15.0.2000.5-5.x86_64.rpm)</br>[Package RPM d’extensibilité Java](https://packages.microsoft.com/rhel/7/mssql-server-2019/mssql-server-extensibility-java-15.0.2000.5-5.x86_64.rpm)</br>[Package RPM PolyBase](https://packages.microsoft.com/rhel/7/mssql-server-2019/mssql-server-polybase-15.0.2000.5-5.x86_64.rpm)|
| Package SLES RPM | 15.0.2000.5-5 | [Package RPM du moteur mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-15.0.2000.5-5.x86_64.rpm)</br>[Package RPM haute disponibilité](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-ha-15.0.2000.5-5.x86_64.rpm)</br>[Package RPM de recherche en texte intégral](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-fts-15.0.2000.5-5.x86_64.rpm)</br>[Package RPM d’extensibilité](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-extensibility-15.0.2000.5-5.x86_64.rpm)</br>[Package RPM d’extensibilité Java](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-extensibility-java-15.0.2000.5-5.x86_64.rpm)</br>[Package RPM PolyBase](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-polybase-15.0.2000.5-5.x86_64.rpm)|
| Package Ubuntu 16.04 Debian | 15.0.2000.5-5 | [Package Debian du moteur](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/mssql-server/mssql-server_15.0.2000.5-5_amd64.deb)</br>[Package Debian haute disponibilité](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/mssql-server-ha/mssql-server-ha_15.0.2000.5-5_amd64.deb)</br>[Package Debian de recherche en texte intégral](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/mssql-server-fts/mssql-server-fts_15.0.2000.5-5_amd64.deb)</br>[Package Debian d’extensibilité](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/mssql-server-extensibility/mssql-server-extensibility_15.0.2000.5-5_amd64.deb)</br>[Package Debian d’extensibilité Java](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/mssql-server-extensibility-java/mssql-server-extensibility-java_15.0.2000.5-5_amd64.deb)</br>[Package RPM PolyBase](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/mssql-server-polybase/mssql-server-polybase_15.0.2000.5-5_amd64.deb)|

## <a name="release-candidate-august-2019"></a><a id="rc"></a> Version Release Candidate (août 2019)

Les sections suivantes fournissent des emplacements de package et des problèmes connus pour la version Release Candidate. Pour en savoir plus sur les nouvelles fonctionnalités de Linux sur SQL Server 2019, consultez [Nouveautés de SQL Server 2019](../sql-server/what-s-new-in-sql-server-ver15.md).

### <a name="package-details"></a>Détails du package

Pour les installations du package manuelles ou hors connexion, vous pouvez télécharger les packages RPM et Debian avec les informations figurant dans la table suivante :

| Package | Version du package | Téléchargements |
|-----|-----|-----|
| Package Red Hat RPM | 15.0.1900.25-1 | [Package RPM du moteur](https://packages.microsoft.com/rhel/7/mssql-server-preview/mssql-server-15.0.1900.25-1.x86_64.rpm)</br>[Package RPM de recherche en texte intégral](https://packages.microsoft.com/rhel/7/mssql-server-preview/mssql-server-fts-15.0.1900.25-1.x86_64.rpm)</br>[Package RPM d’extensibilité](https://packages.microsoft.com/rhel/7/mssql-server-preview/mssql-server-extensibility-15.0.1900.25-1.x86_64.rpm)</br>[Package RPM d’extensibilité Java](https://packages.microsoft.com/rhel/7/mssql-server-preview/mssql-server-extensibility-java-15.0.1900.25-1.x86_64.rpm)</br>[Package RPM PolyBase](https://packages.microsoft.com/rhel/7/mssql-server-preview/mssql-server-polybase-15.0.1900.25-1.x86_64.rpm)|
| Package SLES RPM | 15.0.1900.25-1 | [Package RPM du moteur mssql-server](https://packages.microsoft.com/sles/12/mssql-server-preview/mssql-server-15.0.1900.25-1.x86_64.rpm)</br>[Package RPM de recherche en texte intégral](https://packages.microsoft.com/sles/12/mssql-server-preview/mssql-server-fts-15.0.1900.25-1.x86_64.rpm)</br>[Package RPM d’extensibilité](https://packages.microsoft.com/sles/12/mssql-server-preview/mssql-server-extensibility-15.0.1900.25-1.x86_64.rpm)</br>[Package RPM d’extensibilité Java](https://packages.microsoft.com/sles/12/mssql-server-preview/mssql-server-extensibility-java-15.0.1900.25-1.x86_64.rpm)</br>[Package RPM PolyBase](https://packages.microsoft.com/sles/12/mssql-server-preview/mssql-server-polybase-15.0.1900.25-1.x86_64.rpm)|
| Package Ubuntu 16.04 Debian | 15.0.1900.25-1 | [Package Debian du moteur](https://packages.microsoft.com/ubuntu/16.04/mssql-server-preview/pool/main/m/mssql-server/mssql-server_15.0.1900.25-1_amd64.deb)</br>[Package Debian de recherche en texte intégral](https://packages.microsoft.com/ubuntu/16.04/mssql-server-preview/pool/main/m/mssql-server-fts/mssql-server-fts_15.0.1900.25-1_amd64.deb)</br>[Package Debian d’extensibilité](https://packages.microsoft.com/ubuntu/16.04/mssql-server-preview/pool/main/m/mssql-server-extensibility/mssql-server-extensibility_15.0.1900.25-1_amd64.deb)</br>[Package Debian d’extensibilité Java](https://packages.microsoft.com/ubuntu/16.04/mssql-server-preview/pool/main/m/mssql-server-extensibility-java/mssql-server-extensibility-java_15.0.1900.25-1_amd64.deb)</br>[Package RPM PolyBase](https://packages.microsoft.com/ubuntu/16.04/mssql-server-preview/pool/main/m/mssql-server-polybase/mssql-server-polybase_15.0.1900.25-1_amd64.deb)|

## <a name="known-issues"></a>Problèmes connus

Les sections suivantes décrivent les problèmes connus avec la version en disponibilité générale de SQL Server 2019 (15.x) sur Linux.

### <a name="general"></a>Général

- La longueur du nom d’hôte dans lequel [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] est installé doit être inférieure ou égale à 15 caractères. 

    - **Résolution** : Modifiez le nom dans /etc/nom d’hôte en lui attribuant une longueur de 15 caractères ou moins.

- Le paramétrage manuel de l’heure système à rebours entraînera l’arrêt de la mise à jour de l’heure système interne par [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] dans [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].

    - **Résolution** : Redémarrez [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].

- Seules les installations à instance unique sont prises en charge.

    - **Résolution** : Si vous souhaitez avoir plusieurs instances sur un hôte donné, envisagez d’utiliser des machines virtuelles ou des conteneurs Docker. 

- [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Configuration Manager ne peut pas se connecter à [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] sur Linux.

- La langue par défaut de la connexion **sa** est l’anglais.

    - **Résolution** : Modifiez la langue de la connexion **sa** à l'aide de l’instruction **ALTER LOGIN**.

- Le fournisseur OLEDB journalise l’avertissement suivant : `Failed to verify the Authenticode signature of 'C:\binn\msoledbsql.dll'. Signature verification of SQL Server DLLs will be skipped. Genuine copies of SQL Server are signed. Failure to verify the Authenticode signature might indicate that this is not an authentic release of SQL Server. Install a genuine copy of SQL Server or contact customer support.`

   - **Résolution** : Aucune action n'est requise. Le fournisseur OLEDB est signé avec SHA256. Le moteur de base de données SQL Server ne valide pas correctement le fichier .dll signé.

### <a name="databases"></a>Bases de données

- La base de données MASTER ne peut pas être déplacée avec l’utilitaire mssql-conf. D’autres bases de données système peuvent être déplacées avec mssql-conf.

- Lors de la restauration d’une base de données qui a été sauvegardée sur [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] sur Windows, vous devez utiliser la clause **WITH MOVE** dans l’instruction Transact-SQL.

- Certains algorithmes (suites de chiffrement) pour le protocole TLS (Transport Layer Security) ne fonctionnent pas correctement avec [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] sur Linux. Cela entraîne des échecs de connexion lors d’une tentative de connexion à [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], ainsi que des problèmes lors de l’établissement de connexions entre les réplicas dans des groupes à haute disponibilité.

   - **Résolution** : Modifiez le script de configuration **mssql.conf** pour [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] sur Linux afin de désactiver les suites de chiffrement problématiques, en procédant comme suit :

      1. Ajoutez le code suivant à /var/opt/mssql/mssql.conf.

          ```
          [network]
          tlsciphers= AES256-GCM-SHA384:AES128-GCM-SHA256:AES256-SHA256:AES128-SHA256:AES256-SHA:AES128-SHA:!ECDHE-RSA-AES128-GCM-SHA256:!ECDHE-RSA-AES256-GCM-SHA384:!ECDHE-ECDSA-AES256-GCM-SHA384:!ECDHE-ECDSA-AES128-GCM-SHA256:!ECDHE-ECDSA-AES256-SHA384:!ECDHE-ECDSA-AES128-SHA256:!ECDHE-ECDSA-AES256-SHA:!ECDHE-ECDSA-AES128-SHA:!ECDHE-RSA-AES256-SHA384:!ECDHE-RSA-AES128-SHA256:!ECDHE-RSA-AES256-SHA:!ECDHE-RSA-AES128-SHA:!DHE-RSA-AES256-GCM-SHA384:!DHE-RSA-AES128-GCM-SHA256:!DHE-RSA-AES256-SHA:!DHE-RSA-AES128-SHA:!DHE-DSS-AES256-SHA256:!DHE-DSS-AES128-SHA256:!DHE-DSS-AES256-SHA:!DHE-DSS-AES128-SHA:!DHE-DSS-DES-CBC3-SHA:!NULL-SHA256:!NULL-SHA
          ```

         > [!NOTE]
         > Dans le code précédent, `!` inverse l’expression. Cela indique à OpenSSL de ne pas utiliser la suite de chiffrement suivante.  

      1. Démarrez [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] avec la commande suivante.

          ```bash
          sudo systemctl restart mssql-server
          ```

- Les bases de données [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] sur Windows qui utilisent l’OLTP en mémoire ne peuvent pas être restaurées sur SQL Server 2019 (15.x) sur Linux. Pour restaurer une base de données [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] qui utilise l’OLTP en mémoire, commencez par mettre à niveau les bases de données vers [!INCLUDE[ssSQL15](../includes/sssql16-md.md)] ou SQL Server 2017 ou SQL Server 2019 sur Windows avant de les déplacer vers [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] sur Linux par sauvegarde/restauration ou détachement/attachement.

- L’autorisation utilisateur **ADMINISTER BULK OPERATIONS** n’est pas prise en charge sur Linux pour l’instant.

### <a name="networking"></a>Mise en réseau

Les fonctionnalités qui impliquent des connexions TCP sortantes à partir du processus sqlservr, telles que les serveurs liés ou les groupes de disponibilité, peuvent ne pas fonctionner si les deux conditions suivantes sont réunies :

1. Le serveur cible est spécifié sous la forme d’un nom d’hôte et non d’une adresse IP.

1. IPv6 est désactivé dans le noyau de l’instance source. Pour vérifier si IPv6 est activé dans le noyau sur votre système, tous les tests suivants doivent réussir :

   - `cat /proc/cmdline` imprime le cmdline de démarrage du noyau actuel. La sortie ne doit pas contenir `ipv6.disable=1`.
   - Le répertoire /proc/sys/net/ipv6/ doit exister.
   - Un programme C qui appelle `socket(AF_INET6, SOCK_STREAM, IPPROTO_IP)` doit réussir : syscall doit retourner un fd != -1 et ne pas échouer avec EAFNOSUPPORT.

L’erreur exacte dépend de la fonctionnalité. Pour les serveurs liés, cela se manifeste comme une erreur de délai d’attente de connexion. Pour les groupes de disponibilité, le DDL `ALTER AVAILABILITY GROUP JOIN` sur le secondaire échouera après 5 minutes avec une erreur de délai d’expiration de configuration de téléchargement.

Pour contourner ce problème, procédez comme suit :

1. Utilisez des adresses IP au lieu de noms d’hôtes pour spécifier la cible de la connexion TCP.

1. Activez IPv6 dans le noyau en supprimant `ipv6.disable=1` de la cmdline de démarrage. La façon de procéder dépend de la distribution Linux et du chargeur de démarrage, par exemple le fichier Grub. Si vous ne souhaitez pas que IPv6 soit désactivé, vous pouvez toujours le désactiver en définissant `net.ipv6.conf.all.disable_ipv6 = 1` dans la configuration `sysctl` (par exemple, `/etc/sysctl.conf`). Cela empêchera toujours la carte réseau du système d’obtenir une adresse IPv6, tout en autorisant le fonctionnement des fonctionnalités de sqlservr.

#### <a name="network-file-system-nfs"></a>NFS (Network File System)
Si vous utilisez des partages distants **NFS (Network File System)** en production, notez les exigences de support suivantes :

- Utilisez la version **4.2 ou ultérieure** de NFS. Les versions antérieures de NFS ne prennent pas en charge les fonctionnalités requises telles que la création de fichiers `fallocate` et partiellement alloués, communs aux systèmes de fichiers modernes.
- Localisez uniquement les répertoires **/var/opt/mssql** sur le montage NFS. D’autres fichiers, tels que les binaires du système [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], ne sont pas pris en charge.
- Vérifiez que les clients NFS utilisent l’option `nolock` lors du montage du partage distant.

### <a name="localization"></a>Localisation

- Si vos paramètres régionaux ne sont pas en anglais (en_US) lors de l’installation, vous devez utiliser l’encodage UTF-8 dans votre session/terminal Bash. Si vous utilisez l’encodage ASCII, vous pouvez voir une erreur semblable à la suivante :

   ```
   UnicodeEncodeError: 'ascii' codec can't encode character u'\xf1' in position 8: ordinal not in range(128)
   ```

   Si vous ne pouvez pas utiliser l’encodage UTF-8, exécutez le programme d’installation à l’aide de la variable d’environnement MSSQL_LCID pour spécifier votre choix de langue.

   ```bash
   sudo MSSQL_LCID=<LcidValue> /opt/mssql/bin/mssql-conf setup
   ```

- Quand vous exécutez la configuration mssql-conf et que vous effectuez une installation non anglaise de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], des caractères étendus incorrects sont affichés après le texte localisé « Configuration de SQL Server... ». Ou, pour les installations non latines, la phrase peut manquer complètement. La phrase manquante doit afficher la chaîne localisée suivante : « Le PID de gestion des licences a été traité avec succès. La nouvelle édition est [\<Name\> edition] ». Cette chaîne est générée à titre d’information uniquement et la mise à jour cumulative [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] suivante répond en toutes les langues. Cela n’affecte en rien la réussite de l’installation de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. 

#### <a name="full-text-search"></a>Recherche en texte intégral

- Tous les filtres ne sont pas disponibles dans cette mise en production, y compris les filtres pour les documents Office. Pour obtenir la liste des filtres pris en charge, consultez [Installer la recherche en texte intégral SQL Server sur Linux](sql-server-linux-setup-full-text-search.md#filters).

### <a name="sql-server-integration-services-ssis"></a><a id="ssis"></a> SQL Server Integration Services (SSIS)

- Le package **mssql-server-is** n’est pas pris en charge sur SUSE dans cette mise en production. Il est actuellement pris en charge sur Ubuntu et sur Red Hat Enterprise Linux (RHEL).

- Avec [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] sur Linux CTP 2.1 Refresh et versions ultérieures, les packages [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] peuvent utiliser les connexions ODBC sur Linux. Cette fonctionnalité a été testée avec les pilotes [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] et MySQL ODBC, mais elle devrait également fonctionner avec tout pilote ODBC Unicode conforme à la spécification ODBC. Au moment de la conception, vous pouvez fournir un DSN ou une chaîne de connexion pour vous connecter aux données ODBC ; vous pouvez également utiliser l'authentification Windows. Pour plus d'informations, consultez le [billet de blog annonçant le support d’ODBC sur Linux](https://blogs.msdn.microsoft.com/ssis/2017/06/16/odbc-is-supported-in-ssis-on-linux-ssis-helsinki-ctp2-1-refresh/).

- Les fonctionnalités suivantes ne sont pas prises en charge dans cette mise en production lorsque vous exécutez des packages SSIS sur Linux :
  - [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] Base de données de catalogue
  - Exécution planifiée du package par l’agent SQL
  - Authentification Windows
  - Composants tiers
  - Capture de données modifiées (CDC)
  - [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] Scale-out
  - Feature Pack Azure pour SSIS
  - Support Hadoop et HDFS
  - Microsoft Connector pour SAP BW

Pour obtenir la liste des composants SSIS intégrés qui ne sont pas actuellement pris en charge ou qui sont pris en charge avec les limitations, consultez [Limitations et problèmes connus pour SSIS sur Linux](sql-server-linux-ssis-known-issues.md#components).

Pour plus d’informations concernant SSIS sur Linux, consultez les articles suivants :
-   [Billet de blog annonçant le support SSIS pour Linux](https://blogs.msdn.microsoft.com/ssis/2017/05/17/ssis-helsinki-is-available-in-sql-server-vnext-ctp2-1/).
-   [Installer SQL Server Integration Services (SSIS) sur Linux](sql-server-linux-setup-ssis.md)
-   [Extraire, transformer et charger des données sur Linux avec SSIS](sql-server-linux-migrate-ssis.md)

### <a name="sql-server-management-studio-ssms"></a><a id="ssms"></a> SQL Server Management Studio (SSMS)

Les limitations suivantes s’appliquent à [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] sur Windows connecté à [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] sur Linux.

- Les plans de maintenance ne sont pas pris en charge.

- Management Data Warehouse (MDW) et le collecteur de données dans [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] ne sont pas pris en charge. 

- Les composants d’interface utilisateur [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] qui ont des options d’authentification Windows ou de journal des événements Windows ne fonctionnent pas avec Linux. Vous pouvez toujours utiliser ces fonctionnalités avec d’autres options, telles que les connexions SQL. 

- Impossible de modifier le nombre de fichiers journaux à conserver.

## <a name="next-steps"></a>Étapes suivantes

Pour démarrer, consultez les guides de démarrage rapide suivants :

- [Installer sur Red Hat Enterprise Linux](quickstart-install-connect-red-hat.md)
- [Installer sur SUSE Linux Enterprise Server](quickstart-install-connect-suse.md)
- [Installer sur Ubuntu](quickstart-install-connect-ubuntu.md)
- [Exécuter sur Docker](quickstart-install-connect-ubuntu.md)
- [Approvisionner une machine virtuelle SQL dans Azure](/azure/virtual-machines/linux/sql/provision-sql-server-linux-virtual-machine?toc=/sql/toc/toc.json)
- [Exécuter et connecter - Cloud](quickstart-install-connect-clouds.md)

Pour obtenir des réponses aux questions fréquemment posées, consultez la [FAQ de SQL Server sur Linux](sql-server-linux-faq.md).
