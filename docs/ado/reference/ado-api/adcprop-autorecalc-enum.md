---
description: ADCPROP_AUTORECALC_ENUM
title: ADCPROP_AUTORECALC_ENUM | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- ADCPROP_AUTORECALC_ENUM
helpviewer_keywords:
- ADCPROP_AUTORECALC_ENUM [ADO]
ms.assetid: ded4f087-87b9-4efa-8026-bde53d3e9e8a
author: rothja
ms.author: jroth
ms.openlocfilehash: 9c29d26af25a09c659fe141584b2e0200a674142
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100035849"
---
# <a name="adcprop_autorecalc_enum"></a>ADCPROP_AUTORECALC_ENUM
Spécifie quand le fournisseur [MSDataShape](../../guide/appendixes/microsoft-data-shaping-service-for-ole-db-ado-service-provider.md) recalcule les colonnes agrégées et calculées dans un jeu d’enregistrements hiérarchique.  
  
 Ces constantes sont utilisées uniquement avec le fournisseur **MSDataShape** et la propriété dynamique «**auto Recalc**» du **jeu d’enregistrements** , qui est référencée dans l' [index de propriété dynamique ADO](./ado-dynamic-property-index.md) et documentée dans la documentation [Microsoft Cursor service for OLE DB](../../guide/appendixes/microsoft-cursor-service-for-ole-db-ado-service-component.md) ou [Microsoft Data Shaping Service pour OLE DB](../../guide/appendixes/microsoft-data-shaping-service-for-ole-db-ado-service-provider.md) .  
  
|Constante|Valeur|Description|  
|--------------|-----------|-----------------|  
|**adRecalcAlways**|1|Par défaut. Recalcule chaque fois que le fournisseur **MSDataShape** détermine les valeurs dont dépendent les colonnes calculées.|  
|**adRecalcUpFront**|0|Calcule uniquement lors de la création initiale du **Recordset** hiérarchique.|  
  
## <a name="adowfc-equivalent"></a>Équivalent ADO/WFC  
 Ces constantes n’ont pas d’équivalents ADO/WFC.