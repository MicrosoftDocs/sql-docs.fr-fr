---
title: IMise en œuvre d’une extension pour le traitement des données | Microsoft Docs
description: Découvrez comment créer un pont entre une source de données et un jeu de données dans Reporting Services en implémentant une extension pour le traitement des données.
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: extensions
ms.topic: reference
helpviewer_keywords:
- custom data processing extensions [Reporting Services]
- data sources [Reporting Services], data processing extensions
- data processing extensions [Reporting Services]
- extensions [Reporting Services], data processing
ms.assetid: 8dc2b44e-5ad9-411d-a29f-7213e29321a9
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: f4d4102c44cfedd25ed6a9870a303ebafb2515cf
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100065406"
---
# <a name="implementing-a-data-processing-extension"></a>Implémentation d'une extension pour le traitement des données
  Les extensions pour le traitement des données dans [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] vous permettent de vous connecter à une source de données et de récupérer des données. Elles servent également de pont entre une source de données et un dataset Les extensions pour le traitement des données [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] sont basées sur un sous-ensemble des interfaces de fournisseur de données [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)].  
  
## <a name="in-this-section"></a>Dans cette section  
 [Vue d'ensemble des extensions pour le traitement des données](../../../reporting-services/extensions/data-processing/data-processing-extensions-overview.md)  
 Explique comment écrire une extension personnalisée pour le traitement des données pour [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)].  
  
 [Préparation de la mise en œuvre d'une classe de commande pour une extension](../../../reporting-services/extensions/data-processing/preparing-to-implement-a-data-processing-extension.md)  
 Décrit les interfaces disponibles pour l'implémentation d'une extension pour le traitement des données [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)], ainsi que l'implémentation d'une interface particulière.  
  
 [Création d'une bibliothèque d'extensions pour le traitement des données](../../../reporting-services/extensions/data-processing/creating-a-data-processing-extension-library.md)  
 Décrit l'assignation d'un espace de noms pour votre extension pour le traitement des données [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] et la compilation de votre extension pour le traitement des données dans une DLL de bibliothèque.  
  
 [Mise en œuvre d'une classe de connexion pour une extension pour le traitement des données](../../../reporting-services/extensions/data-processing/implementing-a-connection-class-for-a-data-processing-extension.md)  
 Décrit les attributs d’une connexion et comment implémenter votre propre classe **Connection** pour votre extension pour le traitement des données.  
  
 [Mise en œuvre d'une classe de commande pour une extension pour le traitement des données](../../../reporting-services/extensions/data-processing/implementing-a-command-class-for-a-data-processing-extension.md)  
 Décrit les attributs d’une commande et comment implémenter votre propre classe **Command** pour votre extension pour le traitement des données.  
  
 [Mise en œuvre d'une classe DataReader pour une extension pour le traitement des données](../../../reporting-services/extensions/data-processing/implementing-a-datareader-class-for-a-data-processing-extension.md)  
 Décrit les attributs d’un lecteur de données et comment implémenter votre propre classe **DataReader** pour votre extension pour le traitement des données.  
  
 [Utilisation d'un jeu de données externe avec Reporting Services](../../../reporting-services/extensions/data-processing/using-an-external-dataset-with-reporting-services.md)  
 Décrit comment exposer vos objets **DataSet** personnalisés sur le serveur de rapports pour être exploité.  
  
 [Déploiement d'une extension pour le traitement des données](../../../reporting-services/extensions/data-processing/deploying-a-data-processing-extension.md)  
 Décrit comment déployer votre extension pour le traitement des données.  
  
 [Débogage du code des extensions pour le traitement des données](../../../reporting-services/extensions/data-processing/debugging-data-processing-extension-code.md)  
 Décrit comment déboguer le code dans vos extensions pour le traitement des données.  
  
 Pour un exemple d’extension pour le traitement des données totalement implémentée, consultez [SQL Server Reporting Services Product Samples](https://go.microsoft.com/fwlink/?LinkId=177889) (Exemples Reporting Services pour le produit SQL Server).  
  
## <a name="see-also"></a>Voir aussi  
 [Extensions Reporting Services](../../../reporting-services/extensions/reporting-services-extensions.md)   
 [Bibliothèque d'extensions Reporting Services](../../../reporting-services/extensions/reporting-services-extension-library.md)  
  
  
