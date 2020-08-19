---
description: Valeurs HOST_NAME
title: Valeurs HOST_NAME | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
f1_keywords:
- sql13.rep.newsubwizard.hostnamevalue.f1
ms.assetid: 21548f08-2910-4a55-baac-b911ba9afaf1
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 32926ab1c2e5dcd2c97d296d07c5fab725793f47
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88427951"
---
# <a name="host_name-values"></a>Valeurs HOST_NAME
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Les publications de fusion associées à des filtres paramétrés utilisent la fonction SUSER_SNAME() et/ou HOST_NAME() pour filtrer les données. La fonction concernée est spécifiée dans l'Assistant Nouvelle publication ou la boîte de dialogue **Propriétés de la publication** .  
  
Par défaut, la fonction HOST_NAME() retourne le nom de l'ordinateur qui se connecte au serveur de publication. Lors de l'utilisation de filtres paramétrés, il est fréquent de remplacer cette valeur par une autre dans cette page de l'Assistant. La fonction HOST_NAME() retourne alors la valeur spécifiée à la place du nom de l'ordinateur. Pour plus d’informations, consultez la section « Substitution de la valeur de HOST_NAME() » dans [Filtres paramétrés - Filtres de lignes paramétrés](../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md).  
  
> [!NOTE]  
>  Si vous remplacez la valeur de HOST_NAME(), tous les appels de la fonction HOST_NAME() retourneront la valeur que vous avez spécifiée. Assurez-vous que les autres applications ne dépendent pas de la fonction HOST_NAME() avec retour du nom de l'ordinateur.  
  
## <a name="options"></a>Options  
 **Propriétés de l'abonnement**  
 Entrez une valeur pour chaque abonné dans la colonne **Valeur HOST_NAME** ou acceptez la valeur par défaut, c'est-à-dire le nom de l'ordinateur de l'abonné.  
  
## <a name="see-also"></a>Voir aussi  
 [Create a Pull Subscription](../../relational-databases/replication/create-a-pull-subscription.md)   
 [Create a Push Subscription](../../relational-databases/replication/create-a-push-subscription.md)   
 [Afficher et modifier les propriétés d’un abonnement par extraction](../../relational-databases/replication/view-and-modify-pull-subscription-properties.md)   
 [Afficher et modifier les propriétés d’un abonnement par émission de données](../../relational-databases/replication/view-and-modify-push-subscription-properties.md)   
 [HOST_NAME &#40;Transact-SQL&#41;](../../t-sql/functions/host-name-transact-sql.md)   
 [S'abonner à des publications](../../relational-databases/replication/subscribe-to-publications.md)  
  
  
