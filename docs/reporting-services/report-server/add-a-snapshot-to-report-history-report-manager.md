---
title: Ajouter un instantané à un historique de rapport - Reporting Services | Microsoft Docs
description: Découvrez plus d’informations sur la façon d’ajouter manuellement un instantané à l’historique de rapport dans SQL Server Reporting Services (SSRS).
ms.prod: reporting-services
ms.technology: reporting-services
ms.topic: conceptual
author: maggiesMSFT
ms.author: maggies
ms.reviewer: ''
ms.custom: ''
ms.date: 06/26/2019
ms.openlocfilehash: b11e4f129ea349e44c328b1f665a3935a937506a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100021630"
---
# <a name="add-a-snapshot-to-report-history"></a>Ajouter un instantané à un historique de rapport

L'historique de rapport est un ensemble d'instantanés de rapport que vous créez au fil du temps. Un instantané de rapport est un rapport contenant des informations de mise en page et des résultats de requêtes récupérés à un moment précis. Contrairement aux rapports à la demande, qui récupèrent les résultats des requêtes récentes lorsque vous les sélectionnez, les instantanés de rapport sont traités par planification, puis enregistrés sur un serveur de rapports. Lorsque vous sélectionnez un instantané de rapport pour le visualiser, le serveur de rapports récupère le rapport stocké dans la base de données du serveur de rapports, puis affiche les données et la mise en page telles qu'elles étaient lors de la création de l'instantané.  
  
Les instantanés de rapport ne sont pas enregistrés dans un format de rendu particulier. Ils sont générés dans un format d'affichage final (par exemple, au format HTML) uniquement à la demande d'un utilisateur ou d'une application. Le rendu différé permet de disposer d'un instantané portable. Le rendu du rapport peut être effectué dans le format approprié pour le périphérique ou le navigateur Web à l'origine de la demande.  
  
## <a name="to-manually-add-snapshots-to-report-history"></a>Pour ajouter manuellement des instantanés à un historique de rapport
  
::: moniker range="=sql-server-2016"

1. Dans le Gestionnaire de rapports, accédez à la page **Contenu** , pointez sur l’élément pour lequel vous souhaitez consulter l’historique et cliquez sur la flèche déroulante.
  
2. Dans le menu déroulant, cliquez sur **Afficher l’historique du rapport**.  
  
