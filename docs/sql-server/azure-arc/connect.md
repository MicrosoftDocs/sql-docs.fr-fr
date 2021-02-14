---
title: Se connecter à Azure Arc
titleSuffix: ''
description: Connecter une instance SQL Server à Azure Arc
author: anosov1960
ms.author: sashan
ms.reviewer: mikeray
ms.date: 09/10/2020
ms.topic: conceptual
ms.prod: sql
ms.openlocfilehash: 03841af487d6c715357b543798cbb6ac3c6d853e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100078265"
---
# <a name="connect-your-sql-server-to-azure-arc"></a>Connecter votre SQL Server à Azure Arc

Vous pouvez connecter votre instance SQL Server à Azure Arc sur site en procédant comme suit.

## <a name="prerequisites"></a>Prérequis

* Au moins une instance SQL Server doit être installée sur votre machine.
* Le fournisseur de ressources **Microsoft.AzureArcData** a été inscrit à partir d’une des méthodes ci-dessous :  
    * Utilisation du portail Azure :
        - Sélectionnez **Abonnements**. 
        - Choisir votre abonnement
        - Sous **Paramètres**, sélectionnez **Fournisseurs de ressources**
        - Recherchez `Microsoft.AzureArcData` et sélectionnez **Inscrire**
    * Si vous utilisez PowerShell, exécutez `Register-AzResourceProvider -ProviderNamespace Microsoft.AzureArcData`
    * Si vous utilisez l’interface CLI, exécutez `az provider register --namespace 'Microsoft.AzureArcData`

## <a name="generate-a-registration-script-for-sql-server"></a>Générer un script d’inscription pour SQL Server

Au cours de cette étape, vous allez générer un script qui détecte toutes les instances SQL Server installées sur la machine et les enregistre en tant que ressources __SQL Server - Azure Arc__. Si l’ordinateur physique ou la machine virtuelle hôte n’est pas inscrit auprès d’Azure Arc, le script le fait automatiquement.

1. Recherchez le type de ressource __SQL Server - Azure Arc__ et ajoutez-en un nouveau par le biais du panneau de création.

![Démarrer la création](media/join/start-creation-of-sql-server-azure-arc-resource.png)

2. Passez en revue les prérequis et accédez à l’onglet **Détails du serveur**.  

3. Sélectionnez l’abonnement, le groupe de ressources, la région Azure et le système d’exploitation hôte. Si nécessaire, indiquez également le proxy que votre réseau utilise pour se connecter à Internet.

> [!IMPORTANT]
> Si la machine hébergeant l’instance SQL Server est déjà [connectée à Azure Arc](/azure/azure-arc/servers/onboard-portal), veillez à sélectionner le groupe de ressources qui contient la ressource __Machine - Azure Arc__ correspondante.

![Détails du serveur](media/join/server-details-sql-server-azure-arc.png)

4. Accédez à l’onglet **Exécuter le script** et téléchargez le script d’inscription affiché. Le portail génère le script pour le système d’exploitation hôte que vous avez indiqué.

![Télécharger le script](media/join/download-script-sql-server-azure-arc.png)

## <a name="connect-the-installed-sql-server-instances-to-azure-arc"></a>Connecter les instances SQL Server installées à Azure Arc

Au cours de cette étape, vous allez utiliser le script que vous avez téléchargé depuis le portail Azure et l’exécuter sur l’ordinateur physique ou la machine virtuelle cible. Par conséquent, chaque instance SQL Server installée sur la machine sera inscrite en tant que ressource __SQL Server - Azure Arc__. En outre, si l’agent de configuration d’invité n’est pas installé, il sera installé automatiquement et enregistré en tant que ressource __Machine - Azure Arc__.

> [!NOTE]
> Veillez à exécuter le script à l’aide d’un compte qui répond aux conditions d’autorisation minimales décrites dans la section [Prérequis](overview.md#prerequisites).

### <a name="windows"></a>Windows

1. Lancez une instance administrateur de __powershell.exe__ et connectez-vous à votre module PowerShell à l’aide de vos informations d’identification Azure. Suivez les [instructions de connexion](/powershell/azure/install-az-ps#sign-in).

2. Exécuter le script téléchargé

   ```powershell
   & '.\RegisterSqlServerArc.ps1'
   ```

   > [!NOTE]
   > Si le module AZ pour PowerShell n’est pas encore installé, des problèmes peuvent survenir la première fois. Dans ce cas, suivez les instructions du script pour installer et connecter votre compte et réexécuter le script.

### <a name="linux"></a>Linux

1. Utilisez Azure CLI pour vous connecter avec vos informations d’identification Azure. Suivez les [instructions de connexion](/cli/azure/authenticate-azure-cli).

2. Accordez l’autorisation d’exécution au script téléchargé et exécutez-le.

   ```bash
   sudo chmod +x ./RegisterSqlServerArc.sh
   ./RegisterSqlServerArc.sh
   ```

## <a name="register-sql-server-instances-on-multiple-machines"></a>Inscrire des instances SQL Server sur plusieurs machines

Vous pouvez connecter plusieurs instances SQL Server installées sur plusieurs machines Windows ou Linux à Azure Arc en utilisant le script généré pour une seule machine. Suivez les instructions [Connecter des instances SQL Server à Azure Arc à grande échelle](connect-at-scale.md).

## <a name="validate-the-sql-server---azure-arc-resources"></a>Valider les ressources SQL Server - Azure Arc

Accédez au [portail Azure](https://ms.portal.azure.com/#home) et ouvrez la nouvelle ressource __SQL Server - Azure Arc__ à valider.

![Valider le serveur SQL Server connecté ](media/join/validate-sql-server-azure-arc.png)

## <a name="disconnect-your-sql-server-instance"></a>Déconnecter votre instance SQL Server

Pour déconnecter votre instance SQL Server d’Azure Arc, accédez au portail Azure, ouvrez la ressource __SQL Server – Azure Arc__ pour cette instance, puis cliquez sur le bouton **Annuler l’inscription**.

![Annuler l’inscription de SQL Server](media/join/unregister-sql-server-azure-arc.png)

## <a name="next-steps"></a>Étapes suivantes

* [Configurer Advanced Data Security pour votre instance SQL Server](configure-advanced-data-security.md)

* [Configurer SQL Assessment à la demande pour votre instance SQL Server](assess.md)