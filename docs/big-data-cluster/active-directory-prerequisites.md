---
title: Déployer en mode Active Directory - Prérequis
titleSuffix: SQL Server Big Data Cluster
description: Configurer Active Directory pour Clusters Big Data SQL Server
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.date: 09/30/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 3424863924338a0d3f02102609052d6c159e1043
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048009"
---
# <a name="deploy-big-data-clusters-2019-in-active-directory-mode-prerequisites"></a>Déployez [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] en mode Active Directory : Prérequis

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Ce document explique comment préparer le déploiement d’un cluster Big Data SQL Server en mode d’authentification Active Directory. Le cluster utilise un domaine AD existant pour l’authentification.

>[!Note]
>Avant la version SQL Server 2019 CU5, il existait dans Clusters Big Data une restriction selon laquelle il n’était possible de déployer qu’un seul cluster sur un domaine Active Directory. Elle a été supprimée avec la version CU5. Pour plus d’informations sur les nouvelles fonctionnalités, consultez [Concept : Déploiement de [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] en mode Active Directory](active-directory-deployment-background.md). Les exemples de cet article sont ajustés pour prendre en charge les deux cas d’utilisation de déploiement.

## <a name="background"></a>Arrière-plan

Pour activer l’authentification Active Directory (AD), le cluster BDC crée automatiquement les utilisateurs, les groupes, les comptes d’ordinateur et les noms de principaux de service (SPN) dont les différents services du cluster ont besoin. Pour fournir une certaine autonomie à ces comptes et permettre des autorisations délimitées, nous suggérons de créer une unité d’organisation (UO) avant le déploiement du cluster. Tous les objets AD liés au BDC seront créés au cours du déploiement. 

## <a name="pre-requisites"></a>Conditions préalables

### <a name="organizational-unit-ou"></a>Unité d’organisation (UO)
Une unité d’organisation (UO) est une sous-division d’une instance Active Directory où placer des utilisateurs, des groupes et même d’autres unités d’organisation. Les unités d’organisation peuvent être utilisées pour refléter la structure fonctionnelle ou commerciale d’une organisation. Dans cet article, nous allons créer une unité d’organisation appelée `bdc` comme exemple. 

>[!NOTE]
>L’unité d’organisation représente les limites administratives et permet aux clients de contrôler l’étendue de l’autorité des administrateurs de données. 

Vous pouvez suivre les [principes de conception des unités d’organisation](/windows-server/identity/ad-ds/plan/reviewing-ou-design-concepts) afin de décider de la meilleure structure pour utiliser des UO au sein de votre organisation.

### <a name="ad-account-for-bdc-domain-service-account"></a>Compte AD pour un compte de service de domaine BDC

Pour pouvoir créer automatiquement tous les objets nécessaires dans Active Directory, le BDC a besoin d’un compte AD qui dispose d’autorisations spécifiques pour créer des utilisateurs, des groupes et des comptes d’ordinateurs au sein de l’unité d’organisation fournie. Cet article explique comment configurer l’autorisation de ce compte AD. Nous utilisons un appel de compte AD `bdcDSA` comme exemple dans cet article.

### <a name="auto-generated-active-directory-objects"></a>Objets Active Directory générés automatiquement
Le déploiement de BDC génère automatiquement des noms de comptes et de groupes. Chacun des comptes représente un service dans le BDC et sera géré par le BDC pendant toute la durée de vie du cluster BDC. Ils possèdent les noms de principal du service (SPN) demandé par chaque service.  Pour obtenir la liste complète des comptes, groupes et services AD générés automatiquement, consultez [Objets Active Directory générés automatiquement](active-directory-objects.md).

>[!IMPORTANT]
>Selon la stratégie d’expiration de mot de passe définie dans le contrôleur de domaine, les mots de passe de ces comptes peuvent expirer. La politique d'expiration par défaut est de 42 jours. Il n’existe aucun mécanisme permettant de faire pivoter les informations d’identification de tous les comptes dans le BDC, ainsi le cluster ne pourra plus fonctionner une fois la période d’expiration atteinte. Pour contourner ce problème, mettez à jour la stratégie d’expiration pour les comptes de service BDC sur « Le mot de passe n’expire jamais » dans le contrôleur de domaine. Cette action peut être effectuée avant ou après l’heure d’expiration. Dans ce dernier cas, Active Directory réactivera les mots de passe arrivés à expiration.
>
>L’illustration suivante montre où définir cette propriété pour Utilisateurs et Ordinateurs dans Active Directory.
>
>:::image type="content" source="media/deploy-active-directory/image25.png" alt-text="Définir la stratégie d’expiration de mot de passe":::

