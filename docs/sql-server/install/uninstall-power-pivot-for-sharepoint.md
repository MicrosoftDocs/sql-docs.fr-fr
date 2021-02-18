---
title: Désinstaller Power Pivot pour SharePoint | Microsoft Docs
description: Cet article explique comment désinstaller un Power Pivot pour l’installation de SharePoint, qui est une opération en plusieurs étapes.
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
ms.assetid: 3941a2f0-0d0c-4d1a-8618-7a6a7751beac
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: 375f351c1d80e957fba29f81b9d2afc423c222ad
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336093"
---
# <a name="uninstall-power-pivot-for-sharepoint"></a>Désinstaller Power Pivot pour SharePoint
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  La désinstallation d'une installation de [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] est une opération comportant plusieurs étapes qui inclut la préparation pour la désinstallation, la suppression des fonctionnalités et des solutions de la batterie de serveurs, et la suppression des fichiers programme et des paramètres du Registre.  
  
 **[!INCLUDE[applies](../../includes/applies-md.md)]**  SharePoint 2013 | SharePoint 2010  
  
 **Dans cet article :**  
  
-   [Composants requis](#prereq)  
  
-   [Étape 1 : Check-list prédésinstallation](#bkmk_before)  
  
-   [Étape 2 : Supprimer des fonctionnalités et des solutions de SharePoint](#bkmk_remove)  
  
-   [Étape 3 : Exécuter le programme d’installation de SQL Server pour supprimer des programmes de l’ordinateur local](#bkmk_uninstall)  
  
-   [Étape 4 : Désinstaller le complément Power Pivot pour SharePoint](#bkmk_addin)  
  
-   [Étape 5 : Vérifier la désinstallation](#verify)  
  
-   [Étape 6 : Check-list postdésinstallation](#bkmk_post)  
  
##  <a name="prerequisites"></a><a name="prereq"></a> Conditions préalables  
  
-   Vous devez être administrateur de batterie de serveurs SharePoint ou administrateur d'application de service pour désinstaller des fonctionnalités et des solutions de la batterie.  
  
-   Vous devez être un administrateur système SQL Server et membre du groupe Administrateurs local si vous désinstallez également le moteur de base de données.  
  
-   Vous devez être un administrateur système Analysis Services et membre du groupe Administrateurs local pour désinstaller Analysis Services et [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)].  
  
##  <a name="step-1-pre-uninstall-checklist"></a><a name="bkmk_before"></a> Étape 1 : Check-list prédésinstallation  
 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] est désactivé une fois que le logiciel qui prend en charge le traitement des requêtes et des données est supprimé de la batterie. La première étape consiste à supprimer de façon préemptive les fichiers et les bibliothèques qui ne seront plus opérationnels. Cela vous permet de répondre à toutes les questions ou problèmes relatifs aux données manquantes avant de désinstaller le logiciel.  
  
1.  Supprimez tous les classeurs [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] , documents et bibliothèques associés à une installation [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] pour SharePoint. Ni les bibliothèques ni les documents ne fonctionneront une fois le logiciel désinstallé.  
  
    -   [Supprimer la galerie PowerPivot](/analysis-services/power-pivot-sharepoint/delete-power-pivot-gallery)  
  
    -   [Supprimer une bibliothèque de flux de données PowerPivot](/analysis-services/power-pivot-sharepoint/delete-a-power-pivot-data-feed-library)  
  
2.  Supprimez les classeurs Excel ou les rapports Reporting Services dans les autres bibliothèques contenant ou référençant des données [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] .  
  
3.  Supprimez tout composant WebPart dans un tableau de bord PerformancePoint qui référence des données [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] .  
  
4.  Examinez les autorisations SharePoint sur les sites et bibliothèques existants pour déterminer si vous devez les modifier. Certains scénarios d’accès aux données [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] , plus particulièrement d’accès aux données secondaires via une chaîne de connexion à une URL aux données [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] dans un autre classeur, nécessitent des autorisations d’accès en lecture, qui sont plus élevées que les autorisations d’affichage généralement affectées aux utilisateurs SharePoint qui visitent uniquement un site. Si vous n'avez plus besoin d'autorisations d'accès en lecture, vous pouvez réduire les autorisations en conséquence.  
  
5.  Éventuellement, arrêtez les services et attendez plusieurs jours avant de désinstaller le logiciel. Cette étape n'est pas nécessaire pour la désinstallation, mais elle vous permet de reprendre le service temporairement pendant que vous résolvez les problèmes de migration des données ou de substitution de technologie que vous avez omis.  
  
##  <a name="step-2-remove-features-and-solutions-from-sharepoint"></a><a name="bkmk_remove"></a> Étape 2 : Supprimer des fonctionnalités et des solutions de SharePoint  
 Pour supprimer des services [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] et des applications de SharePoint, utilisez l’outil de configuration de [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] .  
  
-   Vous devez être administrateur de batterie de serveurs, administrateur de serveur sur l’instance Analysis Services et **db_owner** sur la base de données de configuration de la batterie de serveurs.  
  
-   Utilisez la version appropriée de l'outil de configuration pour la version SharePoint. N'utilisez aucun de ces outils avec des installations de [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] .  
  
-   Vérifiez que le service Administration SharePoint est en cours d'exécution.  
  
1.  **Exécutez l'outil de configuration :** Notez que les outils de configuration sont répertoriés uniquement lorsque [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] est installé sur le serveur local. Dans le menu **Démarrer** , pointez sur **Tous les programmes**, cliquez sur [!INCLUDE[ssCurrentUI](../../includes/sscurrentui-md.md)], cliquez sur **Outils de configuration**, puis cliquez sur l'une des options suivantes :  
  
    -   **[!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] pour SharePoint 2013**  
  
    -   **[!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] Outil de configuration**  
  
2.  Sélectionnez **Supprimer des fonctionnalités, des services, des applications et des solutions** , puis cliquez sur **OK**.  
  
3.  Affichez éventuellement la fenêtre en plein écran. Vous devez voir une barre d'icônes en bas de la fenêtre qui comprend les commandes **Valider**, **Exécuter** et **Quitter** .  
  
4.  Passez en revue chaque action dans la liste des tâches afin de comprendre le rôle de chacune d'elles.  
  
     Dans **Supprimer des applications de service [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)]** , vous avez la possibilité de supprimer des données d’application associées à l’application de service. Les données d’application constituent une base de données SQL Server qui a été créée avec l’application de service afin de stocker les planifications d’actualisation des données, les informations d’instance de base de données, les données d’utilisation, ainsi que d’autres données utilisées par [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] pour SharePoint. Elles ne stockent pas les fichiers d’utilisateur, tels que les classeurs [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] . À moins d'avoir une raison particulière de conserver les données d'application (par exemple, si vous avez défini des stratégies de rétention des données associées à l'actualisation des données ou à l'accès aux données), vous pouvez supprimer la base de données d'application sans supprimer aucun des fichiers créés ou enregistrés par les utilisateurs de SharePoint.  
  
     Pour supprimer la base de données, sélectionnez **Supprimer des applications de service [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)]** , puis **Supprimer les données d’application associées à cette application de service**.  
  
5.  Consultez éventuellement les informations détaillées figurant sous l'onglet **Sortie** ou l'onglet **Script** .  
  
     L'onglet Sortie est un résumé des actions qui seront effectuées par l'outil. Ces informations sont enregistrées dans les fichiers journaux à l'emplacement :  
  
     `C:\Program Files\Microsoft SQL Server\130\Tools\PowerPivotTools\SPAddinConfiguration\Log`  
  
     L'onglet Script affiche les applets de commande PowerShell ou référence les fichiers de script PowerShell qui seront exécutés par l'outil.  
  
6.  Cliquez sur **Valider** pour vérifier si chaque action est valide. Si **Valider** n'est pas disponible, cela signifie que toutes les actions sont valides pour votre système.  
  
7.  Cliquez sur **Exécuter** pour exécuter toutes les actions qui sont valides pour cette tâche. **Exécuter** est disponible uniquement lorsque le contrôle de validation a réussi. Quand vous cliquez sur **Exécuter**, l’avertissement suivant s’affiche, vous rappelant que les actions sont traitées en mode batch : « Tous les paramètres de configuration marqués comme étant valides dans l’outil seront appliqués à la batterie de serveurs SharePoint. Voulez-vous continuer ? ».  
  
8.  Cliquez sur **OK** pour continuer.  
  
 **Dépannage des erreurs :**  
  
 Dans l'outil de configuration, affichez les informations d'erreur dans le volet Paramètres pour chaque action. Pour les problèmes liés au déploiement ou la rétraction de solution, vérifiez que le service Administration SharePoint a démarré. Ce service exécute les travaux du minuteur qui déclenchent des modifications de configuration dans une batterie de serveurs. Si le service ne s'exécute pas, le déploiement ou la rétraction de solution échoue. Des erreurs persistantes indiquent qu'un travail existant de déploiement ou de rétraction figure déjà dans la file d'attente et bloque toute action supplémentaire depuis l'outil de configuration. Utilisez la commande PowerShell suivante pour vérifier que le service est en cours d'exécution.  
  
```  
Get-Service | where {$_.displayname -like "*sharepoint* administration*"}  
```  
  
 Pour rechercher et supprimer un travail de déploiement ou de rétraction figurant déjà dans la file d'attente, procédez comme suit :  
  
1.  Pour toutes les autres erreurs, vérifiez les journaux ULS. Pour plus d’informations, consultez [Configurer et afficher les fichiers journaux SharePoint et la journalisation des diagnostics &#40;Power Pivot pour SharePoint&#41;](/analysis-services/power-pivot-sharepoint/configure-and-view-sharepoint-and-diagnostic-logging).  
  
2.  Démarrez SharePoint Management Shell en tant qu'administrateur, puis exécutez la commande suivante pour afficher les travaux dans la file d'attente :  
  
    ```  
    Stsadm -o enumdeployments  
    ```  
  
3.  Passez en revue les déploiements existants pour obtenir les informations suivantes : **Type** indique une rétraction ou un déploiement, **Fichier** correspond à powerpivotwebapp.wsp ou powerpivotfarm.wsp.  
  
4.  Pour les déploiements ou les rétractions liés aux solutions [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)], copiez la valeur GUID de **JobId**, puis collez-la dans la commande suivante (utilisez les commandes Mark, Copy et Paste du menu Edit de l’interpréteur de commandes afin de copier le GUID) :  
  
    ```  
    Stsadm -o canceldeployment -id "<GUID>"  
    ```  
  
5.  Réexécutez la tâche dans l'outil de configuration en cliquant sur **Valider** suivi d' **Exécuter**.  
  
 Ou bien, vous pouvez utiliser PowerShell pour supprimer des fonctionnalités et des solutions de la batterie. Pour plus d’informations, consultez [Référence PowerShell pour Power Pivot pour SharePoint](/analysis-services/powershell/powershell-reference-for-power-pivot-for-sharepoint).  
  
##  <a name="step-3-run-sql-server-setup-to-remove-programs-from-the-local-computer"></a><a name="bkmk_uninstall"></a> Étape 3 : Exécuter le programme d’installation de SQL Server pour supprimer des programmes de l’ordinateur local  
 La suppression des fichiers programme nécessite l'exécution du programme d'installation de SQL Server pour désinstaller le logiciel. La désinstallation supprime les fichiers et les entrées de Registre créés par le programme d'installation. Vous pouvez utiliser la page Programmes et fonctionnalités pour désinstaller le logiciel. Une installation de [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] fait partie d'une installation de SQL Server.  
  
 Vous pouvez désinstaller une partie d'une installation sans affecter les autres instances de SQL Server (ou les autres fonctionnalités de la même instance) déjà installées. Par exemple, vous pouvez désinstaller [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] pour SharePoint en laissant d’autres composants, tels que [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] ou le moteur de base de données, installés.  
  
1.  Sélectionnez **Microsoft SQL Server 2014 (64 bits)** dans la liste des programmes.  
  
2.  Cliquez sur **Désinstaller/Modifier**.  
  
3.  Cliquez sur **Supprimer**. Cela démarre le programme d'installation de SQL Server.  
  
     Dans le programme d’installation, vous pouvez sélectionner l’instance de **[!INCLUDE[ssGemini](../../includes/ssgemini-md.md)]** , puis sélectionner **Analysis Services** et **Intégration SharePoint pour Analysis Services** pour supprimer uniquement cette fonctionnalité et laisser tout le reste en place.  
  
##  <a name="step-4-uninstall-the-power-pivot-for-sharepoint-add-in"></a><a name="bkmk_addin"></a> Étape 4 : Désinstaller le complément Power Pivot pour SharePoint  
 Si votre déploiement [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] comporte plusieurs serveurs et que vous avez installé le complément [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] , désinstallez ce complément [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] de chaque serveur où il est installé pour désinstaller complètement tous les fichiers [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] . Pour plus d’informations, consultez [Installer ou désinstaller le complément Power Pivot pour SharePoint &#40;SharePoint 2013&#41;](/analysis-services/instances/install-windows/install-or-uninstall-the-power-pivot-for-sharepoint-add-in-sharepoint-2013).  
  
##  <a name="step-5-verify-uninstall"></a><a name="verify"></a> Étape 5 : Vérifier la désinstallation  
  
1.  Dans l’Administration centrale, dans **Gérer les services sur le serveur**, connectez-vous au serveur duquel vous avez désinstallé [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] pour SharePoint.  
  
2.  -   Si vous avez désinstallé [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] 2013, vérifiez que **Service système [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] SQL Server** ne s’affiche plus dans la liste.  
  
    -   Si vous avez désinstallé [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] 2010, vérifiez que **SQL Server Analysis Services** et **Service système [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] SQL Server** ne s’affichent plus dans la liste.  
  
3.  Après avoir désinstallé le dernier serveur [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] pour SharePoint dans la batterie de serveurs, procédez comme suit :  
  
    1.  Dans Gestion des applications, dans **Gérer les applications de service**, vérifiez qu’Application de service [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] ne s’affiche plus dans la liste.  
  
    2.  Dans Paramètres système, dans **Gérer les fonctionnalités des batteries de serveurs**, vérifiez que la Fonctionnalité d’intégration [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] n’est plus répertoriée. Dans **Gérer les solutions de la batterie**, vérifiez que les solutions [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] ne sont plus répertoriées.  
  
    3.  Dans Supervision, dans **Configurer la journalisation des diagnostics** et **Configurer la collecte des données d’utilisation et d’intégrité**, vérifiez que les événements et catégories d’événements [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] ne sont plus répertoriés.  
  
    4.  Dans Paramètres généraux de l’application, vérifiez que **Tableau de bord de gestion [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)]** n’est plus répertorié.  
  
##  <a name="step-6-post-uninstall-checklist"></a><a name="bkmk_post"></a> Étape 6 : Check-list postdésinstallation  
 Utilisez la liste ci-dessous pour supprimer les logiciels et fichiers qui n'ont pas été supprimés pendant la désinstallation.  
  
1.  Supprimez tous les fichiers de données et les sous-dossiers de `C:\Program Files\Microsoft SQL Server\MSAS13.PowerPivot`, puis supprimez le dossier lui-même. Cette étape supprime également des fichiers mis en cache précédemment dans le répertoire DATA.  
  
2.  Supprimez tous les classeurs [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] , documents et bibliothèques si vous ne l’avez pas déjà fait.  
  
    -   [Supprimer la galerie PowerPivot](/analysis-services/power-pivot-sharepoint/delete-power-pivot-gallery)  
  
    -   [Supprimer une bibliothèque de flux de données PowerPivot](/analysis-services/power-pivot-sharepoint/delete-a-power-pivot-data-feed-library)  
  
3.  Dans le Service Banque d’informations sécurisé, supprimez toutes les applications cibles qui contiennent des informations d’identification stockées utilisées par [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] pour SharePoint. Quelques entrées (mais pas toutes) du service Banque d’informations sécurisé sont supprimées lorsque vous désinstallez [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] pour SharePoint. Les applications cibles créées pour le compte d’actualisation des données [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] sans assistance plus toutes les applications cibles que vous avez créées pour l’actualisation des données existent encore et doivent être supprimées manuellement.  
  
     À l’inverse, les applications cibles individuelles qui ont été générées automatiquement par le service système [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] sont supprimées automatiquement lorsque [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] est désinstallé.  
  
4.  Dans le Panneau de configuration, cliquez sur **Programmes**, puis sur **Désinstaller un programme** . Désinstallez toutes les bibliothèques clientes Analysis Services qui ne sont plus utilisées. Les bibliothèques Analysis Services ADOMD.NET et Microsoft SQL Server AMO (Analysis Management Objects) ne sont pas supprimées lorsque vous désinstallez [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] pour SharePoint. Étant donné que les bibliothèques peuvent être utilisées par d'autres programmes qui utilisent des données Analysis Services, le programme d'installation de SQL Server ne les désinstalle pas automatiquement. Vous devez désinstaller ces bibliothèques clientes individuellement si vous n'en avez plus besoin.  
  
     Ne désinstallez pas le Complément SQL Server Reporting Services pour SharePoint 2010, sauf si vous suivez une procédure de résolution des problèmes ou d'installation qui vous demande spécifiquement de le désinstaller. Le complément Reporting Services est utilisé par Access Services. Il est installé par l'outil de préparation des produits SharePoint et doit rester sur le système pour prendre en charge des fonctionnalités requises par SharePoint.  
  
     Ne désinstallez pas le fournisseur OLE DB Analysis Services. SharePoint installe le fournisseur OLE DB comme composant requis pour les classeurs Excel qui se connectent aux bases de données Analysis Services. [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] installe une version plus récente, mais cette version offre une compatibilité descendante. Vous devez donc la conserver sur le système pour éviter les problèmes de connexion de données ultérieurement.  
  
## <a name="see-also"></a>Voir aussi  
 [Installer ou désinstaller le complément Power Pivot pour SharePoint &#40;SharePoint 2013&#41;](/analysis-services/instances/install-windows/install-or-uninstall-the-power-pivot-for-sharepoint-add-in-sharepoint-2013)   
 [Outils de configuration de Power Pivot](/analysis-services/power-pivot-sharepoint/power-pivot-configuration-tools)  
  
