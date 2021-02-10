---
description: Informations sur les erreurs liées aux recordsets
title: Informations sur l’erreur de Recordset-Related | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Recordset-related errors [ADO]
- errors [ADO], Recordset-related
ms.assetid: 7e103574-59ad-4790-b5f9-fa8d715e711e
author: rothja
ms.author: jroth
ms.openlocfilehash: 2fcd596455cc1074d46ea6ee45d6c1f037553741
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037099"
---
# <a name="recordset-related-error-information"></a>Informations sur les erreurs liées aux recordsets
Lors du traitement par lots, la propriété **Status** de l’objet **Recordset** fournit des informations sur les enregistrements individuels dans le **Recordset**. Avant qu’une mise à jour par lot ait lieu, la propriété **Status** de l’ensemble d' **enregistrements** reflète des informations sur les enregistrements à ajouter, modifier et supprimer. Une fois la méthode **UpdateBatch** appelée, la propriété **Status** indique la réussite ou l’échec de l’opération. Lorsque vous passez d’un enregistrement à un autre dans le **jeu d’enregistrements**, la valeur de la propriété **Status** change pour décrire l’état de l’enregistrement en cours.
