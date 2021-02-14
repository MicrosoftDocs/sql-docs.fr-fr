---
title: Parties de rapport dans le Concepteur de rapports | Microsoft Docs
description: Publiez des éléments de rapport paginé séparément sur un serveur de rapports ou un site SharePoint afin que d’autres personnes et vous-même puissiez les réutiliser dans d’autres rapports dans le Générateur de rapports.
ms.date: 05/30/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-design
ms.topic: conceptual
f1_keywords:
- sql13.rtp.rptdesigner.components.f1
ms.assetid: 0c34311d-05d6-4bd2-b452-545fa95f8e7f
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 873d299829171f516da4a9eeacce37786b99347b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100062824"
---
# <a name="report-parts-in-report-designer-ssrs"></a>Parties de rapport dans le Concepteur de rapports (SSRS)

  Dans le Concepteur de rapports, une fois que vous avez créé des tables, graphiques et autres éléments de rapport paginé dans un projet, vous pouvez les publier comme des *parties de rapport* sur un serveur de rapports ou sur le site SharePoint intégré avec un serveur de rapports afin que vous et d’autres personnes puissiez les réutiliser dans d’autres rapports.  
  
 En général, les parties de rapport fonctionnent de la même façon dans le Concepteur de rapports et dans le Générateur de rapports. Pour en savoir plus sur les fonctionnalités de base, consultez [Parties de rapports &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/report-parts-report-builder-and-ssrs.md).  
  
 Il existe des différences fondamentales dans la façon dont les parties de rapport fonctionnent dans le Concepteur de rapports. Le flux de travail représente une différence notable. Le Générateur de rapports permet la création collaborative : je crée une partie de rapport et la publie. Vous pouvez la réutiliser, la modifier et la republier. Dans le Concepteur de rapports, la publication est unidirectionnelle : je peux publier une partie de rapport dans le Concepteur de rapports et vous pouvez la réutiliser. Mais je ne peux pas réutiliser une partie de rapport existante dans un rapport dans le Concepteur de rapports. Cette rubrique présente ces différences, après une vue d'ensemble rapide des parties de rapports.  
  
##  <a name="life-cycle-of-report-part-publishing"></a><a name="ComponentWorkflow"></a> Publication du cycle de vie d'une partie de rapport  
 ![rs_ComponentCreation](../../reporting-services/report-design/media/rs-componentcreation.gif "rs_ComponentCreation")  
  
1.  Dans le Concepteur de rapports, une personne A crée un projet qui contient un rapport avec un graphique dépendant d'un dataset incorporé.  
  
2.  La personne A signale le graphique avec son dataset incorporé à publier. Le Concepteur de rapports lui affecte un ID unique. La personne A déploie ensuite le rapport sur le serveur de rapports. Le Concepteur de rapports publie le graphique.  
  
3.  Une personne B crée un rapport vide dans le Générateur de rapports et lui ajoute le graphique. Le graphique fait maintenant partie du rapport de la personne B, avec le dataset incorporé. La personne B peut modifier les instances du graphique et du dataset qui sont dans le rapport. Cela n'aura aucun effet sur les instances du graphique et du dataset sur le serveur de rapports et ne rompra pas non plus la relation entre les instances dans le rapport et sur le serveur de rapports.  
  
     ![rs_BIDScomponentupdate](../../reporting-services/report-design/media/rs-bidscomponentupdate.gif "rs_BIDScomponentupdate")  
  
4.  Dans le Concepteur de rapports, la personne A modifie le graphique dans le rapport d'origine.  
  
5.  La personne A redéploie le rapport, ce qui a pour effet de republier le graphique sur le serveur, mettant ainsi à jour le graphique sur le serveur.  
  
6.  Dans le Générateur de rapports, la personne que B accepte le graphique mis à jour provenant du serveur. Les modifications que la personne B avait apportées au graphique dans son rapport sont ainsi remplacées.  
  
##  <a name="publishing-report-parts"></a><a name="PublishingComponents"></a> Publication de parties de rapport  
 Lorsque vous publiez une partie de rapport, le Générateur de rapports lui affecte un ID unique. À compter de ce moment, il maintient cet ID, peu importe les modifications que vous lui apportez. L'ID lie l'élément de rapport d'origine dans votre rapport à la partie de rapport. Lorsque d'autres auteurs de rapports réutilisent la partie de rapport dans le Générateur de rapports, l'ID lie également la partie de rapport dans leur rapport à celle sur le serveur de rapports.  
  
 Voici les éléments de rapport que vous pouvez publier comme parties de rapport :  
  
