---
title: Ajouter le composant WebPart Visionneuse de rapports de SQL Server Reporting Services à un site SharePoint | Microsoft Docs
description: Affichez un rapport, à partir de SQL Server Reporting Services ou de Power BI Report Server, en ajoutant un composant WebPart Visionneuse de rapports à une page SharePoint.
ms.date: 11/26/2018
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server-sharepoint
ms.topic: conceptual
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: b0fa25f3951ab33001f84f7f44bd4617659a81ae
ms.sourcegitcommit: 7791bd2ba339edc5cd2078a6537c8f6bfe72a19b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/19/2021
ms.locfileid: "98564426"
---
# <a name="add-sql-server-reporting-services-report-viewer-web-part-to-a-sharepoint-page"></a>Ajouter le composant WebPart Visionneuse de rapports de SQL Server Reporting Services à un site SharePoint

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016-and-later](../../includes/ssrs-appliesto-2016-and-later.md)]  [!INCLUDE[ssrs-appliesto-pbirsi](../../includes/ssrs-appliesto-pbirs.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-and-later](../../includes/ssrs-appliesto-sharepoint-2013-and-later.md)] [!INCLUDE[ssrs-appliesto-not-sharepoint-online](../../includes/ssrs-appliesto-not-sharepoint-online.md)]

Affichez un rapport, à partir de SQL Server Reporting Services ou de Power BI Report Server, en ajoutant un composant WebPart Visionneuse de rapports à une page SharePoint.

![Composant WebPart Visionneuse de rapports sur une page SharePoint](media/sharepoint-report-viewer-web-part-on-page.png)

## <a name="prerequisites"></a>Prérequis

* Pour pouvoir charger correctement des rapports, le Service d’émission de jetons Revendications vers Windows (C2WTS) doit être configuré pour la délégation Kerberos contrainte. Pour plus d’informations sur la configuration du Service d’émission de jetons Revendications vers Windows (C2WTS), consultez [Service d’émission de jetons Revendications vers Windows (C2WTS) et Reporting Services](../install-windows/claims-to-windows-token-service-c2wts-and-reporting-services.md).

* Le composant WebPart Visionneuse de rapports doit être déployé sur votre batterie de serveurs SharePoint. Pour plus d’informations sur le déploiement du projet de solution du composant WebPart Visionneuse de rapports, consultez [Déployer le composant WebPart Visionneuse de rapports de SQL Server Reporting Services sur un site SharePoint](deploy-report-viewer-web-part.md).

* Pour ajouter un composant WebPart à une page web, vous devez disposer de l’autorisation Ajouter et personnaliser les pages au niveau du site. Si vous utilisez des paramètres de sécurité par défaut, cette autorisation est accordée aux membres du groupe **Propriétaires** qui ont le niveau d’autorisation Contrôle total.

## <a name="add-web-part"></a>Ajouter le composant WebPart

1. Dans votre site SharePoint, cliquez sur l’icône **engrenage** située en haut à gauche et sélectionnez **Ajouter une page**.

    ![Ajouter une page à un site sharepoint à partir de l’icône d’engrenage.](media/sharepoint-add-a-page.png)

2. Donnez un nom à votre page, puis sélectionnez **Créer**.

3. Dans le concepteur de page, sélectionnez l’onglet de ruban **Insérer**. Sélectionnez ensuite **Composant WebPart** dans la section **Composants WebPart**.

    ![Insérer un composant WebPart à partir du ruban Office.](media/sharepoint-insert-web-part.png)

4. Sous **Catégories**, sélectionnez **SQL Server Reporting Services (mode natif)** . Sous **Composants WebPart**, sélectionnez **Visionneuse de rapports**. Sélectionnez ensuite **Ajouter**.

    ![Ajouter le composant WebPart Visionneuse de rapports.](media/sharepoint-report-viewer-web-part.png)

    Il se peut qu’une erreur initiale se produise. Celle-ci est liée au fait que l’URL du serveur de rapports par défaut est peut-être initialement définie sur *https://localhost* alors qu’il n’est peut-être pas disponible à cet emplacement.

## <a name="configure-the-report-viewer-web-part"></a>Configurer le composant WebPart Visionneuse de rapports

Pour configurer le composant WebPart de manière à ce qu’il pointe vers votre rapport spécifique, effectuez les étapes suivantes.

1. Lorsque vous modifiez la page SharePoint, sélectionnez la flèche vers le bas dans le coin supérieur droit du composant WebPart, puis sélectionnez **Modifier le composant WebPart**.

    ![Modifier le composant WebPart dans le menu déroulant du composant WebPart.](media/sharepoint-edit-web-part.png)

2. Entrez l’**URL du serveur de rapports** qui héberge votre rapport. Elle doit se présenter comme ceci : *https://myrsserver/reportserver* .

3. Entrez le chemin et le nom du rapport que vous souhaitez afficher dans le composant WebPart. Par exemple, */Exemples de rapports AdventureWorks/Ventes de la société*. Dans cet exemple, le rapport *Ventes de la société* se trouve dans un dossier nommé *Exemples de rapports AdventureWorks*.

4. Si des paramètres sont obligatoires pour votre rapport, après avoir spécifié l’URL du serveur de rapports et le nom du rapport, sélectionnez **Charger les paramètres** dans la section **Paramètres**.

5. Sélectionnez **OK** pour enregistrer les modifications apportées à la configuration du composant WebPart.

6. Sélectionnez **Enregistrer** dans le ruban Office pour enregistrer les modifications apportées à la page SharePoint.

![Composant WebPart Visionneuse de rapports sur une page SharePoint](media/sharepoint-report-viewer-web-part-on-page.png)

## <a name="considerations-and-limitations"></a>Observations et limitations

* Le composant WebPart Visionneuse de rapports ne peut pas être utilisé sur les pages « modernes » dans SharePoint.
* Les rapports Power BI ne peuvent pas être utilisés avec le composant WebPart Visionneuse de rapports.
* Si vous ne voyez pas le composant WebPart Visionneuse de rapports à ajouter à votre page, vérifiez que vous avez [déployé le composant WebPart Visionneuse de rapports](deploy-report-viewer-web-part.md).

D’autres questions ? [Essayez de poser une question dans le forum Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231)
