---
title: Déployer Azure SQL Edge avec Azure Data Studio
description: Comment déployer des instances Azure SQL Edge dans Azure Data Studio
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: how-to
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan
ms.custom: ''
ms.date: 09/22/2020
ms.openlocfilehash: b0dd541ce4be57dd9b065b93413ea567c7f121b7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052220"
---
# <a name="deploy-azure-sql-edge-with-azure-data-studio-preview"></a>Déployer Azure SQL Edge avec Azure Data Studio (préversion)

[Azure SQL Edge](/azure/azure-sql-edge/overview) est un moteur de base de données relationnelle optimisé pour les déploiements IoT et Azure IoT Edge. Il offre des fonctionnalités permettant de créer une couche de traitement et de stockage des données hautes performances pour les solutions et applications IoT. Cet article explique comment déployer une instance Azure SQL Edge avec Azure Data Studio et les scénarios de déploiement pris en charge par l’assistant de déploiement.  

Les scénarios suivants sont pris en charge par les différents assistants de déploiement d’Azure Data Studio :

- Instance de conteneur locale
- Instance de conteneur distante
- Nouveaux IoT Hub et machine virtuelle Azure
- Appareil existant d’un IoT Hub Azure
- Plusieurs appareils d’un IoT Hub Azure

| Outils requis | `Docker` | `Azure CLI` |
| ------------- | :---: | :---: |
| Instance de conteneur locale | x | |
| Instance de conteneur distante | | |
| Nouveaux IoT Hub et machine virtuelle Azure | | x |
| Appareil existant d’un IoT Hub Azure |  | x |
| Plusieurs appareils d’un IoT Hub Azure |   |  x |

> [!NOTE]
> Le déploiement d’Azure SQL Edge (préversion) est disponible par le biais de l’extension de déploiement Azure SQL Edge, alors que ces fonctionnalités sont en préversion. Pour rendre les fonctionnalités disponibles, installez l’extension dans Azure Data Studio.

Pour commencer un déploiement dans Azure Data Studio, ouvrez le menu dans la vue **Connexion**, puis sélectionnez l’option **Nouveau déploiement...** .  Pour tous les scénarios Azure SQL Edge, sélectionnez **Azure SQL Edge** sur l’écran suivant et le scénario souhaité dans la liste déroulante **Cible du déploiement**. L’assistant de déploiement génère un notebook TSQL qui peut être exécuté pour terminer le déploiement. Notez que les informations de connexion, y compris le mot de passe de l’administrateur SQL, sont contenues par défaut dans le notebook.

![vue d’ensemble du déploiement](media/deploy-azure-sql-edge/deploy-overview.png)

## <a name="local-container-instance"></a>Instance de conteneur locale

Azure SQL Edge peut être déployé sur un conteneur Docker sur un hôte local en indiquant le nom du conteneur, le mot de passe de l’utilisateur `sa` et le port hôte pour la connectivité Azure SQL Edge.  Une fois le déploiement terminé, plusieurs liens s’affichent sous le notebook et vous permettent d’effectuer d’autres actions :

- **Cliquez ici pour vous connecter à l’instance Azure SQL Edge** : vous connecte à la nouvelle instance Azure SQL Edge dans Azure Data Studio
- **Ouvrir la fenêtre du terminal** : ouvre un terminal (existant ou nouveau) dans Azure Data Studio
- **Arrêter le conteneur** : dans le terminal, copie une commande qui arrête le conteneur lorsqu’elle est exécutée par l’utilisateur
- **Supprimer le conteneur** : dans le terminal, copie une commande qui supprime le conteneur lorsqu’elle est exécutée par l’utilisateur

## <a name="remote-container-instance"></a>Instance de conteneur distante

Azure SQL Edge peut être déployé sur un conteneur Docker sur un hôte distant sur lequel Docker est installé en indiquant les informations relatives au conteneur et celles relatives à la machine hôte/cible.  Une fois le déploiement terminé, un lien de connexion s’affiche sous le notebook.  Comme l’environnement de l’hôte du conteneur est distant, pour arrêter ou supprimer le conteneur, les commandes doivent être copiées pour s’exécuter dans un interpréteur de commandes distant.

## <a name="new-azure-iot-hub-and-vm"></a>Nouveaux IoT Hub et machine virtuelle Azure

L’assistant de déploiement Azure SQL Edge peut créer plusieurs ressources Azure nécessaires pour déployer une machine virtuelle (VM) périphérique connectée à un IoT Hub Azure. Un abonnement Azure actif est requis. Ce déploiement crée :

- Groupe de ressources (si le groupe de ressources actuel n’est pas sélectionné)
- Groupe de sécurité réseau
- Machine virtuelle
- Adresse IP publique statique

Si vous le souhaitez, un fichier dacpac peut être compressé et déployé sur la nouvelle instance Azure SQL Edge au cours du processus.  Si un fichier dacpac est fourni, un compte de stockage Blob Azure est créé dans le même groupe de ressources.

## <a name="existing-device-of-an-azure-iot-hub"></a>Appareil existant d’un IoT Hub Azure

Si vous disposez d’un IoT Hub et d’un appareil connecté, Azure SQL Edge peut être déployé sur l’appareil en fonction du groupe de ressources, du nom de l’IoT Hub et de l’ID de l’appareil.
L’adresse IP fournie dans l’assistant de déploiement est utilisée pour générer un lien connexion rapide sous le notebook.

Si vous le souhaitez, un fichier dacpac peut être compressé et déployé sur la nouvelle instance Azure SQL Edge au cours du processus.  Si un fichier dacpac est fourni, un compte de stockage Blob Azure est créé dans le même groupe de ressources.

## <a name="multiple-devices-of-an-azure-iot-hub"></a>Plusieurs appareils d’un IoT Hub Azure

Si vous disposez d’un IoT Hub et d’appareils connectés, Azure SQL Edge peut être déployé sur l’appareil en fonction du groupe de ressources, du nom de l’IoT Hub et de la [condition cible](/azure/iot-edge/module-deployment-monitoring#target-condition) de sélectionner un ou plusieurs appareils.
L’adresse IP fournie dans l’assistant de déploiement est utilisée pour générer un lien connexion rapide sous le notebook.

Si vous le souhaitez, un fichier dacpac peut être compressé et déployé sur la nouvelle instance Azure SQL Edge au cours du processus.  Si un fichier dacpac est fourni, un compte de stockage Blob Azure est créé dans le même groupe de ressources.

## <a name="next-steps"></a>Étapes suivantes

- [En savoir plus sur Azure SQL Edge](/azure/azure-sql-edge/)