3. Cliquez sur **Nouvel instantané**. Un instantané est créé dans la colonne **Lors de l’exécution** .  
    > [!NOTE]
    > Pour permettre la création d’instantanés, l’administrateur doit configurer l’historique de rapport sur **Autoriser la création manuelle de l’historique**. Pour plus d’informations, consultez [Limiter l’historique de rapport &#40;Gestionnaire de rapports&#41;](../reports/limit-report-history-report-manager.md).

4. Cliquez sur **Appliquer**.
  
## <a name="to-automatically-add-all-snapshots-to-report-history"></a>Pour ajouter automatiquement tous les instantanés à un historique de rapport  
  
1. Pour un rapport déjà configuré pour s'exécuter en tant qu'instantané d'exécution de rapport, vous pouvez définir des propriétés supplémentaires afin d'enregistrer une copie de l'instantané dans l'historique de rapport chaque fois qu'il est actualisé.  
  
2. Dans le Gestionnaire de rapports, accédez à la page **Contenu** , pointez sur l’élément pour lequel vous souhaitez consulter l’historique et cliquez sur la flèche déroulante.  
  
3. Dans le menu déroulant, cliquez sur **Gérer**.  
  
4. Cliquez sur **Options d’instantanés**.  
  
5. Cochez la case **Stocker toutes les captures instantanées d’exécution de rapport dans l’historique**.  
  
6. Cliquez sur **Appliquer**.  
  
## <a name="to-automatically-add-snapshots-to-report-history-based-on-a-schedule"></a>Pour ajouter automatiquement tous les instantanés à un historique de rapport planifié  
  
1. Dans le Gestionnaire de rapports, accédez à la page **Contenu** , pointez sur l’élément pour lequel vous souhaitez consulter l’historique et cliquez sur la flèche déroulante.  
  
2. Dans le menu déroulant, cliquez sur **Gérer**.  
  
3. Cliquez sur **Options d’instantanés**.  
  
4. Cochez la case **Utilisez la planification ci-dessous pour ajouter des instantanés à l’historique de rapport**. Effectuez une des opérations suivantes :  
  
    - Sélectionnez **Planification spécifique aux rapports**. Indiquez les détails de la planification, sélectionnez des dates de début et de fin pour cette opération, puis cliquez sur **OK**.  

    - Sélectionnez **Planification partagée**. Dans la liste, désignez la planification de votre choix.  

5. Cliquez sur **Appliquer**.  
  
## <a name="see-also"></a>Voir aussi

- [Configurer les propriétés d’exécution d’un rapport &#40;Gestionnaire de rapports&#41;](../../reporting-services/reports/configure-execution-properties-for-a-report-report-manager.md)
- [Limiter l’historique de rapport &#40;Gestionnaire de rapports&#41;](../../reporting-services/reports/limit-report-history-report-manager.md)
- [Planifications](../../reporting-services/subscriptions/schedules.md)   
- [Gestionnaire de rapports &#40;SSRS en mode natif&#41;](../web-portal-ssrs-native-mode.md)

::: moniker-end

::: moniker range=">=sql-server-2017"

## <a name="to-manually-add-snapshots-to-report-history"></a>Pour ajouter manuellement des instantanés à un historique de rapport
  
1. Dans le portail web, accédez à l’élément dont vous voulez afficher l’historique, puis cliquez dessus avec le bouton droit.  
  
2. Dans le menu déroulant, sélectionnez **Gérer**.  
  
3. Sélectionnez l’onglet **Instantanés d’historique**.  
  
4. Dans la page **Instantanés d’historique**, sélectionnez **Nouvel instantané d’historique**. Un nouvel instantané est créé et s’affiche en dessous avec la date et l’heure actuelles dans la colonne **Créé**.  
  
    > [!NOTE]
    > Pour permettre la création d’instantanés, l’administrateur doit configurer l’historique de rapport sur **Autoriser la création manuelle de l’historique**. Pour plus d’informations, consultez [Limiter l’historique de rapport (portail web)](../../reporting-services/reports/limit-report-history-report-manager.md).

## <a name="to-add-snapshots-via-a-schedule-to-report-history"></a>Pour ajouter des instantanés via une planification à l'historique de rapport

1. Dans le portail web, accédez à l’élément dont vous voulez afficher l’historique, puis cliquez dessus avec le bouton droit.  
  
2. Dans le menu déroulant, sélectionnez **Gérer**.  
  
3. Sélectionnez l’onglet **Instantanés d’historique**.  
  
4. Dans la page **Instantanés d’historique**, sélectionnez le bouton **Planification et paramètres**.  
  
5. Dans la section **Planification**, sélectionnez l’une des options ci-dessous, ou les deux, si au moins un choix n’est pas déjà sélectionné :
    - **Créer des instantanés d'historique selon une planification**.  
    - **Autoriser les utilisateurs à créer manuellement des instantanés.**  
  
6. Dans la section **Avancé**, sélectionnez **Conserver tous les instantanés d’historique**.  
  
7. Vous pouvez aussi cocher la case **Enregistrer également les instantanés de cache dans l’historique de rapport**.  
  
8.  Sélectionnez **Appliquer** pour enregistrer les paramètres.  

    > [!NOTE]  
    > Pour permettre la création d’instantanés, l’administrateur doit configurer l’historique de rapport sur **Autoriser la création manuelle de l’historique**. Pour plus d’informations, consultez [Limiter l’historique de rapport (portail web)](../../reporting-services/reports/limit-report-history-report-manager.md).

9.  Cliquez sur **Appliquer**.

## <a name="to-automatically-add-all-snapshots-to-report-history"></a>Pour ajouter automatiquement tous les instantanés à un historique de rapport  
  
1. Pour un rapport déjà configuré pour s'exécuter en tant qu'instantané d'exécution de rapport, vous pouvez définir des propriétés supplémentaires afin d'enregistrer une copie de l'instantané dans l'historique de rapport chaque fois qu'il est actualisé.  
  
2. Dans le portail web, accédez à l’élément dont vous voulez afficher l’historique, puis cliquez dessus avec le bouton droit.  
  
3. Dans le menu déroulant, sélectionnez **Gérer**.  
  
4. Sélectionnez l’onglet **Instantanés d’historique**.  
  
5. Dans la page **Instantanés d’historique**, sélectionnez le bouton **Planification et paramètres**.  
  
6. Dans la section **Planification**, sélectionnez l’une des options ci-dessous, ou les deux, si au moins un choix n’est pas déjà sélectionné :
    - **Créer des instantanés d'historique selon une planification**.  
    - **Autoriser les utilisateurs à créer manuellement des instantanés.**  
  
7. Dans la section **Avancé**, sélectionnez **Conserver tous les instantanés d’historique**.  
  
8. Vous pouvez aussi cocher la case **Enregistrer également les instantanés de cache dans l’historique de rapport**.  
  
9. Sélectionnez **Appliquer** pour enregistrer les paramètres.  
  
## <a name="to-automatically-add-snapshots-to-report-history-based-on-a-schedule"></a>Pour ajouter automatiquement tous les instantanés à un historique de rapport planifié  
  
1. Dans le portail web, accédez à l’élément dont vous voulez afficher l’historique, puis cliquez dessus avec le bouton droit.  
  
2. Dans le menu déroulant, sélectionnez **Gérer**.  
  
3. Sélectionnez l’onglet **Instantanés d’historique**.  
  
4. Dans la page **Instantanés d’historique**, sélectionnez le bouton **Planification et paramètres**.  
  
5. Cochez la case **Utilisez la planification ci-dessous pour ajouter des instantanés à l’historique de rapport**. Effectuez une des opérations suivantes :  
  
    - Sélectionnez **Planification spécifique aux rapports**. Indiquez les détails de la planification, sélectionnez des dates de début et de fin pour cette opération, puis cliquez sur **OK**.  

    - Sélectionnez **Planification partagée**. Dans la liste, désignez la planification de votre choix.  

5. Cliquez sur **Appliquer**.  
  
## <a name="see-also"></a>Voir aussi

- [Configurer les propriétés d’exécution d’un rapport (portail web)](../../reporting-services/reports/configure-execution-properties-for-a-report-report-manager.md)
- [Limiter l'historique de rapport (portail web)](../../reporting-services/reports/limit-report-history-report-manager.md)
- [Planifications](../../reporting-services/subscriptions/schedules.md)   
- [Portail web d’un serveur de rapports  &#40;mode natif SSRS&#41;](../web-portal-ssrs-native-mode.md)

::: moniker-end