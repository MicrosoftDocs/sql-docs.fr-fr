---
title: Notes de publication pour SQL Server 2017 sur Linux
description: Cet article contient les notes de publication et fonctionnalités prises en charge pour SQL Server 2017 s’exécutant sur Linux. Les notes de publication sont incluses dans la mise en production la plus récente et dans plusieurs mises en production précédentes.
author: VanMSFT
ms.author: vanto
ms.date: 02/24/2021
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 1314744f-fcaf-46db-800e-2918fa7e1b6c
ms.openlocfilehash: 8e6dba205964b8dd2564641758d4c280fa28485d
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/08/2021
ms.locfileid: "102465005"
---
# <a name="release-notes-for-sql-server-2017-on-linux"></a>Notes de publication pour SQL Server 2017 sur Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Les notes de publication suivantes s’appliquent à [!INCLUDE[ssSQL17](../includes/sssql17-md.md)] s’exécutant sur Linux. Cet article est divisé en sections pour chaque mise en production. La mise en production GA offre une prise en charge détaillée et des problèmes connus répertoriés. Chaque mise à jour cumulative (CU) ou la mise à jour de distribution générale (GDR) a un lien vers un article de support décrivant les modifications de la CU, ainsi que des liens de téléchargement des packages Linux.

> [!TIP]
> Ces notes de publication sont spécifiques aux mises en production [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. Pour plus d’informations sur le nouveau [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)], consultez [Notes de publication pour la préversion SQL Server 2019 sur Linux](sql-server-linux-release-notes-2019.md?view=sql-server-ver15&preserve-view=true).

## <a name="supported-platforms"></a>Plateformes prises en charge