-   Graphiques  
  
-   Jauges  
  
-   Images et images incorporées  
  
-   Cartes  
  
-   Paramètres  
  
-   Rectangles  
  
-   Tables  
  
-   Matrices  
  
-   Listes  
  
 Si vous publiez une partie de rapport qui affiche des données, telles qu'une table, une matrice ou un graphique, vous pouvez la baser sur un dataset partagé ; sinon, lorsque vous publiez la partie de rapport, le dataset dont elle dépend est enregistré en tant que dataset incorporé. Les datasets incorporés peuvent être basés sur les sources de données incorporées, mais les informations d'identification ne sont pas stockées dans les sources de données incorporées. Par conséquent, si votre partie de rapport dépend d'un dataset incorporé qui utilise une source de données incorporée, toute personne qui réutilise cette partie de rapport devra fournir les informations d'identification pour la source de données incorporée. Pour éviter cela, basez vos datasets incorporés et partagés sur les sources de données partagées avec les informations d'identification stockées. Pour plus d’informations, consultez [Parties de rapports et datasets dans le Générateur de rapports](../../reporting-services/report-data/report-parts-and-datasets-in-report-builder.md).  
  
 La publication d'une partie de rapport dans le Concepteur de rapports comporte deux étapes :  
  
1.  Signalez les éléments de rapport que vous voulez publier dans la boîte de dialogue **Publier les parties de rapport** .  
  
2.  Déployez le rapport.  
  
 Lorsque vous déployez le rapport, la partie de rapport est publiée sur un site SharePoint ou un serveur de rapports, et d'autres personnes peuvent la réutiliser. Pour publier une partie de rapport, vous devez disposer d’une connexion et d’autorisations suffisantes pour accéder à un serveur de rapports lorsque vous déployez le rapport.  
  
  
##  <a name="reusing-report-parts"></a><a name="SearchReuseComponents"></a> Réutilisation de parties de rapport  
 Contrairement à ce qui se produit dans le Générateur de rapports, vous ne pouvez pas rechercher et réutiliser une partie de rapport dans un projet autre que celui dans lequel il a été créé.  
  
 Les auteurs de rapport qui travaillent dans le Générateur de rapports peuvent rechercher et réutiliser des parties de rapport que vous publiez dans les rapports qu'ils créent.  
  
##  <a name="republishing-report-parts"></a><a name="RepublishingComponents"></a> Republication de parties de rapport  
 Dans le Concepteur de rapports, vous devez mettre à jour une partie de rapport existante dans le rapport dans lequel vous l'avez créée. Dans le Générateur de rapports, les auteurs de rapport peuvent réutiliser la partie de rapport et la publier comme une nouvelle partie de rapport sans remplacer la partie de rapport que vous avez publiée. S'ils disposent d'autorisations suffisantes, ils peuvent également mettre à jour la partie de rapport que vous avez publiée. Toute personne disposant d'autorisations suffisantes à un dossier sur un site ou un serveur peut mettre à jour les parties de rapport qui y sont stockées. La dernière mise à jour remplace les mises à jour précédentes.  
  
 Vous pouvez modifier puis republier la partie de rapport sur le site ou le serveur. Les auteurs de rapports du Générateur de rapports qui ont ajouté cette partie de rapport à un rapport sont informés de la modification la prochaine fois qu'ils ouvrent ce rapport. Ils peuvent choisir d'accepter ou non vos modifications.  
  
 Vous pouvez également choisir de publier en tant que nouveau rapport un rapport que vous avez déjà publié. Dans la boîte de dialogue Publier les parties de rapports, cliquez sur Publier en tant que nouvelle partie de rapport. Cette nouvelle partie de rapport a un nouvel ID unique et aucune relation avec l'ancienne partie de rapport.  

## <a name="next-steps"></a>Étapes suivantes

[Gestion de parties de rapport](../../reporting-services/report-design/managing-report-parts.md)  

D’autres questions ? [Essayez de poser une question dans le forum Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231)