Les étapes ci-dessous supposent que vous disposez déjà d’un contrôleur de domaine Active Directory. Si vous n’avez pas de contrôleur de domaine, le [guide](https://social.technet.microsoft.com/wiki/contents/articles/37528.create-and-configure-active-directory-domain-controller-in-azure-windows-server.aspx) suivant comprend les étapes qui peuvent être utiles.

## <a name="create-ad-objects"></a>Créer des objets AD

Avant de déployer un cluster BDC avec l’intégration AD, procédez comme suit :

1. Créez une unité d’organisation (UO) dans laquelle tous les objets AD liés au BDC seront stockés. Vous pouvez également choisir de désigner une UO existante lors du déploiement.
1. Créez un compte AD pour le cluster BDC ou utilisez un compte existant et fournissez les autorisations appropriées à ce compte AD dans l’unité d’organisation fournie.

### <a name="create-a-user-in-ad-for-bdc-domain-service-account"></a>Créer un utilisateur dans Active Directory pour le compte de service de domaine du cluster BDC

Le cluster Big Data requiert un compte avec des autorisations spécifiques. Avant de continuer, assurez-vous de disposer d’un compte AD existant ou de créer un nouveau compte, que le cluster Big Data pourra utiliser pour configurer les objets nécessaires.

Pour créer un utilisateur dans Active Directory, vous pouvez cliquer avec le bouton droit sur le domaine ou l’unité d’organisation et sélectionner **Nouveau** > **Utilisateur** :

![Boîte de dialogue Utilisateurs Active Directory](./media/deploy-active-directory/image12.png)

Cet utilisateur est désigné sous le terme de *compte de service de domaine du cluster BDC* dans cet article.

### <a name="create-an-ou"></a>Création d’une unité d’organisation

Sur le contrôleur de domaine, ouvrez **Utilisateurs et ordinateurs Active Directory**. Dans le panneau de gauche, cliquez avec le bouton droit sur le répertoire dans lequel vous souhaitez créer votre UO, sélectionnez **Créer** \> **Unité d’organisation**, puis suivez les invites de l’Assistant pour créer l’UO. Vous pouvez également créer une unité d’organisation avec PowerShell :

```powershell
New-ADOrganizationalUnit -Name "<name>" -Path "<Distinguished name of the directory you wish to create the OU in>"
```

Les exemples de cet article utilisent `bdc` comme nom de l’UO.

![Unité d’organisation Active Directory](./media/deploy-active-directory/image13.png)

![Nouvel Objet - Unité d’organisation](./media/deploy-active-directory/image14.png)

### <a name="set-permissions-for-an-ad-account"></a>Définition d’autorisations pour un compte AD

Que vous ayez créé un nouvel utilisateur AD ou que vous utilisiez un utilisateur Active Directory existant, l’utilisateur doit disposer de certaines autorisations. Ce compte est le compte d’utilisateur que le contrôleur du cluster BDC utilisera lorsqu’il joindra le cluster à Active Directory.

Le compte de service de domaine (DSA) du cluster BDC doit être en mesure de créer des utilisateurs, des groupes et des comptes d’ordinateur dans l’unité d’organisation. Dans les étapes suivantes, nous avons nommé le compte de service de domaine du cluster BDC `bdcDSA`. Vous pouvez choisir n’importe quel nom pour ce compte.

1. Sur le contrôleur de domaine, ouvrez **Utilisateurs et ordinateurs Active Directory**

1. Dans le volet de gauche, accédez à votre domaine, puis à l’unité d’organisation que `bdc` utilisera

1. Cliquez avec le bouton droit sur l’UO, puis sélectionnez **Propriétés**.

1. Accédez à l’onglet Sécurité (assurez-vous que vous avez sélectionné **Fonctionnalités avancées** en cliquant avec le bouton droit sur l’unité d’organisation et en sélectionnant **Afficher**)

    ![Propriétés d’objet BDC](./media/deploy-active-directory/image15.png)

1. Cliquez sur **Ajouter...** et ajoutez l’utilisateur **bdcDSA**.

    ![Ajoutez des propriétés d’objets BDC](./media/deploy-active-directory/image16.png)

    ![Sélectionnez un objet](./media/deploy-active-directory/image17.png)

1. Sélectionnez l’utilisateur **bdcDSA**, puis désactivez toutes les autorisations et cliquez sur **Avancé**.

1. Cliquez sur **Ajouter**

    ![Cliquez sur Ajouter](./media/deploy-active-directory/image18.png)

    - Cliquez sur **Sélectionner un principal**, insérez **bdcDSA**, puis cliquez sur OK.

    - Définissez **Type** sur **Autoriser**

    - Définissez **S’applique à** sur **Cet objet et tous ceux descendants**

        ![Définissez Autoriser pour les propriétés](./media/deploy-active-directory/image19.png)

    - Faites défiler l’affichage vers le bas et cliquez sur **Effacer tout**

    - Refaites défiler l’affichage vers le haut, puis sélectionnez :
       - **Lire toutes les propriétés**
       - **Écrire toutes les propriétés**
       - **Créer des objets ordinateur**
       - **Supprimer des objets ordinateur**
       - **Créer des objets groupe**
       - **Supprimer des objets groupe**
       - **Créer des objets utilisateur**
       - **Supprimer des objets utilisateur**

    - Cliquez sur **OK**

- Cliquez sur **Ajouter**

    - Cliquez sur **Sélectionner un principal**, insérez **bdcDSA**, puis cliquez sur OK.

    - Définissez **Type** sur **Autoriser**

    - Définissez **S’applique à** sur **Objets ordinateur descendants**

    - Faites défiler l’affichage vers le bas et cliquez sur **Effacer tout**

    - Refaites défiler l’affichage vers le haut et sélectionnez **Réinitialiser le mot de passe**

    - Cliquez sur **OK**

- Cliquez sur **Ajouter**

    - Cliquez sur **Sélectionner un principal**, insérez **bdcDSA**, puis cliquez sur OK.

    - Définissez **Type** sur **Autoriser**

    - Définissez **S’applique à** sur **Objets utilisateur descendants**

    - Faites défiler l’affichage vers le bas et cliquez sur **Effacer tout**

    - Refaites défiler l’affichage vers le haut et sélectionnez **Réinitialiser le mot de passe**

    - Cliquez sur **OK**

- Cliquez sur **OK** encore à deux reprises pour fermer les boîtes de dialogue ouvertes

## <a name="next-steps"></a>Étapes suivantes

[Déploiement de [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] en mode Active Directory](active-directory-deploy.md)

[Résolution des problèmes d’intégration Active Directory Clusters Big Data SQL Server](troubleshoot-active-directory.md)

[Concept : Déploiement de [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] en mode Active Directory](active-directory-deployment-background.md)
