---
title: Désinstaller et supprimer Master Data Services | Microsoft Docs
description: Ce processus de désinstallation supprime des dossiers et des fichiers Master Data Services et désinstalle le Gestionnaire de configuration Master Data Services de l’ordinateur local.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
ms.assetid: efc2431c-588b-42e7-b23b-c875145a33f6
author: cawrites
ms.author: chadam
ms.openlocfilehash: 4f57ed3d099afa9f85dcc72f489bac6bbcb713fc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336111"
---
# <a name="uninstall-and-remove-master-data-services"></a>Désinstaller et supprimer Master Data Services
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

   Pour désinstaller la fonctionnalité [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] d’une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], suivez la procédure [Désinstaller une instance existante de SQL Server &#40;programme d’installation&#41;](../../sql-server/install/uninstall-an-existing-instance-of-sql-server-setup.md) et spécifiez [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] comme fonctionnalité à supprimer dans la page **Sélectionner les composants**. Le processus de désinstallation supprime les dossiers et les fichiers [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] et désinstalle [!INCLUDE[ssMDScfgmgr](../../includes/ssmdscfgmgr-md.md)] de l'ordinateur local.  
  
 Pour empêcher la perte de données et l'affectation d'autres ordinateurs dans le système, certains éléments ne sont ni supprimés ni changés par le processus de désinstallation. Consultez le tableau suivant pour déterminer s'il faut laisser ou supprimer des éléments.  
  
|Élément|Description|  
|----------|-----------------|  
|Fichiers et dossiers|Le processus de désinstallation supprime la plupart des dossiers et fichiers du chemin d'installation.<br /><br /> Il ne supprime pas les dossiers Master Data Services et MDSTempDir de l'emplacement d'installation. Une fois le processus de désinstallation terminé, vous pouvez supprimer manuellement ces dossiers du système de fichiers. Pour plus d’informations, consultez [Autorisations d’accès aux dossiers et aux fichiers &#40;Master Data Services&#41;](../../master-data-services/folder-and-file-permissions-master-data-services.md).|  
|[!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] assemblys|Le processus de désinstallation supprime des assemblys [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] du Global Assembly Cache (GAC).|  
|Base de données|Le processus de désinstallation n'affecte pas la base de données [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] . La base de données reste intacte dans l’instance de [!INCLUDE[ssDE](../../includes/ssde-md.md)] afin que vous ne perdiez aucune donnée, notamment les données de référence, les objets modèle, les autorisations d’accès, les règles d’entreprise et ainsi de suite.<br /><br /> Si vous n'avez pas besoin de la base de données, et si vous ne prévoyez pas de la connecter à un autre site ou application Web [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] à l'avenir, vous pouvez la supprimer de l'instance de [!INCLUDE[ssDE](../../includes/ssde-md.md)] qui l'héberge. Pour plus d’informations, consultez [Supprimer une base de données](../../relational-databases/databases/delete-a-database.md).|  
|[!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] et Web.config|Le processus de désinstallation supprime le dossier WebApplication du système de fichiers. Le dossier WebApplication contient les fichiers de l'application Web et le fichier Web.config de [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)].<br /><br /> **\*\* Important \*\*** Avant de procéder à la désinstallation, vous pouvez copier le fichier Web.config vers un autre emplacement pour conserver tous les paramètres personnalisés ou d’autres informations contenues dans le fichier. Une fois le processus de désinstallation terminé, le fichier Web.config n'est pas récupérable.|  
|Éléments IIS (Internet Information Services)|Le processus de désinstallation n'affecte pas les pools d'applications, les sites Web ou les applications Web dans IIS sur l'ordinateur local. Étant donné que le processus de désinstallation supprime le dossier WebApplication et le fichier Web.config pour [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)], toutes les applications Web [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] qui requièrent ces fichiers ne serviront plus le contenu. Si des utilisateurs tentent d’accéder à l’application web, ils obtiennent l’erreur HTTP 500.19 de serveur interne : « La page demandée n’est pas accessible, car les données de configuration de la page sont erronées ».<br /><br /> Si vous n'avez plus besoin du site ou de l'application Web, ni du pool d'applications qui sert votre site ou votre application, vous pouvez utiliser un outil IIS pour les supprimer. Pour plus d’informations, consultez [Guide des opérations IIS 7](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc732976(v=ws.10)) sur [!INCLUDE[msCoName](../../includes/msconame-md.md)] TechNet.|  
|Groupe **MDS_ServiceAccounts**|La désinstallation ne supprime pas le groupe Windows **MDS_ServiceAccounts** et tous les comptes de service ajoutés au groupe. Si vous n'avez plus besoin du groupe ni des comptes, vous pouvez les supprimer.|  
|Registre|Le processus de désinstallation supprime toutes les clés de Registre [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] du Registre Windows.|  
  
## <a name="see-also"></a>Voir aussi  
 [Installer Master Data Services](../../master-data-services/install-windows/install-master-data-services.md)  
  
