---
title: Implémentation d’une classe Connection pour une extension pour le traitement des données | Microsoft Docs
description: Implémentez un objet Connexion pour une extension pour le traitement des données dans Reporting Services. Découvrez les interfaces à implémenter et les éléments à exiger des clients.
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: extensions
ms.topic: reference
helpviewer_keywords:
- connections [Reporting Services], data processing extensions
- Connection class
- data processing extensions [Reporting Services], connections
ms.assetid: 7047d29e-a2c9-4e6f-ad02-635851a38ed7
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: d5b0c6cc93ea6b934be718e88873cf736f31d679
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100061674"
---
# <a name="implementing-a-connection-class-for-a-data-processing-extension"></a>Implémentation d'une classe Connection pour une extension pour le traitement des données
  L’objet **Connection** représente une connexion de base de données ou une ressource similaire et représente le point de départ pour les utilisateurs d’une extension pour le traitement des données [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)]. Il représente des connexions aux serveurs de base de données, bien que toute entité ayant un comportement similaire puisse être exposée comme un objet **Connection**.  
  
 Pour implémenter un objet **Connection**, créez une classe qui implémente <xref:Microsoft.ReportingServices.DataProcessing.IDbConnection> et peut implémenter <xref:Microsoft.ReportingServices.DataProcessing.IDbConnectionExtension>.  
  
 Dans votre implémentation, vous devez garantir qu'une connexion est créée et ouverte avant que les commandes puissent être exécutées. Veillez à ce que votre implémentation oblige les clients à ouvrir et fermer des connexions explicitement, plutôt qu'une implémentation qui ouvre et ferme des connexions implicitement pour le client. Effectuez vos vérifications de la sécurité lorsque la connexion est obtenue. La nécessité d'une connexion existante pour les autres classes dans votre extension pour le traitement des données [!INCLUDE[ssRS](../../../includes/ssrs.md)] garantit ensuite que les vérifications de la sécurité sont toujours effectuées lors de l'utilisation de votre source de données.  
  
 Les propriétés de la connexion souhaitée sont représentées sous la forme d'une chaîne de connexion. Il est fortement recommandé que les extensions pour le traitement des données [!INCLUDE[ssRS](../../../includes/ssrs.md)] prennent en charge la propriété <xref:Microsoft.ReportingServices.DataProcessing.IDbConnection.ConnectionString%2A> à l'aide du système de paire nom/valeur familier défini par OLE DB.  
  
> [!NOTE]  
>  Les objets **Connection** consomment souvent beaucoup de ressources, vous pouvez donc envisager de regrouper des connexions ou faire appel à d’autres techniques pour réduire ce problème.  
  
 <xref:Microsoft.ReportingServices.DataProcessing.IDbConnection> hérite de <xref:Microsoft.ReportingServices.Interfaces.IExtension>. Vous devez implémenter l'interface <xref:Microsoft.ReportingServices.Interfaces.IExtension> dans le cadre de votre implémentation de classe de connexion. L'interface <xref:Microsoft.ReportingServices.Interfaces.IExtension> permet à une classe d'implémenter un nom d'extension localisé et de traiter des informations de configuration spécifiques à l'extension stockées dans le fichier de configuration [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)].  
  
 Votre objet **Connection** contient la propriété <xref:Microsoft.ReportingServices.Interfaces.IExtension.LocalizedName%2A> dans le cadre de son implémentation de <xref:Microsoft.ReportingServices.Interfaces.IExtension>. Il est fortement recommandé que les extensions pour le traitement des données [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] prennent en charge la propriété <xref:Microsoft.ReportingServices.Interfaces.IExtension.LocalizedName%2A>, afin que les utilisateurs soient confrontés à un nom localisé familier pour l'extension dans une interface utilisateur, telle que Gestionnaire de rapports.  
  
 <xref:Microsoft.ReportingServices.Interfaces.IExtension> permet également à votre objet **Connection** d’extraire et traiter les données de configuration personnalisées stockées dans le fichier RSReportServer.config. Pour plus d'informations sur le traitement des données de configuration personnalisées, consultez la méthode <xref:Microsoft.ReportingServices.Interfaces.IExtension.SetConfiguration%2A>.  
  
 La classe qui implémente <xref:Microsoft.ReportingServices.Interfaces.IExtension> n'est pas déchargée de la mémoire lorsque vos classes restantes d'extension pour le traitement des données sont déchargées. De ce fait, vous pouvez utiliser votre classe **Extension** pour stocker des informations d’état intra-connexion ou stocker des données qui peuvent être mises en cache dans la mémoire. Votre classe **Extension** reste en mémoire durant l’exécution du serveur de rapports.  
  
 Vous pouvez étendre votre classe **Connection** pour inclure la prise en charge des informations d’identification dans [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] en implémentant <xref:Microsoft.ReportingServices.DataProcessing.IDbConnectionExtension>. Quand vous implémentez les propriétés <xref:Microsoft.ReportingServices.DataProcessing.IDbConnectionExtension.IntegratedSecurity%2A>, <xref:Microsoft.ReportingServices.DataProcessing.IDbConnectionExtension.UserName%2A> et <xref:Microsoft.ReportingServices.DataProcessing.IDbConnectionExtension.Password%2A> de l’interface <xref:Microsoft.ReportingServices.DataProcessing.IDbConnectionExtension>, vous cochez la case **Sécurité Intégrée** et activez les zones de texte **Nom d’utilisateur** et **Mot de passe** de la boîte de dialogue **Source de données** dans le Concepteur de rapports. Cette opération permet au Générateur de rapports de stocker et d'extraire des informations d'identification pour les sources de données qui prennent en charge l'authentification. Les informations d'identification sont stockées de manière sécurisée et utilisées lors du rendu des rapports en mode aperçu.  
  
> [!NOTE]  
>  L'implémentation implicite de <xref:Microsoft.ReportingServices.DataProcessing.IDbConnectionExtension> nécessite d'implémenter les membres des interfaces <xref:Microsoft.ReportingServices.DataProcessing.IDbConnection> et <xref:Microsoft.ReportingServices.Interfaces.IExtension>.  
>   
>  Pour un exemple d’implémentation de la classe **Connection**, consultez [SQL Server Reporting Services Product Samples](https://go.microsoft.com/fwlink/?LinkId=177889) (Exemples Reporting Services pour le produit SQL Server).  
  
## <a name="see-also"></a>Voir aussi  
 [Extensions Reporting Services](../../../reporting-services/extensions/reporting-services-extensions.md)   
 [Implémentation d’une extension pour le traitement des données](../../../reporting-services/extensions/data-processing/implementing-a-data-processing-extension.md)   
 [Bibliothèque d'extensions Reporting Services](../../../reporting-services/extensions/reporting-services-extension-library.md)  
  
  
