---
title: Extension Azure Arc (préversion)
description: Découvrez comment installer et utiliser l’extension Azure Arc pour essayer Azure Arc Data Services.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: yualan
ms.author: alayu
ms.reviewer: maghan, sstein
ms.custom: ''
ms.date: 09/22/2020
ms.openlocfilehash: 195404df404c6fec7ef98f41a9921d937c16eab7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048589"
---
# <a name="azure-arc-extension-for-azure-data-studio-preview"></a>Extension Azure Arc pour Azure Data Studio (préversion)

L’[extension Azure Arc (préversion)](/azure/azure-arc/data/) est une extension permettant de créer et de gérer des ressources Azure Arc Data Services.

**Les actions principales sont les suivantes :**
- Créer une ressource
    - Contrôleur de données
    - SQL Managed Instance pour Azure Arc
    - PostgreSQL pour Azure Arc
- Gérer une ressource
    - Visualiser le tableau de bord du contrôleur de données
    - Visualiser le tableau de bord de SQL Managed Instance pour Azure Arc
    - Visualiser le tableau de bord de PostgreSQL pour Azure Arc
- Exécuter un Jupyter Book Azure Arc

## <a name="install-the-extension"></a>Installer l’extension
- Installez l’extension **Azure Data CLI** depuis la galerie.
- Installez l’extension **Azure Arc** depuis la galerie.
- Redémarrez Azure Data Studio.

## <a name="sign-in-with-azure-account"></a>Se connecter à votre compte Azure
1. Sélectionner Comptes en bas à gauche
1. Sélectionner Ajouter un compte
1. Cette action lance un navigateur. Connectez-vous à votre compte Azure.

## <a name="create-a-resource"></a>Créer une ressource
Cette extension prend en charge le déploiement de contrôleurs de données Azure Arc, de Postgres pour Azure Arc et de SQL Managed Instance pour Azure Arc. Les déploiements peuvent être effectués à l’aide de l’assistant de déploiement intégré.

1. Sélectionner la vue Connexions dans la barre d’activité de gauche
1. Sélectionner les trois points et sélectionner **Nouveau déploiement**
1. Suivre les invites pour créer une nouvelle ressource Azure Arc.

## <a name="manage-a-resource"></a>Gérer une ressource
Après avoir déployé un contrôleur de données Azure Arc à partir d’azdata, du portail Azure ou d’Azure Data Studio, vous pouvez vous y connecter depuis Azure Data Studio.

1. Ouvrez la vue Connexions dans la barre d’activité de gauche.
1. Développer **Contrôleurs Azure Arc**
1. Sélectionner **Connecter un contrôleur**
1. Renseignez les paramètres et connectez-vous.

Une fois connecté, vous pouvez afficher les ressources déployées sur le contrôleur de données. Vous pouvez ensuite cliquer avec le bouton droit de la souris et sélectionner **Gérer** pour accéder au tableau de bord des ressources.  

Ces tableaux de bord fournissent des informations supplémentaires sur la ressource, y compris la possibilité de l’ouvrir dans le portail Azure.

## <a name="next-steps"></a>Étapes suivantes
Pour en savoir plus sur Azure Arc Data Services, [consultez notre documentation.](/azure/azure-arc/data/)