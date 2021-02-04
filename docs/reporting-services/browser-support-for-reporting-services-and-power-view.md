---
title: Prise en charge des navigateurs pour Reporting Services et Power View | Microsoft Docs
description: Découvrez les versions de navigateurs prises en charge pour la gestion et l’affichage de SQL Server Reporting Services, des contrôles ReportViewer Controls et de Power View.
ms.date: 01/28/2021
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: reporting-services
ms.topic: conceptual
helpviewer_keywords:
- displaying reports
- scripts [Reporting Services], requirements
- viewing reports
- browsers [Reporting Services]
- Web browsers [Reporting Services], about browser support
- browsing reports [Reporting Services]
- components [Reporting Services], browsers
- Web browsers [Reporting Services]
ms.assetid: 48a75bbb-0029-4c43-891d-dc8f4fc0ebe1
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: e246db4f2a7b2a94ce17f8a48acf05b16aebbdf4
ms.sourcegitcommit: 04d101fa6a85618b8bc56c68b9c006b12147dbb5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99049018"
---
# <a name="browser-support-for-reporting-services-and-power-view"></a>Prise en charge des navigateurs pour Reporting Services et Power View

[!INCLUDE[ssrs-appliesto](../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016-and-later](../includes/ssrs-appliesto-2016-and-later.md)] [!INCLUDE[ssrs-appliesto-pbirsi](../includes/ssrs-appliesto-pbirs.md)]

Découvrez les versions de navigateurs prises en charge pour la gestion et l’affichage de SQL Server Reporting Services, des contrôles ReportViewer Controls et de Power View.

> [!NOTE]
> L’intégration de Reporting Services à SharePoint n’est plus disponible après SQL Server 2016.

## <a name="browser-requirements-for-the-web-portal"></a>Navigateurs exigés pour le portail web

Voici la liste actuelle des navigateurs pris en charge pour le portail web.

**Microsoft Windows**  
*Windows 7, 8.1, 10 ; Windows Server 2008 R2, 2012, 2012 R2*
- Microsoft Edge (+)
- Microsoft Internet Explorer 10 ou 11
- Google Chrome (+)
- Mozilla Firefox (+)

**Apple OS X**  
*OS X 10.9-10.11*

- Apple Safari (+)
- Google Chrome (+)
- Mozilla Firefox (+)

**Apple iOS**  
*iPhone et iPad IOS 9 avec iOS 9*

- Apple Safari (+)

**Google Android**  
*Téléphones portables et tablettes avec Android 4.4 (KitKat) ou version ultérieure*

- Google Chrome (+)

 **(+)** Dernière version commercialisée

## <a name="browser-requirements-for-the-reportviewer-web-control-2015"></a>Navigateurs requis pour le contrôle web ReportViewer (2015)

 Voici la liste actuelle des navigateurs pris en charge avec le contrôle web ReportViewer (2015). La Visionneuse de rapports prend en charge l’affichage des rapports du portail web [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] et des bibliothèques SharePoint.  

**Microsoft Windows**  
*Windows 7, 8.1, 10 ; Windows Server 2008 R2, 2012, 2012 R2*

- Microsoft Edge (+)
- Microsoft Internet Explorer 10 ou 11
- Google Chrome (+)
- Mozilla Firefox (+)

**Apple OS X**  
*OS X 10.9-10.11*

- Apple Safari (+)

 **(+)** Dernière version commercialisée

