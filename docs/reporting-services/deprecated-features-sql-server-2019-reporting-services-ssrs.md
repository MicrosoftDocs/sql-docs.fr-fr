---
title: Fonctionnalités déconseillées dans SQL Server 2019 Reporting Services | Microsoft Docs
description: Cet article décrit les fonctionnalités dans SQL Server 2019 Reporting Services qui seront déconseillées dans la prochaine version de SQL Server Reporting Services.
ms.date: 08/31/2020
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: reporting-services
ms.topic: conceptual
helpviewer_keywords:
- Reporting Services, backward compatibility
- deprecated features [Reporting Services]
- HTML OWC rendering extension [Reporting Services]
- Report Server Web service, endpoints
ms.assetid: 3876c01e-f81d-4cce-9104-5106a8c369e6
author: maggiesMSFT
ms.author: maggies
monikerRange: '>= sql-server-ver15'
ms.openlocfilehash: 0491d5ee88b7e7476b51bcdef4a08fc1c354ef7a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100065528"
---
# <a name="deprecated-features-in-sql-server-2019-reporting-services"></a>Fonctionnalités déconseillées dans SQL Server 2019 Reporting Services

[!INCLUDE [ssrs-appliesto](../includes/ssrs-appliesto.md)] [!INCLUDE [ssrs-appliesto-2019-and-later](../includes/ssrs-appliesto-2019-and-later.md)] [!INCLUDE [ssrs-appliesto-pbirs](../includes/ssrs-appliesto-pbirs.md)]

Lorsqu’une fonctionnalité est marquée comme déconseillée, cela signifie que :

- La fonctionnalité est en mode de maintenance uniquement. Aucune nouvelle modification ne sera apportée, notamment les modifications liées à l’interopérabilité avec de nouvelles fonctionnalités.
- Nous nous efforçons de ne pas retirer une fonctionnalité déconseillée des futures versions pour faciliter les mises à niveau. Cependant, dans de rares cas, nous pouvons décider de retirer définitivement une fonctionnalité de Reporting Services si elle limite de futures innovations.
- Pour un nouveau travail de développement, nous vous recommandons de ne pas utiliser des fonctionnalités déconseillées.

**Fonctionnalités déconseillées dans une future version de SQL Server**

SQL Server Reporting Services prend en charge les fonctionnalités suivantes dans la prochaine version de SQL Server, mais les déconseillent dans une version ultérieure. La version spécifique de SQL Server n’a pas été déterminée.

| **Catégorie** | **Fonctionnalité déconseillée** | **Remplacement** |
| --- | --- | --- |
| Serveur de rapports | Galerie de parties de rapports | None |
| Serveur de rapports | Rapports mobiles dans l’Éditeur de rapports mobiles | Rapports Power BI dans les fonctionnalités mobiles de l’offre Power BI Report Server. |
| Serveur de rapports | Formats de rendu XLS et DOC | Les formats XLSX et DOCX sont disponibles et pris en charge. |
| Serveur de rapports | Flux de données Atom | oLe support des flux de données est disponible pour les jeux de données partagés dans SSRS et Power BI Report Server. |
| Serveur de rapports | Épingler à Power BI | La prise en charge des rapports paginés est désormais disponible directement dans le service Power BI.  |

## <a name="see-also"></a>Voir aussi

[Fonctionnalités abandonnées dans SQL Server 2019 Reporting Services (SSRS)](discontinued-functionality-sql-server-reporting-services-2019.md)

D’autres questions ? [Essayez de poser une question dans le forum Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231)