| Plateforme | Système de fichiers | Guide d'installation |
|-----|-----|-----|
| Serveur Red Hat Enterprise Linux 7.3 - 7.8 ou 8.0 - 8.3 | XFS ou EXT4 | [Guide d'installation](quickstart-install-connect-red-hat.md) | 
| SUSE Enterprise Linux Server v12 SP2 - SP5 | XFS ou EXT4 | [Guide d'installation](quickstart-install-connect-suse.md) |
| Ubuntu 16.04 LTS, 18.04 LTS | XFS ou EXT4 | [Guide d'installation](quickstart-install-connect-ubuntu.md) | 
| Moteur Docker 1.8+ sur Windows, Mac ou Linux | N/A | [Guide d'installation](quickstart-install-connect-docker.md) | 

> [!TIP]
> Pour plus d’informations, consultez les [exigences système](sql-server-linux-setup.md#system) pour [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] sur Linux. Pour obtenir la dernière stratégie de support pour [!INCLUDE[ssSQL17](../includes/sssql17-md.md)], consultez la [Stratégie de support technique pour Microsoft SQL Server](https://support.microsoft.com/help/4047326/support-policy-for-microsoft-sql-server).

## <a name="tools"></a>Outils

La plupart des outils client existants qui ciblent [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] peuvent cibler en toute transparence [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] s’exécutant sur Linux. Certains outils peuvent avoir une exigence de version spécifique pour fonctionner correctement avec Linux. Pour obtenir la liste complète des outils [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], consultez [Outils et utilitaires SQL pour SQL Server](../tools/overview-sql-tools.md).

## <a name="release-history"></a>Historique des mises en production

La table suivante énumère l’historique des mises en production pour [!INCLUDE[ssSQL17](../includes/sssql17-md.md)].

| Libérer               | Version       | Date de publication |
|-----------------------|---------------|--------------|
| [CU23](#CU23)         | 14.0.3381.3  | 24/02/2020   |
| [CU22-GDR](#CU22)         | 14.0.3370.1  | 2021-01-12 |
| [CU22](#CU22)         | 14.0.3356.20  | 2020-09-10   |
| [CU21](#CU21)         | 14.0.3335.7   | 2020-07-01   |
| [CU20](#CU20)         | 14.0.3294.2   | 2020-04-10   |
| [CU19](#CU19)         | 14.0.3281.6   | 2020-02-05   |
| [CU18](#CU18)         | 14.0.3257.3   | 09-12-2019   |
| [CU17](#CU17)         | 14.0.3238.1   | 2019-10-08   |
| [CU16](#CU16)         | 14.0.3223.3   | 2019-08-01   |
| [CU15](#CU15)         | 14.0.3162.1   | 2019-05-23   |
| [CU14](#CU14)         | 14.0.3076.1   | 2019-03-25   |
| [CU13](#CU13)         | 14.0.3048.4   | 2018-12-18   |
| [CU12](#CU12)         | 14.0.3045.24  | 2018-10-24   |
| [CU11](#CU11)         | 14.0.3038.14  | 2018-09-20   |
| [CU10](#CU10)         | 14.0.3037.1   | 2018-08-27   |
| [CU9-GDR2](#CU9-GDR2) | 14.0.3035.2   | 2018-08-18   |
| [GDR2](#GDR2)         | 14.0.2002.14  | 2018-08-18   |
| [CU9](#CU9)           | 14.0.3030.27  | 2018-07-18   |
| [CU8](#CU8)           | 14.0.3029.16  | 2018-06-21   |
| [CU7](#CU7)           | 14.0.3026.27  | 2018-05-24   |
| [CU6](#CU6)           | 14.0.3025.34  | 2018-04-19   |
| [CU5](#CU5)           | 14.0.3023.8   | 2018-03-20   |
| [CU4](#CU4)           | 14.0.3022.28  | 2018-02-20   |
| [CU3](#CU3)           | 14.0.3015.40  | 2018-01-03   |
| [GDR1](#GDR1)         | 14.0.2000.63  | 2018-01-03   |
| [CU2](#CU2)           | 14.0.3008.27  | 2017-11-28   |
| [CU1](#CU1)           | 14.0.3006.16  | 2017-10-24   |
| [GA](#GA)             | 14.0.1000.169 | 2017-10-02   |

## <a name="how-to-install-updates"></a><a id="cuinstall"></a> Comment installer les mises à jour

Si vous avez configuré le référentiel de la CU (**mssql-server-2017**), vous obtenez la dernière unité de capacité des packages [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] quand vous effectuez de nouvelles installations. Le référentiel de la CU est la valeur par défaut pour tous les articles d’installation du package pour [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] sur Linux. Si vous avez configuré le référentiel GDR (**mssql-server-2017-gdr**), vous obtiendrez uniquement les mises à jour de sécurité critiques publiées depuis la disponibilité générale. Si vous avez besoin des mises à jours CU ou GDR du conteneur Docker, consultez les images officielles pour [Microsoft SQL Server sur Linux pour le moteur Docker](https://hub.docker.com/r/microsoft/mssql-server). Pour plus d’informations sur la configuration du référentiel, consultez [Configurer les référentiels pour SQL Server sur Linux](sql-server-linux-change-repo.md).

Si vous mettez à jour des packages [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] existants, exécutez la commande de mise à jour appropriée pour chaque package afin d’obtenir la dernière CU. Pour obtenir des instructions de mise à jour spécifiques pour chaque package, consultez les guides d’installation suivants :

- [Installer un package SQL Server](sql-server-linux-setup.md#upgrade)
- [Installer un package de recherche en texte intégral](sql-server-linux-setup-full-text-search.md)
- [Installer SQL Server Integration Services](sql-server-linux-setup-ssis.md)
- [Activer SQL Server Agent](sql-server-linux-setup-sql-agent.md)

## <a name="cu23-february-2021"></a><a id="CU23"></a> CU23 (février 2021)

Il s’agit de la mise en production de la Mise à jour cumulative 23 (CU23) de [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. La version [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] de cette mise en production est 14.0.3381.3. Pour plus d’informations sur les correctifs et les améliorations de cette version, consultez <https://support.microsoft.com/help/5000685>.

### <a name="package-details"></a>Détails du package

Pour les installations du package manuelles ou hors connexion, vous pouvez télécharger les packages RPM et Debian avec les informations figurant dans la table suivante :

> [!NOTE]
> **Ubuntu 18.04** et **RHEL 8** sont maintenant pris en charge sur SQL Server 2017 à compter de CU20.
>
> Les liens d’installation du package hors connexion pour Ubuntu pointent vers les packages Ubuntu 18.04, à l’exception du package SSIS (qui n’est pas disponible pour Ubuntu 18.04). Si vous recherchez les packages Ubuntu 16.04, reportez-vous au chemin de téléchargement <https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/>.
>
> Les liens d’installation de package hors connexion pour Red Hat pointent vers des packages RHEL 8, à l’exception du package SSIS (qui n’est pas disponible pour RHEL 8). Si vous recherchez les packages RHEL 7, reportez-vous au chemin de téléchargement <https://packages.microsoft.com/rhel/7/mssql-server-2017/>.

| Package | Version du package | Téléchargements |
|-----|-----|-----|
| Package Red Hat RPM | 14.0.3381.3-2 | [Package RPM du moteur](https://packages.microsoft.com/rhel/8/mssql-server-2017/mssql-server-14.0.3381.3-2.x86_64.rpm)</br>[Package RPM haute disponibilité](https://packages.microsoft.com/rhel/8/mssql-server-2017/mssql-server-ha-14.0.3381.3-2.x86_64.rpm)</br>[Package RPM de recherche en texte intégral](https://packages.microsoft.com/rhel/8/mssql-server-2017/mssql-server-fts-14.0.3381.3-2.x86_64.rpm)</br>[Package SSIS](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| Package SLES RPM | 14.0.3381.3-2 | [Package RPM du moteur mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3381.3-2.x86_64.rpm)</br>[Package RPM haute disponibilité](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3381.3-2.x86_64.rpm)</br>[Package RPM de recherche en texte intégral](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3381.3-2.x86_64.rpm) | 
| Package Ubuntu 18.04 Debian | 14.0.3381.3-2 | [Package Debian du moteur](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3381.3-2_amd64.deb)</br>[Package Debian haute disponibilité](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3381.3-2_amd64.deb)</br>[Package Debian de recherche en texte intégral](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3381.3-2_amd64.deb)<br/>[Package SSIS](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu22-gdr-january-2021"></a><a id="CU22-GDR"></a> CU22-GDR (janvier 2021)

Il s’agit de la mise en production de la mise à jour cumulative 22-GDR (CU22-GDR) de [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. La version [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] de cette mise en production est 14.0.3370.1. Pour plus d’informations sur les correctifs et les améliorations de cette version, consultez <https://support.microsoft.com/help/4577467>.

### <a name="package-details"></a>Détails du package

Pour les installations du package manuelles ou hors connexion, vous pouvez télécharger les packages RPM et Debian avec les informations figurant dans la table suivante :

> [!NOTE]
> **Ubuntu 18.04** et **RHEL 8** sont maintenant pris en charge sur SQL Server 2017 à compter de CU20.
>
> Les liens d’installation du package hors connexion pour Ubuntu pointent vers les packages Ubuntu 18.04, à l’exception du package SSIS (qui n’est pas disponible pour Ubuntu 18.04). Si vous recherchez les packages Ubuntu 16.04, reportez-vous au chemin de téléchargement <https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/>.
>
> Les liens d’installation de package hors connexion pour Red Hat pointent vers des packages RHEL 8, à l’exception du package SSIS (qui n’est pas disponible pour RHEL 8). Si vous recherchez les packages RHEL 7, reportez-vous au chemin de téléchargement <https://packages.microsoft.com/rhel/7/mssql-server-2017/>.

| Package | Version du package | Téléchargements |
|-----|-----|-----|
| Package Red Hat RPM | 14.0.3370.1-23-18 | [Package RPM du moteur](https://packages.microsoft.com/rhel/8/mssql-server-2017/mssql-server-14.0.3370.1-18.x86_64.rpm)</br>[Package RPM haute disponibilité](https://packages.microsoft.com/rhel/8/mssql-server-2017/mssql-server-ha-14.0.3370.1-18.x86_64.rpm)</br>[Package RPM de recherche en texte intégral](https://packages.microsoft.com/rhel/8/mssql-server-2017/mssql-server-fts-14.0.3370.1-18.x86_64.rpm)</br>[Package SSIS](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) |
| Package SLES RPM | 14.0.3370.1-18 | [Package RPM du moteur mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3370.1-18.x86_64.rpm)</br>[Package RPM haute disponibilité](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3370.1-18.x86_64.rpm)</br>[Package RPM de recherche en texte intégral](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3370.1-18.x86_64.rpm) | 
| Package Ubuntu 18.04 Debian | 14.0.3370.1-18 | [Package Debian du moteur](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3370.1-18_amd64.deb)</br>[Package Debian haute disponibilité](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3370.1-18_amd64.deb)</br>[Package Debian de recherche en texte intégral](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3370.1-18_amd64.deb)<br/>[Package SSIS](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu22-september-2020"></a><a id="CU22"></a> CU22 (septembre 2020)

Il s’agit de la mise en production de la mise à jour cumulative 22 (CU22) de [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. La version [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] de cette mise en production est 14.0.3356.20. Pour plus d’informations sur les correctifs et les améliorations de cette version, consultez <https://support.microsoft.com/help/4577467>.

### <a name="package-details"></a>Détails du package

Pour les installations du package manuelles ou hors connexion, vous pouvez télécharger les packages RPM et Debian avec les informations figurant dans la table suivante :

> [!NOTE]
> **Ubuntu 18.04** et **RHEL 8** sont maintenant pris en charge sur SQL Server 2017 à compter de CU20.
>
> Les liens d’installation du package hors connexion pour Ubuntu pointent vers les packages Ubuntu 18.04, à l’exception du package SSIS (qui n’est pas disponible pour Ubuntu 18.04). Si vous recherchez les packages Ubuntu 16.04, reportez-vous au chemin de téléchargement <https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/>.
>
> Les liens d’installation de package hors connexion pour Red Hat pointent vers des packages RHEL 8, à l’exception du package SSIS (qui n’est pas disponible pour RHEL 8). Si vous recherchez les packages RHEL 7, reportez-vous au chemin de téléchargement <https://packages.microsoft.com/rhel/7/mssql-server-2017/>.

| Package | Version du package | Téléchargements |
|-----|-----|-----|
| Package Red Hat RPM | 14.0.3356.20-23 | [Package RPM du moteur](https://packages.microsoft.com/rhel/8/mssql-server-2017/mssql-server-14.0.3356.20-23.x86_64.rpm)</br>[Package RPM haute disponibilité](https://packages.microsoft.com/rhel/8/mssql-server-2017/mssql-server-ha-14.0.3356.20-23.x86_64.rpm)</br>[Package RPM de recherche en texte intégral](https://packages.microsoft.com/rhel/8/mssql-server-2017/mssql-server-fts-14.0.3356.20-23.x86_64.rpm)</br>[Package SSIS](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| Package SLES RPM | 14.0.3356.20-23 | [Package RPM du moteur mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3356.20-23.x86_64.rpm)</br>[Package RPM haute disponibilité](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3356.20-23.x86_64.rpm)</br>[Package RPM de recherche en texte intégral](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3356.20-23.x86_64.rpm) | 
| Package Ubuntu 18.04 Debian | 14.0.3356.20-23 | [Package Debian du moteur](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3356.20-23_amd64.deb)</br>[Package Debian haute disponibilité](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3356.20-23_amd64.deb)</br>[Package Debian de recherche en texte intégral](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3356.20-23_amd64.deb)<br/>[Package SSIS](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu21-july-2020"></a><a id="CU21"></a> CU21 (juillet 2020)

Il s’agit de la mise en production de la mise à jour cumulative 21 (CU21) de [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. La version [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] de cette mise en production est 14.0.3335.7. Pour plus d’informations sur les correctifs et les améliorations de cette version, consultez <https://support.microsoft.com/help/4557397>.

### <a name="package-details"></a>Détails du package

Pour les installations du package manuelles ou hors connexion, vous pouvez télécharger les packages RPM et Debian avec les informations figurant dans la table suivante :

> [!NOTE]
> **Ubuntu 18.04** et **RHEL 8** sont maintenant pris en charge sur SQL Server 2017 à compter de CU20.
>
> Les liens d’installation du package hors connexion pour Ubuntu pointent vers les packages Ubuntu 18.04, à l’exception du package SSIS (qui n’est pas disponible pour Ubuntu 18.04). Si vous recherchez les packages Ubuntu 16.04, reportez-vous au chemin de téléchargement <https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/>.
>
> Les liens d’installation de package hors connexion pour Red Hat pointent vers des packages RHEL 8, à l’exception du package SSIS (qui n’est pas disponible pour RHEL 8). Si vous recherchez les packages RHEL 7, reportez-vous au chemin de téléchargement <https://packages.microsoft.com/rhel/7/mssql-server-2017/>.

| Package | Version du package | Téléchargements |
|-----|-----|-----|
| Package Red Hat RPM | 14.0.3335.7-17 | [Package RPM du moteur](https://packages.microsoft.com/rhel/8/mssql-server-2017/mssql-server-14.0.3335.7-17.x86_64.rpm)</br>[Package RPM haute disponibilité](https://packages.microsoft.com/rhel/8/mssql-server-2017/mssql-server-ha-14.0.3335.7-17.x86_64.rpm)</br>[Package RPM de recherche en texte intégral](https://packages.microsoft.com/rhel/8/mssql-server-2017/mssql-server-fts-14.0.3335.7-17.x86_64.rpm)</br>[Package SSIS](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| Package SLES RPM | 14.0.3335.7-17 | [Package RPM du moteur mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3335.7-17.x86_64.rpm)</br>[Package RPM haute disponibilité](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3335.7-17.x86_64.rpm)</br>[Package RPM de recherche en texte intégral](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3335.7-17.x86_64.rpm) | 
| Package Ubuntu 18.04 Debian | 14.0.3335.7-17 | [Package Debian du moteur](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3335.7-17_amd64.deb)</br>[Package Debian haute disponibilité](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3335.7-17_amd64.deb)</br>[Package Debian de recherche en texte intégral](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3335.7-17_amd64.deb)<br/>[Package SSIS](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu20-april-2020"></a><a id="CU20"></a> CU20 (avril 2020)

Il s’agit de la mise en production de la mise à jour cumulative 20 (CU20) de [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. La version [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] de cette mise en production est 14.0.3294.2. Pour plus d’informations sur les correctifs et les améliorations de cette version, consultez <https://support.microsoft.com/help/4541283>.

### <a name="package-details"></a>Détails du package

Pour les installations du package manuelles ou hors connexion, vous pouvez télécharger les packages RPM et Debian avec les informations figurant dans la table suivante :

> [!NOTE]
> **Ubuntu 18.04** et **RHEL 8** sont maintenant pris en charge sur SQL Server 2017 à compter de CU20.
>
> Les liens d’installation du package hors connexion pour Ubuntu pointent vers les packages Ubuntu 18.04, à l’exception du package SSIS (qui n’est pas disponible pour Ubuntu 18.04). Si vous recherchez les packages Ubuntu 16.04, reportez-vous au chemin de téléchargement <https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/>.
>
> Les liens d’installation de package hors connexion pour Red Hat pointent vers des packages RHEL 8, à l’exception du package SSIS (qui n’est pas disponible pour RHEL 8). Si vous recherchez les packages RHEL 7, reportez-vous au chemin de téléchargement <https://packages.microsoft.com/rhel/7/mssql-server-2017/>.

| Package | Version du package | Téléchargements |
|-----|-----|-----|
| Package Red Hat RPM | 14.0.3294.2-27 | [Package RPM du moteur](https://packages.microsoft.com/rhel/8/mssql-server-2017/mssql-server-14.0.3294.2-27.x86_64.rpm)</br>[Package RPM haute disponibilité](https://packages.microsoft.com/rhel/8/mssql-server-2017/mssql-server-ha-14.0.3294.2-27.x86_64.rpm)</br>[Package RPM de recherche en texte intégral](https://packages.microsoft.com/rhel/8/mssql-server-2017/mssql-server-fts-14.0.3294.2-27.x86_64.rpm)</br>[Package SSIS](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| Package SLES RPM | 14.0.3294.2-27 | [Package RPM du moteur mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3294.2-27.x86_64.rpm)</br>[Package RPM haute disponibilité](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3294.2-27.x86_64.rpm)</br>[Package RPM de recherche en texte intégral](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3294.2-27.x86_64.rpm) | 
| Package Ubuntu 18.04 Debian | 14.0.3294.2-27 | [Package Debian du moteur](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3294.2-27_amd64.deb)</br>[Package Debian haute disponibilité](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3294.2-27_amd64.deb)</br>[Package Debian de recherche en texte intégral](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3294.2-27_amd64.deb)<br/>[Package SSIS](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu19-february-2020"></a><a id="CU19"></a> CU19 (février 2020)

Il s’agit de la mise en production de la mise à jour cumulative 19 (CU19) de [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. La version [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] de cette mise en production est 14.0.3281.6. Pour plus d’informations sur les correctifs et les améliorations de cette mise en production, consultez [https://support.microsoft.com/help/4535007](https://support.microsoft.com/help/4535007).

### <a name="package-details"></a>Détails du package

Pour les installations du package manuelles ou hors connexion, vous pouvez télécharger les packages RPM et Debian avec les informations figurant dans la table suivante :

| Package | Version du package | Téléchargements |
|-----|-----|-----|
| Package Red Hat RPM | 14.0.3281.6-2 | [Package RPM du moteur](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3281.6-2.x86_64.rpm)</br>[Package RPM haute disponibilité](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3281.6-2.x86_64.rpm)</br>[Package RPM de recherche en texte intégral](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3281.6-2.x86_64.rpm)</br>[Package SSIS](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| Package SLES RPM | 14.0.3281.6-2 | [Package RPM du moteur mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3281.6-2.x86_64.rpm)</br>[Package RPM haute disponibilité](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3281.6-2.x86_64.rpm)</br>[Package RPM de recherche en texte intégral](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3281.6-2.x86_64.rpm) | 
| Package Ubuntu 16.04 Debian | 14.0.3281.6-2 | [Package Debian du moteur](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3281.6-2_amd64.deb)</br>[Package Debian haute disponibilité](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3281.6-2_amd64.deb)</br>[Package Debian de recherche en texte intégral](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3281.6-2_amd64.deb)<br/>[Package SSIS](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu18-december-2019"></a><a id="CU18"></a> CU18 (décembre 2019)

Il s’agit de la mise en production de la mise à jour cumulative 18 (CU18) de [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. La version [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] de cette mise en production est 14.0.3257.3. Pour plus d’informations sur les correctifs et les améliorations de cette mise en production, consultez [https://support.microsoft.com/help/4527377](https://support.microsoft.com/help/4527377).

### <a name="package-details"></a>Détails du package

Pour les installations du package manuelles ou hors connexion, vous pouvez télécharger les packages RPM et Debian avec les informations figurant dans la table suivante :

| Package | Version du package | Téléchargements |
|-----|-----|-----|
| Package Red Hat RPM | 14.0.3257.3-13 | [Package RPM du moteur](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3257.3-13.x86_64.rpm)</br>[Package RPM haute disponibilité](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3257.3-13.x86_64.rpm)</br>[Package RPM de recherche en texte intégral](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3257.3-13.x86_64.rpm)</br>[Package SSIS](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| Package SLES RPM | 14.0.3257.3-13 | [Package RPM du moteur mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3257.3-13.x86_64.rpm)</br>[Package RPM haute disponibilité](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3257.3-13.x86_64.rpm)</br>[Package RPM de recherche en texte intégral](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3257.3-13.x86_64.rpm) | 
| Package Ubuntu 16.04 Debian | 14.0.3257.3-13 | [Package Debian du moteur](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3257.3-13_amd64.deb)</br>[Package Debian haute disponibilité](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3257.3-13_amd64.deb)</br>[Package Debian de recherche en texte intégral](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3257.3-13_amd64.deb)<br/>[Package SSIS](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

### <a name="added-support"></a>Ajout de prise en charge

- La capture des changements de données (CDC) est prise en charge avec SQL Server 2017 sur Linux à partir de CU18.
- La réplication transactionnelle est prise en charge avec SQL Server 2017 sur Linux à partir de CU18.

### <a name="remarks"></a>Notes

Les conteneurs SQL Server 2017 disposent désormais d’un nouveau modèle d’étiquetage comme décrit ci-dessous à l’aide d’exemples.

- `mcr.microsoft.com/mssql/server:<SQL Server Version>-<update>-<Linux Distribution>-<Linux Distribution Version>`

  Ceci permet de tirer (pull) l’image conteneur avec la combinaison décrite dans l’étiquette.

- `mcr.microsoft.com/mssql/server:<SQL Server Version>-latest`

    Ceci permet de tirer (pull) la dernière version de SQL Server sur la dernière version d’Ubuntu prise en charge.

**Exemples :**

`mcr.microsoft.com/mssql/server:2017-CU18-ubuntu-16.04`

Ceci permet de tirer (pull) SQL Server 2017 CU18 en fonction du conteneur Ubuntu 16.04.

`mcr.microsoft.com/mssql/server:2017-latest`

Ceci permet de tirer (pull) la dernière version de SQL Server 2017 (CU18 au moment de la rédaction de cet article) en fonction du conteneur Ubuntu 16.04.

> [!NOTE]
> Nous ne publierons plus de conteneurs avec d’autres modèles d’étiquetage pour les conteneurs SQL Server 2017.


## <a name="cu17-october-2019"></a><a id="CU17"></a> CU17 (octobre 2019)

Il s’agit de la mise en production de la mise à jour cumulative 17 (CU17) de [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. La version [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] de cette mise en production est 14.0.3238.1. Pour plus d’informations sur les correctifs et les améliorations de cette mise en production, consultez [https://support.microsoft.com/help/4515579](https://support.microsoft.com/help/4515579).

### <a name="package-details"></a>Détails du package

Pour les installations du package manuelles ou hors connexion, vous pouvez télécharger les packages RPM et Debian avec les informations figurant dans la table suivante :

| Package | Version du package | Téléchargements |
|-----|-----|-----|
| Package Red Hat RPM | 14.0.3238.1-19 | [Package RPM du moteur](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3238.1-19.x86_64.rpm)</br>[Package RPM haute disponibilité](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3238.1-19.x86_64.rpm)</br>[Package RPM de recherche en texte intégral](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3238.1-19.x86_64.rpm)</br>[Package SSIS](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| Package SLES RPM | 14.0.3238.1-19 | [Package RPM du moteur mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3238.1-19.x86_64.rpm)</br>[Package RPM haute disponibilité](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3238.1-19.x86_64.rpm)</br>[Package RPM de recherche en texte intégral](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3238.1-19.x86_64.rpm) | 
| Package Ubuntu 16.04 Debian | 14.0.3238.1-19 | [Package Debian du moteur](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3238.1-19_amd64.deb)</br>[Package Debian haute disponibilité](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3238.1-19_amd64.deb)</br>[Package Debian de recherche en texte intégral](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3238.1-19_amd64.deb)<br/>[Package SSIS](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu16-august-2019"></a><a id="CU16"></a> CU16 (août 2019)

Il s’agit de la mise en production de la mise à jour cumulative 16 (CU16) de [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. La version [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] de cette mise en production est 14.0.3223.3. Pour plus d’informations sur les correctifs et les améliorations de cette mise en production, consultez [https://support.microsoft.com/help/4508218](https://support.microsoft.com/help/4508218).

### <a name="whats-new"></a>What's New

|Nouvelle fonctionnalité ou mise à jour | Détails |
|:---|:---|
| Support de MSDTC | Prise en charge de MSDTC (Microsoft Distributed Transaction Coordinator) pour SQL Server 2017. Pour plus d’informations, consultez [Comment configurer Microsoft Distributed Transaction Coordinator (MSDTC) sur Linux](sql-server-linux-configure-msdtc.md). |

### <a name="package-details"></a>Détails du package

Pour les installations du package manuelles ou hors connexion, vous pouvez télécharger les packages RPM et Debian avec les informations figurant dans la table suivante :

| Package | Version du package | Téléchargements |
|-----|-----|-----|
| Package Red Hat RPM | 14.0.3223.3-15 | [Package RPM du moteur](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3223.3-15.x86_64.rpm)</br>[Package RPM haute disponibilité](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3223.3-15.x86_64.rpm)</br>[Package RPM de recherche en texte intégral](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3223.3-15.x86_64.rpm)</br>[Package SSIS](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| Package SLES RPM | 14.0.3223.3-15 | [Package RPM du moteur mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3223.3-15.x86_64.rpm)</br>[Package RPM haute disponibilité](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3223.3-15.x86_64.rpm)</br>[Package RPM de recherche en texte intégral](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3223.3-15.x86_64.rpm) | 
| Package Ubuntu 16.04 Debian | 14.0.3223.3-15 | [Package Debian du moteur](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3223.3-15_amd64.deb)</br>[Package Debian haute disponibilité](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3223.3-15_amd64.deb)</br>[Package Debian de recherche en texte intégral](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3223.3-15_amd64.deb)<br/>[Package SSIS](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu15-may-2019"></a><a id="CU15"></a> CU15 (mai 2019)

Il s’agit de la mise en production de la mise à jour cumulative 15 (CU15) de [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. La version [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] de cette mise en production est 14.0.3162.1. Pour plus d’informations sur les correctifs et les améliorations de cette mise en production, consultez [https://support.microsoft.com/help/4498951](https://support.microsoft.com/help/4498951).

### <a name="package-details"></a>Détails du package

Pour les installations du package manuelles ou hors connexion, vous pouvez télécharger les packages RPM et Debian avec les informations figurant dans la table suivante :

| Package | Version du package | Téléchargements |
|-----|-----|-----|
| Package Red Hat RPM | 14.0.3162.1-1 | [Package RPM du moteur](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3162.1-1.x86_64.rpm)</br>[Package RPM haute disponibilité](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3162.1-1.x86_64.rpm)</br>[Package RPM de recherche en texte intégral](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3162.1-1.x86_64.rpm)</br>[Package SSIS](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| Package SLES RPM | 14.0.3162.1-1 | [Package RPM du moteur mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3162.1-1.x86_64.rpm)</br>[Package RPM haute disponibilité](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3162.1-1.x86_64.rpm)</br>[Package RPM de recherche en texte intégral](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3162.1-1.x86_64.rpm) | 
| Package Ubuntu 16.04 Debian | 14.0.3162.1-1 | [Package Debian du moteur](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3162.1-1_amd64.deb)</br>[Package Debian haute disponibilité](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3162.1-1_amd64.deb)</br>[Package Debian de recherche en texte intégral](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3162.1-1_amd64.deb)<br/>[Package SSIS](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu14-mar-2019"></a><a id="CU14"></a> CU14 (mar 2019)

Il s’agit de la mise en production de la mise à jour cumulative 14 (CU14) de [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. La version [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] de cette mise en production est 14.0.3076.1. Pour plus d’informations sur les correctifs et les améliorations de cette mise en production, consultez [https://support.microsoft.com/help/4484710](https://support.microsoft.com/help/4484710).

### <a name="package-details"></a>Détails du package

Pour les installations du package manuelles ou hors connexion, vous pouvez télécharger les packages RPM et Debian avec les informations figurant dans la table suivante :

| Package | Version du package | Téléchargements |
|-----|-----|-----|
| Package Red Hat RPM | 14.0.3076.1-2 | [Package RPM du moteur](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3076.1-2.x86_64.rpm)</br>[Package RPM haute disponibilité](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3076.1-2.x86_64.rpm)</br>[Package RPM de recherche en texte intégral](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3076.1-2.x86_64.rpm)</br>[Package SSIS](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| Package SLES RPM | 14.0.3076.1-2 | [Package RPM du moteur mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3076.1-2.x86_64.rpm)</br>[Package RPM haute disponibilité](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3076.1-2.x86_64.rpm)</br>[Package RPM de recherche en texte intégral](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3076.1-2.x86_64.rpm) | 
| Package Ubuntu 16.04 Debian | 14.0.3076.1-2 | [Package Debian du moteur](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3076.1-2_amd64.deb)</br>[Package Debian haute disponibilité](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3076.1-2_amd64.deb)</br>[Package Debian de recherche en texte intégral](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3076.1-2_amd64.deb)<br/>[Package SSIS](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu13-dec-2018"></a><a id="CU13"></a> CU13 (déc 2018)

Il s’agit de la mise en production de la mise à jour cumulative 13 (CU13) de [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. La version [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] de cette mise en production est 14.0.3048.4. Pour plus d’informations sur les correctifs et les améliorations de cette mise en production, consultez [https://support.microsoft.com/help/4466404](https://support.microsoft.com/help/4466404).

### <a name="package-details"></a>Détails du package

Pour les installations du package manuelles ou hors connexion, vous pouvez télécharger les packages RPM et Debian avec les informations figurant dans la table suivante :

| Package | Version du package | Téléchargements |
|-----|-----|-----|
| Package Red Hat RPM | 14.0.3048.4-1 | [Package RPM du moteur](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3048.4-1.x86_64.rpm)</br>[Package RPM haute disponibilité](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3048.4-1.x86_64.rpm)</br>[Package RPM de recherche en texte intégral](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3048.4-1.x86_64.rpm)</br>[Package SSIS](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| Package SLES RPM | 14.0.3048.4-1 | [Package RPM du moteur mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3048.4-1.x86_64.rpm)</br>[Package RPM haute disponibilité](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3048.4-1.x86_64.rpm)</br>[Package RPM de recherche en texte intégral](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3048.4-1.x86_64.rpm) | 
| Package Ubuntu 16.04 Debian | 14.0.3048.4-1 | [Package Debian du moteur](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3048.4-1_amd64.deb)</br>[Package Debian haute disponibilité](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3048.4-1_amd64.deb)</br>[Package Debian de recherche en texte intégral](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3048.4-1_amd64.deb)<br/>[Package SSIS](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu12-oct-2018"></a><a id="CU12"></a> CU12 (oct 2018)

Il s’agit de la mise en production de la mise à jour cumulative 12 (CU12) de [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. La version [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] de cette mise en production est 14.0.3045.24. Pour plus d’informations sur les correctifs et les améliorations de cette mise en production, consultez [https://support.microsoft.com/help/4464082](https://support.microsoft.com/help/4464082).

### <a name="package-details"></a>Détails du package

Pour les installations du package manuelles ou hors connexion, vous pouvez télécharger les packages RPM et Debian avec les informations figurant dans la table suivante :

| Package | Version du package | Téléchargements |
|-----|-----|-----|
| Package Red Hat RPM | 14.0.3045.24-1 | [Package RPM du moteur](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3045.24-1.x86_64.rpm)</br>[Package RPM haute disponibilité](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3045.24-1.x86_64.rpm)</br>[Package RPM de recherche en texte intégral](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3045.24-1.x86_64.rpm)</br>[Package SSIS](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| Package SLES RPM | 14.0.3045.24-1 | [Package RPM du moteur mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3045.24-1.x86_64.rpm)</br>[Package RPM haute disponibilité](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3045.24-1.x86_64.rpm)</br>[Package RPM de recherche en texte intégral](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3045.24-1.x86_64.rpm) | 
| Package Ubuntu 16.04 Debian | 14.0.3045.24-1 | [Package Debian du moteur](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3045.24-1_amd64.deb)</br>[Package Debian haute disponibilité](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3045.24-1_amd64.deb)</br>[Package Debian de recherche en texte intégral](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3045.24-1_amd64.deb)<br/>[Package SSIS](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu11-sept-2018"></a><a id="CU11"></a> CU11 (sept 2018)

Il s’agit de la mise en production de la mise à jour cumulative 11 (CU11) de [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. La version [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] de cette mise en production est 14.0.3038.14. Pour plus d’informations sur les correctifs et les améliorations de cette mise en production, consultez [https://support.microsoft.com/help/4462262](https://support.microsoft.com/help/4462262).

### <a name="package-details"></a>Détails du package

Pour les installations du package manuelles ou hors connexion, vous pouvez télécharger les packages RPM et Debian avec les informations figurant dans la table suivante :

| Package | Version du package | Téléchargements |
|-----|-----|-----|
| Package Red Hat RPM | 14.0.3038.14-2 | [Package RPM du moteur](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3038.14-2.x86_64.rpm)</br>[Package RPM haute disponibilité](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3038.14-2.x86_64.rpm)</br>[Package RPM de recherche en texte intégral](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3038.14-2.x86_64.rpm)</br>[Package SSIS](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| Package SLES RPM | 14.0.3038.14-2 | [Package RPM du moteur mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3038.14-2.x86_64.rpm)</br>[Package RPM haute disponibilité](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3038.14-2.x86_64.rpm)</br>[Package RPM de recherche en texte intégral](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3038.14-2.x86_64.rpm) | 
| Package Ubuntu 16.04 Debian | 14.0.3038.14-2 | [Package Debian du moteur](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3038.14-2_amd64.deb)</br>[Package Debian haute disponibilité](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3038.14-2_amd64.deb)</br>[Package Debian de recherche en texte intégral](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3038.14-2_amd64.deb)<br/>[Package SSIS](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu10-aug-2018"></a><a id="CU10"></a> CU10 (août 2018)

Il s’agit de la mise en production de la mise à jour cumulative 10 (CU10) de [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. La version [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] de cette mise en production est 14.0.3037.1. Pour plus d’informations sur les correctifs et les améliorations de cette mise en production, consultez [https://support.microsoft.com/help/4342123](https://support.microsoft.com/help/4342123).

### <a name="package-details"></a>Détails du package

Pour les installations du package manuelles ou hors connexion, vous pouvez télécharger les packages RPM et Debian avec les informations figurant dans la table suivante :

| Package | Version du package | Téléchargements |
|-----|-----|-----|
| Package Red Hat RPM | 14.0.3037.1-2 | [Package RPM du moteur](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3037.1-2.x86_64.rpm)</br>[Package RPM haute disponibilité](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3037.1-2.x86_64.rpm)</br>[Package RPM de recherche en texte intégral](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3037.1-2.x86_64.rpm)</br>[Package SSIS](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| Package SLES RPM | 14.0.3037.1-2 | [Package RPM du moteur mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3037.1-2.x86_64.rpm)</br>[Package RPM haute disponibilité](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3037.1-2.x86_64.rpm)</br>[Package RPM de recherche en texte intégral](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3037.1-2.x86_64.rpm) | 
| Package Ubuntu 16.04 Debian | 14.0.3037.1-2 | [Package Debian du moteur](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3037.1-2_amd64.deb)</br>[Package Debian haute disponibilité](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3037.1-2_amd64.deb)</br>[Package Debian de recherche en texte intégral](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3037.1-2_amd64.deb)<br/>[Package SSIS](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu9-gdr2-aug-2018"></a><a id="CU9-GDR2"></a> CU9-GDR2 (août 2018)

Il s’agit d’une mise à jour de sécurité qui comprend également la CU mise en production précédemment (CU9) pour [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. La version [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] de cette mise en production est 14.0.3035.2. Pour plus d’informations sur les correctifs et les améliorations de cette mise en production, consultez [https://support.microsoft.com/help/4293805](https://support.microsoft.com/help/4293805).

### <a name="package-details"></a>Détails du package

Pour les installations du package manuelles ou hors connexion, vous pouvez télécharger les packages RPM et Debian avec les informations figurant dans la table suivante :

| Package | Version du package | Téléchargements |
|-----|-----|-----|
| Package Red Hat RPM | 14.0.3035.2-1 | [Package RPM du moteur](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3035.2-1.x86_64.rpm)</br>[Package RPM haute disponibilité](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3035.2-1.x86_64.rpm)</br>[Package RPM de recherche en texte intégral](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3035.2-1.x86_64.rpm)| 
| Package SLES RPM | 14.0.3035.2-1 | [Package RPM du moteur mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3035.2-1.x86_64.rpm)</br>[Package RPM haute disponibilité](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3035.2-1.x86_64.rpm)</br>[Package RPM de recherche en texte intégral](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3035.2-1.x86_64.rpm) | 
| Package Ubuntu 16.04 Debian | 14.0.3035.2-1 | [Package Debian du moteur](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3035.2-1_amd64.deb)</br>[Package Debian haute disponibilité](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3035.2-1_amd64.deb)</br>[Package Debian de recherche en texte intégral](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3035.2-1_amd64.deb)<br/> |

## <a name="gdr2-aug-2018"></a><a id="GDR2"></a> GDR2 (août 2018)

Il s’agit d’une mise à jour de sécurité qui comprend uniquement les correctifs de sécurité GDR2 (et GDR1) pour [!INCLUDE[ssSQL17](../includes/sssql17-md.md)].  La version [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] de cette mise en production est 14.0.2002.14. Pour plus d’informations sur les correctifs et les améliorations de cette mise en production, consultez [https://support.microsoft.com/help/4293803](https://support.microsoft.com/help/4293803).

### <a name="package-details"></a>Détails du package

Pour les installations du package manuelles ou hors connexion, vous pouvez télécharger les packages RPM et Debian avec les informations figurant dans la table suivante :

| Package | Version du package | Téléchargements |
|-----|-----|-----|
| Package Red Hat RPM | 14.0.2002.14-1 | [Package RPM du moteur](https://packages.microsoft.com/rhel/7/mssql-server-2017-gdr/mssql-server-14.0.2002.14-1.x86_64.rpm)</br>[Package RPM haute disponibilité](https://packages.microsoft.com/rhel/7/mssql-server-2017-gdr/mssql-server-ha-14.0.2002.14-1.x86_64.rpm)</br>[Package RPM de recherche en texte intégral](https://packages.microsoft.com/rhel/7/mssql-server-2017-gdr/mssql-server-fts-14.0.2002.14-1.x86_64.rpm) | 
| Package SLES RPM | 14.0.2002.14-1 | [Package RPM du moteur mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2017-gdr/mssql-server-14.0.2002.14-1.x86_64.rpm)</br>[Package RPM haute disponibilité](https://packages.microsoft.com/sles/12/mssql-server-2017-gdr/mssql-server-ha-14.0.2002.14-1.x86_64.rpm)</br>[Package RPM de recherche en texte intégral](https://packages.microsoft.com/sles/12/mssql-server-2017-gdr/mssql-server-fts-14.0.2002.14-1.x86_64.rpm) | 
| Package Ubuntu 16.04 Debian | 14.0.2002.14-1 | [Package Debian du moteur](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017-gdr/pool/main/m/mssql-server/mssql-server_14.0.2002.14-1_amd64.deb)</br>[Package Debian haute disponibilité](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017-gdr/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.2002.14-1_amd64.deb)</br>[Package Debian de recherche en texte intégral](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017-gdr/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.2002.14-1_amd64.deb) |

## <a name="cu9-jul-2018"></a><a id="CU9"></a> CU9 (juil 2018)

Il s’agit de la mise en production de la mise à jour cumulative 9 (CU9) de [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. La version [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] de cette mise en production est 14.0.3030.27. Pour plus d’informations sur les correctifs et les améliorations de cette mise en production, consultez [https://support.microsoft.com/help/4341265](https://support.microsoft.com/help/4341265).

### <a name="package-details"></a>Détails du package

Pour les installations du package manuelles ou hors connexion, vous pouvez télécharger les packages RPM et Debian avec les informations figurant dans la table suivante :

| Package | Version du package | Téléchargements |
|-----|-----|-----|
| Package Red Hat RPM | 14.0.3030.27-1 | [Package RPM du moteur](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3030.27-1.x86_64.rpm)</br>[Package RPM haute disponibilité](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3030.27-1.x86_64.rpm)</br>[Package RPM de recherche en texte intégral](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3030.27-1.x86_64.rpm)</br>[Package SSIS](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| Package SLES RPM | 14.0.3030.27-1 | [Package RPM du moteur mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3030.27-1.x86_64.rpm)</br>[Package RPM haute disponibilité](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3030.27-1.x86_64.rpm)</br>[Package RPM de recherche en texte intégral](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3030.27-1.x86_64.rpm) | 
| Package Ubuntu 16.04 Debian | 14.0.3030.27-1 | [Package Debian du moteur](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3030.27-1_amd64.deb)</br>[Package Debian haute disponibilité](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3030.27-1_amd64.deb)</br>[Package Debian de recherche en texte intégral](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3030.27-1_amd64.deb)<br/>[Package SSIS](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu8-jun-2018"></a><a id="CU8"></a> CU8 (juin 2018)

Il s’agit de la mise en production de la mise à jour cumulative 8 (CU8) de [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. La version [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] de cette mise en production est 14.0.3029.16. Pour plus d’informations sur les correctifs et les améliorations de cette mise en production, consultez [https://support.microsoft.com/help/4338363](https://support.microsoft.com/help/4338363).

### <a name="package-details"></a>Détails du package

Pour les installations du package manuelles ou hors connexion, vous pouvez télécharger les packages RPM et Debian avec les informations figurant dans la table suivante :

| Package | Version du package | Téléchargements |
|-----|-----|-----|
| Package Red Hat RPM | 14.0.3029.16-1 | [Package RPM du moteur](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3029.16-1.x86_64.rpm)</br>[Package RPM haute disponibilité](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3029.16-1.x86_64.rpm)</br>[Package RPM de recherche en texte intégral](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3029.16-1.x86_64.rpm)</br>[Package SSIS](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| Package SLES RPM | 14.0.3029.16-1 | [Package RPM du moteur mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3029.16-1.x86_64.rpm)</br>[Package RPM haute disponibilité](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3029.16-1.x86_64.rpm)</br>[Package RPM de recherche en texte intégral](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3029.16-1.x86_64.rpm) | 
| Package Ubuntu 16.04 Debian | 14.0.3029.16-1 | [Package Debian du moteur](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3029.16-1_amd64.deb)</br>[Package Debian haute disponibilité](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3029.16-1_amd64.deb)</br>[Package Debian de recherche en texte intégral](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3029.16-1_amd64.deb)<br/>[Package SSIS](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu7-may-2018"></a><a id="CU7"></a> CU7 (mai 2018)

Il s’agit de la mise en production de la mise à jour cumulative 7 (CU7) de [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. La version [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] de cette mise en production est 14.0.3026.27. Pour plus d’informations sur les correctifs et les améliorations de cette mise en production, consultez [https://support.microsoft.com/help/4229789](https://support.microsoft.com/help/4229789).

### <a name="package-details"></a>Détails du package

Pour les installations du package manuelles ou hors connexion, vous pouvez télécharger les packages RPM et Debian avec les informations figurant dans la table suivante :

| Package | Version du package | Téléchargements |
|-----|-----|-----|
| Package Red Hat RPM | 14.0.3026.27-2 | [Package RPM du moteur](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3026.27-2.x86_64.rpm)</br>[Package RPM haute disponibilité](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3026.27-2.x86_64.rpm)</br>[Package RPM de recherche en texte intégral](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3026.27-2.x86_64.rpm)</br>[Package SSIS](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| Package SLES RPM | 14.0.3026.27-2 | [Package RPM du moteur mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3026.27-2.x86_64.rpm)</br>[Package RPM haute disponibilité](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3026.27-2.x86_64.rpm)</br>[Package RPM de recherche en texte intégral](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3026.27-2.x86_64.rpm) | 
| Package Ubuntu 16.04 Debian | 14.0.3026.27-2 | [Package Debian du moteur](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3026.27-2_amd64.deb)</br>[Package Debian haute disponibilité](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3026.27-2_amd64.deb)</br>[Package Debian de recherche en texte intégral](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3026.27-2_amd64.deb)<br/>[Package SSIS](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu6-apr-2018"></a><a id="CU6"></a> CU6 (avr 2018)

Il s’agit de la mise en production de la mise à jour cumulative 6 (CU6) de [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. La version [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] de cette mise en production est 14.0.3025.34. Pour plus d’informations sur les correctifs et les améliorations de cette mise en production, consultez [https://support.microsoft.com/help/4101464](https://support.microsoft.com/help/4101464).

### <a name="package-details"></a>Détails du package

Pour les installations du package manuelles ou hors connexion, vous pouvez télécharger les packages RPM et Debian avec les informations figurant dans la table suivante :

| Package | Version du package | Téléchargements |
|-----|-----|-----|
| Package Red Hat RPM | 14.0.3025.34-3 | [Package RPM du moteur](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3025.34-3.x86_64.rpm)</br>[Package RPM haute disponibilité](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3025.34-3.x86_64.rpm)</br>[Package RPM de recherche en texte intégral](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3025.34-3.x86_64.rpm)</br>[Package SSIS](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| Package SLES RPM | 14.0.3025.34-3 | [Package RPM du moteur mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3025.34-3.x86_64.rpm)</br>[Package RPM haute disponibilité](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3025.34-3.x86_64.rpm)</br>[Package RPM de recherche en texte intégral](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3025.34-3.x86_64.rpm) | 
| Package Ubuntu 16.04 Debian | 14.0.3025.34-3 | [Package Debian du moteur](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3025.34-3_amd64.deb)</br>[Package Debian haute disponibilité](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3025.34-3_amd64.deb)</br>[Package Debian de recherche en texte intégral](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3025.34-3_amd64.deb)<br/>[Package SSIS](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu5-mar-2018"></a><a id="CU5"></a> CU5 (mar 2018)

Il s’agit de la mise en production de la mise à jour cumulative 5 (CU5) de [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. La version [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] de cette mise en production est 14.0.3023.8. Pour plus d’informations sur les correctifs et les améliorations de cette mise en production, consultez [https://support.microsoft.com/help/4092643](https://support.microsoft.com/help/4092643).

### <a name="known-upgrade-issue"></a>Problème de mise à niveau connu

Lorsque vous effectuez une mise à niveau à partir d'une mise en production précédente vers CU5, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] risque de ne pas démarrer avec l’erreur suivante :

```
Error: 4860, Severity: 16, State: 1.
Cannot bulk load. The file "C:\Install\SqlTraceCollect.dtsx" does not exist or you don't have file access rights.
Error: 912, Severity: 21, State: 2.
Script level upgrade for database 'master' failed because upgrade step 'msdb110_upgrade.sql' encountered error 200, state
```

Pour résoudre cette erreur, activez SQL Server agent et redémarrez [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] avec les commandes suivantes :

```bash
sudo /opt/mssql/bin/mssql-conf set sqlagent.enabled true
sudo systemctl start mssql-server
```

### <a name="package-details"></a>Détails du package

Pour les installations du package manuelles ou hors connexion, vous pouvez télécharger les packages RPM et Debian avec les informations figurant dans la table suivante :

| Package | Version du package | Téléchargements |
|-----|-----|-----|
| Package Red Hat RPM | 14.0.3023.8-5 | [Package RPM du moteur](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3023.8-5.x86_64.rpm)</br>[Package RPM haute disponibilité](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3023.8-5.x86_64.rpm)</br>[Package RPM de recherche en texte intégral](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3023.8-5.x86_64.rpm)</br>[Package SSIS](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| Package SLES RPM | 14.0.3023.8-5 | [Package RPM du moteur mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3023.8-5.x86_64.rpm)</br>[Package RPM haute disponibilité](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3023.8-5.x86_64.rpm)</br>[Package RPM de recherche en texte intégral](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3023.8-5.x86_64.rpm) | 
| Package Ubuntu 16.04 Debian | 14.0.3023.8-5 | [Package Debian du moteur](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3023.8-5_amd64.deb)</br>[Package Debian haute disponibilité](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3023.8-5_amd64.deb)</br>[Package Debian de recherche en texte intégral](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3023.8-5_amd64.deb)<br/>[Package SSIS](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu4-feb-2018"></a><a id="CU4"></a> CU4 (fév 2018)

Il s’agit de la mise en production de la mise à jour cumulative 4 (CU4) de [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. La version [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] de cette mise en production est 14.0.3022.28. Pour plus d’informations sur les correctifs et les améliorations de cette mise en production, consultez [https://support.microsoft.com/help/4056498](https://support.microsoft.com/help/4056498).

### <a name="package-details"></a>Détails du package

Pour les installations du package manuelles ou hors connexion, vous pouvez télécharger les packages RPM et Debian avec les informations figurant dans la table suivante :

> [!NOTE]
> À partir de CU4, SQL Server Agent n’est plus installé en tant que package distinct. Il est installé avec le package du moteur et doit être activé pour pouvoir être utilisé.

| Package | Version du package | Téléchargements |
|-----|-----|-----|
| Package Red Hat RPM | 14.0.3022.28-2 | [Package RPM du moteur](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3022.28-2.x86_64.rpm)</br>[Package RPM haute disponibilité](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3022.28-2.x86_64.rpm)</br>[Package RPM de recherche en texte intégral](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3022.28-2.x86_64.rpm)</br>[Package SSIS](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| Package SLES RPM | 14.0.3022.28-2 | [Package RPM du moteur mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3022.28-2.x86_64.rpm)</br>[Package RPM haute disponibilité](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3022.28-2.x86_64.rpm)</br>[Package RPM de recherche en texte intégral](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3022.28-2.x86_64.rpm) | 
| Package Ubuntu 16.04 Debian | 14.0.3022.28-2 | [Package Debian du moteur](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3022.28-2_amd64.deb)</br>[Package Debian haute disponibilité](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3022.28-2_amd64.deb)</br>[Package Debian de recherche en texte intégral](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3022.28-2_amd64.deb)<br/>[Package SSIS](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="gdr1-jan-2018"></a><a id="GDR1"></a> GDR1 (jan 2018)

Il s’agit d’une mise à jour de sécurité qui comprend uniquement les correctifs de sécurité GDR1 pour [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. La version [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] de cette mise en production est 14.0.2000.63. Pour plus d’informations sur les correctifs et les améliorations de cette mise en production, consultez [https://support.microsoft.com/help/4057122](https://support.microsoft.com/help/4057122).

### <a name="package-details"></a>Détails du package

Pour les installations du package manuelles ou hors connexion, vous pouvez télécharger les packages RPM et Debian avec les informations figurant dans la table suivante :

| Package | Version du package | Téléchargements |
|-----|-----|-----|
| Package Red Hat RPM | 14.0.2000.63-3 | [Package RPM du moteur](https://packages.microsoft.com/rhel/7/mssql-server-2017-gdr/mssql-server-14.0.2000.63-3.x86_64.rpm)</br>[Package RPM haute disponibilité](https://packages.microsoft.com/rhel/7/mssql-server-2017-gdr/mssql-server-ha-14.0.2000.63-3.x86_64.rpm)</br>[Package RPM de recherche en texte intégral](https://packages.microsoft.com/rhel/7/mssql-server-2017-gdr/mssql-server-fts-14.0.2000.63-3.x86_64.rpm) | 
| Package SLES RPM | 14.0.2000.63-3 | [Package RPM du moteur mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2017-gdr/mssql-server-14.0.2000.63-3.x86_64.rpm)</br>[Package RPM haute disponibilité](https://packages.microsoft.com/sles/12/mssql-server-2017-gdr/mssql-server-ha-14.0.2000.63-3.x86_64.rpm)</br>[Package RPM de recherche en texte intégral](https://packages.microsoft.com/sles/12/mssql-server-2017-gdr/mssql-server-fts-14.0.2000.63-3.x86_64.rpm) | 
| Package Ubuntu 16.04 Debian | 14.0.2000.63-3 | [Package Debian du moteur](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017-gdr/pool/main/m/mssql-server/mssql-server_14.0.2000.63-3_amd64.deb)</br>[Package Debian haute disponibilité](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017-gdr/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.2000.63-3_amd64.deb)</br>[Package Debian de recherche en texte intégral](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017-gdr/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.2000.63-3_amd64.deb) |

## <a name="cu3-jan-2018"></a><a id="CU3"></a> CU3 (jan 2018)

Il s’agit de la mise en production de la mise à jour cumulative 3 (CU3) de [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. La version [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] de cette mise en production est 14.0.3015.40. Pour plus d’informations sur les correctifs et les améliorations de cette mise en production, consultez [https://support.microsoft.com/help/4052987](https://support.microsoft.com/help/4052987).

### <a name="package-details"></a>Détails du package

Pour les installations du package manuelles ou hors connexion, vous pouvez télécharger les packages RPM et Debian avec les informations figurant dans la table suivante :

| Package | Version du package | Téléchargements |
|-----|-----|-----|
| Package Red Hat RPM | 14.0.3015.40-1 | [Package RPM du moteur](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3015.40-1.x86_64.rpm)</br>[Package RPM haute disponibilité](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3015.40-1.x86_64.rpm)</br>[Package RPM de recherche en texte intégral](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3015.40-1.x86_64.rpm)</br>[Package RPM SQL Server Agent](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-agent-14.0.3015.40-1.x86_64.rpm)</br>[Package SSIS](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| Package SLES RPM | 14.0.3015.40-1 | [Package RPM du moteur mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3015.40-1.x86_64.rpm)</br>[Package RPM haute disponibilité](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3015.40-1.x86_64.rpm)</br>[Package RPM de recherche en texte intégral](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3015.40-1.x86_64.rpm)</br>[Package RPM SQL Server Agent](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-agent-14.0.3015.40-1.x86_64.rpm) | 
| Package Ubuntu 16.04 Debian | 14.0.3015.40-1 | [Package Debian du moteur](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3015.40-1_amd64.deb)</br>[Package Debian haute disponibilité](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3015.40-1_amd64.deb)</br>[Package Debian de recherche en texte intégral](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3015.40-1_amd64.deb)</br>[Package Debian SQL Server Agent](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-agent/mssql-server-agent_14.0.3015.40-1_amd64.deb)<br/>[Package SSIS](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu2-nov-2017"></a><a id="CU2"></a> CU2 (nov 2017)

Il s’agit de la mise en production de la mise à jour cumulative 2 (CU2) de [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. La version [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] de cette mise en production est 14.0.3008.27. Pour plus d’informations sur les correctifs et les améliorations de cette mise en production, consultez [https://support.microsoft.com/help/4052574](https://support.microsoft.com/help/4052574).

### <a name="package-details"></a>Détails du package

Pour les installations du package manuelles ou hors connexion, vous pouvez télécharger les packages RPM et Debian avec les informations figurant dans la table suivante :

| Package | Version du package | Téléchargements |
|-----|-----|-----|
| Package Red Hat RPM | 14.0.3008.27-1 | [Package RPM du moteur](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3008.27-1.x86_64.rpm)</br>[Package RPM haute disponibilité](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3008.27-1.x86_64.rpm)</br>[Package RPM de recherche en texte intégral](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3008.27-1.x86_64.rpm)</br>[Package RPM SQL Server Agent](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-agent-14.0.3008.27-1.x86_64.rpm)</br>[Package SSIS](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| Package SLES RPM | 14.0.3008.27-1 | [Package RPM du moteur mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3008.27-1.x86_64.rpm)</br>[Package RPM haute disponibilité](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3008.27-1.x86_64.rpm)</br>[Package RPM de recherche en texte intégral](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3008.27-1.x86_64.rpm)</br>[Package RPM SQL Server Agent](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-agent-14.0.3008.27-1.x86_64.rpm) | 
| Package Ubuntu 16.04 Debian | 14.0.3008.27-1 | [Package Debian du moteur](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3008.27-1_amd64.deb)</br>[Package Debian haute disponibilité](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3008.27-1_amd64.deb)</br>[Package Debian de recherche en texte intégral](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3008.27-1_amd64.deb)</br>[Package Debian SQL Server Agent](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-agent/mssql-server-agent_14.0.3008.27-1_amd64.deb)<br/>[Package SSIS](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu1-oct-2017"></a><a id="CU1"></a> CU1 (oct 2017)

Il s’agit de la mise en production de la mise à jour cumulative 1 (CU1) de [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. La version [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] de cette mise en production est 14.0.3006.16. Pour plus d’informations sur les correctifs et les améliorations de cette mise en production, consultez [https://support.microsoft.com/help/KB4053439](https://support.microsoft.com/help/4038634).

### <a name="package-details"></a>Détails du package

Pour les installations du package manuelles ou hors connexion, vous pouvez télécharger les packages RPM et Debian avec les informations figurant dans la table suivante :

| Package | Version du package | Téléchargements |
|-----|-----|-----|
| Package Red Hat RPM | 14.0.3006.16-3 | [Package RPM du moteur](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3006.16-3.x86_64.rpm)</br>[Package RPM haute disponibilité](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3006.16-3.x86_64.rpm)</br>[Package RPM de recherche en texte intégral](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3006.16-3.x86_64.rpm)</br>[Package RPM SQL Server Agent](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-agent-14.0.3006.16-3.x86_64.rpm)</br>[Package SSIS](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| Package SLES RPM | 14.0.3006.16-3 | [Package RPM du moteur mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3006.16-3.x86_64.rpm)</br>[Package RPM haute disponibilité](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3006.16-3.x86_64.rpm)</br>[Package RPM de recherche en texte intégral](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3006.16-3.x86_64.rpm)</br>[Package RPM SQL Server Agent](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-agent-14.0.3006.16-3.x86_64.rpm) | 
| Package Ubuntu 16.04 Debian | 14.0.3006.16-3 | [Package Debian du moteur](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3006.16-3_amd64.deb)</br>[Package Debian haute disponibilité](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3006.16-3_amd64.deb)</br>[Package Debian de recherche en texte intégral](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3006.16-3_amd64.deb)</br>[Package Debian SQL Server Agent](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-agent/mssql-server-agent_14.0.3006.16-3_amd64.deb)<br/>[Package SSIS](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="ga-oct-2017"></a><a id="GA"></a> GA (oct 2017)

Il s’agit de la mise en production en disponibilité générale (DG) de [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. La version [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] de cette mise en production est 14.0.1000.169.

### <a name="package-details"></a>Détails du package

Les détails et les emplacements de téléchargement des packages RPM et Debian sont répertoriés dans la table suivante. Vous n’avez pas besoin de télécharger ces packages directement si vous suivez les étapes décrites dans les guides d’installation suivants :

- [Installer un package SQL Server](sql-server-linux-setup.md)
- [Installer un package de recherche en texte intégral](sql-server-linux-setup-full-text-search.md)
- [Installer un package SQL Server Agent](sql-server-linux-setup-sql-agent.md)
- [Installer SQL Server Integration Services](sql-server-linux-setup-ssis.md)

| Package | Version du package | Téléchargements |
|-----|-----|-----|
| Package Red Hat RPM | 14.0.1000.169-2 | [Package RPM du moteur](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.1000.169-2.x86_64.rpm)</br>[Package RPM haute disponibilité](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.1000.169-2.x86_64.rpm)</br>[Package RPM de recherche en texte intégral](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.1000.169-2.x86_64.rpm)</br>[Package RPM SQL Server Agent](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-agent-14.0.1000.169-2.x86_64.rpm)</br>[Package SSIS](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| Package SLES RPM | 14.0.1000.169-2 | [Package RPM du moteur mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.1000.169-2.x86_64.rpm)</br>[Package RPM haute disponibilité](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.1000.169-2.x86_64.rpm)</br>[Package RPM de recherche en texte intégral](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.1000.169-2.x86_64.rpm)</br>[Package RPM SQL Server Agent](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-agent-14.0.1000.169-2.x86_64.rpm) | 
| Package Ubuntu 16.04 Debian | 14.0.1000.169-2 | [Package Debian du moteur](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.1000.169-2_amd64.deb)</br>[Package Debian haute disponibilité](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.1000.169-2_amd64.deb)</br>[Package Debian de recherche en texte intégral](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.1000.169-2_amd64.deb)</br>[Package Debian SQL Server Agent](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-agent/mssql-server-agent_14.0.1000.169-2_amd64.deb)<br/>[Package SSIS](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="known-issues"></a>Problèmes connus

Les sections suivantes décrivent les problèmes connus avec la mise en production en disponibilité générale de [!INCLUDE[ssSQL17](../includes/sssql17-md.md)] sur Linux.

#### <a name="general"></a>Général

- La longueur du nom d’hôte dans lequel [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] est installé doit être inférieure ou égale à 15 caractères. 

    - **Résolution** : Modifiez le nom dans /etc/nom d’hôte en lui attribuant une longueur de 15 caractères ou moins.

- Le paramétrage manuel de l’heure système à rebours entraînera l’arrêt de la mise à jour de l’heure système interne par [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] dans [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].

    - **Résolution** : Redémarrez [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].

- Seules les installations à instance unique sont prises en charge.

    - **Résolution** : Si vous souhaitez avoir plusieurs instances sur un hôte donné, envisagez d’utiliser des machines virtuelles ou des conteneurs Docker. 

- [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Configuration Manager ne peut pas se connecter à [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] sur Linux.

- La langue par défaut de la connexion **sa** est l’anglais.

    - **Résolution** : Modifiez la langue de la connexion **sa** à l'aide de l’instruction **ALTER LOGIN**.

#### <a name="databases"></a>Bases de données

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

- Les bases de données [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] sur Windows qui utilisent l’OLTP en mémoire ne peuvent pas être restaurées sur [!INCLUDE[ssSQL17](../includes/sssql17-md.md)] sur Linux. Pour restaurer une base de données [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] qui utilise l’OLTP en mémoire, commencez par mettre à niveau les bases de données vers [!INCLUDE[sssql15-md](../includes/sssql16-md.md)] ou [!INCLUDE[ssSQL17](../includes/sssql17-md.md)] sur Windows avant de les déplacer vers [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] sur Linux via la sauvegarde/restauration ou le détachement/attachement.

- L’autorisation utilisateur **ADMINISTER BULK OPERATIONS** n’est pas prise en charge sur Linux pour l’instant.

#### <a name="networking"></a>Mise en réseau

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

- Utilisez la version **4.2 ou ultérieure** de NFS. Les versions antérieures de NFS ne prennent pas en charge les fonctionnalités requises, telles que la création de fichiers alloués et partiellement alloués, communs aux systèmes de fichiers modernes.
- Localisez uniquement les répertoires **/var/opt/mssql** sur le montage NFS. D’autres fichiers, tels que les binaires du système [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], ne sont pas pris en charge.
- Vérifiez que les clients NFS utilisent l’option « nolock » lors du montage du partage distant.

#### <a name="localization"></a>Localisation

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

#### <a name="sql-server-integration-services-ssis"></a><a id="ssis"></a> SQL Server Integration Services (SSIS)

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

#### <a name="sql-server-management-studio-ssms"></a><a id="ssms"></a> SQL Server Management Studio (SSMS)

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
- [Exécuter sur Docker](quickstart-install-connect-docker.md)
- [Approvisionner une machine virtuelle SQL dans Azure](/azure/virtual-machines/linux/sql/provision-sql-server-linux-virtual-machine?toc=/sql/toc/toc.json)
- [Exécuter et connecter - Cloud](quickstart-install-connect-clouds.md)

Pour obtenir des réponses aux questions fréquemment posées, consultez la [FAQ de SQL Server sur Linux](sql-server-linux-faq.yml).
