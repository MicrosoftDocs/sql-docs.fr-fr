---
title: Mise en route de SQL Server (sur Linux) dans le cloud
titleSuffix: SQL Server
description: Découvrez comment installer SQL Server sur Red Hat Enterprise Linux (RHEL), SUSE Linux Enterprise Server (SLES) ou Ubuntu dans le cloud de votre choix.
author: VanMSFT
ms.author: vanto
ms.date: 11/04/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: ''
ms.openlocfilehash: ae7502fcba11d9892cbcf8ed2c94ef205e0abc53
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100273130"
---
# <a name="quickstart-run-sql-server-in-the-cloud"></a>Démarrage rapide : Exécuter SQL Server dans le cloud
[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Dans ce démarrage rapide, vous installerez SQL Server sur Red Hat Enterprise Linux (RHEL), SUSE Linux Enterprise Server (SLES) ou Ubuntu dans le cloud de votre choix. Consultez [Approvisionnement d’une machine virtuelle Linux SQL Server dans le portail Azure](/azure/virtual-machines/linux/sql/provision-sql-server-linux-virtual-machine?toc=%252fsql%252ftoc%252ftoc.json) pour exécuter SQL Server sur Linux dans Azure.

> [!NOTE]
> Si vous choisissez d'exécuter une édition payante de SQL Server, vous devez apporter votre propre licence (BYOL).

## <a name="amazon-web-services"></a>Amazon Web Services
1.  Créez un AMI Linux avec au moins 2 Go de mémoire provenant de la Place de marché 
    * [RHEL 7.3+](https://aws.amazon.com/marketplace/pp/B00KWBZVK6)
    * [SLES v12 SP2+](https://aws.amazon.com/marketplace/pp/B00PMM99PI)
    * [Ubuntu 16.04](https://aws.amazon.com/marketplace/pp/B01JBL2M0O)
1.  Connectez-vous à l'AMI avec ssh
1.  Suivez le démarrage rapide pour la distribution Linux que vous avez choisie : 
    * [RHEL](quickstart-install-connect-red-hat.md)
    * [SLES](quickstart-install-connect-suse.md)
    * [Ubuntu](quickstart-install-connect-ubuntu.md)
1.  Configuration pour des connexions distantes : 
    * Ouvrez la [console Amazon EC2]( https://console.aws.amazon.com/ec2/).
    * Dans le volet de navigation, choisissez **Groupes de sécurité**. 
    * Cliquez sur **Inbound, Edit, Add Rule** (Entrant, Modifier, Ajouter une règle).
    * Ajoutez une règle de trafic entrant pour autoriser le trafic sur le port sur lequel SQL Server écoute (port TCP 1433 par défaut)

    
## <a name="digital-ocean"></a>Digital Ocean
1. Connectez-vous au [panneau de contrôle](https://cloud.digitalocean.com/login) et cliquez pour créer un droplet
1. Choisissez un droplet Ubuntu 16.04 avec au moins 2 Go de mémoire
1. Connectez-vous au droplet avec ssh
1. Suivez le [démarrage rapide Ubuntu](quickstart-install-connect-ubuntu.md)
1. Configuration pour des connexions distantes :
    * En haut du Panneau de configuration, suivez le lien **Mise en réseau** et sélectionnez **Pare-feu**
    * Ajoutez une règle de trafic entrant pour autoriser le trafic sur le port sur lequel SQL Server écoute (port TCP 1433 par défaut)
    
## <a name="google-cloud-platform"></a>Google Cloud Platform
1.  Créez une image Linux avec au moins 2 Go de mémoire provenant de Cloud Launcher 
    * [RHEL 7.3+](https://console.cloud.google.com/launcher/details/rhel-cloud/rhel-7)
    * [SLES v12 SP4](https://console.cloud.google.com/launcher/details/suse-cloud/sles-12)
    * [Ubuntu 16.04](https://console.cloud.google.com/launcher/details/ubuntu-os-cloud/ubuntu-xenial)
1.  Connectez-vous à l'image avec ssh
1.  Suivez le démarrage rapide pour la distribution Linux que vous avez choisie : 
    * [RHEL](quickstart-install-connect-red-hat.md)
    * [SLES](quickstart-install-connect-suse.md)
    * [Ubuntu](quickstart-install-connect-ubuntu.md)
1.  Configuration pour des connexions distantes : 
    * Accédez à [Règles de pare-feu](https://console.cloud.google.com/networking/firewalls)
    * Ajoutez une règle de trafic entrant pour autoriser le trafic sur le port sur lequel SQL Server écoute (port TCP par défaut : 1433)