---
description: Formats de vue d'abonnement (Master Data Services)
title: Formats de vue d'abonnement
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: ff1e2566-ac8f-467d-a6d9-12c3f13879b9
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: ac58c9c1edfce6b02f48c31e220d37d842b18527
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88456733"
---
# <a name="subscription-view-formats-master-data-services"></a>Formats de vue d'abonnement (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Selon l'entité ou la hiérarchie dérivée sélectionnée, les formats suivants sont disponibles pour votre vue d'abonnement.  
  
## <a name="subscription-view-formats"></a>Formats de vue d'abonnement  
  
|Nom|Description|  
|----------|-----------------|  
|**Membres feuille**|Contient des membres feuille et leurs valeurs d'attribut associées.|  
|**Historique des membres feuille**|Contient les données historiques des membres feuille et les valeurs d’attribut qui leur sont associées. Le format d’affichage est Dimension à variation lente Type 4.|  
|**Membres feuille SCD Type 2**|Contient les données historiques et actuelles des membres feuille ainsi que les valeurs d’attribut qui leur sont associées. Le format d’affichage est Dimension à variation lente Type 2.|  
|**Membres consolidés**|Contient des membres consolidés et leurs valeurs d'attribut associées.|  
|**Historique des membres consolidés**|Contient les données historiques des membres consolidés et les valeurs d’attribut qui leur sont associées. Le format d’affichage est Dimension à variation lente Type 4.|  
|**Membres consolidés SCD Type 2**|Contient les données historiques et actuelles des membres consolidés ainsi que les valeurs d’attribut qui leur sont associées. Le format d’affichage est Dimension à variation lente Type 2.|  
|**Appartenances à la collection**|Contient une liste de collections et leurs valeurs d'attribut associées.|  
|**Regroupements**|Contient la liste des collections et les membres de chacune, avec les valeurs de pondération et l'ordre de tri.|  
|**Historique des membres de collection**|Contient les données historiques des membres de collection et les valeurs d’attribut qui leur sont associées. Le format d’affichage est Dimension à variation lente Type 4.|  
|**Membres de collection SCD Type 2**|Contient les données historiques et actuelles des membres de collection ainsi que les valeurs d’attribut qui leur sont associées. Le format d’affichage est Dimension à variation lente Type 2.|  
|**Enfant de parent explicite**|Contient des structures de hiérarchie explicite pour une entité dans un format enfant de parent.|  
|**Niveaux explicites**|Contient des structures de hiérarchie explicite pour une entité au format de niveau.|  
|**Enfant de parent dérivé (vue Hiérarchie dérivée)**|Contient une structure de hiérarchie dérivée au format enfant de parent.|  
|**Niveaux dérivés (vue Hiérarchie dérivée)**|Contient une structure de hiérarchie dérivée au format de niveau.|  
  
## <a name="see-also"></a>Voir aussi  
 [Vue d’ensemble : exportation de données &#40;Master Data Services&#41;](../master-data-services/overview-exporting-data-master-data-services.md)   
 [Créer une vue d’abonnement pour exporter des données &#40;Master Data Services&#41;](../master-data-services/create-a-subscription-view-to-export-data-master-data-services.md)  
  
  
