---
description: Propriétés dynamiques du recordset dans XML
title: Propriétés dynamiques du Recordset dans XML | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Recordset dynamic properties in XML [ADO]
ms.assetid: 52f8e379-812a-4db8-9210-94458926301c
author: rothja
ms.author: jroth
ms.openlocfilehash: 2e966c3570ce8f9589ede5f60fca1fc0fa70bbac
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037089"
---
# <a name="recordset-dynamic-properties-in-xml"></a>Propriétés dynamiques du recordset dans XML
Les propriétés suivantes spécifiques au fournisseur de l’ensemble d’enregistrements (à partir du moteur de curseur client) sont actuellement conservées au format XML :  
  
-   Mettre à jour la resynchronisation  
  
-   table unique  
  
-   Schéma unique  
  
-   Catalogue unique  
  
-   Commande Resync  
  
-   IRowsetChange  
  
-   IRowsetUpdate  
  
-   CommandTimeout  
  
-   BatchSize  
  
-   UpdateCriteria  
  
-   Reformer le nom  
  
-   AutoRecalc  
  
 Ces propriétés sont enregistrées dans la section du schéma en tant qu’attributs de la définition de l’élément pour le recordset à conserver. Ces attributs sont définis dans l’espace de noms du schéma de l’ensemble de lignes et doivent avoir le préfixe « rs : ».  
  
## <a name="see-also"></a>Voir aussi  
 [Persistance des enregistrements au format XML](../../../ado/guide/data/persisting-records-in-xml-format.md)
