---
title: Vue d’ensemble des extensions pour le traitement des données | Microsoft Docs
description: Consultez les extensions pour le traitement des données incluses dans Reporting Services et apprenez à ajouter un traitement de données personnalisé à votre serveur de rapports.
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: extensions
ms.topic: reference
helpviewer_keywords:
- data processing extensions [Reporting Services], about extensions
ms.assetid: 1d652605-9313-4c75-98b4-ba4dcbbb222d
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 61c30aa9214b87e9acf74dc12d71dd9e8e6d6fa5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100061174"
---
# <a name="data-processing-extensions-overview"></a>Vue d'ensemble des extensions pour le traitement des données
  Les extensions pour le traitement des données dans [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] vous permettent de vous connecter à une source de données et de récupérer des données. Elles servent également de pont entre une source de données et un dataset Les extensions pour le traitement des données [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] sont basées sur un sous-ensemble des interfaces de fournisseur de données [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)].  
  
 Le tableau suivant répertorie les extensions pour le traitement des données fournies avec [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)].  
  
|Extension pour le traitement des données|Description|  
|-------------------------------|-----------------|  
|Extension pour le traitement des données pour [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]|Utilise le fournisseur de données .NET Framework pour SQL Server pour se connecter au [!INCLUDE[ssDEnoversion](../../../includes/ssdenoversion-md.md)] et récupérer des données de celui-ci.|  
|Extension pour le traitement des données pour OLE DB|Utilise le fournisseur de données .NET Framework pour OLE DB. Avec cette extension, le serveur de rapports peut interroger n'importe quelle source de données possédant un fournisseur OLE DB.|  
|Extension pour le traitement des données pour Oracle|Utilise le fournisseur de données .NET Framework pour Oracle. Avec cette extension, le serveur de rapports peut accéder à des sources de données Oracle par le biais du logiciel de connectivité client Oracle.|  
|Extension pour le traitement des données pour ODBC|Utilise le fournisseur de données .NET Framework pour ODBC. Avec cette extension, le serveur de rapports peut accéder aux données dans n'importe quelle base de données pour laquelle il existe un pilote ODBC.|  
  
 Vous pouvez utiliser l'API de traitement de données [!INCLUDE[ssRS](../../../includes/ssrs.md)] pour ajouter le traitement de données personnalisées à votre serveur de rapports.  
  
> [!NOTE]  
>  [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] intègre la prise en charge des fournisseurs de données dans le [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)]. Si vous avez déjà implémenté un fournisseur de données complet, vous n'avez pas besoin d'implémenter une extension pour le traitement des données [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)]. Toutefois, songez à étendre votre fournisseur de données de manière à inclure les fonctionnalités spécifiques à [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 2005, notamment les informations d'identification de connexion sécurisée et les agrégats côté serveur.  
  
 Chaque extension pour le traitement des données incluse avec [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] utilise un jeu commun d'interfaces. De cette manière, chaque extension implémente des fonctionnalités comparables.  
  
 Vous pouvez développer des extensions pour le traitement des données pour vos propres sources de données ou vous pouvez utiliser les interfaces pour ajouter une couche supplémentaire de traitement des données aux infrastructures de base de données courantes. Vous pouvez déployer vos extensions pour le traitement des données personnalisées pour permettre l'intégration transparente des données dans les serveurs de rapports existants dans votre organisation. Vous pouvez également les utiliser dans le cadre d'une suite de création de rapports personnalisée que vous fournissez à vos consommateurs.  
  
 ![Architecture d'extension pour le traitement de données](../../../reporting-services/extensions/data-processing/media/bk-dataprocess-extensions.gif "Architecture d'extension pour le traitement de données")  
Architecture de l'extension pour le traitement des données Reporting Services  
  
 Les avantages liés à l'implémentation d'une extension pour le traitement des données [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] personnalisée sont les suivants :  
  
-   une architecture d'accès aux données simplifiée, souvent associée à une gestion améliorée et à des performances accrues ;  
  
-   la possibilité d'exposer directement les fonctionnalités spécifiques à l'extension aux consommateurs ;  
  
-   une interface spécifique pour vos consommateurs pour accéder à votre source de données dans [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)].  
  
## <a name="data-extension-process-flow"></a>Flux de traitement de l'extension de données  
 Avant de développer votre extension de données personnalisée, vous devez comprendre comment le serveur de rapports utilise les extensions de données pour traiter les données. Vous devez également comprendre les constructeurs et les méthodes appelés par le serveur de rapports.  
  
 ![Flux de traitement pour l'extension de traitement de données](../../../reporting-services/extensions/data-processing/media/bk-ext-01.gif "Flux de traitement pour l'extension de traitement de données")  
Flux de traitement pas à pas d'une extension de données appelée par le serveur de rapports  
  
 L'illustration montre la séquence d'événements suivante :  
  
1.  Le serveur de rapports crée un objet de connexion et passe la chaîne de connexion et les informations d'identification associées au rapport.  
  
2.  Le texte de commande du rapport est utilisé pour créer un objet de commande. Dans le processus, l'extension pour le traitement des données peut inclure du code qui analyse le texte de la commande et crée d'éventuels paramètres pour la commande.  
  
3.  Une fois l'objet de commande et les éventuels paramètres traités, le lecteur de données généré retourne un jeu de résultats et active le serveur de rapports pour associer les données de rapport à la disposition du rapport.  
  
## <a name="developer-requirements"></a>Spécifications pour le développeur  
 Pour développer une extension pour le traitement des données [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)], vous devez posséder :  
  
-   un ordinateur de déploiement sur lequel est installé le Générateur de rapports ou un serveur de rapports ;  
  
-   un ordinateur de développement sur lequel est installé [!INCLUDE[vsprvsext](../../../includes/vsprvsext-md.md)], ou une version ultérieure, ou le SDK [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)] ;  
  
-   des connaissances approfondies des fonctionnalités et des capacités de [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] ;  
  
-   des connaissances approfondies de l’architecture [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[vstecado](../../../includes/vstecado-md.md)], des fournisseurs de données [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)], des objets DataSet ADO.NET et des interfaces [!INCLUDE[vstecado](../../../includes/vstecado-md.md)] communes ;  
  
-   de l’expérience en développement dans un langage [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)] tel que [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Visual C# ou [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[vbprvb](../../../includes/vbprvb-md.md)] .NET.  
  
## <a name="see-also"></a>Voir aussi  
 [Extensions Reporting Services](../../../reporting-services/extensions/reporting-services-extensions.md)   
 [Bibliothèque d'extensions Reporting Services](../../../reporting-services/extensions/reporting-services-extension-library.md)  
  
  
