---
title: Gestion des parties de rapports | Documents Microsoft
description: Organisez des parties de rapports pour plusieurs utilisateurs et rapports, avec des options flexibles pour la publication, les autorisations et la sécurité.
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-design
ms.topic: conceptual
ms.assetid: 41947b4c-8ecf-4e4f-b30e-66e1d6692b74
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 1ac27d7f3f83a65f6a9e7b06da511bd9b9baf751
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100062634"
---
# <a name="managing-report-parts"></a>Gestion de parties de rapport
  Les parties de rapports peuvent être réutilisées dans les rapports paginés par plusieurs utilisateurs et dans plusieurs rapports. Les utilisateurs peuvent rechercher des parties de rapports sur le serveur et les ajouter à un rapport.  Les utilisateurs peuvent également être informés des mises à jour apportées à la partie de rapport sur le serveur et republier de nouvelles versions d'une partie de rapport. Ces actions de création du rapport peuvent être affectées et contrôlées par les autorisations de sécurité de Reporting Services.  Cette rubrique passe en revue les propriétés et le comportement des parties de rapports une fois que celles-ci sont sur le serveur.  
  
## <a name="managing-report-parts"></a>Gestion de parties de rapport  
 Pour gérer des parties de rapports, vous pouvez utiliser le portail web [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] pour un serveur de rapports en mode natif, ou les pages d’application pour un serveur de rapports en mode intégré SharePoint.  
  
### <a name="server-side-interaction-and-search"></a>Interaction et recherche côté serveur  
 Les parties de rapports peuvent être publiées sur un serveur de rapports en mode natif ou en mode intégré SharePoint. Les utilisateurs peuvent utiliser la fonctionnalité de bibliothèque de parties de rapports dans une application de création de rapports telle que le Générateur de rapports de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour rechercher et ajouter des parties de rapports à leurs rapports. Lorsqu'un utilisateur recherche une partie de rapport, la recherche regarde le catalogue du serveur de rapports indépendamment du mode pour lequel le serveur a été installé.  
  
 Lorsque les parties de rapports sont publiées à partir d'une application de création de rapports telle que le Générateur de rapports, sur un serveur de rapports en mode intégré SharePoint, le catalogue du serveur de rapports est également mis à jour et les recherches à partir de la bibliothèque reflètent précisément la partie de rapport nouvelle ou mise à jour.  
  
#### <a name="directly-uploading-report-parts-to-a-sharepoint-folder"></a>Téléchargement direct de parties de rapports dans un dossier SharePoint  
 Si une partie de rapport est téléchargée directement dans un dossier de documents SharePoint plutôt que publiée à partir d'une application de création de rapports, le catalogue du serveur de rapports n'est pas mis à jour. Les recherches de la bibliothèque de parties de rapports ne trouveront pas la partie de rapport téléchargée. Pour aider à garder vos dossiers SharePoint et votre catalogue du serveur de rapports synchronisés, vous pouvez activer la fonctionnalité de synchronisation des fichiers de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] sur le serveur SharePoint. Pour plus d'informations, consultez [Activer la fonctionnalité Synchronisation de fichiers de serveur de rapports dans l'Administration centrale de SharePoint](../../reporting-services/report-server-sharepoint/activate-the-report-server-file-sync-feature-in-sharepoint-ca.md).  
  
 Les fichiers peuvent également être synchronisés en appelant certaines API d'administration Reporting Services telles que GetProperties et SetProperties.  
  
### <a name="organizing-and-moving-report-parts"></a>Organisation et déplacement de parties de rapports  
 Vous devez prévoir et planifier la manière dont votre équipe utilisera et organisera les parties de rapports, les datasets partagés et les sources de données partagées. Bien que vous puissiez les déplacer ultérieurement, des problèmes peuvent survenir.  
  
#### <a name="native-mode-report-server"></a>Serveur de rapports en mode natif  
 Si vous déplacez une partie de rapport sur un serveur de rapports en mode natif vers tout autre dossier sur le même serveur, cela n'affecte pas la capacité des applications de création de rapports à rechercher ou télécharger des mises à jour de la partie de rapport. En effet, le serveur compte sur la propriété ComponentID unique. Toutefois, si la partie de rapport est déplacée vers un dossier pour lequel un utilisateur ne possède pas d'autorisations, ses recherches ne seront pas en mesure de trouver la partie de rapport et ses rapports ne seront pas notifiés lorsque des mises à jour seront effectuées sur la partie de rapport.  
  
#### <a name="report-server-in-sharepoint-integrated-mode"></a>Serveur de rapports en mode intégré SharePoint  
 Déplacer des parties de rapport vers une bibliothèque de documents ou un dossier différent a le même effet que les télécharger directement sur un serveur SharePoint : le catalogue du serveur de rapports n'est pas synchronisé. Pour éviter cela, activez la fonctionnalité Synchronisation de fichiers de serveur de rapports sur le serveur SharePoint.  
  
 Les sous-dossiers font exception. La recherche les recherchera toujours, donc si vous déplacez manuellement une partie de rapport vers un sous-dossier, elle apparaîtra toujours dans une recherche à partir de la bibliothèque de parties de rapports.  
  
### <a name="report-server-catalog-properties"></a>Propriétés du catalogue du serveur de rapports  
 La table suivante indique le lien qui existe entre les champs de catalogue du serveur de rapports existants et une partie de rapport, ainsi que les nouveaux champs ajoutés au catalogue pour les parties de rapport. Ceux-ci sont exposés dans le portail web [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] et les bibliothèques SharePoint, ainsi que dans les applications de création de rapports telles que le Générateur de rapports.  
  
 (*) indique qu'il s'agit d'une nouveauté pour cette version.  
  
