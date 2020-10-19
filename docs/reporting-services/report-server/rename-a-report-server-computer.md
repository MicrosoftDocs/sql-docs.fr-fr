---
title: Renommer un ordinateur serveur de rapports | Microsoft Docs
description: Découvrez comment reconfigurer un serveur de rapports après un changement de nom d’ordinateur. SQL Server Reporting Services peut ne pas être accessible après un changement de nom d’ordinateur.
ms.date: 06/19/2019
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server
ms.topic: conceptual
helpviewer_keywords:
- renaming report servers
ms.assetid: 82fc4ba2-291a-4939-a025-271b8d687c54
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 2fbe0a54b6f48c97c81b288b8eebb5514cc637ac
ms.sourcegitcommit: 783b35f6478006d654491cb52f6edf108acf2482
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91892409"
---
# <a name="rename-a-report-server-computer"></a>Changement de nom d'un ordinateur serveur de rapports
  Le renommage d’un ordinateur entraîne une modification équivalente du nom pour le serveur web et l’instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (si elle se trouve sur le même ordinateur). Dans certains cas, il est possible que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] ne soit pas accessible après la modification du nom d'un ordinateur. Suivez les instructions de cet article pour reconfigurer un serveur de rapports après un changement de nom d'ordinateur.  
  
## <a name="renaming-a-sql-server-database-engine"></a>Changement de nom d'un moteur de base de données SQL Server  
 Si vous renommez l'instance du  [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] qui exécute la base de données du serveur de rapports, procédez comme suit :  
  
1.  Démarrez l'outil de configuration de Reporting Services et connectez-vous au serveur de rapports qui utilise la base de données de serveur de rapports sur le serveur renommé.  
  
2.  Ouvrez la page Installation de la base de données.  
  
3.  Dans **Nom du serveur**, tapez ou sélectionnez le nom du serveur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , puis cliquez sur **Se connecter**.  
  
4.  Cliquez sur **Appliquer**.  
  
 Si le serveur de rapports utilise une instance de [!INCLUDE[ssDE](../../includes/ssde-md.md)] locale, vous pouvez utiliser *(local)* ou *(local)\nom_instance* pour spécifier le serveur. Si vous utilisez *(local)* pour faire référence au serveur, vous pourrez renommer le serveur et les connexions continueront à fonctionner. Si vous utilisez un serveur distant, ou si [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] est configuré à l'aide du nom du serveur, vous devez mettre à jour les informations de connexion à la base de données lorsque le nom du serveur est modifié.  
  
## <a name="renaming-a-report-server-computer"></a>Changement de nom d'un ordinateur serveur de rapports  
 Si vous renommez un ordinateur qui exécute un serveur de rapports, procédez comme suit :  
  
1.  Ouvrez RSReportServer.config dans un éditeur de texte et modifiez le paramètre **UrlRoot** pour qu'il tienne compte du nouveau nom du serveur. Le paramètre **UrlRoot** est utilisé par les extensions de remise pour composer l'URL permettant d'accéder aux éléments stockés sur le serveur de rapports. Pour changer l'adresse URL du serveur de rapports, vous devez mettre à jour le paramètre **UrlRoot** afin que les abonnements continuent à remettre des rapports comme prévu.  
  
2.  Dans le même fichier, s'il est défini, modifiez le paramètre **ReportServerUrl** en fonction du nouveau nom du serveur. Notez que ce paramètre n'est pas utilisé dans chaque installation. S'il est vide, ne faites rien.  
  
    > [!NOTE]  
    >  Si vous utilisez le service WINS (Windows Internet Naming Service) sur le réseau de votre entreprise, le serveur de rapports et le portail web peuvent continuer à être disponibles sous le nom précédent pendant une certaine période de temps. WINS mappe une adresse IP à chaque ordinateur qui utilise ce service. Une fois que WINS a actualisé l'adresse IP pour l'ordinateur renommé, l'ancien nom ne peut plus être utilisé pour accéder au serveur de rapports ou au portail web.  
  
## <a name="see-also"></a>Voir aussi  
 [RsReportServer.config Configuration File](../../reporting-services/report-server/rsreportserver-config-configuration-file.md)   
 [Gestionnaire de configuration du serveur de rapports &#40;mode natif&#41;](../../reporting-services/install-windows/reporting-services-configuration-manager-native-mode.md)   
 [Serveur de rapports Reporting Services &#40;mode natif&#41;](../../reporting-services/report-server/reporting-services-report-server-native-mode.md)   
 [Démarrer et arrêter le service Report Server](../../reporting-services/report-server/start-and-stop-the-report-server-service.md)   
 [Utilitaire rsconfig &#40;SSRS&#41;](../../reporting-services/tools/rsconfig-utility-ssrs.md)  
  