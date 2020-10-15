---
title: Mettre en cache un dataset partagé | Microsoft Docs
description: Découvrez comment planifier l’expiration d’un jeu de données partagé mis en cache dans le Gestionnaire de rapports. La mise en cache des jeux de données partagés améliore les performances.
ms.date: 03/01/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server
ms.topic: conceptual
ms.assetid: c2d8c81a-da1e-4a8a-9845-fff9a0903d24
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 24bfa991596630165675bc0c8349a04c76085420
ms.sourcegitcommit: a41e1f4199785a2b8019a419a1f3dcdc15571044
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/13/2020
ms.locfileid: "91987195"
---
# <a name="cache-a-shared-dataset"></a>Mettre en cache un dataset partagé
  L'un des moyens d'améliorer les performances est de configurer les propriétés de mise en cache d'un dataset partagé. Lorsqu'un dataset partagé est mis en cache, une copie des résultats de la requête est enregistrée pour une période donnée. Le premier utilisateur qui demande un rapport utilisant le dataset partagé doit attendre que les résultats de la requête et l'ensemble du traitement soient terminés avant de consulter ce rapport. Les utilisateurs suivants qui demandent le rapport dans la période de mise en cache bénéficient de meilleures performances car la requête et le traitement ont déjà eu lieu. Vous pouvez également spécifier un plan d'actualisation du cache pour exécuter la requête et mettre en cache les résultats jusqu'à l'expiration du cache spécifiée.  
  
 Les utilisateurs qui exécutent des rapports basés sur un dataset partagé ou des plans d'actualisation du cache créent le cache de requête et dans les deux cas, le cache est disponible en fonction des options d'expiration du cache.  
  
 Il existe des restrictions concernant les types de datasets partagés que vous pouvez mettre en cache. Par exemple, les résultats de la requête ne peuvent pas être mis en cache si les données varient selon l'identité de l'utilisateur, ou si celles-ci sont récupérées à l'aide du jeton de sécurité de l'utilisateur qui demande le rapport. Pour plus d’informations, consultez [Mettre en cache les datasets partagés &#40;SSRS&#41;](../../reporting-services/report-server/cache-shared-datasets-ssrs.md) et [Mise en cache des rapports &#40;SSRS&#41;](../../reporting-services/report-server/caching-reports-ssrs.md).  
  
### <a name="to-schedule-the-expiration-of-a-cached-report"></a>Pour planifier l'expiration d'un rapport mis en cache  
  
1.  Démarrez le [Gestionnaire de rapports &#40;SSRS en mode natif&#41;](../web-portal-ssrs-native-mode.md).  
  
2.  Dans le Gestionnaire de rapports, accédez au dataset partagé pour lequel vous souhaitez définir des propriétés de mise en cache, pointez sur l'élément et cliquez sur la flèche déroulante.  
  
3.  Dans le menu déroulant, cliquez sur **Gérer**.  
  
4.  Dans le cadre de gauche, cliquez sur **Mise en cache**.  
  
    > [!NOTE]  
    >  Si le message d'erreur **Les informations d'identification utilisées pour exécuter le dataset partagé ne sont pas stockées**apparaît, l'option de mise en cache du dataset partagé est désactivée. Vous devez modifier la source de données pour stocker les informations d'identification ou modifier le dataset partagé de sorte qu'il utilise une source de données différente qui stocke les informations d'identification.  
  
5.  Sélectionnez **Mettre en cache le dataset partagé**.  
  
6.  Sélectionnez l'option pour faire expirer le cache après 30 minutes. Vous pouvez également choisir de faire expirer le cache selon une planification spécifiée.  
  
7.  Cliquez sur **Appliquer**.  
  
## <a name="see-also"></a>Voir aussi  
 [Gérer des datasets partagés](../../reporting-services/report-data/manage-shared-datasets.md)  
  
