---
title: Explorez les ressources Azure SQL avec Azure Resource Explorer
description: Découvrez comment explorer et gérer les Azure SQL Server, Azure SQL Database et Azure SQL Managed Instance à l’aide d’Azure Resource Explorer.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: how-to
author: yanancai
ms.author: yanacai
ms.reviewer: alayu, maghan, sstein
ms.custom: seodec18
ms.date: 09/24/2018
ms.openlocfilehash: 5d28054e9f459bc3334a863e7ee5edb3bd98afea
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048729"
---
# <a name="explore-and-manage-azure-sql-resources-with-azure-resource-explorer"></a>Explorer et gérer les ressources Azure SQL avec Azure Resource Explorer

Dans ce document, vous découvrez comment explorer et gérer les Azure SQL Server, Azure SQL Database et Azure SQL Managed Instance à l’aide d’Azure Resource Explorer dans [!INCLUDE [Azure Data Studio](../includes/name-sos-short.md)].

>[!NOTE]
>Azure Resource Explorer est pris en charge dans SQL Server 2019. Après cela, vous pourrez installer l’extension via le [gestionnaire d’extensions](./extensions/add-extensions.md) ou **Fichier** > **Installer un package à partir d’un package VSIX**.

## <a name="connect-to-azure"></a>Connexion à Azure

Après l’installation du plug-in SQL, une icône Azure s’affiche dans la barre de menus de gauche. Cliquez sur l’icône pour ouvrir Azure Resource Explorer. Si vous ne voyez pas l’icône Azure, cliquez avec le bouton droit sur la barre de menus de gauche, puis sélectionnez **Azure Resource Explorer**.

### <a name="add-an-azure-account"></a>Ajouter un compte Azure

Pour afficher les ressources SQL associées à un compte Azure, vous devez d’abord ajouter le compte à [!INCLUDE [Azure Data Studio](../includes/name-sos-short.md)].

1. Ouvrez la boîte de dialogue **Comptes liés** par le biais de l’icône de gestion des comptes dans la partie inférieure gauche, ou via le lien **Se connecter à Azure...**  dans Azure Resource Explorer.

    ![Se connecter à Azure](media/azure-resource-explorer/sign-in-to-azure.png)

2. Dans la boîte de dialogue **Comptes liés**, cliquez sur **Ajouter un compte**.

    ![Ajouter un compte Azure](media/azure-resource-explorer/add-an-azure-account.png)

3. Cliquez sur **Copier et ouvrir** pour ouvrir le navigateur pour l’authentification.

    ![Ouvrir la page d’authentification dans le navigateur](media/azure-resource-explorer/open-authentication-in-browser.png)

4. Collez le **Code utilisateur** dans la page web, puis cliquez sur **Continuer** pour vous authentifier.

    ![S’authentifier dans le navigateur](media/azure-resource-explorer/authenticate-in-browser.png)

5. Dans [!INCLUDE [Azure Data Studio](../includes/name-sos-short.md)], vous devriez maintenant trouver le compte Azure connecté dans la boîte de dialogue **Comptes liés**.

    ![Compte Azure connecté](media/azure-resource-explorer/signed-in-azure-account.png)

### <a name="add-more-azure-accounts"></a>Ajouter d’autres comptes Azure

Plusieurs comptes Azure sont pris en charge dans [!INCLUDE [Azure Data Studio](../includes/name-sos-short.md)]. Pour ajouter d’autres comptes Azure, cliquez sur le bouton situé en haut à droite de la boîte de dialogue **Comptes liés** et suivez les mêmes étapes qu’avec la section Ajouter un compte Azure pour ajouter d’autres comptes Azure.

![Ajouter un compte Azure](media/azure-resource-explorer/add-more-azure-account.png)

### <a name="remove-an-azure-account"></a>Supprimer un compte Azure

Pour supprimer un compte Azure connecté existant :

1. Ouvrez la boîte de dialogue **Comptes liés** via l’icône de gestion des comptes dans la partie inférieure gauche.
2. Cliquez sur le bouton **X** à droite du compte Azure pour le supprimer.

    ![Supprimer un compte Azure](media/azure-resource-explorer/remove-azure-account.png)

## <a name="filter-subscription"></a>Filtrer les abonnements

Après connexion à un compte Azure, tous les abonnements associés à ce compte Azure s’affichent dans Azure Resource Explorer. Vous pouvez filtrer les abonnements pour chaque compte Azure.

1. Cliquez sur le bouton **Sélectionner un abonnement** à droite du compte Azure.

   ![Filtrer les abonnements](media/azure-resource-explorer/filter-subscription.png)

2. Activez les cases à cocher correspondant aux abonnements de compte que vous souhaitez parcourir, puis cliquez sur **OK**.

   ![Sélectionner un abonnement](media/azure-resource-explorer/select-subscription.png)

## <a name="explore-azure-sql-resources"></a>Explorer les ressources Azure SQL

Pour accéder à une ressource SQL Azure dans Azure Resource Explorer, développez les comptes Azure et le groupe de types de ressource.

Azure Resource Explorer prend actuellement en charge Azure SQL Server, Azure SQL Database et Azure SQL Managed Instance.

## <a name="connect-to-azure-sql-resources"></a>Se connecter aux ressources Azure SQL

Azure Resource Explorer fournit un accès rapide qui vous permet de vous connecter à des serveurs et bases de données SQL pour l’interrogation et la gestion.

1. Explorez la ressource SQL à laquelle vous souhaitez vous connecter à partir de l’arborescence.
2. Cliquez avec le bouton droit sur la ressource et sélectionnez **Se connecter**. Vous trouverez également le bouton de connexion à droite de la ressource.

   ![Se connecter à la ressource Azure SQL](media/azure-resource-explorer/connect-to-azure-sql-resource.png)

3. Dans la boîte de dialogue **Connexion** ouverte, entrez votre mot de passe, puis cliquez sur **Se connecter**.

   ![Dialogue de connexion SQL](media/azure-resource-explorer/sql-connection-dialog.png)
4. La fenêtre **Serveurs** s’ouvre automatiquement avec la nouvelle base de données/le serveur SQL connecté une fois la connexion établie.

## <a name="next-steps"></a>Étapes suivantes

- [Utilisez [!INCLUDE[Azure Data Studio](../includes/name-sos-short.md)] pour vous connecter et interroger la base de données Azure SQL](quickstart-sql-database.md)
- [Utilisez [!INCLUDE[Azure Data Studio](../includes/name-sos-short.md)] pour vous connecter et interroger des données dans Azure Synapse Analytics](quickstart-sql-dw.md)