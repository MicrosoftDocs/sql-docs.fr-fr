---
description: Types de verrous
title: Types de verrous | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- AdLockBatchOptimistic [ADO]
- AdLockReadOnly [ADO]
- AdLockUnspecified [ADO]
- locks [ADO], types
- AdLockOptimistic [ADO]
- AdLockPessimistic [ADO]
ms.assetid: 12a978c0-b8a0-4ef0-87f0-a43c13659272
author: rothja
ms.author: jroth
ms.openlocfilehash: 8d72b169247df929d923f3b2362a497647b8f8e2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036899"
---
# <a name="types-of-locks"></a>Types de verrous
## <a name="adlockbatchoptimistic"></a>adLockBatchOptimistic  
 Indique les mises à jour optimistes par lot. Requis pour le mode de mise à jour par lot.  
  
 De nombreuses applications extraient plusieurs lignes à la fois et doivent ensuite effectuer des mises à jour coordonnées qui incluent l’ensemble des lignes à insérer, mettre à jour ou supprimer. Avec les curseurs batch, un seul aller-retour vers le serveur est nécessaire, améliorant ainsi les performances des mises à jour et diminuant le trafic réseau. À l’aide d’une bibliothèque de curseurs par lot, vous pouvez créer un curseur statique, puis vous déconnecter de la source de données. À ce stade, vous pouvez apporter des modifications aux lignes, puis vous reconnecter et poster les modifications vers la source de données dans un lot.  
  
## <a name="adlockoptimistic"></a>adLockOptimistic  
 Indique que le fournisseur utilise uniquement des enregistrements de verrouillage optimiste lorsque vous appelez la méthode **Update** . Cela signifie qu’il existe un risque qu’un autre utilisateur modifie les données entre le moment où vous modifiez l’enregistrement et celui où vous appelez **Update**, ce qui crée des conflits. Utilisez ce type de verrou dans les situations où les risques de collisions sont faibles ou où les collisions peuvent être facilement résolues.  
  
## <a name="adlockpessimistic"></a>adLockPessimistic  
 Indique le verrouillage pessimiste, enregistrement par enregistrement. Le fournisseur effectue ce qui est nécessaire pour garantir la réussite de la modification des enregistrements, en généralement en verrouillant les enregistrements au niveau de la source de données juste avant la modification. Bien entendu, cela signifie que les enregistrements ne sont pas disponibles pour les autres utilisateurs une fois que vous avez commencé à modifier, jusqu’à ce que vous relâchiez le verrou en appelant **Update.** Utilisez ce type de verrou dans un système où vous ne pouvez pas vous permettre d’avoir des modifications simultanées des données, telles que dans un système de réservation.  
  
## <a name="adlockreadonly"></a>adLockReadOnly  
 Indique des enregistrements en lecture seule. Vous ne pouvez pas modifier les données. Un verrou en lecture seule est le type de verrou « le plus rapide », car il ne nécessite pas que le serveur maintienne un verrou sur les enregistrements.  
  
## <a name="adlockunspecified"></a>adLockUnspecified  
 Ne spécifie pas de type de verrou.
