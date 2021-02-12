---
title: Créer une base de données Master Data Services
description: Créez une base de données Master Data Services lorsque vous avez besoin d’une nouvelle base de données pour prendre en charge l’application Web Data Manager principale et le service Web Master Data Services.
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: 8373bb35-f0f9-4c3c-a53c-dfaa2ce567ac
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 464bfd4714e7cc7680b2a2800d1c2ae1711feeb1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272480"
---
# <a name="create-a-master-data-services-database"></a>Créer une base de données Master Data Services

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Créez une base de données [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] lorsque vous avez besoin d’une nouvelle base de données pour prendre en charge l’application web [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] et le service web [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] .  
  
## <a name="prerequisites"></a>Prérequis  
  
-   Pour plus d’informations sur la configuration requise pour l’ordinateur qui héberge la base de données, consultez [Configuration requise pour la base de données &#40;Master Data Services&#41;](../../master-data-services/install-windows/database-requirements-master-data-services.md).  
  
### <a name="to-create-a-master-data-services-database"></a>Pour créer une base de données Master Data Services  
  
1.  Ouvrez [!INCLUDE[ssMDScfgmgr](../../includes/ssmdscfgmgr-md.md)].  
  
2.  Dans le volet gauche, cliquez sur **Configuration de la base de données**.  
  
3.  Dans la page **Configuration de la base de données** , cliquez sur **Créer une base de données**.  
  
4.  Exécutez l’Assistant **Création d’une base de données** pour créer et configurer la base de données. Pour plus d’informations sur les options d’interface utilisateur de l’Assistant, consultez [Create Database Wizard &#40;Master Data Services Configuration Manager&#41;](../../master-data-services/create-database-wizard-master-data-services-configuration-manager.md) (Assistant Création d’une base de données (Gestionnaire de configuration de Master Data Services)).  
  
## <a name="next-steps"></a>Étapes suivantes  
  
-   Configurez les paramètres système pour la base de données et l'application Web. Pour plus d’informations, consultez [Paramètres système &#40;Master Data Services&#41;](../../master-data-services/system-settings-master-data-services.md).  
  
-   Créez une application web [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] à associer à cette base de données. Pour plus d’informations, consultez [Créer une application web Master Data Manager &#40;Master Data Services&#41;](../../master-data-services/install-windows/create-a-master-data-manager-web-application-master-data-services.md).  
  
-   Configurez un plan de maintenance pour sauvegarder la base de données et les journaux des transactions. Pour plus d’informations, consultez [Configuration requise pour la base de données &#40;Master Data Services&#41;](../../master-data-services/install-windows/database-requirements-master-data-services.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Installer Master Data Services](../../master-data-services/install-windows/install-master-data-services.md)  
  
  
