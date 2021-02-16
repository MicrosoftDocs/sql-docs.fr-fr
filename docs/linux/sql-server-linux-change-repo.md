---
title: Configurer des référentiels Linux pour SQL Server 2017 et 2019
description: Vérifiez et configurez les référentiels source pour SQL Server 2019 et SQL Server 2017 sur Linux. Le référentiel source affecte la version de SQL Server appliquée lors de l’installation et de la mise à niveau.
author: VanMSFT
ms.author: vanto
ms.date: 04/28/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
zone_pivot_groups: ld2-linux-distribution
ms.openlocfilehash: d284a049cb1266dc33c30657f3d86e8421825868
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340396"
---
# <a name="configure-repositories-for-installing-and-upgrading-sql-server-on-linux"></a>Configurer les référentiels pour l’installation et la mise à niveau de SQL Server sur Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

::: zone pivot="ld2-rhel"
Cet article explique comment configurer le référentiel approprié pour les installations et les mises à niveau SQL Server 2017 et SQL Server 2019 sur Linux. En haut, votre sélection actuelle est **Red Hat (RHEL).**
::: zone-end

::: zone pivot="ld2-sles"
Cet article explique comment configurer le référentiel approprié pour les installations et les mises à niveau SQL Server 2017 et SQL Server 2019 sur Linux. En haut, votre sélection actuelle est **SUSE (SLES).**
::: zone-end

::: zone pivot="ld2-ubuntu"
Cet article explique comment configurer le référentiel approprié pour les installations et les mises à niveau SQL Server 2017 et SQL Server 2019 sur Linux. En haut, votre sélection actuelle est **Ubuntu.**
::: zone-end

> [!TIP]
> SQL Server 2019 est maintenant disponible ! Si vous souhaitez l’essayer, lisez cet article pour configurer le nouveau référentiel **mssql-server-2019**. Ensuite, installez à l’aide des instructions du [guide d’installation](sql-server-linux-setup.md).

## <a name="repositories"></a>Référentiels <a id="repositories"></a>

Lorsque vous installez SQL Server sur Linux, vous devez configurer un référentiel Microsoft. Ce référentiel est utilisé pour acquérir le package du moteur de base de données, **mssql-server**, et les packages SQL Server associés. Il existe actuellement cinq référentiels principaux :

| Référentiel | Nom | Description |
|---|---|---|
| **2019** | **mssql-server-2019** | Référentiel contenant la mise à jour cumulative de SQL Server 2019. |
| **2019 GDR** | **mssql-server-2019-gdr** | Référentiel SQL Server 2019 GDR pour les mises à jour critiques uniquement. |
| **2019 Preview** | **mssql-server-preview** | Référentiel contenant la préversion et la version Release Candidate de SQL Server 2019. |
| **2017** | **mssql-server-2017** | Référentiel SQL Server 2017 mise à jour cumulative (CU). |
| **2017 GDR** | **mssql-server-2017-gdr** | Référentiel SQL Server 2017 GDR pour les mises à jour critiques uniquement. |

## <a name="cumulative-update-versus-gdr"></a><a id="cuversusgdr"></a> Mise à jour cumulative et GDR

Il est important de noter qu’il existe deux principaux types de référentiels pour chaque distribution :

- **Mises à jour cumulatives (CU)** : Le référentiel de mise à jour cumulative (CU) contient des packages pour la version de base de SQL Server et des correctifs de bogues ou des améliorations à partir de cette version. Les mises à jour cumulatives sont spécifiques à une version, par exemple SQL Server 2019. Elles sont publiées à un rythme régulier.

- **GDR** : Le référentiel GDR contient des packages pour la version de base de SQL Server et uniquement les correctifs et mises à jour de sécurité critiques depuis cette version. Ces mises à jour sont également ajoutées à la version CU suivante.

