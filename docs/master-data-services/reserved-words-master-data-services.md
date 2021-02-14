---
description: Mots réservés (Master Data Services)
title: Mots réservés
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- reserved words [Master Data Services]
- Master Data Services, reserved words
ms.assetid: 88afd0d0-4362-4394-8357-4e65388fc0fc
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: f87125b2c72ac5c8242965dcb3d0407a4a15614c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340218"
---
# <a name="reserved-words-master-data-services"></a>Mots réservés (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Dans [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], lorsque vous créez des objets de modèle ou des membres, certains mots ne peuvent pas être utilisés. Ces mots peuvent provoquer des erreurs.  
  
> [!NOTE]  
>  Vous devez également limiter votre utilisation des caractères spéciaux (symboles, césure, etc.).  
  
-   [Modèles](../master-data-services/reserved-words-master-data-services.md#models)  
  
-   [Entités](../master-data-services/reserved-words-master-data-services.md#entities)  
  
-   [Hiérarchies explicites](../master-data-services/reserved-words-master-data-services.md#exhierarchies)  
  
-   [Attributs](../master-data-services/reserved-words-master-data-services.md#attributes)  
  
-   [Members](../master-data-services/reserved-words-master-data-services.md#members) (Membres)  
  
##  <a name="models"></a><a name="models"></a> Axisymétriques  
 Si vous créez un modèle dont le nom est défini sur **Nom** ou **Code**, ne sélectionnez pas **Créer une entité portant le même nom que le modèle** , car **Nom** ou **Code** ne peut pas être utilisé pour le nom d’une entité.  
  
##  <a name="entities"></a><a name="entities"></a> Lesquelles  
 Pour les noms d'entité, vous ne pouvez pas utiliser **Name** ou **Code**.  
  
##  <a name="explicit-hierarchies"></a><a name="exhierarchies"></a> Hiérarchies explicites  
 Pour les noms de hiérarchies explicites, vous ne pouvez pas utiliser **Name** ou **Code**.  
  
##  <a name="attributes"></a><a name="attributes"></a> Attributs  
  
-   **Identifiant**  
  
-   **Code**  
  
-   **EnterUserName**  
  
-   **LastChgUserName**  
  
-   **Nom**  
  
-   **EnterDTM**  
  
-   **EnterUserID**  
  
-   **EnterUserName**  
  
-   **LastChgDTM**  
  
-   **LastChgUserID**  
  
-   **Status_ID**  
  
-   **ValidationStatus_ID**  
  
-   **Version_ID**  
  
##  <a name="members"></a><a name="members"></a> Membres  
 Pour les membres, vous ne pouvez pas utiliser **MDMMemberStatus**, **MDMUnused** ou **ROOT** pour la valeur d’attribut **Code** .  
  
## <a name="see-also"></a>Voir aussi  
 [Vue d’ensemble de Master Data Services &#40;MDS&#41;](../master-data-services/master-data-services-overview-mds.md)  
  
  