::: moniker range="=sql-server-2016"

 Si vous utilisez un produit SharePoint intégré à [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)], consultez  [Planifier la prise en charge du navigateur dans SharePoint 2016](https://technet.microsoft.com//library/cc263526\(v=office.16\).aspx).

::: moniker-end

### <a name="authentication-requirements"></a>Exigences relatives à l’authentification

 Les navigateurs prennent en charge des schémas d'authentification spécifiques qui doivent être gérés par le serveur de rapports pour que la demande du client réussisse. Le tableau suivant identifie les types d'authentification par défaut pris en charge par chaque navigateur exécuté sur un système d'exploitation Windows.

|**Type de navigateur**|**Prise en charge**|**Valeur de navigateur par défaut**|**Valeur de serveur par défaut**|
|----------------------|------------------|-------------------------|------------------------|
|**Microsoft Edge** (+)|Negotiated, NTLM, Basic|Negotiate|Oui. Les paramètres d’authentification par défaut fonctionnent avec Microsoft Edge.|
|**Microsoft Internet Explorer**|Negotiated, NTLM, Basic|Negotiate|Oui. Les paramètres d'authentification par défaut fonctionnent avec Internet Explorer.|
|**Google Chrome**(+)|Negotiated, NTLM, Basic|Negotiate|Oui. Les paramètres d'authentification par défaut fonctionnent avec Chrome.|
|**Mozilla Firefox**(+)|NTLM, Basic|NTLM|Oui. Les paramètres d'authentification par défaut fonctionnent avec Firefox.|
|**Apple Safari**(+)|NTLM, Basic|De base|Oui. Les paramètres d'authentification par défaut fonctionnent avec Safari.|

 **(+)** Dernière version commercialisée

### <a name="script-requirements-for-viewing-reports"></a>Contraintes relatives à l’utilisation de scripts pour afficher des rapports

 Pour utiliser la visionneuse de rapports, configurez votre navigateur pour exécuter des scripts.

 Si l'utilisation de scripts n'est pas activée, vous recevez un message d'erreur similaire au suivant lors de l'ouverture d'un rapport :

- **Votre navigateur ne prend pas en charge les scripts ou est configuré pour ne pas les autoriser. Cliquez ici pour afficher ce rapport sans les scripts**.

 Si vous choisissez de visualiser le rapport sans la prise en charge des scripts, le rapport est rendu en HTML sans fonctionnalités d'afficheur de rapports, telles que la barre d'outils Rapport et l'explorateur de documents.

> [!NOTE]
> La barre d'outils Rapport fait partie du composant Visionneuse HTML. Par défaut, la barre d'outils s'affiche en haut de chaque rapport affiché dans une fenêtre de navigateur. La visionneuse de rapports contient des fonctionnalités qui vous permettent de rechercher des informations dans le rapport, d'atteindre une page spécifique, et d'ajuster la taille de la page à des fins d'affichage. Pour plus d'informations sur la barre d'outils du rapport ou sur la Visionneuse HTML, consultez [HTML Viewer and the Report Toolbar](../reporting-services/html-viewer-and-the-report-toolbar.md).

## <a name="browser-support-for-reportviewer-web-server-controls-in-visual-studio"></a>Prise en charge des navigateurs pour les contrôles serveurs Web ReportViewer dans Visual Studio

 Le contrôle serveur Web ReportViewer est utilisé pour inclure la fonctionnalité de rapports dans une application Web ASP.NET. Les contrôles sont inclus dans Visual Studio et prennent en charge des navigateurs et des versions de navigateur différents de ceux des autres composants décrits dans cette rubrique. Le type de navigateur utilisé pour afficher l'application détermine le type de fonctionnalité ReportViewer que vous pouvez fournir dans votre application. Utilisez le tableau de cette rubrique pour déterminer quels sont les navigateurs pris en charge soumis à des restrictions de fonctionnalités de rapport et quelles sont les plateformes prises en charge.  

 Utilisez un navigateur dans lequel la prise en charge du script est activée. Si le navigateur ne peut pas exécuter de scripts, vous ne pourrez pas afficher le rapport.

**Microsoft Windows**  
*Windows 7, 8.1, 10 ; Windows Server 2008 R2, 2012, 2012 R2*

- Microsoft Edge (+)
- Microsoft Internet Explorer 10 ou 11
- Google Chrome (+)
- Mozilla Firefox (+)

 **(+)** Dernière version commercialisée

## <a name="power-view-browser-support"></a>Prise en charge du navigateur Power View

**Microsoft Windows**  
*Windows 7, 8.1, 10 ; Windows Server 2008 R2, 2012, 2012 R2*

- Microsoft Internet Explorer 10 ou 11
- Mozilla Firefox (+)

**Apple OS X**  
*OS X 10.9-10.11*

- Apple Safari (+)

 **(+)** Dernière version commercialisée

::: moniker range="=sql-server-2016"

 Pour en savoir plus sur la prise en charge des navigateurs SharePoint 2016, consultez [Planifier la prise en charge du navigateur dans SharePoint 2013](https://technet.microsoft.com//library/cc263526\(v=office.16\).aspx).

::: moniker-end

## <a name="next-steps"></a>Étapes suivantes

[Recherche et affichage de rapports dans le portail Web](report-builder/finding-and-viewing-reports-in-the-web-portal-report-builder-and-ssrs.md)  
[Outils de Reporting Services](../reporting-services/tools/reporting-services-tools.md)  
[Portail web (SSRS en mode natif)](./web-portal-ssrs-native-mode.md)  
[Visionneuse HTML et barre d’outils des rapports](../reporting-services/html-viewer-and-the-report-toolbar.md)  
[Référence de paramètres d'accès URL](../reporting-services/url-access-parameter-reference.md)  

D’autres questions ? [Essayez de poser une question dans le forum Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231)