Chaque mise à jour CU et GDR contient le package SQL Server complet et toutes les mises à jour précédentes pour ce référentiel. La mise à jour d’une version GDR vers une version CU est prise en charge en modifiant votre référentiel configuré pour SQL Server. Vous pouvez également [passer à une version antérieure](sql-server-linux-setup.md#rollback) avec n’importe quelle version de votre version principale (par exemple: 2017).

> [!NOTE]
> Vous pouvez effectuer la mise à jour d’une version GDR vers CU à tout moment en modifiant les référentiels. La mise à jour d’une version de CU vers une version GDR n’est pas prise en charge.

## <a name="configure-repositories"></a>Configurer les référentiels

::: zone pivot="ld2-rhel"
Utilisez les étapes décrites dans les sections suivantes pour configurer les référentiels sur Red Hat Enterprise Server (RHEL).
::: zone-end

::: zone pivot="ld2-sles"
Utilisez les étapes décrites dans les sections suivantes pour configurer les référentiels sur SUSE Linux Enterprise Server (SLES).
::: zone-end

::: zone pivot="ld2-ubuntu"
Utilisez les étapes décrites dans les sections suivantes pour configurer les référentiels sur Ubuntu.
::: zone-end

## <a name="check-for-previously-configured-repositories"></a>Vérifier les référentiels précédemment configurés

<!--RHEL-->
::: zone pivot="ld2-rhel"
Vérifiez d’abord si vous avez déjà inscrit un référentiel SQL Server.

1. Affichez les fichiers dans le répertoire **/etc/yum.repos.d** à l’aide de la commande suivante:

   ```bash
   sudo ls /etc/yum.repos.d
   ```

2. Recherchez un fichier qui configure le répertoire SQL Server, tel que **mssql-server.repo**.

3. Imprimez le contenu du fichier.

   ```bash
   sudo cat /etc/yum.repos.d/mssql-server.repo
   ```

4. La propriété **Nom** est le référentiel configuré. Vous pouvez l’identifier avec la table dans la section [Référentiels](#repositories) de cet article.

::: zone-end

<!--SLES-->
::: zone pivot="ld2-sles"
Vérifiez d’abord si vous avez déjà inscrit un référentiel SQL Server.

1. Utilisez **zypper info** pour obtenir des informations sur les référentiels précédemment configurés.

   ```bash
   sudo zypper info mssql-server
   ```

2. La propriété **Référentiel** est le référentiel configuré. Vous pouvez l’identifier avec la table dans la section [Référentiels](#repositories) de cet article.

::: zone-end

<!--Ubuntu-->
::: zone pivot="ld2-ubuntu"
Vérifiez d’abord si vous avez déjà inscrit un référentiel SQL Server.

1. Affichez le contenu du fichier **/etc/apt/sources.list**.

   ```bash
   sudo cat /etc/apt/sources.list
   ```

2. Examinez l’URL du package mssql-server. Vous pouvez l’identifier avec la table dans la section [Référentiels](#repositories) de cet article.

::: zone-end

## <a name="remove-old-repository"></a>Supprimer l’ancien référentiel

<!--RHEL-->
::: zone pivot="ld2-rhel"
Si nécessaire, supprimez l’ancien référentiel à l’aide de la commande suivante.

```bash
sudo rm -rf /etc/yum.repos.d/mssql-server.repo
```

Cette commande suppose que le fichier identifié dans la section précédente était nommé **mssql-server.repo**.

::: zone-end

<!--SLES-->
::: zone pivot="ld2-sles"
Si nécessaire, supprimez l’ancien référentiel. Utilisez une des commandes suivantes en fonction du type de référentiel précédemment configuré.

| Référentiel | Commande à supprimer |
|---|---|
| **Préversion (2019)** | `sudo zypper removerepo 'packages-microsoft-com-mssql-server-preview'` |
| **2019 CU** | `sudo zypper removerepo 'packages-microsoft-com-mssql-server-2019'` |
| **2019 GDR** | `sudo zypper removerepo 'packages-microsoft-com-mssql-server-2019-gdr'`|
| **2017 CU** | `sudo zypper removerepo 'packages-microsoft-com-mssql-server-2017'` |
| **2017 GDR** | `sudo zypper removerepo 'packages-microsoft-com-mssql-server-2017-gdr'`|

::: zone-end

<!--Ubuntu-->
::: zone pivot="ld2-ubuntu"
Si nécessaire, supprimez l’ancien référentiel. Utilisez une des commandes suivantes en fonction du type de référentiel précédemment configuré.

> [!NOTE]
> À compter de SQL Server 2019 CU3 et SQL Server 2017 CU20, Ubuntu 18.04 est pris en charge. Si vous utilisez Ubuntu 16.04, remplacez le chemin d’accès ci-dessous par `/ubuntu/16.04` au lieu de `/ubuntu/18.04`, et utilisez le bon [nom de code de distribution](https://releases.ubuntu.com/).

| Référentiel | Commande à supprimer |
|---|---|
| **Préversion (2019)** | `sudo add-apt-repository -r 'deb [arch=amd64] https://packages.microsoft.com/ubuntu/16.04/mssql-server-preview xenial main'` |
| **2019 CU** | `sudo add-apt-repository -r 'deb [arch=amd64] https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019 bionic main'` | 
| **2019 GDR** | `sudo add-apt-repository -r 'deb [arch=amd64] https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019-gdr bionic main'` |
| **2017 CU** | `sudo add-apt-repository -r 'deb [arch=amd64] https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017 xenial main'` | 
| **2017 GDR** | `sudo add-apt-repository -r 'deb [arch=amd64] https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017-gdr xenial main'` |

::: zone-end

## <a name="configure-new-repository"></a>Configurer un nouveau référentiel

<!--RHEL-->
::: zone pivot="ld2-rhel"
Configurez le nouveau référentiel à utiliser pour les installations et les mises à niveau de SQL Server. Utilisez une des commandes suivantes pour configurer le référentiel de votre choix.

> [!NOTE]
> Les commandes suivantes pour SQL Server 2019 pointent vers le référentiel RHEL 8. RHEL 8 n’est pas préinstallé avec python2, ce qui est requis par SQL Server. Pour plus d’informations, consultez le blog suivant sur l’installation de python2 et sa configuration en tant qu’interpréteur par défaut : https://www.redhat.com/en/blog/installing-microsoft-sql-server-red-hat-enterprise-linux-8-beta.
>
> À compter de SQL Server 2017 CU20, RHEL 8 est pris en charge.
>
> Si vous utilisez RHEL 7 ou RHEL 8, vérifiez que les chemins d’accès correspondent à `/rhel/7` ou `/rhel/8`. Nos packages sont indépendants des versions mineures de RHEL. Cela signifie que si vous utilisez RHEL 7.6, vous devez utiliser le chemin `/rhel/7` pour configurer votre dépôt.

| Référentiel | Version | Commande |
|---|---|---|
| **2019 CU** | 2019 | `sudo curl -o /etc/yum.repos.d/mssql-server.repo https://packages.microsoft.com/config/rhel/8/mssql-server-2019.repo` |
| **2019 GDR** | 2019 | `sudo curl -o /etc/yum.repos.d/mssql-server.repo https://packages.microsoft.com/config/rhel/8/mssql-server-2019-gdr.repo` |
| **2017 CU** | 2017 | `sudo curl -o /etc/yum.repos.d/mssql-server.repo https://packages.microsoft.com/config/rhel/8/mssql-server-2017.repo` |
| **2017 GDR** | 2017 | `sudo curl -o /etc/yum.repos.d/mssql-server.repo https://packages.microsoft.com/config/rhel/8/mssql-server-2017-gdr.repo` |

::: zone-end

<!--SLES-->
::: zone pivot="ld2-sles"
Configurez le nouveau référentiel à utiliser pour les installations et les mises à niveau de SQL Server. Utilisez une des commandes suivantes pour configurer le référentiel de votre choix.

| Référentiel | Version | Commande |
|---|---|---|
| **2019 CU** | 2019 | `sudo zypper addrepo -fc https://packages.microsoft.com/config/sles/12/mssql-server-2019.repo` |
| **2019 GDR** | 2019 | `sudo zypper addrepo -fc https://packages.microsoft.com/config/sles/12/mssql-server-2019-gdr.repo` |
| **2017 CU** | 2017 | `sudo zypper addrepo -fc https://packages.microsoft.com/config/sles/12/mssql-server-2017.repo` |
| **2017 GDR** | 2017 | `sudo zypper addrepo -fc https://packages.microsoft.com/config/sles/12/mssql-server-2017-gdr.repo` |

::: zone-end

<!--Ubuntu-->
::: zone pivot="ld2-ubuntu"
Configurez le nouveau référentiel à utiliser pour les installations et les mises à niveau de SQL Server.

> [!NOTE]
> À compter de SQL Server 2019 CU3 et SQL Server 2017 CU20, Ubuntu 18.04 est pris en charge. Les commandes suivantes pour pointent vers le référentiel Ubuntu 18.04.
>
> Si vous utilisez Ubuntu 16.04, remplacez le chemin ci-dessous par `/ubuntu/16.04` au lieu de `/ubuntu/18.04`.

1. Importez les clés GPG de référentiel public.

   ```bash
   sudo curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
   ```

2. Utilisez une des commandes suivantes pour configurer le référentiel de votre choix.

   | Référentiel | Version | Commande |
   |---|---|---|
   | **2019 CU** | 2019 | `sudo add-apt-repository "$(curl https://packages.microsoft.com/config/ubuntu/18.04/mssql-server-2019.list)"` |
   | **2019 GDR** | 2019 | `sudo add-apt-repository "$(curl https://packages.microsoft.com/config/ubuntu/18.04/mssql-server-2019-gdr.list)"` |
   | **2017 CU** | 2017 | `sudo add-apt-repository "$(curl https://packages.microsoft.com/config/ubuntu/18.04/mssql-server-2017.list)"` |
   | **2017 GDR** | 2017 | `sudo add-apt-repository "$(curl https://packages.microsoft.com/config/ubuntu/18.04/mssql-server-2017-gdr.list)"` |

3. Exécutez **apt-get update**.

   ```bash
   sudo apt-get update
   ```

::: zone-end

## <a name="next-steps"></a>Étapes suivantes

Une fois que vous avez configuré le bon référentiel, vous pouvez procéder à [l'installation](sql-server-linux-setup.md#platforms) ou à la [mise à jour](sql-server-linux-setup.md#upgrade) de SQL Server et de tous les packages associés à partir du nouveau référentiel.

::: zone pivot="ld2-rhel"
> [!IMPORTANT]
> À ce stade, si vous choisissez d’utiliser le [démarrage rapide RHEL](quickstart-install-connect-red-hat.md), n’oubliez pas que vous avez déjà configuré le référentiel cible. Ne répétez pas cette étape dans les didacticiels. Cela est particulièrement vrai si vous configurez le référentiel GDR, car le démarrage rapide utilise le référentiel CU.
::: zone-end

::: zone pivot="ld2-sles"
> [!IMPORTANT]
> À ce stade, si vous choisissez d’utiliser le [démarrage rapide SLES](quickstart-install-connect-suse.md), n’oubliez pas que vous avez déjà configuré le référentiel cible. Ne répétez pas cette étape dans les didacticiels. Cela est particulièrement vrai si vous configurez le référentiel GDR, car le démarrage rapide utilise le référentiel CU.
::: zone-end

::: zone pivot="ld2-ubuntu"
> [!IMPORTANT]
> À ce stade, si vous choisissez d’utiliser le [démarrage rapide Ubuntu](quickstart-install-connect-ubuntu.md), n’oubliez pas que vous avez déjà configuré le référentiel cible. Ne répétez pas cette étape dans les didacticiels. Cela est particulièrement vrai si vous configurez le référentiel GDR, car le démarrage rapide utilise le référentiel CU.
::: zone-end

Pour plus d’informations sur l’installation de SQL Server 2017 sur Linux, consultez le [Guide d'installation de SQL Server sur Linux](sql-server-linux-setup.md).
