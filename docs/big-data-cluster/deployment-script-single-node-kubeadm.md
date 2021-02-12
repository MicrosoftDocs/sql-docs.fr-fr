---
title: Déployer un cluster kubeadm à nœud unique
titleSuffix: SQL Server Big Data Clusters
description: Utilisez un script de déploiement bash pour déployer un cluster Big Data SQL Server 2019 sur un cluster kubeadm mononœud.
author: mihaelablendea
ms.author: mihaelab
ms.reviewer: mikeray
ms.metadata: seo-lt-2019
ms.date: 12/13/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 63e1e3776c62033280287e55c288791901a2d15f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100046925"
---
# <a name="deploy-with-a-bash-script-to-a-single-node-kubeadm-cluster"></a>Effectuer un déploiement avec un script bash sur un cluster kubeadm mononœud

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Dans ce tutoriel, vous utilisez un exemple de script de déploiement bash pour déployer un cluster Kubernetes mononœud à l’aide de kubeadm et un cluster Big Data SQL Server sur ce cluster.

## <a name="prerequisites"></a>Prérequis

- Un **serveur** virtuel ou une machine physique Ubuntu 18.04 ou 16.04 « Vanilla ». Toutes les dépendances sont définies par le script et vous exécutez le script à partir de la machine virtuelle.

  > [!NOTE]
  > L’utilisation de machines virtuelles Linux Azure n’est pas encore prise en charge.

- La machine virtuelle doit avoir au moins 8 processeurs, 64 Go de RAM et 100 Go d’espace disque. Après avoir tiré (pull) toutes les images Docker du cluster Big Data, vous disposez de 50 Go pour les données et les journaux à utiliser sur tous les composants.

- Mettez à jour les packages existants avec les commandes ci-dessous pour garantir que l’image du système d’exploitation est à jour.

   ``` bash
   sudo apt update && sudo apt upgrade -y
   sudo systemctl reboot
   ```

## <a name="recommended-virtual-machine-settings"></a>Paramètres recommandés pour la machine virtuelle

1. Utilisez une configuration de mémoire statique pour la machine virtuelle. Par exemple, dans les installations Hyper-V, n’utilisez pas l’allocation de mémoire dynamique, mais allouez à la place les 64 Go recommandés ou plus.

1. Utilisez la fonctionnalité de point de contrôle ou d’instantané dans votre hyperviseur, afin de pouvoir restaurer la machine virtuelle à un état propre.


## <a name="instructions-to-deploy-sql-server-big-data-cluster"></a>Instructions pour déployer un cluster Big Data SQL Server

1. Téléchargez le script sur la machine virtuelle que vous prévoyez d’utiliser pour le déploiement.

   ```bash
   curl --output setup-bdc.sh https://raw.githubusercontent.com/microsoft/sql-server-samples/master/samples/features/sql-big-data-cluster/deployment/kubeadm/ubuntu-single-node-vm/setup-bdc.sh
   ```

2. Rendez le script exécutable avec la commande suivante.

   ```bash
   chmod +x setup-bdc.sh
   ```

3. Exécutez le script (veillez le faire en tant que *sudo*)

   ```bash
   sudo ./setup-bdc.sh
   ```

   Quand vous y êtes invité, indiquez le mot de passe à utiliser pour les points de terminaison externes suivants : contrôleur, maître SQL Server et passerelle. Le mot de passe doit être suffisamment complexe et basé sur les règles existantes pour les mots de passe SQL Server. Le nom d’utilisateur du contrôleur est *admin* par défaut.

4. Configurez un alias pour l’outil **azdata**.

   ```bash
   source ~/.bashrc
   ```

5. Actualisez la configuration des alias pour azdata.

   ```bash
   azdata --version
   ```

## <a name="cleanup"></a>Nettoyage

Le script [cleanup-bdc.sh](https://raw.githubusercontent.com/microsoft/sql-server-samples/master/samples/features/sql-big-data-cluster/deployment/kubeadm/ubuntu-single-node-vm/cleanup-bdc.sh) est fourni pour faciliter la réinitialisation de l’environnement, si elle est nécessaire. Cependant, nous vous recommandons d’utiliser une machine virtuelle à des fins de test et d’utiliser la fonctionnalité d’instantané dans votre hyperviseur pour restaurer la machine virtuelle à un état propre.

## <a name="next-steps"></a>Étapes suivantes

Pour bien démarrer avec les clusters Big Data, consultez [Tutoriel : Charger un exemple de données dans un cluster Big Data SQL Server](tutorial-load-sample-data.md).
