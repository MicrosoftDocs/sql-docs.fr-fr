---
description: Ouvrir une base de connaissances
title: Ouvrir une base de connaissances
ms.date: 06/04/2013
ms.prod: sql
ms.prod_service: data-quality-services
ms.reviewer: ''
ms.technology: data-quality-services
ms.topic: conceptual
f1_keywords:
- sql13.dqs.kb.openkb.f1
ms.assetid: a5f010a5-b762-41c9-881b-bf0c192dca83
author: swinarko
ms.author: sawinark
ms.openlocfilehash: 5d93731b7e28aafbffdf659678c0017d37ce61db
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88395885"
---
# <a name="open-a-knowledge-base"></a>Ouvrir une base de connaissances

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sqlserver.md)]

  Cette rubrique explique comment ouvrir une base de connaissances existante dans [!INCLUDE[ssDQSnoversion](../includes/ssdqsnoversion-md.md)] (DQS) et la préparer pour la gestion de l'arborescence du domaine, la découverte des connaissances ou l'ajout d'une stratégie de correspondance.  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="prerequisites"></a><a name="Prerequisites"></a> Conditions préalables  
 Pour ouvrir une base de connaissances, la base de connaissances doit déjà avoir été créée et publiée (si une autre personne l'a créée) ou fermée (si vous l'avez créée).  
  
###  <a name="security"></a><a name="Security"></a> Sécurité  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Vous devez disposer du rôle dqs_kb_editor ou dqs_administrator sur la base de données DQS_MAIN pour ouvrir une base de connaissances.  
  
##  <a name="open-a-knowledge-base"></a><a name="Open"></a> Ouvrir une base de connaissances  
  
1.  [!INCLUDE[ssDQSInitialStep](../includes/ssdqsinitialstep-md.md)][Exécutez l’Application Data Quality client](../data-quality-services/run-the-data-quality-client-application.md).  
  
2.  Dans l'écran d'accueil de [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)] , cliquez sur **Ouvrir une base de connaissances**.  
  
3.  Sélectionnez une base de connaissances dans la table. Les domaines et les règles de correspondance de la base de connaissances seront affichés dans le volet droit de la page.  
  
    > [!NOTE]  
    >  Vous pouvez effectuer des opérations sur une base de connaissances en cliquant avec le bouton droit dessus dans la table. Vous pouvez ouvrir la base de connaissances, l'enregistrer sous un autre nom, la déverrouiller, ignorer le travail, la renommer ou afficher ses propriétés.  
  
4.  Dans **Sélectionner une activité**, sélectionnez l'activité que vous souhaitez effectuer sur la base de connaissances :  
  
    -   Sélectionnez **Gestion de l'arborescence du domaine** pour accéder aux écrans que vous utilisez pour modifier les domaines de la base de connaissances.  
  
    -   Sélectionnez **Découverte des connaissances** pour accéder à l'Assistant que vous utilisez pour analyser un échantillon de données et remplir les domaines de la base de connaissances avec les résultats.  
  
    -   Sélectionnez **Stratégie de correspondance** pour créer une stratégie de correspondance et l'ajouter à la base de connaissances.  
  
5.  Cliquez sur **Ouvrir**.  
  
    > [!NOTE]  
    >  Vous pouvez également ouvrir la base de connaissances en cliquant dessus avec le bouton droit, puis en cliquant sur Ouvrir. D'autres commandes dans le menu contextuel vous permettent de l'enregistrer sous un autre nom, de la déverrouiller, d'ignorer le travail, de la renommer ou d'afficher ses propriétés.  
  
    > [!NOTE]  
    >  Si vous ne pouvez pas ouvrir la base de connaissances car elle est verrouillée, consultez la section ci-dessous.  
  
## <a name="open-a-recent-knowledge-base"></a>Ouvrir une base de connaissances récente  
 Les cinq bases de connaissances récemment ouvertes sont affichées dans la liste **Base de connaissances récente** dans la page d'accueil de DQS. Cela vous permet d'ouvrir une base de connaissances que vous avez récemment utilisée sans passer par la page **Ouvrir la base de connaissances** .  
  
-   Pour ouvrir une base de connaissances dans la liste Récents qui n'est pas verrouillée, cliquez sur la flèche droite pour la base de connaissances, puis sélectionnez l'activité dans laquelle vous souhaitez ouvrir la base de connaissances.  
  
-   Pour ouvrir une base de connaissances dans la liste Récents que vous avez verrouillée, cliquez sur la base de connaissances et elle s'ouvre dans l'activité et à la page indiquées entre parenthèses.  
  
