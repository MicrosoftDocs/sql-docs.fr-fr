---
description: Persistance des recordsets hiérarchiques
title: Persistance des recordsets hiérarchiques | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- hierarchical Recordsets [ADO], persisting
- persisting hierarchical Recordsets [ADO]
- data shaping [ADO], hierarchical Recordsets
ms.assetid: 43798bb5-98a6-4ad6-9bf8-78154b3a1827
author: rothja
ms.author: jroth
ms.openlocfilehash: ddcda766d729e085ed654757d0125f18ec7a696a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037189"
---
# <a name="persisting-hierarchical-recordsets"></a>Persistance des recordsets hiérarchiques
Vous pouvez enregistrer un **jeu d’enregistrements** hiérarchique dans un fichier au format ADTG ou XML en appelant la méthode [Save](../../../ado/reference/ado-api/save-method.md) . Toutefois, deux limitations s’appliquent lors de l’enregistrement des **jeux d’enregistrements** hiérarchiques au format XML : vous ne pouvez pas enregistrer dans XML si le **jeu d’enregistrements** hiérarchique contient des mises à jour en attente et que vous ne pouvez pas enregistrer un **jeu d’enregistrements** hiérarchique paramétré.  
  
 Pour plus d’informations sur le fournisseur de mise en forme des données, consultez [Microsoft Data Shaping Service for OLE DB](../../../ado/guide/appendixes/microsoft-data-shaping-service-for-ole-db-ado-service-provider.md) (ADO) et [vue d’ensemble du service de mise en forme des données pour OLE DB](/previous-versions/windows/desktop/ms719615(v=vs.85)).  
  
## <a name="see-also"></a>Voir aussi  
 [Exemple de mise en forme des données](../../../ado/guide/data/data-shaping-example.md)   
 [Grammaire de forme formelle](../../../ado/guide/data/formal-shape-grammar.md)   
 [Généralités sur les commandes SHAPE](../../../ado/guide/data/shape-commands-in-general.md)