|Propriété|Description|Partie de rapport<br /><br /> Critères de recherche de bibliothèque|  
|--------------|-----------------|---------------------------------------------|  
|Nom|Il s'agit de l'un des critères qu'un utilisateur peut rechercher dans la bibliothèque de parties de rapports.|Oui|  
|Description|Vous pouvez organiser les noms des parties de rapports de manière à simplifier les recherches des utilisateurs dans la bibliothèque. Par exemple, vous pouvez rechercher la description qui commence par « Ventes>> » pour trouver toutes les parties de rapports impliquant une présentation et des données associées aux ventes.|Oui|  
|CreatedBy|ID de l'utilisateur qui a ajouté la partie de rapport à la base de données du serveur de rapports. Le format exact dépend de la méthode d'authentification. Par exemple, certaines méthodes d'authentification provoquent l'affichage complet du domaine\nom_utilisateur dans les champs CreatedBy et ModifiedBy.|Oui|  
|CreationDate|Date à laquelle la partie de rapport a été créée à l'origine.<br /><br /> Il s'agit de l'un des critères qu'un utilisateur peut rechercher dans la bibliothèque de parties de rapports.|Oui|  
|ModifiedBy|ModifiedBy est l'ID de l'utilisateur auteur des dernières modifications apportées à la partie de rapport.|Oui|  
|DateModification|Date à laquelle la partie de rapport a été modifiée pour la dernière fois sur le serveur.<br /><br /> Ce champ est utilisé dans le cadre de la logique pour déterminer le moment où il existe des mises à jour côté serveur associées à une partie de rapport. Pour plus d'informations, consultez la description de la propriété ComponentID ultérieurement dans cette table.|Oui|  
|SubType (*)|SubType est une chaîne qui indique le type de partie de rapport à rechercher, par exemple « Tableau matriciel » ou « Graphique ».|Oui|  
|ComponentID (*)|ComponentID est un identificateur unique pour la partie de rapport. Il s'agit d'un nouveau champ ajouté au catalogue qui est visible à la fois sur le serveur et sur les applications de création de rapports telles que le Générateur de rapports.<br /><br /> Ce champ est utilisé par les applications clientes lors de la vérification du serveur en vue de détecter des mises à jour d'une partie de rapport. L'application cliente recherche sur le serveur les identificateurs ComponentID qui se trouvent dans le rapport côté client en cours. Lorsqu'il existe un ComponentID correspondant, la valeur ModifiedDate est comparée à la valeur SyncDate côté client de l'élément de rapport.|N0|  
  
## <a name="controlling-access-to-report-parts"></a>Contrôle de l'accès aux parties de rapports  
 Les tableaux suivants indiquent les attributions de rôles par défaut et la manière dont elles vous permettent d'effectuer diverses opérations. Les noms d’attribution de rôle varient en fonction du type de serveur de rapports utilisé.  
  
### <a name="server-in-native-mode"></a>Serveur en mode natif  
  
|Actions|Rôles|  
|-------------|-----------|  
|Ajouter, supprimer, modifier les propriétés de l'élément, gérer la sécurité et télécharger des parties de rapport|Gestionnaire de contenu<br /><br /> Mes rapports|  
|Ajouter, supprimer et télécharger des parties de rapport|Serveur de publication|  
|Rechercher et réutiliser|Browser<br /><br /> Générateur de rapports|  
  
### <a name="server-in-sharepoint-integrated-mode"></a>Serveur en mode intégré SharePoint  
  
|Actions|Role|  
|-------------|----------|  
|Ajouter, supprimer, modifier les propriétés de l'élément, gérer la sécurité et télécharger des parties de rapport|Contrôle total|  
|Ajouter, supprimer, modifier les propriétés de l'élément et télécharger des parties de rapport|Conception<br /><br /> Participez|  
|Rechercher et réutiliser|Lire<br /><br /> Vue seule|  
  
### <a name="security-considerations"></a>Considérations relatives à la sécurité  
  
-   Quand les définitions de parties de rapport sont réutilisées dans un rapport, elles sont copiées en intégralité dans la définition de rapport, avec la propriété ComponentID d’identification. Si une partie de rapport est mise à jour sur le serveur, les utilisateurs peuvent choisir de télécharger les parties de rapports mises à jour dans leur rapport. Les mises à jour sont également des copies complètes de la partie de rapport et remplacent la version existante de la partie de rapport qui était dans le rapport.  
  
    > [!IMPORTANT]  
    >  Dans chacune de ces étapes, vérifiez que les parties de rapports sont réutilisées dans les rapports à partir d'emplacements et d'utilisateurs de confiance.  
  
-   Les parties de rapports utilisent les mêmes stratégies d’autorisation que le type d’élément « Resource » existant. Dans un dossier, il n'y a aucune différenciation entre les éléments de ressource traditionnels et les parties de rapports, du point de vue de l'héritage de la sécurité. La partie de rapport héritera de la même stratégie d'autorisation que les images qui figurent dans le même dossier. Lorsque cette distinction est nécessaire, la sécurité au niveau des éléments peut être configurée pour les parties de rapports souhaitées. Sinon, vous pouvez placer les parties de rapports dans des dossiers séparés avec les autorisations appropriées configurées.  
  
## <a name="see-also"></a>Voir aussi  
 [Parties de rapports et datasets dans le Générateur de rapports](../../reporting-services/report-data/report-parts-and-datasets-in-report-builder.md)   
 [Gestion du contenu du serveur de rapports &#40;SSRS en mode natif&#41;](../../reporting-services/report-server/report-server-content-management-ssrs-native-mode.md)   
 [Parties de rapports dans le Concepteur de rapports &#40;SSRS&#41;](../../reporting-services/report-design/report-parts-in-report-designer-ssrs.md)  
  
  