-   Pour ouvrir une base de connaissances dans la liste Récents qui a été verrouillée par une autre personne, contactez cette personne et demandez-lui de déverrouiller la base de connaissances.  
  
##  <a name="follow-up-after-opening-a-knowledge-base"></a><a name="FollowUp"></a> Suivi : après avoir ouvert une base de connaissances  
 Après avoir ouvert une base de connaissances, la base de connaissances est placée dans l'état indiqué dans la colonne d'état de la table de base de connaissances. Pour les activités de découverte des connaissances et de stratégie de correspondance, la base de connaissances est ouverte dans une page de l'Assistant spécifique. Pour l'activité de gestion de l'arborescence du domaine, la base de connaissances est ouverte dans la page de gestion de l'arborescence du domaine. Pour plus d’informations sur les différents états, consultez [Effectuer une découverte des connaissances](../data-quality-services/perform-knowledge-discovery.md), [Gestion d’un domaine](../data-quality-services/managing-a-domain.md) ou [Créer une stratégie de correspondance](../data-quality-services/create-a-matching-policy.md).  
  
##  <a name="if-the-knowledge-base-is-locked"></a><a name="Locked"></a> Si la base de connaissances est verrouillée  
 L'icône de verrou dans la première colonne indique si la base de connaissances est verrouillée. Le nom d'une base de connaissances verrouillée s'affiche dans une police rouge. Une base de connaissances qui est modifiée par un utilisateur spécifique via une activité de la base de connaissances est marquée comme verrouillée. Une base de connaissances verrouillée ne peut pas être exploitée par un deuxième utilisateur. L'utilisateur qui travaille sur la base de connaissances peut la déverrouiller en cliquant avec le bouton droit sur la base de connaissances dans la table de la page Ouvrir la base de connaissances, en cliquant sur **Déverrouiller**ou en la publiant. Lorsque le curseur est positionné sur une base de connaissances verrouillée, DQS affiche un indicateur qui signale qui a verrouillé la base de connaissances et quand.  
  
##  <a name="state-of-a-knowledge-base"></a><a name="State"></a> État d’une base de connaissances  
 Le champ État indique l'étape d'une activité à laquelle la base de connaissances se trouve. Si vous ouvrez la base de connaissances, elle s'ouvre à cette étape.  
  
-   **\<Empty>**: Le champ État est vide pour une base de connaissances si la base de connaissances a été publiée en cliquant sur **publier** dans l’activité de gestion des domaines, puis sur **Oui-publier la base de connaissances et quitter**.  
  
-   **En cours**: le travail sur la base de connaissances a été enregistré en cliquant sur **publier** dans l’activité de gestion des domaines, puis sur **non-enregistrer le travail dans la base de connaissances et quitter**.  
  
-   **Gestion de l'arborescence du domaine**: des données ont été entrées pour un domaine dans la base de connaissances, mais la base de connaissances n'a pas été publiée et le travail reste dans l'activité de gestion de l'arborescence du domaine. L'activité de découverte des connaissances n'est pas disponible. Cela se produit lorsque vous cliquez sur **Fermer** dans l'écran **Gestion de l'arborescence du domaine** .  
  
-   **Découverte - Mappage**: la base de connaissances a été fermée sur la page **Gestion de la base de connaissances : Mappage** . La base de connaissances est verrouillée, et les activités de gestion de l'arborescence du domaine et de correspondance ne sont pas disponibles.  
  
-   **Découverte - Découvrir**: la base de connaissances a été fermée sur la page **Gestion de la base de connaissances : Analyse** . La base de connaissances est verrouillée et l'activité de gestion de l'arborescence du domaine n'est pas disponible.  
  
-   **Découverte-gestion des valeurs**: la base de connaissances a été fermée sur la page gestion de la **base de connaissances :** gestion des termes de domaine. La base de connaissances est verrouillée et l'activité de gestion de l'arborescence du domaine n'est pas disponible.  
  
-   Stratégie **de correspondance-stratégie de correspondance**: la base de connaissances a été fermée sur la page stratégie de correspondance **-stratégie de** correspondance. La base de connaissances est verrouillée, et les activités de découverte des connaissances et de gestion de l'arborescence du domaine ne sont pas disponibles.  
  
-   **Stratégie de correspondance-résultats de correspondance**: la base de connaissances a été fermée sur la page **des résultats de la stratégie de** correspondance. La base de connaissances est verrouillée, et les activités de découverte des connaissances et de gestion de l'arborescence du domaine ne sont pas disponibles.  
  
  
